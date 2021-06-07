---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；アドレス；xdm:address；データ型；データ型；
solution: Experience Platform
title: 製品リスト項目データタイプ
topic-legacy: overview
description: このドキュメントでは、製品リスト項目のXDMデータタイプの概要を説明します。
source-git-commit: b22dce52563d5f3bbd1796c11d7c7b2a49fa6d5f
workflow-type: tm+mt
source-wordcount: '289'
ht-degree: 14%

---

# [!UICONTROL 製品リスト項目] データタイプ

[!UICONTROL 製品リスト] 項目は、顧客が選択した製品を特定の時点の特定のオプション、価格、使用状況コンテキストで表す標準XDMデータ型です。

このデータタイプで取り込まれる値は、製品レコードと異なる場合があります。 例えば、製品レコードには、購入時に顧客に提供される実際の価格が製品リスト項目に含まれる、すべての顧客に対して一貫した製品情報システムの詳細が含まれます。販売キャンペーンや季節価格によって異なる場合があります。

![](../images/data-types/product-list-item.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `SKU` | [!UICONTROL 文字列] | 在庫管理単位(SKU)：ベンダーによって定義された製品の一意の識別子。 |
| `_id` | [!UICONTROL 文字列] | この製品エントリの品目識別子。 製品自体は`product`を通じて識別されます。 |
| `currencyCode` | [!UICONTROL 文字列] | 製品の価格設定に使用する[ISO 4217](https://www.iso.org/iso-4217-currency-codes.html)アルファベット通貨コード。 |
| `name` | [!UICONTROL 文字列] | この製品表示のユーザーに表示される、製品の表示名。 |
| `priceTotal` | [!UICONTROL Double] | 製品品目の合計価格。 |
| `product` | [!UICONTROL 文字列] (URI) | 製品自体を取り込むXDMスキーマのURI `$id`。 |
| `productAddMethod` | [!UICONTROL 文字列] | 訪問者が製品項目をリストに追加するために使用した方法。 |
| `quantity` | [!UICONTROL 整数] | 顧客が製品を必要と示した数量。 |

{style=&quot;table-layout:auto&quot;}

郵送先住所のデータタイプについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/productlistitem.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/productlistitem.schema.json)
