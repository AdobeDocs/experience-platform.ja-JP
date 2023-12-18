---
title: カテゴリデータタイプ
description: カテゴリデータエクスペリエンスデータモデル (XDM) データタイプについて説明します。
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '98'
ht-degree: 7%

---

# [!UICONTROL カテゴリデータ] データタイプ

[!UICONTROL カテゴリデータ] は、製品のカテゴリに関連する情報を記述する標準の Experience Data Model(XDM) データ型です。

![カテゴリデータタイプの図です。](../images/data-types/category-data.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|-----------------|--------------------|-----------|------------------------------------------|
| [!UICONTROL カテゴリ識別子] | `categoryID` | 文字列 | 商品のカテゴリの識別子。 |
| [!UICONTROL カテゴリ名] | `categoryName` | 文字列 | 商品のカテゴリの名前。 |
| [!UICONTROL カテゴリパス] | `categoryPath` | 文字列 | 商品のカテゴリのパス。 |

{style="table-layout:auto"}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/categorydata.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/categorydata.schema.json)
