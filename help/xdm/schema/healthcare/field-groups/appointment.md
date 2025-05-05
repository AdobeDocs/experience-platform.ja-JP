---
title: 予定スキーマ フィールド グループ
description: 予定スキーマフィールドグループについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: 8224a2ee-51ac-4512-b0e4-5f1ab6bfddc4
source-git-commit: cb39966de77846758c16153f78fcf521f6a421e3
workflow-type: tm+mt
source-wordcount: '1227'
ht-degree: 5%

---

# [!UICONTROL &#x200B; 予定 &#x200B;] スキーマフィールドグループ

[!UICONTROL Appointment] は、[[!DNL XDM Individual Profile] class](../../../classes/individual-profile.md) および [[!DNL Provider class]](../../../classes/provider.md) の標準スキーマフィールドグループです。 患者、医療従事者、関係者、および/または特定の日時のデバイス間でのヘルスケアイベントの予約に関する情報を含む、単一のオブジェクトタイプのフィールド `healthcareAppointment` ータを提供します。

![ 予定フィールドグループ構造のスキーマ図。](../../../images/healthcare/field-groups/appointment/appointment.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL アカウント] | `account` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | 請求に使用されると予想されるアカウントのセット。 |
| [!UICONTROL &#x200B; 予定の種類 &#x200B;] | `appointmentType` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | スロットに予約されている予定または患者のスタイル （サービスタイプではない）。 |
| [!UICONTROL &#x200B; 基準 &#x200B;] | `basedOn` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | 予約の依頼は、手続き依頼などの評価に割り当てられます。 |
| [!UICONTROL &#x200B; 取消しの理由 &#x200B;] | `cancellationReason` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) の配列 | 予定がキャンセルされるコード化された理由。 これは、追加のアクションが必要かどうか、または特定の料金が適用されるかどうかを判断するために、レポート、請求、または処理でよく使用されます。 |
| [!UICONTROL クラス] | `class` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) の配列 | 外来患者、外来患者、入院患者、救急患者など、患者遭遇の分類を表す概念。 |
| [!UICONTROL 識別子] | `identifier` | [[!UICONTROL &#x200B; 識別子 &#x200B;]](../data-types/identifier.md) の配列 | 予定にリンクされている一意の ID のリスト。 これらの識別子は、ビジネス ルールに基づいて割り当てられているか、予定への直接 URL リンクが適切でない場合に割り当てられます。 |
| [!UICONTROL &#x200B; 注 &#x200B;] | `note` | [[!UICONTROL Annotation]](../data-types/annotation.md) の配列 | 予定に関する追加のメモまたはコメント。 |
| [!UICONTROL &#x200B; 元の予定 &#x200B;] | `originatingAppointment` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 関連する予定の定期的なセットの元の予定。 |
| [!UICONTROL &#x200B; 参加者 &#x200B;] | `participant` | オブジェクトの配列 | 予定に関与した参加者のリスト。 詳しくは、[ 以下の節 ](#participant) を参照してください。 |
| [!UICONTROL &#x200B; 患者指導 &#x200B;] | `patientInstruction` | [[!UICONTROL &#x200B; コード化可能な参照 &#x200B;]](../data-types/reference.md) の配列 | 予約に関連する診断。 |
| [!UICONTROL &#x200B; 前の予定 &#x200B;] | `previousAppointment` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 関連する一連の予定の前の予定。 |
| [!UICONTROL 優先度] | `priority` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 予定の優先順位。予定の優先順位を変更する必要がある場合に、十分な情報に基づいた決定を行うために使用できます。 iCal 規格では、`0` は未定義、`1` は最高、`9` は最低の優先度として指定されます。 |
| [!UICONTROL 理由] | `reason` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) の配列 | 予定がスケジュールされている理由（通常、条件または手順）。 |
| [!UICONTROL &#x200B; 繰り返しテンプレート &#x200B;] | `recurrenceTemplate` | オブジェクトの配列 | 定期的な予定の作成に使用する定期的なパターンまたはテンプレートの詳細を含みます。  詳しくは、[ 以下の節 ](#recurrence) を参照してください。 |
| [!UICONTROL &#x200B; 置換 &#x200B;] | `replaces` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | この予定に置き換えられる予定です。 キャンセルがある場合、キャンセルの詳細は、参照先のリソースの `cancellationReason` プロパティで確認できます。 |
| [!UICONTROL &#x200B; 申請期間 &#x200B;] | `requestedPeriod` | [[!UICONTROL &#x200B; 期間 &#x200B;]](../data-types/period.md) の配列 | 予定をスケジュールすることが望ましい一連の日付範囲（場合によっては時間を含む）。 |
| [!UICONTROL &#x200B; サービス区分 &#x200B;] | `serviceCategory` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) の配列 | 任用中に実行されるサービスの幅広いカテゴリ。 |
| [!UICONTROL &#x200B; サービスタイプ &#x200B;] | `serviceType` | [[!UICONTROL &#x200B; コード化可能な参照 &#x200B;]](../data-types/codeable-reference.md) の配列 | 予定中に実行される特定のサービス。 |
| [!UICONTROL &#x200B; スロット &#x200B;] | `slot` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | 予定によって記入される参加者のスケジュールからのタイムスロット。 |
| [!UICONTROL &#x200B; 専門分野 &#x200B;] | `speciality` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) の配列 | この任命で要求されたサービスを実行するために必要な実務担当者の専門分野。 |
| [!UICONTROL 件名] | `subject` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | 予定に関連付けられた患者またはグループ。 |
| [!UICONTROL &#x200B; 支援情報 &#x200B;] | `supportingInformation` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | サポートする予定を立てる際に提供される追加情報。 |
| [!UICONTROL &#x200B; 仮想サービス &#x200B;] | `virtualService` | の配列 [[!UICONTROL &#x200B; 仮想サービスの詳細 &#x200B;]](../data-types/virtual-service-detail.md) | 電話会議など、仮想サービスの接続の詳細。 |
| [!UICONTROL &#x200B; 解約の日 &#x200B;] | `cancellationDate` | 日時 | 予定がキャンセルされた日時。 |
| [!UICONTROL 作成日] | `created` | 日時 | 予定が作成された日時。 |
| [!UICONTROL 説明] | `description` | 文字列 | 予定の簡単な説明。 詳細な情報または展開された情報は、`note` フィールドに入力する必要があります。 |
| [!UICONTROL 終了] | `end` | 日時 | 予定が終了する日時です。 |
| [!UICONTROL &#x200B; 分の期間 &#x200B;] | `minutesDuration` | 整数 | 予定にかかる分数。 この値は、開始時刻と終了時刻の間の時間よりも短くすることができます。 許容される最小値は `0` です。 |
| [!UICONTROL &#x200B; 発生が変更されました &#x200B;] | `occurenceChanged` | ブール値 | この予定が定期的な予定と異なるかどうかを示すフラグ。 |
| [!UICONTROL RecurrenceId] | `RecurrenceId` | 整数 | 定期的な予定の特定のパターンを識別するシーケンス番号です。 最小値は `0` です。 |
| [!UICONTROL 開始] | `start` | 日時 | 予定が行われる日時。 |
| [!UICONTROL ステータス] | `status` | 文字列 | 予定のステータス。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります： <li> `proposed` </li> <li> `pending` </li> <li> `booked` </li> <li> `arrived` </li> <li> `fulfilled` </li> <li> `cancelled` </li> <li> `noshow` </li> <li> `entered-in-error` </li> <li> `checked-in` </li> <li> `waitlist` </li> |


フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/appointment.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/appointment.schema.json)

## `participant` {#participant}

`participant` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ 参加者オブジェクト構造のスキーマ図。](../../../images/healthcare/field-groups/appointment/participant.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; アクター &#x200B;] | `actor` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 予定に参加する個人、デバイス、場所、またはサービス。 |
| [!UICONTROL &#x200B; 期間 &#x200B;] | `period` | [[!UICONTROL &#x200B; 期間 &#x200B;]](../data-types/period.md) | 参加者（アクター）が予定に関与する期間。 |
| [!UICONTROL タイプ] | `type` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) の配列 | 予定における参加者（アクター）の役割。 |
| [!UICONTROL 必須] | `required` | ブール値 | この参加者が出席する必要があるかどうか。 |
| [!UICONTROL &#x200B; ステータス &#x200B;] | `status` | 文字列 | 参加者の受け入れステータス。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります： <li> `accepted` </li> <li> `declined` </li> <li> `tentative` </li> <li> `needs-action` </li> |

## `recurrenceTemplate` {#recurrence}

`recurrenceTemplate` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ 参照テンプレートオブジェクト構造のスキーマ図。](../../../images/healthcare/field-groups/appointment/recurrence-template.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 月間テンプレート &#x200B;] | `monthlyTemplate` | オブジェクトの配列 | 毎月の定期的な予定に関する情報です。 詳しくは、[ 以下の節 ](#monthly-template) を参照してください。 |
| [!UICONTROL &#x200B; 繰り返しタイプ &#x200B;] | `recurrenceType` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 定期的な予定を繰り返す頻度（毎週、毎月、毎年など）。 |
| [!UICONTROL タイムゾーン] | `timezone` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 定期的な予定のタイムゾーンを指定します。 |
| [!UICONTROL &#x200B; 週別テンプレート &#x200B;] | `weeklyTemplate` | オブジェクトの配列 | 毎週の定期的な予定に関する情報です。 詳しくは、以下の [ 節 ](#weekly-template) を参照してください。 |
| [!UICONTROL &#x200B; 年次テンプレート &#x200B;] | `yearlyTemplate` | オブジェクト | 1 年の定期的な予定に関する情報です。 予定が繰り返される n 年目ごとに示す整数値を含む、`yearInterval` という 1 つのプロパティが含まれます。 |
| [!UICONTROL &#x200B; 日付を除く。] | `excludingDate` | 日付の配列 | 休日など、繰り返しから除外する必要のある日付。 |
| [!UICONTROL &#x200B; 繰り返し Id の除外 &#x200B;] | `excludingRecurrenceId` | 整数の配列 | 繰り返しから除外する必要がある繰り返し ID。 これは、除外する予定の `reccurenceID` を指定する `excludingDate` の代わりに使用できます。 |
| [!UICONTROL &#x200B; 最終作成日 &#x200B;] | `lastOccurenceDate` | 日付 | これ以上定期的な予定が予定されない日付です。 |
| [!UICONTROL &#x200B; オカレンス数 &#x200B;] | `occurenceCount` | 整数 | この繰り返しに予定されている予定の数。 最小値は `0` です。 |
| [!UICONTROL &#x200B; 発生日 &#x200B;] | `occurenceDate` | 日付の配列 | 予定がスケジュールされる特定の日付のリスト。 |

## `weeklyTemplate` {#weekly-template}

`weeklyTemplate` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ 週別テンプレートオブジェクト構造のスキーマ図。](../../../images/healthcare/field-groups/appointment/weekly-template.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 金曜日 &#x200B;] | `friday` | ブール値 | 定期的な予定が金曜日に発生するように指定します。 |
| [!UICONTROL &#x200B; 月曜日 &#x200B;] | `monday` | ブール値 | 定期的な予定が月曜日に発生するように指定します。 |
| [!UICONTROL &#x200B; 土曜日 &#x200B;] | `saturday` | ブール値 | 土曜日に定期的な予定が発生するように指定します。 |
| [!UICONTROL &#x200B; 日曜日 &#x200B;] | `sunday` | ブール値 | 定期的な予定が日曜日に発生するように指定します。 |
| [!UICONTROL &#x200B; 木曜日 &#x200B;] | `thursday` | ブール値 | 定期的な予定が木曜日に発生するように指定します。 |
| [!UICONTROL &#x200B; 火曜日 &#x200B;] | `tuesday` | ブール値 | 定期的な予定が火曜日に発生するように指定します。 |
| [!UICONTROL &#x200B; 水曜日 &#x200B;] | `wednesday` | ブール値 | 定期的な予定が水曜日に発生するように指定します。 |
| [!UICONTROL &#x200B; 週の間隔 &#x200B;] | `weekInterval` | 整数 | 予定の繰り返し頻度を n 週単位で指定します。 デフォルトは毎週なので、一般的な値は 2 以上です。 |

## `monthlyTemplate` {#monthly-template}

`monthlyTemplate` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ 月次テンプレートオブジェクト構造のスキーマ図。](../../../images/healthcare/field-groups/appointment/monthly-template.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 曜日 &#x200B;] | `dayOfWeek` | [[!UICONTROL &#x200B; コーディング &#x200B;]] | この特定の曜日に予定が実行されるように指定します。 |
| [!UICONTROL &#x200B; 月の n 週目 &#x200B;] | `nthWeekOfMonth` | [[!UICONTROL &#x200B; コーディング &#x200B;]](../data-types/coding.md) | 予定を受け取る月の n 週目を示します。 |
| [!UICONTROL &#x200B; 日付 &#x200B;] | `dayOfMonth` | 整数 | その月のこの特定の日に予定が発生するように指定します。 |
| [!UICONTROL &#x200B; 月の間隔 &#x200B;] | `monthInterval` | 整数 | 定期的な予定が n か月ごとに発生するように指定します。 |

