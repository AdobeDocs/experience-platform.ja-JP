---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；図形；ジオシェイプ；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: ジオシェイプのデータタイプ
description: ジオシェイプ XDM データタイプについて説明します。
exl-id: 50b9d783-a555-45eb-b154-7dc71389e224
source-git-commit: 1d1224b263b55b290d2cac9c07dfd1b852c4cef5
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 31%

---

# [!UICONTROL &#x200B; ジオシェイプ &#x200B;] データタイプ

[!UICONTROL &#x200B; ジオシェイプ &#x200B;] は、地理的領域の形状を記述する標準 XDM データタイプです。 このデータタイプは、[schema.org](https://schema.org/GeoShape) に記載されている公開仕様に基づいています。

![](../images/data-types/geo-shape.png){width=500}

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_schema.box` | [[!UICONTROL &#x200B; 地理座標 &#x200B;]](./geo-coordinates.md) の配列 | 2 つの座標で形成される長方形で囲まれた地理的領域を表します。 最初の座標は長方形の下隅で、2 番目の座標は上隅です。 |
| `_schema.circle` | [[!UICONTROL &#x200B; 地理座標 &#x200B;]](./geo-coordinates.md) の配列 | 地理的座標を中心とした特定の半径を持つ円形領域を表します。 |
| `_schema.polygon` | [[!UICONTROL &#x200B; ジオサークル &#x200B;]](./geo-circle.md) | 最初と最後の座標が同一である、一連の 4 つ以上の座標。 |
| `_schema.description` | 文字列 | シェイプが何を定義しているかの説明。 |
| `_schema.elevation` | Double | 図形の特定または最小の標高。この値は [WGS84](https://gisgeography.com/wgs84-world-geodetic-system/) データムに準拠し、メートル単位で測定されます。`ceiling` との組み合わせで、このプロパティを使用して、位置の 3 次元の境界ボックスを表すことができます。 |
| `_id` | 文字列 | システムが生成した図形の一意の ID です。 |
| `ceiling` | Double | 図形の最大標高。このプロパティは、`elevation` と組み合わせて使用する場合にのみ有効です。 この値は、[WGS84](https://gisgeography.com/wgs84-world-geodetic-system/) データムに準拠し、メートル単位で測定されます。 `elevation` との組み合わせで、このプロパティを使用して、位置の 3 次元の境界ボックスを表すことができます。 |
