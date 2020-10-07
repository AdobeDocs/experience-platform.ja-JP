---
title: イベントデータの結合
seo-title: Adobe Experience Platform Web SDK：イベントデータの結合
description: Experience Platform Web SDK イベントデータの結合方法について説明します
seo-description: Experience Platform Web SDK イベントデータの結合方法について説明します
keywords: merge;event data;eventMergeId;createEventMergeId;sendEvent;mergeId;merge id;eventMergeIdPromise; Merge Id Promise;
translation-type: tm+mt
source-git-commit: a362b67cec1e760687abb0c22dc8c46f47e766b7
workflow-type: tm+mt
source-wordcount: '408'
ht-degree: 42%

---


# イベントデータの結合

>[!IMPORTANT]
>
>この機能は現在開発中です。 このページで説明されているように、一部のソリューションでイベントデータを結合できるわけではありません。

イベントが発生すると、一部のデータが利用できなくなる場合があります。ユーザーがブラウザーを閉じた場合などに、データが失われないよう、取得したデータを取り込むことができます。一方、後で使用できるようデータも含めることができます。

このような場合、次のようにオプションとして `eventMergeId` を `event` コマンドに渡すことで、データを以前のイベントと結合できます。

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
  "eventMergeId": "ABC123"
});

// Time passes and more data becomes available

alloy("sendEvent", {
  "xdm": {
    "commerce": {
      "order": {
        "payments": [
          {
            "transactionID": "TR426941",
            "paymentAmount": 999.98,
            "paymentType": "credit_card",
            "currencyCode": "USD"
          }
        ]
      }
    }
  }
  "eventMergeId": "ABC123"
});
```

By passing the same `eventMergeID` value to both event commands in this example, the data in the second event command is augmented to data previously sent on the first event command. A record for each event command is created in the [!DNL Experience Data Platform], but during reporting the records are joined together using the `eventMergeID` and appear as a single event.

If you are sending data about a particular event to third-party providers, you can include the same `eventMergeID` with that data as well. Later, if you choose to import the third-party data into the Adobe Experience Platform, `eventMergeID` will be used to merge together all data that was collected as a result of the discrete event that occurred on your webpage.

## JavaScriptでの `eventMergeID`

The `eventMergeID` value can be any string you choose, but remember that all events sent using the same ID are reported as a single event, so be careful to enforce uniqueness when events should not be merged. If you would like the SDK to generate a unique `eventMergeID` on your behalf (following the widely-adopted [UUID v4 specification](https://www.ietf.org/rfc/rfc4122.txt)), you can use the `createEventMergeId` command to do so.

SDK が読み込みを完了する前にユーザーがコマンドを実行する可能性があるので、すべてのコマンドと同様に、promise が返されます。The promise will be resolved with a unique `eventMergeID` as soon as possible. 次のように、promise が解決されるのを待ってから、サーバーにデータを送信できます。

```javascript
var eventMergeIdPromise = alloy("createEventMergeId");

eventMergeIdPromise.then(function(results) {
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
    "mergeId": results.eventMergeId
  });
});

// Time passes and more data becomes available

eventMergeIdPromise.then(function(results) {
  alloy("sendEvent", {
    "xdm": {
      "commerce": {
        "order": {
          "payments": [
            {
              "transactionID": "TR426941",
              "paymentAmount": 999.98,
              "paymentType": "credit_card",
              "currencyCode": "USD"
            }
          ]
        }
      }
    }
    "mergeId": results.eventMergeId
  });
});
```

Follow this same pattern if you would like access to the `eventMergeID` for other reasons (for example, to send it to a third-party provider):

```javascript
var eventMergeIdPromise = alloy("createEventMergeId");

eventMergeIdPromise.then(function(results) {
  // send event merge ID to a third-party provider
  console.log(results.eventMergeId);
});
```

## XDM 形式に関する注意事項

イベントコマンド内で、`mergeId` は実際に `xdm` ペイロードに追加されます。必要に応じて、xdm オプションの一部として、次のように `mergeId` を送信できます。

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
    },
    "eventMergeId": "ABC123"
  }
});
```
