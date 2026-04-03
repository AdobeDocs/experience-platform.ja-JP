---
title: 状態のリスト終了コレクション データ型
description: List of States End Collection Data Type Experience Data Model （XDM）データ型について説明します。
exl-id: e59d12e0-2f18-4637-8a51-41b7b5b59b57
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '121'
ht-degree: 7%

---

# [!UICONTROL List of States End] データ型

List of States End Collection データタイプは、様々なプレイヤー属性の終了状態に関連する情報を表すために設計されたExperience Data Model （XDM）データタイプです。 特定の属性状態（「fullscreen」、「mute」、「closedCaptioning」など）を示す[!UICONTROL Player State Name] プロパティが含まれます。 このデータタイプは、異なるプレイヤーの状態の初期条件をキャプチャして記述するために使用されます。

![状態のリストの図は、コレクションのデータ型を終了します。](../images/data-types/list-of-states-end-collection.png)

| 表示名 | プロパティ | データタイプ | 必須 | 説明 |
|--------------------------------|--------------|-----------|-----------|-------------------------------------------------|
| [!UICONTROL Player State Name] | `name` | 文字列 | × | プレーヤーの状態の名前。 列挙：&quot;fullscreen&quot;, &quot;mute&quot;, &quot;closedCaptioning&quot;, &quot;pictureInPicture&quot;, &quot;inFocus&quot;とそれぞれの意味。 |

{style="table-layout:auto"}
