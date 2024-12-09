---
title: 人物データタイプ
description: ユーザーエクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
source-git-commit: 7f7de89a843d867d18ef84051eee69face0570a0
workflow-type: tm+mt
source-wordcount: '392'
ht-degree: 8%

---

# [!UICONTROL Person] データタイプ

[!UICONTROL  人物 ] は、一般的な人物レコードに関する情報を提供する、標準のエクスペリエンスデータモデル（XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![Person データタイプの構造 ](../../images/data-types/healthcare/person/person.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL アドレス] | `address` | [[!UICONTROL Address]](../healthcare/address.md) の配列 | 人物の 1 つ以上のアドレス。 |
| [!UICONTROL  連絡 ] | `communication` | オブジェクトの配列 | 健康状態について本人とコミュニケーションをとるために使用される言語。 詳しくは、[ 以下の節 ](#communication) を参照してください。 |
| [!UICONTROL 識別子] | `identifier` | [[!UICONTROL  識別子 ]](../healthcare/identifier.md) の配列 | この人物の人間 ID。 |
| [!UICONTROL  人物リンクの詳細 ] | `link` | オブジェクトの配列 | 同じ人物に関するリソースへのリンク。 詳しくは、[ 以下の節 ](#link) を参照してください。 |
| [!UICONTROL  組織の管理 ] | `managingOrganization` | [[!UICONTROL  参考 ]](../healthcare/reference.md) | 患者レコードの保管者である組織。 |
| [!UICONTROL  配偶者の有無 ] | `maritalStatus` | [[!UICONTROL  コード化可能な概念 ]](../healthcare/codeable-concept.md) | 人の婚姻（または民事）の地位 |
| [!UICONTROL 名前] | `name` | [[!UICONTROL  人名 ]](../healthcare/human-name.md) の配列 | 人物に関連付けられた名前。 |
| [!UICONTROL  連絡先詳細 ] | `telecom` | [[!UICONTROL  接点 ]] の配列 | 担当者に連絡するための連絡先詳細。 |
| [!UICONTROL  アクティブ ] | `active` | ブール値 | 人物のレコードが有効に使用されているかどうかを示します。 |
| [!UICONTROL  生年月日 ] | `birthDate` | 日付 | 人物の生年月日。 |
| [!UICONTROL  死亡インジケーター ] | `deceasedBoolean` | ブール値 | 人物が死亡したかどうかを示します。 |
| [!UICONTROL  死亡日時 ] | `deceasedDateTime` | 日時 | 死亡時の死亡日時 |
| [!UICONTROL  性別 ] | `gender` | 文字列 | 人物の性自認。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `female` </li> <li> `male` </li> <li> `other` </li> <li> `unknown`</li> |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/identifier.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/identifier.schema.json)

## `communication` {#communication}

`communication` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ 通信構造 ](../../images/data-types/healthcare/person/communication.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL 言語] | `language` | [[!UICONTROL  コード化可能な概念 ]](../../data-types/healthcare/codeable-concept.md) | ユーザーの健康に関するコミュニケーションに使用できる言語。 |
| [!UICONTROL  優先言語 ] | `preferred` | ブール値 | 言語が優先言語かどうかを示します。 |

## `link` {#link}

`link` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ リンク構造 ](../../images/data-types/healthcare/person/link.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL Target] | `target` | [[!UICONTROL  参考 ]](../../data-types/healthcare/reference.md) | この実際の人物が関連付けられているリソース。 |
| [!UICONTROL Assurance] | `assurance` | 文字列 | リンクに関連付けられた保証のレベル。 このプロパティの値は、次の既知の列挙値の 1 つ以上に等しい必要があります。 <li> `level1` </li> <li> `level2` </li> <li> `level3` </li> <li> `level4` </li> |
