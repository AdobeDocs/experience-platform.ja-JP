---
title: イベントデータの結合
seo-title: Adobe Experience Platform Web SDK：イベントデータの結合
description: Experience Platform Web SDK イベントデータの結合方法について説明します
seo-description: Experience Platform Web SDK イベントデータの結合方法について説明します
translation-type: tm+mt
source-git-commit: 0cc6e233646134be073d20e2acd1702d345ff35f

---


# （ベータ版）イベントデータの結合

>[!IMPORTANT]
>
>Adobe Experience Platform Web SDK は現在ベータ版で、すべてのユーザーが利用できるわけではありません。ドキュメントと機能は変更される場合があります。

イベントが発生すると、一部のデータが利用できなくなる場合があります。ユーザーがブラウザーを閉じた場合などに、データが失われないよう、_取得した_&#x200B;データを取り込むことができます。一方、後で使用できるようデータも含めることができます。

このような場合、次のようにオプションとして `eventMergeId` を `event` コマンドに渡すことで、データを以前のイベントと結合できます。

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
  "eventMergeId": "ABC123"
});

// Time passes and more data becomes available

alloy("event", {
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

この例では、両方のイベントコマンドに同じイベント結合 ID 値を渡すことで、1 番目のイベントコマンドで以前に送信されたデータを、2 番目のイベントコマンドのデータで強化します。各イベントコマンドのレコードは Experience Data Platform で作成されますが、報告時には、イベント結合 ID を使用してレコードを結合し、単一のイベントとして表示します。

特定のイベントに関するデータをサードパーティプロバイダーに送信する場合は、同じイベント結合 ID をそのデータに含めることもできます。サードパーティのデータを後から Adobe Experience Platform に読み込む場合、イベントの結合 ID は、Web ページで発生した個別のイベントの結果として収集されたすべてのデータを結合するために使用されます。

## イベント結合 ID の生成

イベント結合 ID の値には任意の文字列を選択できます。ただし、同じ ID を使用して送信されるすべてのイベントは単一のイベントとしてレポートされるので、イベントを結合しない場合は一意性を強化するように注意してください。自分に変わって SDK で一意のイベント結合 ID を生成する場合（広く採用されている [UUID v4 の仕様](https://www.ietf.org/rfc/rfc4122.txt)に従って）一意のイベント結合 ID を自分に代わって生成する場合は は、`createEventMergeId` コマンドを使用できます。

SDK が読み込みを完了する前にユーザーがコマンドを実行する可能性があるので、すべてのコマンドと同様に、promise が返されます。Promise は、可能な限り早く一意のイベント結合 ID を使用して解決されます。次のように、promise が解決されるのを待ってから、サーバーにデータを送信できます。

```javascript
var eventMergeIdPromise = alloy("createEventMergeId");

eventMergeIdPromise.then(function(eventMergeId) {
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
    "mergeId": eventMergeId
  });
});

// Time passes and more data becomes available

eventMergeIdPromise.then(function(eventMergeId) {
  alloy("event", {
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
    "mergeId": eventMergeId
  });
});
```

他の理由（例えば、サードパーティプロバイダーに送信する場合）でイベント結合 ID にアクセスする場合も、同じパターンに従います。

```javascript
var eventMergeIdPromise = alloy("createEventMergeId");

eventMergeIdPromise.then(function(eventMergeId) {
  // send event merge ID to a third-party provider
  console.log(eventMergeId);
});
```

## XDM 形式に関する注意事項

イベントコマンド内で、`mergeId` は実際に `xdm` ペイロードに追加されます。必要に応じて、xdm オプションの一部として、次のように `mergeId` を送信できます。

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
    },
    "eventMergeId": "ABC123"
  }
});
```
