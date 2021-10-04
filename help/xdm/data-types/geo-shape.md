---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；地域；ジオシェイプ；データ型；データ型；
solution: Experience Platform
title: ジオシェイプデータタイプ
topic-legacy: overview
description: このドキュメントでは、ジオシェイプ XDM データタイプの概要を説明します。
exl-id: 50b9d783-a555-45eb-b154-7dc71389e224
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 36%

---

# [!UICONTROL ジオシェ] イプデータタイプ

[!UICONTROL 地域] 図形は、地理的領域の形状を記述する標準の XDM データ型です。このデータ型は、[schema.org](https://schema.org/GeoShape) に記載されている公開仕様に基づいています。

<img src="../images/data-types/geo-shape.png" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_schema.box` | [[!UICONTROL  地理座標 ]](./geo-coordinates.md) の配列 | 2 つの座標で形成された長方形で囲まれた地理的領域を表します。 最初の座標は長方形の下隅、2 番目の座標は上隅です。 |
| `_schema.circle` | [[!UICONTROL  地理座標 ]](./geo-coordinates.md) の配列 | 特定の半径が地理的座標を中心とする円形の領域を表します。 |
| `_schema.polygon` | [[!UICONTROL ジオサークル]](./geo-circle.md) | 最初と最後の座標が同じである 4 つ以上の座標の系列。 |
| `_schema.description` | 文字列 | 図形が定義する内容の説明。 |
| `_schema.elevation` | Double | 図形の特定または最小の標高。この値は [WGS84](http://gisgeography.com/wgs84-world-geodetic-system/) データムに準拠し、メートル単位で測定されます。`ceiling` との組み合わせで、このプロパティを使用して、位置の 3 次元の境界ボックスを表すことができます。 |
| `_id` | 文字列 | システムで生成された、図形の一意の識別子。 |
| `ceiling` | ダブル | 図形の最大標高。このプロパティは、`elevation` と組み合わせて使用する場合にのみ有効です。 この値は [WGS84](http://gisgeography.com/wgs84-world-geodetic-system/) データムに準拠し、メートル単位で測定されます。 `elevation` との組み合わせで、このプロパティを使用して、位置の 3 次元の境界ボックスを表すことができます。 |
