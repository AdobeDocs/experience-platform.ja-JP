---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；ビーコン；インタラクションの詳細；データ型；データ型；
solution: Experience Platform
title: ジオインタラクションの詳細データタイプ
topic-legacy: overview
description: このドキュメントでは、ジオインタラクションの詳細の XDM データタイプの概要を説明します。
exl-id: c05b098b-3f12-4283-a6d5-5ebf96b9828d
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 6%

---

# [!UICONTROL ジオインタラクションの詳細] データタイプ

[!UICONTROL ジオインタラクションの詳細] は、地理的に定義された領域に含まれる現在の状態を記述する標準の XDM データ型です。

<img src="../images/data-types/geo-interaction-details.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `geoShape` | [[!UICONTROL ジオシェイプ]](./geo-shape.md) | やり取りされる領域のジオシェイプを表します。 このフィールドは、ボックス、円、または多角形を表すことができます。 |
| `deviceGeoAccuracy` | Double | 地域測定デバイスまたはメカニズムの精度（メートル単位で測定）。 |
| `distanceToCenter` | ダブル | ジオサークルの場合のジオの中心までの距離（メートル単位で測定）。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/geo-interaction-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/geo-interaction-details.schema.json)
