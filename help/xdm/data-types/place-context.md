---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；場所コンテキスト；placeContext；データ型；データ型；
solution: Experience Platform
title: コンテキストデータタイプを配置
topic-legacy: overview
description: このドキュメントでは、Place Context XDM データタイプの概要を説明します。
exl-id: d7cf7366-0136-49ee-84d2-ec663db66eb4
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 7%

---

# [!UICONTROL Place contextdata ] type

[!UICONTROL 場所コ] ンテキストは、目標地点情報や地理的座標など、観測されたイベントの場所を示す標準の XDM データ型です。

<img src="../images/data-types/place-context.png" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `POIinteraction` | [[!UICONTROL 目標地点の相互作用]](./poi-interaction.md) | 目標地点 (POI) インタラクションの詳細について説明します。 |
| `activePOIs` | [[!UICONTROL  目標地点の詳細 ]](./poi-details.md) の配列 | イベントの原因となった POI を示します。 |
| `geo` | [[!UICONTROL 地域]](./geo.md) | エクスペリエンスが配信された地域を示します。 |
| `localTime` | DateTime | [RFC 3339](https://tools.ietf.org/html/rfc3339) 形式のタイムスタンプ。指定されたタイムゾーンオフセットでを使用するローカル時間を示します。 書式設定パターンは `yyyy-MM-dd'T'HH:mm:ssXXX` です（例：`2001-07-04T12:08:56-07:00`）。 |
| `localTimezoneOffset` | 整数 | `localTime` 値に対する UTC からの現在のローカルタイムゾーンのオフセット（分）。 該当する場合は、現在の DST オフセットを含める必要があります。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/placecontext.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/placecontext.schema.json)
