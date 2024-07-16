---
title: Salesforce Source コネクタの概要
description: API またはユーザーインターフェイスを使用して Salesforce をAdobe Experience Platformに接続する方法について説明します。
exl-id: 597778ad-3cf8-467c-ad5b-e2850967fdeb
source-git-commit: 5d28db34edd377269e8710b1741098a08616ae5f
workflow-type: tm+mt
source-wordcount: '864'
ht-degree: 20%

---

# [!DNL Salesforce]

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platform は、サードパーティの CRM システムからのデータ取り込みをサポートしています。CRM プロバイダーのサポートは [!DNL Salesforce] を含みます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## [!DNL Salesforce] から XDM へのフィールドマッピング

[!DNL Salesforce] と Platform の間にソース接続を確立するには、[!DNL Salesforce] のソースデータフィールドを、Platform に取り込む前に、適切なターゲット XDM フィールドにマッピングする必要があります。

[!DNL Salesforce] データセットと Platform 間のフィールドマッピングルールについて詳しくは、次を参照してください。

- [連絡先](../adobe-applications/mapping/salesforce.md#contact)
- [リード数](../adobe-applications/mapping/salesforce.md#lead)
- [アカウント](../adobe-applications/mapping/salesforce.md#account)
- [商談](../adobe-applications/mapping/salesforce.md#opportunity)
- [商談連絡先の役割](../adobe-applications/mapping/salesforce.md#opportunity-contact-role)
- [キャンペーン](../adobe-applications/mapping/salesforce.md#campaign)
- [キャンペーンメンバー](../adobe-applications/mapping/salesforce.md#campaign-member)
- [アカウント連絡先の関係](../adobe-applications/mapping/salesforce.md#account-contact-relation)

## [!DNL Salesforce] 名前空間とスキーマ自動生成ユーティリティの設定

[!DNL Salesforce] ソースを [!DNL B2B-CDP] の一部として使用するには、まず [!DNL Postman] ユーティリティを設定して、[!DNL Salesforce] 名前空間とスキーマを自動生成する必要があります。 次のドキュメントでは、[!DNL Postman] ユーティリティの設定に関する追加情報を示します。

- この [GitHub リポジトリ ](https://github.com/adobe/experience-platform-postman-samples/tree/master/Postman%20Collections/CDP%20Namespaces%20and%20Schemas%20Utility) から、名前空間およびスキーマ自動生成ユーティリティのコレクションと環境をダウンロードできます。
- 必要なヘッダーの値を収集する方法やサンプル API 呼び出しを読み取る方法など、Platform API の使用について詳しくは、[Platform API の概要 ](../../../landing/api-guide.md) を参照してください。
- Platform API の資格情報の生成方法について詳しくは、[Experience PlatformAPI の認証とアクセス ](../../../landing/api-authentication.md) に関するチュートリアルを参照してください。
- Platform API の [!DNL Postman] の設定方法について詳しくは、[ 開発者コンソールとの設定  [!DNL Postman]](../../../landing/postman.md) のチュートリアルを参照してください。

Platform 開発者コンソールをセットアップす [!DNL Postman] と、適切な環境値の [!DNL Postman] 環境への適用を開始できます。

次の表に、値の例と、[!DNL Postman] 環境へのデータ入力に関する追加情報を示します。

| 変数 | 説明 | 例 |
| --- | --- | --- |
| `CLIENT_SECRET` | `{ACCESS_TOKEN}` ータの生成に使用される一意の ID。 サー `{CLIENT_SECRET}` スの取得方法について詳しくは、[Experience Platform API の認証とアクセス ](../../../landing/api-authentication.md) に関するチュートリアルを参照してください。 | `{CLIENT_SECRET}` |
| `JWT_TOKEN` | JSON web トークン（JWT）は、{ACCESS_TOKEN} ータの生成に使用される認証資格情報です。 サー `{JWT_TOKEN}` スの生成方法について詳しくは、[Experience PlatformAPI の認証とアクセス ](../../../landing/api-authentication.md) に関するチュートリアルを参照してください。 | `{JWT_TOKEN}` |
| `API_KEY` | Experience PlatformAPI への呼び出しの認証に使用される一意の ID。 サー `{API_KEY}` スの取得方法について詳しくは、[Experience Platform API の認証とアクセス ](../../../landing/api-authentication.md) に関するチュートリアルを参照してください。 | `c8d9a2f5c1e03789bd22e8efdd1bdc1b` |
| `ACCESS_TOKEN` | Experience PlatformAPI を呼び出すために必要な認証トークン。 サー `{ACCESS_TOKEN}` スの取得方法について詳しくは、[Experience Platform API の認証とアクセス ](../../../landing/api-authentication.md) に関するチュートリアルを参照してください。 | `Bearer {ACCESS_TOKEN}` |
| `META_SCOPE` | [!DNL Marketo] に関しては、この値は固定で、常に `ent_dataservices_sdk` に設定されます。 | `ent_dataservices_sdk` |
| `CONTAINER_ID` | `global` コンテナには、標準AdobeおよびExperience Platformパートナー提供のすべてのクラス、スキーマフィールドグループ、データタイプおよびスキーマが格納されます。 [!DNL Marketo] に関しては、この値は固定で、常に `global` に設定されます。 | `global` |
| `PRIVATE_KEY` | API に対する [!DNL Postman] インスタンスのExperience Platformに使用される資格情報。 コンテン {PRIVATE_KEY} の取得方法については、開発者コンソールの設定および [ 開発者コンソールの設定および  [!DNL Postman]](../../../landing/postman.md) に関するチュートリアルを参照してください。 | `{PRIVATE_KEY}` |
| `TECHNICAL_ACCOUNT_ID` | Adobe I/Oへの統合に使用する資格情報。 | `D42AEVJZTTJC6LZADUBVPA15@techacct.adobe.com` |
| `IMS` | Identity Management System （IMS）は、Adobe サービスに対して認証を行うためのフレームワークを提供します。 [!DNL Marketo] に関しては、この値は固定で、常に `ims-na1.adobelogin.com` に設定されます。 | `ims-na1.adobelogin.com` |
| `IMS_ORG` | 製品およびサービスを所有またはライセンスし、そのメンバーへのアクセスを許可できる法人組織。 `{ORG_ID}` ーザー情報の取得方法については、[Developer Console の設定および  [!DNL Postman]](../../../landing/postman.md) に関するチュートリアルを参照してください。 | `ABCEH0D9KX6A7WA7ATQE0TE@adobeOrg` |
| `SANDBOX_NAME` | 使用している仮想サンドボックスパーティションの名前。 | `prod` |
| `TENANT_ID` | 作成するリソースの名前空間が適切に設定され、組織内に含まれていることを確認するために使用される ID。 | `b2bcdpproductiontest` |
| `PLATFORM_URL` | API 呼び出しを行う URL エンドポイント。 この値は固定で、常に `http://platform.adobe.io/` に設定されます。 | `http://platform.adobe.io/` |
| `munchkinId` | [!DNL Marketo] アカウントの一意の ID。 インスタンスの取得方法について詳しくは、[ インスタンスの認証 ](../adobe-applications/marketo/marketo-auth.md) に関するチュー `munchkinId` リアルを参照してください  [!DNL Marketo]  | `123-ABC-456` |
| `sfdc_org_id` | [!DNL Salesforce] アカウントの組織 ID。 [!DNL Salesforce] 組織 ID の取得について詳しくは、次の [[!DNL Salesforce]  ガイド ](https://help.salesforce.com/articleView?id=000325251&amp;type=1&amp;mode=1) を参照してください。 | `00D4W000000FgYJUA0` |
| `has_abm` | [!DNL Marketo Account-Based Marketing] を購読しているかどうかを示すブール値。 | `false` |
| `has_msi` | [!DNL Marketo Sales Insight] に登録されているかどうかを示すブール値。 | `false` |

{style="table-layout:auto"}

### スクリプトの実行

[!DNL Postman] コレクションと環境を設定すると、[!DNL Postman] インターフェイスを使用してスクリプトを実行できます。

[!DNL Postman] インターフェイスで、自動生成ユーティリティのルートフォルダーを選択し、上部のヘッダーから「**[!DNL Run]**」を選択します。

![root-folder](../../images/tutorials/create/salesforce/root-folder.png)

[!DNL Runner] インターフェイスが表示されます。 ここから、すべてのチェックボックスが選択されていることを確認してから選択し **[!DNL Run Namespaces and Schemas Autogeneration Utility]** す。

![run-generator](../../images/tutorials/create/salesforce/run-generator.png)

リクエストが成功すると、ベータ版の仕様に従って B2B 名前空間とスキーマが作成されます。

## API を使用して [!DNL Salesforce] と Platform を接続する

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Salesforce] と Platform を接続する方法について説明します。

- [Flow Service API を使用した Salesforce ベース接続の作成](../../tutorials/api/create/crm/salesforce.md)
- [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
- [Flow Service API を使用して、CRM ソースのデータフローを作成する](../../tutorials/api/collect/crm.md)

## UI を使用した [!DNL Salesforce] の Platform への接続

- [UI での Salesforce ソース接続の作成](../../tutorials/ui/create/crm/salesforce.md)
- [UI での CRM 接続のデータフローの作成](../../tutorials/ui/dataflow/crm.md)
