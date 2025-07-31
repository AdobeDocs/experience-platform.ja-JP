---
title: at.js とExperience Platform web SDKの比較
description: at.js 機能とExperience Platform web SDKの比較
keywords: target;adobe target;activity.id;experience.id;renderDecisions;decisionScopes；事前非表示スニペット；vec；フォームベースの Experience Composer;xdm；オーディエンス；決定；範囲；スキーマ；システム図；図
exl-id: b63fe47d-856a-4cae-9057-51917b3e58dd
source-git-commit: 35429ec2dffacb9c0f2c60b608561988ea487606
workflow-type: tm+mt
source-wordcount: '2183'
ht-degree: 4%

---

# at.js ライブラリと Web SDK の比較

## 概要

ここでは、`at.js` ライブラリとExperience Platform Web SDKの違いの概要を説明します。

## ライブラリのインストール

### at.js のインストール

お客様は、Adobe Experience Cloudの「実装」タブから直接ライブラリをダウンロードできます。 at.js ライブラリは、顧客が持つ設定（clientCode、imsOrgId など）によってカスタマイズされます。

### Web SDKのインストール

事前ビルドバージョンは CDN で使用できます。 CDN のライブラリをページで直接参照するか、ダウンロードして独自のインフラストラクチャにホストすることができます。 縮小形式と非縮小形式で利用できます。 デバッグの目的では、非縮小バージョンが役立ちます。

詳しくは [JavaScript ライブラリを使用した web SDKのインストール ](/help/web-sdk/install/library.md) を参照してください。

## ライブラリの設定

### at.js の設定

すべての at.js ファイルの末尾には、設定オブジェクトをインスタンス化して渡すセクションがあります。 カスタマイズ可能で、ダウンロード時に、そのセクションに現在の顧客設定を入力します。

```javascript
window.adobe.target.init(window, document, {
  "clientCode": "demo",
  "imsOrgId": "",
  "serverDomain": "localhost:5000",
  "timeout": 2000,
  "globalMboxName": "target-global-mbox",
  "version": "2.0.0",
  "defaultContentHiddenStyle": "visibility: hidden;",
  "defaultContentVisibleStyle": "visibility: visible;",
  "bodyHiddenStyle": "body {opacity: 0 !important}",
  "bodyHidingEnabled": true,
  "deviceIdLifetime": 63244800000,
  "sessionIdLifetime": 1860000,
  "selectorsPollingTimeout": 5000,
  "visitorApiTimeout": 2000,
  "overrideMboxEdgeServer": false,
  "overrideMboxEdgeServerTimeout": 1860000,
  "optoutEnabled": false,
  "optinEnabled": false,
  "secureOnly": false,
  "supplementalDataIdParamTimeout": 30,
  "authoringScriptUrl": "//cdn.tt.omtrdc.net/cdn/target-vec.js",
  "urlSizeLimit": 2048,
  "endpoint": "/rest/v1/delivery",
  "pageLoadEnabled": true,
  "viewsEnabled": true,
  "analyticsLogging": "server_side",
  "serverState": {},
  "decisioningMethod": "server-side",
  "legacyBrowserSupport":  false
});
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/targetgobalsettings.html)


### Web SDKの設定

SDKの設定は、[`configure`](/help/web-sdk/commands/configure/overview.md) コマンドで行います。 `configure` コマンドは *常に* 最初に呼び出されます。

## ページ読み込み Target オファーをリクエストし自動的にレンダリングする方法

### at.js の使用

at.js 2.x を使用している場合は、`pageLoadEnabled` 設定を有効にすると、ライブラリは `execute -> pageLoad` を使用して Target Edgeへの呼び出しをトリガーします。 すべての設定がデフォルト値に設定されている場合、カスタムコーディングは必要ありません。at.js がページに追加され、ブラウザーによって読み込まれると、Target Edge呼び出しが実行されます。

### Web SDKの使用

Adobe Target[Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) 内で作成されたコンテンツは、SDKによって自動的に取得およびレンダリングできます。

Target オファーをリクエストして自動的にレンダリングするには、`sendEvent` コマンドを使用し、`renderDecisions` オプションを `true` に設定します。 これにより、自動レンダリングの対象となる、パーソナライズされたコンテンツがSDKで自動的にレンダリングされます。

例：

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
});
```

Experience Platform Web SDKは、WEB SDKが実行したオファーを含む通知を自動的に送信します。通知リクエストペイロードの例を次に示します。

```json
{
  "events": [{
      "xdm": {
        "_experience": {
          "decisioning": {
            "propositions": [
              {
                "id": "AT:eyJhY3Rpdml0eUlkIjoiMTI3MDE5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
                "scope": "cart",
                "scopeDetails": {
                  "decisionProvider": "TGT",
                  "activity": {
                    "id": "127019"
                  },
                  "experience": {
                    "id": "0"
                  },
                  "strategies": [
                    {
                      "step": "entry",
                      "algorithmID": "0",
                      "trafficType": "0"
                    },
                    {
                      "step": "display",
                      "algorithmID": "0",
                      "trafficType": "0"
                    }
                  ],
                  "characteristics": {
                    "eventToken": "bKMxJ8dCR1XlPfDCx+2vSGqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="
                  }
                }
              }
            ]
          }
        },
        "eventType": "display",
        "web": {
          "webPageDetails": {
            "viewName": "cart",
            "URL": "https://alloyio.com/personalizationSpa/cart"
          },
          "webReferrer": {
            "URL": ""
          }
        },
        "device": {
          "screenHeight": 800,
          "screenWidth": 1280,
          "screenOrientation": "landscape"
        },
        "environment": {
          "type": "browser",
          "browserDetails": {
            "viewportWidth": 1280,
            "viewportHeight": 284
          }
        },
        "placeContext": {
          "localTime": "2021-12-10T15:50:34.467+02:00",
          "localTimezoneOffset": -120
        },
        "timestamp": "2021-12-10T13:50:34.467Z",
        "implementationDetails": {
          "name": "https://ns.adobe.com/experience/alloy",
          "version": "2.6.2",
          "environment": "browser"
        }
      }
    }
  ]
}
```

[詳細情報](../rendering-personalization-content.md)

## ページ読み込み Target オファーをリクエストし、自動的にレンダリングしない方法

### at.js の使用

ページ読み込み用にオファーを取得する Target Edgeを呼び出す方法は 2 つあります。

例 1：

```javascript
adobe.target.getOffer({
   mbox: "target-global-mbox", 
   success: console.log,
   error: console.error
});
```

例 2：

```javascript
adobe.target.getOffers({
    request: {
      execute: {
        pageLoad: {}
    }
  }
})
.then(console.log)
.catch(console.error);
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/cmp-atjs-functions.html)

### Web SDKの使用

`sendEvent`: `decisionScopes` の下で、特別なスコープを持つ `__view__` コマンドを実行します。 このスコープをシグナルとして使用し、Target からすべてのページ読み込みアクティビティを取得し、すべてのビューをプリフェッチします。 また、Web SDKは、すべての VEC ビューベースのアクティビティの評価も試みます。 ビューのプリフェッチの無効化は、現在 Web SDKではサポートされていません。

パーソナライゼーションコンテンツにアクセスするには、コールバック関数を指定します。この関数は、SDKがサーバーから正常に応答を受け取った後に呼び出されます。 コールバックには、返されたパーソナライゼーションコンテンツを含む propositions プロパティを含めることができる結果オブジェクトが提供されます。

例：

```javascript
alloy("sendEvent", {
    xdm: {...},
    decisionScopes: ["__view__"]
  }).then(function(result) {
    if (result.propositions) {
      result.propositions.forEach(proposition => {
        proposition.items.forEach(item => {
          if (item.schema === HTML_SCHEMA) {
            // manually apply offer
            document.getElementById("form-based-offer-container").innerHTML =
              item.data.content;
            const executedPropositions = [
              {
                id: proposition.id,
                scope: proposition.scope,
                scopeDetails: proposition.scopeDetails
              }
            ];
          // manually send the display notification event, so that Target/Analytics impressions aare increased
            alloy("sendEvent",{
              "xdm": {
                "eventType": "decisioning.propositionDisplay",
                "_experience": {
                  "decisioning": {
                    "propositions": executedPropositions
                  }
                }
              }
            });
          }
        });
      });
    }
  });
```

[詳細情報](../rendering-personalization-content.md#manually-rendering-content)


## 特定のフォームベースのターゲット mbox のリクエスト方法


### at.js の使用

`getOffer` の関数を使用して、フォームベースのコンポーザーのアクティビティを取得できます。

例 1：

```javascript
adobe.target.getOffer({
   mbox: "hero-banner", 
   success: console.log,
   error: console.error
});
```

例 2：

```javascript
adobe.target.getOffers({
    request: {
      execute: {
        mboxes: [
        {
          index: 0,
          name: "hero-banner"
        }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/cmp-atjs-functions.html)


### Web SDKの使用

Form Based Composer ベースのアクティビティを取得するには、`sendEvent` コマンドを使用して、`decisionScopes` オプションで mbox 名を渡します。 `sendEvent` コマンドは、リクエストされたアクティビティ/提案を含むオブジェクトで解決される promise を返します。
`propositions` 配列は次のようになります。

```javascript
[
  {
    "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
    "scope": "hero-banner",
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
        "eventToken": "2lTS5KA6gj4JuSjOdhqUhGqipfsIHvVzTQxHolz2IpTMromRrB5ztP5VMxjHbs7c6qPG9UF4rvQTJZniWgqbOw=="
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
    "scope": "hero-banner",
    "scopeDetails": {
      "decisionProvider": "TGT",
      "activity": {
        "id": "434689"
      },
      "characteristics": {
        "eventToken": "E0gb6q1+WyFW3FMbbQJmrg=="
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
]
```

例：

```javascript
alloy("sendEvent", {
  xdm: { ...},
  decisionScopes: ["hero-banner"]
}).then(function (result) {
  var propositions = result.propositions;

  if (propositions) {
    // Find the discount proposition, if it exists.
    for (var i = 0; i < propositions.length; i++) {
      var proposition = propositions[i];
      for (var j = 0; j < proposition.items; j++) {
        var item = proposition.items[j];
        if (item.schema === HTML_SCHEMA) {
          // apply offer
          document.getElementById("form-based-offer-container").innerHTML =
            item.data.content;
          const executedPropositions = [
            {
              id: proposition.id,
              scope: proposition.scope,
              scopeDetails: proposition.scopeDetails
            }
          ];

          alloy("sendEvent", {
            "xdm": {
              "eventType": "decisioning.propositionDisplay",
              "_experience": {
                "decisioning": {
                  "propositions": executedPropositions
                }
              }
            }
          });
        }
      }
    }
  }
});
```

[詳細情報](../rendering-personalization-content.md#manually-rendering-content)

## Target アクティビティの適用方法

### at.js の使用

Target アクティビティを適用するには、`applyOffers` の関数を使用します。`adobe.target.applyOffer(options)`

例：

```javascript
adobe.target.getOffers({...})
  .then(response => adobe.target.applyOffers({ response: response }))
  .then(() => console.log("Success"))
  .catch(error => console.log("Error", error));
```

`applyOffers` コマンドについて詳しくは、[ 専用ドキュメント ](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/adobe-target-applyoffers-atjs-2.html) を参照してください。


### Web SDKの使用

`applyPropositions` コマンドを使用して、Target アクティビティを適用できます。

例：

```javascript
alloy("applyPropositions", {
    propositions: [...]
});
```

`applyPropositions` コマンドについて詳しくは、[ 専用ドキュメント ](../../personalization/rendering-personalization-content.md#applypropositions) を参照してください。

## イベントの追跡方法

### at.js の使用

`trackEvent` 関数を使用するか、`sendNotifications` を使用すると、イベントを追跡できます。

この関数は、クリック数やコンバージョン数などのユーザーアクションをレポートするリクエストを実行します。 応答内のアクティビティは配信されません。


**例 1**

```javascript
adobe.target.trackEvent({ 
    "type": "click",
    "mbox": "some-mbox"
});
```

**例 2**

```javascript
adobe.target.sendNotifications({ 
    request: {
       notifications: [{
          ...,
          mbox: {
            name: "some-mbox"
          },
          type: "click",
          ...
       }]
    }
});
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/adobe-target-trackevent.html)

### Web SDKの使用

`sendEvent` コマンドを呼び出し、`_experience.decisioning.propositions` XDM フィールドグループに入力し、`eventType` を 2 つの値のいずれかに設定することで、イベントおよびユーザーアクションを追跡できます。

* `decisioning.propositionDisplay`: Target アクティビティのレンダリングをシグナルで通知します。
* `decisioning.propositionInteract`：マウスクリックなど、アクティビティに対するユーザーのインタラクションを示します。

`_experience.decisioning.propositions` XDM フィールドグループはオブジェクトの配列です。 各オブジェクトのプロパティは、`result.propositions` のコマンドで返される `sendEvent` から派生します。`{ id, scope, scopeDetails }`

**例 1 - アクティビティのレンダリング後に `decisioning.propositionDisplay` イベントを追跡**

```javascript
alloy("sendEvent", {
  xdm: {},
  decisionScopes: ['discount']
}).then(function(result) {
  var propositions = result.propositions;

  var discountProposition;
  if (propositions) {
    // Find the discount proposition, if it exists.
    for (var i = 0; i < propositions.length; i++) {
      var proposition = propositions[i];
      if (proposition.scope === "discount") {
        discountProposition = proposition;
        break;
      }
    }
  }

  if (discountProposition) {
    // Find the item from proposition that should be rendered.
    // Rather than assuming there a single item that has HTML
    // content, find the first item whose schema indicates
    // it contains HTML content.
    for (var j = 0; j < discountProposition.items.length; j++) {
      var discountPropositionItem = discountProposition.items[i];
      if (discountPropositionItem.schema === "https://ns.adobe.com/personalization/html-content-item") {
        var discountHtml = discountPropositionItem.data.content;
        // Render the content
        var dailySpecialElement = document.getElementById("daily-special");
        dailySpecialElement.innerHTML = discountHtml;
        
        // For this example, we assume there is only a single place to update in the HTML.
        break;  
      }
    }
    // Send a "decisioning.propositionDisplay" event signaling that the proposition has been rendered.
    alloy("sendEvent", {
      "xdm": {
        "eventType": "decisioning.propositionDisplay",
        "_experience": {
          "decisioning": {
            "propositions": [{
              "id": id,
              "scope": scope,
              "scopeDetails": scopeDetails
            }],
            "propositionEventType": {
              "display": 1
            }
          }
        }
      }
    });
  }
});
```

**例 2 - クリック指標が発生した後の `decisioning.propositionInteract` イベントの追跡**

```javascript
alloy("sendEvent", {
  xdm: { ...},
  decisionScopes: ["hero-banner"]
}).then(function (result) {
  var propositions = result.propositions;

  if (propositions) {
    // Find the discount proposition, if it exists.
    for (var i = 0; i < propositions.length; i++) {
      var proposition = propositions[i];
      for (var j = 0; j < proposition.items.length; j++) {
        var item = proposition.items[j];

        if (item.schema === "https://ns.adobe.com/personalization/measurement") {
          // add metric to the DOM element
          const button = document.getElementById("form-based-click-metric");

          button.addEventListener("click", event => {
            const executedPropositions = [
              {
                id: proposition.id,
                scope: proposition.scope,
                scopeDetails: proposition.scopeDetails
              }
            ];
            // send the click track event
            alloy("sendEvent", {
              "xdm": {
                "eventType": "decisioning.propositionInteract",
                "_experience": {
                  "decisioning": {
                    "propositions": executedPropositions
                  }
                }
              }
            });
          });
        }
      }
    }
  }
});
```

[詳細情報](../rendering-personalization-content.md#manually-rendering-content)

**例 3 - アクションを実行した後に発生したイベントの追跡**

この例では、ボタンのクリックなど、特定のアクションの実行後に発生したイベントを追跡します。
`__adobe.target` データオブジェクトを介して、追加のカスタムパラメーターを追加できます。

`commerce` XDM オブジェクトを追加することもできます。

```js
alloy("sendEvent", {
    "xdm": {
        "_experience": {
            "decisioning": {
                "propositions": [
                    {
                        "scope": "orderConfirm" //example scope name
                    }
                ],
                "propositionEventType": {
                    "display": 1
                }
            }
        },
        "eventType": "decisioning.propositionDisplay"
    },
    "commerce": {
        "order": {
            "purchaseID": "a8g784hjq1mnp3",
            "purchaseOrderNumber": "VAU3123",
            "currencyCode": "USD",
            "priceTotal": 999.98
        }
    },
    "data": {
        "__adobe": {
            "target": {
                "pageType": "Order Confirmation",
                "user.categoryId": "Insurance"
            }
        }
    }
})
```

## シングルページアプリケーションでビューの変更をトリガーする方法

### at.js の使用

`adobe.target.triggerView` 関数を使用します。 この関数は、新しいページが読み込まれるときや、ページ上のコンポーネントが再レンダリングされるときに呼び出すことができます。 adobe.target.triggerView （）は、Visual Experience Composer （VEC）を使用して A/B テストおよびエクスペリエンスのターゲット設定（XT）アクティビティを作成するために、単一ページアプリケーション（SPA）に実装する必要があります。 adobe.target.triggerView （）がサイトに実装されていない場合、VEC を SPA に利用することはできません。

**例**

```javascript
adobe.target.triggerView("homeView")
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/adobe-target-triggerview-atjs-2.html)


### Web SDKの使用

シングルページアプリケーションのビューの変更をトリガーまたは通知するには、`web.webPageDetails.viewName` コマンドの `xdm` オプションの下で `sendEvent` プロパティを設定します。 Web SDKは、で指定された `viewName` のオファーがある場合、ビューのキャッシュをチェック `sendEvent`、そのオファーを実行してディスプレイ通知イベントを送信します。

**例**

```javascript
alloy("sendEvent", {
  renderDecisions: true,
  xdm:{
    web:{
      webPageDetails:{
        viewName: "homeView"
      }
    }
  }
});
```

[詳細情報](./spa-implementation.md#implementing-xdm-views)

## レスポンストークンの活用方法

Adobe Targetから返されるPersonalization コンテンツには、アクティビティ、オファー、エクスペリエンス、ユーザープロファイル、地域情報などに関する詳細である [ レスポンストークン ](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html) が含まれます。 これらの詳細は、サードパーティのツールと共有したり、デバッグに使用したりできます。 レスポンストークンは、Adobe Target ユーザーインターフェイスで設定できます。

### at.js の使用

at.js カスタムイベントを使用して Target 応答をリッスンし、応答トークンを読み取ります。

**例**

```javascript
document.addEventListener(adobe.target.event.REQUEST_SUCCEEDED, function(e) { 
  console.log("Request succeeded", e.detail); 
}); 
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html)


### Web SDKの使用

>[!IMPORTANT]
>
>Experience Platform Web SDK バージョン 2.6.0 以降を使用していることを確認します。

応答トークンは、`propositions` コマンドの結果で公開される `sendEvent` の一部として返されます。 各提案には `items` の配列が含まれ、Target 管理 UI で有効になっている場合、各項目には応答トークンが入力された `meta` オブジェクトがあります。 [詳細情報](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html)

**例**

```javascript
alloy("sendEvent", {
    renderDecisions: true,
    xdm: {}
  }).then(function(result) {
    if (result.propositions) {
      // Format of result.propositions:
      /*
        [
            {
                "id": "",
                "scope": "",
                "items": [
                    {
                        "id": "",
                        "schema": "",
                        "data": {},
                        "meta": { // RESPONSE TOKENS
                            "activity.name": ...,
                            "offer.id": ...,
                            "profile.activeActivities": ...
                        }
                    }
                ],
                "scopeDetails": {}
                "renderAttempted": false
            }
        ]
      */
    }
  });
```

[詳細情報](./accessing-response-tokens.md)

## フリッカーの管理方法

### at.js の使用

at.js を使用すると、at.js が処理するように `bodyHidingEnabled: true` を設定することで、ちらつきを管理できます
dom の変更を取得して適用する前に、パーソナライズされたコンテナを事前に非表示にします。
パーソナライズされたコンテンツを含むページセクションは、at.js `bodyHiddenStyle` を上書きすることで、事前に非表示にすることができます。
デフォルトでは、`bodyHiddenStyle` はHTML `body` ージ全体を非表示にします。
どちらの設定も、`window.targetGlobalSettings` を使用して上書きできます。 at.js を読み込む前に `window.targetGlobalSettings` を配置してください。

### Web SDKの使用

顧客は、Web SDKを使用して、次の例に示すように、configure コマンドで事前非表示スタイルを設定できます。

```javascript
alloy("configure", {
  datastreamId: "configurationId",
  orgId: "orgId@AdobeOrg",
  debugEnabled: true,
  prehidingStyle: "body { opacity: 0 !important }"
});
```

Web SDK非同期を読み込む場合、次のスニペットは、web SDKが挿入される前にページに挿入することをお勧めします。

```html
<script>
  !function(e,a,n,t){
  if (a) return;
  var i=e.head;if(i){
  var o=e.createElement("style");
  o.id="alloy-prehiding",o.innerText=n,i.appendChild(o),
  setTimeout(function(){o.parentNode&&o.parentNode.removeChild(o)},t)}}
  (document, document.location.href.indexOf("adobe_authoring_enabled") !== -1, "body { opacity: 0 !important }", 3000);
</script>
```

## A4T の処理方法

### at.js の使用

at.js を使用してサポートされている A4T ログには、次の 2 種類があります。

* Analytics クライアントサイドログ
* Analytics サーバーサイドログ

#### Analytics クライアントサイドログ

**例 1:Target のグローバル設定の使用**

Analytics のクライアントサイドログは、at.js 設定で `analyticsLogging: client_side` を設定するか、`window.targetglobalSettings` オブジェクトを上書きすることで有効にできます。
このオプションを設定すると、返されるペイロードの形式は次のようになります。

```json
{
  "analytics": {
    "payload": {
      "pe": "tnt",
      "tnta": "167169:0:0|0|100,167169:0:0|2|100,167169:0:0|1|100"
    }
  }
}
```

その後、ペイロードは Data Insertion API を介して Analytics に転送できます。

例 2：すべての `getOffers` 関数で設定する

```javascript
adobe.target.getOffers({
      request: {
        experienceCloud: {
          analytics: {
            logging: "client_side"
          }
        },
        prefetch: {
          mboxes: [{
            index: 0,
            name: "a1-serverside-xt"
          }]
        }
      }
    })
    .then(console.log)
```

応答ペイロードは次のようになります。

```json
{
  "prefetch": {
    "mboxes": [{
      "index": 0,
      "name": "a1-serverside-xt",
      "options": [{
        "content": "<img src=\"http://s7d2.scene7.com/is/image/TargetAdobeTargetMobile/L4242-xt-usa?tm=1490025518668&fit=constrain&hei=491&wid=980&fmt=png-alpha\"/>",
        "type": "html",
        "eventToken": "n/K05qdH0MxsiyH4gX05/2qipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
        "responseTokens": {
          "profile.memberlevel": "0",
          "geo.city": "bucharest",
          "activity.id": "167169",
          "experience.name": "USA Experience",
          "geo.country": "romania"
        }
      }],
      "analytics": {
        "payload": {
          "pe": "tnt",
          "tnta": "167169:0:0|0|100,167169:0:0|2|100,167169:0:0|1|100"
        }
      }
    }]
  }
}
```

`tnta`Data Insertion API[ を使用して、Analytics ペイロード（](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md) トークン）を Analytics ヒットに含める必要があります。

#### Analytics サーバーサイドログ

Analytics サーバーサイドログは、at.js 設定で `analyticsLogging: server_side` を設定するか、`window.targetglobalSettings` オブジェクトを上書きすることで有効にできます。
その後、データは次のようにフローします。

![Analytics サーバーサイドログのワークフローを示す図 ](assets/a4t-server-side-atjs.png)

[詳細情報](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4timplementation.html)

### Web SDKの使用

Web SDKは、次の項目もサポートしています。

* Analytics クライアントサイドログ
* Analytics サーバーサイドログ

#### Analytics クライアントサイドログ

Analytics クライアントサイドログは、その DataStream 設定に対してAdobe Analyticsが無効になっている場合に有効になります。

![Analytics クライアントサイドログのワークフローを示す図 ](assets/analytics-disabled-datastream-config.png)

顧客は、`tnta`Data Insertion API[ を使用して Analytics と共有する必要がある Analytics トークン（](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md)）にアクセスできます
`sendEvent` コマンドを連結してで、結果として得られる提案配列を繰り返し処理します。

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
    var proposition = results.propositions[i];
    var renderAttempted = proposition.renderAttempted;

    if (renderAttempted === true) {
      var analyticsPayload = getAnalyticsPayload(proposition);
      if (analyticsPayload !== undefined) {
        analyticsPayloads.add(analyticsPayload);
      }
    }
  }
  var analyticsPayloadsToken = concatenateAnalyticsPayloads(analyticsPayloads);
  // send the page view Analytics hit with collected Analytics payload using Data Insertion API
});
```

次の図は、Analytics クライアントサイドが有効な場合のデータフローを示しています。

![Analytics クライアントサイドログのデータフロー図 ](assets/analytics-client-side-logging.png)

#### Analytics サーバーサイドログ

その DataStream 設定に対して Analytics が有効になっている場合、Analytics サーバーサイドログは有効になります。

![Analytics 設定を示すデータストリーム UI。](assets/analytics-enabled-datastream-config.png)

サーバーサイド分析ログが有効になっている場合、Analytics レポートが表示されるように、Analytics と共有する必要がある A4T ペイロード
正しいインプレッション数とコンバージョン数はEdge Network レベルで共有されるので、お客様は追加の処理を行う必要はありません。

サーバーサイド分析ログが有効な場合のシステムへのデータのフローは次のとおりです。

![ サーバーサイド分析ログのデータフローを示す図 ](assets/analytics-server-side-logging.png)

## Target のグローバル設定方法

### at.js の使用

Target Standard/Premium UI や REST API を使用して設定を行う代わりに、`window.targetGlobalSettings` を使用して at.js ライブラリで設定を上書きできます。

上書きは、at.js が読み込まれる前か、管理/実装/at.js 設定を編集/コード設定/ライブラリヘッダーで定義する必要があります。

例：

```javascript
window.targetGlobalSettings = {  
   timeout: 200, // using custom timeout  
   visitorApiTimeout: 500, // using custom API timeout  
   enabled: document.location.href.indexOf('https://www.adobe.com') >= 0 // enabled ONLY on adobe.com  
};
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/targetgobalsettings.html)

### Web SDKの使用

この機能は Web SDKではサポートされていません。

## Target プロファイル属性の更新方法

### at.js の使用

**例 1**

```javascript
adobe.target.getOffer({
   mbox: "target-global-mbox",
   params: {
     "profile.name": "test",
     "profile.gender": "female"
   },
   success: console.log,
   error: console.error
});
```

**例 2**

```javascript
adobe.target.getOffers({
    request: {
      execute: {
        pageLoad: {
          profileParameters: {
            name: "test",
            gender: "female"
          }
        }
    }
  }
})
.then(console.log)
.catch(console.error);
```

### Web SDKの使用

Target プロファイルを更新するには、`sendEvent` コマンドを使用して、`data.__adobe.target` を使用してキー名のプレフィックスとして `profile` プロパティを設定します。

**例**

```javascript
alloy("sendEvent", {
  renderDecisions: true,
  data: {
    __adobe: {
      target: {
        "profile.gender": "female",
        "profile.age": 30
      }
    }
  }
});
```

## Target Recommendations の使用方法

### at.js の使用

**例 1**

```javascript
adobe.target.getOffer({
   mbox: "target-global-mbox",
   params: {
     "entity.name": "T-shirt",
     "entity.id": "1234"
   },
   success: console.log,
   error: console.error
});
```

**例 2**

```javascript
adobe.target.getOffers({
    request: {
      execute: {
        pageLoad: {
          parameters: {
            "entity.name": "T-shirt",
            "entity.id": "1234"
          }
        }
    }
  }
})
.then(console.log)
.catch(console.error);
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/adobe-target-getoffers-atjs-2.html)


### Web SDKの使用

推奨データを送信するには、`sendEvent` コマンドを使用して、`data.__adobe.target` を使用してキー名のプレフィックスとして `entity` プロパティを設定します。

**例**

```javascript
alloy("sendEvent", {
  renderDecisions: true,
  data: {
    __adobe: {
      target: {
        "entity.name": "T-shirt",
        "entity.id": "1234"
      }
    }
  }
});
```

## サードパーティ ID の使用方法

### at.js の使用

at.js の使用 `mbox3rdPartyId` または `getOffer` を使用して `getOffers` を送信する方法は複数あります。

**例 1**

```javascript
adobe.target.getOffer({
  mbox:"test",
  params:{
    "mbox3rdPartyId": "1234"
  },
  success: console.log,
  error: console.error
});
```

**例 2**

```javascript
adobe.target.getOffers({
    request: {
      id:{
        thirdPartyId: "1234"
      },
      execute: {
        pageLoad: {}
    }
  }
})
.then(console.log)
.catch(console.error);
```

または、`mbox3rdPartyId` または `targetPageParams` のいずれかで `targetPageParamsAll` を設定する方法があります。
`targetPageParams` で設定すると、`target-global-mbox` とも呼ばれる `pag-lLoad` のリクエストで送信されます。
推奨は、すべてのターゲットリクエストで送信されるので、`targetPageParamsAll` を使用して設定する必要があります。
`targetPageParamsAll` を使用する利点は、ページ上で `mbox3rdPartyId` を 1 回定義できることで、すべてのターゲットリクエストが適切な `mbox3rdPartyId` を持つようになります。

```javascript
window.targetPageParamsAll = function() {
      return {
        "mbox3rdPartyId": "1234"
      };
    };
```

```javascript
window.targetPageParams = function() {
  return {
    "mbox3rdPartyId": "1234"
  };
};
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/targetpageparams.html)

### Web SDKの使用

Web SDKは、Target サードパーティ ID をサポートしています。 ただし、もう 2、3 の手順が必要です。 ソリューションの詳細に入る前に、`identityMap` について少し説明します。
顧客は ID マップを使用して複数の ID を送信できます。 すべての ID に名前空間が設定されています。 各名前空間には、1 つ以上の ID を設定できます。 特定の ID をプライマリとしてマークできます。
この知識を念頭に置くと、Target サードパーティ ID を使用するために web sdk をセットアップする際に必要な手順を確認できます。

1. データストリーム設定ページで Target サードパーティ ID を含む名前空間を設定します。

![ 「ターゲットサードパーティ ID 名前空間」フィールドを示すデータストリーム UI](assets/mbox-3-party-id-setup.png)

1. 次のように、sendEvent コマンドごとに、その ID 名前空間を送信します。

```javascript
alloy("sendEvent", {
  "renderDecisions": true,
  "xdm": {
    "identityMap": {
      "TGT3PID": [
        {
          "id": "1234",
          "primary": true
        }
      ]
    }
  }
});
```

## プロパティトークンの設定方法

### at.js の使用

at.js の使用プロパティトークンの設定方法は、`targetPageParams` と `targetPageParamsAll` の 2 とおりあります。 `targetPageParams` を使用すると、プロパティトークンが `target-global-mbox` 呼び出しに追加されますが、`targetPageParamsAll` を使用すると、トークンがすべての target 呼び出しに追加されます。

**例 1**

```javascript
   window.targetPageParamsAll = function() {
      return {
        "at_property": "1234"
      };
    };
```

**例 2**

```javascript
window.targetPageParams = function() {
      return {
        "at_property": "1234"
      };
    };
```

### Web SDKの使用

お客様は、Web SDKを使用して、Adobe Target namespace でデータストリーム設定を指定する際に、より高いレベルでプロパティを設定できます。
![Adobe Target設定を示すデータストリーム UI。](assets/at-property-setup.png)
つまり、その特定のデータストリーム設定に対するすべての Target 呼び出しに、そのプロパティトークンが含まれます。

## mbox をプリフェッチする方法

### at.js の使用

この機能は at.js 2.x でのみ使用できます。at.js 2.x には、`getOffers` という名前の新しい関数があります。 `getOffers` を使用すると、顧客は 1 つ以上の mbox のコンテンツをプリフェッチできます。 次に例を示します。

```javascript
adobe.target.getOffers({
    request: {
      prefetch: {
        mboxes: [{
          index: 0,
          name: "test-mbox",
          parameters: {
            ...
          },
          profileParameters: {
            ...
          }
        }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

メモ：`mbox` 配列のすべての `mboxes` に独自のインデックスを設定することを強くお勧めします。 通常、最初の mbox には `index=0` があり、次の mbox には `index=1` があります。

### Web SDKの使用

この機能は、現在 Web SDKではサポートされていません。

## Target 実装のデバッグ方法

### at.js の使用

At.js は、次のデバッグ機能を公開します。

* mbox 無効化 – Target が取得とレンダリングを行うのを無効にし、Target のインタラクションなしにページが壊れているかどうかを確認します
* mbox のデバッグ - at.js はすべてのアクションをログに記録します
* Target Trace - Bullseye で生成された mbox トレーストークンを使用すると、決定プロセスに関与した詳細を含むトレースオブジェクトをオブジェクトの下で使用でき `window.___target_trace` す。

注意：これらのデバッグ機能はすべて、[Adobe Experience Platform Debuggerの機能強化で利用でき ](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) す。

### Web SDKの使用

Web SDKを使用する場合、複数のデバッグ機能があります。

* [Assurance](/help/assurance/home.md) の使用
* [Web SDKのデバッグが有効](/help/web-sdk/use-cases/debugging.md)
* [Web SDK モニタリングフック ](https://github.com/adobe/alloy/wiki/Monitoring-Hooks) の使用
* [Adobe Experience Platform Debugger](/help/debugger/home.md) の使用
* ターゲット トレース
