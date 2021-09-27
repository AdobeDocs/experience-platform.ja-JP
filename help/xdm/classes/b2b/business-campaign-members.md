---
title: XDM Business Campaignメンバークラス
description: このドキュメントでは、エクスペリエンスデータモデル(XDM)のXDM Business Campaign Membersクラスの概要を説明します。
source-git-commit: d83ad2870b6099d3c6359dcc7cd000ecad8a238f
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 6%

---

# [!UICONTROL XDM Business Campaignメンバ] ークラス（ベータ版）

>[!IMPORTANT]
>
>このクラスは、現在ベータ版であるリアルタイム顧客データプラットフォームB2Bエディションの一部として使用できます。 ドキュメントと機能は変更される場合があります。

[!UICONTROL XDM Business Campaignメンバ] ーは、ビジネスキャンペーンに関連付けられた連絡先またはリードを記述する標準のエクスペリエンスデータモデル(XDM)クラスです。

![](../../images/classes/b2b/business-campaign-members.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `campaignKey` | [[!UICONTROL B2Bソース]](../../data-types/b2b-source.md) | 関連するキャンペーンの複合識別子。 |
| `campaignMemberKey` | [[!UICONTROL B2Bソース]](../../data-types/b2b-source.md) | キャンペーンメンバーシップエンティティの複合識別子。 |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソース・システム監査属性]](../../data-types/external-source-system-audit-attributes.md) | キャンペーンのメンバーシップが外部ソースシステムから取得された場合、このオブジェクトはそのシステムの監査属性をキャプチャします。 |
| `personKey` | [[!UICONTROL B2Bソース]](../../data-types/b2b-source.md) | 関連するキャンペーンのメンバーである個人の複合識別子。 |
| `_id` | 文字列 | レコードの一意の識別子。 これは、`campaignMemberID`とは別のシステム生成値です。 |
| `campaignID` | 文字列 | 関連するキャンペーンの一意のID。 |
| `campaignMemberID` | 文字列 | キャンペーンメンバーシップエンティティの一意のID。 |
| `personId` | 文字列 | 関連するキャンペーンのメンバーである個人の一意のID。 |

このクラスと他のB2Bクラスとの概念的な関係と、Adobe Experience Platform UIでこれらの関係を確立する方法については、 Real-time CDP B2B Edition](../../tutorials/relationship-b2b.md)の[スキーマの関係に関するガイドを参照してください。
