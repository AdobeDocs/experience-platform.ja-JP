---
title: コード化可能な概念データタイプ
description: コード化可能な概念エクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: c172a7cd-24c6-484b-8552-8745dfd3a8e9
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '103'
ht-degree: 9%

---

# [!UICONTROL &#x200B; コード化可能な概念 &#x200B;] データタイプ

[!UICONTROL &#x200B; コード化可能な概念 &#x200B;] は、あるリソースから別のリソースへの参照を記述する標準の Experience Data Model （XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![&#x200B; コード化可能な概念データタイプ構造 &#x200B;](../../../images/healthcare/data-types/codeable-concept.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; コーディング &#x200B;] | `coding` | [[!UICONTROL &#x200B; コーディング &#x200B;]](../data-types/coding.md) の配列 | 用語システムによって定義されるコード。 |
| [!UICONTROL テキスト] | `text` | 文字列 | 概念のプレーンテキスト表現。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/codeablereference.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/codeableconcept.schema.json)
