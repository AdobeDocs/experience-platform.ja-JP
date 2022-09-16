---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；poi；インタラクション；目標点；データ型；データ型；
solution: Experience Platform
title: 目標点インタラクションのデータタイプ
topic-legacy: overview
description: このドキュメントでは、目標地点インタラクションの XDM データタイプの概要を説明します。
exl-id: 398f56d9-1802-458d-b565-4096beb5b014
source-git-commit: 12c3f440319046491054b3ef3ec404798bb61f06
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 4%

---

# [!UICONTROL 目標点インタラクション] データタイプ

[!UICONTROL 目標点インタラクション] は、モバイルデバイスが範囲内に入るたびにモバイルアプリケーションに ID 情報を通信するワイヤレスデバイスを表す標準 XDM データ型です。

<img src="../images/data-types/poi-interaction.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `poiDetail` | [[!UICONTROL 目標地点の詳細]](./poi-details.md) | イベントの原因となった POI の詳細を説明します。 |
| `poiEntries` | オブジェクト | 個人が POI に入った回数を表します。 次の 2 つのプロパティが含まれます。 <ul><li>`id`:測定の一意の ID。</li><li>`value`:測定の定量化可能値。</li></ul> |
| `poiExits` | オブジェクト | 個人が POI から出た回数を表します。 次の 2 つのプロパティが含まれます。 <ul><li>`id`:測定の一意の ID。</li><li>`value`:測定の定量化可能値。</li></ul> |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/poi-interaction.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/poi-interaction.schema.json)
