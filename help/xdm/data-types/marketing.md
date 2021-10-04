---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；デバイス；データ型；データ型；
solution: Experience Platform
title: マーケティングデータタイプ
topic-legacy: overview
description: このドキュメントでは、マーケティング XDM データタイプの概要を説明します。
exl-id: b5ac0127-15fe-42b6-b7fc-2fbcda7e7e27
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 7%

---

#  Marketingdata type

 マーケティングは、特定のタッチポイントでアクティブなマーケティングアクティビティを記述する標準の XDM データ型です。

![](../images/data-types/marketing.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `campaignGroup` | 文字列 | キャンペーングループの名前（複数のキャンペーンを `50%_DISCOUNT` のようにグループ化する場合）。 |
| `campaignName` | 文字列 | `50%_DISCOUNT_USA` や `50%_DISCOUNT_ASIA` など、マーケティングキャンペーンの名前。 |
| `trackingCode` | 文字列 | イベントが関連付けられているマーケティングキャンペーンを識別するために使用できるトラッキングコード。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/marketing.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/marketing.schema.json)
