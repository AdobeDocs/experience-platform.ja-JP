---
title: Real-time Customer Data Platform B2B Edition のスキーマ
description: Real-time Customer Data Platform B2B Edition の Experience Data Model(XDM) スキーマの役割の概要です。
exl-id: 3b18d377-108f-443f-86ae-dc7537cf9013
source-git-commit: 8718512a9768158183b9fb6b9e336081e47cd889
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 8%

---

# Real-time Customer Data Platform B2B Edition のスキーマ

Real-time Customer Data Platform B2B Edition は、いくつかの標準を提供します [エクスペリエンスデータモデル (XDM) クラス](../../xdm/schema/composition.md#class) は、アカウント、商談、キャンペーンなど、重要な B2B データエンティティに関する詳細をキャプチャするためのものです。 さらに、Real-time CDP B2B Edition を使用すると、これらのスキーマ間で多対 1 の関係を定義して、高度なセグメント化の使用例に参加できます。

>[!IMPORTANT]
>
>B2B スキーマがに参加するには、リアルタイム CDP B2B エディションへのアクセス権が必要です。 [リアルタイム顧客プロファイル](../../profile/home.md).

リアルタイム CDP B2B エディションでは、次の標準クラスが提供されます。

* [XDM ビジネスアカウント](../../xdm/classes/b2b/business-account.md)
* [XDM ビジネスアカウント人物関係](../../xdm/classes/b2b/business-account-person-relation.md)
* [XDM ビジネスキャンペーン](../../xdm/classes/b2b/business-campaign.md)
* [XDM ビジネスキャンペーンメンバー](../../xdm/classes/b2b/business-campaign-members.md)
* [XDM ビジネスオポチュニティ](../../xdm/classes/b2b/business-opportunity.md)
* [XDM ビジネスオポチュニティ人物関係](../../xdm/classes/b2b/business-opportunity-person-relation.md)
* [XDM ビジネスマーケティングリスト](../../xdm/classes/b2b/business-marketing-list.md)
* [XDM ビジネスマーケティングリストメンバー](../../xdm/classes/b2b/business-marketing-list-members.md)

2 つのスキーマ間に多対 1 の関係を作成する手順については、 [B2B スキーマの関係の定義](../../xdm/tutorials/relationship-b2b.md).

B2B ソース接続を使用している場合は、ツールを使用して、必要なスキーマとそれらの間の関係を自動的に生成できます。 詳しくは、 [B2B 名前空間](../../sources/connectors/adobe-applications/marketo/marketo-namespaces.md) 詳しくは、ソースのドキュメントを参照してください。
