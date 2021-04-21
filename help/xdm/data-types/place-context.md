---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ；配置コンテキスト；配置コンテキスト；データ型；データ型；
solution: Experience Platform
title: コンテキストデータタイプを配置
topic-legacy: overview
description: このドキュメントでは、Place Context XDMデータタイプの概要を説明します。
exl-id: d7cf7366-0136-49ee-84d2-ec663db66eb4
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 6%

---

# [!UICONTROL Place ] contextdata type

[!UICONTROL 「配置] コンテキスト」は、目標地点情報や地理的座標など、観測されたイベントの場所を示す標準のXDMデータ型です。

<img src="../images/data-types/place-context.png" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `POIinteraction` | [[!UICONTROL 目標地点の相互作用]](./poi-interaction.md) | 目標地点(POI)の操作の詳細を説明します。 |
| `activePOIs` | [[!UICONTROL 目標地点の詳細]](./poi-details.md)の配列 | イベントの原因となったPOIについて説明します。 |
| `geo` | [[!UICONTROL 地域]](./geo.md) | エクスペリエンスが配信された地域を示します。 |
| `localTime` | DateTime | [RFC 3339](https://tools.ietf.org/html/rfc3339)形式のタイムスタンプ。指定されたタイムゾーンオフセットを使用するローカル時間を示します。 形式設定パターンは`yyyy-MM-dd'T'HH:mm:ssXXX`です（例：`2001-07-04T12:08:56-07:00`）。 |
| `localTimezoneOffset` | 整数 | `localTime`値に対する現在のローカルタイムゾーンのオフセット（UTCからの分）です。 該当する場合は、現在のDSTオフセットを含める必要があります。 |

Mixinの詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/placecontext.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/placecontext.schema.json)
