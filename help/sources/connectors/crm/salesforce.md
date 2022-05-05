---
keywords: Experience Platform；ホーム；人気の高いトピック；crm スキーマ；crm;CRM;salesforce;Salesforce
solution: Experience Platform
title: Salesforce ソースコネクタの概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用して Salesforce をAdobe Experience Platformに接続する方法を説明します。
exl-id: 597778ad-3cf8-467c-ad5b-e2850967fdeb
source-git-commit: fa861e9740e05b4fcc4e8039bb288301d42b8357
workflow-type: tm+mt
source-wordcount: '897'
ht-degree: 17%

---

# [!DNL Salesforce] コネクタ

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platform は、サードパーティの CRM システムからのデータ取り込みをサポートしています。CRM プロバイダーのサポートは [!DNL Salesforce] を含みます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## 次のフィールドマッピング [!DNL Salesforce] XDM に

間にソース接続を確立するには [!DNL Salesforce] また、Platform、 [!DNL Salesforce] Platform に取り込む前に、ソースデータフィールドを適切なターゲット XDM フィールドにマッピングする必要があります。

次の間のフィールドマッピングルールの詳細については、 [!DNL Salesforce] データセットとプラットフォーム：

- [連絡先](../adobe-applications/mapping/salesforce.md#contact)
- [リード数](../adobe-applications/mapping/salesforce.md#lead)
- [アカウント](../adobe-applications/mapping/salesforce.md#account)
- [商談](../adobe-applications/mapping/salesforce.md#opportunity)
- [商談連絡先の役割](../adobe-applications/mapping/salesforce.md#opportunity-contact-role)
- [キャンペーン](../adobe-applications/mapping/salesforce.md#campaign)
- [キャンペーンメンバー](../adobe-applications/mapping/salesforce.md#campaign-member)

## 設定 [!DNL Salesforce] 名前空間とスキーマ自動生成ユーティリティ

次の手順で [!DNL Salesforce] ～の一部としての出所 [!DNL B2B-CDP]を設定する場合、まず [!DNL Postman] 自動生成するユーティリティ [!DNL Salesforce] 名前空間とスキーマ。 次のドキュメントでは、 [!DNL Postman] ユーティリティ：

- このから、名前空間とスキーマの自動生成ユーティリティのコレクションと環境をダウンロードできます [GitHub リポジトリ](https://github.com/adobe/experience-platform-postman-samples/tree/master/Postman%20Collections/CDP%20Namespaces%20and%20Schemas%20Utility).
- 必要なヘッダーの値の収集方法の詳細や API 呼び出し例の読み取りなど、Platform API の使用に関する詳細については、 [Platform API の概要](../../../landing/api-guide.md).
- Platform API の資格情報を生成する方法について詳しくは、 [Experience PlatformAPI の認証とアクセス](../../../landing/api-authentication.md).
- の設定方法について詳しくは、 [!DNL Postman] Platform API については、 [開発者コンソールの設定および [!DNL Postman]](../../../landing/postman.md).

Platform デベロッパーコンソールと [!DNL Postman] を設定すると、適切な環境値を [!DNL Postman] 環境。

次の表に、値の例と、 [!DNL Postman] 環境：

| 変数 | 説明 | 例 |
| --- | --- | --- |
| `CLIENT_SECRET` | の生成に使用される一意の ID `{ACCESS_TOKEN}`. に関するチュートリアルを参照してください。 [Experience PlatformAPI の認証とアクセス](../../../landing/api-authentication.md) 」を参照してください。 `{CLIENT_SECRET}`. | `{CLIENT_SECRET}` |
| `JWT_TOKEN` | JSON Web トークン (JWT) は、{ACCESS_TOKEN} の生成に使用される認証資格情報です。 に関するチュートリアルを参照してください。 [Experience PlatformAPI の認証とアクセス](../../../landing/api-authentication.md) 」を参照してください。 `{JWT_TOKEN}`. | `{JWT_TOKEN}` |
| `API_KEY` | Experience PlatformAPI の呼び出しを認証するために使用される一意の識別子。 に関するチュートリアルを参照してください。 [Experience PlatformAPI の認証とアクセス](../../../landing/api-authentication.md) 」を参照してください。 `{API_KEY}`. | `c8d9a2f5c1e03789bd22e8efdd1bdc1b` |
| `ACCESS_TOKEN` | Experience PlatformAPI の呼び出しを完了するために必要な認証トークン。 に関するチュートリアルを参照してください。 [Experience PlatformAPI の認証とアクセス](../../../landing/api-authentication.md) 」を参照してください。 `{ACCESS_TOKEN}`. | `Bearer {ACCESS_TOKEN}` |
| `META_SCOPE` | 次に関して： [!DNL Marketo]の場合、この値は固定で、常に次の値に設定されます。 `ent_dataservices_sdk`. | `ent_dataservices_sdk` |
| `CONTAINER_ID` | この `global` コンテナには、すべての標準AdobeおよびExperience Platformパートナーが提供するクラス、スキーマフィールドグループ、データ型、スキーマが格納されます。 次に関して： [!DNL Marketo]の場合、この値は固定値で、常に `global`. | `global` |
| `PRIVATE_KEY` | 認証に使用する資格情報 [!DNL Postman] インスタンスからExperience PlatformAPI へ 開発者コンソールの設定に関するチュートリアルと [開発者コンソールの設定および [!DNL Postman]](../../../landing/postman.md) を参照してください。 | `{PRIVATE_KEY}` |
| `TECHNICAL_ACCOUNT_ID` | Adobe I/Oへの統合に使用する資格情報。 | `D42AEVJZTTJC6LZADUBVPA15@techacct.adobe.com` |
| `IMS` | Identity Management System(IMS) は、Adobe サービスに対する認証のフレームワークを提供します。 次に関して： [!DNL Marketo]の場合、この値は固定で、常に次の値に設定されます。 `ims-na1.adobelogin.com`. | `ims-na1.adobelogin.com` |
| `IMS_ORG` | 製品やサービスを所有またはライセンスし、そのメンバーへのアクセスを許可できる企業エンティティ。 に関するチュートリアルを参照してください。 [開発者コンソールの設定および [!DNL Postman]](../../../landing/postman.md) を参照してください。 `{IMS_ORG}` 情報。 | `ABCEH0D9KX6A7WA7ATQE0TE@adobeOrg` |
| `SANDBOX_NAME` | 使用している仮想サンドボックスパーティションの名前です。 | `prod` |
| `TENANT_ID` | 作成するリソースの名前空間が適切に付けられ、IMS 組織内に含まれるようにするために使用される ID。 | `b2bcdpproductiontest` |
| `PLATFORM_URL` | API 呼び出しをおこなう URL エンドポイント。 この値は固定で、常に次の値に設定されます。 `http://platform.adobe.io/`. | `http://platform.adobe.io/` |
| `munchkinId` | の一意の ID [!DNL Marketo] アカウント に関するチュートリアルを参照してください。 [認証 [!DNL Marketo] インスタンス](../adobe-applications/marketo/marketo-auth.md) 」を参照してください。 `munchkinId`. | `123-ABC-456` |
| `sfdc_org_id` | の組織 ID [!DNL Salesforce] アカウント 次を参照してください。 [[!DNL Salesforce] ガイド](https://help.salesforce.com/articleView?id=000325251&amp;type=1&amp;mode=1) を参照してください。 [!DNL Salesforce] 組織 ID。 | `00D4W000000FgYJUA0` |
| `has_abm` | 購読しているかどうかを示す boolean 値です [!DNL Marketo Account-Based Marketing]. | `false` |
| `has_msi` | 購読しているかどうかを示す boolean 値です。 [!DNL Marketo Sales Insight]. | `false` |

{style=&quot;table-layout:auto&quot;}

### スクリプトの実行

を [!DNL Postman] コレクションと環境の設定時に、 [!DNL Postman] インターフェイス。

内 [!DNL Postman] インタフェースで、auto-generator ユーティリティのルートフォルダを選択し、 **[!DNL Run]** を上部のヘッダーから削除します。

![root-folder](../../images/tutorials/create/salesforce/root-folder.png)

この [!DNL Runner] インターフェイスが表示されます。 ここから、すべてのチェックボックスが選択されていることを確認してから、「 」を選択します。 **[!DNL Run Namespaces and Schemas Autogeneration Utility]**.

![ランジェネレーター](../../images/tutorials/create/salesforce/run-generator.png)

リクエストが成功すると、ベータ仕様に従って B2B 名前空間とスキーマが作成されます。

## 接続 [!DNL Salesforce] API を使用して Platform に接続

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Salesforce] と Platform を接続する方法について説明します。

- [フローサービス API を使用した Salesforce ベース接続の作成](../../tutorials/api/create/crm/salesforce.md)
- [フローサービス API を使用したデータテーブルの調査](../../tutorials/api/explore/tabular.md)
- [Flow Service API を使用して、CRM ソースのデータフローを作成する](../../tutorials/api/collect/crm.md)

## 接続 [!DNL Salesforce] UI を使用して Platform に接続

- [UI での Salesforce ソース接続の作成](../../tutorials/ui/create/crm/salesforce.md)
- [UI での CRM 接続のデータフローの作成](../../tutorials/ui/dataflow/crm.md)
