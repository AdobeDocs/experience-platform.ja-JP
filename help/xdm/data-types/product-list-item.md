---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；スキーマ；アドレス；xdm:address；データ型；データ型；
solution: Experience Platform
title: 製品リスト項目データタイプ
description: このドキュメントでは、製品リスト項目の XDM データ型の概要を説明します。
exl-id: 056fdb5b-6782-4e29-9d62-90b270c05795
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 14%

---

# [!UICONTROL 製品リスト項目] データタイプ

[!UICONTROL 製品リスト項目] は、特定の時点に対する特定のオプション、価格、使用状況コンテキストを持つ顧客が選択した製品を表す標準 XDM データ型です。

このデータタイプで取り込まれる値は、製品レコードとは異なる場合があります。 例えば、商品レコードには、すべての顧客に対して一貫した商品情報システムの詳細が含まれます。商品リスト項目には、購入時に顧客に提供された実際の価格が含まれますが、販売キャンペーンや季節ごとの価格によって異なる場合があります。

![](../images/data-types/product-list-item.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `selectedOptions` | オブジェクトの配列 | 設定可能な製品に対して選択したカスタムオプションが含まれます。 各リスト項目は、次のプロパティを持つオブジェクトです。<ul><li>`attribute`:設定可能な属性の名前。</li><li>`value`:属性の値。</li></ul> |
| `SKU` | [!UICONTROL 文字列] | 在庫管理単位 (SKU)。ベンダーによって定義された製品の一意の ID。 |
| `_id` | [!UICONTROL 文字列] | この製品エントリの品目識別子。 製品自体は、を通じて識別されます。 `product`. |
| `currencyCode` | [!UICONTROL 文字列] | この [ISO 4217](https://www.iso.org/iso-4217-currency-codes.html) 商品の価格設定に使用するアルファベットの通貨コード。 |
| `discountAmount` | [!UICONTROL 倍精度浮動小数点] | 製品が割引されている場合、これは通常価格と製品の特別価格の差を表します。 |
| `name` | [!UICONTROL 文字列] | この製品表示のユーザーに提示された製品の表示名。 |
| `priceTotal` | [!UICONTROL 倍精度浮動小数点] | 製品品目の合計価格。 |
| `product` | [!UICONTROL 文字列] (URI) | URI `$id` 製品自体を取り込む XDM スキーマのデータバインディングを作成します。 |
| `productAddMethod` | [!UICONTROL 文字列] | 訪問者が製品項目をリストに追加するために使用した方法。 |
| `productImageUrl` | [!UICONTROL 文字列] | 商品のメイン画像の URL。 |
| `quantity` | [!UICONTROL 整数] | 顧客が製品を必要と示した数量。 |
| `unitOfMeasureCode` | [!UICONTROL 文字列] | 標準 [測定単位コード](https://ucum.org/ucum) 製品の `quantity` プロパティ。 |

{style="table-layout:auto"}

郵送先住所のデータタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/productlistitem.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/productlistitem.schema.json)
