---
title: 連絡先データタイプ
description: 連絡先エクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: bbb9a5e1-b0d5-4c07-93a9-c1573dacad73
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 7%

---

# [!UICONTROL &#x200B; 連絡先 &#x200B;] データタイプ

[!UICONTROL &#x200B; 連絡先 &#x200B;] は、個人の連絡先の詳細を記述する標準の Experience Data Model （XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![ 連絡先データタイプの構造 ](../../../images/healthcare/data-types/contact-point.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 期間 &#x200B;] | `period` | [[!UICONTROL &#x200B; 期間 &#x200B;]](../data-types/period.md) | 連絡先が使用中だった/ある期間。 |
| [!UICONTROL ランク] | `rank` | 整数 | 連絡先の優先使用を示すランク。 最小値は `1`、最大値は `2147483647` です。ここで、`1` は最も特異性が高くなります。 |
| [!UICONTROL &#x200B; システム &#x200B;] | `system` | 文字列 | ユーザーに連絡するためのシステム。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `phone` </li> <li> `fax` </li> <li> `email` </li> <li> `pager`</li> <li> `url`</li> <li> `sms`</li> <li> `other`</li> |
| [!UICONTROL &#x200B; 用途 &#x200B;] | `use` | 文字列 | 連絡先の目的。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `home` </li> <li> `work` </li> <li> `temp` </li> <li> `old`</li> <li> `mobile`</li> |
| [!UICONTROL 値] | `value` | 文字列 | 連絡先の詳細。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/contactpoint.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/contactpoint.schema.json)
