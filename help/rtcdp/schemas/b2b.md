---
title: Real-time Customer Data Platform B2B Edition のスキーマ
description: Real-time Customer Data Platform B2B Edition におけるエクスペリエンスデータモデル（XDM）スキーマの役割の概要です。
exl-id: 3b18d377-108f-443f-86ae-dc7537cf9013
source-git-commit: 8718512a9768158183b9fb6b9e336081e47cd889
workflow-type: ht
source-wordcount: '220'
ht-degree: 100%

---

# Real-time Customer Data Platform B2B Edition のスキーマ

Real-time Customer Data Platform B2B Edition には、アカウント、機会、キャンペーンなどといった基本的な B2B データエンティティに関する詳細をキャプチャする[エクスペリエンスデータモデル (XDM) クラス](../../xdm/schema/composition.md#class)が標準で複数用意されています。さらに、Real-time CDP B2B Edition を使用すると、これらのスキーマ間で多対 1 の関係を定義できるので、高度なセグメント化のユースケースに加えることができます。

>[!IMPORTANT]
>
>B2B スキーマを[リアルタイム顧客プロファイル](../../profile/home.md)に加えるには、Real-time CDP B2B Edition へのアクセス権が必要です。

Real-time CDP B2B Edition には、次の標準クラスが用意されています。

* [XDM Business Account](../../xdm/classes/b2b/business-account.md)
* [XDM Business Account Person Relation](../../xdm/classes/b2b/business-account-person-relation.md)
* [XDM Business Campaign](../../xdm/classes/b2b/business-campaign.md)
* [XDM Business Campaign Members](../../xdm/classes/b2b/business-campaign-members.md)
* [XDM Business Opportunity](../../xdm/classes/b2b/business-opportunity.md)
* [XDM Business Opportunity Person Relation](../../xdm/classes/b2b/business-opportunity-person-relation.md)
* [XDM Business Marketing List](../../xdm/classes/b2b/business-marketing-list.md)
* [XDM Business Marketing List Members](../../xdm/classes/b2b/business-marketing-list-members.md)

2 つのスキーマ間に多対 1 の関係を作成する手順については、[B2B スキーマの関係の定義](../../xdm/tutorials/relationship-b2b.md)に関するチュートリアルを参照してください。

B2B ソース接続を使用している場合は、ツールを使用して、必要なスキーマとそれらの間の関係を自動的に生成できます。 詳しくは、ソースに関するドキュメントにある [B2B 名前空間](../../sources/connectors/adobe-applications/marketo/marketo-namespaces.md)のガイドを参照してください。
