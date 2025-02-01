---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；場所コンテキスト；placeContext；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: コンテキスト データ タイプを配置
description: 場所コンテキスト XDM データタイプについて説明します。
exl-id: d7cf7366-0136-49ee-84d2-ec663db66eb4
source-git-commit: 1d1224b263b55b290d2cac9c07dfd1b852c4cef5
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 4%

---

# [!UICONTROL  場所のコンテキスト ] データタイプ

[!UICONTROL  場所コンテキスト ] は、目標地点の情報や地理的座標など、観測されたイベントの場所を記述する標準の XDM データタイプです。

![](../images/data-types/place-context.png){width=500}

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `POIinteraction` | [[!UICONTROL  目標点インタラクション ]](./poi-interaction.md) | 目標点（POI）インタラクションの詳細を説明します。 |
| `activePOIs` | [[!UICONTROL POI の詳細 ]](./poi-details.md) の配列 | イベントの原因となった POI について説明します。 |
| `geo` | [[!UICONTROL  地域 ]](./geo.md) | エクスペリエンスが提供された地理的な場所を表します。 |
| `localTime` | 日時 | [RFC 3339](https://tools.ietf.org/html/rfc3339) 形式のタイムスタンプで、指定されたタイムゾーンオフセットでを使用するローカル時間を示します。 書式設定パターンは `yyyy-MM-dd'T'HH:mm:ssXXX` です（例：`2001-07-04T12:08:56-07:00`）。 |
| `localTimezoneOffset` | 整数 | `localTime` 値に対する、UTC からの現在のローカルタイムゾーンオフセット （分）。 該当する場合は、現在の DST オフセットも含まれます。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/placecontext.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/placecontext.schema.json)
