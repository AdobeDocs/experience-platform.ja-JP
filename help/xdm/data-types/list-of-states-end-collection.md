---
title: 状態のリスト：終了コレクションのデータタイプ
description: 状態のリストについて説明します。エンドコレクションデータタイプ エクスペリエンスデータモデル（XDM）データタイプ。
exl-id: e59d12e0-2f18-4637-8a51-41b7b5b59b57
source-git-commit: 799a384556b43bc844782d8b67416c7eea77fbf0
workflow-type: tm+mt
source-wordcount: '131'
ht-degree: 6%

---

# [!UICONTROL &#x200B; 状態のリスト終了 &#x200B;] データタイプ

状態のリスト エンドコレクションデータタイプ データタイプは、様々なプレーヤー属性の終了状態に関連する情報を表すために設計されたエクスペリエンスデータモデル（XDM）データタイプです。 特定の属性の状態を示す [!UICONTROL &#x200B; プレーヤーの状態名 &#x200B;] プロパティ（「fullscreen」、「mute」、「closedCaptioning」など）が含まれます。 このデータタイプは、様々なプレーヤー状態の初期条件をキャプチャおよび記述するために使用されます。

![&#x200B; 状態のリストエンドコレクションデータタイプの図。](../images/data-types/list-of-states-end-collection.png)

| 表示名 | プロパティ | データタイプ | 必須 | 説明 |
|--------------------------------|--------------|-----------|-----------|-------------------------------------------------|
| [!UICONTROL &#x200B; プレイヤーの州名 &#x200B;] | `name` | 文字列 | × | プレイヤーの状態の名前。 列挙：「fullscreen」、「mute」、「closedCaptioning」、「pictureInPicture」、「inFocus」とそれぞれの意味。 |

{style="table-layout:auto"}
