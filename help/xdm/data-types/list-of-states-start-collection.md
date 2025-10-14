---
title: 状態のリストの収集を開始データタイプ
description: 状態のリスト開始データタイプ エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: adeb3e91-7266-41ce-b406-f7fd5dbb2236
source-git-commit: 799a384556b43bc844782d8b67416c7eea77fbf0
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 7%

---

# [!UICONTROL &#x200B; 状態のリストの開始 &#x200B;] データタイプ

[!UICONTROL &#x200B; 状態の開始のリスト &#x200B;] データタイプは、様々なプレーヤー属性の開始状態に関連する情報を表すために設計されたエクスペリエンスデータモデル（XDM）データタイプです。 特定の属性の状態を示す [!UICONTROL &#x200B; プレーヤーの状態名 &#x200B;] プロパティ（「fullscreen」、「mute」、「closedCaptioning」など）が含まれます。 このデータタイプは、様々なプレーヤー状態の初期条件をキャプチャおよび記述するために使用されます。

![&#x200B; 状態のリストの開始 [!UICONTROL &#x200B; データタイプ &#x200B;] 図 &#x200B;](../images/data-types/list-of-states-start-collection.png)

| 表示名 | プロパティ | データタイプ | 必須 | 説明 |
|--------------------------------|--------------|-----------|-----------|-------------------------------------------------|
| [!UICONTROL &#x200B; プレイヤーの州名 &#x200B;] | `name` | 文字列 | × | プレイヤーの状態の名前。 列挙：「fullscreen」、「mute」、「closedCaptioning」、「pictureInPicture」、「inFocus」とそれぞれの意味。 |

{style="table-layout:auto"}
