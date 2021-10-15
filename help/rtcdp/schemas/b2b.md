---
title: Real-time Customer Data Platform B2B Edition のスキーマ
description: Real-time Customer Data Platform B2B Edition での Experience Data Model(XDM) スキーマの役割の概要です。
exl-id: 3b18d377-108f-443f-86ae-dc7537cf9013
source-git-commit: 65afc75d89a4183d0ad03200f6c08c72dd104180
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 7%

---

# Real-time Customer Data Platform B2B Edition（ベータ版）のスキーマ

>[!IMPORTANT]
>
>Real-time Customer Data Platform B2B Edition は現在ベータ版です。 ドキュメントと機能は変更される場合があります。

Real-time Customer Data Platform B2B Edition は、アカウント、オポチュニティ、キャンペーンなど、重要な B2B データエンティティの詳細を取り込む、いくつかの標準的な [Experience Data Model (XDM) クラス ](../../xdm/schema/composition.md#class) を提供します。 さらに、Real-time CDP B2B Edition を使用すると、これらのスキーマ間で多対 1 の関係を定義して、高度なセグメント化の使用例に参加できます。

リアルタイム CDP B2B エディションでは、次の標準クラスが提供されます。

* [XDM ビジネスアカウント](../../xdm/classes/b2b/business-account.md)
* [XDM ビジネスアカウント担当者の関係](../../xdm/classes/b2b/business-account-person-relation.md)
* [XDM Business Campaign](../../xdm/classes/b2b/business-campaign.md)
* [XDM Business Campaign メンバー](../../xdm/classes/b2b/business-campaign-members.md)
* [XDM ビジネス機会](../../xdm/classes/b2b/business-opportunity.md)
* [XDM Business Opportunity Person 関係](../../xdm/classes/b2b/business-opportunity-person-relation.md)
* [XDM ビジネスマーケティングリスト](../../xdm/classes/b2b/business-marketing-list.md)
* [XDM ビジネスマーケティングリストメンバー](../../xdm/classes/b2b/business-marketing-list-members.md)

2 つのスキーマ間に多対 1 の関係を作成する手順については、[B2B スキーマの関係の定義 ](../../xdm/tutorials/relationship-b2b.md) に関するチュートリアルを参照してください。

B2B ソース接続を使用している場合は、ツールを使用して、必要なスキーマとそれらの間の関係を自動的に生成できます。 詳しくは、ソースドキュメントの [B2B 名前空間 ](../../sources/connectors/adobe-applications/marketo/marketo-namespaces.md) のガイドを参照してください。
