---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ；支払い項目；データ型；データ型；
solution: Experience Platform
title: 支払品目データタイプ
topic-legacy: overview
description: このドキュメントでは、支払品目エクスペリエンスデータモデル(XDM)のデータタイプの概要を説明します。
exl-id: d25a358b-73c1-468b-a9c5-808385689932
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 24%

---

# [!UICONTROL 支払] 項目データタイプ

[!UICONTROL 支払] 項目は、支払の種類、金額および関連通貨を定義する注文に関連付けられた支払を記述する、標準のExperience Data Model(XDM)データ型です。

<img src="../images/data-types/payment-item.PNG" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `currencyCode` | 文字列 | 注文の合計に使用される ISO 4217 通貨コード。すべてのインスタンスは、正規式`^[A-Z]{3}$`に準拠する必要があります。 例として、`USD` および `EUR` があります。 |
| `paymentAmount` | Double | 支払の値。 |
| `paymentType` | 文字列 | この注文の支払方法。次の列挙値を指定できます。 <li> `cash` </li> <li> `credit_card` </li> <li> `debit_card` </li> <li> `gift_card` </li> <li> `check` </li> <li> `paypal` </li> <li> `wire_transfer` </li> <li> `credit_card_reference` </li> <li> `other` </li> |
| `transactionID` | 文字列 | この支払品目の一意のトランザクション識別子。 |

データ型の詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/data/paymentitem.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/data/paymentitem.schema.json)
