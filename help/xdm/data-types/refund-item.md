---
title: 払い戻し品目データタイプ
description: 返金品目エクスペリエンスデータモデル (XDM) データタイプについて説明します。
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 13%

---

# [!UICONTROL 払い戻し項目] データタイプ

[!UICONTROL 払い戻し項目] は標準の Experience Data Model(XDM) データタイプで、注文に関連する返金に関連する情報を取得する方法を説明します。

![払い戻し品目のデータタイプを示す図。](../images/data-types/refund-item.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|--------------------|-----------------------|-----------|---------------------------------------------------------------------------------------------------|
| [!UICONTROL トランザクション ID] | `transactionID` | 文字列 | この返金品目の一意のトランザクション識別子。 |
| [!UICONTROL 払い戻し金額] | `refundAmount` | 数値 | 払い戻しの値。 |
| [!UICONTROL 払い戻しの理由] | `refundReason` | 文字列 | 払い戻しが行われた理由。 |
| [!UICONTROL 払い戻しの支払いタイプ] | `refundPaymentType` | 文字列 | この注文の支払方法。カスタム値を使用できます。 |
| [!UICONTROL 通貨コード] | `currencyCode` | 文字列 | この返金項目に使用される ISO 4217 通貨コード。 例： 「USD」、「EUR」。 |

{style="table-layout:auto"}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/refunditem.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/refunditem.schema.json)
