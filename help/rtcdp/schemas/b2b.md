---
title: Real-time Customer Data Platform B2B Edition のスキーマ
description: Adobe Real-Time Customer Data Platform B2B editionにおけるエクスペリエンスデータモデル（XDM）スキーマの役割を概説します。
feature: Get Started, Data Management, Schemas
badgeB2B: label="B2B edition" type="Informative" url="https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html newtab=true"
exl-id: 3b18d377-108f-443f-86ae-dc7537cf9013
source-git-commit: 09f671af0d04251ab7b0a71528cb4b9745594b1c
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 47%

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
