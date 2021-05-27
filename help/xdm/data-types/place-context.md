---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；場所コンテキスト；placeContext；データ型；データ型；
solution: Experience Platform
title: コンテキストデータタイプを配置
topic-legacy: overview
description: このドキュメントでは、Place Context XDMデータタイプの概要を説明します。
exl-id: d7cf7366-0136-49ee-84d2-ec663db66eb4
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 7%

---

# [!UICONTROL contextdata型] を配置する

[!UICONTROL 場所コ] ンテキストは、目標地点情報や地理的座標など、観察されたイベントの場所を示す標準のXDMデータ型です。

<img src="../images/data-types/place-context.png" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `POIinteraction` | [[!UICONTROL 目標地点のインタラクション]](./poi-interaction.md) | 目標地点(POI)インタラクションの詳細を説明します。 |
| `activePOIs` | [[!UICONTROL 目標地点の詳細]](./poi-details.md)の配列 | イベントの原因となったPOIを示します。 |
| `geo` | [[!UICONTROL 地域]](./geo.md) | エクスペリエンスが配信された地域を示します。 |
| `localTime` | DateTime | [RFC 3339](https://tools.ietf.org/html/rfc3339)形式のタイムスタンプ。指定されたタイムゾーンオフセットでを使用するローカル時間を示します。 フォーマットパターンは`yyyy-MM-dd'T'HH:mm:ssXXX`です（例：`2001-07-04T12:08:56-07:00`）。 |
| `localTimezoneOffset` | 整数 | `localTime`値に対するUTCからの現在のローカルタイムゾーンのオフセット（分）。 該当する場合は、現在のDSTオフセットを含める必要があります。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/placecontext.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/placecontext.schema.json)
