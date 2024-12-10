---
title: 注釈データタイプ
description: 注釈エクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: f46b5fb6-d64a-4a37-91f6-b470599d9130
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '110'
ht-degree: 11%

---

# [!UICONTROL  注釈 ] データタイプ

[!UICONTROL  注釈 ] は、オーサーに対するアトリビューションを持つテキストノードを含む、標準のエクスペリエンスデータモデル（XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![ 注釈データタイプ構造 ](../../../images/healthcare/data-types/annotation.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL  オーサーリファレンス ] | `authorReference` | [[!UICONTROL  参考 ]](../data-types/reference.md) | 作成者への参照。 |
| [!UICONTROL  作成者 ] | `authorString` | 文字列 | 注釈に責任を持つ個人。 |
| [!UICONTROL テキスト] | `text` | 文字列 | 注釈のコンテンツ。 |
| [!UICONTROL 時間] | `time` | 日時 | 注釈が作成された時点。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/annotation.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/annotation.schema.json)
