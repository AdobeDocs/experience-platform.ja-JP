---
title: エラーの詳細収集データタイプ
description: Error Details Collection Experience Data Model （XDM）データタイプについて説明します。
exl-id: 54b03147-9bca-46af-86c8-90e42b4de26b
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '101'
ht-degree: 10%

---

# [!UICONTROL Error Details] コレクション データ型

[!UICONTROL Error Details] コレクションは、エラーの詳細を説明する標準のExperience Data Model （XDM） データ型です。 [!UICONTROL Error Details] コレクション データ型を使用して、エラーソースとIDの詳細を取得します。 エラーIDはエラーを識別し、エラーソースはプレーヤーまたは外部ソースから発生しているかどうかを指定します。

![ エラーの詳細データ型の図。](../images/data-types/error-details-collection.png)

| 表示名 | プロパティ | データタイプ | 必須 | 説明 |
|----------------------------|--------------|-----------|----------|-----------------------------------------------|
| [!UICONTROL Error ID] | `name` | 文字列 | × | エラーID。 |
| [!UICONTROL Error Source] | `source` | 文字列 | × | エラーソース。 列挙：「player」、「external」とそれぞれの意味。 |

{style="table-layout:auto"}
