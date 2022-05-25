---
title: Platform Web SDK での A4T データのクライアント側ログ
description: Experience PlatformWeb SDK を使用して、Adobe Analytics for Target(A4T) のクライアント側ログを有効にする方法について説明します。
seo-title: Client-side logging for A4T data in the Platform Web SDK
seo-description: Learn how to enable client-side logging for Adobe Analytics for Target (A4T) using the Experience Platform Web SDK.
keywords: target;A4T；ログ；Web SDK;Experience;Platform;
exl-id: 7071d7e4-66e0-4ab5-a51a-1387bbff1a6d
source-git-commit: fb0d8aedbb88aad8ed65592e0b706bd17840406b
workflow-type: tm+mt
source-wordcount: '1159'
ht-degree: 4%

---

# Platform Web SDK での A4T データのクライアント側ログ

## 概要 {#overview}

Adobe Experience Platform Web SDK を使用すると、 [Adobe Analytics for Target(A4T)](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=ja) web アプリケーションのクライアント側のデータ。

クライアント側のログは、 [!DNL Target] データがクライアント側で返され、データを収集して Analytics と共有できます。 このオプションは、 [Data Insertion API](https://experienceleague.adobe.com/docs/analytics/import/c-data-insertion-api.html).

>[!NOTE]
>
>を使用してこれを実行するメソッド [AppMeasurement.js](https://experienceleague.adobe.com/docs/analytics/implementation/js/overview.html?lang=ja) は現在開発中で、近い将来に提供される予定です。

このドキュメントでは、Web SDK 用のクライアント側 A4T ログを設定する手順を説明し、一般的な使用例の実装例を示します。

## 前提条件 {#prerequisites}

このチュートリアルでは、パーソナライゼーションのための Web SDK の使用に関する基本的な概念とプロセスに精通していることを前提としています。 紹介が必要な場合は、次のドキュメントを確認してください。

* [Web SDK の設定](../../../fundamentals/configuring-the-sdk.md)
* [イベントの送信](../../../fundamentals/tracking-events.md)
* [パーソナライゼーションコンテンツのレンダリング](../../rendering-personalization-content.md)

## Analytics クライアント側ログの設定 {#set-up-client-side-logging}

以下のサブセクションでは、Web SDK 実装で Analytics のクライアント側ログを有効にする方法について説明します。

### Analytics クライアント側ログを有効にする {#enable-analytics-client-side-logging}

お使いの実装で Analytics クライアント側ログを有効にすると見なすには、 [datastream](../../../datastreams/overview.md).

![Analytics データストリーム設定が無効です](../assets/disable-analytics-datastream.png)

### 取得 [!DNL A4T] SDK からのデータを取得し、Analytics に送信します。 {#a4t-to-analytics}

このレポート方法が正しく機能するには、 [!DNL A4T] から取得した関連データ [`sendEvent`](../../../fundamentals/tracking-events.md) 」コマンドを使用して、Analytics ヒットに追加されました。

Target Edge が提案応答を計算する際に、Analytics クライアント側のログが有効かどうか（つまり、データストリームで Analytics が無効な場合）を確認します。 クライアント側ログが有効な場合、応答の各提案に Analytics トークンが追加されます。

フローは次のようになります。

![クライアント側ログフロー](../assets/analytics-client-side-logging.png)

次に、 `interact` Analytics クライアント側ログが有効な場合の応答。 Analytics レポートを持つアクティビティの提案の場合、 `scopeDetails.characteristics.analyticsToken` プロパティ。

```json
{
  "requestId": "1234",
  "handle": [
    {
      "payload": [
        {
          "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
          "scope": "a4t-test",
          "scopeDetails": {
            "decisionProvider": "TGT",
            "activity": {
              "id": "434689"
            },
            "experience": {
              "id": "0"
            },
            "strategies": [
              {
                "algorithmID": "0",
                "trafficType": "0"
              }
            ],
            "characteristics": {
              "eventToken": "2lTS5KA6gj4JuSjOdhqUhGqipfsIHvVzTQxHolz2IpTMromRrB5ztP5VMxjHbs7c6qPG9UF4rvQTJZniWgqbOw==",
              "analyticsToken": "434689:0:0|2,434689:0:0|1"
            }
          },
          "items": [
            {
              "id": "1184844",
              "schema": "https://ns.adobe.com/personalization/html-content-item",
              "meta": {
                "geo.state": "bucuresti",
                "activity.id": "434689",
                "experience.id": "0",
                "activity.name": "a4t test form based activity",
                "offer.id": "1184844",
                "profile.tntId": "04608610399599289452943468926942466370-pybgfJ"
              },
              "data": {
                "id": "1184844",
                "format": "text/html",
                "content": "<div> analytics impressions </div>"
              }
            }
          ]
        },
        {
          "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
          "scope": "a4t-test",
          "scopeDetails": {
            "decisionProvider": "TGT",
            "activity": {
              "id": "434689"
            },
            "characteristics": {
              "eventToken": "E0gb6q1+WyFW3FMbbQJmrg==",
              "analyticsToken": "434689:0:0|32767"
            }
          },
          "items": [
            {
              "id": "434689",
              "schema": "https://ns.adobe.com/personalization/measurement",
              "data": {
                "type": "click",
                "format": "application/vnd.adobe.target.metric"
              }
            }
          ]
        }
      ],
      "type": "personalization:decisions",
      "eventIndex": 0
    }
  ]
}
```

フォームベースの Experience Composer アクティビティの提案には、同じ提案の下に、コンテンツとクリック指標項目の両方を含めることができます。 したがって、コンテンツの分析トークンを単一でで表示する代わりに、 `scopeDetails.characteristics.analyticsToken` プロパティの代わりに、ディスプレイとクリック分析トークンの両方を `scopeDetails.characteristics.analyticsTokens` オブジェクト、内 `display` および `click` 対応するプロパティ。

```json
{
  "requestId": "1234",
  "handle": [
    {
      "payload": [
        {
          "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
          "scope": "a4t-test",
          "scopeDetails": {
            "decisionProvider": "TGT",
            "activity": {
              "id": "434689"
            },
            "experience": {
              "id": "0"
            },
            "strategies": [
              {
                "algorithmID": "0",
                "trafficType": "0"
              }
            ],
            "characteristics": {
              "eventTokens": {
                "display": "2lTS5KA6gj4JuSjOdhqUhGqipfsIHvVzTQxHolz2IpTMromRrB5ztP5VMxjHbs7c6qPG9UF4rvQTJZniWgqbOw==",
                "click": "E0gb6q1+WyFW3FMbbQJmrg=="
              },
              "analyticsTokens": {
                "display": "434689:0:0|2,434689:0:0|1",
                "click": "434689:0:0|32767"
              }
            }
          },
          "items": [
            {
              "id": "1184844",
              "schema": "https://ns.adobe.com/personalization/html-content-item",
              "meta": {
                "geo.state": "bucuresti",
                "activity.id": "434689",
                "experience.id": "0",
                "activity.name": "a4t test form based activity",
                "offer.id": "1184844",
                "profile.tntId": "04608610399599289452943468926942466370-pybgfJ"
              },
              "data": {
                "id": "1184844",
                "format": "text/html",
                "content": "<div> analytics impressions </div>"
              }
            },
            {
              "id": "434689",
              "schema": "https://ns.adobe.com/personalization/measurement",
              "data": {
                "type": "click",
                "format": "application/vnd.adobe.target.metric"
              }
            }
          ]
        }
      ],
      "type": "personalization:decisions",
      "eventIndex": 0
    }
  ]
}
```

次の値すべて： `scopeDetails.characteristics.analyticsToken`、および `scopeDetails.characteristics.analyticsTokens.display` （表示されたコンテンツの場合）および `scopeDetails.characteristics.analyticsTokens.click` （クリック指標の場合）は、収集され、 `tnta` タグを [Data Insertion API](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md) 呼び出し。

>[!IMPORTANT]
>
>注意： `analyticsToken`/`analyticsTokens` プロパティには、複数のトークンを含めることができます。複数のトークンは、コンマで区切られた 1 つの文字列として連結されます。
>
>次の節で示す実装例では、複数の Analytics トークンが反復的に収集されています。 Analytics トークンの配列を連結するには、次のような関数を使用します。
>
>
```javascript
>var concatenateAnalyticsPayloads = function concatenateAnalyticsPayloads(analyticsPayloads) {
>   if (analyticsPayloads.size > 1) {
>       return [].concat(analyticsPayloads).join(',');
>   }
>   return [].concat(analyticsPayloads).join();
>};
>```

## 実装例 {#implementation-examples}

以下のサブセクションでは、一般的な使用例に対して Analytics のクライアント側ログを実装する方法を示します。

### フォームベースの Experience Composer アクティビティ {#form-based-composer}

Web SDK を使用して、提案の実行を [Adobe Target Form-Based Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html) アクティビティ。

特定の決定範囲の提案をリクエストする場合、返される提案には適切な Analytics トークンが含まれます。 ベストプラクティスは、Platform Web SDK を連携させることです `sendEvent` コマンドを実行し、返された提案を繰り返し処理して、Analytics トークンを同時に収集しながら実行します。

トリガーa `sendEvent` コマンドを使用して、次のようなフォームベースの Experience Composer アクティビティ範囲を設定できます。

```javascript
alloy("sendEvent", {
    "decisionScopes": ["a4t-test"],
    "xdm": {
      "web": {
        "webPageDetails": {
          "name": "Home Page"
        }
      }
    }
  }
).then(function(results) {
  for (var i = 0; i < results.propositions.length; i++) {
    //Execute the propositions and collect the Analytics payload
  }
});
```

ここから、提案を実行するコードを実装し、最終的に Analytics に送信されるペイロードを構築する必要があります。 これは例です `results.propositions` 次を含む可能性があります。

```json
[
  {
    "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
    "scope": "a4t-test",
    "scopeDetails": {
      "decisionProvider": "TGT",
      "activity": {
        "id": "434689"
      },
      "experience": {
        "id": "0"
      },
      "strategies": [
        {
          "algorithmID": "0",
          "trafficType": "0"
        }
      ],
      "characteristics": {
        "eventToken": "2lTS5KA6gj4JuSjOdhqUhGqipfsIHvVzTQxHolz2IpTMromRrB5ztP5VMxjHbs7c6qPG9UF4rvQTJZniWgqbOw==",
        "analyticsToken": "434689:0:0|2,434689:0:0|1"
      }
    },
    "items": [
      {
        "id": "1184844",
        "schema": "https://ns.adobe.com/personalization/html-content-item",
        "meta": {
          "geo.state": "bucuresti",
          "activity.id": "434689",
          "experience.id": "0",
          "activity.name": "a4t test form based activity",
          "offer.id": "1184844",
          "profile.tntId": "04608610399599289452943468926942466370-pybgfJ"
        },
        "data": {
          "id": "1184844",
          "format": "text/html",
          "content": "<div> analytics impressions </div>"
        }
      }
    ]
  },
  {
    "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
    "scope": "a4t-test",
    "scopeDetails": {
      "decisionProvider": "TGT",
      "activity": {
        "id": "434689"
      },
      "characteristics": {
        "eventToken": "E0gb6q1+WyFW3FMbbQJmrg==",
        "analyticsToken": "434689:0:0|32767"
      }
    },
    "items": [
      {
        "id": "434689",
        "schema": "https://ns.adobe.com/personalization/measurement",
        "data": {
          "type": "click",
          "format": "application/vnd.adobe.target.metric"
        }
      }
    ]
  },
  {
    "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
    "scope": "a4t-test",
    "scopeDetails": {
      "decisionProvider": "TGT",
      "activity": {
        "id": "434688"
      },
      "experience": {
        "id": "0"
      },
      "strategies": [
        {
          "algorithmID": "0",
          "trafficType": "0"
        }
      ],
      "characteristics": {
        "eventTokens": {
          "display": "91TS5KA6gj4JuSjOdhqUhGqipfsIHvVzTQxHolz2IpTMromRrB5ztP5VMxjHbs7c6qPG9UF4rvQTJZniWgqgEt==",
          "click": "Tagb6q1+WyFW3FMbbQJrtg=="
        },
        "analyticsTokens": {
          "display": "434688:0:0|2,434688:0:0|1",
          "click": "434688:0:0|32767"
        }
      }
    },
    "items": [
      {
        "id": "1184845",
        "schema": "https://ns.adobe.com/personalization/html-content-item",
        "meta": {
          "geo.state": "bucuresti",
          "activity.id": "434688",
          "experience.id": "0",
          "activity.name": "a4t test form based activity 1",
          "offer.id": "1184845"
        },
        "data": {
          "id": "1184845",
          "format": "text/html",
          "content": "<div> analytics impressions 1</div>"
        }
      },
      {
        "id": "434688",
        "schema": "https://ns.adobe.com/personalization/measurement",
        "data": {
          "type": "click",
          "format": "application/vnd.adobe.target.metric"
        }
      }
    ]
  }
]
```

コンテンツ項目を使用した提案から Analytics トークンを抽出するには、次のような関数を実装します。

```javascript
function getDisplayAnalyticsPayload(proposition) {
  if (!proposition || !proposition.scopeDetails || !proposition.scopeDetails.characteristics) {
    return;
  }
  var characteristics = proposition.scopeDetails.characteristics;
  if (characteristics.analyticsTokens) {
    return characteristics.analyticsTokens.display;
  }
  return characteristics.analyticsToken;
}
```

提案は、 `schema` 対象の項目のプロパティ。 フォームベースの Experience Composer アクティビティでは、次の 4 つの提案項目スキーマがサポートされます。

```javascript
var HTML_SCHEMA = "https://ns.adobe.com/personalization/html-content-item";
var MEASUREMENT_SCHEMA = "https://ns.adobe.com/personalization/measurement";
var JSON_SCHEMA = "https://ns.adobe.com/personalization/json-content-item";
var REDIRECT_SCHEMA = "https://ns.adobe.com/personalization/redirect-item";
```

`HTML_SCHEMA` および `JSON_SCHEMA` は、オファーのタイプを反映するスキーマです。 `MEASUREMENT_SCHEMA` は、DOM 要素に添付する必要がある指標を反映します。

訪問者が実際に以前に表示したコンテンツをクリックした時点で、クリック指標の Analytics ペイロードを収集し、コンテンツ項目とは別に Analytics に送信する必要があります。

この場合、A4T ペイロード数のクリック指標を取得する次のヘルパー関数が役に立ちます。

```javascript
function getClickAnalyticsPayload(proposition) {
  if (!proposition || !proposition.scopeDetails || !proposition.scopeDetails.characteristics) {
    return;
  }
  var characteristics = proposition.scopeDetails.characteristics;
  if (characteristics.analyticsTokens) {
    return characteristics.analyticsTokens.click;
  }
  return characteristics.analyticsToken;
}
```

#### 実装の概要 {#implementation-summary}

要約すると、フォームベースの Experience Composer アクティビティを Platform Web SDK で適用する際には、次の手順を実行する必要があります。

1. フォームベースの Experience Composer アクティビティオファーを取得するイベントを送信する
1. コンテンツの変更をページに適用する。
1. を `decisioning.propositionDisplay` 通知イベント；
1. SDK 応答から Analytics 表示トークンを収集し、Analytics ヒットのペイロードを作成する。
1. ペイロードを [Data Insertion API](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md);
1. 配信された提案にクリック指標がある場合、クリックが実行されたときに `decisioning.propositionInteract` 通知イベント。 この `onBeforeEventSend` 傍受時に `decisioning.propositionInteract` イベント、次のアクションが発生します。
   1. クリック分析トークンの収集元 `xdm._experience.decisioning.propositions`
   1. クリック分析ヒットを、収集した Analytics ペイロードと共にを介して送信する [Data Insertion API](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md);

```javascript
alloy("sendEvent", {
    "decisionScopes": ["a4t-test"],
    "xdm": {
      "web": {
        "webPageDetails": {
          "name": "Home Page"
        }
      }
    }
  }
).then(function(results) {
  var analyticsPayload = new Set();
  results.propositions.forEach(function (proposition) {
    proposition.items.forEach(function (item) {
      if (item.schema === HTML_SCHEMA) {
        // 1. Apply offer
        // 2. Collect executed propositions and send the decisioning.propositionDisplay notification event
        // 3. Collect the display Analytics tokens
      }
      if (item.schema === MEASUREMENT_SCHEMA) {
        // Setup click listener, so that when clicked:
        // 1. Collect clicked propositions and send the decisioning.propositionInteract notification event
        // Note: onBeforeEventSend handler should be configured, so that when intercepting decisioning.propositionInteract events:
        //   1. Collect the click Analytics tokens from xdm._experience.decisioning.propositions
        //   2. Send the click Analytics hit with the collected Analytics payload via Data Insertion API
      }
    });
  });
  // Send the page view Analytics hit with the collected display Analytics payload via Data Insertion API
});
```

### Visual Experience Composer のアクティビティ {#visual-experience-composer-acitivties}

Web SDK を使用すると、 [Visual Experience Composer(VEC)](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html).

>[!NOTE]
>
>この使用例を実装する手順は、 [フォームベースの Experience Composer アクティビティ](#form-based-composer). 詳しくは、前の節を参照してください。

自動レンダリングが有効な場合、ページ上で実行された提案から Analytics トークンを収集できます。 ベストプラクティスは、Platform Web SDK を連携させることです `sendEvent` コマンドを繰り返し処理して、返された提案を繰り返し処理し、Web SDK がレンダリングしようとした提案をフィルタリングします。

**例**

```javascript
alloy("sendEvent", {
    "renderDecisions": true,
    "xdm": {
      "web": {
        "webPageDetails": {
          "name": "Home Page"
        }
      }
    }
  }
).then(function (results) {
  var analyticsPayloads = new Set();
  
  for (var i = 0; i < results.propositions.length; i++) {
  
    var proposition = propositions[i];
    var renderAttempted = proposition.renderAttempted;

    if (renderAttempted === true) {
      var analyticsPayload = getDisplayAnalyticsPayload(proposition);
      
      if (analyticsPayload !== undefined) {
        analyticsPayloads.add(analyticsPayload);
      }
    }
  }
  var analyticsPayloadsToken = concatenateAnalyticsPayloads(analyticsPayloads);
  // Send the page view Analytics hit with collected Analytics payload via Data Insertion API
});
```

### 使用 `onBeforeEventSend` ページ指標を処理するには {#using-onbeforeeventsend}

Adobe Targetアクティビティを使用すると、DOM に手動で関連付けるか、DOM に自動的に関連付ける（VEC が作成したアクティビティ）など、ページ上で様々な指標を設定できます。 どちらのタイプも、Web ページでのエンドユーザー間のやり取りが遅れます。

これを考慮するためのベストプラクティスは、 `onBeforeEventSend` Adobe Experience Platform Web SDK フック この `onBeforeEventSend` フックは、 `configure` コマンドに含まれ、データストリームを介して送信されるすべてのイベントに反映されます。

次に、 `onBeforeEventSent` は、Analytics のヒットをトリガーするように設定できます。

```javascript
alloy("configure", {
  edgeConfigId: "datastream configuration ID",
  orgId: "adobe ORG ID",
  onBeforeEventSend: function(options) {
    const xdm = options.xdm;
    const eventType = xdm.eventType;
    if (eventType === "decisioning.propositionInteract") {
      const analyticsPayloads = new Set();
      const propositions = xdm._experience.decisioning.propositions;

      for (var i = 0; i < propositions.length; i++) {
        var proposition = propositions[i];
        analyticsPayloads.add(getClickAnalyticsPayload(proposition));
      }
      // Trigger the Analytics hit
    }
  }
});
```

## 次の手順 {#next-steps}

このガイドでは、Web SDK の A4T データのクライアント側ログについて説明しました。 詳しくは、 [サーバー側ログ](server-side.md) を参照してください。
