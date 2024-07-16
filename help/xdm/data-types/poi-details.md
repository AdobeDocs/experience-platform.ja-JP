---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM；フィールド；スキーマ；スキーマ；poi;poi の詳細；目標地点の詳細；目標地点の詳細；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: POI の詳細データ タイプ
description: 目標点の詳細 XDM データタイプについて説明します。
exl-id: cab5463b-97a0-400d-a00c-0cd8bf9301a5
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 16%

---

# [!UICONTROL POI の詳細 ] データタイプ

[!UICONTROL  目標点の詳細 ] は、イベントが観測された地理的関連データを記述する標準 XDM データタイプです。

<img src="../images/data-types/poi-details.png" width="550" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `beaconInteractionDetails` | [[!UICONTROL  ビーコン ]](./beacon.md) | POI インタラクションでアクティブなビーコンの詳細を表します。 |
| `geoInteractionDetails` | [[!UICONTROL  ジオインタラクションの詳細 ]](./geo-interaction-details.md) | POI インタラクションでアクティブなジオの詳細を表します。 |
| `category` | 文字列 | POI 定義の管理者によって POI を整理するために割り当てられる一般的なカテゴリ。 |
| `distanceToPOICenter` | Double | POI の中心からの推定距離（メートル）。 |
| `locatingType` | 文字列 | 位置を特定するために使用されるメカニズム。 使用できる値は次のとおりです。 <ul><li>`beacon`</li><li>`gps`</li><li>`ip`</li><li>`ip+wifi`</li><li>`wifi-triangulation`</li></ul> |
| `name` | 文字列 | POI に付けられた名前。 |
| `poiID` | 文字列 | POI の一意の ID。 |
| `type` | 文字列 | POI 定義の管理者が選択したタイピングスキーマを使用した POI の一般的なタイプ。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/poi-detail.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/poi-detail.schema.json)
