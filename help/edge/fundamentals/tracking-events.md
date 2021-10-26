---
title: Adobe Experience Platform Web SDK を使用したイベントの追跡
description: Adobe Experience Platform Web SDK のイベントの追跡方法について説明します。
keywords: sendEvent;xdm;eventType;datasetId;sendBeacon;sendBeacon;send Beacon;documentUnloading;document Unloading;onBeforeEventSend;
exl-id: 8b221cae-3490-44cb-af06-85be4f8d280a
source-git-commit: ac167a205d2bc7e18d320bc1270da7e99bd05975
workflow-type: tm+mt
source-wordcount: '1209'
ht-degree: 32%

---

# トラックイベント

イベントデータをAdobe Experience Cloudに送信するには、 `sendEvent` コマンド この `sendEvent` コマンドは、 にデータを送信し、パーソナライズされたコンテンツ、ID、オーディエンスの宛先を取得する主な方法です。[!DNL Experience Cloud]

Adobe Experience Cloud に送信されるデータは、次の 2 つのカテゴリに分類されます。

* XDM データ
* XDM 以外のデータ

## XDM データの送信

XDM データは、Adobe Experience Platform 内で作成したスキーマとコンテンツと同じ構造を持つオブジェクトです。[スキーマの作成方法の詳細について説明します。](../../xdm/tutorials/create-schema-ui.md)

分析、パーソナライゼーション、オーディエンスまたは宛先の一部とする XDM データは、 `xdm` オプション。


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

間に時間がかかる場合があります `sendEvent` コマンドが実行され、データがサーバーに送信されたとき（例えば、Web SDK ライブラリが完全に読み込まれていない場合や、同意をまだ受け取っていない場合）。 の一部を変更する場合 `xdm` オブジェクトを `sendEvent` コマンドを使用する場合は、 `xdm` object _before_ 実行 `sendEvent` コマンド 以下に例を示します。

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

この例では、データレイヤーのクローンを作成し、JSON にシリアライズしてからデシリアライズします。 次に、クローン結果が `sendEvent` コマンド これにより、 `sendEvent` コマンドは、 `sendEvent` コマンドが実行され、元のデータレイヤーオブジェクトに対する後の変更が、サーバーに送信されたデータに反映されなくなりました。 イベント駆動型データレイヤーを使用している場合、データのクローンは既に自動的に処理されている可能性が高くなります。 例えば、 [Adobeクライアントデータレイヤー](https://github.com/adobe/adobe-client-data-layer/wiki)、 `getState()` メソッドは、以前のすべての変更に関する計算済みのクローンスナップショットを提供します。 また、Adobe Experience Platform Web SDK タグ拡張機能を使用している場合は、自動的に処理されます。

>[!NOTE]
>
>「 XDM 」フィールドの各イベントで送信できるデータの上限は 32 KB です。


## XDM　以外のデータの送信

XDM スキーマと一致しないデータは、 `data` オプション `sendEvent` コマンド この機能は、Web SDK のバージョン 2.5.0 以降でサポートされます。

これは、Adobe Targetプロファイルを更新したり、Target Recommendations属性を送信したりする必要がある場合に役立ちます。 [これらの Target 機能の詳細をご覧ください。](../personalization/adobe-target/target-overview.md#single-profile-update)

今後、 `data` 」オプションを選択し、XDM サーバーサイドにマッピングします。

**プロファイル属性とRecommendations属性をAdobe Targetに送信する方法：**

```javascript
alloy("sendEvent", {
  data: {
    __adobe: {
      target: {
        "profile.gender": "female",
        "profile.age": 30,
        "entity.id" : "123",
        "entity.genre" : "Drama"
      }
    }
  }
});
```


### `eventType` の設定 {#event-types}

XDM ExperienceEvent スキーマには、オプションの `eventType` フィールド ここには、レコードのプライマリイベントタイプが表示されます。イベントタイプを設定すると、送信する様々なイベントを区別するのに役立ちます。 XDM には、使用可能な事前定義済みのイベントタイプがいくつか用意されています。また、使用事例に応じて独自のカスタムイベントタイプを常に作成することもできます。 詳しくは、 XDM のドキュメントを参照してください。 [事前定義されたすべてのイベントタイプのリスト](../../xdm/classes/experienceevent.md#eventType).

これらのイベントタイプは、タグ拡張を使用する場合や、タグを使用せずに常に渡すことができる場合に、ドロップダウンに表示されます。 これらは、 `xdm` オプション。


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

### データセット ID の上書き

場合によっては、設定 UI で設定したデータセット以外のデータセットにイベントを送信する必要があります。 そのためには、 `datasetId` オプション `sendEvent` コマンド：


```javascript
var myXDMData = { ... };

alloy("sendEvent", {
  "xdm": myXDMData,
  "type": "commerce.checkout",
  "datasetId": "YOUR_DATASET_ID"
});
```

### ID 情報の追加

カスタム ID 情報はイベントに追加することもできます。 詳しくは、 [Experience CloudID の取得](../identity/overview.md).

## sendBeacon API の使用

Web ページのユーザーが離脱する直前にイベントデータを送信するのは、困難な場合があります。リクエストに時間がかかりすぎると、ブラウザーによってリクエストがキャンセルされる場合があります。一部のブラウザーではこの間に、データを簡単に収集できるよう、`sendBeacon` と呼ばれる Web 標準 API が実装されています。`sendBeacon` を使用する場合、ブラウザーはグローバルブラウジングコンテキストで Web リクエストをおこないます。これは、ブラウザーがバックグラウンドでビーコンリクエストをおこない、ページナビゲーションを保持しないことを意味します。Adobe Experience Platform [!DNL Web SDK] 使う `sendBeacon`、オプションを追加します。 `"documentUnloading": true` をイベントコマンドに追加します。  次に例を示します。


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

ブラウザーでは、`sendBeacon` で一度に送信できるデータの量に制限が設けられています。多くのブラウザーでは、上限は 64K です。ペイロードが大きすぎてブラウザーがイベントを拒否した場合、Adobe Experience Platform [!DNL Web SDK] は、通常のトランスポート方法（例えば、fetch）を使用してにフォールバックします。

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
}).then(function(result) {
    // Tracking the event succeeded.
  })
  .catch(function(error) {
    // Tracking the event failed.
  });
```


### 10. `result` object

10. `sendEvent` コマンドは、 `result` オブジェクト。 10. `result` オブジェクトには次のプロパティが含まれます。

**提案**:パーソナライゼーションオファー。訪問者に適したオファー。 [提案の詳細を説明します。](../personalization/rendering-personalization-content.md#manually-rendering-content)

**決定**:このプロパティは非推奨です。 代わりに `propositions` を使用してください。

**宛先**:外部のパーソナライゼーションプラットフォーム、コンテンツ管理システム、広告サーバー、お客様の Web サイトで実行されているその他のアプリケーションと共有できるAdobe Experience Platformのセグメント。 [宛先の詳細を説明します。](https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/personalization/custom-personalization.html?lang=en)

>[!WARNING]
>
>`destinations` は現在ベータ版です。 ドキュメントと機能は変更される場合があります。

**推論**:リアルタイムの機械学習のインサイト。 [リアルタイム機械学習の詳細を説明します。](https://experienceleague.adobe.com/docs/experience-platform/data-science-workspace/real-time-machine-learning/home.html?lang=en)

>[!WARNING]
>
>`inferences` は現在ベータ版です。 ドキュメントと機能は変更される場合があります。



## イベントのグローバルな変更 {#modifying-events-globally}

イベントのフィールドをグローバルに追加、削除、または変更する場合は、`onBeforeEventSend` コールバックを設定できます。このコールバックは、イベントが送信されるたびに呼び出されます。このコールバックは、`xdm` フィールドを含むイベントオブジェクトで渡されます。変更 `content.xdm` ：イベントと共に送信されるデータを変更します。


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

以下の `onBeforeEventSend` callback:

* イベント XDM は、コールバックの間に変更できます。 コールバックが返された後、 content.xdm および content.data オブジェクトの変更されたフィールドと値は、イベントと共に送信されます。

   ```javascript
   onBeforeEventSend: function(content){
     //sets a query parameter in XDM
     const queryString = window.location.search;
     const urlParams = new URLSearchParams(queryString);
     content.xdm.marketing.trackingCode = urlParams.get('cid')
   }
   ```

* このコールバックで例外がスローされると、イベントの処理が中断され、イベントは送信されません。
* このコールバックが `false`を呼び出すと、イベントの処理が中断され、エラーが発生せずにイベントが送信されなくなります。 このメカニズムにより、イベントデータを調べて `false` を返します。

   >[!NOTE]
   >ページの最初のイベントで false が返されないように、注意が必要です。 最初のイベントで false を返すと、パーソナライゼーションに悪影響を与える場合があります。

```javascript
   onBeforeEventSend: function(content) {
     // ignores events from bots
     if (MyBotDetector.isABot()) {
       return false;
     }
   }
```

ブール値以外の戻り値 `false` は、コールバックの後にイベントの処理と送信を許可します。

* イベントは、イベントタイプを調べることでフィルタリングできます ( [イベントタイプ](#event-types)):

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
