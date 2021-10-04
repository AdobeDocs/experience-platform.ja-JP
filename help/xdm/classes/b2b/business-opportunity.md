---
title: XDM Business Opportunity クラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の XDM Business Opportunity クラスの概要を説明します。
exl-id: d816b0f9-fd37-45da-aa55-247f7f662da0
source-git-commit: b5cdd72238f7b4519de1c789f4294b9698415327
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 9%

---

# [!UICONTROL XDM Business ] Opportunityclass（ベータ版）

>[!IMPORTANT]
>
>このクラスは、現在ベータ版のリアルタイム顧客データプラットフォーム B2B エディションの一部です。 ドキュメントと機能は変更される場合があります。

[!UICONTROL XDM Business Opportunity] は、ビジネスオポチュニティに最低限必要なプロパティを取り込む、標準の Experience Data Model(XDM) クラスです。

![](../../images/classes/b2b/business-opportunity.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `accountKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | この営業案件に関連付けられている勘定の複合識別子。 |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソース・システム監査属性]](../../data-types/external-source-system-audit-attributes.md) | オポチュニティが外部ソース・システムから発生した場合、このオブジェクトはそのシステムの監査属性を取り込みます。 |
| `opportunityKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | 営業案件エンティティの複合識別子。 |
| `_id` | 文字列 | レコードの一意の識別子。 これは、`opportunityID` とは別のシステム生成値です。 |
| `accountID` | 文字列 | このオポチュニティに関連付けられているアカウントの一意の ID。 |
| `opportunityDescription` | 文字列 | 営業案件の説明。 |
| `opportunityID` | 文字列 | 営業案件エンティティの一意の ID です。 |
| `opportunityName` | 文字列 | 商談の名前。 |
| `opportunityStage` | 文字列 | 商談の販売段階。 |
| `opportunityType` | 文字列 | 商談のタイプ。 |

{style=&quot;table-layout:auto&quot;}

このクラスが概念的に他の B2B クラスと関連付けられる方法と、Adobe Experience Platform UI でこれらの関係を確立する方法については、Real-time CDP B2B Edition](../../tutorials/relationship-b2b.md) の [ スキーマの関係に関するガイドを参照してください。
