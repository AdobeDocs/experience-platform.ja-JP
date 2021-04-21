---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ；ビーコン；インタラクションの詳細；データ型；データ型；
solution: Experience Platform
title: 地域インタラクション詳細データタイプ
topic-legacy: overview
description: このドキュメントでは、地域インタラクション詳細XDMデータタイプの概要を説明します。
exl-id: c05b098b-3f12-4283-a6d5-5ebf96b9828d
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 5%

---

# [!UICONTROL 地域インタラクションの] 詳細データタイプ

[!UICONTROL 地理的な操作の] 詳細は、地理的に定義された領域に現在含まれている状態を示す標準のXDMデータ型です。

<img src="../images/data-types/geo-interaction-details.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `geoShape` | [[!UICONTROL ジオシェイプ]](./geo-shape.md) | 対話操作を行う領域の地域形状を示します。 このフィールドには、ボックス、円または多角形を記述できます。 |
| `deviceGeoAccuracy` | Double | 地域測定デバイスまたはメカニズムの精度（メートル単位）。 |
| `distanceToCenter` | 重複 | 地理円の場合、地理の中心までの距離（メートル単位）。 |

データ型の詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/geo-interaction-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/geo-interaction-details.schema.json)
