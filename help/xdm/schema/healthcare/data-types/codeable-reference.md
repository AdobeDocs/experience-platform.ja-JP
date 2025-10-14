---
title: コード化可能な参照データタイプ
description: コード化可能な参照エクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: 5ac4bc82-3c8e-4622-8a4e-c954bd6e6411
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 7%

---

# [!UICONTROL &#x200B; コード化可能な参照 &#x200B;] データタイプ

[!UICONTROL &#x200B; コード化可能な参照 &#x200B;] は、リソースまたは概念への参照を記述する標準の Experience Data Model （XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![&#x200B; コード化可能な参照データタイプ構造 &#x200B;](../../../images/healthcare/data-types/codeable-reference.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 概念 &#x200B;] | `concept` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 概念への参照（クラス）。 |
| [!UICONTROL &#x200B; 参考 &#x200B;] | `reference` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | リソースへの参照。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/codeablereference.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/codeablereference.schema.json)
