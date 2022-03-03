---
title: 製品クラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の Product クラスの概要を説明します。
source-git-commit: c0437b8f9d93c46dbec991a33a893a5b9e0cdf2c
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 15%

---

# [!UICONTROL 製品] クラス

エクスペリエンスデータモデル (XDM) では、 [!UICONTROL 製品] クラスは、製品を定義するプロパティの最小セットをキャプチャします。

![](../images/classes/product.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `productListPrice` | [通貨](../data-types/currency.md) | 販売および割引前の商品のデフォルト価格を表します。 |
| `_id` | 文字列 | レコードの、システムで生成される一意の文字列識別子。 このフィールドは、個々のレコードの一意性を追跡し、データの重複を防ぎ、ダウンストリームサービスでそのレコードを検索するために使用します。<br><br>このフィールドはシステムで生成されるので、データの取り込み中に明示的な値は指定されません。 ただし、必要に応じて独自の一意の ID 値を指定することもできます。 |
| `productDescription` | 文字列 | 製品の説明。 |
| `productID` | 文字列 | 商品の一意の ID。 |
| `productLastModifiedDate` | 日時 | An [RFC3339](https://datatracker.ietf.org/doc/html/rfc3339) 更新でこの製品が最後に変更された日時のタイムスタンプ。 |
| `productManufacturedDate` | 日時 | An [RFC3339](https://datatracker.ietf.org/doc/html/rfc3339) この製品が作成された日時のタイムスタンプ。 |
| `productName` | 文字列 | 製品名。 |
| `productRating` | 文字列 | 製品の顧客レビュー評価。 |

{style=&quot;table-layout:auto&quot;}

## 互換性のあるフィールドグループ {#field-groups}

Adobeには、 [!DNL XDM Individual Profile] クラス。 次に、クラスで一般的に使用されるフィールドグループのリストを示します。

* [[!UICONTROL 製品カタログ]](../field-groups/product/product-catalog.md)
* [[!UICONTROL 製品カテゴリ]](../field-groups/product/product-category.md)
