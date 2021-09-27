---
title: リアルタイム顧客データプラットフォームB2Bエディションのスキーマ
description: リアルタイム顧客データプラットフォームB2Bエディションにおけるエクスペリエンスデータモデル(XDM)スキーマの役割の概要です。
source-git-commit: d83ad2870b6099d3c6359dcc7cd000ecad8a238f
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 3%

---

# リアルタイム顧客データプラットフォームB2Bエディション（ベータ版）のスキーマ

>[!IMPORTANT]
>
>リアルタイム顧客データプラットフォームB2Bエディションは現在ベータ版です。 ドキュメントと機能は変更される場合があります。

リアルタイム顧客データプラットフォームB2Bエディションは、アカウント、オポチュニティ、キャンペーンなど、重要なB2Bデータエンティティの詳細を取り込む、いくつかの標準的な[エクスペリエンスデータモデル(XDM)クラス](../../xdm/schema/composition.md#class)を提供します。 さらに、Real-time CDP B2B Editionを使用すると、これらのスキーマ間で多対1の関係を定義して、高度なセグメント化の使用例に参加できます。

リアルタイムCDP B2Bエディションでは、次の標準クラスが提供されます。

* [XDMビジネスアカウント](../../xdm/classes/b2b/business-account.md)
* [XDMビジネスアカウント担当者の関係](../../xdm/classes/b2b/business-account-person-relation.md)
* [XDM Business Campaign](../../xdm/classes/b2b/business-campaign.md)
* [XDM Business Campaignメンバー](../../xdm/classes/b2b/business-campaign-members.md)
* [XDMビジネス機会](../../xdm/classes/b2b/business-opportunity.md)
* [XDM Business Opportunity Person関係](../../xdm/classes/b2b/business-opportunity-person-relation.md)
* [XDM Business Marketing List](../../xdm/classes/b2b/business-marketing-list.md)
* [XDMビジネスマーケティングリストメンバー](../../xdm/classes/b2b/business-marketing-list-members.md)

2つのスキーマ間に多対1の関係を作成する手順については、[B2Bスキーマの関係の定義](../../xdm/tutorials/relationship-b2b.md)に関するチュートリアルを参照してください。

B2Bソース接続を使用している場合は、ツールを使用して、必要なスキーマとそれらの間の関係を自動的に生成できます。 詳しくは、ソースドキュメントの[B2B名前空間](../../sources/connectors/adobe-applications/marketo/marketo-namespaces.md)のガイドを参照してください。
