---
title: 住所データ タイプ
description: アドレスエクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
source-git-commit: 7f7de89a843d867d18ef84051eee69face0570a0
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 10%

---

# [!UICONTROL  アドレス ] データタイプ

[!UICONTROL  アドレス ] は、標準の Experience Data Model （XDM）データタイプで、（GPS やその他の位置定義形式とは異なり）郵便規則を使用して表現されたアドレスを記述します。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![ アドレスタイプ構造 ](../../images/data-types/healthcare/address.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL  期間 ] | `period` | [[!UICONTROL  期間 ]](../healthcare/period.md) | アドレスが使用中だった/ある期間。 |
| [!UICONTROL  市区町村 ] | `city` | 文字列 | 市区町村の名前。 |
| [!UICONTROL 国] | `country` | 文字列 | ISO 3166 国際規格で説明されている国コード。 コードは alpha-2 または alpha-3 のいずれかです。 |
| [!UICONTROL  地区 ] | `district` | 文字列 | 地区の名前。 |
| [!UICONTROL 行] | `line` | 文字列 | 番地、番号、方向、私書箱など。 |
| [!UICONTROL  郵便番号 ] | `postalCode` | 文字列 | 郵便番号。 |
| [!UICONTROL  都道府県 ] | `state` | 文字列 | 国の小単位。 略語は使用できます。 |
| [!UICONTROL テキスト] | `text` | 文字列 | アドレスのテキスト表現。 |
| [!UICONTROL タイプ] | `type` | 文字列 | アドレスタイプ。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `postal` </li> <li> `physical` </li> <li> `both` </li> |
| [!UICONTROL  用途 ] | `use` | 文字列 | 住所の目的。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `home` </li> <li> `work` </li> <li> `temp` </li> <li> `old`</li> <li> `billing`</li> |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/address.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/address.schema.json)
