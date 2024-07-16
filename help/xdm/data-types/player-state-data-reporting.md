---
title: プレーヤーの状態データのレポートデータタイプ
description: プレーヤー状態データレポートのエクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: b01e126d-2467-46b3-8da7-8ec4580595b3
source-git-commit: 799a384556b43bc844782d8b67416c7eea77fbf0
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 21%

---

# [!UICONTROL  プレーヤーの状態データレポート ] データタイプ

[!UICONTROL  プレーヤー状態データレポート ] は、メディアプレーヤー内の様々な状態とその発生について説明する、標準のエクスペリエンスデータモデル（XDM）データタイプです。 [!UICONTROL  プレーヤー状態データレポート ] データタイプを使用すると、フルスクリーン、ミュート、クローズドキャプション、ピクチャーインピクチャー、インフォーカス状態など、様々なプレーヤー状態をキャプチャできます。 各状態について、状態が設定されているかどうか、発生回数、メディアの再生中にアクティブのままである合計時間が記録されます。

![ プレーヤーの状態データレポートのデータタイプを示す図。](../images/data-types/player-state-data-information.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|-------------------|----------------|-----------|----------------------------------------------|
| [!UICONTROL  プレイヤーの州名 ] | `name` | 文字列 | プレイヤーの状態の名前。 列挙：「fullscreen」、「mute」、「closedCaptioning」、「pictureInPicture」、「inFocus」とそれぞれの意味。 |
| [!UICONTROL  プレイヤーの状態の設定 ] | `isSet` | ブール値 | プレイヤーの状態がその状態に設定されているかどうか。 |
| [!UICONTROL  プレイヤーの状態のカウント ] | `count` | 整数 | ストリーム上でプレイヤーの状態が設定された回数。 |
| [!UICONTROL  プレイヤーの状態の時間 ] | `time` | 整数 | そのプレイヤーの状態の合計持続時間。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、[ 公開 XDM リポジトリ ](https://github.com/adobe/xdm/blob/master/components/datatypes/playerstatedata.schema.json) を参照してください。
