---
title: Oracle NetSuite Sourceの概要
description: API またはユーザーインターフェイスを使用して、Oracle NetSuite をAdobe Experience Platformに接続する方法について説明します。
last-substantial-update: 2024-01-30T00:00:00Z
badge: ベータ版
exl-id: 1dd30660-c990-4d3f-a64f-2a17e426f56d
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '756'
ht-degree: 11%

---

# [!DNL Oracle NetSuite]

>[!NOTE]
>
>[!DNL Oracle NetSuite] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、[ ソースの概要 ](../../home.md#terms-and-conditions) を参照してください。

Adobe Experience Platformを使用すると、データを外部ソースから取得しながら、Experience Platform サービスを使用して、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platformでは、データのサードパーティのマーケティング自動処理システムの取り込みがサポートされています。 マーケティング自動化プロバイダーのサポートには、[!DNL Oracle NetSuite] が含まれます。

[[!DNL Oracle NetSuite]](https://www.netsuite.com/) は、ERP/金融、CRM、e コマースソリューションを含むクラウドベースのビジネス管理スイートです。

2 つの異なるソースを使用して、[!DNL Oracle NetSuite] からExperience Platformにデータを取り込むことができます。

* [!DNL Oracle NetSuite Activities] ソースを使用してイベントデータを取り込みます。
* [!DNL Oracle NetSuite Entities] ソースを使用して、顧客および連絡先データを取り込みます。

2 つの [!DNL Oracle NetSuite] ソースについて詳しくは、次の表を参照してください。

| ソース | タイプ | 説明 |
| --- | --- | --- |
| [[!DNL Oracle NetSuite Activities]](#oracle-netsuite-activities) | イベント | カレンダーに追加されたスケジュール済みアクティビティを取得します。 |
| [[!DNL Oracle NetSuite Entities]](#oracle-netsuite-entities) | 顧客 | 顧客名、住所、キー識別子などの詳細を含む、特定の顧客データを取得します。 |
| [[!DNL Oracle NetSuite Entities]](#oracle-netsuite-entities) | 連絡先 | 連絡先名、メール、電話番号、顧客に関連付けられたカスタム連絡先関連フィールドを取得します。 |

## IP アドレス許可リスト {#ip-allow-list}

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要がある場合があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## 前提条件 {#prerequisites}

[!DNL Oracle NetSuite] データをExperience Platformに取り込むには、まず、次の点を確認する必要があります。

* **[!DNL Oracle NetSuite] アカウント**。
   * 有効なアカウントをお持ちでない場合は、[[!DNL Oracle NetSuite]](https://www.NetSuite.com/portal/company/contactus.shtml) にお問い合わせください。
* [!DNL Oracle NetSuite] の製品の **アクティブな購読**。
* **アカウント ID**。
   * [!DNL Oracle NetSuite] ソースは、OAuth 2.0 を使用して [!DNL Oracle NetSuite] API と通信します。 アカウント ID がない場合は、[ アカウント ID の取得方法 ](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_1498754928.html#Finding-Your-NetSuite-Account-ID) に関する [!DNL Oracle] ドキュメントを参照してください。
* **クライアント ID** と **クライアントシークレット** の組み合わせ。
   * [!DNL Oracle NetSuite] API にアクセスするには、クライアント ID とクライアントシークレットが必要です。 この手順では、管理者が以下を保有していることも確認する必要があります。
      * OAuth 2.0 機能を有効にし、適切な OAuth 2.0 役割を設定しました。
      * ユーザーを OAuth 2.0 の役割に割り当て、必要な統合レコードを作成しました。
* **アクセストークン** および **更新トークン**。
   * アクセストークンと更新トークンの生成方法については、[OAuth 2.0 認証コード付与フロー ](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_158074210415.html#OAuth-2.0-Authorization-Code-Grant-Flow) に関する [!DNL Oracle] ガイドを参照してください。

### 必要な資格情報の収集 {#gather-credentials}

[!DNL Oracle NetSuite] をExperience Platformに接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| クライアント ID | [!DNL Oracle NetSuite] で統合レコードを作成する際に生成されるクライアント ID 値。 詳しくは、[ 統合レコードの作成 ](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_157771733782.html#procedure_157838925981) 方法に関する [!DNL Oracle] ガイドを参照してください。 | `7fce.....b42f`<br> 値は 64 文字の文字列です。 |
| クライアントシークレット | 統合レコードの作成時に生成されるクライアントシークレット値。 詳しくは、[ 統合レコードの作成 ](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_157771733782.html#procedure_157838925981) 方法に関する [!DNL Oracle] ガイドを参照してください。 | `5c98.....1b46`<br> 値は 64 文字の文字列です。 |
| 認証テスト URL | （任意） [!DNL NetSuite] 認証テスト URL。 | `https://{ACCOUNT_ID}.app.netsuite.com<br>/app/login/oauth2/authorize.nl?response_type=code<br>&redirect_uri=https%3A%2F%2Fapi.github.com<br>&scope=rest_webservices<br>&state=ykv2XLx1BpT5Q0F3MRPHb94j<br>&client_id={CLIENT_ID}` |
| アクセストークン | アクセストークンは JSON web トークン（JWT）形式で、60 分間のみ有効です。 アクセストークンの取得方法について詳しくは、[NetSuite の OAuth 2.0 認証 ](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_158081952044.html#Step-Two-POST-Request-to-the-Token-Endpoint) に関する [!DNL Oracle] ガイドを参照してください。 | `eyJr......f4V0`<br> 値は、JSON web トークン（JWT）形式の 1024 文字の文字列です。 |
| 更新トークン | アクセストークンの有効期限が切れた後は、更新を使用して新しいアクセストークンを生成します。 更新トークンは 7 日間有効です。 アクセストークンの取得方法について詳しくは、[NetSuite の OAuth 2.0 認証 ](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_158081952044.html#Step-Two-POST-Request-to-the-Token-Endpoint) に関する [!DNL Oracle] ガイドを参照してください。 | `eyJr......dmxM`<br> 値は、JSON web トークン（JWT）形式の 1024 文字の文字列です。 |
| アクセストークン URL | アプリケーションが POST リクエストを送信するトークンエンドポイント。 | `https://{ACCOUNT_ID}.suitetalk.api.netsuite.com<br>/services/rest/auth/oauth2/v1/token` |

>[!IMPORTANT]
>
>更新トークンの有効期限が切れた後、更新されたトークンを使用して、Experience Platformで新しいアカウントを作成する必要があります。

## [!DNL Oracle NetSuite Activities] をExperience Platformに接続 {#oracle-netsuite-activities}

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Oracle NetSuite Activities] をExperience Platformに接続する方法について説明しています。

* [ ソース接続とデータフローを作成し、API を使用してExperience Platformに  [!DNL Oracle NetSuite Activities]  ータを取り込みます ](../../tutorials/api/create/marketing-automation/oracle-netsuite-activities.md)。
* [UI を使用してアカウ  [!DNL Oracle NetSuite Activities]  トをExperience Platformに接続します ](../../tutorials/ui/create/marketing-automation/oracle-netsuite-activities.md)。
* [UI を使用したソース接続のデータフローの作成 ](../../tutorials/ui/dataflow/marketing-automation.md)。

## [!DNL Oracle NetSuite Entities] をExperience Platformに接続 {#oracle-netsuite-entities}

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Oracle NetSuite Entities] をExperience Platformに接続する方法について説明しています。

* [ ソース接続とデータフローを作成し、API を使用してExperience Platformに  [!DNL Oracle NetSuite Entities]  ータを取り込みます ](../../tutorials/api/create/marketing-automation/oracle-netsuite-entities.md)。
* [UI を使用してアカウ  [!DNL Oracle NetSuite Entities]  トをExperience Platformに接続します ](../../tutorials/ui/create/marketing-automation/oracle-netsuite-entities.md)。
* [UI を使用したソース接続のデータフローの作成 ](../../tutorials/ui/dataflow/marketing-automation.md)。
