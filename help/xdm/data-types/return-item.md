---
title: 項目データタイプを返す
description: 「Return Item Experience Data Model(XDM)」データタイプについて説明します。
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 6%

---

# [!UICONTROL 項目を返す] データタイプ

[!UICONTROL 項目を返す] は標準のエクスペリエンスデータモデル (XDM) データタイプで、購入した品目の返却プロセスに関する重要な詳細を取り込みます。

![戻り値の項目のデータ型を示す図です。](../images/data-types/return-item.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|-----------------------------|------------------------------|-----------|--------------------------------------------------------|
| [!UICONTROL 戻りステータス] | `returnStatus` | 文字列 | 返される項目のステータス（例：「保留」、「承認済み」）。 |
| [!UICONTROL リターン理由] | `returnReason` | 文字列 | 品目の返品がリクエストされた理由。 |
| [!UICONTROL 返品品目条件] | `returnItemCondition` | 文字列 | 返品が要求された品目の条件。 |
| [!UICONTROL 解像度を返す] | `returnResolution` | 文字列 | 返品から期待される目的の解決または結果（返金や為替など）。 |
| [!UICONTROL リターン数] | `returnQuantityRequested` | 整数 | 買い物客が返品をリクエストした品目の数量。 |
| [!UICONTROL 承認済返品数量] | `returnQuantityAuthorized` | 整数 | 返却を許可された品目の数量。 |
| [!UICONTROL 返品受入数量] | `returnQuantityReceived` | 整数 | 受け取った返品品目の数量。 |
| [!UICONTROL 返品承認数量] | `returnQuantityApproved` | 整数 | 返品が完全に完了し承認された品目の数量。 |

{style="table-layout:auto"}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/returnitem.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/returnitem.schema.json)
