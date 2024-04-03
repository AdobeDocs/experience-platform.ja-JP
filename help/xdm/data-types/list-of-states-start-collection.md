---
title: List of States Start コレクションのデータ型
description: 状態のリスト開始データタイプエクスペリエンスデータモデル (XDM) データタイプについて説明します。
source-git-commit: e9107092b60361216744e154752f48546b5bad73
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 5%

---

# [!UICONTROL List of States Start] データタイプ

The [!UICONTROL List of States Start] データタイプは、様々なプレーヤー属性の開始状態に関する情報を表すために設計された、エクスペリエンスデータモデル (XDM) データタイプです。 これには、 [!UICONTROL プレーヤーステート名] 特定の属性の状態を示すプロパティ（例：「fullscreen」、「mute」、「closedCaptioning」）。 このデータ型は、様々なプレーヤーステートの初期条件をキャプチャおよび記述するために使用されます。

![の図 [!UICONTROL List of States Start] データタイプ。](../images/data-types/list-of-states-start-collection.png)

| 表示名 | プロパティ | データタイプ | 必須 | 説明 |
|--------------------------------|--------------|-----------|-----------|-------------------------------------------------|
| [!UICONTROL プレーヤーステート名] | `name` | 文字列 | × | プレーヤーの状態の名前。 列挙： &quot;fullscreen&quot;、&quot;mute&quot;、&quot;closedCaptioning&quot;、&quot;pictureInPicture&quot;、&quot;inFocus&quot;と、それぞれの意味を持ちます。 |

{style="table-layout:auto"}
