---
title: ケア計画スキーマフィールドグループ
description: ケアプランスキーマフィールドグループについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
source-git-commit: e01a6cd4cf42338f87222a5e2871cd6c3683309a
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 4%

---

# [!UICONTROL  ケア計画 ] スキーマフィールドグループ

[!UICONTROL  ケアプラン ] は、[[!DNL XDM Individual Profile]  クラス ](../../classes/individual-profile.md) の標準スキーマフィールドグループです。 患者またはグループのヘルスケアプランをキャプチャする単一のオブジェクトタイプのフィールド `healthcareCarePlan` を提供します。

![ フィールドグループ構造 ](../../images/field-groups/healthcare-care-plan/care-plan.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL アクティビティ] | `activity` | オブジェクトの配列 | 計画の一部として発生した、または発生が予定されているアクションを識別します。 詳しくは、[ 以下の節 ](#activity) を参照してください。 |
| [!UICONTROL  住所 ] | `addresses` | [[!UICONTROL  コード化可能な参照 ]](../../data-types/healthcare/codeable-reference.md) の配列 | は、ケアプランが処理する条件または関心を識別します。 |
| [!UICONTROL  基準 ] | `basedOn` | [[!UICONTROL  参照 ]](../../data-types/healthcare/reference.md) の配列 | このケアプランによって全体または一部が履行される、より高レベルのリクエストリソース。 |
| [!UICONTROL  ケアチーム ] | `careTeam` | [[!UICONTROL  参照 ]](../../data-types/healthcare/reference.md) の配列 | この計画で想定されるケアに関与すると予想されるすべての人物および組織を特定します。 |
| [!UICONTROL カテゴリ] | `category` | [[!UICONTROL  コード化可能な概念 ]](../../data-types/healthcare/codeable-concept.md) の配列 | 複数の既存のプランの違いをサポートするために、どのようなプランがあるかを特定します。 |
| [!UICONTROL  投稿者 ] | `contributor` | [[!UICONTROL  参照 ]](../../data-types/healthcare/reference.md) の配列 | ケア計画のコンテンツを提供した個人、組織またはデバイスを識別します。 |
| [!UICONTROL  保護者 ] | `custodian` | [[!UICONTROL  参考 ]](../../data-types/healthcare/reference.md) | データが入力された場合、管理者はケア計画に責任を負い、そのケア計画に関連付けられます。 |
| [!UICONTROL  出会い ] | `encounter` | [[!UICONTROL  参考 ]](../../data-types/healthcare/reference.md) | ケアプランが作成された際の出会い。 |
| [!UICONTROL  目標 ] | `goal` | [[!UICONTROL  参照 ]](../../data-types/healthcare/reference.md) の配列 | 計画を実行する目的。 |
| [!UICONTROL 識別子] | `identifier` | [[!UICONTROL  識別子 ]](../../data-types/healthcare/identifier.md) の配列 | リソースが更新されてサーバーからサーバーに伝播される際に一定の状態が維持される、実行者または他のシステムによって、このケア計画に割り当てられたビジネス識別子。 |
| [!UICONTROL  注 ] | `note` | [[!UICONTROL Annotation]](../../data-types/healthcare/annotation.md) の配列 | 他の属性で扱われていないケアプランに関する一般的なメモ。 |
| [!UICONTROL  一部 ] | `partOf` | [[!UICONTROL  参照 ]](../../data-types/healthcare/reference.md) の配列 | この特定のケア計画が構成要素または段階である、より大きなケア計画。 |
| [!UICONTROL  期間 ] | `period` | [[!UICONTROL  期間 ]](../../data-types/healthcare/period.md) | プランが発効した（または発効が予定されている）日時と終了日を示します。 |
| [!UICONTROL  置換 ] | `replaces` | [[!UICONTROL  参照 ]](../../data-types/healthcare/reference.md) の配列 | 機能がこのケア計画に引き継がれる、完了または終了したケア計画。 |
| [!UICONTROL 件名] | `subject` | [[!UICONTROL  参考 ]](../../data-types/healthcare/reference.md) | 計画で意図したケアが記述されている患者またはグループを識別します。 |
| [!UICONTROL  サポート情報 ] | `supportingInfo` | [[!UICONTROL  参照 ]](../../data-types/healthcare/reference.md) の配列 | 計画の作成に影響を及ぼした患者の記録の一部を特定する。 これには、併存疾患、最近の処置、制限事項、最近の評価などが含まれます。 |
| [!UICONTROL 作成日] | `created` | 日時 | このケア プランがシステムで作成された日時を表します（多くの場合、システムで生成された日付）。 |
| [!UICONTROL 説明] | `description` | 文字列 | プランの範囲と特性の説明。 |
| [!UICONTROL Canonical をインスタンス化 ] | `instantiatesCanonical` | 文字列の配列 | FHIR が定義したプロトコル、ガイドライン、アンケート、またはこの計画によって全体または一部が準拠しているその他の定義を指す URL。 |
| [!UICONTROL Uri をインスタンス化 ] | `instantiatesUri` | 文字列の配列 | 外部で管理されるプロトコル、ガイドライン、アンケート、またはこの計画によって全体または一部に固定されているその他の定義を指す URL。URI で表されます。 |
| [!UICONTROL  インテント ] | `intent` | 文字列 | ケアプランの目的。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `proposal` </li> <li> `plan` </li> <li> `order` </li> <li> `option` </li> <li> `directive` </li> |
| [!UICONTROL ステータス] | `status` | 文字列 | ケア計画のステータス。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `draft` </li> <li> `active` </li> <li> `on-hold` </li> <li> `revoked` </li> <li> `completed` </li> <li> `entered-in-error` </li> <li> `unknown` </li> |
| [!UICONTROL タイトル] | `title` | 文字列 | ケアプランの名前。 |

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/careplan.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/careplan.schema.json)

## `activity` {#activity}

`activity` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ アクティビティ構造 ](../../images/field-groups/healthcare-care-plan/activity.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL  実行されたアクティビティ ] | `performedActivity` | [[!UICONTROL  コード化可能な参照 ]](../../data-types/healthcare/codeable-reference.md) の配列 | 予定や手続きなど、アクティビティの結果。 |
| [!UICONTROL  計画アクティビティの参照 ] | `plannedActivityReference` | [[!UICONTROL  参考 ]](../../data-types/healthcare/reference.md) | 提案されたアクティビティの詳細。 |
| [!UICONTROL  進行状況 ] | `progress` | [[!UICONTROL Annotation]](../../data-types/healthcare/annotation.md) の配列 | アクティビティの遵守、ステータスまたは進捗に関するメモ。 |
