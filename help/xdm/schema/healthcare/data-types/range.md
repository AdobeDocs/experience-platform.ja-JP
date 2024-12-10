---
title: 範囲データタイプ
description: 範囲エクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: 66f8b574-04d9-435f-8743-4ff89c4c0079
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 8%

---

# [!UICONTROL  範囲 ] データタイプ

[!UICONTROL  範囲 ] は、下限および上限の値によって連結された一連の値を提供する、標準のエクスペリエンスデータモデル（XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![ 範囲データタイプ構造 ](../../../images/healthcare/data-types/range.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL  高 ] | `high` | [[!UICONTROL  単純量 ]](../data-types/simple-quantity.md) | 上限。 |
| [!UICONTROL  低 ] | `low` | [[!UICONTROL  単純量 ]](../data-types/simple-quantity.md) | 下限。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/range.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/range.schema.json)
