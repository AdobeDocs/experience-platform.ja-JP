---
title: 製品
seo-title: Adobe Experience Platform Web SDK での製品のサポート
description: Experience Platform Web SDK で製品または買い物かごを使用している場合に、データを追加する方法について説明します。
seo-description: Experience Platform Web SDK で製品または買い物かごを使用している場合に、データを追加する方法について説明します。
translation-type: tm+mt
source-git-commit: 4bff4b20ccc1913151aa1783d5123ffbb141a7d0
workflow-type: tm+mt
source-wordcount: '1314'
ht-degree: 100%

---


# 製品

サイト上に製品がある場合は、アドビのほとんどの機能を有効にするために、このデフォルトのセットを送信することができます。これは提案ですが、最初から非常に強力なデータセットを提供します。

このドキュメントでは、[ExperienceEvent Commerce Details](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/experienceevent-commerce.schema.md) mixin を使用します。`commerce` mixin は　2　つの部分（`commerce` オブジェクトと `productListItems` 配列）に分類されます。`commerce` オブジェクトを使用すると、`productListItems` 配列に対してどのアクションが発生しているかを指定できます。

>[!Tip]
>Adobe Analytics に詳しい方向けに説明すると、`commerce` は `events` と最も密接に関係しています。`productListItems` は、`products`　変数とより密接に関連しています。

## 製品に関するアクション

以下に、`commerce` オブジェクトで使用可能な `measures` のリストを示します。

>[!Tip]
>測定には、2 つのフィールド（`id` と `value`）があります。ほとんどの場合、`value` フィールドのみを使用します（例：`'value':1`）。この `id` フィールドでは、測定が送信されたタイミングを追跡可能な一意の ID を設定できます。[測定](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/measure.schema.md)については、XDM のドキュメントを参照してください。

| **測定** | **推奨** | **説明** |
|---|---|---|
| [cartAbandons](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmcartabandons) | オプション | ユーザーが買い物かごにアクセスできなくなった、または購入できなくなった。 |
| [checkouts](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmcheckouts) | 強く推奨 | ユーザーは製品を閲覧しなくなったが、製品の購入処理を進めている。 |
| [productListAdds](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmproductlistadds) | 強く推奨 | 製品がリストに追加されている。必ず、`productListItems` で製品を同時に設定してください。 |
| [productListOpens](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmproductlistopens) | オプション | 新しい製品リストが作成されます。（例：新しい買い物かごが作成される） |
| [productListRemovals](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmproductlistremovals) | 強く推奨 | 製品が製品リストから削除されています。 |
| [productListReopens](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmproductlistreopens) | オプション | 製品リストがユーザーによって再アクティブ化されています。これは、リマーケティングキャンペーンで頻繁に発生します。 |
| [productListViews](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmproductlistviews) | 強く推奨 | 製品のリストが表示されている。 |
| [productViews](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmproductviews) | 強く推奨 | 製品の表示が発生した。`productListItems` で表示する製品は必ず設定してください 。 |
| [purchases](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmpurchases) | 強く推奨 | 注文を受け付けている。製品リストが必要です。 |
| [saveForLaters](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmsaveforlaters) | オプション | 後で使用するために製品を保存します。 |

SDK　でこれらの `Measures` を設定する方法の例を以下に示します。

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

コマースオブジェクトには、注文の詳細を収集するための、`order` と呼ばれる特別なフィールドもあります。

| **Order** | **オプション** | **推奨** | **説明** |
|---|---|---|---|
| [currencyCode](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/order.schema.md#xdmcurrencycode) |  |  | 注文合計の [ISO 4217](https://ja.wikipedia.org/wiki/ISO_4217) 通貨。 |
| [payments[paymentItems]](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/order.schema.md#xdmpayments) |  |  | 注文の支払の一覧。[paymentItem](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/paymentitem.schema.md#payment-item-schema) には、次が含まれます。 |
|  | [currencyCode](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/order.schema.md#xdmcurrencycode) | オプション | この支払い方法の [ISO 4217](https://ja.wikipedia.org/wiki/ISO_4217) 通貨。 |
|  | [paymentAmount](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/paymentitem.schema.md#xdmpaymentamount) | 強く推奨 | 指定した通貨コードでの支払金額。 |
|  | [paymentType](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/paymentitem.schema.md#xdmpaymenttype) | 強く推奨 | 支払のタイプ（例：`credit_card`、`gift_card`、`paypal`）。詳しくは、[既知の値](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/paymentitem.schema.md#xdmpaymenttype-known-values)のリストを参照。 |
|  | [transactionID](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/paymentitem.schema.md#xdmtransactionid) | オプション | この支払トランザクションの一意の ID。 |
| [priceTotal](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/order.schema.md#xdmpricetotal) |  | 強く推奨 | すべての割引と税金が適用された後の、この注文の合計。 |
| [purchaseID](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/order.schema.md#xdmpurchaseid) |  | 強く推奨 | 販売者がこの購入に割り当てた一意の ID。 |
| [purchaseOrderNumber](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/order.schema.md#xdmpurchaseordernumber) |  | オプション | 購入者がこの購入に割り当てた一意の ID。 |

SDK の一般的な購入例を次に示します。

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

## 製品のリスト

製品リストには、対応するアクションに関連する製品が示されます。[productListItems](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md) のリストです。各製品には、多数のオプションフィールドがあります。

| **フィールド** | **推奨** | **説明** |
|---|---|---|
| [currencyCode](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md#xdmcurrencycode) | オプション | 製品の [ISO 4217](https://ja.wikipedia.org/wiki/ISO_4217) 通貨。異なる通貨コードを持つ製品がある場合と、該当する場合にのみ便利です。例えば、購入があった場合や買い物かごに追加された場合などです。 |
| [priceTotal](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md#xdmpricetotal) | 強く推奨 | 該当する場合にのみ設定する必要があります。例えば、製品のバリエーションが異なると、価格が異なるのに `productListAdds` に記載されている場合があるので、`productView` では設定できない場合があります。 |
| [product](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md#xdmproduct) | 強く推奨 | 製品の XDM ID。 |
| [productAddMethod](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md#xdmproductaddmethod) | 強く推奨 | 訪問者が製品項目をリストに追加するために使用した方法。`productListAdds` 測定とともに設定され、製品がリストに追加された場合にのみ使用します。例として、`add to cart button`、`quick add`、および `upsell` があります。 |
| [productName](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md#xdmname) | 強く推奨 | 製品の表示名または人が読み取り可能な名前に設定します。 |
| [quantity](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md#xdmquantity) | 強く推奨 | 顧客が製品を必要と示した数量。`productListAdds`、`productListRemoves`、`purchases`、`saveForLaters`、などで設定する必要があります。 |
| [SKU](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md) | 強く推奨 | 店舗在庫単位（Store Keeping Unit）。製品の一意の ID です。 |

## 例

`productView` イベント

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

`productView` イベント

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

`checkout` イベント

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

`purchase` イベント

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
