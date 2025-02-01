---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；地域；円；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: ジオサークルのデータタイプ
description: ジオサークル XDM データタイプについて説明します。
exl-id: fa041f4f-9955-44e9-b235-a643e07d402c
source-git-commit: 1d1224b263b55b290d2cac9c07dfd1b852c4cef5
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 17%

---

# [!UICONTROL  ジオサークル ] データタイプ

[!UICONTROL  ジオサークル ] は、特定の座標セットを中心とした特定の半径を指定して、円形の地理的領域を表す標準 XDM データタイプです。 このデータタイプは、[schema.org](https://schema.org/GeoCircle) に記載されている公開仕様に基づいています。

![](../images/data-types/geo-circle.png){width=400}

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_schema.coordinates` | [[!UICONTROL  地理座標 ]](./geo-coordinates.md) | 円の中心の地理的座標を表します。 |
| `_schema.description` | 文字列 | サークルに何が含まれているかの説明。 |
| `_schema.radius` | Double | 円の半径の長さ。この値は、[WGS84](https://gisgeography.com/wgs84-world-geodetic-system/) データムに準拠し、メートル単位で測定されます。 |
| `_id` | 文字列 | サークルの一意のシステム生成 ID。 |
