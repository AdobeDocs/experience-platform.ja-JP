---
title: カスタムメタデータ詳細情報データタイプ
description: カスタムメタデータ詳細情報エクスペリエンスデータモデル (XDM) データタイプについて説明します。
source-git-commit: 65f3dcf1cacfbc4e8a598244810d238bd88f64bd
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 9%

---

# [!UICONTROL カスタムメタデータの詳細情報] データタイプ

[!UICONTROL カスタムメタデータの詳細情報] は標準のエクスペリエンスデータモデル (XDM) データタイプで、カスタムメタデータを保存するための構造を定義します。 以下を使用します。 [!UICONTROL カスタムメタデータの詳細情報] コンテンツやインタラクションに関連付けられたカスタムメタデータの名前や値などの詳細を取り込むためのデータタイプ。

![カスタムメタデータ詳細情報データタイプの図です。](../images/data-types/custom-metadata-details-information.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|--------------------------------------------|------------------|-----------|-----------------------------------------|
| [!UICONTROL カスタムメタデータフィールド名] | `name` | 文字列 | カスタムフィールドの名前。 |
| [!UICONTROL カスタムメタデータフィールド値] | `value` | 文字列 | カスタムフィールドの値。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/components/datatypes/custommetadatadetails.schema.json)
