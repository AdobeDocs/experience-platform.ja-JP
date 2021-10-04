---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；poi；インタラクション；目標地点；データ型；データ型；
solution: Experience Platform
title: POI（目標地点）インタラクションデータタイプ
topic-legacy: overview
description: このドキュメントでは、目標地点インタラクションの XDM データタイプの概要を説明します。
exl-id: 398f56d9-1802-458d-b565-4096beb5b014
source-git-commit: 12c3f440319046491054b3ef3ec404798bb61f06
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 4%

---

# [!UICONTROL 目標地点のインタラクシ] ョンデータ型

[!UICONTROL 目標地点インタ] ラクションは、モバイルデバイスが範囲内に収まるときに ID 情報をモバイルアプリケーションに通信するワイヤレスデバイスを記述する標準の XDM データ型です。

<img src="../images/data-types/poi-interaction.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `poiDetail` | [[!UICONTROL 目標地点の詳細]](./poi-details.md) | イベントの原因となった POI の詳細を説明します。 |
| `poiEntries` | オブジェクト | 個人が POI に入った回数を示します。 次の 2 つのプロパティが含まれます。 <ul><li>`id`:メジャーの一意の識別子。</li><li>`value`:測定の定量化可能値。</li></ul> |
| `poiExits` | オブジェクト | 個人が POI から退出した回数を示します。 次の 2 つのプロパティが含まれます。 <ul><li>`id`:メジャーの一意の識別子。</li><li>`value`:測定の定量化可能値。</li></ul> |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/poi-interaction.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/poi-interaction.schema.json)
