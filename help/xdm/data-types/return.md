---
title: データタイプを返す
description: Return Experience Data Model(XDM) データタイプについて説明します。
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 6%

---

# [!UICONTROL 戻る] データタイプ

[!UICONTROL 戻る] は、Return Murchandise Authorization(RMA) に関連する重要な情報を取得する、標準の Experience Data Model(XDM) データタイプです。

![戻り値のデータ型を表す図です。](../images/data-types/return.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|----------------------------------|----------------------|-----------|--------------------------------------------------|
| [!UICONTROL 戻り値 ID] | `returnID` | 文字列 | この RMA の一意の ID。 |
| [!UICONTROL 戻りステータス] | `returnStatus` | 文字列 | RMA の現在のステータス（「保留」や「クローズ」など）。 |
| [!UICONTROL 注文購入 ID] | `purchaseID` | 文字列 | RMA が関連する注文/購入の一意の ID。 |

{style="table-layout:auto"}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/return.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/return.schema.json)

