---
title: XDMビジネスキャンペーンクラス
description: このドキュメントでは、エクスペリエンスデータモデル(XDM)のXDMビジネスキャンペーンクラスの概要を説明します。
source-git-commit: 5fd82b02eb25f3d575de695c2f2b14a5e5b18400
workflow-type: tm+mt
source-wordcount: '181'
ht-degree: 4%

---

# [!UICONTROL XDM Business ] Campaignclass

>[!NOTE]
>
>このクラスは、リアルタイム顧客データプラットフォームB2Bエディションにアクセスできる組織でのみ使用できます。

[!UICONTROL XDM Business Campaign] は、ビジネスキャンペーンに最低限必要なプロパティを取り込む、標準のエクスペリエンスデータモデル(XDM)クラスです。

![](../../images/classes/b2b/business-campaign.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `campaignKey` | [[!UICONTROL B2Bソース]](../../data-types/b2b-source.md) | キャンペーンエンティティの複合識別子。 |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソース・システム監査属性]](../../data-types/external-source-system-audit-attributes.md) | キャンペーンが外部ソースシステムから送信された場合、このオブジェクトはそのシステムの監査属性をキャプチャします。 |
| `_id` | 文字列 | レコードの一意の識別子。 これは、`campaignID`とは別のシステム生成値です。 |
| `campaignDescription` | 文字列 | キャンペーンの説明。 |
| `campaignID` | 文字列 | キャンペーンエンティティの一意の識別子。 |
| `campaignName` | 文字列 | キャンペーンの名前。 |
| `campaignType` | 文字列 | キャンペーンのタイプまたはターゲットオーディエンス。 |

このクラスと他のB2Bクラスとの概念的な関係と、Adobe Experience Platform UIでこれらの関係を確立する方法については、 Real-time CDP B2B Edition](../../tutorials/relationship-b2b.md)の[スキーマの関係に関するガイドを参照してください。
