---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；ジオ；座標；データ型；データ型；
solution: Experience Platform
title: ジオコーディネートのデータタイプ
description: 地域座標 XDM データタイプについて説明します。
exl-id: 3c80eb44-852f-4a95-bd13-b6197ffe62da
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 13%

---

# [!UICONTROL 地域座標] データタイプ

[!UICONTROL 地域座標] は、場所の地理的座標を記述する標準の XDM データ型です。 このデータタイプは、に記載されているパブリック仕様に基づいています。 [schema.org](https://schema.org/GeoCoordinates).

<img src="../images/data-types/geo-coordinates.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_schema.description` | 文字列 | 座標が何を識別するかの説明。 |
| `_schema.elevation` | Double | 定義された座標の特定の標高。 値は、 [WGS84](https://gisgeography.com/wgs84-world-geodetic-system/) データムとはメートル単位で測定されます。 |
| `_schema.latitude` | Double | 地理的ポイントの符号付き垂直座標。 |
| `_schema.longitude` | Double | 地理的ポイントの符号付き水平座標。 |
| `_id` | 文字列 | システムで生成された、座標の一意の ID。 |
