---
title: 製品
seo-title: Adobe Experience Platform Web SDKを使用したサポート製品
description: エクスペリエンスプラットフォームWeb SDKを使用して、製品または買い物かごがある場合にデータを追加する方法を説明します。
seo-description: エクスペリエンスプラットフォームWeb SDKを使用して、製品または買い物かごがある場合にデータを追加する方法を説明します。
translation-type: tm+mt
source-git-commit: 0cc6e233646134be073d20e2acd1702d345ff35f

---


# （ベータ版）製品

>[!IMPORTANT]
>
>Adobe Experience Platform Web SDKは現在ベータ版で、すべてのユーザーが利用できるわけではありません。 ドキュメントと機能は変更される場合があります。

サイトに製品がある場合は、これがアドビのほとんどの機能を有効にするために送信する可能性のあるデフォルトのセットです。 これは提案ですが、最初から非常に強力なデータセットを提供します。

このドキュメントでは、 [ExperienceEvent Commerce Detailsミックスインを使用します](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/experienceevent-commerce.schema.md) 。 ミックス `commerce` インは2つの部分に分割されています。オブジ `commerce` ェクトと配 `productListItems` 列です。 このオ `commerce` ブジェクトを使用すると、配列に対してどのアクションが発生しているかを指定 `productListItems` できます。

>[!Tip]
>Adobe Analyticsに詳しい場合は、変数 `commerce` と最も密接な関係があり `events` ます。 は、変 `productListItems` 数とより密接に関連してい `products` ます。

## 製品に関するアクション

以下に、オブジェクトで使用 `measures` 可能なリストを示 `commerce` します。

>[!Tip]
>メジャーには次の2つのフィールドがあります。 `id` と `value`ほとんどの場合、フィールドのみを使 `value` 用します(例： `'value':1`)。 このフ `id` ィールドでは、メジャーがいつ送信されたかを追跡するために使用できる一意の識別子を設定できます。 測定については、XDMのドキュメントを参照し [てくださ](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/measure.schema.md)い。

| **測定** | **推奨** | **説明** |
|---|---|---|
| [cartAbounds](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmcartabandons) | オプション | 買い物かごにアクセスできなくなり、ユーザーが購入できなくなりました。 |
| [チェックアウト](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmcheckouts) | 強く推奨 | ユーザーが製品を閲覧しなくなったが、製品の購入を進めている。 |
| [productListAdds](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmproductlistadds) | 強く推奨 | 製品がリストに追加されます。 必ず、製品を同時に設定し `productListItems` てください。 |
| [productListOpens](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmproductlistopens) | オプション | 新しい製品リストが作成されます。 （例えば、新しい買い物かごが作成されます）。 |
| [productListRemovals](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmproductlistremovals) | 強く推奨 | 製品が製品リストから削除されました。 |
| [productListReopens](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmproductlistreopens) | オプション | 製品リストがユーザーによって再アクティブ化されます。 これは、リマーケティングキャンペーンでよく発生します。 |
| [productListViews](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmproductlistviews) | 強く推奨 | 製品のリストが表示されます。 |
| [productViews](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmproductviews) | 強く推奨 | 製品の表示が発生しました。 で表示する製品は必ず設定してください `productListItems`。 |
| [購入](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmpurchases) | 強く推奨 | 注文が受け付けられる。 製品リストが必要です。 |
| [saveForLaters](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmsaveforlaters) | オプション | 製品は、今後使用するために保存されます。 |

SDKでの設定方法の例を次に `Measures` 示します。

```javascript
alloy("event", {
  "xdm":{
    "commerce":{
      "productViews":{
        "value":1
      }
    }
  }
});
```

コマースオブジェクトには、注文の詳細を収集するための特別なフィールドもありま `order`す。

| **Order** | **オプション** | **推奨** | **説明** |
|---|---|---|---|
| [currencyCode](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/order.schema.md#xdmcurrencycode) |  |  | 注文 [合計の](https://en.wikipedia.org/wiki/ISO_4217) ISO 4217通貨。 |
| [payments[paymentItems]](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/order.schema.md#xdmpayments) |  |  | 注文の支払の一覧。 paymentItemには [](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/paymentitem.schema.md#payment-item-schema) 、次が含まれます。 |
|  | [currencyCode](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/order.schema.md#xdmcurrencycode) | オプション | この支 [払方法の](https://en.wikipedia.org/wiki/ISO_4217) ISO 4217通貨。 |
|  | [paymentAmount](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/paymentitem.schema.md#xdmpaymentamount) | 強く推奨 | 指定した通貨コードでの支払の値。 |
|  | [paymentType](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/paymentitem.schema.md#xdmpaymenttype) | 強く推奨 | 支払のタイプ(例：、 `credit_card`、 `gift_card`、 `paypal`)。 詳しくは、既知の値のリ [ストを参照](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/paymentitem.schema.md#xdmpaymenttype-known-values) 。 |
|  | [transactionID](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/paymentitem.schema.md#xdmtransactionid) | オプション | この支払トランザクションの一意のID。 |
| [priceTotal](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/order.schema.md#xdmpricetotal) |  | 強く推奨 | すべての割引と税金が適用された後の、この注文の合計。 |
| [purchaseID](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/order.schema.md#xdmpurchaseid) |  | 非常に推奨される | 販売者がこの購入に割り当てた一意の識別子。 |
| [purchaseOrderNumber](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/order.schema.md#xdmpurchaseordernumber) |  | オプション | 購入者がこの購入に対して割り当てる一意の識別子。 |

SDKの一般的な購入例を次に示します。

```javascript
alloy("event",{
  "xdm":{
    "commerce":{
      "order":{
        "purchaseID":"123456789",
        "currenceCode":"USD",
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
        "qauntity":1
      }
    ]
  }
});
```

## 製品のリスト

製品リストには、対応するアクションに関連する製品が示されます。 これは、 [productListItemsのリストです](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md)。 各製品には、多数のオプションフィールドがあります。

| **フィールド** | **推奨** | **説明** |
|---|---|---|
| [currencyCode](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md#xdmcurrencycode) | オプション | 製品 [のISO 4217](https://en.wikipedia.org/wiki/ISO_4217) 通貨。 これは、異なる通貨コードを持つ製品を使用できる場合と、該当する場合にのみ便利です。 例えば、購入があった場合や買い物かごに追加された場合などです。 |
| [priceTotal](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md#xdmpricetotal) | 強く推奨 | 該当する場合にのみ設定する必要があります。 例えば、製品のバリエーションが異なると価格が異な `productView` る場合があるので、オンに設定できない場合がありま `productListAdds`す。 |
| [product](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md#xdmproduct) | 強く推奨 | 製品のXDM ID。 |
| [productAddMethod](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md#xdmproductaddmethod) | 強く推奨 | 訪問者が製品項目をリストに追加するために使用したメソッド。 メジャーと `productListAdds` 共に設定され、製品がリストに追加された場合にのみ使用します。 、、など `add to cart button`があ `quick add`ります `upsell`。 |
| [productName](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md#xdmname) | 強く推奨 | これは、製品の表示名または人が読み取り可能な名前に設定されます。 |
| [数量](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md#xdmquantity) | 強く推奨 | 顧客が製品に対して必要と示した数量。 、、、などに設 `productListAdds`定す `productListRemoves`る必 `purchases`要が `saveForLaters`あります。 |
| [SKU](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md) | 強く推奨 | 保管単位を参照してください。 製品の一意の識別子です。 |

## 例

`productView` イベント

```javascript
alloy("event",{
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
alloy("event",{
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
alloy("event",{
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
alloy("event",{
  "xdm":{
    "commerce":{
      "order":{
        "purchaseID":"123456789",
        "currenceCode":"USD",
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
        "qauntity":1
      }
    ]
  }
});
```
