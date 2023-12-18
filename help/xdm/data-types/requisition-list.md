---
title: 購買依頼リスト・データ・タイプ
description: 購買依頼リストエクスペリエンスデータモデル (XDM) のデータタイプについて説明します。
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '125'
ht-degree: 5%

---

# [!UICONTROL 購買依頼リスト] データタイプ

[!UICONTROL 購買依頼リスト] は、調達または購入のためのキュレーションされた品目のコレクションを記述する標準の Experience Data Model(XDM) データタイプです。 以下を使用します。 [!UICONTROL 購買依頼リスト] 購買依頼リストを識別および記述するデータ・タイプ。

![の図 [!UICONTROL 購買依頼リスト] データタイプ。](../images/data-types/requisition-list.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|---------------------------|-------------------|-----------|--------------------------------------------------|
| [!UICONTROL 購買依頼リスト ID] | `ID` | 文字列 | 購買依頼リストの一意の ID。 |
| [!UICONTROL 購買依頼リスト名] | `name` | 文字列 | 顧客が指定した購買依頼リストの名前。 |
| [!UICONTROL 購買依頼リスト摘要] | `description` | 文字列 | 顧客が指定した購買依頼リストの説明。 |

{style="table-layout:auto"}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/requisitionlist.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/requisitionlist.schema.json)
