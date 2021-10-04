---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；地域；座標；データ型；データ型；
solution: Experience Platform
title: 地理座標データタイプ
topic-legacy: overview
description: このドキュメントでは、地理座標 XDM データタイプの概要を説明します。
exl-id: 3c80eb44-852f-4a95-bd13-b6197ffe62da
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '131'
ht-degree: 15%

---

# [!UICONTROL 地域コーデ] ィネータデータタイプ

[!UICONTROL 地域] 座標は、ある場所の地理的座標を記述する標準の XDM データ型です。このデータ型は、[schema.org](https://schema.org/GeoCoordinates) に記載されている公開仕様に基づいています。

<img src="../images/data-types/geo-coordinates.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_schema.description` | 文字列 | 座標が何を識別するかの説明。 |
| `_schema.elevation` | Double | 定義した座標の特定の標高。 この値は [WGS84](http://gisgeography.com/wgs84-world-geodetic-system/) データムに準拠し、メートル単位で測定されます。 |
| `_schema.latitude` | ダブル | 地理的ポイントの符号付き垂直座標。 |
| `_schema.longitude` | ダブル | 地理的ポイントの符号付き水平座標。 |
| `_id` | 文字列 | システムで生成された、座標の一意の ID。 |
