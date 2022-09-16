---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；ジオ；ジオシェイプ；データ型；データ型；
solution: Experience Platform
title: ジオシェイプデータタイプ
topic-legacy: overview
description: このドキュメントでは、ジオシェイプ XDM データタイプの概要を説明します。
exl-id: 50b9d783-a555-45eb-b154-7dc71389e224
source-git-commit: dc81da58594fac4ce304f9d030f2106f0c3de271
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 34%

---

# [!UICONTROL ジオシェイプ] データタイプ

[!UICONTROL ジオシェイプ] は、地理的領域の形状を記述する標準の XDM データ型です。 このデータタイプは、に記載されているパブリック仕様に基づいています。 [schema.org](https://schema.org/GeoShape).

<img src="../images/data-types/geo-shape.png" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_schema.box` | の配列 [[!UICONTROL 地域座標]](./geo-coordinates.md) | 2 つの座標で形成された長方形で囲まれた地理的領域を表します。 最初の座標は長方形の下隅、2 番目の座標は上隅です。 |
| `_schema.circle` | の配列 [[!UICONTROL 地域座標]](./geo-coordinates.md) | 地理的座標を中心とした特定の半径を持つ円形の領域を表します。 |
| `_schema.polygon` | [[!UICONTROL ジオサークル]](./geo-circle.md) | 最初と最後の座標が同じである 4 つ以上の座標の系列。 |
| `_schema.description` | 文字列 | シェイプが定義している内容の説明。 |
| `_schema.elevation` | Double | 図形の特定または最小の標高。この値は [WGS84](https://gisgeography.com/wgs84-world-geodetic-system/) データムに準拠し、メートル単位で測定されます。`ceiling` との組み合わせで、このプロパティを使用して、位置の 3 次元の境界ボックスを表すことができます。 |
| `_id` | 文字列 | シェイプに対してシステムで生成された一意の識別子。 |
| `ceiling` | ダブル | 図形の最大標高。このプロパティは、 `elevation`. 値は [WGS84](https://gisgeography.com/wgs84-world-geodetic-system/) データムとはメートル単位で測定されます。 `elevation` との組み合わせで、このプロパティを使用して、位置の 3 次元の境界ボックスを表すことができます。 |
