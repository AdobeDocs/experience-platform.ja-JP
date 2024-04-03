---
title: エラー詳細コレクションのデータタイプ
description: エラー詳細収集エクスペリエンスデータモデル (XDM) データタイプについて説明します。
source-git-commit: 899c656a7295896b291ac5c80df873711bc6f149
workflow-type: tm+mt
source-wordcount: '111'
ht-degree: 8%

---

# [!UICONTROL エラーの詳細] コレクションデータタイプ

[!UICONTROL エラーの詳細] コレクションは、エラーの詳細を説明する標準の Experience Data Model(XDM) データ型です。 以下を使用します。 [!UICONTROL エラーの詳細] エラーソースと ID の詳細をキャプチャするためのコレクションデータ型です。 エラー ID はエラーを識別し、エラーソースは、エラーの送信元がプレーヤーか外部ソースかを指定します。

![Error Details Information データ型の図です。](../images/data-types/error-details-collection.png)

| 表示名 | プロパティ | データタイプ | 必須 | 説明 |
|----------------------------|--------------|-----------|----------|-----------------------------------------------|
| [!UICONTROL エラー ID] | `name` | 文字列 | × | エラー ID。 |
| [!UICONTROL エラーソース] | `source` | 文字列 | × | エラーソース。 列挙：「プレーヤー」、「外部」、それぞれの意味を持つ。 |

{style="table-layout:auto"}
