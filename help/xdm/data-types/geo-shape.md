---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ；地理；地理的形状；データ型；データ型；
solution: Experience Platform
title: 図形データ型
topic-legacy: overview
description: このドキュメントでは、Geo Shape XDMデータタイプの概要を説明します。
exl-id: 50b9d783-a555-45eb-b154-7dc71389e224
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 36%

---

# [!UICONTROL Geo ] Shapedata type

[!UICONTROL 地理] 図形は、地理的な領域の形状を記述する標準のXDMデータ型です。このデータ型は、[スキーマ.org](https://schema.org/GeoShape)に記載されている公開仕様に基づいています。

<img src="../images/data-types/geo-shape.png" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_schema.box` | [[!UICONTROL 地理座標]](./geo-coordinates.md)の配列 | 2つの座標で形成された長方形で囲まれた地理的領域を表します。 最初の座標は長方形の下隅、2番目の座標は上隅です。 |
| `_schema.circle` | [[!UICONTROL 地理座標]](./geo-coordinates.md)の配列 | 地理的な座標を中心に、特定の半径を持つ円状の領域を表します。 |
| `_schema.polygon` | [[!UICONTROL 地域サークル]](./geo-circle.md) | 最初と最後の座標が同じである 4 つ以上の座標の系列。 |
| `_schema.description` | 文字列 | 図形が定義する内容の説明です。 |
| `_schema.elevation` | Double | 図形の特定または最小の標高。この値は [WGS84](http://gisgeography.com/wgs84-world-geodetic-system/) データムに準拠し、メートル単位で測定されます。`ceiling` との組み合わせで、このプロパティを使用して、位置の 3 次元の境界ボックスを表すことができます。 |
| `_id` | 文字列 | システムによって生成された、図形の一意の識別子。 |
| `ceiling` | 重複 | 図形の最大標高。このプロパティは、`elevation`と組み合わせて使用した場合にのみ有効です。 この値は[WGS84](http://gisgeography.com/wgs84-world-geodetic-system/)データムに準拠し、メートル単位で測定されます。 `elevation` との組み合わせで、このプロパティを使用して、位置の 3 次元の境界ボックスを表すことができます。 |
