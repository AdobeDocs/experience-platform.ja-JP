---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；支払い項目；データ型；データ型；
solution: Experience Platform
title: 支払い項目データタイプ
description: このドキュメントでは、支払い項目エクスペリエンスデータモデル (XDM) データタイプの概要を説明します。
exl-id: d25a358b-73c1-468b-a9c5-808385689932
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 26%

---

# [!UICONTROL 支払い項目] データタイプ

[!UICONTROL 支払い項目] は標準の Experience Data Model(XDM) データ型で、注文に関連付けられた支払いを記述し、支払いのタイプ、金額および関連する通貨を定義します。

<img src="../images/data-types/payment-item.PNG" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `currencyCode` | 文字列 | 注文の合計に使用される ISO 4217 通貨コード。すべてのインスタンスは、正規表現に準拠している必要があります `^[A-Z]{3}$`. 例として、`USD` および `EUR` があります。 |
| `paymentAmount` | Double | 支払の値。 |
| `paymentType` | 文字列 | この注文の支払方法。使用できる列挙値は次のとおりです。 <li> `cash` </li> <li> `credit_card` </li> <li> `debit_card` </li> <li> `gift_card` </li> <li> `check` </li> <li> `paypal` </li> <li> `wire_transfer` </li> <li> `credit_card_reference` </li> <li> `other` </li> |
| `transactionID` | 文字列 | この支払品目の一意のトランザクション識別子。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/data/paymentitem.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/data/paymentitem.schema.json)
