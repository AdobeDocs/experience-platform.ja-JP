---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；デバイス；データ型；データ型；
solution: Experience Platform
title: マーケティングデータタイプ
description: マーケティング XDM データタイプについて説明します。
exl-id: b5ac0127-15fe-42b6-b7fc-2fbcda7e7e27
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '114'
ht-degree: 6%

---

# [!UICONTROL マーケティング] データタイプ

[!UICONTROL マーケティング] は、特定のタッチポイントでアクティブなマーケティングアクティビティを記述する標準の XDM データ型です。

![](../images/data-types/marketing.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `campaignGroup` | 文字列 | キャンペーングループの名前 ( 複数のキャンペーンが、 `50%_DISCOUNT`) をクリックします。 |
| `campaignName` | 文字列 | マーケティングキャンペーンの名前（例： ） `50%_DISCOUNT_USA` または `50%_DISCOUNT_ASIA`. |
| `trackingCode` | 文字列 | イベントが関連付けられているマーケティングキャンペーンを識別するために使用できるトラッキングコード。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/marketing.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/marketing.schema.json)
