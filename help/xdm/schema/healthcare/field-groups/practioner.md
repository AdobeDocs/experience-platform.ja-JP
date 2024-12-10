---
title: 実務担当者スキーマフィールドグループ
description: プラクショナースキーマフィールドグループについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: 71210303-a3dd-458c-9c8a-ac8b546c2b1d
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 8%

---

# [!UICONTROL  実務担当者 ] スキーマフィールドグループ

[!UICONTROL Practioner] は、[[!DNL XDM Individual Profile] class](../../../classes/individual-profile.md) および [[!DNL Provider class]](../../../classes/provider.md) の標準スキーマフィールドグループです。 これは、ヘルスケアまたは関連サービスのプロビジョニングに直接または間接的に関与するユーザーに関する情報を含む、単一のオブジェクトタイプのフィールド `healthcarePractioner` ータを提供します。

![ フィールドグループ構造 ](../../../images/healthcare/field-groups/practitioner/practitioner.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL アドレス] | `address` | [[!UICONTROL Address]](../data-types/address.md) の配列 | 勤務地を超える実務担当者の住所（自宅住所など）。 |
| [!UICONTROL  連絡 ] | `communication` | オブジェクトの配列 | 実務担当者とのコミュニケーションに使用できる言語。 詳しくは、以下の [ 節 ](#communication) を参照してください |
| [!UICONTROL 識別子] | `identifier` | [[!UICONTROL  識別子 ]](../data-types/identifier.md) の配列 | この役割のこの人物に適用される識別子。 |
| [!UICONTROL 名前] | `name` | [[!UICONTROL  人名 ]](../data-types/human-name.md) の配列 | 実践者に関連付けられた名前。 |
| [!UICONTROL  資格 ] | `qualification` | オブジェクトの配列 | 医療従事者によるケアの提供を承認または許可する公式の資格、認定、認定、トレーニング、ライセンス、またはそれに類似するもの。 詳しくは、[ 以下の節 ](#qualification) を参照してください。 |
| [!UICONTROL  連絡先詳細 ] | `telecom` | [[!UICONTROL  連絡先 ]](../data-types/contact-point.md) の配列 | 実務担当者の連絡先の詳細。 |
| [!UICONTROL  アクティブ ] | `active` | ブール値 | 実務担当者レコードが有効に使用されているかどうかを示します。 |
| [!UICONTROL  生年月日 ] | `birthDate` | 日付 | 実務担当者の生年月日。 |
| [!UICONTROL  死亡インジケーター ] | `deceasedBoolean` | ブール値 | 実務担当者が死亡したかどうかを示します。 |
| [!UICONTROL  死亡日時 ] | `deceasedDateTime` | 日時 | 施術者が死亡した日時。 |
| [!UICONTROL  性別 ] | `gender` | 文字列 | 人物の性自認。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `female` </li> <li> `male` </li> <li> `other` </li> <li> `unknown`</li> |

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/practitioner.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/practitioner.schema.json)

## `communication` {#communication}

`communication` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ 通信構造 ](../../../images/healthcare/field-groups/practitioner/communication.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL 言語] | `language` | [[!UICONTROL  コード化可能な概念 ]](../data-types/codeable-concept.md) | 健康状態について患者とコミュニケーションを取るために使用できる言語。 |
| [!UICONTROL  優先言語 ] | `preferred` | ブール値 | 言語が優先言語かどうかを示します。 |

## `qualification` {#qualification}

`qualification` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ 認定構造 ](../../../images/healthcare/field-groups/practitioner/qualification.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL コード] | `code` | [[!UICONTROL  コード化可能な概念 ]](../data-types/codeable-concept.md) | 選定のコード化表現。 |
| [!UICONTROL 識別子] | `identifier` | [[!UICONTROL  識別子 ]](../data-types/identifier.md) の配列 | 選定の識別子。 |
| [!UICONTROL  発行者 ] | `issuer` | [[!UICONTROL  参考 ]](../data-types/reference.md) | 資格を規制および発行する組織。 |
| [!UICONTROL  期間 ] | `period` | [[!UICONTROL  期間 ]](../data-types/period.md) | 資格が有効な期間。 |
