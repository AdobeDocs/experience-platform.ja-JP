---
title: 比率データタイプ
description: 比率エクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: 8b530af6-0e64-4c30-a7d7-eb221b0b6181
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 6%

---

# [!UICONTROL &#x200B; 比率 &#x200B;] データタイプ

[!UICONTROL &#x200B; 比率 &#x200B;] は、分子と分母を介して 2 つの [[!UICONTROL &#x200B; 数量 &#x200B;]](../data-types/quantity.md) 値の比率を提供する、標準のエクスペリエンスデータモデル（XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![ 比率データタイプ構造 ](../../../images/healthcare/data-types/ratio.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 分母 &#x200B;] | `denominator` | [[!UICONTROL &#x200B; 単純量 &#x200B;]](../data-types/simple-quantity.md) | 分母の値。 |
| [!UICONTROL &#x200B; 分子 &#x200B;] | `numerator` | [[!UICONTROL &#x200B; 数量 &#x200B;]](../data-types/quantity.md) | 分子の値。 |

>[!NOTE]
>
> `denominator` と `numerator` は、HL7 FHIR リリース 5 に従って作成された仕様により、異なるデータタイプを持っています。

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/ratio.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/ratio.schema.json)
