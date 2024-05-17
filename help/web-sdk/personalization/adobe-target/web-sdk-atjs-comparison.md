---
title: at.js とExperience Platform Web SDK の比較
description: at.js 機能とExperience Platform Web SDK の比較を説明します
keywords: target;adobe target;activity.id;experience.id;renderDecisions;decisionScopes；事前非表示スニペット；vec；フォームベースの Experience Composer;xdm；オーディエンス；決定；範囲；スキーマ；システム図；図
exl-id: b63fe47d-856a-4cae-9057-51917b3e58dd
source-git-commit: ca1574f3f95840fce246fb4ed8845583fa0ff093
workflow-type: tm+mt
source-wordcount: '2175'
ht-degree: 6%

---

# at.js ライブラリと Web SDK の比較

## 概要

この記事では、の相違点の概要を説明します `at.js` ライブラリと Experience Platform Web SDK。

## ライブラリのインストール

### at.js のインストール

お客様は、Adobe Experience Cloudの「実装」タブから直接ライブラリをダウンロードできます。 at.js ライブラリは、顧客が持つ設定（clientCode、imsOrgId など）によってカスタマイズされます。

### Web SDK のインストール

事前ビルドバージョンは CDN で使用できます。 CDN のライブラリをページで直接参照するか、ダウンロードして独自のインフラストラクチャにホストすることができます。 縮小形式と非縮小形式で利用できます。 デバッグの目的では、非縮小バージョンが役立ちます。

参照： [JavaScript ライブラリを使用した Web SDK のインストール](/help/web-sdk/install/library.md) を参照してください。

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


### Web SDK の設定

SDK の設定は、を使用して行います。 [`configure`](/help/web-sdk/commands/configure/overview.md) コマンド。 この `configure` コマンドは *常に* 最初に呼び出されます。

## ページ読み込み Target オファーをリクエストし自動的にレンダリングする方法

### at.js の使用

設定を有効にした場合は、at.js 2.x を使用 `pageLoadEnabled`を選択すると、ライブラリは Target Edge への呼び出しを次のようにトリガーします `execute -> pageLoad`. すべての設定がデフォルト値に設定されている場合、カスタムコーディングは必要ありません。at.js がページに追加され、ブラウザーによって読み込まれると、Target Edge 呼び出しが実行されます。

### Web SDK の使用

Adobe Targetの内で作成されたコンテンツ [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) SDK による取得と自動レンダリングが可能です。

Target オファーをリクエストして自動的にレンダリングするには、 `sendEvent` コマンドおよび設定 `renderDecisions` 対するオプション `true`. これにより、自動レンダリングの対象となる、パーソナライズされたコンテンツが SDK によって自動的にレンダリングされます。

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

Experience PlatformWeb SDK は、WEB SDK が実行したオファーを含む通知を自動的に送信します。通知リクエストペイロードの例を次に示します。

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

ページ読み込み用にオファーを取得する Target Edge を呼び出す方法は 2 つあります。

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

### Web SDK の使用

実行 `sendEvent` ～の下に特殊範囲を持つ命令 `decisionScopes`: `__view__`. このスコープをシグナルとして使用し、Target からすべてのページ読み込みアクティビティを取得し、すべてのビューをプリフェッチします。 また、Web SDK は、すべての VEC 表示ベースのアクティビティの評価も試みます。 ビューのプリフェッチの無効化は、現在 Web SDK ではサポートされていません。

パーソナライゼーションコンテンツにアクセスするには、コールバック関数を指定します。この関数は、SDK がサーバーから正常に応答を受信した後に呼び出されます。 コールバックには、返されたパーソナライゼーションコンテンツを含む propositions プロパティを含めることができる結果オブジェクトが提供されます。

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

フォームベースの Composer アクティビティを取得するには、 `getOffer` 関数：

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


### Web SDK の使用

フォームベースの Composer ベースのアクティビティを取得するには、を使用します `sendEvent` コマンドを実行し、の下に mbox 名を渡す `decisionScopes` オプション。 この `sendEvent` コマンドは、要求されたアクティビティ/提案を含むオブジェクトで解決された promise を返します。このようにして、 `propositions` 配列は次のようになります。

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

Target アクティビティを適用するには、次を使用します `applyOffers` 関数： `adobe.target.applyOffer(options)`

例：

```javascript
adobe.target.getOffers({...})
  .then(response => adobe.target.applyOffers({ response: response }))
  .then(() => console.log("Success"))
  .catch(error => console.log("Error", error));
```

の詳細情報 `applyOffers` コマンドを [専用ドキュメント](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/adobe-target-applyoffers-atjs-2.html).


### Web SDK の使用

Target アクティビティを適用するには、次を使用します `applyPropositions` コマンド。

例：

```javascript
alloy("applyPropositions", {
    propositions: [...]
});
```

の詳細情報 `applyPropositions` コマンドを [専用ドキュメント](../../personalization/rendering-personalization-content.md#applypropositions).

## イベントの追跡方法

### at.js の使用

イベントを追跡するには、 `trackEvent` 関数または使用 `sendNotifications`.

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

### Web SDK の使用

イベントとユーザーのアクションを追跡するには、を呼び出します `sendEvent` コマンド、入力#コマンド ソウニュウ# `_experience.decisioning.propositions` XDM フィールドグループと `eventType` 2 つの値のいずれか：

* `decisioning.propositionDisplay`:Target アクティビティのレンダリングをシグナルします。
* `decisioning.propositionInteract`：マウスクリックなど、アクティビティに対するユーザーのインタラクションを示します。

この `_experience.decisioning.propositions` XDM フィールドグループは、オブジェクトの配列です。 各オブジェクトのプロパティは、 `result.propositions` が返されます。 `sendEvent` コマンド： `{ id, scope, scopeDetails }`

**例 1 - トラック a `decisioning.propositionDisplay` アクティビティのレンダリング後のイベント**

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
      xdm: {
        eventType: "decisioning.propositionDisplay",
        _experience: {
          decisioning: {
            propositions: [
              {
                id: discountProposition.id,
                scope: discountProposition.scope,
                scopeDetails: discountProposition.scopeDetails
              }
            ]
          }
        }
      }
    });
  }
});
```

**例 2 - トラック a `decisioning.propositionInteract` クリック指標の発生後のイベント**

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
以下を使用して、追加のカスタムパラメーターを追加できます `__adobe.target` データオブジェクト。

```js
//replicates an at.js trackEvent call
alloy("sendEvent", {
    "type": "decisioning.propositionDisplay",
    "xdm": {
        "_experience": {
            "decisioning": {
                "propositions": [{
                    "scope": "sumbitButtonClick" // Or any mbox/location name you want to use in Adobe Target
                }]
            }
        }
    }
});
```

## シングルページアプリケーションでビューの変更をトリガーする方法

### at.js の使用

の使用 `adobe.target.triggerView` 関数。 この関数は、新しいページが読み込まれるときや、ページ上のコンポーネントが再レンダリングされるときに呼び出すことができます。adobe.target.triggerView （）は、Visual Experience Composer （VEC）を使用して A/B テストとエクスペリエンスのターゲット設定（XT）アクティビティを作成するために、シングルページアプリケーション（SPA）用に実装する必要があります。 adobe.target.triggerView （）がサイトに実装されていない場合、VEC はSPAに使用できません。

**例**

```javascript
adobe.target.triggerView("homeView")
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/adobe-target-triggerview-atjs-2.html)


### Web SDK の使用

シングルページアプリケーションのビューの変更をトリガーまたは通知するには、 `web.webPageDetails.viewName` 以下のプロパティ： `xdm` オプション `sendEvent` コマンド。 のオファーがある場合、Web SDK はビューのキャッシュをチェックします。 `viewName` で指定 `sendEvent` これらのアクションを実行し、表示通知イベントを送信します。

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

Adobe Targetから返されるパーソナライゼーションコンテンツには、次が含まれます [レスポンストークン](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html)アクティビティ、オファー、エクスペリエンス、ユーザープロファイル、地域情報などに関する詳細です。 これらの詳細は、サードパーティのツールと共有したり、デバッグに使用したりできます。 レスポンストークンは、Adobe Target ユーザーインターフェイスで設定できます。

### at.js の使用

at.js カスタムイベントを使用して Target 応答をリッスンし、応答トークンを読み取ります。

**例**

```javascript
document.addEventListener(adobe.target.event.REQUEST_SUCCEEDED, function(e) { 
  console.log("Request succeeded", e.detail); 
}); 
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html)


### Web SDK の使用

>[!IMPORTANT]
>
>Platform Web SDK バージョン 2.6.0 以降を使用していることを確認します。

応答トークンは、 `propositions` の結果で公開される `sendEvent` コマンド。 各提案には、の配列が含まれます `items`各アイテムにはが含まれます。 `meta` target 管理 UI で有効になっている場合は、応答トークンが入力されたオブジェクト。 [詳細情報](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html)

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

at.js を使用すると、次のように設定してちらつきを管理できます。 `bodyHidingEnabled: true` そのため、DOM の変更を取得して適用する前に、パーソナライズされたコンテナを事前に非表示にする処理は at.js が行います。
パーソナライズされたコンテンツを含むページセクションは、at.js を上書きすることで事前非表示にすることができます `bodyHiddenStyle`.
デフォルト `bodyHiddenStyle` HTML全体を非表示 `body`.
どちらの設定も、を使用して上書きできます。 `window.targetGlobalSettings`. `window.targetGlobalSettings` は、at.js を読み込む前に配置してください。

### Web SDK の使用

顧客は、Web SDK を使用して、次の例に示すように、configure コマンドで事前非表示スタイルを設定できます。

```javascript
alloy("configure", {
  edgeConfigId: "configurationId",
  orgId: "orgId@AdobeOrg",
  debugEnabled: true,
  prehidingStyle: "body { opacity: 0 !important }"
});
```

Web SDK の非同期を読み込む場合、Web SDK が挿入される前に、次のスニペットをページに挿入することをお勧めします。

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

Analytics クライアントサイドログは、次の設定を行うことで有効にできます `analyticsLogging: client_side` at.js 設定または上書きによる `window.targetglobalSettings` オブジェクト。
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

例 2：毎に設定 `getOffers` 関数：

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

Analytics ペイロード （`tnta` トークン）を使用して、Analytics ヒットに含める必要があります [データ挿入 API](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md).

#### Analytics サーバーサイドログ

Analytics サーバーサイドログは、次の設定を行うことで有効にできます `analyticsLogging: server_side` at.js 設定または上書きによる `window.targetglobalSettings` オブジェクト。
その後、データは次のようにフローします。

![Analytics サーバーサイドログのワークフローを示す図](assets/a4t-server-side-atjs.png)

[詳細情報](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4timplementation.html)

### Web SDK の使用

Web SDK は、次の項目もサポートしています。

* Analytics クライアントサイドログ
* Analytics サーバーサイドログ

#### Analytics クライアントサイドログ

Analytics クライアントサイドログは、その DataStream 設定に対してAdobe Analyticsが無効になっている場合に有効になります。

![Analytics クライアントサイドログのワークフローを示す図](assets/analytics-disabled-datastream-config.png)

顧客が Analytics トークン（`tnta`を使用して Analytics と共有する必要がある） [データ挿入 API](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md)
を鎖でつないだ `sendEvent` コマンドを実行し、結果の提案配列を繰り返し処理します。

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

![Analytics クライアントサイドログのデータフロー図](assets/analytics-client-side-logging.png)

#### Analytics サーバーサイドログ

その DataStream 設定に対して Analytics が有効になっている場合、Analytics サーバーサイドログは有効になります。

![Analytics 設定を表示するデータストリーム UI。](assets/analytics-enabled-datastream-config.png)

サーバーサイド分析ログが有効な場合、Analytics レポートで正しいインプレッション数が示され、コンバージョンがEdge Networkレベルで共有されるように、Analytics と共有する必要がある A4T ペイロードを設定します。これにより、お客様は他の処理を行う必要がなくなります。

サーバーサイド分析ログが有効な場合のシステムへのデータのフローは次のとおりです。

![サーバーサイド分析ログのデータフローを示す図](assets/analytics-server-side-logging.png)

## Target のグローバル設定方法

### at.js の使用

Target Standard／Premium UI や REST API を使用して設定を構成する代わりに、`window.targetGlobalSettings` を使用して at.js ライブラリで設定を上書きすることができます。

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

### Web SDK の使用

この機能は Web SDK ではサポートされていません。

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

### Web SDK の使用

Target プロファイルを更新するには、を使用します `sendEvent` コマンドおよび設定 `data.__adobe.target` プロパティ、使用するキー名のプレフィックス `profile`.

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

## Target Recommendationsの使用方法

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


### Web SDK の使用

レコメンデーションデータを送信するには、 `sendEvent` コマンドおよび設定 `data.__adobe.target` プロパティ、使用するキー名のプレフィックス `entity`.

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

at.js を使用する場合、複数の送信方法があります `mbox3rdPartyId`、使用 `getOffer` または `getOffers`:

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

または、を設定する方法があります `mbox3rdPartyId` 次のいずれか： `targetPageParams` または `targetPageParamsAll`.
で設定する場合 `targetPageParams`。次のリクエストで送信されます `target-global-mbox` 別名 `pag-lLoad`.
推奨は、を使用して設定する必要があります。 `targetPageParamsAll` すべての target リクエストで送信されるからです。
使用する利点 `targetPageParamsAll` は、 `mbox3rdPartyId` を 1 回ページに送信すると、すべての target リクエストが適切に処理されます。 `mbox3rdPartyId`.

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

### Web SDK の使用

Web SDK は、Target サードパーティ ID をサポートしています。 ただし、もう 2、3 の手順が必要です。 ソリューションの詳細に入る前に、以下について少し説明します `identityMap`.
顧客は ID マップを使用して複数の ID を送信できます。 すべての ID に名前空間が設定されています。 各名前空間には、1 つ以上の ID を設定できます。 特定の ID をプライマリとしてマークできます。
この知識を念頭に置くと、Target サードパーティ ID を使用するために web sdk をセットアップする際に必要な手順を確認できます。

1. データストリーム設定ページで Target サードパーティ ID を含む名前空間を設定します。

![「ターゲットサードパーティ ID 名前空間」フィールドを示すデータストリーム UI](assets/mbox-3-party-id-setup.png)

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

at.js の使用プロパティトークンを設定するには、次のいずれかを使用します `targetPageParams` または `targetPageParamsAll`. 使用 `targetPageParams` プロパティトークンをに追加します。 `target-global-mbox` を呼び出しますが、を使用します `targetPageParamsAll` すべての target 呼び出しにトークンを追加します。

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

### Web SDK の使用

Web SDK を使用すると、お客様は、Adobe Target名前空間の下でデータストリーム設定を設定する際に、より高いレベルでプロパティを設定できます。
![Adobe Target設定を示すデータストリーム UI。](assets/at-property-setup.png)
つまり、その特定のデータストリーム設定に対するすべての Target 呼び出しに、そのプロパティトークンが含まれます。

## mbox をプリフェッチする方法

### at.js の使用

この機能は at.js 2.x でのみ使用できます。at.js 2.x には、という名前の新しい関数があります。 `getOffers`. `getOffers` 顧客が 1 つ以上の mbox のコンテンツをプリフェッチできるようにします。 次に例を示します。

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

メモ：以下の条件を満たすことを確認することを強くお勧めします `mbox` が含まれる `mboxes` 配列には独自のインデックスがあります。 通常、最初の mbox は以下を持ちます。 `index=0`、次の 1 `index=1`等。

### Web SDK の使用

この機能は、現在 Web SDK ではサポートされていません。

## Target 実装のデバッグ方法

### at.js の使用

At.js は、次のデバッグ機能を公開します。

* mbox 無効化 – Target が取得とレンダリングを行うのを無効にし、Target のインタラクションなしにページが壊れているかどうかを確認します
* mbox のデバッグ - at.js はすべてのアクションをログに記録します
* Target Trace - Bullseye で生成された mbox トレーストークンを使用すると、決定プロセスに関与した詳細を含むトレースオブジェクトをの下で使用できます。 `window.___target_trace` オブジェクト

メモ：これらのデバッグ機能はすべて、の拡張機能で利用できます。 [Adobe Experience Platform Debugger](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)

### Web SDK の使用

Web SDK を使用する場合、複数のデバッグ機能があります。

* 使用 [Assurance](/help/assurance/home.md)
* [Web SDK デバッグ有効](/help/web-sdk/use-cases/debugging.md)
* 使用方法 [Web SDK 監視フック](https://github.com/adobe/alloy/wiki/Monitoring-Hooks)
* 使用方法 [Adobe Experience Platform Debugger](/help/debugger/home.md)
* ターゲット トレース
