---
title: 参照データタイプ
description: 参照エクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: eb724dbb-2918-43b5-8e50-c8aabfe6e96c
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 11%

---

# [!UICONTROL &#x200B; 参照 &#x200B;] データタイプ

[!UICONTROL &#x200B; 参照 &#x200B;] は、標準の Experience Data Model （XDM）データタイプで、あるリソースから別のリソースへの参照を提供します。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![&#x200B; 参照データタイプ構造 &#x200B;](../../../images/healthcare/data-types/reference.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL 識別子] | `identifier` | [[!UICONTROL 識別子]](../data-types/identifier.md) | リテラル参照が不明な場合の論理参照。 |
| [!UICONTROL &#x200B; 表示 &#x200B;] | `display` | 文字列 | 参照の代替テキスト。 |
| [!UICONTROL &#x200B; 参考 &#x200B;] | `reference` | 文字列 | リテラル参照、相対 URL、内部 URL、絶対 URL。 |
| [!UICONTROL タイプ] | `type` | 文字列 | 参照が参照する型で、URI として表されます。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/reference.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/reference.schema.json)
