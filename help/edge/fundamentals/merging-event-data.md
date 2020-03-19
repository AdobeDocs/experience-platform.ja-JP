---
title: イベントデータのマージ
seo-title: Adobe Experience Platform Web SDKイベントデータの結合
description: エクスペリエンスプラットフォームWeb SDKイベントデータを結合する方法を説明します。
seo-description: エクスペリエンスプラットフォームWeb SDKイベントデータを結合する方法を説明します。
translation-type: tm+mt
source-git-commit: 0cc6e233646134be073d20e2acd1702d345ff35f

---


# （ベータ）イベントデータのマージ

>[!IMPORTANT]
>
>Adobe Experience Platform Web SDKは現在ベータ版で、すべてのユーザーが利用できるわけではありません。 ドキュメントと機能は変更される場合があります。

イベントの発生時に、一部のデータが利用できない場合があります。 ユーザーがブラウザーを閉じ _た場合_ 、失われないように、取得したデータを取り込むことができます。 一方、後で使用可能になるデータも含めることができます。

このような場合、次のようにコマンドにオプションとして渡すことで、データを以 `eventMergeId` 前のイベントとマー `event` ジできます。

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

この例では、両方のイベントコマンドに同じイベント結合ID値を渡すことで、2番目のイベントコマンドのデータが、1番目のイベントコマンドで以前に送信されたデータに増大します。 各イベントコマンドのレコードはExperience Data Platformで作成されますが、レポート時には、イベント結合IDを使用してレコードが結合され、単一のイベントとして表示されます。

特定のイベントに関するデータをサードパーティプロバイダーに送信する場合は、同じイベントマージIDをそのデータに含めることもできます。 後で、サードパーティのデータをAdobe Experience Platformに読み込む場合、イベントの結合IDは、Webページで発生した個別のイベントの結果として収集されたすべてのデータを結合するために使用されます。

## イベントマージIDの生成

イベントマージIDの値は任意の文字列を選択できますが、同じIDを使用して送信されるすべてのイベントが単一のイベントとしてレポートされるので、イベントをマージしない場合は一意性を強制するように注意してください。 SDKが(広く採用されている [UUID v4の仕様に従って](https://www.ietf.org/rfc/rfc4122.txt))一意のイベントマージIDを自分に代わって生成する場合は `createEventMergeId` 、コマンドを使用して生成できます。

SDKが読み込みを完了する前にコマンドを実行する可能性があるので、すべてのコマンドと同様に、promiseが返されます。 プロミスは、可能な限り早く一意のイベントマージIDを使用して解決されます。 次のように、プロミスが解決されるのを待ってから、サーバーにデータを送信できます。

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

他の理由（例えば、サードパーティプロバイダーに送信する場合）でイベントマージIDにアクセスする場合も、同じパターンに従います。

```javascript
var eventMergeIdPromise = alloy("createEventMergeId");

eventMergeIdPromise.then(function(eventMergeId) {
  // send event merge ID to a third-party provider
  console.log(eventMergeId);
});
```

## XDM形式に関する注意

イベントコマンド内で、は実 `mergeId` 際にペイロードに追加さ `xdm` れます。  必要に応じて、xdm `mergeId` オプションの一部として、次のように送信できます。

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
