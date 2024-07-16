---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM；フィールド；スキーマ；スキーマ；デバイス；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: マーケティングデータタイプ
description: マーケティング XDM データタイプについて説明します。
exl-id: b5ac0127-15fe-42b6-b7fc-2fbcda7e7e27
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '114'
ht-degree: 6%

---

# [!UICONTROL  マーケティング ] データタイプ

[!UICONTROL  マーケティング ] は、特定のタッチポイントでアクティブなマーケティングアクティビティを記述する標準 XDM データタイプです。

![](../images/data-types/marketing.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `campaignGroup` | 文字列 | キャンペーングループの名前（複数のキャンペーンが `50%_DISCOUNT` のようにグループ化されている場合）。 |
| `campaignName` | 文字列 | マーケティングキャンペーンの名前（`50%_DISCOUNT_USA`、`50%_DISCOUNT_ASIA` など）。 |
| `trackingCode` | 文字列 | イベントが関連付けられているマーケティングキャンペーンを識別するために使用できるトラッキングコード。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/marketing.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/marketing.schema.json)
