---
title: コーディングデータタイプ
description: コーディングエクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: 23b789da-1feb-4001-8268-f0d7e2e8563b
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 10%

---

# [!UICONTROL  コーディング ] データタイプ

[!UICONTROL  コーディング ] は、用語システムで定義されたコードへの参照を記述する標準の Experience Data Model （XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![ コーディングデータタイプの構造 ](../../../images/healthcare/data-types/coding.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL コード] | `code` | 文字列 | システムで定義される構文の記号。 |
| [!UICONTROL  表示 ] | `display` | 文字列 | システムによって定義された表示域。 |
| [!UICONTROL  システム ] | `system` | 文字列 | 識別子の値の名前空間。URI として表されます。 |
| [!UICONTROL  ユーザーによって選択されています ] | `userSelected` | ブール値 | このコーディングがユーザーによって選択されたかどうかを示す識別子。 デフォルト値は false です。 |
| [!UICONTROL バージョン] | `version` | 文字列 | システムのバージョン。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/coding.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/coding.schema.json)
