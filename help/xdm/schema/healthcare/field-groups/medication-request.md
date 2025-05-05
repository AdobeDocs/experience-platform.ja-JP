---
title: 医薬品リクエストスキーマフィールドグループ
description: 投薬リクエストスキーマフィールドグループについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: 8502380f-9557-4ca6-84bc-65010dfc6066
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '997'
ht-degree: 3%

---

# [!UICONTROL &#x200B; 投薬リクエスト &#x200B;] スキーマフィールドグループ

[!UICONTROL &#x200B; 投薬リクエスト &#x200B;] は、[[!DNL Medication]  クラス ](../../../classes/medication.md)、[[!DNL XDM Individual Profile]  クラス ](../../../classes/individual-profile.md)、[[!DNL Provider class]](../../../classes/provider.md) の標準スキーマフィールドグループです。 薬の供給の注文または要求と、患者への薬の投与の指示の両方をキャプチャする単一のオブジェクトタイプのフィールド `healthcareMedicationDispense` を提供します。

![ フィールドグループ構造 ](../../../images/healthcare/field-groups/medication-request/medication-request.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 基準 &#x200B;] | `basedOn` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | この投薬要求によって満たされた計画または要求。 |
| [!UICONTROL カテゴリ] | `category` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) の配列 | 投薬リクエストの分類またはグループ化。 |
| [!UICONTROL &#x200B; 治療の種類 &#x200B;] | `courseOfTherapyType` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 患者への薬の投与のための全体的なパターンの説明。 |
| [!UICONTROL &#x200B; デバイス &#x200B;] | `device` | [[!UICONTROL &#x200B; コード化可能な参照 &#x200B;]](../data-types/codeable-reference.md) の配列 | 投薬の投与に使用されるデバイスのタイプ。 |
| [!UICONTROL &#x200B; 交付の請求 &#x200B;] | `dispenseRequest` | オブジェクト | 調剤リクエストの具体的な詳細を示します。通常、投薬指示と呼ばれます。 詳しくは、[ 以下の節 ](#dispense-request) を参照してください。 |
| [!UICONTROL &#x200B; 服薬指示 &#x200B;] | `dosageInstructions` | 配列 [[!UICONTROL Dose]](../data-types/dosage.md) | 薬が患者によってどのように使用されるかについての具体的な指示。 |
| [!UICONTROL &#x200B; 有効線量期間 &#x200B;] | `effectiveDosePeriod` | [[!UICONTROL &#x200B; 期間 &#x200B;]](../data-types/period.md) | 薬を服用する期間。 複数の `dosageInstruction` 線がある場合（例えば、用量を漸減する場合）、これは投薬指示の最も早い日付と最も遅い日付です。 |
| [!UICONTROL &#x200B; 出会い &#x200B;] | `encounter` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | リクエストが作成された際の発生。 |
| [!UICONTROL &#x200B; イベント履歴 &#x200B;] | `eventHistory` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | 申請の完了、主要な状態の遷移の瞬間、関連する更新など、投薬申請に関連するイベントのレコードへのリンク。 |
| [!UICONTROL &#x200B; グループ識別子 &#x200B;] | `groupIdentifier` | [[!UICONTROL 識別子]](../data-types/identifier.md) | 1 人の作成者によってアクティブ化された、複数の独立したリクエストインスタンス間の共有識別子。 |
| [!UICONTROL 識別子] | `identifier` | [[!UICONTROL &#x200B; 識別子 &#x200B;]](../data-types/identifier.md) の配列 | ビジネスプロセスで定義される投薬要求に関連付けられた識別子、および/またはリソース自体を参照する直接 URL 参照が適切でない場合に、リソースを参照するために使用されます。 |
| [!UICONTROL &#x200B; 情報Source] | `informationSource` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | ソースが `requester` 以外のユーザーである場合は、リクエストの情報を提供した個人または組織。 |
| [!UICONTROL &#x200B; 保険 &#x200B;] | `insurance` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | 保険プラン、補償範囲延長、事前認証、および/または要求されたサービスの提供に必要な事前決定。 |
| [!UICONTROL &#x200B; 医薬品 &#x200B;] | `medication` | [[!UICONTROL &#x200B; コード化可能な参照 &#x200B;]](../data-types/codeable-reference.md) | リクエストされている薬を識別します。 これは、薬の詳細を表すリソースへのリンク、または薬を識別するコードである必要があります。 |
| [!UICONTROL &#x200B; 注 &#x200B;] | `note` | [[!UICONTROL Annotation]](../data-types/annotation.md) の配列 | 他の属性では伝えられない、処方箋に関する追加情報です。 |
| [!UICONTROL &#x200B; 実行者 &#x200B;] | `performer` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | 投薬治療/投与の特定の実行者。 デバイスの場合、これは薬の投与を行うことを目的とした装置です。 |
| [!UICONTROL &#x200B; 実行者の種類 &#x200B;] | `performerType` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 薬の投与に対する実行者のタイプを示します。 |
| [!UICONTROL &#x200B; 先時効 &#x200B;] | `priorPrescription` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | この請求によって差し替えられる命令又は処方箋の記載。 |
| [!UICONTROL 理由] | `reason` | [[!UICONTROL &#x200B; コード化可能な参照 &#x200B;]](../data-types/reference.md) の配列 | 薬を注文する、または注文しない理由または表示。 |
| [!UICONTROL &#x200B; レコーダー &#x200B;] | `recorder` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 別の個人に代わって注文を入力した人物。 |
| [!UICONTROL &#x200B; 要求者 &#x200B;] | `requester` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | リクエストを開始し、アクティベーションに対して責任を持つ個人、組織またはデバイス。 |
| [!UICONTROL &#x200B; ステータスの理由 &#x200B;] | `statusReason` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | リクエストの現在の状態の理由。 |
| [!UICONTROL 件名] | `subject` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 薬の投与をリクエストされた個人またはグループ。 |
| [!UICONTROL &#x200B; 交代 &#x200B;] | `substitution` | オブジェクト | 代用が調剤に含まれる可能性があるか、含まれる必要があるかを示します。 次の 3 つのプロパティが含まれます。 <li>`allowedBoolean`：代用が許可されている場合に true となるブール値。</li> <li>`allowedCodeableConcept`：処方者が代用を許可している場合にコードを提供する [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) 値。</li> <li>`reason`：置換の理由を示す [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) 値。</li> |
| [!UICONTROL &#x200B; 支援情報 &#x200B;] | `supportingInformation` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | 患者さんの身長や体重など、薬を満たすことを支援する情報。 |
| [!UICONTROL &#x200B; 作成日時 &#x200B;] | `authoredOn` | 日時 | 処方箋が書かれた日付（およびオプションで時間）。 |
| [!UICONTROL &#x200B; 実行しない &#x200B;] | `doNotPerform` | ブール値 | ブール値の指標が true の場合、患者は薬の服用を停止（または開始しない）する必要があります。 |
| [!UICONTROL &#x200B; インテント &#x200B;] | `intent` | 文字列 | リクエストの目的。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `proposal` </li> <li> `plan` </li> <li> `order` </li> <li> `original-order` </li> <li> `reflex-order` </li> <li> `filler-order` </li> <li> `instance-order` </li> <li> `option` </li> |
| [!UICONTROL 優先度] | `priority` | 文字列 | リクエストの優先度。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `routine` </li> <li> `urgent` </li> <li> `asap` </li> <li> `stat` </li> |
| [!UICONTROL &#x200B; 表示投与量指示 &#x200B;] | `renderedDosageInstruction` | 文字列 | 全ての用量指示に含まれる用量の完全な表示。 増量投与や漸減投与などの複雑な投与を表すために複数投与指示が含まれる場合に使用される。 |
| [!UICONTROL &#x200B; 報告 &#x200B;] | `reported` | ブール値 | このレコードが、元のプライマリソースのレコードとしてではなく、セカンダリレポートされたレコードとしてキャプチャされたかどうかを示します。 |
| [!UICONTROL ステータス] | `status` | 文字列 | 分注のステータス。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `preperation` </li> <li> `in-progress` </li> <li> `cancelled` </li> <li> `on-hold` </li> <li> `completed` </li> <li> `entered-in-error` </li> <li> `stopped` </li> <li> `declined` </li> <li> `unknown` </li> |
| [!UICONTROL &#x200B; ステータスが変更されました &#x200B;] | `statusChanged` | 日時 | のステータスが変更された日付（およびオプションで時間）。 |

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/medicationrequest.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/medicationrequest.schema.json)

## `dispenseRequest` {#dispense-request}

`dispenseRequest` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ リクエスト構造のディスペンス ](../../../images/healthcare/field-groups/medication-request/dispense-request.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 調剤間隔 &#x200B;] | `dispenseInterval` | [[!UICONTROL 期間]](../data-types/duration.md) | 薬の調剤の間に発生する必要がある最小期間。 |
| [!UICONTROL &#x200B; ディスペンサー &#x200B;] | `dispenser` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 処方者が指定した薬を調剤する組織。 |
| [!UICONTROL &#x200B; ディスペンサー指示 &#x200B;] | `dispenserInstruction` | [[!UICONTROL Annotation]](../data-types/annotation.md) の配列 | 患者に提供されるカウンセリングなど、ディスペンサーの追加情報 |
| [!UICONTROL &#x200B; 投与計画書 &#x200B;] | `doseAdministrationAid` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 薬剤の調剤に提供するアドヒアランス包装の種類に関する情報。 |
| [!UICONTROL &#x200B; 予想供給期間 &#x200B;] | `expectedSupplyDuration` | [[!UICONTROL 期間]](../data-types/duration.md) | 供給された製品が使用される予想される期間、または分配が持続すると予想される時間。 |
| [!UICONTROL &#x200B; 初期塗りつぶし &#x200B;] | `initialFill` | オブジェクト | 初期塗りつぶしの情報。 次の 2 つのプロパティが含まれます。 <li>`quantity`：最初のディスペンスで提供する量を示す [[!UICONTROL &#x200B; 単純量 &#x200B;]](../data-types/simple-quantity.md) 値。</li> <li>`duration`：最初のディスペンスが持続すると予想される時間の長さを提供する [[!UICONTROL &#x200B; 期間 &#x200B;]](../data-types/duration.md) 値。</li> |
| [!UICONTROL &#x200B; 数量 &#x200B;] | `quantity` | [[!UICONTROL &#x200B; 単純量 &#x200B;]](../data-types/simple-quantity.md) | 盛土に対して分配される金額。 |
| [!UICONTROL &#x200B; 有効期間 &#x200B;] | `validityPeriod` | [[!UICONTROL &#x200B; 期間 &#x200B;]](../data-types/period.md) | 時効の有効期間。 |
| [!UICONTROL &#x200B; 許可される繰り返し数 &#x200B;] | `numberOfRepeatsAllowed` | 整数 | 許可されたリフィルの数（最小値は 0）。 |
