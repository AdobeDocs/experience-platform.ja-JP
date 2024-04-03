---
title: プレーヤーステートデータレポートのデータタイプ
description: プレーヤーステートデータレポートのエクスペリエンスデータモデル (XDM) データタイプについて説明します。
source-git-commit: b6b916c76d1b2babb673d419ab69ae414dd42f20
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 4%

---

# [!UICONTROL プレーヤーステートデータのレポート] データタイプ

[!UICONTROL プレーヤーステートデータのレポート] は、メディアプレーヤー内の様々な状態とその発生件数を記述する標準的な Experience Data Model(XDM) データ型です。 以下を使用します。 [!UICONTROL プレーヤーステートデータのレポート] フルスクリーン、ミュート、クローズドキャプション、ピクチャーインピクチャー、フォーカス設定ステートなど、様々なプレーヤーステートをキャプチャするためのデータタイプ。 ステートごとに、ステートが設定されているかどうか、発生数、メディア再生中にアクティブなままの合計時間が記録されます。

![プレーヤーステートデータレポートのデータタイプを示す図です。](../images/data-types/player-state-data-information.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|-------------------|----------------|-----------|----------------------------------------------|
| [!UICONTROL プレーヤーステート名] | `name` | 文字列 | プレーヤーの状態の名前。 列挙： &quot;fullscreen&quot;、&quot;mute&quot;、&quot;closedCaptioning&quot;、&quot;pictureInPicture&quot;、&quot;inFocus&quot;と、それぞれの意味を持ちます。 |
| [!UICONTROL プレーヤーステートセット] | `isSet` | ブール値 | プレーヤーの状態がその状態に設定されているかどうか。 |
| [!UICONTROL プレーヤーの状態の数] | `count` | 整数 | ストリーム上でプレーヤーの状態が設定された回数。 |
| [!UICONTROL プレーヤーステートタイム] | `time` | 整数 | そのプレーヤーの状態の合計時間。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/components/datatypes/playerstatedata.schema.json)
