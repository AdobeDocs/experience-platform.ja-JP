---
title: コード化可能な概念データタイプ
description: コード化可能な概念エクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
source-git-commit: 7f7de89a843d867d18ef84051eee69face0570a0
workflow-type: tm+mt
source-wordcount: '103'
ht-degree: 9%

---

# [!UICONTROL  コード化可能な概念 ] データタイプ

[!UICONTROL  コード化可能な概念 ] は、あるリソースから別のリソースへの参照を記述する標準の Experience Data Model （XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![ コード化可能な概念データタイプ構造 ](../../images/data-types/healthcare/codeable-concept.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL  コーディング ] | `coding` | [[!UICONTROL  コーディング ]](../healthcare/coding.md) の配列 | 用語システムによって定義されるコード。 |
| [!UICONTROL テキスト] | `text` | 文字列 | 概念のプレーンテキスト表現。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/codeablereference.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/codeableconcept.schema.json)
