---
keywords: Experience Platform;home;popular topics;schema;Schema;XDM;fields;schemas;Schemas;place context;placeContext;datatype;data-type;data type;
solution: Experience Platform
title: コンテキストデータタイプを配置
topic: overview
description: このドキュメントでは、Place Context XDMデータタイプの概要を説明します。
translation-type: tm+mt
source-git-commit: f5bddb39c16eb25e85297f56e331d3aa51510eb9
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 6%

---


# [!UICONTROL コンテキスト] データ型を配置

[!UICONTROL 「配置コンテキスト] 」は、目標地点情報や地理的座標など、観測されたイベントの場所を示す標準のXDMデータ型です。

<img src="../images/data-types/place-context.png" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `POIinteraction` | [[!UICONTROL 目標地点の相互作用]](./poi-interaction.md) | 目標地点(POI)の操作の詳細を説明します。 |
| `activePOIs` | 目標 [[!UICONTROL 地点の詳細の配列]](./poi-details.md) | イベントの原因となったPOIについて説明します。 |
| `geo` | [[!UICONTROL 地域]](./geo.md) | エクスペリエンスが配信された地域を示します。 |
| `localTime` | DateTime | RFC 3339 [](https://tools.ietf.org/html/rfc3339) 形式のタイムスタンプ。指定されたタイムゾーンオフセットを使用してローカル時間を示します。 形式設定パターン `yyyy-MM-dd'T'HH:mm:ssXXX` は(例えば、 `2001-07-04T12:08:56-07:00`)です。 |
| `localTimezoneOffset` | 整数 | 値に対する現在のローカルタイムゾーンのオフセット（UTCからの分単位） `localTime` です。 該当する場合は、現在のDSTオフセットを含める必要があります。 |

Mixinの詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/placecontext.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/placecontext.schema.json)