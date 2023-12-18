---
title: 買い物かごのデータタイプ
description: 買い物かごエクスペリエンスデータモデル (XDM) のデータタイプについて説明します。
source-git-commit: c3590dc2cfe47eb634136eeb88578f965598760d
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 5%

---

# [!UICONTROL 買い物かご] データタイプ

[!UICONTROL 買い物かご] は、買い物かごに関連するプロパティを提供する標準の Experience Data Model(XDM) データタイプです。 このデータ型を使用して、販売者によって割り当てられた一意の ID(`Cart ID`) とソース (`Cart Source`) で、1 つ以上の製品が買い物かごに追加されました。

![の図 [!UICONTROL 買い物かご] データタイプ。](../images/data-types/cart.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|----------------|-------------------|-----------|------------------------------------------------------------|
| [!UICONTROL 買い物かご ID] | `cartID` | 文字列 | 販売者が買い物かごに割り当てた一意の ID。 |
| [!UICONTROL 買い物かごのソース] | `cartSource` | 文字列 | 1 つ以上の製品が買い物かごに追加された場所。 |

{style="table-layout:auto"}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/cart.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/cart.schema.json)
