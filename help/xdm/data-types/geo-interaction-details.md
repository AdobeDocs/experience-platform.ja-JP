---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；ビーコン；インタラクションの詳細；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: ジオインタラクションの詳細データタイプ
description: ジオインタラクションの詳細 XDM データタイプについて説明します。
exl-id: c05b098b-3f12-4283-a6d5-5ebf96b9828d
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 13%

---

# [!UICONTROL  ジオインタラクションの詳細 ] データタイプ

[!UICONTROL  ジオインタラクションの詳細 ] は、地理的に定義された領域に含まれる現在の状態を記述する標準の XDM データタイプです。

<img src="../images/data-types/geo-interaction-details.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `geoShape` | [[!UICONTROL  ジオシェイプ ]](./geo-shape.md) | 操作される領域のジオシェイプを表します。 このフィールドは、ボックス、円、またはポリゴンを記述できます。 |
| `deviceGeoAccuracy` | Double | ジオ測定デバイスまたはメカニズムの精度 (メートル単位で測定)。 |
| `distanceToCenter` | Double | ジオサークルの場合のジオの中心までの距離（メートル単位で測定）。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/geo-interaction-details.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/geo-interaction-details.schema.json)
