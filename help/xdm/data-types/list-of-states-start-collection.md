---
title: 状態のリスト開始コレクション データ型
description: 状態のリスト開始データ型エクスペリエンスデータモデル（XDM）データ型について説明します。
exl-id: adeb3e91-7266-41ce-b406-f7fd5dbb2236
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '108'
ht-degree: 8%

---

# [!UICONTROL List of States Start] データ型

[!UICONTROL List of States Start] データ型は、様々なプレーヤー属性の開始状態に関連する情報を表すために設計されたExperience Data Model （XDM） データ型です。 特定の属性状態（「fullscreen」、「mute」、「closedCaptioning」など）を示す[!UICONTROL Player State Name] プロパティが含まれます。 このデータタイプは、異なるプレイヤーの状態の初期条件をキャプチャして記述するために使用されます。

![ データタイプ [!UICONTROL List of States Start]の図。](../images/data-types/list-of-states-start-collection.png)

| 表示名 | プロパティ | データタイプ | 必須 | 説明 |
|--------------------------------|--------------|-----------|-----------|-------------------------------------------------|
| [!UICONTROL Player State Name] | `name` | 文字列 | × | プレーヤーの状態の名前。 列挙：&quot;fullscreen&quot;, &quot;mute&quot;, &quot;closedCaptioning&quot;, &quot;pictureInPicture&quot;, &quot;inFocus&quot;とそれぞれの意味。 |

{style="table-layout:auto"}
