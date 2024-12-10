---
title: 投与量データタイプ
description: Dose Experience Data Model （XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: 56eda38b-a7f7-40da-af08-73cfe9db0c7e
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '353'
ht-degree: 6%

---

# [!UICONTROL  用量 ] データタイプ

[!UICONTROL  投与量 ] は、薬の服用方法や服用方法を説明する、標準的なエクスペリエンスデータモデル（XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![ 剤量データ型構造 ](../../../images/healthcare/data-types/dosage/dosage.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL  追加指示 ] | `additionalInstruction` | [[!UICONTROL  コード化可能な概念 ]](../data-types/codeable-concept.md) の配列 | 患者に対する指示または警告の追加。 |
| [!UICONTROL  必要に応じて ] | `asNeededFor` | [[!UICONTROL  コード化可能な概念 ]](../data-types/codeable-concept.md) の配列 | 必要に応じて薬を服用する必要がある問題を説明します。 |
| [!UICONTROL  用量および速度 ] | `doseAndRate` | オブジェクトの配列 | 投薬量、投薬量又は標準的な投薬量。 詳しくは、以下の [ 節 ](#dose-and-rate) を参照してください |
| [!UICONTROL 1 回当たりの最大投与量 ] | `maxDosePerAdministration` | [[!UICONTROL  単純量 ]](../data-types/simple-quantity.md) | 1 回の投与あたりの薬の上限。 |
| [!UICONTROL 1 生涯あたりの最大線量 ] | `maxDosePerLifetime` | [[!UICONTROL  単純量 ]](../data-types/simple-quantity.md) | 患者の生涯あたりの投薬の上限。 |
| [!UICONTROL 1 期間あたりの最大投与量 ] | `maxDosePerPeriod` | [[!UICONTROL Ratio]](../data-types/ratio.md) の配列 | 単位時間当たりの投薬の上限。 |
| [!UICONTROL 方法] | `method` | [[!UICONTROL  コード化可能な概念 ]](../data-types/codeable-concept.md) | 薬を投与するための技術。 |
| [!UICONTROL  ルート ] | `route` | [[!UICONTROL  コード化可能な概念 ]](../data-types/codeable-concept.md) | 薬が体に入る方法。 |
| [!UICONTROL  身体部位 ] | `site` | [[!UICONTROL  コード化可能な概念 ]](../data-types/codeable-concept.md) | 薬物を投与するための身体部位。 |
| [!UICONTROL  タイミング ] | `timing` | [[!UICONTROL  タイミング ]](../data-types/timing.md) | 薬を投与する必要がある場合。 |
| [!UICONTROL  必要に応じて ] | `asNeeded` | ブール値 | 薬が必要に応じて服用されるべきかどうかを示す指標。 |
| [!UICONTROL  患者の指示 ] | `patientInstruction` | 文字列 | 患者または消費者が理解する条件での指示。 |
| [!UICONTROL  シーケンス ] | `Integer` | [[!UICONTROL  コード化可能な概念 ]](../data-types/codeable-concept.md) | 投薬指示の順序。 |
| [!UICONTROL テキスト] | `text` | 文字列 | テキストの投与量の指示を計画します。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/dosage.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/dosage.schema.json)

## `doseAndRate` {#dose-and-rate}

`doseAndRate` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ 線量および線量構造 ](../../../images/healthcare/data-types/dosage/dose-and-rate.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL  線量量 ] | `doseQuantity` | [[!UICONTROL  単純量 ]](../data-types/simple-quantity.md) | 1 回の投薬量。 |
| [!UICONTROL  線量範囲 ] | `doseRange` | [[!UICONTROL 範囲]](../data-types/range.md) | 1 回の投薬量。 |
| [!UICONTROL  料率の数量 ] | `rateQuantity` | [[!UICONTROL  単純量 ]](../data-types/simple-quantity.md) | 単位時間当たりの投薬量。 |
| [!UICONTROL  レート範囲 ] | `rateRange` | [[!UICONTROL 範囲]](../data-types/range.md) | 単位時間当たりの投薬量。 |
| [!UICONTROL  レート比 ] | `rateRatio` | [[!UICONTROL  比率 ]](../data-types/ratio.md) | 単位時間当たりの投薬量。 |
| [!UICONTROL タイプ] | `type` | [[!UICONTROL  コード化可能な概念 ]](../data-types/codeable-concept.md) | 指定された投与量または速度。 |
