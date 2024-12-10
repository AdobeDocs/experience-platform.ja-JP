---
title: 識別子データタイプ
description: 識別子エクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: 0664f52d-bea6-4aa1-b2a5-de0bd6d5edd9
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 9%

---

# [!UICONTROL  識別子 ] データタイプ

[!UICONTROL  識別子 ] は、計算用の識別子を提供する標準の Experience Data Model （XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![ 識別子データタイプ構造 ](../../../images/healthcare/data-types/identifier.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL  期間 ] | `period` | [[!UICONTROL  期間 ]](../data-types/period.md) | ID が使用に対して有効または有効だった期間。 |
| [!UICONTROL タイプ] | `type` | [[!UICONTROL  コード化可能な概念 ]](../data-types/codeable-concept.md) | 識別子の説明。 |
| [!UICONTROL  譲渡人 ] | `assigner` | 文字列 | ID を発行した組織。 |
| [!UICONTROL  システム ] | `system` | 文字列 | 識別子の値の名前空間。URI で表されます。 |
| [!UICONTROL  用途 ] | `use` | 文字列 | 識別子の使用。 このプロパティの値は、次の既知の列挙値の 1 つ以上に等しい必要があります。 <li> `usual` </li> <li> `offical` </li> <li> `temp` </li> <li> `secondary` </li> <li> `old` </li> |
| [!UICONTROL 値] | `value` | 文字列 | ID の一意の値。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/identifier.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/identifier.schema.json)
