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

# [!UICONTROL &#x200B; 範囲 &#x200B;] データタイプ

[!UICONTROL &#x200B; 範囲 &#x200B;] は、下限および上限の値によって連結された一連の値を提供する、標準のエクスペリエンスデータモデル（XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![&#x200B; 範囲データタイプ構造 &#x200B;](../../../images/healthcare/data-types/range.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 高 &#x200B;] | `high` | [[!UICONTROL &#x200B; 単純量 &#x200B;]](../data-types/simple-quantity.md) | 上限。 |
| [!UICONTROL &#x200B; 低 &#x200B;] | `low` | [[!UICONTROL &#x200B; 単純量 &#x200B;]](../data-types/simple-quantity.md) | 下限。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/range.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/range.schema.json)
