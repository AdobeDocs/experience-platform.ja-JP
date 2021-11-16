---
title: XDM ビジネスオポチュニティ人物関係クラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の XDM ビジネスオポチュニティ人物関係クラスの概要を説明します。
exl-id: 7be193d2-52eb-4b28-953b-5e0fc21d8f93
source-git-commit: 8718512a9768158183b9fb6b9e336081e47cd889
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 4%

---

# [!UICONTROL XDM ビジネスオポチュニティ人物関係] クラス

>[!IMPORTANT]
>
>このクラスは、 [Real-time Customer Data Platform B2B エディション](../../../rtcdp/b2b-overview.md). このクラスがに参加するには、Real-time CDP B2B Edition へのアクセス権が必要です。 [リアルタイム顧客プロファイル](../../../profile/home.md).

[!UICONTROL XDM ビジネスオポチュニティ人物関係] は、ビジネスオポチュニティに関連付けられている個人の最小限必要なプロパティをキャプチャする、標準 Experience Data Model(XDM) クラスです。

![](../../images/classes/b2b/business-opportunity-person-relation.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソースシステム監査属性]](../../data-types/external-source-system-audit-attributes.md) | ビジネス人物関係が外部ソースシステムから取得された場合、このオブジェクトはそのシステムの監査属性をキャプチャします。 |
| `opportunityKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | オポチュニティと人物の関係におけるオポチュニティの複合識別子。 |
| `opportunityPersonKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | オポチュニティと人物の関係エンティティの複合識別子。 |
| `personKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | オポチュニティと人物の関係にある人物の複合識別子。 |
| `_id` | 文字列 | レコードの一意の ID。 これは、クラスで取り込まれる他の ID フィールドとは別の、システムで生成される値です。 |
| `opportunityID` | 文字列 | オポチュニティと人物の関係におけるオポチュニティの一意の識別子。 |
| `opportunityPersonID` | 文字列 | オポチュニティと人物の関係エンティティの一意の識別子 |
| `isPrimary` | Boolean | 人物がこのオポチュニティの主要連絡先かどうかを示します。 |
| `personID` | 文字列 | オポチュニティと人物の関係にある人物の一意の識別子。 |
| `personRole` | 文字列 | オポチュニティと人物の関係における人物の役割。 |

{style=&quot;table-layout:auto&quot;}

詳しくは、 [リアルタイム CDP B2B エディションのスキーマの関係](../../tutorials/relationship-b2b.md) このクラスが他の B2B クラスと概念的にどのように関連しているか、およびAdobe Experience Platform UI でこれらの関係を確立する方法を学びます。
