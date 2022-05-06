---
keywords: Experience Platform；ホーム；人気の高いトピック；Marketoソースコネクタ；名前空間；スキーマ；b2b;B2B
solution: Experience Platform
title: B2B 名前空間とスキーマ
topic-legacy: overview
description: このドキュメントでは、B2B ソースコネクタの作成時に必要なカスタム名前空間の概要を説明します。
exl-id: f1592be5-987e-41b8-9844-9dea5bd452b9
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '1688'
ht-degree: 14%

---

# B2B 名前空間とスキーマ

このドキュメントでは、B2B ソースで使用する名前空間とスキーマの基になる設定に関する情報を提供します。 このドキュメントでは、B2B 名前空間とスキーマの生成に必要なPostman自動化ユーティリティの設定に関する詳細も説明します。

>[!IMPORTANT]
>
>次へのアクセス権が必要です： [Real-time Customer Data Platform B2B エディション](../../../../rtcdp/b2b-overview.md) B2B スキーマがに参加するために [リアルタイム顧客プロファイル](../../../../profile/home.md).

## B2B 名前空間とスキーマ自動生成ユーティリティの設定

B2B 名前空間とスキーマ自動生成ユーティリティを使用する最初の手順は、Platform デベロッパーコンソールを設定し、 [!DNL Postman] 環境。

- このから、名前空間とスキーマの自動生成ユーティリティのコレクションと環境をダウンロードできます [GitHub リポジトリ](https://github.com/adobe/experience-platform-postman-samples/tree/master/Postman%20Collections/CDP%20Namespaces%20and%20Schemas%20Utility).
- 必要なヘッダーの値の収集方法の詳細や API 呼び出し例の読み取りなど、Platform API の使用に関する詳細については、 [Platform API の概要](../../../../landing/api-guide.md).
- Platform API の資格情報を生成する方法について詳しくは、 [Experience PlatformAPI の認証とアクセス](../../../../landing/api-authentication.md).
- の設定方法について詳しくは、 [!DNL Postman] Platform API については、 [開発者コンソールの設定および [!DNL Postman]](../../../../landing/postman.md).

Platform デベロッパーコンソールと [!DNL Postman] を設定すると、適切な環境値を [!DNL Postman] 環境。

次の表に、値の例と、 [!DNL Postman] 環境：

| 変数 | 説明 | 例 |
| --- | --- | --- |
| `CLIENT_SECRET` | の生成に使用される一意の ID `{ACCESS_TOKEN}`. に関するチュートリアルを参照してください。 [Experience PlatformAPI の認証とアクセス](../../../../landing/api-authentication.md) 」を参照してください。 `{CLIENT_SECRET}`. | `{CLIENT_SECRET}` |
| `JWT_TOKEN` | JSON Web トークン (JWT) は、{ACCESS_TOKEN} の生成に使用される認証資格情報です。 に関するチュートリアルを参照してください。 [Experience PlatformAPI の認証とアクセス](../../../../landing/api-authentication.md) 」を参照してください。 `{JWT_TOKEN}`. | `{JWT_TOKEN}` |
| `API_KEY` | Experience PlatformAPI の呼び出しを認証するために使用される一意の識別子。 に関するチュートリアルを参照してください。 [Experience PlatformAPI の認証とアクセス](../../../../landing/api-authentication.md) 」を参照してください。 `{API_KEY}`. | `c8d9a2f5c1e03789bd22e8efdd1bdc1b` |
| `ACCESS_TOKEN` | Experience PlatformAPI の呼び出しを完了するために必要な認証トークン。 に関するチュートリアルを参照してください。 [Experience PlatformAPI の認証とアクセス](../../../../landing/api-authentication.md) 」を参照してください。 `{ACCESS_TOKEN}`. | `Bearer {ACCESS_TOKEN}` |
| `META_SCOPE` | 次に関して： [!DNL Marketo]の場合、この値は固定で、常に次の値に設定されます。 `ent_dataservices_sdk`. | `ent_dataservices_sdk` |
| `CONTAINER_ID` | この `global` コンテナには、すべての標準AdobeおよびExperience Platformパートナーが提供するクラス、スキーマフィールドグループ、データ型、スキーマが格納されます。 次に関して： [!DNL Marketo]の場合、この値は固定値で、常に `global`. | `global` |
| `PRIVATE_KEY` | 認証に使用する資格情報 [!DNL Postman] インスタンスからExperience PlatformAPI へ 開発者コンソールの設定に関するチュートリアルと [開発者コンソールの設定および [!DNL Postman]](../../../../landing/postman.md) を参照してください。 | `{PRIVATE_KEY}` |
| `TECHNICAL_ACCOUNT_ID` | Adobe I/Oへの統合に使用する資格情報。 | `D42AEVJZTTJC6LZADUBVPA15@techacct.adobe.com` |
| `IMS` | Identity Management System(IMS) は、Adobe サービスに対する認証のフレームワークを提供します。 次に関して： [!DNL Marketo]の場合、この値は固定で、常に次の値に設定されます。 `ims-na1.adobelogin.com`. | `ims-na1.adobelogin.com` |
| `IMS_ORG` | 製品やサービスを所有またはライセンスし、そのメンバーへのアクセスを許可できる企業エンティティ。 に関するチュートリアルを参照してください。 [開発者コンソールの設定および [!DNL Postman]](../../../../landing/postman.md) を参照してください。 `{ORG_ID}` 情報。 | `ABCEH0D9KX6A7WA7ATQE0TE@adobeOrg` |
| `SANDBOX_NAME` | 使用している仮想サンドボックスパーティションの名前です。 | `prod` |
| `TENANT_ID` | 作成するリソースの名前空間が適切に付けられ、IMS 組織内に含まれるようにするために使用される ID。 | `b2bcdpproductiontest` |
| `PLATFORM_URL` | API 呼び出しをおこなう URL エンドポイント。 この値は固定で、常に次の値に設定されます。 `http://platform.adobe.io/`. | `http://platform.adobe.io/` |

{style=&quot;table-layout:auto&quot;}

### スクリプトの実行

を [!DNL Postman] コレクションと環境の設定時に、 [!DNL Postman] インターフェイス。

内 [!DNL Postman] インタフェースで、auto-generator ユーティリティのルートフォルダを選択し、 **[!DNL Run]** を上部のヘッダーから削除します。

![root-folder](../images/marketo/root-folder.png)

この [!DNL Runner] インターフェイスが表示されます。 ここから、すべてのチェックボックスが選択されていることを確認してから、「 」を選択します。 **[!DNL Run Namespaces and Schemas Autogeneration Utility]**.

![ランジェネレーター](../images/marketo/run-generator.png)

リクエストが成功すると、B2B に必要な名前空間とスキーマが作成されます。

## B2B 名前空間

ID 名前空間は、 [[!DNL Identity Service]](../../../../identity-service/home.md) id のコンテキストまたはタイプを区別するために役立つ 完全修飾 ID には、ID 値と名前空間が含まれます。詳しくは、 [名前空間の概要](../../../../identity-service/namespaces.md) を参照してください。

B2B 名前空間は、エンティティのプライマリ ID で使用されます。

次の表に、B2B 名前空間の基になる設定に関する情報を示します。

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 表示名 | ID シンボル | ID タイプ |
| --- | --- | --- |
| B2B 担当者 | `b2b_person` | `CROSS_DEVICE` |
| B2B アカウント | `b2b_account` | `B2B_ACCOUNT` |
| B2B 商談 | `b2b_opportunity` | `B2B_OPPORTUNITY` |
| B2B 商談人物関係 | `b2b_opportunity_person_relation` | `B2B_OPPORTUNITY_PERSON` |
| B2B キャンペーン | `b2b_campaign` | `B2B_CAMPAIGN` |
| B2B キャンペーンメンバー | `b2b_campaign_member` | `B2B_CAMPAIGN_MEMBER` |
| B2B マーケティングリスト | `b2b_marketing_list` | `B2B_MARKETING_LIST` |
| B2B マーケティングリストメンバー | `b2b_marketing_list_member` | `B2B_MARKETING_LIST_MEMBER` |
| B2B アカウント人物関係 | `b2b_account_person_relation` | `B2B_ACCOUNT_PERSON` |

{style=&quot;table-layout:auto&quot;}

## B2B スキーマ

Experience Platform では、スキーマを使用して、一貫性のある再利用可能な方法でデータの構造を記述します。システムをまたいで一貫したデータを定義することで、意味を保有しやすくなり、データから価値を得ることができます。

データを Platform に取り込む前に、スキーマを構成して、データの構造を記述し、各フィールドに含めることができるデータの種類を制限する必要があります。スキーマは、基本クラスと 0 個以上のスキーマフィールドグループで構成されます。

デザインの原則やベストプラクティスなど、スキーマ構成モデルについて詳しくは、[スキーマ構成の基本](../../../../xdm/schema/composition.md)を参照してください。

次の表に、B2B スキーマの基礎となる設定に関する情報を示します。

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| スキーマ名 | 基本クラス | フィールドグループ | [!DNL Profile] スキーマ内 | プライマリID | プライマリID 名前空間 | セカンダリ | セカンダリid 名前空間 | 関係 | 備考 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| B2B アカウント | [XDM Business Account](../../../../xdm/classes/b2b/business-account.md) | XDM ビジネスアカウントの詳細 | 有効 | `accountKey.sourceKey` 基本クラスで | B2B アカウント | `extSourceSystemAudit.externalKey.sourceKey` 基本クラスで | B2B アカウント | <ul><li>`accountParentKey.sourceKey` 「 XDM Business Account Details 」フィールドグループ</li><li>宛先プロパティ： `/accountKey/sourceKey`</li><li>タイプ：1 対 1</li><li>参照スキーマ：B2B アカウント</li><li>名前空間：B2B アカウント</li></ul> |
| B2B 担当者 | [XDM 個人プロファイル](../../../../xdm/classes/individual-profile.md) | <ul><li>XDM ビジネスパーソンの詳細</li><li>XDM ビジネスパーソンのコンポーネント</li><li>identityMap</li><li>同意および環境設定の詳細</li></ul> | 有効 | `b2b.personKey.sourceKey` （XDM ビジネス人物詳細フィールドグループ） | B2B 担当者 | <ol><li>`extSourceSystemAudit.externalKey.sourceKey` （XDM ビジネス人物詳細フィールドグループ）</li><li>`workEmail.address` （XDM ビジネス人物詳細フィールドグループ）</ol></li> | <ol><li>B2B 担当者</li><li>メール</li></ol> | <ul><li>`personComponents.sourceAccountKey.sourceKey` （XDM ビジネス人物コンポーネントフィールドグループ）</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B アカウント</li><li>名前空間：B2B アカウント</li><li>宛先プロパティ：accountKey.sourceKey</li><li>現在のスキーマからの関係名：アカウント</li><li>参照スキーマからの関係名：人</li></ul> |
| B2B 商談 | [XDM Business Opportunity](../../../../xdm/classes/b2b/business-opportunity.md) | XDM ビジネスオポチュニティの詳細 | 有効 | `opportunityKey.sourceKey` 基本クラスで | B2B 商談 | `extSourceSystemAudit.externalKey.sourceKey` 基本クラスで | B2B 商談 | <ul><li>`accountKey.sourceKey` 基本クラスで</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B アカウント</li><li>名前空間：B2B アカウント</li><li>宛先プロパティ： `accountKey.sourceKey`</li><li>現在のスキーマからの関係名：アカウント</li><li>参照スキーマからの関係名：商談</li></ul> |
| B2B 商談人物関係 | [XDM Business Opportunity Person Relation](../../../../xdm/classes/b2b/business-opportunity-person-relation.md) | なし | 有効 | `opportunityPersonKey.sourceKey` 基本クラスで | B2B 商談人物関係 | `extSourceSystemAudit.externalKey.sourceKey` 基本クラスで | B2B 商談人物関係 | **最初の関係**<ul><li>`personKey.sourceKey` 基本クラスで</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B 担当者</li><li>名前空間：B2B 担当者</li><li>宛先プロパティ：b2b.personKey.sourceKey</li><li>現在のスキーマからの関係名：人物</li><li>参照スキーマからの関係名：商談</li></ul>**2 番目の関係**<ul><li>`opportunityKey.sourceKey` 基本クラスで</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B 商談 </li><li>名前空間：B2B 商談 </li><li>宛先プロパティ： `opportunityKey.sourceKey`</li><li>現在のスキーマからの関係名：商談</li><li>参照スキーマからの関係名：人</li></ul> |
| B2B キャンペーン | [XDM Business Campaign](../../../../xdm/classes/b2b/business-campaign.md) | XDM ビジネスキャンペーンの詳細 | 有効 | `campaignKey.sourceKey` 基本クラスで | B2B キャンペーン | `extSourceSystemAudit.externalKey.sourceKey` 基本クラスで | B2B キャンペーン |
| B2B キャンペーンメンバー | [XDM Business Campaign Members](../../../../xdm/classes/b2b/business-campaign-members.md) | XDM ビジネスキャンペーンメンバーの詳細 | 有効 | `ccampaignMemberKey.sourceKey` 基本クラスで | B2B キャンペーンメンバー | `extSourceSystemAudit.externalKey.sourceKey` 基本クラスで | B2B キャンペーンメンバー | **最初の関係**<ul><li>`personKey.sourceKey` 基本クラスで</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B 担当者</li><li>名前空間：B2B 担当者</li><li>宛先プロパティ： `b2b.personKey.sourceKey`</li><li>現在のスキーマからの関係名：人物</li><li>参照スキーマからの関係名：キャンペーン</li></ul>**2 番目の関係**<ul><li>`campaignKey.sourceKey` 基本クラスで</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B キャンペーン</li><li>名前空間：B2B キャンペーン</li><li>宛先プロパティ： `campaignKey.sourceKey`</li><li>現在のスキーマからの関係名：Campaign</li><li>参照スキーマからの関係名：人</li></ul> |
| B2B マーケティングリスト | [XDM Business Marketing List](../../../../xdm/classes/b2b/business-marketing-list.md) | なし | 有効 | `marketingListKey.sourceKey` 基本クラスで | B2B マーケティングリスト | なし | なし | なし | 静的リストはから同期されていません [!DNL Salesforce] したがって、セカンダリ ID を持ちません。 |
| B2B マーケティングリストメンバー | [XDM Business Marketing List Members](../../../../xdm/classes/b2b/business-marketing-list-members.md) | なし | 有効 | `marketingListMemberKey.sourceKey` 基本クラスで | B2B マーケティングリストメンバー | なし | なし | **最初の関係**<ul><li>`PersonKey.sourceKey` 基本クラスで</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B 担当者</li><li>名前空間：B2B 担当者</li><li>宛先プロパティ： `b2b.personKey.sourceKey`</li><li>現在のスキーマからの関係名：人物</li><li>参照スキーマからの関係名：マーケティングリスト</li></ul>**2 番目の関係**<ul><li>`marketingListKey.sourceKey` 基本クラスで</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B マーケティングリスト</li><li>名前空間：B2B マーケティングリスト</li><li>宛先プロパティ： `marketingListKey.sourceKey`</li><li>現在のスキーマからの関係名：マーケティングリスト</li><li>参照スキーマからの関係名：人</li></ul> | 静的リストメンバーはから同期されていません [!DNL Salesforce] したがって、セカンダリ ID を持ちません。 |
| B2B アクティビティ | [XDM ExperienceEvent](../../../../xdm/classes/experienceevent.md) | <ul><li>Web ページにアクセス</li><li>新規リード</li><li>リードの変換</li><li>リストに追加</li><li>リストから削除</li><li>商談に追加</li><li>商談から削除</li><li>フォーム入力済み</li><li>リンククリック数</li><li>メール配信済み</li><li>メール開封済み</li><li>メールのクリック</li><li>バウンスメール</li><li>ソフトバウンスメール</li><li>メール配信停止済み</li><li>変更されたスコア</li><li>更新された商談</li><li>キャンペーン進行のステータス変更済み</li><li>人物識別子</li><li>Marketo Web URL</li><li>注目のアクション</li></ul> | 有効 | `personKey.sourceKey` 担当者識別子フィールドグループの | B2B 担当者 | なし | なし | **最初の関係**<ul><li>`listOperations.listKey.sourceKey` field</li><li>タイプ：1 対 1</li><li>参照スキーマ：B2B マーケティングリスト</li><li>名前空間：B2B マーケティングリスト</li></ul>**2 番目の関係**<ul><li>`opportunityEvent.opportunityKey.sourceKey` フィールド</li><li>タイプ：1 対 1</li><li>参照スキーマ：B2B 商談</li><li>名前空間：B2B 商談</li></ul>**第 3 の関係**<ul><li>`leadOperation.campaignProgression.campaignKey.sourceKey` フィールド</li><li>タイプ：1 対 1</li><li>参照スキーマ：B2B キャンペーン</li><li>名前空間：B2B キャンペーン</li></ul> | `ExperienceEvent` はエンティティとは異なります。 エクスペリエンスイベントの ID は、アクティビティを実行した人です。 |
| B2B アカウント人物関係 | [XDM Business Account Person Relation](../../../../xdm/classes/b2b/business-account-person-relation.md) | ID マップ | 有効 | `accountPersonKey.sourceKey` 基本クラスで | B2B アカウント人物関係 | なし | なし | **最初の関係**<ul><li>`personKey.sourceKey` 基本クラスで</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B 担当者</li><li>名前空間：B2B 担当者</li><li>宛先プロパティ： `b2b.personKey.SourceKey`</li><li>現在のスキーマからの関係名：人</li><li>参照スキーマからの関係名：アカウント</li></ul>**2 番目の関係**<ul><li>`accountKey.sourceKey` 基本クラスで</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B アカウント</li><li>名前空間：B2B アカウント</li><li>宛先プロパティ： `accountKey.sourceKey`</li><li>現在のスキーマからの関係名：アカウント</li><li>参照スキーマからの関係名：人</li></ul> |

{style=&quot;table-layout:auto&quot;}

## 次の手順

次の手順で [!DNL Marketo] データを Platform に送信する場合は、 [UI でのMarketoソースコネクタの作成](../../../tutorials/ui/create/adobe-applications/marketo.md).
