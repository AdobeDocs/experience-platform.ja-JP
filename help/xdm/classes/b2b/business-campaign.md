---
title: XDM ビジネスキャンペーンクラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の XDM ビジネスキャンペーンクラスの概要を説明します。
exl-id: 4e3228a1-74be-43af-b355-45d84afb1611
source-git-commit: b5cdd72238f7b4519de1c789f4294b9698415327
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 10%

---

# [!UICONTROL XDM ビジネス] キャンペーンクラス（ベータ版）

>[!IMPORTANT]
>
>このクラスは、現在ベータ版のReal-time Customer Data Platform B2B Edition の一部です。 ドキュメントと機能は変更される場合があります。

[!UICONTROL XDM Business Campaign] は、ビジネスキャンペーンに最低限必要なプロパティを取り込む、標準のエクスペリエンスデータモデル (XDM) クラスです。

![](../../images/classes/b2b/business-campaign.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `campaignKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | キャンペーンエンティティの複合識別子。 |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソース・システム監査属性]](../../data-types/external-source-system-audit-attributes.md) | キャンペーンが外部ソースシステムから送信された場合、このオブジェクトはそのシステムの監査属性を取り込みます。 |
| `_id` | 文字列 | レコードの一意の識別子。 これは、`campaignID` とは別のシステム生成値です。 |
| `campaignDescription` | 文字列 | キャンペーンの説明。 |
| `campaignID` | 文字列 | キャンペーンエンティティの一意の識別子。 |
| `campaignName` | 文字列 | キャンペーンの名前。 |
| `campaignType` | 文字列 | キャンペーンのタイプまたはターゲットオーディエンス。 |

{style=&quot;table-layout:auto&quot;}

このクラスが概念的に他の B2B クラスと関連付けられる方法と、Adobe Experience Platform UI でこれらの関係を確立する方法については、Real-time CDP B2B Edition](../../tutorials/relationship-b2b.md) の [ スキーマの関係に関するガイドを参照してください。
