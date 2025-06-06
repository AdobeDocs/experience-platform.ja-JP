---
title: カバレッジスキーマフィールドグループ
description: カバレッジ スキーマフィールドグループについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: 7b84c0cf-3bd4-4ba8-a8cc-85e6b3f2b59e
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '759'
ht-degree: 6%

---

# [!UICONTROL &#x200B; カバレッジ &#x200B;] スキーマフィールドグループ

[!UICONTROL Coverage] は、[[!DNL Plan] class](../../../classes/plan.md) の標準スキーマフィールドグループです。 保険プランの高レベルの識別子と記述子、通常は保険カードに表示される情報を提供することを目的とした、単一のオブジェクトタイプのフィールド `healthcareCoverage` ールが提供されます。この情報は、医療製品およびサービスの提供に対する支払いの一部または全部に使用できます。

![ フィールドグループ構造 ](../../../images/healthcare/field-groups/coverage/coverage.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; プラン受益者 &#x200B;] | `beneficiary` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 商品またはサービスが提供された場合に保険の給付を受ける当事者および患者。 |
| [!UICONTROL クラス] | `class` | オブジェクトの配列 | アンダーライター固有の分類子のスイート。 詳しくは、[ 以下の節 ](#class) を参照してください。 |
| [!UICONTROL &#x200B; 連絡先 &#x200B;] | `contract` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | この保険契約を構成する保険証券。 |
| [!UICONTROL &#x200B; 受益者の費用等 &#x200B;] | `costToBeneficiary` | オブジェクトの配列 | コストカテゴリと関連金額を示すコードのスイート。ポリシーに詳しく記載されており、ヘルスカードに含まれている場合があります。 詳しくは、[ 以下の節 ](#cost-to-beneficiary) を参照してください。 |
| [!UICONTROL &#x200B; 例外 &#x200B;] | `exception` | オブジェクトの配列 | 患者のコストとその有効期間に対する例外または削減を示すコードのスイート。 詳しくは、[ 以下の節 ](#exception) を参照してください。 |
| [!UICONTROL 識別子] | `identifier` | [[!UICONTROL &#x200B; 識別子 &#x200B;]](../data-types/identifier.md) の配列 | 保険業者によって発行された補償範囲の識別子。 |
| [!UICONTROL &#x200B; 保険計画 &#x200B;] | `insurancePlan` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 保険プランの詳細、給付および費用は、この保険の補償範囲を構成します。 |
| [!UICONTROL &#x200B; 保険者 &#x200B;] | `insurer` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | プログラムまたはプラン引受人、支払者または保険会社。 |
| [!UICONTROL &#x200B; 支払期限 &#x200B;] | `paymentBy` | オブジェクトの配列 | 支払い当事者へのリンク、およびオプションで支払いに責任があるもの。 詳しくは、[ 以下の節 ](#payment-by) を参照してください。 |
| [!UICONTROL &#x200B; 補償開始日および終了日 &#x200B;] | `period` | [[!UICONTROL &#x200B; 期間 &#x200B;]](../data-types/period.md) | カバレッジがアクティブである期間。 開始日が指定されていない場合は、開始日が不明であることを示します。終了日が指定されていない場合は、カバレッジが進行中であることを示します。 |
| [!UICONTROL &#x200B; 保険契約者 &#x200B;] | `policyHolder` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 保険証券を保有する当事者。 |
| [!UICONTROL &#x200B; 受益関係 &#x200B;] | `relationship` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | サブスクライバーに対する受取人の関係。 |
| [!UICONTROL &#x200B; 購読者 &#x200B;] | `subscriber` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | ポリシーとの契約関係を持つ関係者。 |
| [!UICONTROL &#x200B; サブスクライバー識別子 &#x200B;] | `subscriberId` | [[!UICONTROL &#x200B; 識別子 &#x200B;]](../data-types/identifier.md) の配列 | 保険業者がサブスクライバーの ID を割り当てた。 |
| [!UICONTROL タイプ] | `type` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | カバレッジのタイプ。 |
| [!UICONTROL &#x200B; 扶養家族の数 &#x200B;] | `dependent` | 文字列 | 補償範囲の下の扶養家族の指定。 |
| [!UICONTROL &#x200B; 種類 &#x200B;] | `kind` | 文字列 | カバレッジの種類。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `insurance` </li> <li> `self-pay` </li> <li> `other` </li> |
| [!UICONTROL &#x200B; 保険者ネットワーク &#x200B;] | `network` | 文字列 | 受取人がネットワーク内の料金でカバーされる治療を求めることができるプロバイダーのネットワーク、それ以外の場合は、ネットワーク外の契約条件が適用されます。 |
| [!UICONTROL &#x200B; 適用命令 &#x200B;] | `order` | 整数 | カバレッジの相対的な順序（最小値は `0`）。 |
| [!UICONTROL ステータス] | `status` | 文字列 | カバレッジのステータス。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `active` </li> <li> `cancelled` </li> <li> `draft` </li> <li> `entered-in-error` </li> |
| [!UICONTROL &#x200B; 代位 &#x200B;] | `subrogation` | ブール値 | `true` の場合、この保険インスタンスは裁定のためではなく、保険会社に費用を回収するための詳細を提供するために含まれています。 |

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/coverage.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/coverage.schema.json)

## `class` {#class}

`class` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ クラス構造 ](../../../images/healthcare/field-groups/coverage/class.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL タイプ] | `type` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) の配列 | 保険業者固有のクラスラベル、または番号とオプションの名前が提供される分類のタイプ。 例えば、タイプは、補償のクラス、雇用主グループ、ポリシー、またはプランを識別するために使用できます。 |
| [!UICONTROL 値] | `value` | [[!UICONTROL 識別子]](../data-types/identifier.md) | 保険業者が発行したラベルに関連付けられた英数字の識別子。 |
| [!UICONTROL 名前] | `name` | 文字列 | クラスの簡単な説明。 |

## `costToBeneficiary` {#cost-to-beneficiary}

`costToBeneficiary` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ 受取人コスト構造 ](../../../images/healthcare/field-groups/coverage/cost-to-beneficiary.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL カテゴリ] | `category` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 製品とサービスが提供される一般的な特典の種類を識別するためのコード。 |
| [!UICONTROL ネットワーク] | `network` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 利点がネットワーク内プロバイダーとネットワーク外プロバイダーのどちらを参照するかを示すコード。 |
| [!UICONTROL &#x200B; 期間 &#x200B;] | `term` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 最大ライフタイムメリットなどの値の期間。 |
| [!UICONTROL タイプ] | `type` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 治療に関連する患者中心のコストのカテゴリ。 |
| [!UICONTROL &#x200B; 単位 &#x200B;] | `unit` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 福利厚生が個人に適用されるか、ファミリに適用されるかを示します。 |

## `exception` {#exception}

`exception` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ 例外構造 ](../../../images/healthcare/field-groups/coverage/exception.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL タイプ] | `type` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 特定の例外のコード。 |
| [!UICONTROL &#x200B; 期間 &#x200B;] | `period` | [[!UICONTROL &#x200B; 期間 &#x200B;]](../data-types/period.md) | 例外がアクティブである期間。 |

## `paymentBy` {#payment-by}

`paymentBy` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ 組織内支払 ](../../../images/healthcare/field-groups/coverage/payment-by.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 当事者 &#x200B;] | `party` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 治療費用の非保険支払いを提供する関係者のリスト。 |
| [!UICONTROL &#x200B; 責務 &#x200B;] | `responsibility` | 文字列 | 財務責任の説明。 |
