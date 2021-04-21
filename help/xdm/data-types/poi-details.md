---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ;poi;poi詳細；目標点の詳細；データ型；データ型；
solution: Experience Platform
title: 目標地点の詳細データ型
topic-legacy: overview
description: このドキュメントでは、目標地点の詳細XDMデータ型の概要を説明します。
exl-id: cab5463b-97a0-400d-a00c-0cd8bf9301a5
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 14%

---

# [!UICONTROL 目標地点の] 詳細データ型

[!UICONTROL 目標地点の] 詳細は、イベントが監視された地理的関連データを示す標準XDMデータ型です。

<img src="../images/data-types/poi-details.png" width="550" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `beaconInteractionDetails` | [[!UICONTROL Beacon]](./beacon.md) | POIインタラクションでアクティブなビーコンの詳細を説明します。 |
| `geoInteractionDetails` | [[!UICONTROL 地域とのやり取りの詳細]](./geo-interaction-details.md) | POIインタラクションでアクティブな地域の詳細を説明します。 |
| `category` | 文字列 | POI定義の管理者がPOIを編成するために割り当てる一般的なカテゴリ。 |
| `distanceToPOICenter` | Double | POIセンターからの推定距離（メートル単位）。 |
| `locatingType` | 文字列 | 位置の決定に使用されるメカニズム。 次の値を指定できます。 <ul><li>`beacon`</li><li>`gps`</li><li>`ip`</li><li>`ip+wifi`</li><li>`wifi-triangulation`</li></ul> |
| `name` | 文字列 | POIに付けられた名前。 |
| `poiID` | 文字列 | POIを表す一意の識別子です。 |
| `type` | 文字列 | POI 定義の管理者が選択したタイピングスキーマを使用した POI の一般的なタイプ。 |

Mixinの詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/poi-detail.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/poi-detail.schema.json)
