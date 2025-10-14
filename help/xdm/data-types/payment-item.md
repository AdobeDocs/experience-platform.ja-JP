---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；支払い項目；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: 支払項目のデータ タイプ
description: 支払項目エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: d25a358b-73c1-468b-a9c5-808385689932
source-git-commit: e028fbb82b37b3940b308a860c26f8b5f9884d3a
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 19%

---

# [!UICONTROL &#x200B; 支払い項目 &#x200B;] データタイプ

[!UICONTROL &#x200B; 支払い項目 &#x200B;] は、注文に関連付けられた支払いを記述する標準の Experience Data Model （XDM）データタイプで、支払いのタイプ、金額、関連する通貨を定義します。

![&#x200B; 支払い項目の画像 &#x200B;](../images/data-types/payment-item.PNG){width=400}

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `currencyCode` | 文字列 | 注文合計に使用される ISO 4217 通貨コード。 すべてのインスタンスは、正規表現 `^[A-Z]{3}$` に従う必要があります。 例としては、`USD` や `EUR` があります。 |
| `paymentAmount` | Double | 支払の値。 |
| `paymentType` | 文字列 | この注文の支払方法。使用可能な列挙値は次のとおりです。 <li> `cash` </li> <li> `credit_card` </li> <li> `debit_card` </li> <li> `gift_card` </li> <li> `check` </li> <li> `paypal` </li> <li> `wire_transfer` </li> <li> `credit_card_reference` </li> <li> `other` </li> |
| `transactionID` | 文字列 | この支払品目の一意のトランザクション識別子。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/data/paymentitem.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/data/paymentitem.schema.json)
