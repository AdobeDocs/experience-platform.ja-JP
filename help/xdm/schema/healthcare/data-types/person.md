---
title: 人物データタイプ
description: ユーザーエクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: a19823f2-25d0-45cb-86f4-7816041b27f9
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '392'
ht-degree: 8%

---

# [!UICONTROL Person] データタイプ

[!UICONTROL &#x200B; 人物 &#x200B;] は、一般的な人物レコードに関する情報を提供する、標準のエクスペリエンスデータモデル（XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![Person データタイプの構造 ](../../../images/healthcare/data-types/person/person.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL アドレス] | `address` | [[!UICONTROL Address]](../data-types/address.md) の配列 | 人物の 1 つ以上のアドレス。 |
| [!UICONTROL &#x200B; 連絡 &#x200B;] | `communication` | オブジェクトの配列 | 健康状態について本人とコミュニケーションをとるために使用される言語。 詳しくは、[ 以下の節 ](#communication) を参照してください。 |
| [!UICONTROL 識別子] | `identifier` | [[!UICONTROL &#x200B; 識別子 &#x200B;]](../data-types/identifier.md) の配列 | この人物の人間 ID。 |
| [!UICONTROL &#x200B; 人物リンクの詳細 &#x200B;] | `link` | オブジェクトの配列 | 同じ人物に関するリソースへのリンク。 詳しくは、[ 以下の節 ](#link) を参照してください。 |
| [!UICONTROL &#x200B; 組織の管理 &#x200B;] | `managingOrganization` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 患者レコードの保管者である組織。 |
| [!UICONTROL &#x200B; 配偶者の有無 &#x200B;] | `maritalStatus` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 人の婚姻（または民事）の地位 |
| [!UICONTROL 名前] | `name` | [[!UICONTROL &#x200B; 人名 &#x200B;]](../data-types/human-name.md) の配列 | 人物に関連付けられた名前。 |
| [!UICONTROL &#x200B; 連絡先詳細 &#x200B;] | `telecom` | [[!UICONTROL &#x200B; 接点 &#x200B;]] の配列 | 担当者に連絡するための連絡先詳細。 |
| [!UICONTROL &#x200B; アクティブ &#x200B;] | `active` | ブール値 | 人物のレコードが有効に使用されているかどうかを示します。 |
| [!UICONTROL &#x200B; 生年月日 &#x200B;] | `birthDate` | 日付 | 人物の生年月日。 |
| [!UICONTROL &#x200B; 死亡インジケーター &#x200B;] | `deceasedBoolean` | ブール値 | 人物が死亡したかどうかを示します。 |
| [!UICONTROL &#x200B; 死亡日時 &#x200B;] | `deceasedDateTime` | 日時 | 死亡時の死亡日時 |
| [!UICONTROL &#x200B; 性別 &#x200B;] | `gender` | 文字列 | 人物の性自認。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `female` </li> <li> `male` </li> <li> `other` </li> <li> `unknown`</li> |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/identifier.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/identifier.schema.json)

## `communication` {#communication}

`communication` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ 通信構造 ](../../../images/healthcare/data-types/person/communication.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL 言語] | `language` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | ユーザーの健康に関するコミュニケーションに使用できる言語。 |
| [!UICONTROL &#x200B; 優先言語 &#x200B;] | `preferred` | ブール値 | 言語が優先言語かどうかを示します。 |

## `link` {#link}

`link` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ リンク構造 ](../../../images/healthcare/data-types/person/link.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL Target] | `target` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | この実際の人物が関連付けられているリソース。 |
| [!UICONTROL Assurance] | `assurance` | 文字列 | リンクに関連付けられた保証のレベル。 このプロパティの値は、次の既知の列挙値の 1 つ以上に等しい必要があります。 <li> `level1` </li> <li> `level2` </li> <li> `level3` </li> <li> `level4` </li> |
