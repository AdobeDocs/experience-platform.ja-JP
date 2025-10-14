---
title: 住所データ タイプ
description: アドレスエクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: 21213bd5-00f4-43cc-80cf-2c0dcf878a23
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 10%

---

# [!UICONTROL &#x200B; アドレス &#x200B;] データタイプ

[!UICONTROL &#x200B; アドレス &#x200B;] は、標準の Experience Data Model （XDM）データタイプで、（GPS やその他の位置定義形式とは異なり）郵便規則を使用して表現されたアドレスを記述します。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![&#x200B; アドレスタイプ構造 &#x200B;](../../../images/healthcare/data-types/address.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 期間 &#x200B;] | `period` | [[!UICONTROL &#x200B; 期間 &#x200B;]](../data-types/period.md) | アドレスが使用中だった/ある期間。 |
| [!UICONTROL &#x200B; 市区町村 &#x200B;] | `city` | 文字列 | 市区町村の名前。 |
| [!UICONTROL 国] | `country` | 文字列 | ISO 3166 国際規格で説明されている国コード。 コードは alpha-2 または alpha-3 のいずれかです。 |
| [!UICONTROL &#x200B; 地区 &#x200B;] | `district` | 文字列 | 地区の名前。 |
| [!UICONTROL 行] | `line` | 文字列 | 番地、番号、方向、私書箱など。 |
| [!UICONTROL &#x200B; 郵便番号 &#x200B;] | `postalCode` | 文字列 | 郵便番号。 |
| [!UICONTROL &#x200B; 都道府県 &#x200B;] | `state` | 文字列 | 国の小単位。 略語は使用できます。 |
| [!UICONTROL テキスト] | `text` | 文字列 | アドレスのテキスト表現。 |
| [!UICONTROL タイプ] | `type` | 文字列 | アドレスタイプ。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `postal` </li> <li> `physical` </li> <li> `both` </li> |
| [!UICONTROL &#x200B; 用途 &#x200B;] | `use` | 文字列 | 住所の目的。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `home` </li> <li> `work` </li> <li> `temp` </li> <li> `old`</li> <li> `billing`</li> |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/address.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/address.schema.json)
