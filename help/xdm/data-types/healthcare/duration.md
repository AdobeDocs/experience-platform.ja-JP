---
title: 期間データタイプ
description: 期間エクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
source-git-commit: 7f7de89a843d867d18ef84051eee69face0570a0
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 9%

---

# [!UICONTROL  期間 ] データタイプ

[!UICONTROL  期間 ] は、時間の長さを記述する標準のエクスペリエンスデータモデル（XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![ 期間データタイプ構造 ](../../images/data-types/healthcare/duration.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL コード] | `code` | 文字列 | 時間単位のコード化された形式。 |
| [!UICONTROL  システム ] | `system` | 文字列 | コード化された単位を記述するシステム。URI で表されます。 |
| [!UICONTROL  単位 ] | `unit` | 文字列 | 時間の単位は、ミリ秒、秒、分、時間、日、週、月、または年単位で表されます。 このプロパティの値は、次の既知の列挙値の 1 つ以上に等しい必要があります。 <li> `ms` （ミリ秒） </li> <li> `s` （秒） </li> <li> `min` （分） </li> <li> `h` （時間） </li>  <li> `d` （日） </li> <li> `wk` （週間） </li> <li> `mo` （月） </li> <li> `a` （年） </li> |
| [!UICONTROL 値] | `value` | Double | 時間の単位を表す数値。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/duration.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/duration.schema.json)
