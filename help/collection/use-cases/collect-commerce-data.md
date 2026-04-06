---
title: Adobe Experience Platform Web SDKを使用して、コマース、商品、注文情報を収集します
description: Adobe Experience Platform Web SDKを使用して、商品またはショッピングカートに関連するデータを追加する方法を説明します。
exl-id: 3c79e776-89ef-494b-a2ea-3c23efce09ae
source-git-commit: e4ee4accdb28dafda7e37625eb84062bb6e53644
workflow-type: tm+mt
source-wordcount: '786'
ht-degree: 37%

---

# コマース、商品、注文情報の収集

組織で製品やサービスを販売している場合は、このページを、それらの製品やサービスの追跡方法に関するガイドとして使用できます。

このページでは、XDM [Commerce スキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md) フィールドグループを使用します。

このフィールドグループは、次の2つの主要な部分で構成されています。

* `commerce` オブジェクト。 このオブジェクトを使用すると、`productListItems`配列に対して実行されるアクションを指定できます。
* `productListItems`配列。

>[!TIP]
>
>Adobe Analyticsに精通している場合、`commerce` オブジェクトには、`events`変数にコマースイベントに似たデータが含まれています。 `productListItems` オブジェクト配列には、`products`変数に類似したデータが含まれています。

## `commerce` オブジェクト {#commerce-object}

この節では、`commerce` オブジェクトで使用可能なフィールドについて説明します。

>[!TIP]
>
>測定には、2 つのフィールド（`id` と `value`）があります。ほとんどの場合、`value` フィールドのみを使用します（例：`'value':1`）。 `id` フィールドを使用すると、メジャーの送信時にトラッキング用の一意のIDを設定できます。 詳しくは、[Measure](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/data/measure.schema.md)のXDM ドキュメントを参照してください。

| 測定 | レコメンデーション | 説明 |
|---|---|---|
| [`cartAbandons`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmcartabandons) | オプション | ユーザーが買い物かごにアクセスできなくなった、または購入できなくなった。 |
| [`checkouts`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmcheckouts) | 強くお勧めします | ユーザーは製品を閲覧しなくなったが、製品の購入処理を進めている。 |
| [`productListAdds`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmproductlistadds) | 強くお勧めします | 製品がリストに追加されている。必ず、`productListItems` で製品を同時に設定してください。 |
| [`productListOpens`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmproductlistopens) | オプション | 新しい製品リストが作成されます。例えば、新しいショッピングカートが作成されます。 |
| [`productListRemovals`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmproductlistremovals) | 強くお勧めします | 製品が製品リストから削除されています。 |
| [`productListReopens`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmproductlistreopens) | オプション | 製品リストがユーザーによって再アクティブ化されています。このアクションは、リマーケティングキャンペーンでよくおこなわれます。 |
| [`productListViews`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmproductlistviews) | 強くお勧めします | 製品のリストが表示されている。 |
| [`productViews`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmproductviews) | 強くお勧めします | 製品のビューが発生しました。 `productListItems` で表示する製品は必ず設定してください 。 |
| [`purchases`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmpurchases) | 強くお勧めします | 注文を受け付けている。製品リストが必要です。 |
| [`saveForLaters`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/commerce.schema.md#xdmsaveforlaters) | オプション | 後で使用するために製品を保存します。 |

{style="table-layout:auto"}

### `Commerce` オブジェクトの例

以下のセクションを展開して、`commerce` オブジェクトのフィールドを使用したWeb SDK コマンドの例を確認します。

+++ productViews

`sendEvent` フィールドを`productViews`に設定する基本的なWeb SDK `1`呼び出し：

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

`commerce` オブジェクトには、注文の詳細を収集するための専用オブジェクトが含まれています。 これは`order` オブジェクトと呼ばれます。

この節では、`order` オブジェクトでサポートされているすべてのフィールドについて説明します。

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

### 注文オブジェクトの例

以下のセクションを展開して、`commerce` オブジェクトを使用したWeb SDK コマンドの例を確認します。

+++ `Order` オブジェクトの例

`sendEvent`配列内の複数の商品に適用される`order` オブジェクトを設定するWeb SDK `productListItems`呼び出し：

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

製品リストには、対応するアクションに関連する製品が示されます。[productListItems](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md) のリストです。各製品にはいくつかのオプションのフィールドがあります。

| フィールド | レコメンデーション | 説明 |
|---|---|---|
| [`currencyCode`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md#xdmcurrencycode) | オプション | 製品の[ISO 4217](https://ja.wikipedia.org/wiki/ISO_4217)通貨。 このフィールドは通常、異なる通貨コードを持つ製品リストに複数の製品がある場合にのみ適用されます。 |
| [`priceTotal`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md#xdmpricetotal) | 強くお勧めします | 該当する場合にのみ、このフィールドを設定します。 例えば、商品の異なるバリエーションは異なる価格を持つことができますが、`productView` イベントでは異なる価格を持つことができるため、`productListAdds` イベントに設定できない場合があります。 |
| [`product`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md#xdmproduct) | 強くお勧めします | 製品の XDM ID。 |
| [`productAddMethod`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md#xdmproductaddmethod) | 強くお勧めします | 訪問者が製品項目をリストに追加するために使用した方法。`productListAdds`個のメジャーで設定し、製品がリストに追加されたときにのみ使用します。 例として、`add to cart button`、`quick add`、および `upsell` があります。 |
| [`productName`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md#xdmname) | 強くお勧めします | 製品の表示名または読みやすい名前。 |
| [`quantity`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md#xdmquantity) | 強くお勧めします | 顧客が製品を必要と示した数量。`productListAdds`、`productListRemoves`、`purchases`、`saveForLaters`、などで設定する必要があります。 |
| [`SKU`](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/productlistitem.schema.md#xdmsku) | 強くお勧めします | 店舗在庫単位（Store Keeping Unit）。製品の一意の ID です。 |

### 製品リストの例

以下のセクションを展開して、`productListItems` オブジェクトを使用したWeb SDK コマンドの例を確認します。

+++ `productListItems`の例

`sendEvent` アレイ内の複数の製品に対して`productViews`を設定するWeb SDK `productListItems`呼び出し：

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

+++ `productListAdds`の例

`sendEvent` アレイ内の複数の製品に対して`productListAdds` イベントを設定するWeb SDK `productListItems`呼び出し：

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

+++ `checkouts`の例

`sendEvent` アレイ内の複数の製品に対して`checkouts` イベントを設定するWeb SDK `productListItems`呼び出し：

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
