---
title: 要求リストのデータ タイプ
description: 購買依頼リストエクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: cbea6b08-9d4d-4cbe-b0c5-506bccc6df67
source-git-commit: 8be502c9eea67119dc537a5d63a6c71e0bff1697
workflow-type: tm+mt
source-wordcount: '125'
ht-degree: 7%

---

# [!UICONTROL &#x200B; 購買依頼一覧 &#x200B;] データ タイプ

[!UICONTROL &#x200B; 購買依頼リスト &#x200B;] は、調達または購入のためにキュレートされた品目のコレクションを記述する、標準の Experience Data Model （XDM）データタイプです。 [!UICONTROL &#x200B; 購買依頼リスト &#x200B;] データ・タイプを使用して、購買依頼リストを識別および記述します。

![[!UICONTROL &#x200B; 購買依頼リスト &#x200B;] データ型のダイアグラム ](../images/data-types/requisition-list.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|---------------------------|-------------------|-----------|--------------------------------------------------|
| [!UICONTROL &#x200B; 購買依頼リスト ID] | `ID` | 文字列 | 要求リストの一意の ID。 |
| [!UICONTROL &#x200B; 購買依頼一覧名 &#x200B;] | `name` | 文字列 | 顧客によって指定された購買依頼リストの名称。 |
| [!UICONTROL &#x200B; 購買依頼一覧摘要 &#x200B;] | `description` | 文字列 | 顧客によって指定された要求リストの説明。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/requisitionlist.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/requisitionlist.schema.json)
