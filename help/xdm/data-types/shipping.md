---
title: 配送先データタイプ
description: Shipping Experience Data Model(XDM) データタイプについて説明します。
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 9%

---

# [!UICONTROL 送料] データタイプ

[!UICONTROL 送料] は、1 つ以上の製品の出荷に関する詳細を提供する標準の Experience Data Model(XDM) データタイプです。 物流に関する詳細や、注文品の配送に関する詳細が含まれます。


![の図 [!UICONTROL 送料] データタイプ。](../images/data-types/shipping.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|----------------------|-----------------------|-----------|------------------------------------------------------|
| [!UICONTROL 発送方法] | `shippingMethod` | 文字列 | 顧客が選択した発送方法。 |
| [!UICONTROL 送料] | `shippingAmount` | 数値 | 顧客が送料を支払う必要があった金額。 |
| [!UICONTROL 通貨コード] | `currencyCode` | 文字列 | 製品の価格設定に使用される ISO 4217 アルファベット通貨コード。 |
| [!UICONTROL 発送先] | `shippingDestination` | 文字列 | ユーザーが指定した出荷先の宛先（例：home、store など）。 |
| [!UICONTROL 出荷日] | `shipDate` | 文字列 | 1 つ以上の注文品が出荷された日付。 |
| [!UICONTROL 発送先住所] | `address` | [[!UICONTROL 住所]](./address.md) | 送付先住所。 |
| [!UICONTROL 追跡番号] | `trackingNumber` | 数値 | 輸送業者が提供する追跡番号。 |
| [!UICONTROL トラッキング URL] | `trackingURL` | 文字列 | 注文項目の発送ステータスを追跡する URL。 |

{style="table-layout:auto"}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/shipping.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/shipping.schema.json)
