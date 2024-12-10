---
title: 数量データ タイプ
description: 数量エクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: 881fe8a4-0253-4b75-9833-b97bb50cc87e
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 11%

---

# [!UICONTROL  数量 ] データタイプ

[!UICONTROL  数量 ] は、測定された量または測定可能な量を提供する標準の Experience Data Model （XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![ 数量データタイプの構造 ](../../../images/healthcare/data-types/quantity.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL コード] | `code` | 文字列 | 単位のコード化された形式。 |
| [!UICONTROL  コンパレータ ] | `comparator` | 文字列 | 比較演算子。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `<` </li> <li> `<=` </li> <li> `>=` </li> <li> `>`</li> <li> `ad`</li> |
| [!UICONTROL  システム ] | `system` | 文字列 | コード化された単位形式を定義するシステム。URI で表されます。 |
| [!UICONTROL  単位 ] | `unit` | 文字列 | 単位表現。 |
| [!UICONTROL 値] | `value` | Double | 数値。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/quantity.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/quantity.schema.json)
