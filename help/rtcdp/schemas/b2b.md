---
title: Real-time Customer Data Platform B2B Edition のスキーマ
description: Adobe Real-time Customer Data Platform B2B Edition の Experience Data Model(XDM) スキーマの役割の概要です。
exl-id: 3b18d377-108f-443f-86ae-dc7537cf9013
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 45%

---

# Real-time Customer Data Platform B2B Edition のスキーマ

Adobe Real-time Customer Data Platform B2B Edition は、いくつかの標準を提供します [エクスペリエンスデータモデル (XDM) クラス](../../xdm/schema/composition.md#class) は、アカウント、商談、キャンペーンなど、重要な B2B データエンティティに関する詳細をキャプチャするためのものです。 また、Real-Time CDP B2B Edition では、これらのスキーマ間に多対 1 の関係を定義して、高度なセグメント化の使用例に参加できます。

>[!IMPORTANT]
>
>B2B スキーマがに参加するには、Real-Time CDP B2B Edition へのアクセス権が必要です [リアルタイム顧客プロファイル](../../profile/home.md).

Real-Time CDP B2B Edition では、次の標準クラスが提供されます。

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
