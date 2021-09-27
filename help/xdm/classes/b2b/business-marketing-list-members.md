---
title: XDM Business Marketing List Membersクラス
description: このドキュメントでは、エクスペリエンスデータモデル(XDM)のXDM Business Marketing List Membersクラスの概要を説明します。
source-git-commit: d83ad2870b6099d3c6359dcc7cd000ecad8a238f
workflow-type: tm+mt
source-wordcount: '236'
ht-degree: 6%

---

# [!UICONTROL XDM Business Marketing List Membersclass(ベ] ータ版)

>[!IMPORTANT]
>
>このクラスは、現在ベータ版であるリアルタイム顧客データプラットフォームB2Bエディションの一部として使用できます。 ドキュメントと機能は変更される場合があります。

[!UICONTROL XDM Business Marketing Listメンバー] は、マーケティングリストに関連付けられたメンバー、個人、または連絡先を記述する、標準のExperience Data Model(XDM)クラスです。

![](../../images/classes/b2b/business-marketing-list-members.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソース・システム監査属性]](../../data-types/external-source-system-audit-attributes.md) | マーケティングリストのメンバーシップが外部ソースシステムから取得された場合、このオブジェクトはそのシステムの監査属性を取り込みます。 |
| `marketingListKey` | [[!UICONTROL B2Bソース]](../../data-types/b2b-source.md) | 個人がメンバーであるマーケティングリストの複合識別子。 |
| `marketingListMemberKey` | [[!UICONTROL B2Bソース]](../../data-types/b2b-source.md) | マーケティングリストメンバーシップエンティティの複合識別子。 |
| `personKey` | [[!UICONTROL B2Bソース]](../../data-types/b2b-source.md) | マーケティングリストのメンバーである個人の複合識別子。 |
| `_id` | 文字列 | レコードの一意の識別子。 これは、`marketingListMemberID`とは別のシステム生成値です。 |
| `marketingListID` | 文字列 | マーケティングリストの一意のID。 |
| `marketingListMemberID` | 文字列 | マーケティングリストのメンバーシップエンティティの一意のID。 |
| `personId` | 文字列 | 個人の一意のID。 |

このクラスと他のB2Bクラスとの概念的な関係と、Adobe Experience Platform UIでこれらの関係を確立する方法については、 Real-time CDP B2B Edition](../../tutorials/relationship-b2b.md)の[スキーマの関係に関するガイドを参照してください。
