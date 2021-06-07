---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；デバイス；データ型；データ型；
solution: Experience Platform
title: マーケティングデータタイプ
topic-legacy: overview
description: このドキュメントでは、マーケティングXDMデータタイプの概要を説明します。
source-git-commit: cb4afb0979bd65a9a82a6018323fa7beacdbf605
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 7%

---


#  マーケティングデータタイプ

 マーケティングは、特定のタッチポイントでアクティブなマーケティングアクティビティを記述する標準のXDMデータ型です。

![](../images/data-types/marketing.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `campaignGroup` | 文字列 | キャンペーングループの名前（複数のキャンペーンが`50%_DISCOUNT`のようにグループ化されている場合）。 |
| `campaignName` | 文字列 | `50%_DISCOUNT_USA`や`50%_DISCOUNT_ASIA`など、マーケティングキャンペーンの名前。 |
| `trackingCode` | 文字列 | イベントが関連付けられているマーケティングキャンペーンを識別するために使用できるトラッキングコード。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/marketing.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/marketing.schema.json)
