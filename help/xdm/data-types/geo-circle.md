---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；ジオ；サークル；データ型；データ型；
solution: Experience Platform
title: ジオサークルのデータタイプ
description: このドキュメントでは、ジオサークル XDM データタイプの概要を説明します。
exl-id: fa041f4f-9955-44e9-b235-a643e07d402c
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '133'
ht-degree: 21%

---

# [!UICONTROL ジオサークル] データタイプ

[!UICONTROL ジオサークル] は、特定の座標セットを中心とする特定の半径を指定し、円形の地理的地域を表す標準的な XDM データ型です。 このデータタイプは、に記載されているパブリック仕様に基づいています。 [schema.org](https://schema.org/GeoCircle).

<img src="../images/data-types/geo-circle.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_schema.coordinates` | [[!UICONTROL 地理座標]](./geo-coordinates.md) | 円の中心の地理的座標を表します。 |
| `_schema.description` | 文字列 | 円に何が含まれているかの説明。 |
| `_schema.radius` | Double | 円の半径の長さ。この値は [WGS84](https://gisgeography.com/wgs84-world-geodetic-system/) データムに準拠し、メートル単位で測定されます。 |
| `_id` | 文字列 | システムで生成された、サークルの一意の ID。 |
