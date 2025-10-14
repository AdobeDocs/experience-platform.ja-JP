---
title: アカウントスキーマフィールドグループ
description: アカウントスキーマフィールドグループについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: 376716bd-f79f-421d-b163-0f0e50876b48
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '935'
ht-degree: 7%

---

# [!UICONTROL &#x200B; アカウント &#x200B;] スキーマフィールドグループ

[!UICONTROL &#x200B; アカウント &#x200B;] は、[[!DNL XDM Individual Profile]  クラス &#x200B;](../../../classes/individual-profile.md) および [[!DNL Provider class]](../../../classes/provider.md) の標準スキーマフィールドグループです。 患者または個人のグループに提供される医療サービスに関連するトランザクション、サービス、その他の財務情報（保険証券や請求目的など）を記録するために使用される、単一のオブジェクトタイプのフィールド `healthcareAccount` ータを提供します。

![&#x200B; フィールドグループ構造 &#x200B;](../../../images/healthcare/field-groups/account/account.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 残高 &#x200B;] | `balance` | オブジェクトの配列 | 財務システムによって計算および処理される勘定残高。 詳しくは、以下の [&#x200B; 節 &#x200B;](#balances) を参照してください。 |
| [!UICONTROL &#x200B; 請求の状況 &#x200B;] | `billingStatus` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | これにより、請求プロセスを通じてアカウントのライフサイクルが追跡されます。 これは、トランザクションがアカウントに割り当てられたときの処理方法を示します。 |
| [!UICONTROL &#x200B; 対象範囲 &#x200B;] | `coverage` | オブジェクトの配列 | このアカウントのコストをカバーする責任者と、その適用順序。 詳しくは、[&#x200B; 以下の節 &#x200B;](#coverage) を参照してください。 |
| [!UICONTROL 通貨] | `currency` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | アカウントのデフォルトの通貨。 |
| [!UICONTROL &#x200B; 診断 &#x200B;] | `diagnosis` | オブジェクトの配列 | 請求に関連する一連の診断は、請求を作成する処理の前に適切に順序付けることができるアカウントに保存されています。 詳しくは、[&#x200B; 以下の節 &#x200B;](#diagnosis) を参照してください。 |
| [!UICONTROL &#x200B; 保証人 &#x200B;] | `guarantor` | オブジェクトの配列 | 他の支払いオプションが不足している場合、アカウントのバランスを取る担当者。 詳しくは、[&#x200B; 以下の節 &#x200B;](#guarantor) を参照してください。 |
| [!UICONTROL 識別子] | `identifier` | [[!UICONTROL &#x200B; 識別子 &#x200B;]](../data-types/identifier.md) の配列 | アカウントの参照に使用される一意の ID。 人間による使用を目的としている場合としていない場合があります（例：クレジットカード番号）。 |
| [!UICONTROL &#x200B; 所有者 &#x200B;] | `owner` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | サービスエリア、病院、部門などを示します。 アカウントの管理を担当します。 |
| [!UICONTROL &#x200B; 手順 &#x200B;] | `procedure` | オブジェクトの配列 | 請求に関連する一連の手順は、請求を作成する処理の前に適切に順序付けることができるアカウントに保存されています。 詳しくは、[&#x200B; 以下の節 &#x200B;](#procedure) を参照してください。 |
| [!UICONTROL &#x200B; 関連アカウント &#x200B;] | `relatedAccount` | オブジェクトの配列 | このアカウントに関連するその他の関連アカウント。 詳しくは、[&#x200B; 以下の節 &#x200B;](#related-account) を参照してください。 |
| [!UICONTROL &#x200B; 供用期間 &#x200B;] | `servicePeriod` | [[!UICONTROL &#x200B; 期間 &#x200B;]](../data-types/period.md) | このアカウントに関連付けられているサービスの日付範囲。 |
| [!UICONTROL 件名] | `subject` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | 費用が発生するエンティティを識別します。 サービスまたは商品の即時の受領者は、対象に関連するエンティティである可能性がありますが、費用は最終的にアカウントの対象によって発生しました。 |
| [!UICONTROL タイプ] | `type` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | レポートおよび検索目的でアカウントを分類します。 |
| [!UICONTROL &#x200B; 計算日時 &#x200B;] | `calculatedAt` | 日時 | 残高が計算された時間。 |
| [!UICONTROL 説明] | `description` | 文字列 | アカウントの追跡内容と使用方法に関する追加情報を提供します。 |
| [!UICONTROL 名前] | `name` | 文字列 | アカウントの名前。 |
| [!UICONTROL ステータス] | `status` | 文字列 | アカウントのステータス。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `active` </li> <li> `inactive` </li> <li> `entered-in-error` </li> <li> `on-hold` </li> <li> `unknown`</li> |

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/account.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/account.schema.json)

## `balances` {#balances}

`balances` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![&#x200B; バランス構造 &#x200B;](../../../images/healthcare/field-groups/account/balance.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 集計 &#x200B;] | `aggregate` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 誰が残高のこの部分を支払うと期待されています。 |
| [!UICONTROL &#x200B; 量 &#x200B;] | `amount` | [[!UICONTROL &#x200B; 金額 &#x200B;]](../data-types/money.md) | 期間プロパティで定義された年齢に対して計算された実際の残高。 |
| [!UICONTROL &#x200B; 期間 &#x200B;] | `term` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | アカウントの期間。 |
| [!UICONTROL &#x200B; 見積り &#x200B;] | `estimate` | ブール値 | 金額が予測値の場合。 |

## `coverage` {#coverage}

`coverage` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![&#x200B; カバレッジ構造 &#x200B;](../../../images/healthcare/field-groups/account/coverage.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 対象範囲 &#x200B;] | `coverage` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | このアカウントのコストをカバーする責任者と、その適用順序。 |
| [!UICONTROL 優先度] | `priority` | 整数 | このアカウントのコンテキストでのカバレッジの優先度（最小値は `0`）。 |

## `diagnosis` {#diagnosis}

`diagnosis` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![&#x200B; 診断構造 &#x200B;](../../../images/healthcare/field-groups/account/diagnosis.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL 条件] | `condition` | [[!UICONTROL &#x200B; コード化可能な参照 &#x200B;]](../data-types/codeable-reference.md) | アカウントに関連する診断。 |
| [!UICONTROL &#x200B; パッケージコード &#x200B;] | `packageCode` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) の配列 | パッケージコードを使用すると、1 つの製品（DRG など）として価格設定または提供される可能性のある診断をグループ化できます。 |
| [!UICONTROL タイプ] | `type` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) の配列 | この診断がアカウントに関連していることを入力します（例：入場、請求、退会）。 |
| [!UICONTROL &#x200B; 診断年月日 &#x200B;] | `dateOfDiagnosis` | 日時 | 診断の日付（コーディングされた診断の場合）。 |
| [!UICONTROL &#x200B; 入院時 &#x200B;] | `onAdmission` | ブール値 | 入院時に診断が認められたかどうか。 |
| [!UICONTROL &#x200B; スクエンス &#x200B;] | `sequence` | 整数 | （タイプごとの）診断のランキング（最小値は `0`）。 |

## `guarantor` {#guarantor}

`guarantor` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![&#x200B; 保証人の構造 &#x200B;](../../../images/healthcare/field-groups/account/guarantor.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 当事者 &#x200B;] | `party` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 責任を負うエンティティ。 |
| [!UICONTROL &#x200B; 期間 &#x200B;] | `period` | [[!UICONTROL &#x200B; 期間 &#x200B;]](../data-types/period.md) | 保証人がアカウントの責任を受け入れる期間。 |
| [!UICONTROL &#x200B; 保留中 &#x200B;] | `onHold` | ブール値 | 保証人は、信用保留に置かれるか、その他の方法で一時的に役割を停止される場合があります。 |

## `procedure` {#procedure}

`procedure` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![&#x200B; プロシージャ構造 &#x200B;](../../../images/healthcare/field-groups/account/procedure.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL コード] | `code` | [[!UICONTROL &#x200B; コード化可能な参照 &#x200B;]](../data-types/codeable-reference.md) | アカウントに関連する手順。 |
| [!UICONTROL &#x200B; デバイス &#x200B;] | `device` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | アカウントに関連する手順に関連付けられたデバイス。 |
| [!UICONTROL タイプ] | `type` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) の配列 | 手続き値をアカウントの請求に使用する方法。 |
| [!UICONTROL &#x200B; パッケージコード &#x200B;] | `packageCode` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) の配列 | パッケージコードを使用して、1 つの製品（DRG など）として価格設定または配信できるプロシージャをグループ化できます。 |
| [!UICONTROL &#x200B; 送達年月日 &#x200B;] | `dateOfService` | 日時 | コード化されたプロシージャを使用する場合の日付。 プロシージャへの参照を使用する場合は、プロシージャの日付を使用する必要があります。 |
| [!UICONTROL &#x200B; シーケンス &#x200B;] | `sequence` | 整数 | （タイプごとの）プロシージャのランキング（最小値は `0`）。 |

## `relatedAccount` {#related-account}

`relatedAccount` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![relatedAccount 構造 &#x200B;](../../../images/healthcare/field-groups/account/related-account.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL アカウント] | `account` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 関連付けられたアカウントへの参照。 |
| [!UICONTROL 関係] | `relationship` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 関連付けられたアカウントの関係。 |
