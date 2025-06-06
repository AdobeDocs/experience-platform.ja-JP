---
title: 払戻品目データ タイプ
description: 払戻品目エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: 9968d314-c6f3-49d9-b860-709d7478c43a
source-git-commit: 8be502c9eea67119dc537a5d63a6c71e0bff1697
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 13%

---

# [!UICONTROL &#x200B; 払戻品目 &#x200B;] データタイプ

[!UICONTROL &#x200B; 払戻項目 &#x200B;] は、注文に関連付けられた払い戻しに関連する取得情報を記述する、標準のエクスペリエンスデータモデル（XDM）データタイプです。

![ 払戻品目データタイプの図。](../images/data-types/refund-item.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|--------------------|-----------------------|-----------|---------------------------------------------------------------------------------------------------|
| [!UICONTROL &#x200B; トランザクション ID] | `transactionID` | 文字列 | この払戻品目の一意のトランザクション ID。 |
| [!UICONTROL &#x200B; 払戻金額 &#x200B;] | `refundAmount` | 数値 | 払い戻しの金額。 |
| [!UICONTROL &#x200B; 還付事由 &#x200B;] | `refundReason` | 文字列 | 払い戻しが発行された理由。 |
| [!UICONTROL &#x200B; 払戻金納付の種類 &#x200B;] | `refundPaymentType` | 文字列 | この注文の支払方法。カスタム値を使用できます。 |
| [!UICONTROL &#x200B; 通貨コード &#x200B;] | `currencyCode` | 文字列 | この払い戻し項目に使用される ISO 4217 通貨コード。 例：「USD」、「EUR」。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/refunditem.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/refunditem.schema.json)
