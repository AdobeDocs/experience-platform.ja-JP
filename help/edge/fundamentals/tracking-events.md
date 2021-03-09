---
title: Adobe Experience PlatformWeb SDKを使用したイベントの追跡
description: Adobe Experience PlatformWeb SDKイベントの追跡方法を説明します。
keywords: sendEvent;xdm;eventType;datasetId;sendBeacon;sendBeacon;send Beacon;documentUnloading;ドキュメントのアンロード；onBeforeEventSend;
translation-type: tm+mt
source-git-commit: 25cf425df92528cec88ea027f3890abfa9cd9b41
workflow-type: tm+mt
source-wordcount: '1397'
ht-degree: 34%

---


# 追跡イベント

イベントデータをAdobe Experience Cloudに送信するには、`sendEvent`コマンドを使用します。 この `sendEvent` コマンドは、 にデータを送信し、パーソナライズされたコンテンツ、ID、オーディエンスの宛先を取得する主な方法です。[!DNL Experience Cloud]

Adobe Experience Cloud に送信されるデータは、次の 2 つのカテゴリに分類されます。

* XDM データ
* XDM 以外のデータ（現在はサポートされていません）

## XDM データの送信

XDM データは、Adobe Experience Platform 内で作成したスキーマとコンテンツと同じ構造を持つオブジェクトです。[スキーマの作成方法の詳細について説明します。](../../xdm/tutorials/create-schema-ui.md)

解析、パーソナライゼーション、オーディエンス、または宛先に含めたいXDMデータは、`xdm`オプションを使用して送信する必要があります。


```javascript
alloy("sendEvent", {
  "xdm": {
    "commerce": {
      "order": {
        "purchaseID": "a8g784hjq1mnp3",
        "purchaseOrderNumber": "VAU3123",
        "currencyCode": "USD",
        "priceTotal": 999.98
      }
    }
  }
});
```

`sendEvent`コマンドが実行されてから、データがサーバーに送信されるまでの間に時間が経過する場合があります（例えば、Web SDKライブラリが完全に読み込まれていない場合や、同意がまだ受け取られていない場合）。 `sendEvent`コマンドの実行後に`xdm`オブジェクトの一部を変更する場合は、`sendEvent`コマンドの実行前に`xdm`オブジェクト&#x200B;_を_&#x200B;クローンすることを強くお勧めします。 次に例を示します。

```javascript
var clone = function(value) {
  return JSON.parse(JSON.stringify(value));
};

var dataLayer = {
  "commerce": {
    "order": {
      "purchaseID": "a8g784hjq1mnp3",
      "purchaseOrderNumber": "VAU3123",
      "currencyCode": "USD",
      "priceTotal": 999.98
    }
  }
};

alloy("sendEvent", {
  "xdm": clone(dataLayer)
});

// This change will not be reflected in the data sent to the 
// server for the prior sendEvent command.
dataLayer.commerce = null;
```

この例では、データレイヤーをJSONにシリアル化して複製し、その後デシリアライズします。 次に、コピーされた結果が`sendEvent`コマンドに渡されます。 これにより、`sendEvent`コマンドが`sendEvent`コマンドの実行時に存在したデータレイヤーのスナップショットが確実に保持され、後で元のデータレイヤーオブジェクトに対する変更がサーバーに送信されるデータに反映されなくなります。 イベント主導型のデータレイヤーを使用している場合は、データのクローン作成は既に自動的に処理されている可能性があります。 例えば、[Adobeクライアントデータレイヤー](https://github.com/adobe/adobe-client-data-layer/wiki)を使用している場合、`getState()`メソッドは、すべての以前の変更に関する計算済みのクローンスナップショットを提供します。 これは、Adobe Experience Platform LaunchでAdobe Experience PlatformWeb SDK拡張を使用している場合も自動的に処理されます。

>[!NOTE]
>
>XDMフィールド内の各イベントで送信できるデータには、32 KBの制限があります。

### XDM　以外のデータの送信

現在、XDM　スキーマと一致しないデータの送信はサポートされていません。将来的にはサポートが予定されています。

### `eventType` の設定 {#event-types}

XDMエクスペリエンスイベントには、オプションの`eventType`フィールドがあります。 ここには、レコードのプライマリイベントタイプが表示されます。イベントタイプを設定すると、送信するイベントを区別するのに役立ちます。 XDMには、ユーザが使用できる定義済みのイベントタイプがいくつか用意されています。また、ユースケースに合わせて独自のカスタムイベントタイプを常に作成することもできます。 以下は、XDMが提供するあらかじめ定義されたイベントタイプのリストです。 [詳しくは、XDMの公開リポートを参照してください](https://github.com/adobe/xdm/blob/master/docs/reference/behaviors/time-series.schema.md#xdmeventtype-known-values)。


| **イベントタイプ:** | **定義:** |
| ---------------------------------- | ------------ |
| advertising.completes | 時間指定メディアアセットが視聴され、完了したかどうかを示します。これは、ビデオ全体が視聴されたことを意味するわけではありません。閲覧者は先にスキップした可能性があります |
| advertising.timePlayed | 特定の時間指定メディアアセットに対するユーザーの滞在時間を示します。 |
| advertising.federated | エクスペリエンスイベントがデータフェデレーション（顧客間のデータ共有）を通じて作成されたかどうかを示します。 |
| advertising.clicks | 広告に対するクリック操作 |
| advertising.conversions | パフォーマンス評価のイベントをトリガーするお客様の事前定義済みのアクション |
| advertising.firstQuartiles | デジタルビデオ広告が通常の速度で25%再生されました |
| advertising.impressions | エンドユーザーに対する広告のインプレッション（複数可）で、閲覧可能性がある |
| advertising.midpoints | デジタルビデオ広告が通常の速度で50%再生されました |
| advertising.starts | デジタルビデオ広告の再生が開始されました |
| advertising.thirdQuartiles | デジタルビデオ広告が通常の速度で75%再生されました |
| web.webpagedetails.pageViews | Webページの表示が発生しました |
| web.webinteraction.linkClicks | Webリンクのクリックが発生しました |
| commerce.checkouts | 製品リストのチェックアウトプロセス中のイベント。チェックアウトプロセスに複数のステップがある場合は、複数のチェックアウトアクションが存在する可能性があります。複数のステップがある場合、イベント時間の情報と参照先のページまたはエクスペリエンスが使用され、個々のイベントが順番に表すステップが識別されます。 |
| commerce.productListAdds | 製品の製品リストへの追加。買い物かごに商品を追加する例 |
| commerce.productListOpens | 新しい製品リストの初期化。例：買い物かごが作成される場合 |
| commerce.productListRemovals | 製品エントリの製品リストからの削除例えば、製品が買い物かごから削除される場合 |
| commerce.productListReopens | アクセスできなくなった（破棄された）製品リストが、ユーザーによって再度アクティブ化されました。リマーケティングアクティビティを使用した例 |
| commerce.productListViews | 製品リストの表示が発生しました |
| commerce.productViews | 製品の表示が発生しました |
| commerce.purchases | 注文が受け入れられました。コマースコンバージョンで必要なアクションは購入のみです。購入では、商品リストが参照されている必要があります |
| commerce.saveForLaters | 製品のリストは、今後の使用のために保存されます。product wishリストの例 |
| delivery.feedback | 配信のフィードバックイベント。 電子メール配信用のフィードバックイベントの例 |


これらのイベントタイプは、Adobe Experience Platform Launchの拡張機能を使用している場合や、Experience Platform Launchなしでいつでも渡すことができる場合は、ドロップダウンに表示されます。 `xdm`オプションの一部として渡すことができます。


```javascript
alloy("sendEvent", {
  "xdm": {
    "eventType": "commerce.purchases",
    "commerce": {
      "order": {
        "purchaseID": "a8g784hjq1mnp3",
        "purchaseOrderNumber": "VAU3123",
        "currencyCode": "USD",
        "priceTotal": 999.98
      }
    }
  }
});
```

または、`eventType` オプションを使用して、`type` をイベントコマンドに渡すことができます。これはバックグラウンドで XDM データに追加されます。`type` をオプションとして指定すると、XDM ペイロードを変更しなくても、より簡単に `eventType` を設定できるようになります。


```javascript
var myXDMData = { ... };

alloy("sendEvent", {
  "xdm": myXDMData,
  "type": "commerce.purchases"
});
```

### データセットIDの上書き

場合によっては、設定UIで設定されたデータセット以外のデータセットにイベントを送信する必要があります。 そのためには、`sendEvent`コマンドに`datasetId`オプションを設定する必要があります。


```javascript
var myXDMData = { ... };

alloy("sendEvent", {
  "xdm": myXDMData,
  "type": "commerce.checkout",
  "datasetId": "YOUR_DATASET_ID"
});
```

### ID情報の追加

カスタムID情報をイベントに追加することもできます。 [Experience CloudIDの取得](../identity/overview.md)を参照してください。

## sendBeacon API の使用

Web ページのユーザーが離脱する直前にイベントデータを送信するのは、困難な場合があります。リクエストに時間がかかりすぎると、ブラウザーによってリクエストがキャンセルされる場合があります。一部のブラウザーではこの間に、データを簡単に収集できるよう、`sendBeacon` と呼ばれる Web 標準 API が実装されています。`sendBeacon` を使用する場合、ブラウザーはグローバルブラウジングコンテキストで Web リクエストをおこないます。これは、ブラウザーがバックグラウンドでビーコンリクエストをおこない、ページナビゲーションを保持しないことを意味します。[!DNL Web SDK]に`sendBeacon`を使うように指示するには、イベントコマンドに`"documentUnloading": true`を追加します。  次に例を示します。


```javascript
alloy("sendEvent", {
  "documentUnloading": true,
  "xdm": {
    "commerce": {
      "order": {
        "purchaseID": "a8g784hjq1mnp3",
        "purchaseOrderNumber": "VAU3123",
        "currencyCode": "USD",
        "priceTotal": 999.98
      }
    }
  }
});
```

ブラウザーでは、`sendBeacon` で一度に送信できるデータの量に制限が設けられています。多くのブラウザーでは、上限は 64K です。ペイロードが大きすぎるのでブラウザーがイベントを拒否した場合、Adobe Experience Platform[!DNL Web SDK]は通常の転送方法（例えばフェッチなど）を使用してフォールバックします。

## イベントからの応答の処理

イベントからの応答を処理する場合は、次のように成功または失敗の通知を受け取ることができます。


```javascript
alloy("sendEvent", {
  "renderDecisions": true,
  "xdm": {
    "commerce": {
      "order": {
        "purchaseID": "a8g784hjq1mnp3",
        "purchaseOrderNumber": "VAU3123",
        "currencyCode": "USD",
        "priceTotal": 999.98
      }
    }
  }
}).then(function(results) {
    // Tracking the event succeeded.
  })
  .catch(function(error) {
    // Tracking the event failed.
  });
```

## イベントのグローバルな変更 {#modifying-events-globally}

イベントのフィールドをグローバルに追加、削除、または変更する場合は、`onBeforeEventSend` コールバックを設定できます。このコールバックは、イベントが送信されるたびに呼び出されます。このコールバックは、`xdm` フィールドを含むイベントオブジェクトで渡されます。`content.xdm`を変更して、イベントと共に送信されるデータを変更します。


```javascript
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "onBeforeEventSend": function(content) {
    // Change existing values
    content.xdm.web.webPageDetails.URL = xdm.web.webPageDetails.URL.toLowerCase();
    // Remove existing values
    delete content.xdm.web.webReferrer.URL;
    // Or add new values
    content.xdm._adb3lettersandnumbers.mycustomkey = "value";
  }
});
```

`xdm` フィールドは次の順序で設定されます。

1. イベントコマンド　`alloy("sendEvent", { xdm: ... });`　にオプションとして渡される値
2. 自動的に収集された値（[自動情報](../data-collection/automatic-information.md)を参照）
3. `onBeforeEventSend` コールバックで加えられた変更です。

`onBeforeEventSend`コールバックに関する注意事項

* イベントXDMは、コールバック中に変更できます。 コールバックが返された後、
content.xdmおよびcontent.dataオブジェクトは、イベントと共に送信されます。

   ```javascript
   onBeforeEventSend: function(content){
     //sets a query parameter in XDM
     const queryString = window.location.search;
     const urlParams = new URLSearchParams(queryString);
     content.xdm.marketing.trackingCode = urlParams.get('cid')
   }
   ```

* このコールバックによって例外がスローされると、イベントの処理は中断され、イベントは送信されません。
* コールバックが`false`のboolean値を返すと、イベント処理は中断し、
エラーがない場合、イベントは送信されません。 このメカニズムにより、特定のイベントを
イベントデータを調べ、イベントを送信すべきでない場合は`false`を返します。

   >[!NOTE]
   >ページの最初のイベントでfalseが返されないように、注意が必要です。 最初のイベントでfalseを返すと、パーソナライゼーションに悪影響を与える可能性があります。

```javascript
   onBeforeEventSend: function(content) {
     // ignores events from bots
     if (MyBotDetector.isABot()) {
       return false;
     }
   }
```

ブール値`false`以外の戻り値は、イベントがコールバックの後で処理および送信できるようにします。

* イベントは、イベントタイプを調べることでフィルタリングできます([イベントタイプ](#event-types)を参照)。

```javascript
    onBeforeEventSend: function(content) {  
      // augments XDM if link click event is to a partner website
      if (
        content.xdm.eventType === "web.webinteraction.linkClicks" &&
        content.xdm.web.webInteraction.URL ===
          "http://example.com/partner-page.html"
      ) {
        content.xdm.partnerWebsiteClick = true;
      }
   }
```

## 実行可能なエラーの可能性

イベントを送信する際、送信されるデータが大きすぎる（要求全体で 32KB を超える）場合は、エラーが発生する可能性があります。この場合、送信されるデータの量を減らす必要があります。
