---
title: 目標スキーマフィールドグループ
description: 目標スキーマフィールドグループについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: 87715274-cc9d-41da-9ca7-1634903b4e8f
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '506'
ht-degree: 6%

---

# [!UICONTROL &#x200B; 目標 &#x200B;] スキーマフィールドグループ

[!UICONTROL &#x200B; 目標 &#x200B;] は、[[!DNL XDM Individual Profile]  クラス ](../../../classes/individual-profile.md) および [[!DNL Provider class]](../../../classes/provider.md) の標準スキーマフィールドグループです。 これは、患者、グループ、組織のケアの意図した目的を説明する、単一のオブジェクトタイプフィールド `healthcareGoal` を提供します。

![ フィールドグループ構造 ](../../../images/healthcare/field-groups/goal/goal.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 達成状況 &#x200B;] | `achievementStatus` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | ターゲットに対する目標に向かう進行状況または欠如を表します。 |
| [!UICONTROL &#x200B; 住所 &#x200B;] | `addresses` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | 目標によって対処されることを意図した条件およびその他のヘルスレコード要素。 |
| [!UICONTROL カテゴリ] | `category` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) の配列 | 食事や行動など、目標のカテゴリを示します。 |
| [!UICONTROL 説明] | `description` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 目標を説明するコードまたはテキスト。 |
| [!UICONTROL 識別子] | `identifier` | [[!UICONTROL &#x200B; 識別子 &#x200B;]](../data-types/identifier.md) の配列 | リソースが更新されてサーバーからサーバーに伝播されても一定の状態が維持される、実行者またはその他のシステムによって、この目標に割り当てられたビジネス ID。 |
| [!UICONTROL &#x200B; 注 &#x200B;] | `note` | [[!UICONTROL Annotation]](../data-types/annotation.md) の配列 | 目標に関するコメント。 |
| [!UICONTROL &#x200B; 結果 &#x200B;] | `outcome` | [[!UICONTROL &#x200B; コード化可能な参照 &#x200B;]](../data-types/codeable-reference.md) の配列 | 目標のステータスが評価された際の変更（または変更の欠如）を識別します。 |
| [!UICONTROL 優先度] | `priority` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 目標の達成または達成に関連する、相互に合意された重要性レベルを識別します。 |
| [!UICONTROL ソース] | `source` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 患者や医療従事者など、目標のソースを示します。 |
| [!UICONTROL &#x200B; コード化可能な概念を開始 &#x200B;] | `startCodeableConcept` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | その後、目標を説得する必要があるイベント。 |
| [!UICONTROL &#x200B; 件名 |]`subject` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 目標が設定されているユーザー、グループ、または組織を識別します。 |
| [!UICONTROL Target] | `target` | オブジェクトの配列 | 目標の特定の手順のタイムラインを示します。 詳しくは、[ 以下の節 ](#target) を参照してください。 |
| [!UICONTROL &#x200B; コンティニュアス &#x200B;] | `continous` | ブール値 | 目標を達成した後、目標を維持するために継続的なアクティビティが必要かどうかを示します。 |
| [!UICONTROL &#x200B; ライフサイクルステータス &#x200B;] | `lifecycleStatus` | 文字列 | 目標のライフサイクルのステータス。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `proposed` </li> <li> `planned` </li> <li> `accepted` </li> <li> `active` </li> <li> `on-hold` </li> <li> `completed` </li> <li> `cancelled` </li> <li> `entered-in-error` </li> <li> `rejected` </li> |
| [!UICONTROL &#x200B; 開始日 &#x200B;] | `startDate` | 日付 | 目標の達成を開始する日付。 |
| [!UICONTROL &#x200B; 状況報告日 &#x200B;] | `statusDate` | 日付 | ステータスが作成された日時を識別します。 |
| [!UICONTROL &#x200B; ステータスの理由 &#x200B;] | `statusReason` | 文字列 | 現在のステータスの理由をキャプチャします。 |

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/goal.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/goal.example.1.json)

## `target` {#target}

`target` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ ターゲット構造 ](../../../images/healthcare/field-groups/goal/target.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; コード化可能な概念の詳細 &#x200B;] | `detailCodeableConcept` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 目標の達成を示すために達成されるターゲットコード。 |
| [!UICONTROL &#x200B; 詳細数量 &#x200B;] | `detailQuantity` | [[!UICONTROL &#x200B; 数量 &#x200B;]](../data-types/quantity.md) | 目標の達成を示すために達成されるターゲット数量。 |
| [!UICONTROL &#x200B; 詳細範囲 &#x200B;] | `detailRange` | [[!UICONTROL 範囲]](../data-types/range.md) | 目標の達成を示すために達成されるターゲット範囲。 |
| [!UICONTROL &#x200B; 詳細率 &#x200B;] | `detailRatio` | [[!UICONTROL &#x200B; 比率 &#x200B;]](../data-types/ratio.md) | 目標の達成を示すために達成されるターゲット比率。 |
| [!UICONTROL 測定] | `measure` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 値であるパラメーターがトラッキングされています。 |
| [!UICONTROL &#x200B; 詳細ブール値 &#x200B;] | `detailBoolean` | ブール値 | 目標の達成を示します。 |
| [!UICONTROL &#x200B; 詳細な整数 &#x200B;] | `detailInteger` | 整数 | 目標の達成を示すために達成されるターゲット番号。 |
| [!UICONTROL &#x200B; 詳細文字列 &#x200B;] | `detailString` | 文字列 | 目標の達成を示すために達成されるターゲット値。 |
| [!UICONTROL &#x200B; 期限日 &#x200B;] | `dueDate` | 日付 | ターゲットを満たす必要がある日付。 |
