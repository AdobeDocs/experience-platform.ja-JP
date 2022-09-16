---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；順序；データ型；データ型；データ型；
solution: Experience Platform
title: 注文データタイプ
topic-legacy: overview
description: このドキュメントでは、Order Experience Data Model(XDM) データタイプの概要を説明します。
exl-id: abfc6d53-ffe6-4692-ad65-03d556831fa0
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 23%

---

# [!UICONTROL 注文] データタイプ

[!UICONTROL 注文] は、製品リストに配置される注文を記述する標準 Experience Data Model(XDM) データ型です。

<img src="../images/data-types/order.PNG" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `payments` | の配列 [[!UICONTROL 支払い項目]](./payment-item.md) | この注文のリスト。 |
| `currencyCode` | 文字列 | 注文の合計に使用される ISO 4217 通貨コード。すべてのインスタンスは、正規表現に準拠している必要があります `^[A-Z]{3}$`. 例として、`USD` および `EUR` があります。 |
| `priceTotal` | Double | すべての割引と税金が適用された後の、この注文の合計価格。 |
| `purchaseID` | 文字列 | 販売者がこの購入または契約に割り当てた一意の ID。 これは販売者が定義するので、ID が一意であるという保証はありません。 |
| `purchaseOrderNumber` | 文字列 | 購入者がこの購入または契約に割り当てた一意の ID。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/data/order.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/data/order.schema.json)
