---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ；地理；円；データ型；データ型；
solution: Experience Platform
title: 地域サークルのデータタイプ
topic-legacy: overview
description: このドキュメントでは、Geo Circle XDMデータタイプの概要を説明します。
exl-id: fa041f4f-9955-44e9-b235-a643e07d402c
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '133'
ht-degree: 23%

---

# [!UICONTROL GeoCircledata] タイプ

[!UICONTROL 地理] 円は、特定の一連の座標を中心とする特定の半径を指定した、円形の地理的な領域を表す標準のXDMデータ型です。このデータ型は、[スキーマ.org](http://schema.org/GeoCircle)に記載されている公開仕様に基づいています。

<img src="../images/data-types/geo-circle.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_schema.coordinates` | [[!UICONTROL 地理座標]](./geo-coordinates.md) | 円の中心の地理的座標を示します。 |
| `_schema.description` | 文字列 | 円に含まれる内容の説明。 |
| `_schema.radius` | Double | 円の半径の長さ。この値は [WGS84](http://gisgeography.com/wgs84-world-geodetic-system/) データムに準拠し、メートル単位で測定されます。 |
| `_id` | 文字列 | システム生成でサークルに一意のID。 |
