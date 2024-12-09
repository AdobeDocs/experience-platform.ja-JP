---
title: 金額データ タイプ
description: マネーエクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
source-git-commit: 7f7de89a843d867d18ef84051eee69face0570a0
workflow-type: tm+mt
source-wordcount: '93'
ht-degree: 18%

---

# [!UICONTROL Money] データ型

[!UICONTROL Money] は、標準的なエクスペリエンスデータモデル（XDM）データタイプで、ある程度認識された通貨で経済的なユーティリティを提供します。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![Money データ型の構造 ](../../images/data-types/healthcare/money.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL 通貨] | `currency` | 文字列 | ISO 4217 通貨コード。 |
| [!UICONTROL 値] | `value` | Double | 数値。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/money.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/money.schema.json)
