---
title: 参照データタイプ
description: 参照エクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
source-git-commit: 7f7de89a843d867d18ef84051eee69face0570a0
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 11%

---

# [!UICONTROL  参照 ] データタイプ

[!UICONTROL  参照 ] は、標準の Experience Data Model （XDM）データタイプで、あるリソースから別のリソースへの参照を提供します。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![ 参照データタイプ構造 ](../../images/data-types/healthcare/reference.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL 識別子] | `identifier` | [[!UICONTROL 識別子]](../healthcare/identifier.md) | リテラル参照が不明な場合の論理参照。 |
| [!UICONTROL  表示 ] | `display` | 文字列 | 参照の代替テキスト。 |
| [!UICONTROL  参考 ] | `reference` | 文字列 | リテラル参照、相対 URL、内部 URL、絶対 URL。 |
| [!UICONTROL タイプ] | `type` | 文字列 | 参照が参照する型で、URI として表されます。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/reference.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/reference.schema.json)
