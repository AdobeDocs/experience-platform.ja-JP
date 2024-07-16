---
title: 出荷データ タイプ
description: 出荷エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: c3a58e46-c80e-4896-b21c-47dc5a6c869b
source-git-commit: 8be502c9eea67119dc537a5d63a6c71e0bff1697
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 18%

---

# [!UICONTROL  送料 ] データタイプ

[!UICONTROL  配送 ] は、1 つ以上の製品の出荷に関連する詳細を提供する、標準のエクスペリエンスデータモデル（XDM）データタイプです。 ロジスティクスの詳細と注文した商品の配送に関する詳細が含まれます。


![[!UICONTROL  出荷 ] データタイプの図。](../images/data-types/shipping.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|----------------------|-----------------------|-----------|------------------------------------------------------|
| [!UICONTROL  発送方法 ] | `shippingMethod` | 文字列 | 顧客が選択した配送方法。 |
| [!UICONTROL  送金額 ] | `shippingAmount` | 数値 | お客様が配送用に支払う必要があった金額。 |
| [!UICONTROL  通貨コード ] | `currencyCode` | 文字列 | 商品の価格設定に使用されるアルファベットの ISO 4217 通貨コード。 |
| [!UICONTROL  出荷先 ] | `shippingDestination` | 文字列 | ユーザーが指定した出荷先（自宅、店舗など）。 |
| [!UICONTROL  出荷日 ] | `shipDate` | 文字列 | 注文から 1 つ以上の品目が出荷された日付。 |
| [!UICONTROL  発送先住所 ] | `address` | [[!UICONTROL address]](./address.md) | 配送先住所。 |
| [!UICONTROL  追跡番号 ] | `trackingNumber` | 数値 | 配送業者によって指定されたトラッキング番号。 |
| [!UICONTROL  トラッキング URL] | `trackingURL` | 文字列 | 注文項目の配送ステータスをトラッキングする URL。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/shipping.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/shipping.schema.json)
