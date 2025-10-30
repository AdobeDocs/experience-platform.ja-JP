---
title: Real-time Customer Data Platform B2B Edition のスキーマ
description: Adobe Real-Time Customer Data Platform B2B editionにおけるエクスペリエンスデータモデル（XDM）スキーマの役割を概説します。
feature: Get Started, Data Management, Schemas
badgeB2B: label="B2B edition" type="Informative" url="https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html newtab=true"
exl-id: 3b18d377-108f-443f-86ae-dc7537cf9013
source-git-commit: be2ad7a02d4bdf5a26a0847c8ee7a9a93746c2ad
workflow-type: tm+mt
source-wordcount: '844'
ht-degree: 24%

---

# Real-time Customer Data Platform B2B Edition のスキーマ

Adobe Real-Time Customer Data Platform B2B editionには、アカウント、オポチュニティ、キャンペーンなどといった [ 基本的な B2B データエンティティに関する詳細をキャプチャする ](../../xdm/schema/composition.md#class) エクスペリエンスデータモデル （XDM） クラスが標準で複数用意されています。 さらに、Real-Time CDP B2B editionを使用すると、これらのスキーマ間で多対 1 の関係を定義できるので、高度なセグメント化のユースケースに加えることができます。

>[!IMPORTANT]
>
>B2B スキーマは、Experience Platform アプリケーション（[Customer Journey Analytics B2B edition](https://experienceleague.adobe.com/en/docs/analytics-platform/using/cja-overview/cja-b2b/cja-b2b-edition) など）で使用できます。 <br/> ただし、（内のプロファイル） B2B スキーマを（リアルタイム顧客プロファイル [ に加えるには、Real-Time CDP B2B editionへのアクセス権が必要 ](../../profile/home.md) す。

Real-Time CDP B2B editionには、次の標準クラスが用意されています。

* [XDM Business Account](../../xdm/classes/b2b/business-account.md)
* [XDM Business Account Person Relation](../../xdm/classes/b2b/business-account-person-relation.md)
* [XDM Business Campaign](../../xdm/classes/b2b/business-campaign.md)
* [XDM Business Campaign Members](../../xdm/classes/b2b/business-campaign-members.md)
* [XDM Business Opportunity](../../xdm/classes/b2b/business-opportunity.md)
* [XDM Business Opportunity Person Relation](../../xdm/classes/b2b/business-opportunity-person-relation.md)
* [XDM Business Marketing List](../../xdm/classes/b2b/business-marketing-list.md)
* [XDM Business Marketing List Members](../../xdm/classes/b2b/business-marketing-list-members.md)

スキーマがお客様の B2B ワークフローにどのように適合するかを理解するには、[エンドツーエンドのチュートリアル](../b2b-tutorial.md)を参照してください。

2 つのスキーマ間に多対 1 の関係を作成する手順については、[B2B スキーマの関係の定義](../../xdm/tutorials/relationship-b2b.md)に関するチュートリアルを参照してください。

B2B ソース接続を使用している場合は、ツールを使用して、必要なスキーマとそれらの間の関係を自動的に生成できます。 詳しくは、ソースに関するドキュメントにある [B2B 名前空間](../../sources/connectors/adobe-applications/marketo/marketo-namespaces.md)のガイドを参照してください。


B2B スキーマの基盤となる設定に関する情報を次の表に示します。

>[!NOTE]
>
>左右にスクロールして、テーブルの内容をすべて表示してください。

| スキーマ名 | 基本クラス | フィールドグループ | スキーマ内の [!DNL Profile] | プライマリ ID | プライマリ ID 名前空間 | セカンダリID | セカンダリid 名前空間 | 関係 | メモ |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| B2B アカウント | [XDM ビジネスアカウント ](../../xdm/classes/b2b/business-account.md) | XDM ビジネスアカウントの詳細 | 有効 | 基本クラスの `accountKey.sourceKey` | B2B アカウント | 基本クラスの `extSourceSystemAudit.externalKey.sourceKey` | B2B アカウント | <ul><li>XDM ビジネスアカウントの詳細フィールドグループの `accountParentKey.sourceKey`</li><li>宛先のプロパティ：`/accountKey/sourceKey`</li><li>タイプ : 1 対 1</li><li>参照スキーマ：B2B アカウント</li><li>名前空間：B2B アカウント</li></ul> |  |
| B2B ユーザー | [XDM 個人プロファイル](../../xdm/classes/individual-profile.md) | <ul><li>XDM ビジネスパーソンの詳細</li><li>XDM ビジネスパーソンのコンポーネント</li><li>identityMap</li><li>同意と環境設定の詳細</li></ul> | 有効 | XDM ビジネス人物の詳細フィールドグループの `b2b.personKey.sourceKey` | B2B ユーザー | <ol><li>XDM ビジネス人物の詳細フィールドグループの `extSourceSystemAudit.externalKey.sourceKey`</li><li>XDM ビジネス人物の詳細フィールドグループの `workEmail.address`</ol></li> | <ol><li>B2B ユーザー</li><li>メール</li></ol> | <ul><li>XDM ビジネスユーザーコンポーネントフィールドグループの `personComponents.sourceAccountKey.sourceKey`</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B アカウント</li><li>名前空間：B2B アカウント</li><li>宛先プロパティ：accountKey.sourceKey</li><li>現在のスキーマからの関係名：アカウント</li><li>参照スキーマからの関係名：人物</li></ul> |  |
| B2B オポチュニティ | [XDM ビジネスオポチュニティ ](../../xdm/classes/b2b/business-opportunity.md) | XDM ビジネスオポチュニティの詳細 | 有効 | 基本クラスの `opportunityKey.sourceKey` | B2B オポチュニティ | 基本クラスの `extSourceSystemAudit.externalKey.sourceKey` | B2B オポチュニティ | <ul><li>基本クラスの `accountKey.sourceKey`</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B アカウント</li><li>名前空間：B2B アカウント</li><li>宛先のプロパティ：`accountKey.sourceKey`</li><li>現在のスキーマからの関係名：アカウント</li><li>参照スキーマからの関係名：商談</li></ul> |  |
| B2B オポチュニティ人物関係 | [XDM ビジネスオポチュニティ人物関係 ](../../xdm/classes/b2b/business-opportunity-person-relation.md) | なし | 有効 | 基本クラスの `opportunityPersonKey.sourceKey` | B2B オポチュニティ人物関係 | | | **最初の関係**<ul><li>基本クラスの `personKey.sourceKey`</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B Person</li><li>名前空間：B2B Person</li><li>宛先プロパティ：b2b.personKey.sourceKey</li><li>現在のスキーマからの関係名：人物</li><li>参照スキーマからの関係名：商談</li></ul>**2 番目の関係**<ul><li>基本クラスの `opportunityKey.sourceKey`</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B オポチュニティ </li><li>名前空間：B2B オポチュニティ </li><li>宛先のプロパティ：`opportunityKey.sourceKey`</li><li>現在のスキーマからの関係名：商談</li><li>参照スキーマからの関係名：人物</li></ul> |  |
| B2B キャンペーン | [XDM ビジネスキャンペーン ](../../xdm/classes/b2b/business-campaign.md) | XDM ビジネスキャンペーンの詳細 | 有効 | 基本クラスの `campaignKey.sourceKey` | B2B キャンペーン | | | | |
| B2B キャンペーンメンバー | [XDM ビジネスキャンペーンメンバー ](../../xdm/classes/b2b/business-campaign-members.md) | XDM ビジネスキャンペーンメンバーの詳細 | 有効 | 基本クラスの `ccampaignMemberKey.sourceKey` | B2B キャンペーンメンバー | | | **最初の関係**<ul><li>基本クラスの `personKey.sourceKey`</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B Person</li><li>名前空間：B2B Person</li><li>宛先のプロパティ：`b2b.personKey.sourceKey`</li><li>現在のスキーマからの関係名：人物</li><li>参照スキーマからの関係名：キャンペーン</li></ul>**2 番目の関係**<ul><li>基本クラスの `campaignKey.sourceKey`</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B キャンペーン</li><li>名前空間：B2B キャンペーン</li><li>宛先のプロパティ：`campaignKey.sourceKey`</li><li>現在のスキーマからの関係名：Campaign</li><li>参照スキーマからの関係名：人物</li></ul> |  |
| B2B マーケティングリスト | [XDM ビジネスマーケティングリスト ](../../xdm/classes/b2b/business-marketing-list.md) | なし | 有効 | 基本クラスの `marketingListKey.sourceKey` | B2B マーケティングリスト | なし | なし | なし | 静的リストは [!DNL Salesforce] から同期されないので、セカンダリ ID がありません。 |
| B2B マーケティングリストメンバー | [XDM ビジネスマーケティングリストメンバー ](../../xdm/classes/b2b/business-marketing-list-members.md) | なし | 有効 | 基本クラスの `marketingListMemberKey.sourceKey` | B2B マーケティングリストメンバー | なし | なし | **最初の関係**<ul><li>基本クラスの `PersonKey.sourceKey`</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B Person</li><li>名前空間：B2B Person</li><li>宛先のプロパティ：`b2b.personKey.sourceKey`</li><li>現在のスキーマからの関係名：人物</li><li>参照スキーマからの関係名：マーケティングリスト</li></ul>**2 番目の関係**<ul><li>基本クラスの `marketingListKey.sourceKey`</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B マーケティングリスト</li><li>名前空間：B2B マーケティングリスト</li><li>宛先のプロパティ：`marketingListKey.sourceKey`</li><li>現在のスキーマからの関係名：マーケティングリスト</li><li>参照スキーマからの関係名：人物</li></ul> | 静的リストメンバーは [!DNL Salesforce] から同期されないので、セカンダリ ID がありません。 |
| B2B アカウント人物関係 | [XDM ビジネスアカウント人物関係 ](../../xdm/classes/b2b/business-account-person-relation.md) | ID マップ | 有効 | 基本クラスの `accountPersonKey.sourceKey` | B2B アカウント人物関係 | なし | なし | **最初の関係**<ul><li>基本クラスの `personKey.sourceKey`</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B Person</li><li>名前空間：B2B Person</li><li>宛先のプロパティ：`b2b.personKey.SourceKey`</li><li>現在のスキーマからの関係名：人物</li><li>参照スキーマからの関係名：アカウント</li></ul>**2 番目の関係**<ul><li>基本クラスの `accountKey.sourceKey`</li><li>タイプ：多対 1</li><li>参照スキーマ：B2B アカウント</li><li>名前空間：B2B アカウント</li><li>宛先のプロパティ：`accountKey.sourceKey`</li><li>現在のスキーマからの関係名：アカウント</li><li>参照スキーマからの関係名：人物</li></ul> |  |

{style="table-layout:auto"}

