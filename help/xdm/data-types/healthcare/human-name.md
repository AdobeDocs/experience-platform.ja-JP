---
title: 人名データタイプ
description: 人名エクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
source-git-commit: 7f7de89a843d867d18ef84051eee69face0570a0
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 6%

---

# [!UICONTROL  人間の名前 ] データタイプ

[!UICONTROL Human Name] は、人間またはその他の生体情報の名前に関する情報を提供する、標準の Experience Data Model （XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![ 人名データタイプ構造 ](../../images/data-types/healthcare/human-name.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL  期間 ] | `period` | [[!UICONTROL  期間 ]](../healthcare/period.md) | 名前がまたは使用中だった期間。 |
| [!UICONTROL  ファミリ ] | `family` | 文字列 | 家族または姓。 |
| [!UICONTROL  指定 ] | `given` | 文字列の配列 | 指定した名前（ミドルネームを含む）。 |
| [!UICONTROL  プレフィックス ] | `prefix` | 文字列の配列 | 指定された名前または名の前の名前の部分。 |
| [!UICONTROL  接尾辞 ] | `suffix` | 文字列の配列 | 姓や姓の後の名前の部分。 |
| [!UICONTROL テキスト] | `text` | 文字列 | フルネームのプレーンテキスト表現。 |
| [!UICONTROL  用途 ] | `use` | 文字列 | 名前の使用。 このプロパティの値は、次の既知の列挙値の 1 つ以上に等しい必要があります。 <li> `usual` </li> <li> `offical` </li> <li> `temp` </li> <li> `nickname` </li> <li> `anonymous` </li> <li> `old` </li> <li> `maiden` </li> |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/humanname.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/humanname.schema.json)
