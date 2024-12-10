---
title: 単純数量データ タイプ
description: 単純数量エクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: 92d3d6a8-1d0f-43a4-a93f-8df79605c4e6
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '115'
ht-degree: 12%

---

# [!UICONTROL  単純量 ] データタイプ

[!UICONTROL  単純数量 ] は、測定または測定可能な量を提供する標準の Experience Data Model （XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![ 単純量データ型の構造 ](../../../images/healthcare/data-types/simple-quantity.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL コード] | `code` | 文字列 | 単位のコード化された形式。 |
| [!UICONTROL  システム ] | `system` | 文字列 | コード化された単位形式を定義するシステム。URI で表されます。 |
| [!UICONTROL  単位 ] | `unit` | 文字列 | 単位の表現。 |
| [!UICONTROL 値] | `value` | Double | 数値。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/simplequantity.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/simplequantity.schema.json)
