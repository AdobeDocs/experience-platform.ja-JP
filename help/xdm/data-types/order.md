---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ；順序；データ型；データ型；
solution: Experience Platform
title: 注文データタイプ
topic-legacy: overview
description: このドキュメントでは、Order Experience Data Model(XDM)データタイプの概要を説明します。
exl-id: abfc6d53-ffe6-4692-ad65-03d556831fa0
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 22%

---

#  Orderdata型

 Orderは、製品リストに対して発注された注文を説明する標準のExperience Data Model(XDM)データ型です。

<img src="../images/data-types/order.PNG" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `payments` | [[!UICONTROL 支払項目]](./payment-item.md)の配列 | この注文のリスト。 |
| `currencyCode` | 文字列 | 注文の合計に使用される ISO 4217 通貨コード。すべてのインスタンスは、正規式`^[A-Z]{3}$`に準拠する必要があります。 例として、`USD` および `EUR` があります。 |
| `priceTotal` | Double | すべての割引と税金が適用された後の、この注文の合計価格。 |
| `purchaseID` | 文字列 | この購入または契約に対して売り手が割り当てた一意の識別子。 これは販売者が定義するので、IDが一意である保証はありません。 |
| `purchaseOrderNumber` | 文字列 | 購入者がこの購入または契約に対して割り当てた一意の識別子。 |

データ型の詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/data/order.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/data/order.schema.json)
