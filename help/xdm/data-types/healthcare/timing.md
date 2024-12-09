---
title: タイミングデータタイプ
description: タイミングエクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
source-git-commit: 0640d0c2daab67ad7a77d3265ccae45851ba45b8
workflow-type: tm+mt
source-wordcount: '111'
ht-degree: 8%

---

# [!UICONTROL  タイミング ] データタイプ

[!UICONTROL  タイミング ] は、定期的に発生する可能性のあるイベントに関する情報を提供するタイミングスケジュールを記述する、標準の Experience Data Model （XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![ タイミングデータタイプの構造 ](../../images/data-types/healthcare/timing.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL  イベント ] | `event` | DateTime の配列 | イベントが発生するタイミング。 |
| [!UICONTROL  繰り返し ] | `repeat` | [[!UICONTROL  繰り返し ]](../healthcare/repeat.md) | イベントが発生するタイミングに関する情報。 |
| [!UICONTROL コード] | `code` | [[!UICONTROL  コード化可能な概念 ]](../healthcare/codeable-concept.md) | イベントに関連するコード。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/timing.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/timing.schema.json)
