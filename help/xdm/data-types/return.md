---
title: 返されるデータタイプ
description: リターンエクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: 1fd99a25-547f-49e7-8980-dda7db2ebb8a
source-git-commit: 8be502c9eea67119dc537a5d63a6c71e0bff1697
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 8%

---

# [!UICONTROL &#x200B; 戻り値 &#x200B;] データタイプ

[!UICONTROL Return] は、Return Merchandise Authorization （RMA）に関連する重要な情報をキャプチャする標準の Experience Data Model （XDM）データタイプです。

![ 戻り値のデータタイプの図。](../images/data-types/return.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|----------------------------------|----------------------|-----------|--------------------------------------------------|
| [!UICONTROL &#x200B; リターン ID] | `returnID` | 文字列 | この RMA の一意の ID。 |
| [!UICONTROL &#x200B; 復帰ステータス &#x200B;] | `returnStatus` | 文字列 | RMA の現在のステータス （保留中またはクローズ済みなど）。 |
| [!UICONTROL &#x200B; 注文購入 ID] | `purchaseID` | 文字列 | RMA に関連する注文/購入の一意の ID。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/return.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/return.schema.json)
