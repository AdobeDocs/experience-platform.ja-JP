---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM；フィールド；スキーマ；スキーマ；poi；インタラクション；目標点；目標点；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: 目標点インタラクションデータタイプ
description: 目標点インタラクション XDM データタイプについて説明します。
exl-id: 398f56d9-1802-458d-b565-4096beb5b014
source-git-commit: 1d1224b263b55b290d2cac9c07dfd1b852c4cef5
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 3%

---

# [!UICONTROL &#x200B; 目標点インタラクション &#x200B;] データタイプ

[!UICONTROL &#x200B; 目標点インタラクション &#x200B;] は、モバイルデバイスが範囲内に入るとモバイルアプリケーションに ID 情報を通信するワイヤレスデバイスを記述する、標準の XDM データタイプです。

![](../images/data-types/poi-interaction.png){width=400}

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `poiDetail` | [[!UICONTROL POI の詳細 &#x200B;]](./poi-details.md) | イベントの原因となった POI の詳細を表します。 |
| `poiEntries` | オブジェクト | 人物が POI にエントリした回数を表します。 次の 2 つのプロパティが含まれます。 <ul><li>`id`：測定の一意の ID。</li><li>`value`：測定の定量化可能な値。</li></ul> |
| `poiExits` | オブジェクト | 人物が POI から退出した回数を表します。 次の 2 つのプロパティが含まれます。 <ul><li>`id`：測定の一意の ID。</li><li>`value`：測定の定量化可能な値。</li></ul> |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/poi-interaction.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/poi-interaction.schema.json)
