---
title: 比率データタイプ
description: 比率エクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
source-git-commit: 7d33c5474629b2b02c19e752cdf2cb0a2ba19f19
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 6%

---

# [!UICONTROL  比率 ] データタイプ

[!UICONTROL  比率 ] は、分子と分母を介して 2 つの [[!UICONTROL  数量 ]](../healthcare/quantity.md) 値の比率を提供する、標準のエクスペリエンスデータモデル（XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![ 比率データタイプ構造 ](../../images/data-types/healthcare/ratio.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL  分母 ] | `denominator` | [[!UICONTROL  単純量 ]](../healthcare/simple-quantity.md) | 分母の値。 |
| [!UICONTROL  分子 ] | `numerator` | [[!UICONTROL  数量 ]](../healthcare/quantity.md) | 分子の値。 |

>[!NOTE]
>
> `denominator` と `numerator` は、HL7 FHIR リリース 5 に従って作成された仕様により、異なるデータタイプを持っています。

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/ratio.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/ratio.schema.json)
