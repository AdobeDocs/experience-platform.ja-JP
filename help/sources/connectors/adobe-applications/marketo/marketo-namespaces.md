---
title: B2B 名前空間とスキーマ
description: このドキュメントでは、B2B ソースコネクタの作成時に必要なカスタム名前空間の概要を説明します。
exl-id: f1592be5-987e-41b8-9844-9dea5bd452b9
source-git-commit: 5e8bb04ca18159eab98b2f7f0bba8cb1488a1f26
workflow-type: tm+mt
source-wordcount: '1620'
ht-degree: 18%

---

# B2B 名前空間とスキーマ

>[!NOTE]
>
>Adobe Experience Platform UI でテンプレートを使用すると、B2B および B2C データのアセット作成を迅速化できます。 詳しくは、[Platform UI でのテンプレートの使用 ](../../../tutorials/ui/templates.md) に関するガイドを参照してください。

このドキュメントでは、B2B ソースで使用する名前空間およびスキーマの基盤となるセットアップに関する情報を提供します。 このドキュメントでは、B2B 名前空間とスキーマを生成するために必要なPostman自動化ユーティリティの設定に関する詳細も説明します。

>[!IMPORTANT]
>
>B2B スキーマを { リアルタイム顧客プロファイル ](../../../../rtcdp/b2b-overview.md) に加えるには、{0[Adobe Real-time Customer Data Platform B2B Edition](../../../../profile/home.md) へのアクセス権が必要です。[

## B2B 名前空間とスキーマ自動生成ユーティリティの設定

B2B 名前空間とスキーマ自動生成ユーティリティを使用する最初の手順は、Platform 開発者コンソールと [!DNL Postman] 環境を設定することです。

- この [GitHub リポジトリ ](https://github.com/adobe/experience-platform-postman-samples/tree/master/Postman%20Collections/CDP%20Namespaces%20and%20Schemas%20Utility) から、名前空間およびスキーマ自動生成ユーティリティのコレクションと環境をダウンロードできます。
- 必要なヘッダーの値を収集する方法やサンプル API 呼び出しを読み取る方法など、Platform API の使用について詳しくは、[Platform API の概要 ](../../../../landing/api-guide.md) を参照してください。
- Platform API の資格情報の生成方法について詳しくは、[Experience PlatformAPI の認証とアクセス ](../../../../landing/api-authentication.md) に関するチュートリアルを参照してください。
- Platform API の [!DNL Postman] の設定方法について詳しくは、[ 開発者コンソールとの設定  [!DNL Postman]](../../../../landing/postman.md) のチュートリアルを参照してください。

Platform 開発者コンソールをセットアップす [!DNL Postman] と、適切な環境値の [!DNL Postman] 環境への適用を開始できます。

次の表に、値の例と、[!DNL Postman] 環境へのデータ入力に関する追加情報を示します。

| 変数 | 説明 | 例 |
| --- | --- | --- |
| `CLIENT_SECRET` | `{ACCESS_TOKEN}` ータの生成に使用される一意の ID。 サー `{CLIENT_SECRET}` スの取得方法について詳しくは、[Experience Platform API の認証とアクセス ](../../../../landing/api-authentication.md) に関するチュートリアルを参照してください。 | `{CLIENT_SECRET}` |
| `JWT_TOKEN` | JSON web トークン（JWT）は、{ACCESS_TOKEN} ータの生成に使用される認証資格情報です。 サー `{JWT_TOKEN}` スの生成方法について詳しくは、[Experience PlatformAPI の認証とアクセス ](../../../../landing/api-authentication.md) に関するチュートリアルを参照してください。 | `{JWT_TOKEN}` |
| `API_KEY` | Experience PlatformAPI への呼び出しの認証に使用される一意の ID。 サー `{API_KEY}` スの取得方法について詳しくは、[Experience Platform API の認証とアクセス ](../../../../landing/api-authentication.md) に関するチュートリアルを参照してください。 | `c8d9a2f5c1e03789bd22e8efdd1bdc1b` |
| `ACCESS_TOKEN` | Experience PlatformAPI を呼び出すために必要な認証トークン。 サー `{ACCESS_TOKEN}` スの取得方法について詳しくは、[Experience Platform API の認証とアクセス ](../../../../landing/api-authentication.md) に関するチュートリアルを参照してください。 | `Bearer {ACCESS_TOKEN}` |
| `META_SCOPE` | [!DNL Marketo] に関しては、この値は固定で、常に `ent_dataservices_sdk` に設定されます。 | `ent_dataservices_sdk` |
| `CONTAINER_ID` | `global` コンテナには、標準AdobeおよびExperience Platformパートナー提供のすべてのクラス、スキーマフィールドグループ、データタイプおよびスキーマが格納されます。 [!DNL Marketo] に関しては、この値は固定で、常に `global` に設定されます。 | `global` |
| `PRIVATE_KEY` | API に対する [!DNL Postman] インスタンスのExperience Platformに使用される資格情報。 コンテン {PRIVATE_KEY} の取得方法については、開発者コンソールの設定および [ 開発者コンソールの設定および  [!DNL Postman]](../../../../landing/postman.md) に関するチュートリアルを参照してください。 | `{PRIVATE_KEY}` |
| `TECHNICAL_ACCOUNT_ID` | Adobe I/Oへの統合に使用する資格情報。 | `D42AEVJZTTJC6LZADUBVPA15@techacct.adobe.com` |
| `IMS` | Identity Management System （IMS）は、Adobe サービスに対して認証を行うためのフレームワークを提供します。 [!DNL Marketo] に関しては、この値は固定で、常に `ims-na1.adobelogin.com` に設定されます。 | `ims-na1.adobelogin.com` |
| `IMS_ORG` | 製品およびサービスを所有またはライセンスし、そのメンバーへのアクセスを許可できる法人組織。 `{ORG_ID}` ーザー情報の取得方法については、[Developer Console の設定および  [!DNL Postman]](../../../../landing/postman.md) に関するチュートリアルを参照してください。 | `ABCEH0D9KX6A7WA7ATQE0TE@adobeOrg` |
| `SANDBOX_NAME` | 使用している仮想サンドボックスパーティションの名前。 | `prod` |
| `TENANT_ID` | 作成するリソースの名前空間が適切に設定され、組織内に含まれていることを確認するために使用される ID。 | `b2bcdpproductiontest` |
| `PLATFORM_URL` | API 呼び出しを行う URL エンドポイント。 この値は固定で、常に `http://platform.adobe.io/` に設定されます。 | `http://platform.adobe.io/` |

{style="table-layout:auto"}

### スクリプトの実行

[!DNL Postman] コレクションと環境を設定すると、[!DNL Postman] インターフェイスを使用してスクリプトを実行できます。

[!DNL Postman] インターフェイスで、自動生成ユーティリティのルートフォルダーを選択し、上部のヘッダーから「**[!DNL Run]**」を選択します。

![root-folder](../images/marketo/root-folder.png)

[!DNL Runner] インターフェイスが表示されます。 ここから、すべてのチェックボックスが選択されていることを確認してから選択し **[!DNL Run Namespaces and Schemas Autogeneration Utility]** す。

![run-generator](../images/marketo/run-generator.png)

リクエストが成功すると、B2B に必要な名前空間とスキーマが作成されます。

## B2B 名前空間

ID 名前空間は、ID のコンテキストを区別するのに役立つ [[!DNL Identity Service]](../../../../identity-service/home.md) のコンポーネントです。 完全修飾 ID には、ID 値と名前空間が含まれます。 詳しくは、[ 名前空間の概要 ](../../../../identity-service/features/namespaces.md) を参照してください。

B2B 名前空間は、エンティティのプライマリ ID で使用されます。

B2B 名前空間の基盤となるセットアップに関する情報を次の表に示します。

>[!NOTE]
>
>左右にスクロールして、テーブルの内容をすべて表示してください。

| 表示名 | ID シンボル | ID タイプ |
| --- | --- | --- |
| B2B ユーザー | `b2b_person` | `CROSS_DEVICE` |
| B2B アカウント | `b2b_account` | `B2B_ACCOUNT` |
| B2B オポチュニティ | `b2b_opportunity` | `B2B_OPPORTUNITY` |
| B2B オポチュニティ人物関係 | `b2b_opportunity_person_relation` | `B2B_OPPORTUNITY_PERSON` |
| B2B キャンペーン | `b2b_campaign` | `B2B_CAMPAIGN` |
| B2B キャンペーンメンバー | `b2b_campaign_member` | `B2B_CAMPAIGN_MEMBER` |
| B2B マーケティングリスト | `b2b_marketing_list` | `B2B_MARKETING_LIST` |
| B2B マーケティングリストメンバー | `b2b_marketing_list_member` | `B2B_MARKETING_LIST_MEMBER` |
| B2B アカウント人物関係 | `b2b_account_person_relation` | `B2B_ACCOUNT_PERSON` |

{style="table-layout:auto"}

## B2B スキーマ

Experience Platform では、スキーマを使用して、一貫性のある再利用可能な方法でデータの構造を記述します。システムをまたいで一貫したデータを定義することで、意味を保有しやすくなり、データから価値を得ることができます。

データを Platform に取り込む前に、スキーマを構成して、データの構造を記述し、各フィールドに含めることができるデータの種類を制限する必要があります。スキーマは、基本クラスと 0 個以上のスキーマフィールドグループで構成されます。

デザインの原則やベストプラクティスなど、スキーマ構成モデルについて詳しくは、[スキーマ構成の基本](../../../../xdm/schema/composition.md)を参照してください。

B2B スキーマの基盤となる設定に関する情報を次の表に示します。

>[!NOTE]
>
>左右にスクロールして、テーブルの内容をすべて表示してください。

| スキーマ名 | 基本クラス | フィールドグループ | スキーマ内の [!DNL Profile] | プライマリ ID | プライマリ ID 名前空間 | セカンダリID | セカンダリid 名前空間 | 関係 | メモ |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| B2B アカウント | [XDM ビジネスアカウント ](../../../../xdm/classes/b2b/business-account.md) | XDM ビジネスアカウントの詳細 | 有効 | 基本クラスの `accountKey.sourceKey` | B2B アカウント | 基本クラスの `extSourceSystemAudit.externalKey.sourceKey` | B2B アカウント | <ul><li>XDM ビジネスアカウントの詳細フィールドグループの `accountParentKey.sourceKey`</li><li>宛先のプロパティ：`/accountKey/sourceKey`</li><li>タイプ : 1 対 1</li><li>参照スキーマ：B2B アカウント</li><li>名前空間：B2B アカウント</li></ul> |
| B2B ユーザー | [XDM 個人プロファイル](../../../../xdm/classes/individual-profile.md) | <ul><li>XDM ビジネスパーソンの詳細</li><li>XDM ビジネスパーソンのコンポーネント</li><li>identityMap</li><li>同意と好みの詳細</li></ul> | 有効 | XDM ビジネス人物の詳細フィールドグループの `b2b.personKey.sourceKey` | B2B ユーザー | <ol><li>XDM ビジネス人物の詳細フィールドグループの `extSourceSystemAudit.externalKey.sourceKey`</li><li>XDM ビジネス人物の詳細フィールドグループの `workEmail.address`</ol></li> | <ol><li>B2B ユーザー</li><li>メール</li></ol> | <ul><li>XDM ビジネスユーザーコンポーネントフィールドグループの `personComponents.sourceAccountKey.sourceKey`</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B アカウント</li><li>名前空間：B2B アカウント</li><li>宛先プロパティ：accountKey.sourceKey</li><li>現在のスキーマからの関係名：アカウント</li><li>参照スキーマからの関係名：人物</li></ul> |
| B2B オポチュニティ | [XDM ビジネスオポチュニティ ](../../../../xdm/classes/b2b/business-opportunity.md) | XDM ビジネスオポチュニティの詳細 | 有効 | 基本クラスの `opportunityKey.sourceKey` | B2B オポチュニティ | 基本クラスの `extSourceSystemAudit.externalKey.sourceKey` | B2B オポチュニティ | <ul><li>基本クラスの `accountKey.sourceKey`</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B アカウント</li><li>名前空間：B2B アカウント</li><li>宛先のプロパティ：`accountKey.sourceKey`</li><li>現在のスキーマからの関係名：アカウント</li><li>参照スキーマからの関係名：商談</li></ul> |
| B2B オポチュニティ人物関係 | [XDM ビジネスオポチュニティ人物関係 ](../../../../xdm/classes/b2b/business-opportunity-person-relation.md) | なし | 有効 | 基本クラスの `opportunityPersonKey.sourceKey` | B2B オポチュニティ人物関係 | 基本クラスの `extSourceSystemAudit.externalKey.sourceKey` | B2B オポチュニティ人物関係 | **最初の関係**<ul><li>基本クラスの `personKey.sourceKey`</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B Person</li><li>名前空間：B2B Person</li><li>宛先プロパティ：b2b.personKey.sourceKey</li><li>現在のスキーマからの関係名：人物</li><li>参照スキーマからの関係名：商談</li></ul>**2 番目の関係**<ul><li>基本クラスの `opportunityKey.sourceKey`</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B オポチュニティ </li><li>名前空間：B2B オポチュニティ </li><li>宛先のプロパティ：`opportunityKey.sourceKey`</li><li>現在のスキーマからの関係名：商談</li><li>参照スキーマからの関係名：人物</li></ul> |
| B2B キャンペーン | [XDM ビジネスキャンペーン ](../../../../xdm/classes/b2b/business-campaign.md) | XDM ビジネスキャンペーンの詳細 | 有効 | 基本クラスの `campaignKey.sourceKey` | B2B キャンペーン | 基本クラスの `extSourceSystemAudit.externalKey.sourceKey` | B2B キャンペーン |
| B2B キャンペーンメンバー | [XDM ビジネスキャンペーンメンバー ](../../../../xdm/classes/b2b/business-campaign-members.md) | XDM ビジネスキャンペーンメンバーの詳細 | 有効 | 基本クラスの `ccampaignMemberKey.sourceKey` | B2B キャンペーンメンバー | 基本クラスの `extSourceSystemAudit.externalKey.sourceKey` | B2B キャンペーンメンバー | **最初の関係**<ul><li>基本クラスの `personKey.sourceKey`</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B Person</li><li>名前空間：B2B Person</li><li>宛先のプロパティ：`b2b.personKey.sourceKey`</li><li>現在のスキーマからの関係名：人物</li><li>参照スキーマからの関係名：キャンペーン</li></ul>**2 番目の関係**<ul><li>基本クラスの `campaignKey.sourceKey`</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B キャンペーン</li><li>名前空間：B2B キャンペーン</li><li>宛先のプロパティ：`campaignKey.sourceKey`</li><li>現在のスキーマからの関係名：Campaign</li><li>参照スキーマからの関係名：人物</li></ul> |
| B2B マーケティングリスト | [XDM ビジネスマーケティングリスト ](../../../../xdm/classes/b2b/business-marketing-list.md) | なし | 有効 | 基本クラスの `marketingListKey.sourceKey` | B2B マーケティングリスト | なし | なし | なし | 静的リストは [!DNL Salesforce] から同期されないので、セカンダリ ID がありません。 |
| B2B マーケティングリストメンバー | [XDM ビジネスマーケティングリストメンバー ](../../../../xdm/classes/b2b/business-marketing-list-members.md) | なし | 有効 | 基本クラスの `marketingListMemberKey.sourceKey` | B2B マーケティングリストメンバー | なし | なし | **最初の関係**<ul><li>基本クラスの `PersonKey.sourceKey`</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B Person</li><li>名前空間：B2B Person</li><li>宛先のプロパティ：`b2b.personKey.sourceKey`</li><li>現在のスキーマからの関係名：人物</li><li>参照スキーマからの関係名：マーケティングリスト</li></ul>**2 番目の関係**<ul><li>基本クラスの `marketingListKey.sourceKey`</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B マーケティングリスト</li><li>名前空間：B2B マーケティングリスト</li><li>宛先のプロパティ：`marketingListKey.sourceKey`</li><li>現在のスキーマからの関係名：マーケティングリスト</li><li>参照スキーマからの関係名：人物</li></ul> | 静的リストメンバーは [!DNL Salesforce] から同期されないので、セカンダリ ID がありません。 |
| B2B アクティビティ | [XDM ExperienceEvent](../../../../xdm/classes/experienceevent.md) | <ul><li>Web ページを訪問</li><li>新しいリード</li><li>リードを変換</li><li>リストに追加</li><li>リストから削除</li><li>オポチュニティに追加</li><li>オポチュニティから削除</li><li>フォームに入力済み</li><li>リンククリック数</li><li>電子メール配信済み</li><li>電子メールの開封</li><li>電子メールのクリック</li><li>電子メールバウンス</li><li>電子メールバウンス (ソフト)</li><li>電子メール登録解除済み</li><li>変更されたスコア</li><li>更新されたオポチュニティ</li><li>キャンペーン進行のステータスが変更されました</li><li>人物識別子</li><li>Marketo Web URL</li><li>興味深い瞬間</li><li>Webhook を呼び出す</li><li>キャンペーン頻度を変更</li><li>変更された収益ステージ</li><li>リードを結合</li><li>送信済み電子メール</li><li>キャンペーンストリームを変更</li><li>キャンペーンに追加</li></ul> | 有効 | 人物識別子フィールドグループの `personKey.sourceKey` | B2B ユーザー | なし | なし | **最初の関係**<ul><li>`listOperations.listKey.sourceKey` フィールド</li><li>タイプ : 1 対 1</li><li>参照スキーマ：B2B マーケティングリスト</li><li>名前空間：B2B マーケティングリスト</li></ul>**2 番目の関係**<ul><li>`opportunityEvent.opportunityKey.sourceKey` フィールド</li><li>タイプ : 1 対 1</li><li>参照スキーマ：B2B オポチュニティ</li><li>名前空間：B2B オポチュニティ</li></ul>**第三の関係**<ul><li>`leadOperation.campaignProgression.campaignKey.sourceKey` フィールド</li><li>タイプ : 1 対 1</li><li>参照スキーマ：B2B キャンペーン</li><li>名前空間：B2B キャンペーン</li></ul> | `ExperienceEvent` はエンティティとは異なります。 エクスペリエンスイベントの ID は、アクティビティを行ったユーザーです。 |
| B2B アカウント人物関係 | [XDM ビジネスアカウント人物関係 ](../../../../xdm/classes/b2b/business-account-person-relation.md) | ID マップ | 有効 | 基本クラスの `accountPersonKey.sourceKey` | B2B アカウント人物関係 | なし | なし | **最初の関係**<ul><li>基本クラスの `personKey.sourceKey`</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B Person</li><li>名前空間：B2B Person</li><li>宛先のプロパティ：`b2b.personKey.SourceKey`</li><li>現在のスキーマからの関係名：人物</li><li>参照スキーマからの関係名：アカウント</li></ul>**2 番目の関係**<ul><li>基本クラスの `accountKey.sourceKey`</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B アカウント</li><li>名前空間：B2B アカウント</li><li>宛先のプロパティ：`accountKey.sourceKey`</li><li>現在のスキーマからの関係名：アカウント</li><li>参照スキーマからの関係名：人物</li></ul> |

{style="table-layout:auto"}

## 次の手順

[!DNL Marketo] データを Platform に接続する方法については、[UI でのMarketo ソースコネクタの作成 ](../../../tutorials/ui/create/adobe-applications/marketo.md) に関するチュートリアルを参照してください。
