---
title: 可用性データ タイプ
description: 可用性エクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: 18c0b767-adf0-480e-9cf2-63e21d05b362
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 9%

---

# [!UICONTROL &#x200B; 可用性 &#x200B;] データタイプ

[!UICONTROL &#x200B; 可用性 &#x200B;] は、項目の可用性データを記述する標準のエクスペリエンスデータモデル（XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![ 可用性データタイプ構造 ](../../../images/healthcare/data-types/availability/availability.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 利用可能時間 &#x200B;] | `availableTime` | オブジェクトの配列 | 項目が使用可能な時間。 詳しくは、[ 以下の節 ](#available-time) を参照してください。 |
| [!UICONTROL &#x200B; 利用不可の時間 &#x200B;] | `notAvailableTime` | 文字列 | 指定された理由で項目が使用できない時間。 詳しくは、[ 以下の節 ](#not-available-time) を参照してください。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/availability.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/availability.schema.json)

## `availableTime` {#available-time}

`availableTime` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ 使用可能な時間構造 ](../../../images/healthcare/data-types/availability/available-time.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 終日 &#x200B;] | `allDay` | ブール値 | 項目が常に使用可能かどうかを示すブール値。 |
| [!UICONTROL &#x200B; 利用可能な終了時刻 &#x200B;] | `availableEndTime` | 文字列 | 項目が使用可能でなくなる時間です。 `allDay` が `true` の場合、これは無視されます。 |
| [!UICONTROL &#x200B; 利用可能な開始時間 &#x200B;] | `availableStartTime` | 文字列 | 項目が使用可能になり始める時間です。 `allDay` が `true` の場合、これは無視されます。 |
| [!UICONTROL &#x200B; 曜日 &#x200B;] | `daysOfWeek` | 文字列の配列 | 使用可能な日の詳細を示す文字列の配列。 このプロパティの値は、次の既知の列挙値の 1 つ以上に等しい必要があります。 <li> `mon` </li> <li> `tues` </li> <li> `wed` </li> <li> `thurs`</li>  <li> `fri` </li> <li> `sat`</li> <li> `sun`</li> |

## `notAvailableTime` {#not-available-time}

`notAvailableTime` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ 使用できない時間構造 ](../../../images/healthcare/data-types/availability/not-available-time.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL During] | `during` | [[!UICONTROL &#x200B; 期間 &#x200B;]](../data-types/period.md) | 項目が使用可能でなくなる期間。 |
| [!UICONTROL 説明] | `description` | 文字列 | 項目が使用できない理由。 |
