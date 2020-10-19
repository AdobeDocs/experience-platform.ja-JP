---
keywords: Experience Platform;home;popular topics;schema;Schema;XDM;fields;schemas;Schemas;geo;circle;datatype;data-type;data type;
solution: Experience Platform
title: 地域サークルデータタイプ
topic: overview
description: このドキュメントでは、Geo Circle XDMデータタイプの概要を説明します。
translation-type: tm+mt
source-git-commit: 27ce9b6e8608bbfccac25387ba96f998272273c1
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 26%

---


# [!UICONTROL 地域サークル] （データ型）

[!UICONTROL 地理円] (Geo Circle)は、特定の一連の座標を中心にした特定の半径を指定した、円形の地理的な領域を表す標準的なXDMデータ型です。 このデータ型は、 [スキーマ.orgに記載されている公開仕様に基づいています](http://schema.org/GeoCircle)。

<img src="../images/data-types/geo-circle.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_schema.coordinates` | [[!UICONTROL 地理座標]](./geo-coordinates.md) | 円の中心の地理的座標を示します。 |
| `_schema.description` | 文字列 | 円に含まれる内容の説明。 |
| `_schema.radius` | Double | 円の半径の長さ。この値は [WGS84](http://gisgeography.com/wgs84-world-geodetic-system/) データムに準拠し、メートル単位で測定されます。 |
| `_id` | 文字列 | システム生成でサークルに一意のID。 |
