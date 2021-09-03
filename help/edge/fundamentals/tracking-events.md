---
title: Adobe Experience Platform Web SDKを使用したイベントの追跡
description: Adobe Experience Platform Web SDKのイベントの追跡方法について説明します。
keywords: sendEvent;xdm;eventType;datasetId;sendBeacon;sendBeacon;send Beacon;documentUnloading;document Unloading;onBeforeEventSend;
exl-id: 8b221cae-3490-44cb-af06-85be4f8d280a
source-git-commit: 53a14b2b7d7ca8bdd278f2aeec2c2e8a30fdac7b
workflow-type: tm+mt
source-wordcount: '1082'
ht-degree: 34%

---

# トラックイベント

イベントデータをAdobe Experience Cloudに送信するには、`sendEvent`コマンドを使用します。 この `sendEvent` コマンドは、 にデータを送信し、パーソナライズされたコンテンツ、ID、オーディエンスの宛先を取得する主な方法です。[!DNL Experience Cloud]

Adobe Experience Cloud に送信されるデータは、次の 2 つのカテゴリに分類されます。

* XDM データ
* XDM以外のデータ

## XDM データの送信

XDM データは、Adobe Experience Platform 内で作成したスキーマとコンテンツと同じ構造を持つオブジェクトです。[スキーマの作成方法の詳細について説明します。](../../xdm/tutorials/create-schema-ui.md)

分析、パーソナライゼーション、オーディエンスまたは宛先の一部とするXDMデータは、`xdm`オプションを使用して送信する必要があります。


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

`sendEvent`コマンドが実行されてからデータがサーバーに送信されるまでに時間がかかる場合があります（例えば、Web SDKライブラリが完全に読み込まれていないか、同意をまだ受け取っていない場合）。 `sendEvent`コマンドの実行後に`xdm`オブジェクトの一部を変更する場合は、`sendEvent`コマンドの実行前に`xdm`オブジェクト&#x200B;_をコピーすることを強くお勧めします。_&#x200B;次に例を示します。

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

この例では、データレイヤーをJSONにシリアライズし、デシリアライズすることで、データレイヤーを複製します。 次に、クローン結果が`sendEvent`コマンドに渡されます。 これにより、`sendEvent`コマンドが実行されたときのデータレイヤーのスナップショットが`sendEvent`コマンドに含まれ、後で元のデータレイヤーオブジェクトに対する変更がサーバーに送信されたデータに反映されなくなります。 イベント駆動型データレイヤーを使用している場合、データのクローンは既に自動的に処理されている可能性が高くなります。 例えば、[Adobeクライアントデータレイヤー](https://github.com/adobe/adobe-client-data-layer/wiki)を使用している場合、`getState()`メソッドは、以前のすべての変更に関する計算済みのクローンスナップショットを提供します。 また、Adobe Experience Platform Web SDKタグ拡張機能を使用している場合は、自動的に処理されます。

>[!NOTE]
>
>XDMフィールドの各イベントで送信できるデータには、32 KBの制限があります。


## XDM　以外のデータの送信

XDMスキーマと一致しないデータは、`sendEvent`コマンドの`data`オプションを使用して送信する必要があります。 この機能は、Web SDKのバージョン2.5.0以降でサポートされています。

これは、Adobe Targetプロファイルを更新するか、Target Recommendations属性を送信する必要がある場合に役立ちます。 [Targetのこれらの機能について詳しくは、こちらを参照してください。](../personalization/adobe-target/target-overview.md#single-profile-update)

将来は、`data`オプションの下の完全なデータレイヤーを送信し、XDMサーバー側にマッピングできます。

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

XDM ExperienceEventスキーマには、オプションの`eventType`フィールドがあります。 ここには、レコードのプライマリイベントタイプが表示されます。イベントタイプを設定すると、送信する様々なイベントを区別するのに役立ちます。 XDMには、使用できる定義済みのイベントタイプがいくつか用意されています。また、使用事例に合わせて独自のカスタムイベントタイプを常に作成することもできます。 XDMのドキュメントを参照して、事前定義されたすべてのイベントタイプ](../../xdm/classes/experienceevent.md#eventType)の[リストを確認してください。

これらのイベントタイプは、タグ拡張を使用する場合や、タグを使用せずに常に渡すことができる場合に、ドロップダウンに表示されます。 これらは、`xdm`オプションの一部として渡すことができます。


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

カスタムID情報をイベントに追加することもできます。 [Experience CloudID](../identity/overview.md)の取得を参照してください。

## sendBeacon API の使用

Web ページのユーザーが離脱する直前にイベントデータを送信するのは、困難な場合があります。リクエストに時間がかかりすぎると、ブラウザーによってリクエストがキャンセルされる場合があります。一部のブラウザーではこの間に、データを簡単に収集できるよう、`sendBeacon` と呼ばれる Web 標準 API が実装されています。`sendBeacon` を使用する場合、ブラウザーはグローバルブラウジングコンテキストで Web リクエストをおこないます。これは、ブラウザーがバックグラウンドでビーコンリクエストをおこない、ページナビゲーションを保持しないことを意味します。Adobe Experience Platform [!DNL Web SDK]に`sendBeacon`を使用するように伝えるには、イベントコマンドにオプション`"documentUnloading": true`を追加します。  次に例を示します。


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

ブラウザーでは、`sendBeacon` で一度に送信できるデータの量に制限が設けられています。多くのブラウザーでは、上限は 64K です。ペイロードが大きすぎてブラウザーがイベントを拒否した場合、Adobe Experience Platform [!DNL Web SDK]は、通常のトランスポート方法（フェッチなど）を使用してフォールバックします。

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

`onBeforeEventSend`コールバックに関する注意事項を次に示します。

* コールバック中にイベントXDMを変更できます。 コールバックが返された後、
content.xdmおよびcontent.dataオブジェクトは、イベントと共に送信されます。

   ```javascript
   onBeforeEventSend: function(content){
     //sets a query parameter in XDM
     const queryString = window.location.search;
     const urlParams = new URLSearchParams(queryString);
     content.xdm.marketing.trackingCode = urlParams.get('cid')
   }
   ```

* このコールバックで例外がスローされると、イベントの処理が中断され、イベントは送信されません。
* このコールバックが`false`のブール値を返した場合、イベント処理は中断され、
エラーがない場合、イベントは送信されません。 このメカニズムにより、特定のイベントを簡単に無視できます。
イベントデータを調べ、イベントを送信しない場合は`false`を返す。

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

ブール値`false`以外の戻り値を使用すると、コールバックの後にイベントを処理して送信できます。

* イベントは、イベントタイプを調べることでフィルタリングできます（[イベントタイプ](#event-types)を参照）。

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
