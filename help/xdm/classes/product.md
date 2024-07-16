---
title: 製品クラス
description: エクスペリエンスデータモデル（XDM）の Product クラスについて説明します。
exl-id: 911680ae-b761-4945-9ad3-0233eaea89b0
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 15%

---

# [!UICONTROL Product] クラス

エクスペリエンスデータモデル（XDM）の [!UICONTROL Product] クラスは、小売製品を定義するプロパティの最小セットをキャプチャします。

![](../images/classes/product.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `productListPrice` | [通貨](../data-types/currency.md) | セールおよび割引前の商品のデフォルトの価格を表します。 |
| `_id` | 文字列 | レコードに対してシステムで生成された一意の文字列識別子。 このフィールドは、個々のレコードの一意性を追跡、データの重複を防ぎ、ダウンストリームのサービスでそのレコードを検索するために使用されます。<br><br> このフィールドはシステムで生成されるので、データ取り込み時に明示的な値は指定されません。 ただし、必要に応じて、独自の一意の ID 値を指定することもできます。 |
| `productDescription` | 文字列 | 商品の説明。 |
| `productID` | 文字列 | 商品の一意の ID。 |
| `productLastModifiedDate` | 日時 | この製品が更新で最後に変更された日時の [RFC3339](https://datatracker.ietf.org/doc/html/rfc3339) タイムスタンプ。 |
| `productManufacturedDate` | 日時 | この製品が作成されたときの [RFC3339](https://datatracker.ietf.org/doc/html/rfc3339) タイムスタンプ。 |
| `productName` | 文字列 | 商品の名前。 |
| `productRating` | 文字列 | 商品のカスタマーレビュー評価。 |

{style="table-layout:auto"}

## 互換性のあるフィールドグループ {#field-groups}

Adobeでは、[!UICONTROL Product] クラスで使用するためのいくつかの標準フィールドグループを提供しています。 このクラスで一般的に使用されるフィールドグループは次のとおりです。

* [[!UICONTROL  製品カタログ ]](../field-groups/product/product-catalog.md)
* [[!UICONTROL  製品カテゴリ ]](../field-groups/product/product-category.md)
