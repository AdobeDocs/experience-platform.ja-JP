---
title: 投薬調剤スキーマフィールドグループ
description: 医薬品の調剤スキーマフィールドグループについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: e897c4e0-23ad-4d79-834f-cfbe2dbec771
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '689'
ht-degree: 4%

---

# [!UICONTROL  投薬調剤 ] スキーマフィールドグループ

[!UICONTROL  投薬調剤 ] は、[[!DNL Medication]  クラス ](../../../classes/medication.md)、[[!DNL XDM Individual Profile]  クラス ](../../../classes/individual-profile.md) および [[!DNL Provider class]](../../../classes/provider.md) の標準スキーマフィールドグループです。 指定された人または患者に対して調剤される、または調剤された薬に関する情報を収集する単一のオブジェクトタイプのフィールド `healthcareMedicationDispense` を提供します。

![ フィールドグループ構造 ](../../../images/healthcare/field-groups/medication-dispense/medication-dispense.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL  認可の時効 ] | `authorizingPrescription` | [[!UICONTROL  参照 ]](../data-types/reference.md) の配列 | 処方箋の調剤を許可するオーダー。 |
| [!UICONTROL  基準 ] | `basedOn` | [[!UICONTROL  参照 ]](../data-types/reference.md) の配列 | 薬の調剤は基づいている。 |
| [!UICONTROL カテゴリ] | `category` | [[!UICONTROL  コード化可能な概念 ]](../data-types/codeable-concept.md) の配列 | 調剤される薬剤のカテゴリーは、例えば、薬剤の法的カテゴリーや薬剤の分類などである。 |
| [!UICONTROL  供給日数 ] | `daysSupply` | [[!UICONTROL  単純量 ]](../data-types/simple-quantity.md) | 薬が患者に供給される日数。 |
| [!UICONTROL  移動先 ] | `destination` | [[!UICONTROL  参考 ]](../data-types/reference.md) | 調剤イベントの一環として、薬が発送された、または発送される施設または場所。 |
| [!UICONTROL  服薬指示 ] | `dosageInstruction` | 配列 [[!UICONTROL Dose]](../data-types/dosage.md) | 薬が患者によってどのように使用されるかを説明します。 |
| [!UICONTROL  出会い ] | `encounter` | [[!UICONTROL  参考 ]](../data-types/reference.md) | このイベントのコンテキストを確立する出会い。 |
| [!UICONTROL  イベント履歴 ] | `eventHistory` | [[!UICONTROL  参照 ]](../data-types/reference.md) の配列 | ディスペンスに関連して発生したイベントの概要。 |
| [!UICONTROL 識別子] | `identifier` | [[!UICONTROL  識別子 ]](../data-types/identifier.md) の配列 | ディスペンスに関連付けられた識別子。 識別子は、ビジネスプロセスで定義する必要があり、直接 URL 参照が適切でない場合は、識別子を使用して URL を参照する必要があります。 |
| [!UICONTROL 場所] | `location` | [[!UICONTROL  参考 ]](../data-types/reference.md) | 薬が調剤された主な物理的な場所。 |
| [!UICONTROL  医薬品 ] | `medication` | [[!UICONTROL  コード化可能な参照 ]](../data-types/codeable-reference.md) | リクエストされている薬を識別します。 これは、薬の詳細を表すリソースへのリンク、または薬を識別するコードである必要があります。 |
| [!UICONTROL  実行されていない理由 ] | `notPerformedReason` | [[!UICONTROL  コード化可能な参照 ]](../data-types/codeable-reference.md) | 薬が調剤されなかった理由。 |
| [!UICONTROL  注 ] | `note` | [[!UICONTROL Annotation]](../data-types/annotation.md) の配列 | 調剤に関する追加情報。 |
| [!UICONTROL  一部 ] | `partOf` | [[!UICONTROL  参照 ]](../data-types/reference.md) の配列 | 調剤をトリガーした手順または投薬リクエスト。 |
| [!UICONTROL  実行者 ] | `performer` | オブジェクトの配列 | ディスペンシングイベントを実行したユーザーまたは内容を示します。 詳しくは、[ 以下の節 ](#performer) を参照してください。 |
| [!UICONTROL  数量 ] | `quantity` | [[!UICONTROL  単純量 ]](../data-types/simple-quantity.md) | 測定単位を含む、調剤された薬の量。 |
| [!UICONTROL  受信者 ] | `receiver` | [[!UICONTROL  参照 ]](../data-types/reference.md) の配列 | 薬を受け取った人、または薬が届いた場所を特定します。 |
| [!UICONTROL 件名] | `subject` | [[!UICONTROL  参考 ]](../data-types/reference.md) | 薬を投与する個人またはグループを表すリソースへのリンク。 |
| [!UICONTROL  交代 ] | `substitution` | オブジェクト | 調剤の一部として代用が行われたかどうかを示します。 次の 4 つのプロパティが含まれます。 <li>`wasSubstituted`: ディスペンサーが代用をリクエストした場合に true となるブール値。</li> <li>`type`：置換が行われたかどうかを示すコードを提供する [[!UICONTROL  コード可能な概念 ]](../data-types/codeable-concept.md) 値。</li> <li>`reason`：置換の理由を含む [[!UICONTROL  コード可能な概念 ]](../data-types/codeable-concept.md) 値の配列。</li> <li>`responsibleParty`：代替の責任者または関係者を提供する [[!UICONTROL  参照 ]](../data-types/reference.md) 値。 </li> |
| [!UICONTROL  支援情報 ] | `supportingInformation` | [[!UICONTROL  参照 ]](../data-types/reference.md) の配列 | 調剤される薬剤を支援する追加情報。 |
| [!UICONTROL タイプ] | `type` | [[!UICONTROL  コード化可能な概念 ]](../data-types/codeable-concept.md) | 実行される分配イベントのタイプを表します（緊急入力または部分入力など）。 |
| [!UICONTROL  記録済み ] | `recorded` | 日時 | `whenPrepared` または `whenHandedOver` が入力されていない場合に分配アクティビティが開始された日時。 |
| [!UICONTROL  表示投与量指示 ] | `renderedDosageInstruction` | 文字列 | 全ての用量指示に含まれる用量の完全な表示。 増量投与や漸減投与などの複雑な投与を表すために複数投与指示が含まれる場合に使用される。 |
| [!UICONTROL ステータス] | `status` | 文字列 | 分注のステータス。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `preperation` </li> <li> `in-progress` </li> <li> `cancelled` </li> <li> `on-hold` </li> <li> `completed` </li> <li> `entered-in-error` </li> <li> `stopped` </li> <li> `declined` </li> <li> `unknown` </li> |
| [!UICONTROL  ステータスが変更されました ] | `statusChanged` | 日時 | 払出レコードの状態が変更された日時。 |
| [!UICONTROL  引き渡し時 ] | `whenHandedOver` | 日時 | 調剤された薬剤が患者に提供された日時。 |
| [!UICONTROL  準備が整ったら ] | `whenPrepared` | 日時 | 調剤された薬剤が包装され、レビューされた日時。 |

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/medicationdispense.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/medicationdispense.schema.json)

## `performer` {#performer}

`performer` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ パフォーマンス構造 ](../../../images/healthcare/field-groups/medication-dispense/performer.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL  アクター ] | `actor` | [[!UICONTROL  参考 ]](../data-types/reference.md) | アクションを実行した実務担当者（または同様の担当者）。 俳優が薬のディスペンサーであると仮定されるべきです。 |
| [!UICONTROL 関数] | `function` | [[!UICONTROL  コード化可能な概念 ]](../data-types/codeable-concept.md) | 日付入力、パッケージャー、最終チェッカーなど、分注のパフォーマのタイプ。 |
