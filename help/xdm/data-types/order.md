---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM；フィールド；スキーマ；スキーマ；順序；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: 注文データタイプ
description: 注文エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: abfc6d53-ffe6-4692-ad65-03d556831fa0
source-git-commit: 09ca510da0819ab38687edadbcc632ccbbe8ef83
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 13%

---

# [!UICONTROL  順序 ] データタイプ

[!UICONTROL  注文 ] は、製品リストに対する注文を記述する標準の Experience Data Model （XDM）データタイプです。

![[!UICONTROL Order] データタイプの図。](../images/data-types/order.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|-------------------------|-------------------------|-----------|------------------------------------------------------------------------------------------------------------------|
| 購入 ID | `purchaseID` | 文字列 | この購入または契約の販売者によって割り当てられた一意の ID。 ID は販売者によって定義されるので、ID が一意であるという保証はありません。 |
| 発注番号 | `purchaseOrderNumber` | 文字列 | この購入または契約の購入者によって割り当てられた一意の ID。 |
| 支払リスト | `payments` | [[!UICONTROL  支払項目 ]](./payment-item.md) の配列 | この注文の支払いのリスト。 支払いの詳細は、[!UICONTROL  支払項目 ] の仕様に記載されています。 |
| 払い戻しリスト | `refunds` | 配列 [[!UICONTROL  払戻品目 ]](./refund-item.md) | この注文の払い戻しのリスト。 払い戻しは、[!UICONTROL  払い戻し項目 ] 仕様に詳しく記載されています。 |
| 情報を返す | `returns` | [[!UICONTROL  情報を返す ]](./return.md) | 発行された RMA （返品承認）。 戻り値の詳細は、[!UICONTROL Return Info] 仕様に記載されています。 |
| 通貨 | `currencyCode` | 文字列 | 注文合計に使用される ISO 4217 通貨コード。 例としては、`USD` や `EUR` があります。 すべてのインスタンスは、パターン `^[A-Z]{3}$` と一致する必要があります。 |
| 税額 | `taxAmount` | 数値 | 最終支払の一部として購入者が支払った税額。 |
| 割引額 | `discountAmount` | 数値 | 個々の製品ではなく、注文全体に適用される通常価格と特別価格の差。 |
| 価格合計 | `priceTotal` | 数値 | すべての割引と税金が適用された後の、この注文の合計価格。 |
| 注文タイプ | `orderType` | 文字列 | 実行された注文のタイプ。 使用可能な値は `checkout` および `instant_purchase` です。 |
| 最終更新日 | `lastUpdatedDate` | 文字列 | コマースシステムで特定の注文レコードが最後に更新された時間。 形式：日時。 |
| 作成日 | `createdDate` | 文字列 | コマースシステムで新しい注文が作成された日時。 形式：日時。 |
| キャンセル日 | `cancelDate` | 文字列 | 買い物客によって注文のキャンセルが開始された日時。 形式：日時。 |
| 払い戻された合計金額 | `refundTotal` | 数値 | 注文の払い戻しに含まれる合計金額。すべての払い戻された項目を組み合わせ、割引などの後に表示されます。 が適用されました。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/data/order.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/data/order.schema.json)
