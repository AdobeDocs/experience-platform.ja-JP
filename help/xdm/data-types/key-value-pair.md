---
title: キーと値のペアのデータタイプ
description: キーと値のペアの Experience Data Model （XDM）データタイプについて説明します。
exl-id: 2a1a7537-9019-4cf2-bfa1-9c760f9656dd
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '104'
ht-degree: 88%

---

# [!UICONTROL キーと値のペア]のデータタイプ

[!UICONTROL キーと値のペア]は、一般的なキーと値のペアの詳細を取り込んだ標準的な Experience Data Model（XDM）データタイプです。このデータタイプは、リスト変数の配列項目を記述するために、[[!UICONTROL Adobe Analytics の ExperienceEvent フル拡張機能]フィールドグループ](../field-groups/event/analytics-full-extension.md)で使用されます。

![キーと値のペアの構造](../images/data-types/key-value-pair.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `key` | 文字列 | 汎用の変数または値のキー（名前）です。 |
| `value` | 文字列 | 変数の値です。 |

{style="table-layout:auto"}

データタイプについて詳しくは、[パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/analytics/keyvalue.schema.json)を参照してください。
