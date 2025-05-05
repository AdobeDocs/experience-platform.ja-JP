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

# [!UICONTROL &#x200B; 繰り返し &#x200B;] データタイプ

[!UICONTROL &#x200B; 繰り返し &#x200B;] は、イベントがスケジュールされるタイミングを説明する一連のルールを提供する、標準の Experience Data Model （XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![ 繰り返しデータタイプ構造 ](../../../images/healthcare/data-types/reference.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 連結期間 &#x200B;] | `boundsPeriod` | [[!UICONTROL &#x200B; 期間 &#x200B;]](../data-types/period.md) | 開始時刻と終了時刻。 |
| [!UICONTROL &#x200B; 境界範囲 &#x200B;] | `boundsRange` | [[!UICONTROL 範囲]](../data-types/range.md) | 範囲の制限。 |
| [!UICONTROL &#x200B; バインドされた期間 &#x200B;] | `boundsDuration` | [[!UICONTROL 期間]](../data-types/duration.md) | 期間の制限。 |
| [!UICONTROL Count] | `count` | 整数 | 繰り返す回数（最小値は `0`）。 |
| [!UICONTROL &#x200B; 最大数 &#x200B;] | `countMax` | 整数 | 繰り返す最大回数（最小値は `0`）。 |
| [!UICONTROL &#x200B; 曜日 &#x200B;] | `dayOfWeek` | 文字列の配列 | 使用可能な日の詳細を示す文字列の配列。 このプロパティの値は、次の既知の列挙値の 1 つ以上に等しい必要があります。 <li> `mon` </li> <li> `tues` </li> <li> `wed` </li> <li> `thurs`</li>  <li> `fri` </li> <li> `sat`</li> <li> `sun`</li> |
| [!UICONTROL 期間] | `duration` | Double | 時間の長さ。 |
| [!UICONTROL &#x200B; 最大継続時間 &#x200B;] | `durationMax` | Double | 最大時間。 |
| [!UICONTROL &#x200B; 期間単位 &#x200B;] | `durationUnit` | 文字列 | 期間の単位。 このプロパティの値は、次の既知の列挙値の 1 つ以上に等しい必要があります。 <li> `s` （秒） </li> <li> `min` （分） </li> <li> `h` （毎時） </li> <li> `d` （毎日） </li>  <li> `wk` （毎週） </li> <li> `mo` （毎月） </li> <li> `a` （年間）</li> |
| [!UICONTROL 頻度] | `frequency` | Double | 期間内に発生する必要がある繰り返し数（最小値は `0`）。 |
| [!UICONTROL &#x200B; 最大周波数 &#x200B;] | `frequencyMax` | Double | 期間で発生する必要がある繰り返しの最大数（最小値は `0`）。 |
| [!UICONTROL &#x200B; オフセット &#x200B;] | `offset` | 整数 | イベントまでの分（前後）。 |
| [!UICONTROL &#x200B; 期間 &#x200B;] | `period` | Double | 頻度が適用される期間。 |
| [!UICONTROL &#x200B; 最長期間等 &#x200B;] | `periodMax` | Double | 期間の上限。 |
| [!UICONTROL &#x200B; 期間単位 &#x200B;] | `periodUnit` | 文字列 | 時間の単位。 このプロパティの値は、次の既知の列挙値の 1 つ以上に等しい必要があります。 <li> `s` （秒） </li> <li> `min` （分） </li> <li> `h` （毎時） </li> <li> `d` （毎日） </li>  <li> `wk` （毎週） </li> <li> `mo` （毎月） </li> <li> `a` （年間）</li> |
| [!UICONTROL &#x200B; 時刻 &#x200B;] | `timeOfDay` | 文字列の配列 | アクションが発生する時刻。 |
| [!UICONTROL When] | `when` | 文字列の配列 | アクションの期間のコード。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/repeat.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/repeat.schema.json)
