---
title: タイミングデータタイプ
description: タイミングエクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: e1bc16ed-4dd8-4316-b3c8-88d49d393859
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '111'
ht-degree: 8%

---

# [!UICONTROL &#x200B; タイミング &#x200B;] データタイプ

[!UICONTROL &#x200B; タイミング &#x200B;] は、定期的に発生する可能性のあるイベントに関する情報を提供するタイミングスケジュールを記述する、標準の Experience Data Model （XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![&#x200B; タイミングデータタイプの構造 &#x200B;](../../../images/healthcare/data-types/timing.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL &#x200B; イベント &#x200B;] | `event` | DateTime の配列 | イベントが発生するタイミング。 |
| [!UICONTROL &#x200B; 繰り返し &#x200B;] | `repeat` | [[!UICONTROL &#x200B; 繰り返し &#x200B;]](../data-types/repeat.md) | イベントが発生するタイミングに関する情報。 |
| [!UICONTROL コード] | `code` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | イベントに関連するコード。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/timing.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/timing.schema.json)
