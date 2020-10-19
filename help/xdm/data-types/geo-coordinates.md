---
keywords: Experience Platform;home;popular topics;schema;Schema;XDM;fields;schemas;Schemas;geo;coordinates;datatype;data-type;data type;
solution: Experience Platform
title: 地域座標データタイプ
topic: overview
description: このドキュメントでは、地理座標のXDMデータタイプの概要を説明します。
translation-type: tm+mt
source-git-commit: f5bddb39c16eb25e85297f56e331d3aa51510eb9
workflow-type: tm+mt
source-wordcount: '114'
ht-degree: 17%

---


# [!UICONTROL 地域座標] データタイプ

[!UICONTROL 地域座標] (Geo Coordinates)は、ある場所の地理的座標を記述する標準のXDMデータ型です。 このデータ型は、 [スキーマ.orgに記載されている公開仕様に基づいています](https://schema.org/GeoCoordinates)。

<img src="../images/data-types/geo-coordinates.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_schema.description` | 文字列 | 座標が何を識別するかの説明。 |
| `_schema.elevation` | Double | 定義した座標の特定の標高。 The value must conform to the [WGS84](http://gisgeography.com/wgs84-world-geodetic-system/) datum and is measured in meters. |
| `_schema.latitude` | Double | 地理的な点の符号付き垂直座標です。 |
| `_schema.longitude` | Double | 地理的位置の符号付き水平座標です。 |
| `_id` | 文字列 | システムによって生成された、座標の一意のID。 |
