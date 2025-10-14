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

# [!UICONTROL &#x200B; 投薬調剤 &#x200B;] スキーマフィールドグループ

[!UICONTROL &#x200B; 投薬調剤 &#x200B;] は、[[!DNL Medication]  クラス &#x200B;](../../../classes/medication.md)、[[!DNL XDM Individual Profile]  クラス &#x200B;](../../../classes/individual-profile.md) および [[!DNL Provider class]](../../../classes/provider.md) の標準スキーマフィールドグループです。 指定された人または患者に対して調剤される、または調剤された薬に関する情報を収集する単一のオブジェクトタイプのフィールド `healthcareMedicationDispense` を提供します。

![&#x200B; フィールドグループ構造 &#x200B;](../../../images/healthcare/field-groups/medication-dispense/medication-dispense.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 認可の時効 &#x200B;] | `authorizingPrescription` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | 処方箋の調剤を許可するオーダー。 |
| [!UICONTROL &#x200B; 基準 &#x200B;] | `basedOn` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | 薬の調剤は基づいている。 |
| [!UICONTROL カテゴリ] | `category` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) の配列 | 調剤される薬剤のカテゴリーは、例えば、薬剤の法的カテゴリーや薬剤の分類などである。 |
| [!UICONTROL &#x200B; 供給日数 &#x200B;] | `daysSupply` | [[!UICONTROL &#x200B; 単純量 &#x200B;]](../data-types/simple-quantity.md) | 薬が患者に供給される日数。 |
| [!UICONTROL &#x200B; 移動先 &#x200B;] | `destination` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 調剤イベントの一環として、薬が発送された、または発送される施設または場所。 |
| [!UICONTROL &#x200B; 服薬指示 &#x200B;] | `dosageInstruction` | 配列 [[!UICONTROL Dose]](../data-types/dosage.md) | 薬が患者によってどのように使用されるかを説明します。 |
| [!UICONTROL &#x200B; 出会い &#x200B;] | `encounter` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | このイベントのコンテキストを確立する出会い。 |
| [!UICONTROL &#x200B; イベント履歴 &#x200B;] | `eventHistory` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | ディスペンスに関連して発生したイベントの概要。 |
| [!UICONTROL 識別子] | `identifier` | [[!UICONTROL &#x200B; 識別子 &#x200B;]](../data-types/identifier.md) の配列 | ディスペンスに関連付けられた識別子。 識別子は、ビジネスプロセスで定義する必要があり、直接 URL 参照が適切でない場合は、識別子を使用して URL を参照する必要があります。 |
| [!UICONTROL 場所] | `location` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 薬が調剤された主な物理的な場所。 |
| [!UICONTROL &#x200B; 医薬品 &#x200B;] | `medication` | [[!UICONTROL &#x200B; コード化可能な参照 &#x200B;]](../data-types/codeable-reference.md) | リクエストされている薬を識別します。 これは、薬の詳細を表すリソースへのリンク、または薬を識別するコードである必要があります。 |
| [!UICONTROL &#x200B; 実行されていない理由 &#x200B;] | `notPerformedReason` | [[!UICONTROL &#x200B; コード化可能な参照 &#x200B;]](../data-types/codeable-reference.md) | 薬が調剤されなかった理由。 |
| [!UICONTROL &#x200B; 注 &#x200B;] | `note` | [[!UICONTROL Annotation]](../data-types/annotation.md) の配列 | 調剤に関する追加情報。 |
| [!UICONTROL &#x200B; 一部 &#x200B;] | `partOf` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | 調剤をトリガーした手順または投薬リクエスト。 |
| [!UICONTROL &#x200B; 実行者 &#x200B;] | `performer` | オブジェクトの配列 | ディスペンシングイベントを実行したユーザーまたは内容を示します。 詳しくは、[&#x200B; 以下の節 &#x200B;](#performer) を参照してください。 |
| [!UICONTROL &#x200B; 数量 &#x200B;] | `quantity` | [[!UICONTROL &#x200B; 単純量 &#x200B;]](../data-types/simple-quantity.md) | 測定単位を含む、調剤された薬の量。 |
| [!UICONTROL &#x200B; 受信者 &#x200B;] | `receiver` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | 薬を受け取った人、または薬が届いた場所を特定します。 |
| [!UICONTROL 件名] | `subject` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 薬を投与する個人またはグループを表すリソースへのリンク。 |
| [!UICONTROL &#x200B; 交代 &#x200B;] | `substitution` | オブジェクト | 調剤の一部として代用が行われたかどうかを示します。 次の 4 つのプロパティが含まれます。 <li>`wasSubstituted`: ディスペンサーが代用をリクエストした場合に true となるブール値。</li> <li>`type`：置換が行われたかどうかを示すコードを提供する [[!UICONTROL &#x200B; コード可能な概念 &#x200B;]](../data-types/codeable-concept.md) 値。</li> <li>`reason`：置換の理由を含む [[!UICONTROL &#x200B; コード可能な概念 &#x200B;]](../data-types/codeable-concept.md) 値の配列。</li> <li>`responsibleParty`：代替の責任者または関係者を提供する [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) 値。 </li> |
| [!UICONTROL &#x200B; 支援情報 &#x200B;] | `supportingInformation` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | 調剤される薬剤を支援する追加情報。 |
| [!UICONTROL タイプ] | `type` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 実行される分配イベントのタイプを表します（緊急入力または部分入力など）。 |
| [!UICONTROL &#x200B; 記録済み &#x200B;] | `recorded` | 日時 | `whenPrepared` または `whenHandedOver` が入力されていない場合に分配アクティビティが開始された日時。 |
| [!UICONTROL &#x200B; 表示投与量指示 &#x200B;] | `renderedDosageInstruction` | 文字列 | 全ての用量指示に含まれる用量の完全な表示。 増量投与や漸減投与などの複雑な投与を表すために複数投与指示が含まれる場合に使用される。 |
| [!UICONTROL ステータス] | `status` | 文字列 | 分注のステータス。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `preperation` </li> <li> `in-progress` </li> <li> `cancelled` </li> <li> `on-hold` </li> <li> `completed` </li> <li> `entered-in-error` </li> <li> `stopped` </li> <li> `declined` </li> <li> `unknown` </li> |
| [!UICONTROL &#x200B; ステータスが変更されました &#x200B;] | `statusChanged` | 日時 | 払出レコードの状態が変更された日時。 |
| [!UICONTROL &#x200B; 引き渡し時 &#x200B;] | `whenHandedOver` | 日時 | 調剤された薬剤が患者に提供された日時。 |
| [!UICONTROL &#x200B; 準備が整ったら &#x200B;] | `whenPrepared` | 日時 | 調剤された薬剤が包装され、レビューされた日時。 |

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/medicationdispense.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/medicationdispense.schema.json)

## `performer` {#performer}

`performer` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![&#x200B; パフォーマンス構造 &#x200B;](../../../images/healthcare/field-groups/medication-dispense/performer.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; アクター &#x200B;] | `actor` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | アクションを実行した実務担当者（または同様の担当者）。 俳優が薬のディスペンサーであると仮定されるべきです。 |
| [!UICONTROL 関数] | `function` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 日付入力、パッケージャー、最終チェッカーなど、分注のパフォーマのタイプ。 |
