---
title: 状態のリストの終了コレクションのデータタイプ
description: 状態のリストのエンドコレクションデータタイプエクスペリエンスデータモデル (XDM) データタイプについて説明します。
source-git-commit: e9107092b60361216744e154752f48546b5bad73
workflow-type: tm+mt
source-wordcount: '131'
ht-degree: 5%

---

# [!UICONTROL List of States End] データタイプ

List of States End Collection データ型は、 Experience Data Model(XDM) データ型で、様々なプレーヤー属性の終了状態に関連する情報を表すために設計されたものです。 これには、 [!UICONTROL プレーヤーステート名] 特定の属性の状態を示すプロパティ（例：「fullscreen」、「mute」、「closedCaptioning」）。 このデータ型は、様々なプレーヤーステートの初期条件をキャプチャおよび記述するために使用されます。

![List of States End Collection データ型の図です。](../images/data-types/list-of-states-end-collection.png)

| 表示名 | プロパティ | データタイプ | 必須 | 説明 |
|--------------------------------|--------------|-----------|-----------|-------------------------------------------------|
| [!UICONTROL プレーヤーステート名] | `name` | 文字列 | × | プレーヤーの状態の名前。 列挙： &quot;fullscreen&quot;、&quot;mute&quot;、&quot;closedCaptioning&quot;、&quot;pictureInPicture&quot;、&quot;inFocus&quot;と、それぞれの意味を持ちます。 |

{style="table-layout:auto"}
