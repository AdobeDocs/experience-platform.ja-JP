---
title: イベントのトラッキング
seo-title: Adobe Experience Platform Web SDK のイベントのトラッキング
description: Experience Platform Web SDK のイベントのトラッキング方法について説明します
seo-description: Experience Platform Web SDK のイベントのトラッキング方法について説明します
translation-type: tm+mt
source-git-commit: e9fb726ddb84d7a08afb8c0f083a643025b0f903
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 97%

---


# イベントのトラッキング

イベントデータを Adobe Experience Cloud に送信するには、`event` コマンドを使用します。この `event` コマンドは、Experience Cloud にデータを送信し、パーソナライズされたコンテンツ、ID、オーディエンスの宛先を取得する主な方法です。

Adobe Experience Cloud に送信されるデータは、次の 2 つのカテゴリに分類されます。

* XDM データ
* XDM 以外のデータ（現在はサポートされていません）

## XDM データの送信

XDM データは、Adobe Experience Platform 内で作成したスキーマとコンテンツと同じ構造を持つオブジェクトです。[スキーマの作成方法の詳細について説明します。](../../xdm/tutorials/create-schema-ui.md)

分析、パーソナライゼーション、オーディエンスまたは宛先の一部とする XDM データは、`xdm` オプションを使用して送信する必要がありあｍす。

```javascript
alloy("event", {
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
>XDMフィールド内の各イベントで送信できるデータには、32 KBの制限があります。

### XDM　以外のデータの送信

現在、XDM　スキーマと一致しないデータの送信はサポートされていません。将来的にはサポートが予定されています。

### `eventType` の設定

XDM エクスペリエンスイベントには、`eventType` フィールドがあります。ここには、レコードのプライマリイベントタイプが表示されます。これは、`xdm` オプションの一部として渡すことができます。

```javascript
alloy("event", {
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

alloy("event", {
  "xdm": myXDMData,
  "type": "commerce.purchases"
});
```

## sendBeacon API の使用

Web ページのユーザーが離脱する直前にイベントデータを送信するのは、困難な場合があります。リクエストに時間がかかりすぎると、ブラウザーによってリクエストがキャンセルされる場合があります。一部のブラウザーではこの間に、データを簡単に収集できるよう、`sendBeacon` と呼ばれる Web 標準 API が実装されています。`sendBeacon` を使用する場合、ブラウザーはグローバルブラウジングコンテキストで Web リクエストをおこないます。これは、ブラウザーがバックグラウンドでビーコンリクエストをおこない、ページナビゲーションを保持しないことを意味します。Adobe Experience Platform Web SDK に `sendBeacon` を使用するよう伝えるには、イベントコマンドにオプション `"documentUnloading": true` を追加します。次に例を示します。

```javascript
alloy("event", {
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

ブラウザーでは、`sendBeacon` で一度に送信できるデータの量に制限が設けられています。多くのブラウザーでは、上限は 64K です。ペイロードが大きすぎてブラウザーがイベントを拒否した場合、Adobe Experience Platform Web SDK は、通常の転送方法（フェッチなど）を使用するようフォールバックします。

## イベントからの応答の処理

イベントからの応答を処理する場合は、次のように成功または失敗の通知を受け取ることができます。

```javascript
alloy("event", {
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
  "configId": "ebebf826-a01f-4458-8cec-ef61de241c93",
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

1. イベントコマンド　`alloy("event", { xdm: ... });`　にオプションとして渡される値
2. 自動的に収集された値（[自動情報](../reference/automatic-information.md)を参照）
3. `onBeforeEventSend` コールバックで加えられた変更です。

`onBeforeEventSend` コールバックでが例外をスローしてもイベントは送信されますが、コールバック内で加えられ変更は、最終的なイベントには適用されません。

## 実行可能なエラーの可能性

イベントを送信する際、送信されるデータが大きすぎる（要求全体で 32KB を超える）場合は、エラーが発生する可能性があります。この場合、送信されるデータの量を減らす必要があります。

デバッグが有効な場合、サーバーは、設定された XDM スキーマに対して送信されるイベントデータを同期的に検証します。データがスキーマと一致しない場合、不一致の詳細がサーバーから返され、エラーが発生します。この場合は、スキーマと一致するようにデータを変更します。デバッグが有効になっていない場合、サーバーはデータを非同期で検証するので、対応するエラーは発生しません。
