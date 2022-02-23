---
title: キーと値のペアのデータタイプ
description: このドキュメントでは、キー値ペアエクスペリエンスデータモデル (XDM) データタイプの概要を説明します。
source-git-commit: bf815eb014dc87f74ba3d42478eadcb1e8144c3c
workflow-type: tm+mt
source-wordcount: '121'
ht-degree: 7%

---

# [!UICONTROL キーと値のペア] データタイプ

[!UICONTROL キーと値のペア] は、汎用のキー値ペアの詳細を取り込む標準的な Experience Data Model(XDM) データ型です。 このデータタイプは、 [[!UICONTROL Adobe Analytics ExperienceEvent Full 拡張機能] フィールドグループ](../field-groups/event/analytics-full-extension.md) を使用して、list 変数の配列項目を記述します。

![キーと値のペアの構造](../images/data-types/key-value-pair.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `key` | 文字列 | 汎用変数または値のキー（名前）。 |
| `value` | 文字列 | 変数の値。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/analytics/keyvalue.schema.json).
