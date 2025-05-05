---
title: 返品品目のデータ型
description: 返品項目エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: e703d65b-a133-484e-96d6-6b1f50fc1e48
source-git-commit: 8be502c9eea67119dc537a5d63a6c71e0bff1697
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 7%

---

# [!UICONTROL &#x200B; 戻り値の項目 &#x200B;] データタイプ

[!UICONTROL &#x200B; 返品アイテム &#x200B;] は、購入した品目の返品プロセスに関連する重要な詳細を取得する、標準の Experience Data Model （XDM）データタイプです。

![ 返品品目データ型の図。](../images/data-types/return-item.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|-----------------------------|------------------------------|-----------|--------------------------------------------------------|
| [!UICONTROL &#x200B; 復帰ステータス &#x200B;] | `returnStatus` | 文字列 | 返される項目のステータス（保留中や承認済みなど）。 |
| [!UICONTROL &#x200B; 返品理由 &#x200B;] | `returnReason` | 文字列 | 品目に対して返品が要求された理由。 |
| [!UICONTROL &#x200B; 返品品目の条件 &#x200B;] | `returnItemCondition` | 文字列 | 返品を要求する品目の条件。 |
| [!UICONTROL &#x200B; 再来訪の解決策 &#x200B;] | `returnResolution` | 文字列 | 返品から期待される目的の解決策または結果（返金、交換など）。 |
| [!UICONTROL &#x200B; 返品希望数量 &#x200B;] | `returnQuantityRequested` | 整数 | 買い物客が返品をリクエストした品目の数量。 |
| [!UICONTROL &#x200B; 返品許可数量 &#x200B;] | `returnQuantityAuthorized` | 整数 | 返品が許可されている品目の数量。 |
| [!UICONTROL &#x200B; 返品受入数量 &#x200B;] | `returnQuantityReceived` | 整数 | 返品された品目の受入数量。 |
| [!UICONTROL &#x200B; 返品数量の承認 &#x200B;] | `returnQuantityApproved` | 整数 | 返品が完全に完了し、承認された品目の数量。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/returnitem.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/returnitem.schema.json)
