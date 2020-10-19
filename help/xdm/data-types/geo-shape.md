---
keywords: Experience Platform;home;popular topics;schema;Schema;XDM;fields;schemas;Schemas;geo;geo shape;datatype;data-type;data type;
solution: Experience Platform
title: 図形データ型
topic: overview
description: このドキュメントでは、Geo Shape XDMデータタイプの概要を説明します。
translation-type: tm+mt
source-git-commit: 27ce9b6e8608bbfccac25387ba96f998272273c1
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 39%

---


# [!UICONTROL 地理形状] データ型

[!UICONTROL [地理形状] ]は、地理的な領域の形状を記述する標準のXDMデータ型です。 このデータ型は、 [スキーマ.orgに記載されている公開仕様に基づいています](https://schema.org/GeoShape)。

<img src="../images/data-types/geo-shape.png" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_schema.box` | 地理 [[!UICONTROL 座標の配列]](./geo-coordinates.md) | 2つの座標で形成された長方形で囲まれた地理的領域を表します。 最初の座標は長方形の下隅、2番目の座標は上隅です。 |
| `_schema.circle` | 地理 [[!UICONTROL 座標の配列]](./geo-coordinates.md) | 地理的な座標を中心に、特定の半径を持つ円状の領域を表します。 |
| `_schema.polygon` | [[!UICONTROL 地域サークル]](./geo-circle.md) | 最初と最後の座標が同じである 4 つ以上の座標の系列。 |
| `_schema.description` | 文字列 | 図形が定義する内容の説明です。 |
| `_schema.elevation` | Double | 図形の特定または最小の標高。この値は [WGS84](http://gisgeography.com/wgs84-world-geodetic-system/) データムに準拠し、メートル単位で測定されます。`ceiling` との組み合わせで、このプロパティを使用して、位置の 3 次元の境界ボックスを表すことができます。 |
| `_id` | 文字列 | システムによって生成された、図形の一意の識別子。 |
| `ceiling` | Double | 図形の最大標高。このプロパティは、と組み合わせて使用する場合のみ有効で `elevation`す。 The value conforms to the [WGS84](http://gisgeography.com/wgs84-world-geodetic-system/) datum and is measured in meters. `elevation` との組み合わせで、このプロパティを使用して、位置の 3 次元の境界ボックスを表すことができます。 |
