---
title: イベントの追跡
seo-title: Adobe Experience Platform Web SDKイベントの追跡
description: エクスペリエンスプラットフォームWeb SDKイベントの追跡方法を説明します。
seo-description: エクスペリエンスプラットフォームWeb SDKイベントの追跡方法を説明します。
translation-type: tm+mt
source-git-commit: 0cc6e233646134be073d20e2acd1702d345ff35f

---


# イベントの追跡

>[!IMPORTANT]
>
>Adobe Experience Platform Web SDKは現在ベータ版で、すべてのユーザーが利用できるわけではありません。 ドキュメントと機能は変更される場合があります。

イベントデータをAdobe Experience Cloudに送信するには、コマンドを使用 `event` します。 このコ `event` マンドは、Experience Cloudにデータを送信し、パーソナライズされたコンテンツ、ID、オーディエンスの宛先を取得する主な方法です。

Adobe Experience Cloudに送信されるデータは、次の2つのカテゴリに分類されます。

* XDMデータ
* XDM以外のデータ（現在はサポートされていません）

## XDMデータの送信

XDMデータは、Adobe Experience Platform内で作成したスキーマとコンテンツと構造が一致するオブジェクトです。 [スキーマの作成方法の詳細を表示します。](https://www.adobe.io/apis/experienceplatform/home/tutorials/alltutorials.html#!api-specification/markdown/narrative/tutorials/schema_editor_tutorial/schema_editor_tutorial.md)

解析、パーソナライゼーション、オーディエンスまたは宛先に含めたいXDMデータは、このオプションを使用して送信し `xdm` ます。

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

### XDM以外のデータの送信

現在、XDMスキーマと一致しないデータの送信はサポートされていません。 今後のサポートが予定されています。

### 設定 `eventType`

XDMエクスペリエンスイベントには、フィールドがあ `eventType` ります。 レコードのプライマリイベントタイプを保持します。 これは、オプションの一部として渡すことがで `xdm` きます。

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

または、このオ `eventType` プションを使用して、をイベントコマンドに渡すことがで `type` きます。 背後で、これはXDMデータに追加されます。 をオプションと `type` して指定すると、XDMペイロードを変更しなくても、よ `eventType` り簡単に設定できるようになります。

```javascript
var myXDMData = { ... };

alloy("event", {
  "xdm": myXDMData,
  "type": "commerce.purchases"
});
```

### ビューの開始

ビューが開始されたら、コマンド内でに設定してSDKに通知す `viewStart` るこ `true` とが重要 `event` です。 これは、特に、SDKがパーソナライズされたコンテンツを取得し、レンダリングする必要があることを示します。 現在パーソナライゼーションを使用していない場合でも、ページ上のコードを変更する必要がないので、後でパーソナライゼーションやその他の機能を有効にすることが大幅に簡略化されます。 また、データを収集した後で分析レポートを表示する場合、ビューを追跡すると便利です。

ビューの定義は、コンテキストによって異なります。

* 通常のWebサイトでは、各Webページは一意の表示と見なされます。 この場合、に設定されたイベ `viewStart` ントは、 `true` 可能な限り早くページの先頭で実行する必要があります。
* 単一ページアプリ\(SPA\)では、ビューの定義は少ない。 通常は、ユーザーがアプリ内を移動し、ほとんどのコンテンツが変更されたことを意味します。 シングルページアプリの技術的な基礎に精通しているユーザーの場合、通常、これはアプリケーションが新しいルートを読み込むときです。 ユーザーが新しいビューに移動した場合、ビューの定義を選択すると __、に設定されたイベ `viewStart` ントが `true` 実行されます。

に設定されたイベ `viewStart` ントは、 `true` Adobe Experience Cloudにデータを送信し、Adobe Experience Cloudからコンテンツを要求する主なメカニズムです。 次に、ビューの開始方法を示します。

```javascript
alloy("event", {
  "viewStart": true,
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

データの送信後、サーバーはパーソナライズされたコンテンツなどを使用して応答します。 このパーソナライズされたコンテンツは、自動的にビューにレンダリングされます。 リンクハンドラーも、新しいビューのコンテンツに自動的に添付されます。

## sendBeacon APIの使用

Webページのユーザーが移動する直前にイベントデータを送信するのは難しい場合があります。 リクエストに時間がかかりすぎると、ブラウザーがリクエストをキャンセルする場合があります。 一部のブラウザーでは、この時間中にデータをより簡単に収集できるよ `sendBeacon` うに、と呼ばれるWeb標準APIが実装されています。 を使用する場合、 `sendBeacon`ブラウザはグローバルブラウジングコンテキストでWebリクエストを行います。 これは、ブラウザーがバックグラウンドでビーコンリクエストを行い、ページナビゲーションを保持しないことを意味します。 Adobe Experience Platform Web SDKに使用を伝えるには、イベ `sendBeacon`ントコマンドにオプション `"documentUnloading": true` を追加します。  次に例を示します。

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

ブラウザーでは、一度に送信できるデータの量に制限が設 `sendBeacon` けられています。 多くのブラウザーでは、制限は64Kです。 ペイロードが大きすぎてブラウザーがイベントを拒否した場合、Adobe Experience Platform Web SDKは、通常のトランスポート方法（例えば、フェッチ）を使用する方法にフォールバックします。

## イベントからの応答の処理

イベントからの応答を処理する場合は、次のように成功または失敗の通知を受け取ることができます。

```javascript
alloy("event", {
  "viewStart": true,
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
}).then(function() {
    // Tracking the event succeeded.
  })
  .catch(function(error) {
    // Tracking the event failed.
  });
```

## イベントのグローバルな変更 {#modifying-events-globally}

イベントのフィールドをグローバルに追加、削除、または変更する場合は、コールバックを設定で `onBeforeEventSend` きます。  このコールバックは、イベントが送信されるたびに呼び出されます。  このコールバックは、フィールドを持つイベントオブジェクトで渡 `xdm` されます。  イベント `event.xdm` で送信されるデータを変更する場合は、変更します。

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

1. イベントコマンドにオプションとして渡される値 `alloy("event", { xdm: ... });`
2. 自動的に収集された値。  (自動情報 [を参照](../reference/automatic-information.md))。
3. コールバックで行われた変 `onBeforeEventSend` 更です。

コールバック `onBeforeEventSend` が例外をスローした場合、イベントは送信されます。ただし、コールバック内で行われた変更は、最終的なイベントには適用されません。

## 実行可能なエラーの可能性

イベントの送信時に、送信されるデータが大きすぎる（完全な要求で32 KBを超える）場合は、エラーが発生する可能性があります。 この場合、送信されるデータの量を減らす必要があります。

デバッグが有効な場合、サーバーは、設定されたXDMスキーマに対して送信されるイベントデータを同期的に検証します。 データがスキーマと一致しない場合、不一致の詳細がサーバーから返され、エラーが発生します。 この場合は、スキーマに一致するようにデータを変更します。 デバッグが有効になっていない場合、サーバーはデータを非同期で検証するので、対応するエラーは発生しません。
