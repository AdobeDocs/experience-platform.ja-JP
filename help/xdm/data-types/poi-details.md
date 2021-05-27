---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；poi;poi詳細；目標地点；目標地点の詳細；データ型；データ型；
solution: Experience Platform
title: 目標地点の詳細データ型
topic-legacy: overview
description: このドキュメントでは、目標地点の詳細のXDMデータタイプの概要を説明します。
exl-id: cab5463b-97a0-400d-a00c-0cd8bf9301a5
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 15%

---

# [!UICONTROL 目標地点の詳細デ] ータ型

[!UICONTROL 目標地点の詳細] は、イベントが観察された地理関連データを記述する標準のXDMデータ型です。

<img src="../images/data-types/poi-details.png" width="550" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `beaconInteractionDetails` | [[!UICONTROL ビーコン]](./beacon.md) | POIインタラクションでアクティブなビーコンの詳細を説明します。 |
| `geoInteractionDetails` | [[!UICONTROL 地域インタラクションの詳細]](./geo-interaction-details.md) | POIインタラクションでアクティブな地域の詳細を説明します。 |
| `category` | 文字列 | POI定義の管理者がPOIを整理するために割り当てた一般的なカテゴリ。 |
| `distanceToPOICenter` | Double | POIセンターからの推定距離（メートル単位）。 |
| `locatingType` | 文字列 | 位置の特定に使用されるメカニズム。 指定できる値は次のとおりです。 <ul><li>`beacon`</li><li>`gps`</li><li>`ip`</li><li>`ip+wifi`</li><li>`wifi-triangulation`</li></ul> |
| `name` | 文字列 | POIに付けられた名前。 |
| `poiID` | 文字列 | POIの一意の識別子。 |
| `type` | 文字列 | POI 定義の管理者が選択したタイピングスキーマを使用した POI の一般的なタイプ。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/poi-detail.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/poi-detail.schema.json)
