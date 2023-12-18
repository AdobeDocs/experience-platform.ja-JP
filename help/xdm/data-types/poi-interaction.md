---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；poi；インタラクション；目標点；データ型；データ型；
solution: Experience Platform
title: POI（目標点）インタラクションのデータタイプ
description: 目標地点インタラクションの XDM データタイプについて説明します。
exl-id: 398f56d9-1802-458d-b565-4096beb5b014
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 3%

---

# [!UICONTROL 目標点インタラクション] データタイプ

[!UICONTROL 目標点インタラクション] は、モバイルデバイスが範囲内に入るたびにモバイルアプリケーションに ID 情報を通信するワイヤレスデバイスを表す標準的な XDM データ型です。

<img src="../images/data-types/poi-interaction.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `poiDetail` | [[!UICONTROL 目標地点の詳細]](./poi-details.md) | イベントの原因となった POI の詳細を説明します。 |
| `poiEntries` | オブジェクト | 個人が POI に入った回数を表します。 次の 2 つのプロパティが含まれます。 <ul><li>`id`：測定の一意の ID。</li><li>`value`：測定の定量化可能な値。</li></ul> |
| `poiExits` | オブジェクト | 個人が POI から出た回数を表します。 次の 2 つのプロパティが含まれます。 <ul><li>`id`：測定の一意の ID。</li><li>`value`：測定の定量化可能な値。</li></ul> |

{style="table-layout:auto"}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/poi-interaction.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/poi-interaction.schema.json)
