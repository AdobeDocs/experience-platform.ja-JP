---
title: Adobe Experience Platform Web SDKを使用して、コマース、商品、注文の情報を収集します
description: Adobe Experience Platform web SDKを使用して、商品や買い物かごに関連するデータを追加する方法を説明します。
exl-id: 3c79e776-89ef-494b-a2ea-3c23efce09ae
source-git-commit: 35429ec2dffacb9c0f2c60b608561988ea487606
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 37%

---

# コマース、製品および注文の情報を収集します

組織が製品またはサービスを販売している場合、このページを使用して、それらの製品およびサービスを追跡する方法を確認できます。

このページでは、XDM [Commerce Schema](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md) フィールドグループを使用します。

このフィールドグループは、次の 2 つの主要な部分で構成されます。

* `commerce` オブジェクト。 このオブジェクトを使用すると、`productListItems` 配列に対して実行されるアクションを指定できます。
* `productListItems` 配列。

>[!TIP]
>
>Adobe Analyticsに精通している場合、`commerce` オブジェクトには、`events` 変数のコマースイベントに似たデータが含まれます。 `productListItems` オブジェクト配列には、`products` 変数に類似したデータが含まれています。

## `commerce` オブジェクト {#commerce-object}

ここでは、`commerce` オブジェクトで使用できるフィールドについて説明します。

>[!TIP]
>
>測定には、2 つのフィールド（`id` と `value`）があります。ほとんどの場合、使用するのは `value` フィールド（例：`'value':1`）のみです。 「`id`」フィールドを使用すると、測定が送信された際にトラッキングする一意の ID を設定できます。 詳しくは、[&#x200B; 測定 &#x200B;](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/measure.schema.md) の XDM ドキュメントを参照してください。

| 測定 | レコメンデーション | 説明 |
|---|---|---|
| [`cartAbandons`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmcartabandons) | オプション | ユーザーが買い物かごにアクセスできなくなった、または購入できなくなった。 |
| [`checkouts`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmcheckouts) | 強くお勧めします | ユーザーは製品を閲覧しなくなったが、製品の購入処理を進めている。 |
| [`productListAdds`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmproductlistadds) | 強くお勧めします | 製品がリストに追加されている。必ず、`productListItems` で製品を同時に設定してください。 |
| [`productListOpens`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmproductlistopens) | オプション | 新しい製品リストが作成されます。例えば、新しい買い物かごが作成されるとします。 |
| [`productListRemovals`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmproductlistremovals) | 強くお勧めします | 製品が製品リストから削除されています。 |
| [`productListReopens`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmproductlistreopens) | オプション | 製品リストがユーザーによって再アクティブ化されています。このアクションは、リマーケティングキャンペーンでよく発生します。 |
| [`productListViews`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmproductlistviews) | 強くお勧めします | 製品のリストが表示されている。 |
| [`productViews`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmproductviews) | 強くお勧めします | 商品のビューが発生しました。 `productListItems` で表示する製品は必ず設定してください 。 |
| [`purchases`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmpurchases) | 強くお勧めします | 注文を受け付けている。製品リストが必要です。 |
| [`saveForLaters`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmsaveforlaters) | オプション | 後で使用するために製品を保存します。 |

{style="table-layout:auto"}

### `Commerce` オブジェクトの例

以下のセクションを展開して、`commerce` オブジェクトのフィールドを使用した web SDK コマンドの例を確認します。

+++`productViews`

`sendEvent` フィールドを `productViews` に設定して、基本的な Web SDK `1` 呼び出しを行います。

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

## `order` オブジェクト {#order-object}

`commerce` オブジェクトには、注文の詳細を収集するための専用オブジェクトが含まれています。 これは `order` オブジェクトと呼ばれます。

ここでは、`order` オブジェクトでサポートされているすべてのフィールドについて説明します。

| フィールド | オプション | レコメンデーション | 説明 |
|---|---|---|---|
| [`currencyCode`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/order.schema.md#xdmcurrencycode) |  |  | 注文合計の [ISO 4217](https://ja.wikipedia.org/wiki/ISO_4217) 通貨。 |
| [`payments[]`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/order.schema.md#xdmpayments) |  |  | 注文の支払の一覧。[paymentItem](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/paymentitem.schema.md) には、次が含まれます。 |
|  | [`currencyCode`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/paymentitem.schema.md#xdmcurrencycode) | オプション | この支払い方法の [ISO 4217](https://ja.wikipedia.org/wiki/ISO_4217) 通貨。 |
|  | [`paymentAmount`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/paymentitem.schema.md#xdmpaymentamount) | 強くお勧めします | 指定した通貨コードでの支払金額。 |
|  | [`paymentType`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/paymentitem.schema.md#xdmpaymenttype) | 強くお勧めします | 支払のタイプ（例：`credit_card`、`gift_card`、`paypal`）。詳しくは、[既知の値](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/paymentitem.schema.md#xdmpaymenttype-known-values)のリストを参照。 |
|  | [`transactionID`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/paymentitem.schema.md#xdmtransactionid) | オプション | この支払トランザクションの一意の ID。 |
| [`priceTotal`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/order.schema.md#xdmpricetotal) |  | 強くお勧めします | すべての割引と税金が適用された後の、この注文の合計。 |
| [`purchaseID`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/order.schema.md#xdmpurchaseid) |  | 強くお勧めします | 販売者がこの購入に割り当てた一意の ID。 |
| [`purchaseOrderNumber`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/order.schema.md#xdmpurchaseordernumber) |  | オプション | 購入者がこの購入に割り当てた一意の ID。 |

### Order オブジェクトの例

次のセクションを展開すると、`commerce` オブジェクトを使用した web SDK コマンドの例が表示されます。

+++`Order` オブジェクトの例

Web SDK `sendEvent` は、`order` 配列の複数の商品に適用される `productListItems` オブジェクトを設定して呼び出します。

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

## 商品リストオブジェクト {#product-list-object}

製品リストには、対応するアクションに関連する製品が示されます。[productListItems](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md) のリストです。各製品には、複数のオプションフィールドがあります。

| フィールド | レコメンデーション | 説明 |
|---|---|---|
| [`currencyCode`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md#xdmcurrencycode) | オプション | 製品の [ISO 4217](https://ja.wikipedia.org/wiki/ISO_4217) 通貨。 このフィールドは通常、商品リストに異なる通貨コードを持つ複数の商品がある場合にのみ適用されます。 |
| [`priceTotal`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md#xdmpricetotal) | 強くお勧めします | 該当する場合のみ、このフィールドを設定します。 例えば、製品のバリエーションが異なれば価格は異な `productView` が、`productListAdds` のイベントでは異なる可能性があるので、イベントでを設定できない場合があります。 |
| [`product`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md#xdmproduct) | 強くお勧めします | 製品の XDM ID。 |
| [`productAddMethod`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md#xdmproductaddmethod) | 強くお勧めします | 訪問者が製品項目をリストに追加するために使用した方法。`productListAdds` の測定で設定され、製品がリストに追加された場合にのみ使用されます。 例として、`add to cart button`、`quick add`、および `upsell` があります。 |
| [`productName`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md#xdmname) | 強くお勧めします | 商品の表示名または人間が判読できる名前。 |
| [`quantity`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md#xdmquantity) | 強くお勧めします | 顧客が製品を必要と示した数量。`productListAdds`、`productListRemoves`、`purchases`、`saveForLaters`、などで設定する必要があります。 |
| [`SKU`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md#xdmsku) | 強くお勧めします | 店舗在庫単位（Store Keeping Unit）。製品の一意の ID です。 |

### 製品リストの例

以下のセクションを展開して、`productListItems` オブジェクトを使用した web SDK コマンドの例を確認します。

+++`productListItems` 例

Web SDK`sendEvent` 呼び出して、`productViews` 配列の複数の商品の `productListItems` を設定します。

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

Web SDKは、`sendEvent` 配列の複数の商品に対して `productListAdds` イベントを設定するために呼び出すことがで `productListItems` ます。

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

Web SDKは、`sendEvent` 配列の複数の商品に対して `checkouts` イベントを設定するために呼び出すことがで `productListItems` ます。

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
