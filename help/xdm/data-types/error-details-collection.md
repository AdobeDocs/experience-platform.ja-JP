---
title: エラーの詳細コレクション データ型
description: エラー詳細収集エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: 54b03147-9bca-46af-86c8-90e42b4de26b
source-git-commit: 799a384556b43bc844782d8b67416c7eea77fbf0
workflow-type: tm+mt
source-wordcount: '111'
ht-degree: 12%

---

# [!UICONTROL &#x200B; エラーの詳細 &#x200B;] 収集データタイプ

[!UICONTROL &#x200B; エラーの詳細 &#x200B;] コレクションは、エラーの詳細を説明する標準の Experience Data Model （XDM）データタイプです。 [!UICONTROL &#x200B; エラーの詳細 &#x200B;] 収集データタイプを使用して、エラーソースと ID の詳細をキャプチャします。 エラー ID がエラーを識別し、エラーソースが、プレーヤーと外部ソースのどちらから発生したかを特定します。

![ エラーの詳細情報のデータタイプを示す図。](../images/data-types/error-details-collection.png)

| 表示名 | プロパティ | データタイプ | 必須 | 説明 |
|----------------------------|--------------|-----------|----------|-----------------------------------------------|
| [!UICONTROL &#x200B; エラー ID] | `name` | 文字列 | × | エラー ID。 |
| [!UICONTROL &#x200B; エラーSource] | `source` | 文字列 | × | エラーソース。 列挙：「player」、「external」とそれぞれの意味。 |

{style="table-layout:auto"}
