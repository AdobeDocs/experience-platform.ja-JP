---
title: Adobe Experience PlatformWeb SDKでのイベントデータの結合
description: Experience Platform Web SDK イベントデータの結合方法について説明します
keywords: merge;イベントデータ；eventMergeId;createEventMergeId;sendEvent;mergeId;merge id;eventMergeIdPromise;Merge Id Promise;
translation-type: tm+mt
source-git-commit: 69f2e6069546cd8b913db453dd9e4bc3f99dd3d9
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 48%

---


# イベントデータの結合

>[!IMPORTANT]
>
>この機能は現在開発中です。 このページで説明されているように、一部のソリューションでイベントデータを結合できるわけではありません。

イベントが発生すると、一部のデータが利用できなくなる場合があります。ユーザーがブラウザーを閉じた場合などに、データが失われないよう、取得したデータを取り込むことができます。一方、後で使用できるようデータも含めることができます。

このような場合、次のようにオプションとして `mergeId` を `event` コマンドに渡すことで、データを以前のイベントと結合できます。

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
  },
  "mergeId": "ABC123"
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
  },
  "mergeId": "ABC123"
});
```

この例では、`mergeId`オプションに同じ値を両方のイベントコマンドに渡すことで、2番目のイベントコマンドのデータは、最初のイベントコマンドで以前に送信されたデータに増大します。 [!DNL Experience Data Platform]には各イベントコマンドのレコードが作成されますが、レポート中はイベント結合IDを使用してレコードが結合され、単一のイベントとして表示されます。

特定のイベントに関するデータをサードパーティプロバイダーに送信する場合は、同じイベント結合 ID をそのデータに含めることもできます。後で、サードパーティのデータをAdobe Experience Platformに読み込む場合は、イベント結合IDを使用して、Webページ上で発生した個別のイベントの結果として収集されたすべてのデータが結合されます。

## イベント結合 ID の生成

イベント結合IDには任意の文字列を指定できますが、同じIDを使用して送信されるすべてのイベントは1つのイベントとしてレポートされるので、イベントを結合しない場合は一意性を強化するように注意してください。 自分に変わって SDK で一意のイベント結合 ID を生成する場合（広く採用されている [UUID v4 の仕様](https://www.ietf.org/rfc/rfc4122.txt)に従って）一意のイベント結合 ID を自分に代わって生成する場合は は、`createEventMergeId` コマンドを使用できます。

SDK が読み込みを完了する前にユーザーがコマンドを実行する可能性があるので、すべてのコマンドと同様に、promise が返されます。Promise は、可能な限り早く一意のイベント結合 ID を使用して解決されます。次のように、promise が解決されるのを待ってから、サーバーにデータを送信できます。

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
    },
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
    },
    "mergeId": results.eventMergeId
  });
});
```

他の理由（例えば、サードパーティプロバイダーに送信する場合）でイベント結合 ID にアクセスする場合も、同じパターンに従います。

```javascript
var eventMergeIdPromise = alloy("createEventMergeId");

eventMergeIdPromise.then(function(results) {
  // send event merge ID to a third-party provider
  console.log(results.eventMergeId);
});
```

## XDM 形式に関する注意事項

イベントコマンド内で、イベントマージIDは、お客様に代わって正しい場所の`xdm`ペイロードに追加されます。  必要に応じて、次のように、`xdm`オプションの一部としてイベントマージIDを送信できます。

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

イベント結合IDを`xdm`オブジェクトに直接追加する場合は、`mergeId`の代わりに`eventMergeID`という名前が使用されていることに注意してください。
