---
keywords: Experience Platform；ホーム；人気のあるトピック；Marketoソースコネクタ；名前空間;スキーマ
solution: Experience Platform
title: Marketo名前空間
topic-legacy: overview
description: このドキュメントでは、Marketo Engageソースコネクタを作成する際に必要なカスタム名前空間の概要を説明します。
exl-id: f1592be5-987e-41b8-9844-9dea5bd452b9
source-git-commit: af728fb508c514db3d5871114f9a406c1ed428f2
workflow-type: tm+mt
source-wordcount: '1670'
ht-degree: 12%

---

# （ベータ版） [!DNL Marketo Engage]名前空間とスキーマ

>[!IMPORTANT]
>
>[!DNL Marketo Engage]ソースは現在ベータ版です。 この機能とドキュメントは、変更される場合があります。

このドキュメントは、[!DNL Marketo Engage]で使用されるB2B名前空間とスキーマ（以下「[!DNL Marketo]」と呼ばれる）の基になる設定に関する情報を提供します。 このドキュメントでは、[!DNL Marketo] B2B名前空間とスキーマの生成に必要なPostmanオートメーションユーティリティの設定に関する詳細も説明します。

## [!DNL Marketo]名前空間およびスキーマ自動生成ユーティリティの設定

[!DNL Marketo]名前空間とスキーマ自動生成ユーティリティを使用する最初の手順は、プラットフォーム開発者コンソールと[!DNL Postman]環境を設定することです。

- この[GitHubリポジトリ](https://git.corp.adobe.com/marketo-engineering/namespace_schema_utility)から、名前空間とスキーマの自動生成ユーティリティの収集と環境をダウンロードできます。
- 必要なヘッダーの値の収集方法やサンプルAPI呼び出しを読む方法など、プラットフォームAPIの使用に関する詳細は、[プラットフォームAPIの使用の手引き](../../../../landing/api-guide.md)のガイドを参照してください。
- プラットフォームAPI用の資格情報を生成する方法について詳しくは、[Experience PlatformAPIの認証とアクセス](../../../../landing/api-authentication.md)のチュートリアルを参照してください。
- プラットフォームAPI用に[!DNL Postman]を設定する方法について詳しくは、[開発者コンソールと [!DNL Postman]](../../../../landing/postman.md)の設定に関するチュートリアルを参照してください。

プラットフォーム開発者コンソールと[!DNL Postman]の設定により、適切な環境値を[!DNL Postman]環境に適用する開始が可能になりました。

次の表に、値の例と、[!DNL Postman]環境の入力に関する追加情報を示します。

| 変数 | 説明 | 例 |
| --- | --- | --- |
| `CLIENT_SECRET` | `{ACCESS_TOKEN}`の生成に使用される一意の識別子。 `{CLIENT_SECRET}`を取得する方法については、[Experience PlatformAPIの認証とアクセス](../../../../landing/api-authentication.md)のチュートリアルを参照してください。 | `{CLIENT_SECRET}` |
| `JWT_TOKEN` | JSON Web Token(JWT)は、{ACCESS_TOKEN}の生成に使用する認証資格情報です。 `{JWT_TOKEN}`を生成する方法については、[Experience PlatformAPIの認証とアクセス](../../../../landing/api-authentication.md)のチュートリアルを参照してください。 | `{JWT_TOKEN}` |
| `API_KEY` | Experience PlatformAPIへの呼び出しを認証するために使用される一意の識別子。 `{API_KEY}`を取得する方法については、[Experience PlatformAPIの認証とアクセス](../../../../landing/api-authentication.md)のチュートリアルを参照してください。 | `c8d9a2f5c1e03789bd22e8efdd1bdc1b` |
| `ACCESS_TOKEN` | Experience PlatformAPIへの呼び出しを完了するのに必要な認証トークン。 `{ACCESS_TOKEN}`を取得する方法については、[Experience PlatformAPIの認証とアクセス](../../../../landing/api-authentication.md)のチュートリアルを参照してください。 | `Bearer {ACCESS_TOKEN}` |
| `META_SCOPE` | [!DNL Marketo]に関しては、この値は固定値で、常に次に設定されます。`ent_dataservices_sdk`. | `ent_dataservices_sdk` |
| `CONTAINER_ID` | `global`コンテナは、標準AdobeとExperience Platformパートナーが提供するクラス、スキーマフィールドグループ、データ型、スキーマをすべて保持します。 [!DNL Marketo]に関しては、この値は固定値で、常に`global`に設定されます。 | `global` |
| `PRIVATE_KEY` | [!DNL Postman]インスタンスをExperience PlatformAPIに対して認証するために使用する秘密鍵証明書です。 {PRIVATE_KEY}の取得方法については、開発者コンソールの設定のチュートリアルと[開発者コンソールと [!DNL Postman]](../../../../landing/postman.md)の設定のチュートリアルを参照してください。 | `{PRIVATE_KEY}` |
| `TECHNICAL_ACCOUNT_ID` | Adobe I/Oへの統合に使用する秘密鍵証明書です。 | `D42AEVJZTTJC6LZADUBVPA15@techacct.adobe.com` |
| `IMS` | Identity Managementシステム(IMS)は、Adobeサービスに対する認証のフレームワークを提供します。 [!DNL Marketo]に関しては、この値は固定値で、常に次に設定されます。`ims-na1.adobelogin.com`. | `ims-na1.adobelogin.com` |
| `IMS_ORG` | 製品やサービスを所有またはライセンスし、そのメンバーにアクセスを許可できる企業体。 `{IMS_ORG}`情報を取得する方法については、[開発者コンソールの設定と [!DNL Postman]](../../../../landing/postman.md)のチュートリアルを参照してください。 | `ABCEH0D9KX6A7WA7ATQE0TE@adobeOrg` |
| `SANDBOX_NAME` | 使用している仮想サンドボックスパーティションの名前です。 | `prod` |
| `TENANT_ID` | 作成するリソースの名前が正しく指定され、IMS組織内に含まれていることを確認するために使用されるID。 | `b2bcdpproductiontest` |
| `PLATFORM_URL` | API呼び出しを行うURLエンドポイント。 この値は固定値で、常に次の値に設定されます。`http://platform.adobe.io/`. | `http://platform.adobe.io/` |
| `munchkinId` | [!DNL Marketo]アカウントの一意のID。 `munchkinId`を取得する方法については、[ [!DNL Marketo] インスタンス](./marketo-auth.md)の認証のチュートリアルを参照してください。 | `123-ABC-456` |
| `sfdc_org_id` | [!DNL Salesforce]アカウントの組織ID。 [!DNL Salesforce]組織IDの取得について詳しくは、次の[[!DNL Salesforce] ガイド](https://help.salesforce.com/articleView?id=000325251&amp;type=1&amp;mode=1)を参照してください。 | `00D4W000000FgYJUA0` |
| `msd_org_id` | [!DNL Dynamics]アカウントの組織ID。 [!DNL Dynamics]組織IDの取得について詳しくは、次の[[!DNL Microsoft Dynamics] ガイド](https://docs.microsoft.com/en-us/power-platform/admin/determine-org-id-name)を参照してください。 | `f6438fab-67e8-4814-a6b5-8c8dcdf7a98f` |
| `has_abm` | [!DNL Marketo Account-Based Marketing]をサブスクライブしているかどうかを示すboolean値です。 | `false` |
| `has_msi` | [!DNL Marketo Sales Insight]にサブスクライブされているかどうかを示すboolean値です。 | `false` |

{style=&quot;table-layout:auto&quot;}

### スクリプトの実行

[!DNL Postman]コレクションと環境を設定すると、[!DNL Postman]インターフェイスを通じてスクリプトを実行できるようになります。

[!DNL Postman]インターフェイスで、auto-generatorユーティリティのルートフォルダを選択し、一番上のヘッダから&#x200B;**[!DNL Run]**&#x200B;を選択します。

![root-folder](../images/marketo/root-folder.png)

[!DNL Runner]インターフェイスが表示されます。 ここから、すべてのチェックボックスが選択されていることを確認し、**[!DNL Run Adobe I/O Access Token Generation + Automate Namespace creation]**&#x200B;を選択します。

![ランジェネレータ](../images/marketo/run-generator.png)

リクエストが成功すると、ベータ仕様に従ってB2B名前空間とスキーマが作成されます。

## [!DNL Marketo] 名前空間

ID名前空間は、IDが関連付けられるコンテキストのインジケーターとして機能する[[!DNL Identity Service]](../../../../identity-service/home.md)のコンポーネントです。

完全修飾 ID には、ID 値と名前空間が含まれます。新しい[!DNL Marketo]インスタンスとデータセットの組み合わせのそれぞれに対して、新しいカスタム名前空間が必要です。 例えば、`programs`データセットを取り込む[!DNL Marketo]ソースコネクタには独自のカスタム名前空間が必要で、同じデータセットを取り込む別のMarketoソースコネクタにも独自の新しいカスタム名前空間が必要です。 詳しくは、[名前空間の概要](../../../../identity-service/namespaces.md)を参照してください。

[!DNL Marketo]名前空間は、エンティティのプライマリIDで使用されます。

次の表に、[!DNL Marketo]名前空間の基本的な設定に関する情報を示します。

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 表示名 | 識別記号 | ID タイプ | 発行者のタイプ | 発行者のエンティティタイプ | Munchkin IDの例 |
| --- | --- | --- | --- | --- | --- |
| `marketo_person_{MUNCHKIN_ID}` | 自動生成された | `CROSS_DEVICE` | [!DNL Marketo] | `person` | `123-ABC-789` |
| `marketo_company_{MUNCHKIN_ID}` | 自動生成された | `B2B_ACCOUNT` | [!DNL Marketo] | `company` | `123-ABC-789` |
| `marketo_opportunity_{MUNCHKIN_ID}` | 自動生成された | `B2B_OPPORTUNITY` | [!DNL Marketo] | `opportunity` | `123-ABC-789` |
| `marketo_opportunity_contact_role_{MUNCHKIN_ID}` | 自動生成された | `B2B_OPPORTUNITY_PERSON` | [!DNL Marketo] | `opportunity contact role` | `123-ABC-789` |
| `marketo_program_{MUNCHKIN_ID}` | 自動生成された | `B2B_CAMPAIGN` | [!DNL Marketo] | `program` | `123-ABC-789` |
| `marketo_program_member_{MUNCHKIN_ID}` | 自動生成された | `B2B_CAMPAIGN_MEMBER` | [!DNL Marketo] | `program member` | `123-ABC-789` |
| `marketo_static_list_{MUNCHKIN_ID}` | 自動生成された | `B2B_MARKETING_LIST` | [!DNL Marketo] | `static list` | `123-ABC-789` |
| `marketo_static_list_member_{MUNCHKIN_ID}` | 自動生成された | `B2B_MARKETING_LIST_MEMBER` | [!DNL Marketo] | `static list member` | `123-ABC-789` |
| `marketo_named_account_{MUNCHKIN_ID}` | 自動生成された | `B2B_ACCOUNT` | [!DNL Marketo] | `named account` | `123-ABC-789` |

{style=&quot;table-layout:auto&quot;}

### [!DNL Salesforce] 名前空間

[!DNL Salesforce]統合をサブスクライブしている場合、[!DNL Salesforce]名前空間はエンティティのセカンダリIDで使用されます。

次の表に、[!DNL Salesforce]名前空間の基本的な設定に関する情報を示します。

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 表示名 | 識別記号 | ID タイプ | 発行者のタイプ | 発行者のエンティティタイプ | [!DNL Salesforce] 購読組織IDの例 |
| --- | --- | --- | --- | --- | --- |
| `salesforce_lead_{SALESFORCE_ORGANIZATION_ID}` | 自動生成された | `CROSS_DEVICE` | [!DNL Salesforce] | `lead` | `00DA0000000Hz79` |
| `salesforce_account_{SALESFORCE_ORGANIZATION_ID}` | 自動生成された | `B2B_ACCOUNT` | [!DNL Salesforce] | `account` | `00DA0000000Hz79` |
| `salesforce_opportunity_{SALESFORCE_ORGANIZATION_ID}` | 自動生成された | `B2B_OPPORTUNITY` | [!DNL Salesforce] | `opportunity` | `00DA0000000Hz79` |
| `salesforce_opportunity_contact_role_{SALESFORCE_ORGANIZATION_ID}` | 自動生成された | `B2B_OPPORTUNITY_PERSON` | [!DNL Salesforce] | `opportunity contact role` | `00DA0000000Hz79` |
| `salesforce_campaign_{SALESFORCE_ORGANIZATION_ID}` | 自動生成された | `B2B_CAMPAIGN` | [!DNL Salesforce] | `campaign` | `00DA0000000Hz79` |
| `salesforce_campaign_member_{SALESFORCE_ORGANIZATION_ID}` | 自動生成された | `B2B_CAMPAIGN_MEMBER` | [!DNL Salesforce] | `campaign member` | `00DA0000000Hz79` |

{style=&quot;table-layout:auto&quot;}

### [!DNL Microsoft Dynamics] 名前空間

[!DNL Dynamics]統合をサブスクライブしている場合、[!DNL Dynamics]名前空間はエンティティのセカンダリIDとして使用されます。

次の表に、[!DNL Dynamics]名前空間の基本的な設定に関する情報を示します。

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 表示名 | 識別記号 | ID タイプ | 発行者のタイプ | 発行者のエンティティタイプ | [!DNL Dynamics] 購読組織IDの例 |
| --- | --- | --- | --- | --- | --- |
| `microsoft_person_{DYNAMICS_ID}` | 自動生成された | `CROSS_DEVICE` | [!DNL Microsoft] | `person` | `94cahe38-e51h-3d57-a9c6-2edklb7184mh` |
| `microsoft_account_{DYNAMICS_ID}` | 自動生成された | `B2B_ACCOUNT` | [!DNL Microsoft] | `account` | `94cahe38-e51h-3d57-a9c6-2edklb7184mh` |
| `microsoft_opportunity_{DYNAMICS_ID}` | 自動生成された | `B2B_OPPORTUNITY` | [!DNL Microsoft] | `opportunity` | `94cahe38-e51h-3d57-a9c6-2edklb7184mh` |
| `microsoft_opportunity_contact_connection_{DYNAMICS_ID}` | 自動生成された | `B2B_OPPORTUNITY_PERSON` | [!DNL Microsoft] | `opportunity relationship` | `94cahe38-e51h-3d57-a9c6-2edklb7184mh` |
| `microsoft_campaign_{DYNAMICS_ID}` | 自動生成された | `B2B_CAMPAIGN` | [!DNL Microsoft] | `campaign` | `94cahe38-e51h-3d57-a9c6-2edklb7184mh` |
| `microsoft_campaign_member_{DYNAMICS_ID}` | 自動生成された | `B2B_CAMPAIGN_MEMBER` | [!DNL Microsoft] | `campaign member` | `94cahe38-e51h-3d57-a9c6-2edklb7184mh` |

{style=&quot;table-layout:auto&quot;}

## [!DNL Marketo] スキーマ

Experience Platform では、スキーマを使用して、一貫性のある再利用可能な方法でデータの構造を記述します。システムをまたいで一貫したデータを定義することで、意味を保有しやすくなり、データから価値を得ることができます。

データを Platform に取り込む前に、スキーマを構成して、データの構造を記述し、各フィールドに含めることができるデータの種類を制限する必要があります。スキーマは、基本クラスと、0個以上のスキーマフィールドグループで構成されます。

デザインの原則やベストプラクティスなど、スキーマ構成モデルについて詳しくは、[スキーマ構成の基本](../../../../xdm/schema/composition.md)を参照してください。

次の表に、[!DNL Marketo]スキーマの基本的な設定に関する情報を示します。

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| スキーマ名 | 基本クラス | フィールドグループ | [!DNL Profile] スキーマで | プライマリ同一性 | プライマリ同一性名前空間 | セカンダリ同一性 | セカンダリ同一性名前空間 | Relationship | 備考 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `[!DNL Marketo] Company {MUNCHKIN_ID}` | XDMビジネスアカウント | XDMビジネスアカウントの詳細 | 有効 | `accountID` 基本クラスで | `marketo_company_{MUNCHKIN_ID}` | `extSourceSystemAudit.externalID` 基本クラスで | `salesforce_account_{SALESFORCE_ORGANIZATION_ID}` | <ul><li>`accountParentID` （XDM Business Account Detailsフィールドグループ内）</li><li>タイプ：1対1</li><li>参照スキーマ:`[!DNL Marketo] Company {MUNCHKIN_ID}`</li><li>名前空間: `marketo_company_{MUNCHKIN_ID}`</li></ul> |
| `[!DNL Marketo] Person {MUNCHKIN_ID}` | XDM 個人プロファイル | <ul><li>XDMビジネス・パーソンの詳細</li><li>XDM Business Personコンポーネント</li><li>IdentityMap</li></ul> | 有効 | `personID` 基本クラスで | `marketo_person_{MUNCHKIN_ID}` | <ol><li>`extSourceSystemAudit.externalID` （XDM Business Person Detailsフィールドグループ）</li><li>`workEmail.address` （XDM Business Person Detailsフィールドグループ）</li><li>`identityMap` 「IDマップ」フィールドグループの</ol></li> | <ol><li>`salesforce_lead_{SALESFORCE_ORGANIZATION_ID}`</li><li>Email</li><li>ECID</li></ol> | <ul><li>`personComponents.sourceAccountID` （XDM Business Person Componentsフィールドグループ）</li><li>タイプ：多対1</li><li>参照スキーマ:`[!DNL Marketo] Company {MUNCHKIN_ID}`</li><li>名前空間: `marketo_company_{MUNCHKIN_ID}`</li><li>Destinationプロパティ：`accountID`</li><li>現在のスキーマからの関係名：アカウント</li><li>参照スキーマからの関係名：ユーザー</li></ul> |
| `[!DNL Marketo] Opportunity {MUNCHKIN_ID}` | XDMビジネス・オポチュニティ | XDMビジネス・オポチュニティの詳細 | 有効 | `opportunityID` 基本クラスで | `marketo_opportunity_{MUNCHKIN_ID}` | `extSourceSystemAudit.externalID` 基本クラスで | `salesforce_opportunity_{SALESFORCE_ORGANIZATION_ID}` | <ul><li>`accountID` 基本クラスで</li><li>タイプ：多対1</li><li>参照スキーマ:`[!DNL Marketo] Company {MUNCHKIN_ID}`</li><li>名前空間: `marketo_company_{MUNCHKIN_ID}`</li><li>Destinationプロパティ：`accountID`</li><li>現在のスキーマからの関係名：アカウント</li><li>参照スキーマからの関係名：オポチュニティ</li></ul> |
| `[!DNL Marketo] Opportunity Contact Role {MUNCHKIN_ID}` | XDM Business Opportunity Person関係 | なし | 有効 | `opportunityPersonID` 基本クラスで | `marketo_opportunity_contact_role_{MUNCHKIN_ID}` | `extSourceSystemAudit.externalID` 基本クラスで | `salesforce_opportunity_contact_role_{SALESFORCE_ORGANIZATION_ID}` | 最初の関係<ul><li>`personID` 基本クラスで</li><li>タイプ：多対1</li><li>参照スキーマ:`[!DNL Marketo] Person {MUNCHKIN_ID}`</li><li>名前空間: `marketo_person_{MUNCHKIN_ID}`</li><li>Destinationプロパティ：`personID`</li><li>現在のスキーマからの関係名：人</li><li>参照スキーマからの関係名：オポチュニティ</li></ul>第2の関係<ul><li>`opportunityID` 基本クラスで</li><li>タイプ：多対1</li><li>参照スキーマ:`[!DNL Marketo] Opportunity {MUNCHKIN_ID}`</li><li>名前空間: `marketo_opportunity_{MUNCHKIN_ID}`</li><li>Destinationプロパティ：`opportunityID`</li><li>現在のスキーマからの関係名：オポチュニティ</li><li>参照スキーマからの関係名：ユーザー</li></ul> |
| `[!DNL Marketo] Program {MUNCHKIN_ID}` | XDMビジネスキャンペーン | XDMビジネスキャンペーンの詳細 | 有効 | `campaignID` 基本クラスで | `marketo_program_{MUNCHKIN_ID}` | `extSourceSystemAudit.externalID` 基本クラスで | `salesforce_campaign_{SALESFORCE_ORGANIZATION_ID}` |
| `[!DNL Marketo] Program Member {MUNCHKIN_ID}` | XDMビジネスキャンペーンメンバ | XDMビジネスキャンペーンメンバの詳細 | 有効 | `campaignMemberID` 基本クラスで | `marketo_program_member_{MUNCHKIN_ID}` | なし | なし | 最初の関係<ul><li>`personID` 基本クラスで</li><li>タイプ：多対1</li><li>参照スキーマ:Marketoの人{MUNCHKIN_ID}</li><li>名前空間: `marketo_person_{MUNCHKIN_ID}`</li><li>Destinationプロパティ：`personID`</li><li>現在のスキーマからの関係名：人</li><li>参照スキーマからの関係名：プログラム</li></ul>第2の関係<ul><li>`campaignID` 基本クラスで</li><li>タイプ：多対1</li><li>参照スキーマ:`[!DNL Marketo] Program {MUNCHKIN_ID}`</li><li>名前空間: `marketo_program_{MUNCHKIN_ID}`</li><li>Destinationプロパティ：`campaignID`</li><li>現在のスキーマからの関係名：プログラム</li><li>参照スキーマからの関係名：ユーザー</li></ul> |
| `[!DNL Marketo] Static List {MUNCHKIN_ID}` | XDMビジネスマーケティングリスト | なし | 有効 | `marketingListID` 基本クラスで | `marketo_static_list_{MUNCHKIN_ID}` | なし | なし | なし | 静的なリストは[!DNL Salesforce]と同期されないため、セカンダリIDを持ちません。 |
| `[!DNL Marketo] Static List Member {MUNCHKIN_ID}` | XDMビジネスマーケティングリストメンバー | なし | 有効 | `marketingListMemberID` 基本クラスで | `marketo_static_list_member_{MUNCHKIN_ID}` | なし | なし | 最初の関係<ul><li>`personID` 基本クラスで</li><li>タイプ：多対1</li><li>参照スキーマ:`[!DNL Marketo] Person {MUNCHKIN_ID}`</li><li>名前空間: `marketo_person_{MUNCHKIN_ID}`</li><li>Destinationプロパティ：`personID`</li><li>現在のスキーマからの関係名：人</li><li>参照スキーマからの関係名：リスト</li></ul>第2の関係<ul><li>`marketingListID` 基本クラスで</li><li>タイプ：多対1</li><li>参照スキーマ:`[!DNL Marketo] Static List {MUNCHKIN_ID}`</li><li>名前空間: `marketo_static_list_{MUNCHKIN_ID}`</li><li>Destinationプロパティ：`marketingListID`</li><li>現在のスキーマからの関係名：リスト</li><li>参照スキーマからの関係名：ユーザー</li></ul> | 静的なリストメンバは[!DNL Salesforce]と同期されないため、セカンダリIDを持ちません。 |
| `[!DNL Marketo] Named Account {MUNCHKIN_ID}` | XDMビジネスアカウント | XDMビジネスアカウントの詳細 | 有効 | `accountID` 基本クラスで | `marketo_named_account_{MUNCHKIN_ID}` | `extSourceSystemAudit.externalID` 基本クラスで | `salesforce_account_{SALESFORCE_ORGANIZATION_ID}` | <ul><li>`accountParentID` （XDM Business Account Detailsフィールドグループ内）</li><li>タイプ：1対1</li><li>参照スキーマ:`[!DNL Marketo] Named Account {MUNCHKIN_ID}`</li><li>名前空間: `marketo_named_account_{MUNCHKIN_ID}` |
| [!DNL Marketo] アクティビティ `{MUNCHKIN ID}` | XDM ExperienceEvent | <ul><li>Webページにアクセス</li><li>新しいリード</li><li>リードの変換</li><li>リスト追加へ</li><li>リストから削除</li><li>To Opportunity</li><li>オポチュニティから削除</li><li>入力済みのフォーム</li><li>リンククリック数</li><li>電子メール配信</li><li>開封済み電子メール</li><li>電子メールがクリックされました</li><li>電子メールのバウンス</li><li>電子メールのバウンス（ソフト）</li><li>Email Unsubscribed</li><li>スコアの変更</li><li>オポチュニティの更新</li><li>キャンペーンの進行状況の変更</li><li>個人ID</li><li>MarketoウェブURL | 有効 | `personID` （Person Identifierフィールドグループ） | `marketo_person_{MUNCHKIN_ID}` | なし | なし | 最初の関係<ul><li>`listOperations.listID` field</li><li>タイプ：1対1</li><li>参照スキーマ:`[!DNL Marketo] Static List {MUNCHKIN_ID}`</li><li>名前空間: `marketo_static_list_{MUNCHKIN_ID}`</li></ul>第2の関係<ul><li>`opportunityEvent.opportunityID` field</li><li>タイプ：1対1</li><li>参照スキーマ:`[!DNL Marketo] Opportunity {MUNCHKIN_ID}`</li><li>名前空間: `marketo_opportunity_{MUNCHKIN_ID}`</li></ul>3番目の関係<ul><li>`leadOperation.campaignProgression.campaignID` field</li><li>タイプ：1対1</li><li>参照スキーマ:`[!DNL Marketo] Program {MUNCHKIN_ID}`</li><li>名前空間: `marketo_program_{MUNCHKIN_ID}`</li></ul> | `[!DNL Marketo] Activity {MUNCHKIN_ID}`スキーマの主IDは`personID`です。これは`[!DNL Marketo] Person {MUNCHKIN_ID}`スキーマの主IDと同じです。 |

{style=&quot;table-layout:auto&quot;}

## 次の手順

[!DNL Marketo]データをプラットフォームに接続する方法については、[UI](../../../tutorials/ui/create/adobe-applications/marketo.md)でのMarketoソースコネクタの作成に関するチュートリアルを参照してください。
