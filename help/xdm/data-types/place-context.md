---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；場所のコンテキスト；placeContext；データ型；データ型；
solution: Experience Platform
title: 場所コンテキストデータタイプ
description: このドキュメントでは、Place Context XDM データタイプの概要を説明します。
exl-id: d7cf7366-0136-49ee-84d2-ec663db66eb4
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 7%

---

# [!UICONTROL 場所のコンテキスト] データタイプ

[!UICONTROL 場所のコンテキスト] は、目標地点情報や地理的座標など、観測されたイベントの場所を示す標準的な XDM データ型です。

<img src="../images/data-types/place-context.png" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `POIinteraction` | [[!UICONTROL 目標点インタラクション]](./poi-interaction.md) | 目標点 (POI) インタラクションに関する詳細を説明します。 |
| `activePOIs` | の配列 [[!UICONTROL 目標地点の詳細]](./poi-details.md) | イベントの原因となった POI を示します。 |
| `geo` | [[!UICONTROL ジオ]](./geo.md) | エクスペリエンスが配信された地域を示します。 |
| `localTime` | 日時 | のタイムスタンプ [RFC 3339](https://tools.ietf.org/html/rfc3339) 形式。指定したタイムゾーンオフセットを使用してローカル時間を示します。 書式設定パターンは次のとおりです。 `yyyy-MM-dd'T'HH:mm:ssXXX` ( 例： `2001-07-04T12:08:56-07:00`) をクリックします。 |
| `localTimezoneOffset` | 整数 | の UTC からの現在のローカルタイムゾーンのオフセット（分） `localTime` の値です。 該当する場合は、現在の DST オフセットを含める必要があります。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/placecontext.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/placecontext.schema.json)
