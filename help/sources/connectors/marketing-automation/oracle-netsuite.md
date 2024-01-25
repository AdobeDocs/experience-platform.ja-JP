---
title: OracleNetSuite ソースの概要
description: API またはユーザーインターフェイスを使用してOracleNetSuite をAdobe Experience Platformに接続する方法について説明します。
hide: true
hidefromtoc: true
badge: ベータ版
source-git-commit: 053cf0af327b39830f025686e0f8f67c27f1c45c
workflow-type: tm+mt
source-wordcount: '748'
ht-degree: 21%

---

# [!DNL Oracle NetSuite]

>[!NOTE]
>
>[!DNL Oracle NetSuite] ソースはベータ版です。詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platformは、サードパーティのマーケティング自動化システムのデータ取り込みをサポートしています。 マーケティング自動化プロバイダーのサポートには、[!DNL Oracle NetSuite] が含まれます。

[[!DNL Oracle NetSuite]](https://www.netsuite.com/) は、ERP/金融、CRM、e コマースの各ソリューションを含む、クラウドベースのビジネス管理スイートです。

2 つの異なるソースを使用して、データを取り込むことができます。 [!DNL Oracle NetSuite] EXPERIENCE PLATFORM:

* 以下を使用します。 [!DNL Oracle NetSuite Activities] イベントデータを取り込むソース。
* 以下を使用します。 [!DNL Oracle NetSuite Entities] 顧客および連絡先データの取り込み元。

次の表に、この 2 つの詳細を示します [!DNL Oracle NetSuite] ソース。

| ソース | タイプ | 説明 |
| --- | --- | --- |
| [[!DNL Oracle NetSuite Activities]](#oracle-netsuite-activities) | イベント | カレンダーに追加されたスケジュール済みアクティビティを取得します。 |
| [[!DNL Oracle NetSuite Entities]](#oracle-netsuite-entities) | 顧客 | 顧客名、住所、主要識別子などの詳細を含む、特定の顧客データを取得します。 |
| [[!DNL Oracle NetSuite Entities]](#oracle-netsuite-entities) | 連絡先 | 顧客に関連付けられた連絡先名、E メール、電話番号、およびカスタム連絡先関連のフィールドを取得します。 |

## IP アドレス許可リスト {#ip-allow-list}

ソースコネクタを使用する前に、IP アドレスのリストを許可リストに追加する必要が生じる場合があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## 前提条件 {#prerequisites}

次を持ってくる前に、 [!DNL Oracle NetSuite] Experience Platformにデータを送信する場合は、まず、次の条件を満たす必要があります。

* **An [!DNL Oracle NetSuite] アカウント**.
   * 連絡先 [[!DNL Oracle NetSuite]](https://www.NetSuite.com/portal/company/contactus.shtml) 有効なアカウントをまだ持っていない場合は、をクリックします。
* An **アクティブな購読** 任意の [!DNL Oracle NetSuite] 製品。
* An **アカウント ID**.
   * The [!DNL Oracle NetSuite] ソースは、OAuth 2.0 を使用して [!DNL Oracle NetSuite] API アカウント ID がない場合は、 [!DNL Oracle] に関するドキュメント [アカウント ID を取得する方法](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_1498754928.html#Finding-Your-NetSuite-Account-ID).
* A **クライアント ID** および **クライアント秘密鍵** 組み合わせ。
   * にアクセスするには、クライアント ID とクライアント秘密鍵が必要です [!DNL Oracle NetSuite] API この手順の間に、管理者が以下を行っていることを確認する必要があります。
      * OAuth 2.0 機能を有効にし、適切な OAuth 2.0 の役割を設定します。
      * OAuth 2.0 の役割にユーザーを割り当て、必要な統合レコードを作成しました。
* An **アクセストークン** および **更新トークン**.
   * 詳しくは、 [!DNL Oracle] ～に関するガイド [OAuth 2.0 認証コード付与フロー](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_158074210415.html#OAuth-2.0-Authorization-Code-Grant-Flow) を参照してください。

### 必要な資格情報の収集 {#gather-credentials}

[!DNL Oracle NetSuite] を Platform に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| クライアント ID | 統合レコードを [!DNL Oracle NetSuite]. 詳しくは、 [!DNL Oracle] ～する方法に関するガイド [統合レコードの作成](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_157771733782.html#procedure_157838925981) を参照してください。 | `7fce.....b42f`<br>値は 64 文字の文字列です。 |
| クライアントシークレット | 統合レコードの作成時に生成されるクライアント秘密鍵の値です。 詳しくは、 [!DNL Oracle] ～する方法に関するガイド [統合レコードの作成](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_157771733782.html#procedure_157838925981) を参照してください。 | `5c98.....1b46`<br>値は 64 文字の文字列です。 |
| 認証テスト URL | （オプション） [!DNL NetSuite] 認証テスト URL。 | `https://{ACCOUNT_ID}.app.netsuite.com<br>/app/login/oauth2/authorize.nl?response_type=code<br>&redirect_uri=https%3A%2F%2Fapi.github.com<br>&scope=rest_webservices<br>&state=ykv2XLx1BpT5Q0F3MRPHb94j<br>&client_id={CLIENT_ID}` |
| アクセストークン | アクセストークンは JSON Web トークン (JWT) 形式で、60 分間のみ有効です。 アクセストークンの取得方法の詳細については、 [!DNL Oracle] ～に関するガイド [NetSuite の OAuth 2.0 認証](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_158081952044.html#Step-Two-POST-Request-to-the-Token-Endpoint). | `eyJr......f4V0`<br> 値は 1024 文字の文字列で、JSON Web トークン (JWT) 形式で表されます。 |
| 更新トークン | 更新を使用して、アクセストークンの有効期限が切れた後に、新しいアクセストークンを生成します。 更新トークンは 7 日間有効です。 アクセストークンの取得方法の詳細については、 [!DNL Oracle] ～に関するガイド [NetSuite の OAuth 2.0 認証](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_158081952044.html#Step-Two-POST-Request-to-the-Token-Endpoint). | `eyJr......dmxM`<br> 値は 1024 文字の文字列で、JSON Web トークン (JWT) 形式で表されます。 |
| トークン URL にアクセス | アプリケーションがPOSTリクエストを送信するトークンエンドポイント。 | `https://{ACCOUNT_ID}.suitetalk.api.netsuite.com<br>/services/rest/auth/oauth2/v1/token` |

>[!IMPORTANT]
>
>更新トークンの有効期限が切れた後、更新されたトークンとExperience Platformして新しいアカウントを作成する必要があります。

## [!DNL Oracle NetSuite Activities] を Platform に接続 {#oracle-netsuite-activities}

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Oracle NetSuite Activities] と Platform を接続する方法について説明します。

* [ソース接続とデータフローを作成して、 [!DNL Oracle NetSuite Activities] API を使用した Platform へのデータの取得](../../tutorials/api/create/marketing-automation/oracle-netsuite-activities.md).
* [接続する [!DNL Oracle NetSuite Activities] UI を使用してExperience Platformにアカウント](../../tutorials/ui/create/marketing-automation/oracle-netsuite-activities.md).
* [UI を使用したソース接続のデータフローの作成](../../tutorials/ui/dataflow/marketing-automation.md).

## [!DNL Oracle NetSuite Entities] を Platform に接続 {#oracle-netsuite-entities}

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Oracle NetSuite Entities] と Platform を接続する方法について説明します。

* [ソース接続とデータフローを作成して、 [!DNL Oracle NetSuite Entities] API を使用した Platform へのデータの取得](../../tutorials/api/create/marketing-automation/oracle-netsuite-entities.md).
* [接続する [!DNL Oracle NetSuite Entities] UI を使用してExperience Platformにアカウント](../../tutorials/ui/create/marketing-automation/oracle-netsuite-entities.md).
* [UI を使用したソース接続のデータフローの作成](../../tutorials/ui/dataflow/marketing-automation.md).
