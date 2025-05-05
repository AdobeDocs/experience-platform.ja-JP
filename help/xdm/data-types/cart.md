---
title: 買い物かごデータタイプ
description: 買い物かごエクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: 24ae3882-60f3-4962-b0b5-7dba48170da8
source-git-commit: 8be502c9eea67119dc537a5d63a6c71e0bff1697
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 15%

---

# [!UICONTROL &#x200B; 買い物かご &#x200B;] データタイプ

[!UICONTROL &#x200B; 買い物かご &#x200B;] は、買い物かごに関連するプロパティを提供する、標準のエクスペリエンスデータモデル（XDM）データタイプです。 このデータタイプを使用して、販売者（`Cart ID`）およびソース（`Cart Source`）によって割り当てられた、1 つ以上の製品が買い物かごに追加された一意の ID をキャプチャします。

![[!UICONTROL &#x200B; 買い物かご &#x200B;] データタイプの図。](../images/data-types/cart.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|----------------|-------------------|-----------|------------------------------------------------------------|
| [!UICONTROL &#x200B; 買い物かご ID] | `cartID` | 文字列 | 販売者が買い物かごに割り当てた一意の ID。 |
| [!UICONTROL &#x200B; 買い物かごのSource] | `cartSource` | 文字列 | 1 つ以上の商品が買い物かごに追加された場所。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/cart.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/cart.schema.json)
