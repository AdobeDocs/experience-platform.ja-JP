---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；poi；インタラクション；目標地点；データ型；データ型；
solution: Experience Platform
title: POI（目標地点）インタラクションデータタイプ
topic-legacy: overview
description: このドキュメントでは、目標地点インタラクションのXDMデータタイプの概要を説明します。
exl-id: 398f56d9-1802-458d-b565-4096beb5b014
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 4%

---

# [!UICONTROL 目標地点のインタラクシ] ョンデータ型

[!UICONTROL 目標地点インタ] ラクションは、モバイルデバイスが範囲内に収まるときにID情報をモバイルアプリケーションに通信するワイヤレスデバイスを記述する標準のXDMデータ型です。

<img src="../images/data-types/poi-interaction.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `poiDetail` | [[!UICONTROL 目標地点の詳細]](./poi-details.md) | イベントの原因となった目標地点の詳細を説明します。 |
| `poiEntries` | オブジェクト | 人がPOIに入った回数を示します。 次の2つのプロパティが含まれます。 <ul><li>`id`:測定の一意の識別子。</li><li>`value`:測定の定量化可能値。</li></ul> |
| `poiExits` | オブジェクト | 人がPOIから退出した回数を示します。 次の2つのプロパティが含まれます。 <ul><li>`id`:測定の一意の識別子。</li><li>`value`:測定の定量化可能値。</li></ul> |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/poi-interaction.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/poi-interaction.schema.json)
