---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；アドレス；xdm：アドレス；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: 商品リスト項目のデータタイプ
description: 製品リスト項目 XDM データタイプについて説明します。
exl-id: 056fdb5b-6782-4e29-9d62-90b270c05795
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '342'
ht-degree: 18%

---

# [!UICONTROL  製品リスト項目 ] データタイプ

[!UICONTROL  製品リスト項目 ] は、顧客が選択した製品を、特定の時点での特定のオプション、価格、使用コンテキストで表す標準 XDM データタイプです。

このデータタイプで取り込まれる値は、製品レコードとは異なる場合があります。 例えば、製品レコードには、すべての顧客に対して一貫性のある製品情報システムからの詳細が含まれています。製品リスト項目には、購入時に顧客に提供された実際の価格があり、これは販売キャンペーンや季節的な価格によって異なる場合があります。

![](../images/data-types/product-list-item.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `selectedOptions` | オブジェクトの配列 | 設定可能な製品用に選択されたカスタムオプションが含まれます。 各リスト項目は、次のプロパティを持つオブジェクトです。<ul><li>`attribute`：設定可能な属性の名前。</li><li>`value`：属性の値。</li></ul> |
| `SKU` | [!UICONTROL 文字列] | 最小在庫管理単位 (SKU)。ベンダーによって定義された商品の一意の ID。 |
| `_id` | [!UICONTROL 文字列] | この商品エントリの品目識別子。 製品自体は、`product` を通じて識別されます。 |
| `currencyCode` | [!UICONTROL 文字列] | 商品の価格設定に使用されるアルファベットの [ISO 4217](https://www.iso.org/iso-4217-currency-codes.html) 通貨コード。 |
| `discountAmount` | [!UICONTROL 倍精度浮動小数点] | 商品が割引される場合、これは商品の通常価格と特別価格の差を表します。 |
| `name` | [!UICONTROL 文字列] | この商品表示でユーザーに提示される商品の表示名。 |
| `priceTotal` | [!UICONTROL 倍精度浮動小数点] | 商品品目の合計価格。 |
| `product` | [!UICONTROL  文字列 ] （URI） | 製品自体をキャプチャする XDM スキーマの URI`$id`。 |
| `productAddMethod` | [!UICONTROL 文字列] | 訪問者によってリストに商品項目を追加するために使用されたメソッド。 |
| `productImageUrl` | [!UICONTROL 文字列] | 商品のメイン画像の URL。 |
| `quantity` | [!UICONTROL  整数 ] | 顧客が製品を必要と示した数量。 |
| `unitOfMeasureCode` | [!UICONTROL 文字列] | `quantity` プロパティに関連する商品の標準 [ 測定単位コード ](https://ucum.org/ucum)。 |

{style="table-layout:auto"}

住所データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/productlistitem.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/productlistitem.schema.json)
