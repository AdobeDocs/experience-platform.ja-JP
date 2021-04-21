---
keywords: Experience Platform；ホーム；人気のあるトピック；Marketoソースコネクタ；名前空間;スキーマ
solution: Experience Platform
title: 'Marketo名前空間 '
topic-legacy: overview
description: このドキュメントでは、Marketo Engageソースコネクタを作成する際に必要なカスタム名前空間の概要を説明します。
translation-type: tm+mt
source-git-commit: bea6b35627b0e913c894c38ba9553085ba0aa26f
workflow-type: tm+mt
source-wordcount: '1215'
ht-degree: 16%

---


# （ベータ版） [!DNL Marketo Engage]名前空間とスキーマ

>[!IMPORTANT]
>
>[!DNL Marketo Engage]ソースは現在ベータ版です。 この機能とドキュメントは、変更される場合があります。

このドキュメントは、[!DNL Marketo Engage]で使用されるB2B名前空間とスキーマ（以下「[!DNL Marketo]」と呼ばれる）の基になる設定に関する情報を提供します。 このドキュメントでは、[!DNL Marketo] B2B名前空間とスキーマの生成に必要なPostmanオートメーションユーティリティの設定に関する詳細も説明します。

## 前提条件

B2B名前空間とスキーマを生成する前に、まずプラットフォーム開発者コンソールと[!DNL Postman]環境を設定する必要があります。 詳しくは、[デベロッパーコンソールの設定と [!DNL Postman]](../../../../landing/postman.md)のチュートリアルを参照してください。

プラットフォーム開発者コンソールと[!DNL Postman]を設定したら、[!DNL Marketo]環境に次の変数を適用します。

| 環境変数 | 値の例 | 備考 |
| --- | --- | --- |
| `PRIVATE_KEY` | `{PRIVATE_KEY}` |
| `SANDBOX_NAME` | `prod` |
| `TENANT_ID` | `b2bcdpproductiontest` |
| `munchkinId` | `123-ABC-456 ` | 詳しくは、[ [!DNL Marketo] インスタンス](./marketo-auth.md)の認証のチュートリアルを参照してください。 |
| `sfdc_org_id` | `00D4W000000FgYJUA0` | 組織IDの取得について詳しくは、次の[[!DNL Salesforce] ガイド](https://help.salesforce.com/articleView?id=000325251&amp;type=1&amp;mode=1)を参照してください。 |
| `msd_org_id` | `f6438fab-67e8-4814-a6b5-8c8dcdf7a98f` | 組織IDの取得について詳しくは、次の[[!DNL Microsoft Dynamics] ガイド](https://docs.microsoft.com/en-us/power-platform/admin/determine-org-id-name)を参照してください。 |
| `has_abm` | `false` | アカウントベースのマーケティングを登録している場合、この値は`true`に設定されます。 |
| `has_msi` | `false` | [!DNL Marketo Sales Insight]をサブスクライブしている場合、この値は`true`に設定されます。 |

{style=&quot;table-layout:auto&quot;}

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
| `salesforce_person_{SALESFORCE_ORGANIZATION_ID}` | 自動生成された | `CROSS_DEVICE` | [!DNL Salesforce] | `person` | `00DA0000000Hz79` |
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

| 表示名 | 識別記号 | ID タイプ | 発行者のタイプ | 発行者のエンティティタイプ | [!DNL Salesforce] 購読組織IDの例 |
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

データを Platform に取り込む前に、スキーマを構成して、データの構造を記述し、各フィールドに含めることができるデータの種類を制限する必要があります。スキーマは、基本クラスと 0 個以上の Mixin で構成されます。

デザインの原則やベストプラクティスなど、スキーマ構成モデルについて詳しくは、[スキーマ構成の基本](../../../../xdm/schema/composition.md)を参照してください。

次の表に、[!DNL Marketo]スキーマの基本的な設定に関する情報を示します。

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| スキーマ名 | 基本クラス | Mixin | プライマリ同一性 | プライマリ同一性名前空間 | セカンダリ同一性 | セカンダリ同一性名前空間 | Relationship | 備考 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [!DNL Marketo] 会社{MUNCHKIN_ID} | XDMビジネスアカウント | XDMビジネスアカウントの詳細 | `accountID` 基本クラスで | `marketo_company_{MUNCHKIN_ID}` | `extSourceSystemAudit.externalID` 基本クラスで | `salesforce_account_{SALESFORCE_ORGANIZATION_ID}` | <ul><li>`accountParentID` (XDM Business Account Details Mixin)</li><li>タイプ：1対1</li><li>参照スキーマ:Marketo会社{MUNCHKIN_ID}</li><li>名前空間: `marketo_company_{MUNCHKIN_ID}`</li></ul> |
| [!DNL Marketo] ユーザー{MUNCHKIN_ID} | XDM 個人プロファイル | <ul><li>XDMビジネス・パーソンの詳細</li><li>XDM Business Personコンポーネント</li></ul> | `personID` 基本クラスで | `marketo_person_{MUNCHKIN_ID}` | <ol><li>`extSourceSystemAudit.externalID` (XDM Business Person Details Mixin)</li><li>`workEmail.address` (XDM Business Person Details Mixin)</li><li>`identityMap` IDマップミックスインの</ol></li> | <ol><li>`salesforce_person_{SALESFORCE_ORGANIZATION_ID}`</li><li>Email</li><li>ECID</li></ol> | <ul><li>`personComponents.sourceAccountID` （XDM Business PersonコンポーネントMixin）</li><li>タイプ：多対1</li><li>参照スキーマ:Marketo会社{MUNCHKIN_ID}</li><li>名前空間: `marketo_company_{MUNCHKIN_ID}`</li><li>Destinationプロパティ：`accountID`</li><li>現在のスキーマからの関係名：アカウント</li><li>参照スキーマからの関係名：ユーザー</li></ul> |
| [!DNL Marketo] オポチュニティ{MUNCHKIN_ID} | XDMビジネス・オポチュニティ | XDMビジネス・オポチュニティの詳細 | `opportunityID` 基本クラスで | `marketo_opportunity_{MUNCHKIN_ID}` | `extSourceSystemAudit.externalID` 基本クラスで | `salesforce_opportunity_{SALESFORCE_ORGANIZATION_ID}` | <ul><li>`accountID` 基本クラスで</li><li>タイプ：多対1</li><li>参照スキーマ:Marketo会社{MUNCHKIN_ID}</li><li>名前空間: `marketo_company_{MUNCHKIN_ID}`</li><li>Destinationプロパティ：`accountID`</li><li>現在のスキーマからの関係名：アカウント</li><li>参照スキーマからの関係名：オポチュニティ</li></ul> |
| [!DNL Marketo] オポチュニティ連絡先ロール{MUNCHKIN_ID} | XDM Business Opportunity Person関係 | なし | `opportunityPersonID` 基本クラスで | `marketo_opportunity_contact_role_{MUNCHKIN_ID}` | `extSourceSystemAudit.externalID` 基本クラスで | `salesforce_opportunity_contact_role_{SALESFORCE_ORGANIZATION_ID}` | 最初の関係<ul><li>`personID` 基本クラスで</li><li>タイプ：多対1</li><li>参照スキーマ:Marketoの人{MUNCHKIN_ID}</li><li>名前空間: `marketo_person_{MUNCHKIN_ID}`</li><li>Destinationプロパティ：`personID`</li><li>現在のスキーマからの関係名：人</li><li>参照スキーマからの関係名：オポチュニティ</li></ul>第2の関係<ul><li>`opportunityID` 基本クラスで</li><li>タイプ：多対1</li><li>参照スキーマ:Marketoオポチュニティ{MUNCHKIN_ID}</li><li>名前空間: `marketo_opportunity_{MUNCHKIN_ID}`</li><li>Destinationプロパティ：`opportunityID`</li><li>現在のスキーマからの関係名：オポチュニティ</li><li>参照スキーマからの関係名：ユーザー</li></ul> |
| [!DNL Marketo] プログラム{MUNCHKIN_ID} | XDMビジネスキャンペーン | XDMビジネスキャンペーンの詳細 | `campaignID` 基本クラスで | `marketo_program_{MUNCHKIN_ID}` | `extSourceSystemAudit.externalID` 基本クラスで | `salesforce_campaign_SALESFORCE_ORGANIZATION_ID}` |
| [!DNL Marketo] プログラムメンバ{MUNCHKIN_ID} | XDMビジネスキャンペーンメンバ | XDMビジネスキャンペーンメンバの詳細 | `campaignMemberID` 基本クラスで | `marketo_program_member_{MUNCHKIN_ID}` | なし | なし | 最初の関係<ul><li>`personID` 基本クラスで</li><li>タイプ：多対1</li><li>参照スキーマ:Marketoの人{MUNCHKIN_ID}</li><li>名前空間: `marketo_person_{MUNCHKIN_ID}`</li><li>Destinationプロパティ：`personID`</li><li>現在のスキーマからの関係名：人</li><li>参照スキーマからの関係名：プログラム</li></ul>第2の関係<ul><li>`campaignID` 基本クラスで</li><li>タイプ：多対1</li><li>参照スキーマ:Marketoプログラム{MUNCHKIN_ID}</li><li>名前空間: `marketo_program_{MUNCHKIN_ID}`</li><li>Destinationプロパティ：campaignID</li><li>現在のスキーマからの関係名：プログラム</li><li>参照スキーマからの関係名：ユーザー</li></ul> |
| [!DNL Marketo] 静的リスト{MUNCHKIN_ID} | XDMビジネスマーケティングリスト | なし | `marketingListID` 基本クラスで | `marketo_static_list_{MUNCHKIN_ID}` | なし | なし | なし | 静的なリストは[!DNL Salesforce]と同期されないため、セカンダリIDを持ちません |
| [!DNL Marketo] 静的リストメンバ{MUNCHKIN_ID} | XDMビジネスマーケティングリストメンバー | なし | `marketingListMemberID` 基本クラスで | `marketo_static_list_member_{MUNCHKIN_ID}` | なし | なし | 最初の関係<ul><li>`personID` 基本クラスで</li><li>タイプ：多対1</li><li>参照スキーマ:Marketoの人{MUNCHKIN_ID}</li><li>名前空間: `marketo_person_{MUNCHKIN_ID}`</li><li>Destinationプロパティ：`personID`</li><li>現在のスキーマからの関係名：人</li><li>参照スキーマからの関係名：リスト</li></ul>第2の関係<ul><li>`marketingListID` 基本クラスで</li><li>タイプ：多対1</li><li>参照スキーマ:Marketo静的リスト{MUNCHKIN_ID}</li><li>名前空間: `marketo_static_list_{MUNCHKIN_ID}`</li><li>Destinationプロパティ：`marketingListID`</li><li>現在のスキーマからの関係名：リスト</li><li>参照スキーマからの関係名：ユーザー</li></ul> |
| [!DNL Marketo] 指定されたアカウント{MUNCHKIN_ID} | XDMビジネスアカウント | XDMビジネスアカウントの詳細 | `accountID` 基本クラスで | `marketo_named_account_{MUNCHKIN_ID}` | `extSourceSystemAudit.externalID` 基本クラスで | `salesforce_account_{SALESFORCE_ORGANIZATION_ID}` | <ul><li>`accountParentID` (XDM Business Account Details Mixin)</li><li>タイプ：1対1</li><li>参照スキーマ:Marketoがアカウント{MUNCHKIN_ID}に指定</li><li>名前空間: `marketo_named_account_{MUNCHKIN_ID}` |
| [!DNL Marketo] アクティビティ{MUNCHKIN ID} | XDMエクスペリエンスイベント | <ul><li>Webページにアクセス</li><li>新しいリード</li><li>リードの変換</li><li>リスト追加へ</li><li>リストから削除</li><li>To Opportunity</li><li>オポチュニティから削除</li><li>入力済みのフォーム</li><li>リンククリック数</li><li>電子メール配信</li><li>開封済み電子メール</li><li>電子メールがクリックされました</li><li>電子メールのバウンス</li><li>電子メールのバウンス（ソフト）</li><li>Email Unsubscribed</li><li>スコアの変更</li><li>オポチュニティの更新</li><li>キャンペーンの進行状況の変更</li><li>個人ID</li><li>MarketoウェブURL | `personID` （個人識別子Mixin） | marketo_person_{MUNCHKIN_ID} | なし | なし | 最初の関係<ul><li>`listOperations.listID` field</li><li>タイプ：1対1</li><li>参照スキーマ:Marketo静的リスト{MUNCHKIN_ID}</li><li>名前空間: `marketo_static_list_{MUNCHKIN_ID}`</li></ul>第2の関係<ul><li>`opportunityEvent.opportunityID` field</li><li>タイプ：1対1</li><li>参照スキーマ:Marketoオポチュニティ{MUNCHKIN_ID}</li><li>名前空間: `marketo_opportunity_{MUNCHKIN_ID}`</li></ul>3番目の関係<ul><li>`leadOperation.campaignProgression.campaignID` field</li><li>タイプ：1対1</li><li>参照スキーマ:Marketoプログラム{MUNCHKIN_ID}</li><li>名前空間: `marketo_program_{MUNCHKIN_ID}`</li></ul> |

{style=&quot;table-layout:auto&quot;}

>[!NOTE]
>
>[!DNL Real-time Customer Profile]のすべてのスキーマを有効にします

## 次の手順

[!DNL Marketo]データをプラットフォームに接続する方法については、[UI](../../../tutorials/ui/create/adobe-applications/marketo.md)でのMarketoソースコネクタの作成に関するチュートリアルを参照してください。