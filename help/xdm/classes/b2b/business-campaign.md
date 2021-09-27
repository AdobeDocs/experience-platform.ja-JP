---
title: XDMビジネスキャンペーンクラス
description: このドキュメントでは、エクスペリエンスデータモデル(XDM)のXDMビジネスキャンペーンクラスの概要を説明します。
source-git-commit: 19bb39b66f3a3eb93fd0138ac021568021d77b0f
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 6%

---

# [!UICONTROL XDM Business ] Campaignclass

>[!NOTE]
>
>このクラスは、リアルタイム顧客データプラットフォームのB2Bエディションにアクセスできる組織でのみ使用できます。

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
