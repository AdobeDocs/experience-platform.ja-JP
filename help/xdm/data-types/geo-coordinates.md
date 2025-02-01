---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；地域；座標；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: 地理座標のデータ タイプ
description: 地理座標 XDM データタイプについて説明します。
exl-id: 3c80eb44-852f-4a95-bd13-b6197ffe62da
source-git-commit: 1d1224b263b55b290d2cac9c07dfd1b852c4cef5
workflow-type: tm+mt
source-wordcount: '124'
ht-degree: 12%

---

# [!UICONTROL  地理座標 ] データタイプ

[!UICONTROL  ジオコーディネート ] は、場所の地理的座標を表す標準 XDM データタイプです。 このデータタイプは、[schema.org](https://schema.org/GeoCoordinates) に記載されている公開仕様に基づいています。

![](../images/data-types/geo-coordinates.png){width=400}

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_schema.description` | 文字列 | 座標が何を識別するかの説明。 |
| `_schema.elevation` | Double | 定義された座標の特定の標高。 この値は、[WGS84](https://gisgeography.com/wgs84-world-geodetic-system/) データムに準拠する必要があり、メートル単位で測定されます。 |
| `_schema.latitude` | Double | 地理的なポイントの符号付き垂直座標。 |
| `_schema.longitude` | Double | 地理的なポイントの符号付き水平座標。 |
| `_id` | 文字列 | 座標の一意のシステム生成 ID。 |
