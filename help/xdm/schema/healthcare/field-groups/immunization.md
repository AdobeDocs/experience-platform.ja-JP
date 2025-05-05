---
title: 免疫スキーマフィールドグループ
description: 予防接種スキーマフィールドグループについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: a392e26b-7631-4f54-b9ad-cc4586673ac5
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 8%

---

# [!UICONTROL &#x200B; 予防接種 &#x200B;] スキーマフィールドグループ

[!UICONTROL &#x200B; 予防接種 &#x200B;] は、[[!DNL XDM Experience Event]  クラス ](../../../classes/experienceevent.md) の標準スキーマフィールドグループです。 免疫イベント情報を取得する単一のオブジェクトタイプフィールド `healthcareImmunization` を提供します。

![ フィールドグループ構造 ](../../../images/healthcare/field-groups/immunization/immunization.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 使用製品 &#x200B;] | `administeredProduct` | [[!UICONTROL &#x200B; コード化可能な参照 &#x200B;]](../data-types/codeable-reference.md) | 投与された製品。 |
| [!UICONTROL &#x200B; 基準 &#x200B;] | `basedOn` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | 予防接種イベントの基になる権限。 |
| [!UICONTROL &#x200B; 線量量 &#x200B;] | `doseQuantity` | [[!UICONTROL &#x200B; 単純量 &#x200B;]](../data-types/simple-quantity.md) | 投与されるワクチンの量。 |
| [!UICONTROL &#x200B; 出会い &#x200B;] | `encounter` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 予防接種を受けた人との出会いは一部でした。 |
| [!UICONTROL &#x200B; 資金Source] | `fundingSource` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | ワクチンの資金源。 |
| [!UICONTROL 識別子] | `identifier` | [[!UICONTROL &#x200B; 識別子 &#x200B;]](../data-types/identifier.md) の配列 | ビジネス識別子。 |
| [!UICONTROL &#x200B; 情報Source] | `informationSource` | [[!UICONTROL &#x200B; コード化可能な参照 &#x200B;]](../data-types/codeable-reference.md) | レポートされたレコードのソースを示します。 |
| [!UICONTROL 場所] | `location` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 予防接種が行われた場所。 |
| [!UICONTROL &#x200B; 製造元 &#x200B;] | `manufacturer` | [[!UICONTROL &#x200B; コード化可能な参照 &#x200B;]](../data-types/codeable-reference.md) | ワクチンの製造元。 |
| [!UICONTROL &#x200B; 注 &#x200B;] | `note` | [[!UICONTROL Annotation]](../data-types/annotation.md) の配列 | 追加の予防接種記録。 |
| [!UICONTROL &#x200B; 患者 &#x200B;] | `patient` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 予防接種を受けた人。 |
| [!UICONTROL バッチ] | `performer` | オブジェクトの配列 | 予防接種イベントを行った人。 詳しくは、[ 以下の節 ](#performer) を参照してください。 |
| [!UICONTROL &#x200B; プログラムの実施要件 &#x200B;] | `programEligibility` | オブジェクトの配列 | 特定のワクチン接種プログラムに対する患者の実施要件。 詳しくは、[ 以下の節 ](#program-eligibility) を参照してください。 |
| [!UICONTROL &#x200B; 適用されるプロトコル &#x200B;] | `protocolApplied` | オブジェクトの配列 | プロバイダーから提供されるプロトコル。 詳しくは、[ 以下の節 ](#protocol-applied) を参照してください。 |
| [!UICONTROL &#x200B; 反応 &#x200B;] | `reaction` | オブジェクトの配列 | 予防接種後の反応の詳細。 詳しくは、[ 以下の節 ](#reaction) を参照してください。 |
| [!UICONTROL 理由] | `reason` | [[!UICONTROL &#x200B; コード化可能な参照 &#x200B;]](../data-types/codeable-reference.md) の配列 | 予防接種の理由。 |
| [!UICONTROL &#x200B; ルート &#x200B;] | `route` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | ワクチンが体に入った方法。 |
| [!UICONTROL &#x200B; サイト &#x200B;] | `site` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | ワクチンを接種した身体部位 |
| [!UICONTROL &#x200B; ステータスの理由 &#x200B;] | `statusReason` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 現在のステータスの理由。 |
| [!UICONTROL &#x200B; 次性の理由 &#x200B;] | `subpotentReason` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) の配列 | ワクチンが弱い理由。 |
| [!UICONTROL &#x200B; 支援情報 &#x200B;] | `supportingInformation` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | 予防接種を支援する追加情報。 |
| [!UICONTROL &#x200B; ワクチンコード &#x200B;] | `vaccineCode` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 投与されるワクチンのコード。 |
| [!UICONTROL &#x200B; 有効期限 &#x200B;] | `expirationDate` | 日付 | ワクチンの有効期限。 |
| [!UICONTROL &#x200B; 次性である &#x200B;] | `isSubpotent` | ブール値 | ワクチンが次性かどうかを示す指標。 |
| [!UICONTROL &#x200B; ロット番号 &#x200B;] | `lotNumber` | 文字列 | ワクチンのロット番号。 |
| [!UICONTROL &#x200B; 発生日時 &#x200B;] | `occurenceDateTime` | 日時 | ワクチンの接種日。 |
| [!UICONTROL Occureence 文字列 &#x200B;] | `occurenceString` | 文字列 | ワクチンの接種日。 |
| [!UICONTROL プライマリSource] | `primarySource` | ブール値 | データがプライマリソースからキャプチャされたかどうかを示します。 |
| [!UICONTROL ステータス] | `status` | 文字列 | 予防接種の状態です。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `completed` </li> <li> `entered-in-error` </li> <li> `not-done` </li> |

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/immunization.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/immunization.schema.json)

## `performer` {#performer}

`performer` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ パフォーマンス構造 ](../../../images/healthcare/field-groups/immunization/performer.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; アクター &#x200B;] | `actor` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 実行していた個人または組織。 |
| [!UICONTROL 関数] | `function` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 実行されたパフォーマンスのタイプ。 |

## `programEligibility` {#program-eligibility}

`programEligibility` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![programEligibility structure](../../../images/healthcare/field-groups/immunization/program-eligibility.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL プログラム] | `program` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 実施要件が宣言されるプログラム。 |
| [!UICONTROL &#x200B; プログラムステータス &#x200B;] | `programStatus` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | プログラムに対する患者の実施要件ステータス。 |

## `protocolApplied` {#protocol-applied}

`protocolApplied` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![protocolApplied 構造体 ](../../../images/healthcare/field-groups/immunization/protocol-applied.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 権限 &#x200B;] | `authority` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | レコメンデーションを公開する責任者。 |
| [!UICONTROL &#x200B; 標的疾患 &#x200B;] | `targetDisease` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) の配列 | ワクチンが標的とする予防可能な疾患。 |
| [!UICONTROL &#x200B; 線量数 &#x200B;] | `doseNumber` | 文字列 | シリーズ内の線量数。 |
| [!UICONTROL &#x200B; シリーズ &#x200B;] | `series` | 文字列 | ワクチンシリーズの名前。 |
| [!UICONTROL &#x200B; 連続投与 &#x200B;] | `seriesDoses` | 文字列 | イミュニティの推奨投与回数。 |

## `reaction` {#reaction}

`reaction` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ 反応構造 ](../../../images/healthcare/field-groups/immunization/reaction.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 表示 &#x200B;] | `manifestation` | [[!UICONTROL &#x200B; コード化可能な参照 &#x200B;]](../data-types/codeable-concept.md) | 反応に関する追加情報。 |
| [!UICONTROL 日付] | `date` | 日時 | 反応が始まったとき。 |
| [!UICONTROL &#x200B; 報告 &#x200B;] | `reported` | 文字列 | 反応が自己報告されたかどうかを示します。 |
