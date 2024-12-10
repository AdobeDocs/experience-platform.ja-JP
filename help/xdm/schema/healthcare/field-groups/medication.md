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

# [!UICONTROL  投薬 ] スキーマフィールドグループ

[!UICONTROL  薬 ] は、[[!DNL Medication]  クラス ](../../../classes/medication.md) の標準スキーマフィールドグループです。 薬に関する情報を収集する単一のオブジェクトタイプのフィールド `healthcareMedication` を提供します。

![ フィールドグループ構造 ](../../../images/healthcare/field-groups/medication/medication.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| ---|  --- | --- | --- |
| [!UICONTROL バッチ] | `batch` | オブジェクト | パッケージ化された薬の詳細。 次の 2 つのプロパティが含まれます。 <li>`lotNumber`: バッチに割り当てられた識別子の文字列値。</li> <li>`expirationDate`: バッチの有効期限が切れる日時の値。</li> |
| [!UICONTROL コード] | `code` | [[!UICONTROL  コード化可能な概念 ]](../data-types/codeable-concept.md) | この薬を識別するコード。 |
| [!UICONTROL  定義 ] | `definition` | [[!UICONTROL  参考 ]](../data-types/reference.md) | 薬の定義。 |
| [!UICONTROL  剤形 ] | `doseForm` | [[!UICONTROL  コード化可能な概念 ]](../data-types/codeable-concept.md) | 錠剤やカプセル剤など、薬の服用形態を表します。 |
| [!UICONTROL 識別子] | `identifier` | [[!UICONTROL  識別子 ]](../data-types/identifier.md) の配列 | 薬の識別子。 |
| [!UICONTROL  材料 ] | `ingredient` | オブジェクトの配列 | 薬の成分情報を表します。 詳しくは、[ 以下の節 ](#ingredient) を参照してください。 |
| [!UICONTROL  製造販売業者 ] | `marketingAuthorizationHolder` | [[!UICONTROL  参考 ]](../data-types/reference.md) | 薬を販売する権限を持つ組織。 |
| [!UICONTROL  合計量 ] | `totalVolume` | [[!UICONTROL  数量 ]](../data-types/quantity.md) | 製品コードがパッケージサイズを推測しない場合に薬で提供される製品の量です。 |
| [!UICONTROL ステータス] | `status` | 文字列 | 投薬のステータス。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `active` </li> <li> `inactive` </li> <li> `entered-in-error` </li> |

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/medication.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/medication.schema.json)

## `ingredient` {#ingredient}

`ingredient` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ 成分構造 ](../../../images/healthcare/field-groups/medication/ingredient.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL  項目 ] | `item` | [[!UICONTROL  コード化可能な参照 ]](../data-types/codeable-reference.md) | 記載されている成分。 |
| [!UICONTROL  強度コード化可能な概念 ] | `strengthCodeableConcept` | [[!UICONTROL  コード化可能な概念 ]](../data-types/codeable-concept.md) | システム定義の用語で表された、原料の存在量。 |
| [!UICONTROL  強度量 ] | `strengthQuantity` | [[!UICONTROL  数量 ]](../data-types/quantity.md) | 存在する原料の量。 |
| [!UICONTROL  強度比 ] | `strengthRatio` | [[!UICONTROL  比率 ]](../data-types/ratio.md) | 存在する原料の比率。 |
| [!UICONTROL  アクティブ ] | `isActive` | ブール値 | 原料が有効かどうかを示します。 |
