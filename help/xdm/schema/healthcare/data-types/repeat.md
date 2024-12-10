---
title: 繰り返しデータタイプ
description: 繰り返しエクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: 9d40bc1d-33d1-4c33-a143-13fdcf8dc255
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 7%

---

# [!UICONTROL  繰り返し ] データタイプ

[!UICONTROL  繰り返し ] は、イベントがスケジュールされるタイミングを説明する一連のルールを提供する、標準の Experience Data Model （XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![ 繰り返しデータタイプ構造 ](../../../images/healthcare/data-types/reference.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL  連結期間 ] | `boundsPeriod` | [[!UICONTROL  期間 ]](../data-types/period.md) | 開始時刻と終了時刻。 |
| [!UICONTROL  境界範囲 ] | `boundsRange` | [[!UICONTROL 範囲]](../data-types/range.md) | 範囲の制限。 |
| [!UICONTROL  バインドされた期間 ] | `boundsDuration` | [[!UICONTROL 期間]](../data-types/duration.md) | 期間の制限。 |
| [!UICONTROL Count] | `count` | 整数 | 繰り返す回数（最小値は `0`）。 |
| [!UICONTROL  最大数 ] | `countMax` | 整数 | 繰り返す最大回数（最小値は `0`）。 |
| [!UICONTROL  曜日 ] | `dayOfWeek` | 文字列の配列 | 使用可能な日の詳細を示す文字列の配列。 このプロパティの値は、次の既知の列挙値の 1 つ以上に等しい必要があります。 <li> `mon` </li> <li> `tues` </li> <li> `wed` </li> <li> `thurs`</li>  <li> `fri` </li> <li> `sat`</li> <li> `sun`</li> |
| [!UICONTROL 期間] | `duration` | Double | 時間の長さ。 |
| [!UICONTROL  最大継続時間 ] | `durationMax` | Double | 最大時間。 |
| [!UICONTROL  期間単位 ] | `durationUnit` | 文字列 | 期間の単位。 このプロパティの値は、次の既知の列挙値の 1 つ以上に等しい必要があります。 <li> `s` （秒） </li> <li> `min` （分） </li> <li> `h` （毎時） </li> <li> `d` （毎日） </li>  <li> `wk` （毎週） </li> <li> `mo` （毎月） </li> <li> `a` （年間）</li> |
| [!UICONTROL 頻度] | `frequency` | Double | 期間内に発生する必要がある繰り返し数（最小値は `0`）。 |
| [!UICONTROL  最大周波数 ] | `frequencyMax` | Double | 期間で発生する必要がある繰り返しの最大数（最小値は `0`）。 |
| [!UICONTROL  オフセット ] | `offset` | 整数 | イベントまでの分（前後）。 |
| [!UICONTROL  期間 ] | `period` | Double | 頻度が適用される期間。 |
| [!UICONTROL  最長期間等 ] | `periodMax` | Double | 期間の上限。 |
| [!UICONTROL  期間単位 ] | `periodUnit` | 文字列 | 時間の単位。 このプロパティの値は、次の既知の列挙値の 1 つ以上に等しい必要があります。 <li> `s` （秒） </li> <li> `min` （分） </li> <li> `h` （毎時） </li> <li> `d` （毎日） </li>  <li> `wk` （毎週） </li> <li> `mo` （毎月） </li> <li> `a` （年間）</li> |
| [!UICONTROL  時刻 ] | `timeOfDay` | 文字列の配列 | アクションが発生する時刻。 |
| [!UICONTROL When] | `when` | 文字列の配列 | アクションの期間のコード。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/repeat.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/repeat.schema.json)
