---
title: イベントのトラッキング
seo-title: Adobe Experience Platform Web SDK のイベントのトラッキング
description: Experience Platform Web SDK のイベントのトラッキング方法について説明します
seo-description: Experience Platform Web SDK のイベントのトラッキング方法について説明します
keywords: sendEvent;xdm;eventType;datasetId;sendBeacon;send Beacon;documentUnloading;document Unloading;onBeforeEventSend;
translation-type: tm+mt
source-git-commit: 0928dd3eb2c034fac14d14d6e53ba07cdc49a6ea
workflow-type: tm+mt
source-wordcount: '1138'
ht-degree: 53%

---


# イベントのトラッキング

To send event data to Adobe Experience Cloud, use the `sendEvent` command. この `sendEvent` コマンドは、 にデータを送信し、パーソナライズされたコンテンツ、ID、オーディエンスの宛先を取得する主な方法です。[!DNL Experience Cloud]

Adobe Experience Cloud に送信されるデータは、次の 2 つのカテゴリに分類されます。

* XDM データ
* XDM 以外のデータ（現在はサポートされていません）

## XDM データの送信

XDM データは、Adobe Experience Platform 内で作成したスキーマとコンテンツと同じ構造を持つオブジェクトです。[スキーマの作成方法の詳細について説明します。](../../xdm/tutorials/create-schema-ui.md)

Any XDM data that you would like to be part of your analytics, personalization, audiences, or destinations should be sent using the `xdm` option.


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

>[!NOTE]
>
>XDMフィールド内の各イベントで送信できるデータには、32 KBの制限があります。

### XDM　以外のデータの送信

現在、XDM　スキーマと一致しないデータの送信はサポートされていません。将来的にはサポートが予定されています。

### `eventType` の設定

In an XDM experience event, there is an optional `eventType` field. ここには、レコードのプライマリイベントタイプが表示されます。イベントタイプを設定すると、送信するイベントを区別するのに役立ちます。 XDMには、ユーザが使用できる定義済みのイベントタイプがいくつか用意されています。また、ユースケースに合わせて独自のカスタムイベントタイプを常に作成することもできます。 以下は、XDMが提供するあらかじめ定義されたイベントタイプのリストです。 [詳しくは、XDMの公開リポートを参照してください](https://github.com/adobe/xdm/blob/master/docs/reference/behaviors/time-series.schema.md#xdmeventtype-known-values)。


| **イベントタイプ:** | **定義:** |
| ---------------------------------- | ------------ |
| advertising.completes | 時間指定メディアアセットが視聴され、完了したかどうかを示します。これは、ビデオ全体が視聴されたことを意味するわけではありません。閲覧者は先にスキップした可能性があります |
| advertising.timePlayed | 特定の時間指定メディアアセットに対するユーザーの滞在時間を示します。 |
| advertising.federated | エクスペリエンスイベントがデータフェデレーション（顧客間のデータ共有）を通じて作成されたかどうかを示します。 |
| advertising.clicks | 広告に対するクリック操作 |
| advertising.conversions | パフォーマンス評価のイベントをトリガーする、お客様が事前に定義したアクション |
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


これらのイベントタイプは、Adobe Experience Platform Launchの拡張機能を使用している場合や、Experience Platform Launchなしでいつでも渡すことができる場合は、ドロップダウンに表示されます。 They can be passed in as part of the `xdm` option.


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

場合によっては、設定UIで設定されたデータセット以外のデータセットにイベントを送信する必要があります。 その場合は、 `datasetId``sendEvent` コマンドでオプションを設定する必要があります。


```javascript
var myXDMData = { ... };

alloy("sendEvent", {
  "xdm": myXDMData,
  "type": "commerce.checkout",
  "datasetId": "YOUR_DATASET_ID"
});
```

### ID情報の追加

カスタムID情報をイベントに追加することもできます。 Experience CloudIDの [取得を参照してください](../identity/overview.md)。

## sendBeacon API の使用

Web ページのユーザーが離脱する直前にイベントデータを送信するのは、困難な場合があります。リクエストに時間がかかりすぎると、ブラウザーによってリクエストがキャンセルされる場合があります。一部のブラウザーではこの間に、データを簡単に収集できるよう、`sendBeacon` と呼ばれる Web 標準 API が実装されています。`sendBeacon` を使用する場合、ブラウザーはグローバルブラウジングコンテキストで Web リクエストをおこないます。これは、ブラウザーがバックグラウンドでビーコンリクエストをおこない、ページナビゲーションを保持しないことを意味します。To tell Adobe Experience Platform [!DNL Web SDK] to use `sendBeacon`, add the option `"documentUnloading": true` to the event command.  次に例を示します。


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

ブラウザーでは、`sendBeacon` で一度に送信できるデータの量に制限が設けられています。多くのブラウザーでは、上限は 64K です。If the browser rejects the event because the payload is too large, Adobe Experience Platform [!DNL Web SDK] falls back to using its normal transport method (for example, fetch).

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

イベントのフィールドをグローバルに追加、削除、または変更する場合は、`onBeforeEventSend` コールバックを設定できます。このコールバックは、イベントが送信されるたびに呼び出されます。このコールバックは、`xdm` フィールドを含むイベントオブジェクトで渡されます。イベントで送信されるデータを変更する場合は、`event.xdm` を変更します。


```javascript
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "onBeforeEventSend": function(event) {
    // Change existing values
    event.xdm.web.webPageDetails.URL = xdm.web.webPageDetails.URL.toLowerCase();
    // Remove existing values
    delete event.xdm.web.webReferrer.URL;
    // Or add new values
    event.xdm._adb3lettersandnumbers.mycustomkey = "value";
  }
});
```

`xdm` フィールドは次の順序で設定されます。

1. イベントコマンド　`alloy("sendEvent", { xdm: ... });`　にオプションとして渡される値
2. 自動的に収集された値（[自動情報](../data-collection/automatic-information.md)を参照）
3. `onBeforeEventSend` コールバックで加えられた変更です。

`onBeforeEventSend` コールバックでが例外をスローしてもイベントは送信されますが、コールバック内で加えられ変更は、最終的なイベントには適用されません。

## 実行可能なエラーの可能性

イベントを送信する際、送信されるデータが大きすぎる（要求全体で 32KB を超える）場合は、エラーが発生する可能性があります。この場合、送信されるデータの量を減らす必要があります。

デバッグが有効な場合、サーバーは、設定された XDM スキーマに対して送信されるイベントデータを同期的に検証します。データがスキーマと一致しない場合、不一致の詳細がサーバーから返され、エラーが発生します。この場合は、スキーマと一致するようにデータを変更します。デバッグが有効になっていない場合、サーバーはデータを非同期で検証するので、対応するエラーは発生しません。
