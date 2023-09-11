---
title: Adobe Experience Platform Web SDK を使用して、コマース、製品、注文に関する情報を収集する
description: Adobe Experience Platform Web SDK を使用して、製品や買い物かごに関連するデータを追加する方法について説明します。
source-git-commit: cb47f70fe75eb0dfe26fb3c3557658cf6cff5a17
workflow-type: tm+mt
source-wordcount: '1109'
ht-degree: 27%

---


# コマース、製品、注文情報の収集

組織が製品やサービスを販売している場合は、このページを使用して、製品やサービスの追跡方法を確認できます。

このページでは XDM を使用します [コマーススキーマ](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md) フィールドグループを使用します。

このフィールドグループは、次の 2 つの主要な部分で構成されます。

* The `commerce` オブジェクト。 このオブジェクトを使用すると、 `productListItems` 配列。
* The `productListItems` 配列。

>[!TIP]
>
>Adobe Analyticsに詳しい方は、 `commerce` オブジェクトには、 `events` 変数を使用します。 The `productListItems` オブジェクト配列には、 `products` 変数を使用します。

## The `commerce` object {#commerce-object}

この節では、 `commerce` オブジェクト。

>[!TIP]
>
>測定には、2 つのフィールド（`id` と `value`）があります。ほとんどの場合、 `value` フィールド ( 例： `'value':1`) をクリックします。 The `id` 「 」フィールドでは、測定が送信された際の追跡用の一意の識別子を設定できます。 詳しくは、 XDM のドキュメントを参照してください。 [測定](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/measure.schema.md) を参照してください。

| 測定 | 推奨 | 説明 |
|---|---|---|
| [`cartAbandons`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmcartabandons) | オプション | ユーザーが買い物かごにアクセスできなくなった、または購入できなくなった。 |
| [`checkouts`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmcheckouts) | 強く推奨 | ユーザーは製品を閲覧しなくなったが、製品の購入処理を進めている。 |
| [`productListAdds`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmproductlistadds) | 強く推奨 | 製品がリストに追加されている。必ず、`productListItems` で製品を同時に設定してください。 |
| [`productListOpens`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmproductlistopens) | オプション | 新しい製品リストが作成されます。例えば、新しい買い物かごが作成されるとします。 |
| [`productListRemovals`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmproductlistremovals) | 強く推奨 | 製品が製品リストから削除されています。 |
| [`productListReopens`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmproductlistreopens) | オプション | 製品リストがユーザーによって再アクティブ化されています。このアクションは、リマーケティングキャンペーンで頻繁に発生します。 |
| [`productListViews`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmproductlistviews) | 強く推奨 | 製品のリストが表示されている。 |
| [`productViews`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmproductviews) | 強く推奨 | 製品の表示が発生しました。 `productListItems` で表示する製品は必ず設定してください 。 |
| [`purchases`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmpurchases) | 強く推奨 | 注文を受け付けている。製品リストが必要です。 |
| [`saveForLaters`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmsaveforlaters) | オプション | 後で使用するために製品を保存します。 |

{style="table-layout:auto"}

### `Commerce` オブジェクトの例

以下の節を展開して、 `commerce` オブジェクト。

+++`productViews`

基本的な Web SDK `sendEvent` を呼び出し、 `productViews` ～に向かって `1`:

```javascript
alloy("sendEvent", {
  "xdm":{
    "commerce":{
      "productViews":{
        "value":1
      }
    }
  }
});
```

+++

## The `order` object {#order-object}

The `commerce` オブジェクトには、注文の詳細を収集するための専用のオブジェクトが含まれています。 これは、 `order` オブジェクト。

この節では、 `order` オブジェクト。

| フィールド | オプション | 推奨 | 説明 |
|---|---|---|---|
| [`currencyCode`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/order.schema.md#xdmcurrencycode) |  |  | 注文合計の [ISO 4217](https://ja.wikipedia.org/wiki/ISO_4217) 通貨。 |
| [`payments[]`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/order.schema.md#xdmpayments) |  |  | 注文の支払の一覧。[paymentItem](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/paymentitem.schema.md) には、次が含まれます。 |
|  | [`currencyCode`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/paymentitem.schema.md#xdmcurrencycode) | オプション | この支払い方法の [ISO 4217](https://ja.wikipedia.org/wiki/ISO_4217) 通貨。 |
|  | [`paymentAmount`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/paymentitem.schema.md#xdmpaymentamount) | 強く推奨 | 指定した通貨コードでの支払金額。 |
|  | [`paymentType`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/paymentitem.schema.md#xdmpaymenttype) | 強く推奨 | 支払のタイプ（例：`credit_card`、`gift_card`、`paypal`）。詳しくは、[既知の値](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/paymentitem.schema.md#xdmpaymenttype-known-values)のリストを参照。 |
|  | [`transactionID`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/paymentitem.schema.md#xdmtransactionid) | オプション | この支払トランザクションの一意の ID。 |
| [`priceTotal`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/order.schema.md#xdmpricetotal) |  | 強く推奨 | すべての割引と税金が適用された後の、この注文の合計。 |
| [`purchaseID`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/order.schema.md#xdmpurchaseid) |  | 強く推奨 | 販売者がこの購入に割り当てた一意の ID。 |
| [`purchaseOrderNumber`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/order.schema.md#xdmpurchaseordernumber) |  | オプション | 購入者がこの購入に割り当てた一意の ID。 |

### Order オブジェクトの例

以下の節を展開して、 `commerce` オブジェクト。

+++`Order` オブジェクトの例

Web SDK `sendEvent` を呼び出し、 `order` 複数の製品に適用されるオブジェクト `productListItems` 配列：

```javascript
alloy("sendEvent",{
  "xdm":{
    "commerce":{
      "order":{
        "purchaseID":"123456789",
        "currencyCode":"USD",
        "priceTotal":39.98,
        "payments":[
          {
            "transactionID":"amx12345",
            "paymentAmount":39.98,
            "paymentType":"credit_card",
            "currencyCode":"USD"
          }
        ]
      }
    },
    "productListItems":[
      {
        "SKU":"HT105",
        "name":"The Big Floppy Hat",
        "priceTotal":29.99,
        "quantity":1
      },
      {
        "SKU":"HT104",
        "name":"The Small Floppy Hat",
        "priceTotal":9.99,
        "quantity":1
      }
    ]
  }
});
```

+++

## 製品リストオブジェクト {#product-list-object}

製品リストには、対応するアクションに関連する製品が示されます。[productListItems](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md) のリストです。各製品には、複数のオプションフィールドがあります。

| フィールド | 推奨 | 説明 |
|---|---|---|
| [`currencyCode`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md#xdmcurrencycode) | オプション | The [ISO 4217](https://ja.wikipedia.org/wiki/ISO_4217) 通貨を設定します。 このフィールドは、通常、異なる通貨コードを持つ製品リストに複数の製品がある場合にのみ適用されます。 |
| [`priceTotal`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md#xdmpricetotal) | 強く推奨 | このフィールドは、該当する場合にのみ設定します。 例えば、 `productView` 製品の異なるバリエーションは、異なる価格を持つことができますが、 `productListAdds` イベント。 |
| [`product`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md#xdmproduct) | 強く推奨 | 製品の XDM ID。 |
| [`productAddMethod`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md#xdmproductaddmethod) | 強く推奨 | 訪問者が製品項目をリストに追加するために使用した方法。次で設定： `productListAdds` はを測定し、製品がリストに追加された場合にのみ使用します。 例として、`add to cart button`、`quick add`、および `upsell` があります。 |
| [`productName`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md#xdmname) | 強く推奨 | 製品の表示名または人間が読み取り可能な名前。 |
| [`quantity`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md#xdmquantity) | 強く推奨 | 顧客が製品を必要と示した数量。`productListAdds`、`productListRemoves`、`purchases`、`saveForLaters`、などで設定する必要があります。 |
| [`SKU`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md#xdmsku) | 強く推奨 | 店舗在庫単位（Store Keeping Unit）。製品の一意の ID です。 |

### 製品リストの例

以下のセクションを展開して、 `productListItems` オブジェクト。

+++`productListItems` 例

Web SDK `sendEvent` を呼び出し、 `productViews` ( `productListItems` 配列：

```javascript
alloy("sendEvent",{
  "xdm":{
    "commerce":{
      "productViews":{
        "value":1
      }
    },
    "productListItems":[
      {
        "SKU":"HT105",
        "name":"The Big Floppy Hat",
      },
      {
        "SKU":"HT104",
        "name":"The Small Floppy Hat",
      }
    ]
  }
});
```

+++

+++`productListAdds` 例

Web SDK `sendEvent` を呼び出し、 `productListAdds` イベント `productListItems` 配列：

```javascript
alloy("sendEvent",{
  "xdm":{
    "commerce":{
      "productListAdds":{
        "value":1
      }
    },
    "productListItems":[
      {
        "SKU":"HT105",
        "name":"The Big Floppy Hat",
        "quantity":1,
        "priceTotal":29.99,
        "productAddMethod":"Add to Cart Button"
      },
      {
        "SKU":"HT104",
        "name":"The Small Floppy Hat",
        "quantity":1,
        "priceTotal":9.99,
        "productAddMethod":"Add-on"
      }
    ]
  }
});
```

+++

+++`checkouts` 例

Web SDK `sendEvent` を呼び出し、 `checkouts` イベント `productListItems` 配列：

```javascript
alloy("sendEvent",{
  "xdm":{
    "commerce":{
      "checkouts":{
        "value":1
      }
    },
    "productListItems":[
      {
        "SKU":"HT105",
        "name":"The Big Floppy Hat",
        "quantity":1,
        "priceTotal":29.99
      },
      {
        "SKU":"HT104",
        "name":"The Small Floppy Hat",
        "quantity":1,
        "priceTotal":9.99
      }
    ]
  }
});
```

+++
