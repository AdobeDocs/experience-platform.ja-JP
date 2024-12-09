---
title: 患者スキーマフィールドグループ
description: 「患者スキーマ」フィールドグループについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
source-git-commit: e01a6cd4cf42338f87222a5e2871cd6c3683309a
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 8%

---

# [!UICONTROL  患者 ] スキーマフィールドグループ

[!UICONTROL Patient] は、[[!DNL XDM Individual Profile] class](../../classes/individual-profile.md) の標準スキーマフィールドグループです。 ケアやその他の健康関連サービスを受ける個人または動物の人口統計やその他の管理上の詳細をキャプチャする単一のオブジェクトタイプフィールド `healthcarePatient` を提供します。

![ フィールドグループ構造 ](../../images/field-groups/patient.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL アドレス] | `address` | [[!UICONTROL Address]](../../data-types/healthcare/address.md) の配列 | 患者の住所情報。 |
| [!UICONTROL  連絡 ] | `communication` | オブジェクトの配列 | 患者の健康状態について患者とコミュニケーションをとるために使用される言語。 詳しくは、[ 以下の節 ](#communication) を参照してください。 |
| [!UICONTROL  患者との接触 ] | `contact` | オブジェクトの配列 | 患者の連絡パーティ（後見人、パートナー、友人など）。 詳しくは、[ 以下の節 ](#contact) を参照してください。 |
| [!UICONTROL  一般開業医 ] | `generalPractioner` | [[!UICONTROL  参照 ]](../../data-types/healthcare/reference.md) の配列 | 患者の主治医。 |
| [!UICONTROL 識別子] | `identifier` | [[!UICONTROL  識別子 ]](../../data-types/healthcare/identifier.md) の配列 | 患者の識別子。 |
| [!UICONTROL  患者リンクの詳細 ] | `link` | オブジェクトの配列 | 同じ人物に関する患者または関係者のリソースへのリンク。 詳しくは、[ 以下の節 ](#link) を参照してください。 |
| [!UICONTROL  組織の管理 ] | `managingOrganization` | [[!UICONTROL  参考 ]](../../data-types/healthcare/reference.md) | 患者の記録の管理組織。 |
| [!UICONTROL  配偶者の有無 ] | `maritalStatus` | [[!UICONTROL  コード化可能な概念 ]](../../data-types/healthcare/codeable-concept.md) | 患者の婚姻ステータス。 |
| [!UICONTROL 名前] | `name` | [[!UICONTROL  人名 ]](../../data-types/healthcare/human-name.md) の配列 | 患者に関連付けられた名前。 |
| [!UICONTROL  連絡先詳細 ] | `telecom` | [[!UICONTROL  連絡先 ]](../../data-types/healthcare/contact-point.md) の配列 | 連絡先の詳細（電話番号やメールアドレスなど）。この情報を使用して患者に連絡を取ることができます。 |
| [!UICONTROL  アクティブ ] | `active` | ブール値 | 患者のレコードが有効に使用されているかどうかを示します。 |
| [!UICONTROL  生年月日 ] | `birthDate` | 日付 | 患者の生年月日。 |
| [!UICONTROL  死亡インジケーター ] | `deceasedBoolean` | ブール値 | 患者が死亡したかどうかを示します。 |
| [!UICONTROL  死亡日時 ] | `deceasedDateTime` | 日時 | 患者の死亡日時。 |
| [!UICONTROL  性別 ] | `gender` | 文字列 | 人物の性自認。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `female` </li> <li> `male` </li> <li> `other` </li> <li> `unknown`</li> |
| [!UICONTROL  多胎の一部である ] | `multipleBirthBoolean` | ブール値 | 患者が多胎に含まれているかどうかを示します。 |
| [!UICONTROL  生年月日 ] | `multipleBirthInteger` | 整数 | シーケンスの出生番号。 |

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/patient.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/patient.schema.json)

## `communication` {#communication}

`communication` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ 通信構造 ](../../images/field-groups/healthcare-patient/communication.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL 言語] | `language` | [[!UICONTROL  コード化可能な概念 ]](../../data-types/healthcare/codeable-concept.md) | 健康状態について患者とコミュニケーションを取るために使用できる言語。 |
| [!UICONTROL  優先言語 ] | `preferred` | ブール値 | 言語が優先言語かどうかを示します。 |

## `contact` {#contact}

`contact` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ 接触構造 ](../../images/field-groups/healthcare-patient/contact.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL  連絡先 ] | `address` | [[!UICONTROL アドレス]](../../data-types/healthcare/address.md) | 担当者の住所。 |
| [!UICONTROL  連絡先名 ] | `name` | [[!UICONTROL  人間の名前 ]](../../data-types/healthcare/human-name.md) | 連絡担当者の名前。 |
| [!UICONTROL  連絡先の組織 ] | `organization` | [[!UICONTROL  参考 ]](../../data-types/healthcare/reference.md) | 担当者に関連付けられている組織。 |
| [!UICONTROL  連絡期間 ] | `period` | [[!UICONTROL  期間 ]](../../data-types/healthcare/period.md) | 連絡先が使用中またはだった期間。 |
| [!UICONTROL  関係&#39;] | `relationship` | [[!UICONTROL  コード化可能な概念 ]](../../data-types/healthcare/codeable-concept.md) | 患者と連絡担当者の関係。 |
| [!UICONTROL  連絡先詳細 ] | `telecom` | オブジェクトの配列 | 担当者の連絡先詳細。 詳しくは、[ 以下の節 ](#telecom) を参照してください。 |
| [!UICONTROL  性別 ] | `gender` | 文字列 | 人物の性自認。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `female` </li> <li> `male` </li> <li> `other` </li> <li> `unknown`</li> |

### `telecom` {#telecom}

`telecom` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ 通信構造 ](../../images/field-groups/healthcare-patient/telecom.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL  連絡窓口 ] | `contactPoint` | [[!UICONTROL  連絡窓口 ]](../../data-types/healthcare/contact-point.md) | 人物の連絡先の詳細。 |

## `link` {#link}

`link` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ リンク構造 ](../../images/field-groups/healthcare-patient/link.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL その他] | `other` | [[!UICONTROL  参考 ]](../../data-types/healthcare/reference.md) | 同じ人物に関する患者または関係者のリソースへのリンク。 |
| [!UICONTROL タイプ] | `type` | 文字列 | 2 つの患者リソース間のリンクのタイプ。 |
