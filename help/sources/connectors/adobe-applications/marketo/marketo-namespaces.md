---
keywords: Experience Platform；ホーム；人気のあるトピック；Marketoソースコネクタ；名前空間；スキーマ；b2b;B2B
solution: Experience Platform
title: B2B 名前空間とスキーマ
topic-legacy: overview
description: このドキュメントでは、B2B ソースコネクタの作成時に必要なカスタム名前空間の概要を説明します。
exl-id: f1592be5-987e-41b8-9844-9dea5bd452b9
source-git-commit: 15fd870565d50bd4e320a1acf61413f45c1f537c
workflow-type: tm+mt
source-wordcount: '1679'
ht-degree: 10%

---

# （ベータ版）B2B 名前空間とスキーマ

>[!IMPORTANT]
>
>この機能は現在ベータ版です。 ドキュメントと機能は変更される場合があります。

このドキュメントでは、B2B ソースで使用する名前空間とスキーマの基礎となる設定に関する情報を提供します。 このドキュメントでは、B2B 名前空間とスキーマの生成に必要な Postman オートメーションユーティリティの設定に関する詳細も説明します。

## B2B 名前空間とスキーマ自動生成ユーティリティの設定

B2B 名前空間とスキーマ自動生成ユーティリティを使用する最初の手順は、Platform デベロッパーコンソールと [!DNL Postman] 環境を設定することです。

- この [GitHub リポジトリ ](https://github.com/adobe/experience-platform-postman-samples/tree/master/Postman%20Collections/CDP%20Namespaces%20and%20Schemas%20Utility) から、名前空間とスキーマの自動生成ユーティリティコレクションおよび環境をダウンロードできます。
- 必要なヘッダーの値の収集方法の詳細やサンプル API 呼び出しを読む方法など、Platform API の使用に関する詳細については、[Platform API の使用の手引き ](../../../../landing/api-guide.md) を参照してください。
- Platform API の資格情報を生成する方法について詳しくは、[Experience PlatformAPI の認証とアクセス ](../../../../landing/api-authentication.md) に関するチュートリアルを参照してください。
- Platform API 用に [!DNL Postman] を設定する方法について詳しくは、[ 開発者コンソールの設定および  [!DNL Postman]](../../../../landing/postman.md) の設定に関するチュートリアルを参照してください。

Platform 開発者コンソールと [!DNL Postman] の設定が完了したら、適切な環境値の [!DNL Postman] 環境への適用を開始できます。

次の表に、値の例と、[!DNL Postman] 環境の設定に関する追加情報を示します。

| 変数 | 説明 | 例 |
| --- | --- | --- |
| `CLIENT_SECRET` | `{ACCESS_TOKEN}` の生成に使用される一意の識別子。 `{CLIENT_SECRET}` を取得する方法について詳しくは、[Experience PlatformAPI の認証とアクセス ](../../../../landing/api-authentication.md) に関するチュートリアルを参照してください。 | `{CLIENT_SECRET}` |
| `JWT_TOKEN` | JSON Web トークン (JWT) は、{ACCESS_TOKEN} の生成に使用される認証資格情報です。 `{JWT_TOKEN}` の生成方法については、[Experience PlatformAPI の認証とアクセス ](../../../../landing/api-authentication.md) に関するチュートリアルを参照してください。 | `{JWT_TOKEN}` |
| `API_KEY` | Experience PlatformAPI の呼び出しを認証するために使用される一意の識別子。 `{API_KEY}` を取得する方法について詳しくは、[Experience PlatformAPI の認証とアクセス ](../../../../landing/api-authentication.md) に関するチュートリアルを参照してください。 | `c8d9a2f5c1e03789bd22e8efdd1bdc1b` |
| `ACCESS_TOKEN` | Experience PlatformAPI の呼び出しを完了するために必要な認証トークン。 `{ACCESS_TOKEN}` を取得する方法について詳しくは、[Experience PlatformAPI の認証とアクセス ](../../../../landing/api-authentication.md) に関するチュートリアルを参照してください。 | `Bearer {ACCESS_TOKEN}` |
| `META_SCOPE` | [!DNL Marketo] に関しては、この値は固定値で、常に次の値に設定されます。`ent_dataservices_sdk`. | `ent_dataservices_sdk` |
| `CONTAINER_ID` | `global` コンテナには、標準のAdobeとExperience Platformパートナーが提供するクラス、スキーマフィールドグループ、データ型、スキーマがすべて格納されます。 [!DNL Marketo] に関しては、この値は固定値で、常に `global` に設定されます。 | `global` |
| `PRIVATE_KEY` | [!DNL Postman] インスタンスをExperience PlatformAPI に対して認証するために使用する資格情報。 {PRIVATE_KEY} の取得方法については、開発者コンソールの設定と [ 開発者コンソールの設定および  [!DNL Postman]](../../../../landing/postman.md) の設定に関するチュートリアルを参照してください。 | `{PRIVATE_KEY}` |
| `TECHNICAL_ACCOUNT_ID` | Adobe I/Oへの統合に使用する秘密鍵証明書。 | `D42AEVJZTTJC6LZADUBVPA15@techacct.adobe.com` |
| `IMS` | Identity Managementシステム (IMS) は、Adobe サービスに対する認証のフレームワークを提供します。 [!DNL Marketo] に関しては、この値は固定値で、常に次の値に設定されます。`ims-na1.adobelogin.com`. | `ims-na1.adobelogin.com` |
| `IMS_ORG` | 製品やサービスを所有またはライセンスし、そのメンバーへのアクセスを許可できる企業エンティティ。 `{IMS_ORG}` 情報を取得する方法については、[ 開発者コンソールの設定と  [!DNL Postman]](../../../../landing/postman.md) に関するチュートリアルを参照してください。 | `ABCEH0D9KX6A7WA7ATQE0TE@adobeOrg` |
| `SANDBOX_NAME` | 使用する仮想サンドボックスパーティションの名前。 | `prod` |
| `TENANT_ID` | 作成するリソースの名前空間が適切に設定され、IMS 組織内に含まれるようにするために使用される ID。 | `b2bcdpproductiontest` |
| `PLATFORM_URL` | API 呼び出しをおこなう URL エンドポイント。 この値は固定値で、常に次の値に設定されます。`http://platform.adobe.io/`. | `http://platform.adobe.io/` |

{style=&quot;table-layout:auto&quot;}

### スクリプトの実行

[!DNL Postman] コレクションと環境の設定で、[!DNL Postman] インターフェイスを介してスクリプトを実行できるようになりました。

[!DNL Postman] インターフェイスで、自動生成ユーティリティのルートフォルダを選択し、上部のヘッダから **[!DNL Run]** を選択します。

![root-folder](../images/marketo/root-folder.png)

[!DNL Runner] インターフェイスが表示されます。 ここから、すべてのチェックボックスが選択されていることを確認し、**[!DNL Run Namespaces and Schemas Autogeneration Utility]** を選択します。

![ランジェネレータ](../images/marketo/run-generator.png)

リクエストが成功すると、B2B に必要な名前空間とスキーマが作成されます。

## B2B 名前空間

ID 名前空間は、ID のコンテキストやタイプを区別するための [[!DNL Identity Service]](../../../../identity-service/home.md) のコンポーネントです。 完全修飾 ID には、ID 値と名前空間が含まれます。詳しくは、[ 名前空間の概要 ](../../../../identity-service/namespaces.md) を参照してください。

B2B 名前空間は、エンティティのプライマリ ID で使用されます。

次の表に、B2B 名前空間の基になる設定に関する情報を示します。

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 表示名 | ID シンボル | ID タイプ |
| --- | --- | --- |
| B2B 人 | `b2b_person` | `CROSS_DEVICE` |
| B2B アカウント | `b2b_account` | `B2B_ACCOUNT` |
| B2B オポチュニティ | `b2b_opportunity` | `B2B_OPPORTUNITY` |
| B2B 商談担当者関係 | `b2b_opportunity_person_relation` | `B2B_OPPORTUNITY_PERSON` |
| B2B キャンペーン | `b2b_campaign` | `B2B_CAMPAIGN` |
| B2B キャンペーンメンバー | `b2b_campaign_member` | `B2B_CAMPAIGN_MEMBER` |
| B2B マーケティングリスト | `b2b_marketing_list` | `B2B_MARKETING_LIST` |
| B2B マーケティングリストメンバー | `b2b_marketing_list_member` | `B2B_MARKETING_LIST_MEMBER` |
| B2B 口座個人関連 | `b2b_account_person_relation` | `B2B_ACCOUNT_PERSON` |

{style=&quot;table-layout:auto&quot;}

## B2B スキーマ

Experience Platform では、スキーマを使用して、一貫性のある再利用可能な方法でデータの構造を記述します。システムをまたいで一貫したデータを定義することで、意味を保有しやすくなり、データから価値を得ることができます。

データを Platform に取り込む前に、スキーマを構成して、データの構造を記述し、各フィールドに含めることができるデータの種類を制限する必要があります。スキーマは、基本クラスと 0 個以上のスキーマフィールドグループで構成されます。

デザインの原則やベストプラクティスなど、スキーマ構成モデルについて詳しくは、[スキーマ構成の基本](../../../../xdm/schema/composition.md)を参照してください。

次の表に、B2B スキーマの基盤となる設定に関する情報を示します。

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| スキーマ名 | 基本クラス | フィールドグループ | [!DNL Profile] スキーマ内 | プライマリID | プライマリID 名前空間 | セカンダリ | セカンダリID 名前空間 | 関係 | 備考 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| B2B アカウント | XDM ビジネスアカウント | XDM ビジネスアカウントの詳細 | 有効 | `accountKey.sourceKey` 基底クラスで | B2B アカウント | `extSourceSystemAudit.externalKey.sourceKey` 基底クラスで | B2B アカウント | <ul><li>`accountParentKey.sourceKey` 「 XDM Business Account Details 」フィールドグループ</li><li>宛先プロパティ：`/accountKey/sourceKey`</li><li>型：1 対 1 の</li><li>参照スキーマ：B2B アカウント</li><li>名前空間：B2B アカウント</li></ul> |
| B2B 人 | XDM 個人プロファイル | <ul><li>XDM ビジネス担当者の詳細</li><li>XDM Business Person コンポーネント</li><li>IdentityMap</li><li>同意と環境設定の詳細</li></ul> | 有効 | `b2b.personKey.sourceKey` （XDM Business Person Details Field Group の） | B2B 人 | <ol><li>`extSourceSystemAudit.externalKey.sourceKey` （XDM ビジネス担当者詳細フィールドグループ）</li><li>`workEmail.address` （XDM ビジネス担当者詳細フィールドグループ）</ol></li> | <ol><li>B2B 人</li><li>メール</li></ol> | <ul><li>`personComponents.sourceAccountKey.sourceKey` （XDM Business Person Components フィールドグループ）</li><li>型：多対 1</li><li>参照スキーマ：B2B アカウント</li><li>名前空間：B2B アカウント</li><li>宛先プロパティ：accountKey.sourceKey</li><li>現在のスキーマの関係名：アカウント</li><li>参照スキーマの関係名：People</li></ul> |
| B2B オポチュニティ | XDM ビジネス機会 | XDM ビジネス機会の詳細 | 有効 | `opportunityKey.sourceKey` 基底クラスで | B2B オポチュニティ | `extSourceSystemAudit.externalKey.sourceKey` 基底クラスで | B2B オポチュニティ | <ul><li>`accountKey.sourceKey` 基底クラスで</li><li>型：多対 1</li><li>参照スキーマ：B2B アカウント</li><li>名前空間：B2B アカウント</li><li>宛先プロパティ：`accountKey.sourceKey`</li><li>現在のスキーマの関係名：アカウント</li><li>参照スキーマの関係名：機会</li></ul> |
| B2B 商談担当者関係 | XDM Business Opportunity Person 関係 | なし | 有効 | `opportunityPersonKey.sourceKey` 基底クラスで | B2B 商談担当者関係 | `extSourceSystemAudit.externalKey.sourceKey` 基底クラスで | B2B 商談担当者関係 | **最初の関係**<ul><li>`personKey.sourceKey` 基底クラスで</li><li>型：多対 1</li><li>参照スキーマ：B2B 人</li><li>名前空間：B2B 人</li><li>宛先プロパティ：b2b.personKey.sourceKey</li><li>現在のスキーマの関係名：ユーザー</li><li>参照スキーマの関係名：機会</li></ul>**第 2 の関係**<ul><li>`opportunityKey.sourceKey` 基底クラスで</li><li>型：多対 1</li><li>参照スキーマ：B2B オポチュニティ </li><li>名前空間：B2B オポチュニティ </li><li>宛先プロパティ：`opportunityKey.sourceKey`</li><li>現在のスキーマの関係名：商談</li><li>参照スキーマの関係名：People</li></ul> |
| B2B キャンペーン | XDM Business Campaign | XDM ビジネスキャンペーンの詳細 | 有効 | `campaignKey.sourceKey` 基底クラスで | B2B キャンペーン | `extSourceSystemAudit.externalKey.sourceKey` 基底クラスで | B2B キャンペーン |
| B2B キャンペーンメンバー | XDM Business Campaign メンバー | XDM Business Campaign メンバーの詳細 | 有効 | `ccampaignMemberKey.sourceKey` 基底クラスで | B2B キャンペーンメンバー | `extSourceSystemAudit.externalKey.sourceKey` 基底クラスで | B2B キャンペーンメンバー | **最初の関係**<ul><li>`personKey.sourceKey` 基底クラスで</li><li>型：多対 1</li><li>参照スキーマ：B2B 人</li><li>名前空間：B2B 人</li><li>宛先プロパティ：`b2b.personKey.sourceKey`</li><li>現在のスキーマの関係名：ユーザー</li><li>参照スキーマの関係名：Campaigns</li></ul>**第 2 の関係**<ul><li>`campaignKey.sourceKey` 基底クラスで</li><li>型：多対 1</li><li>参照スキーマ：B2B キャンペーン</li><li>名前空間：B2B キャンペーン</li><li>宛先プロパティ：`campaignKey.sourceKey`</li><li>現在のスキーマの関係名：Campaign</li><li>参照スキーマの関係名：People</li></ul> |
| B2B マーケティングリスト | XDM ビジネスマーケティングリスト | なし | 有効 | `marketingListKey.sourceKey` 基底クラスで | B2B マーケティングリスト | なし | なし | なし | 静的リストは [!DNL Salesforce] から同期されないので、セカンダリ ID を持ちません。 |
| B2B マーケティングリストメンバー | XDM ビジネスマーケティングリストメンバー | なし | 有効 | `marketingListMemberKey.sourceKey` 基底クラスで | B2B マーケティングリストメンバー | なし | なし | **最初の関係**<ul><li>`PersonKey.sourceKey` 基底クラスで</li><li>型：多対 1</li><li>参照スキーマ：B2B 人</li><li>名前空間：B2B 人</li><li>宛先プロパティ：`b2b.personKey.sourceKey`</li><li>現在のスキーマの関係名：ユーザー</li><li>参照スキーマの関係名：マーケティングリスト</li></ul>**第 2 の関係**<ul><li>`marketingListKey.sourceKey` 基底クラスで</li><li>型：多対 1</li><li>参照スキーマ：B2B マーケティングリスト</li><li>名前空間：B2B マーケティングリスト</li><li>宛先プロパティ：`marketingListKey.sourceKey`</li><li>現在のスキーマの関係名：マーケティングリスト</li><li>参照スキーマの関係名：People</li></ul> | 静的リストメンバーは [!DNL Salesforce] から同期されないので、セカンダリ ID を持ちません。 |
| B2B アクティビティ | XDM ExperienceEvent | <ul><li>Web ページにアクセス</li><li>新しいリード</li><li>リードの変換</li><li>リストに追加</li><li>リストから削除</li><li>商談に追加</li><li>商談から削除</li><li>入力済みフォーム</li><li>リンククリック数</li><li>電子メール配信</li><li>開封済みの電子メール</li><li>電子メールのクリック</li><li>電子メールのバウンス</li><li>電子メールバウンスソフト</li><li>E メールの購読解除</li><li>変更されたスコア</li><li>更新されたオポチュニティ</li><li>変更されたキャンペーン進行のステータス</li><li>ユーザー ID</li><li>Marketo Web URL</li><li>面白い瞬間</li></ul> | 有効 | `personKey.sourceKey` 個人 ID フィールドグループの | B2B 人 | なし | なし | **最初の関係**<ul><li>`listOperations.listKey.sourceKey` field</li><li>型：1 対 1 の</li><li>参照スキーマ：B2B マーケティングリスト</li><li>名前空間：B2B マーケティングリスト</li></ul>**第 2 の関係**<ul><li>`opportunityEvent.opportunityKey.sourceKey` field</li><li>型：1 対 1 の</li><li>参照スキーマ：B2B オポチュニティ</li><li>名前空間：B2B オポチュニティ</li></ul>**3 番目の関係**<ul><li>`leadOperation.campaignProgression.campaignKey.sourceKey` field</li><li>型：1 対 1 の</li><li>参照スキーマ：B2B キャンペーン</li><li>名前空間：B2B キャンペーン</li></ul> | `ExperienceEvent` エンティティとは異なります。エクスペリエンスイベントの ID は、アクティビティを実行した人です。 |
| B2B 口座個人関連 | XDM ビジネスアカウント担当者の関係 | ID マップ | 有効 | `accountPersonKey.sourceKey` 基底クラスで | B2B 口座個人関連 | なし | なし | **最初の関係**<ul><li>`personKey.sourceKey` 基底クラスで</li><li>型：多対 1</li><li>参照スキーマ：B2B 人</li><li>名前空間：B2B 人</li><li>宛先プロパティ：`b2b.personKey.SourceKey`</li><li>現在のスキーマの関係名：People</li><li>参照スキーマの関係名：アカウント</li></ul>**第 2 の関係**<ul><li>`accountKey.sourceKey` 基底クラスで</li><li>型：多対 1</li><li>参照スキーマ：B2B アカウント</li><li>名前空間：B2B アカウント</li><li>宛先プロパティ：`accountKey.sourceKey`</li><li>現在のスキーマの関係名：アカウント</li><li>参照スキーマの関係名：People</li></ul> |

{style=&quot;table-layout:auto&quot;}

## 次の手順

[!DNL Marketo] データを Platform に接続する方法については、[UI でのMarketoソースコネクタの作成 ](../../../tutorials/ui/create/adobe-applications/marketo.md) に関するチュートリアルを参照してください。
