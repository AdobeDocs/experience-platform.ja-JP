---
title: 投薬スキーマフィールドグループ
description: 薬スキーマフィールドグループについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: e1f53ff8-3079-4b2f-9e73-31a773907a63
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '284'
ht-degree: 7%

---

# [!UICONTROL &#x200B; 投薬 &#x200B;] スキーマフィールドグループ

[!UICONTROL &#x200B; 薬 &#x200B;] は、[[!DNL Medication]  クラス &#x200B;](../../../classes/medication.md) の標準スキーマフィールドグループです。 薬に関する情報を収集する単一のオブジェクトタイプのフィールド `healthcareMedication` を提供します。

![&#x200B; フィールドグループ構造 &#x200B;](../../../images/healthcare/field-groups/medication/medication.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| ---|  --- | --- | --- |
| [!UICONTROL バッチ] | `batch` | オブジェクト | パッケージ化された薬の詳細。 次の 2 つのプロパティが含まれます。 <li>`lotNumber`: バッチに割り当てられた識別子の文字列値。</li> <li>`expirationDate`: バッチの有効期限が切れる日時の値。</li> |
| [!UICONTROL コード] | `code` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | この薬を識別するコード。 |
| [!UICONTROL &#x200B; 定義 &#x200B;] | `definition` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 薬の定義。 |
| [!UICONTROL &#x200B; 剤形 &#x200B;] | `doseForm` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 錠剤やカプセル剤など、薬の服用形態を表します。 |
| [!UICONTROL 識別子] | `identifier` | [[!UICONTROL &#x200B; 識別子 &#x200B;]](../data-types/identifier.md) の配列 | 薬の識別子。 |
| [!UICONTROL &#x200B; 材料 &#x200B;] | `ingredient` | オブジェクトの配列 | 薬の成分情報を表します。 詳しくは、[&#x200B; 以下の節 &#x200B;](#ingredient) を参照してください。 |
| [!UICONTROL &#x200B; 製造販売業者 &#x200B;] | `marketingAuthorizationHolder` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 薬を販売する権限を持つ組織。 |
| [!UICONTROL &#x200B; 合計量 &#x200B;] | `totalVolume` | [[!UICONTROL &#x200B; 数量 &#x200B;]](../data-types/quantity.md) | 製品コードがパッケージサイズを推測しない場合に薬で提供される製品の量です。 |
| [!UICONTROL ステータス] | `status` | 文字列 | 投薬のステータス。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `active` </li> <li> `inactive` </li> <li> `entered-in-error` </li> |

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/medication.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/medication.schema.json)

## `ingredient` {#ingredient}

`ingredient` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![&#x200B; 成分構造 &#x200B;](../../../images/healthcare/field-groups/medication/ingredient.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; 項目 &#x200B;] | `item` | [[!UICONTROL &#x200B; コード化可能な参照 &#x200B;]](../data-types/codeable-reference.md) | 記載されている成分。 |
| [!UICONTROL &#x200B; 強度コード化可能な概念 &#x200B;] | `strengthCodeableConcept` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | システム定義の用語で表された、原料の存在量。 |
| [!UICONTROL &#x200B; 強度量 &#x200B;] | `strengthQuantity` | [[!UICONTROL &#x200B; 数量 &#x200B;]](../data-types/quantity.md) | 存在する原料の量。 |
| [!UICONTROL &#x200B; 強度比 &#x200B;] | `strengthRatio` | [[!UICONTROL &#x200B; 比率 &#x200B;]](../data-types/ratio.md) | 存在する原料の比率。 |
| [!UICONTROL &#x200B; アクティブ &#x200B;] | `isActive` | ブール値 | 原料が有効かどうかを示します。 |
