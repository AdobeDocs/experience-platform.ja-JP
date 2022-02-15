---
title: at.js と Platform Web SDK の比較
description: at.js の機能と Web SDK の比較方法を説明します
keywords: target;adobe target;activity.id;experience.id;renderDecisions;decisionScopes；スニペットの事前非表示；vec；フォームベースの Experience Composer;xdm；オーディエンス；決定；スコープ；スキーマ；システム図；ダイアグラム
source-git-commit: 95c6d0d20ee04affb4b67c3d9f90d80e655e2752
workflow-type: tm+mt
source-wordcount: '2275'
ht-degree: 7%

---

# at.js ライブラリと Web SDK の比較

## 概要

この記事では、 `at.js` ライブラリと Experience Platform Web SDK の両方に存在します。

## ライブラリのインストール

### at.js のインストール

お客様が、「実装」タブのAdobe Experience Cloudから直接ライブラリをダウンロードできるようになっています。 at.js ライブラリは、お客様が以下のような設定でカスタマイズされています。clientCode、imsOrgId など

### Web SDK のインストール

事前ビルドバージョンは、CDN で使用できます。 CDN 上のライブラリをページで直接参照することも、独自のインフラストラクチャでダウンロードしてホストすることもできます。 縮小形式および縮小されていない形式で使用できます。 非縮小バージョンは、デバッグに役立ちます。

URL 構造：https://cdn1.adoberesources.net/alloy/[バージョン]/alloy.min.js または alloy.js （非縮小版）

例：

* 縮小済み： [https://cdn1.adoberesources.net/alloy/2.6.4/alloy.min.js](https://cdn1.adoberesources.net/alloy/2.6.4/alloy.min.js)
* 縮小解除済み： [https://cdn1.adoberesources.net/alloy/2.6.4/alloy.js](https://cdn1.adoberesources.net/alloy/2.6.4/alloy.js)

[詳細情報](../../fundamentals/installing-the-sdk.md)

## ライブラリの設定

### at.js の設定

各 at.js ファイルの最後に、設定オブジェクトをインスタンス化して渡すセクションがあります。 ダウンロード時にカスタマイズ可能で、そのセクションに現在の顧客設定が入力されます。

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

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/targetgobalsettings.html?lang=en)


### Web SDK の設定

SDK の設定は、`configure` コマンドを使用しておこないます。

>[!IMPORTANT]
>
>`configure` が *常に* 最初に呼び出されたコマンド。

例：

```javascript
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId":"ADB3LETTERSANDNUMBERS@AdobeOrg"
});
```

設定時には、さまざまなオプションを設定できます。すべてのオプションは、以下で確認できます（カテゴリ別にグループ化されています）。

[詳細情報](../../fundamentals/configuring-the-sdk.md)


## ページ読み込みターゲットオファーを要求して自動的にレンダリングする方法

### at.js の使用

at.js 2.x の使用（設定を有効にした場合） `pageLoadEnabled`を使用する場合、ライブラリは、Target Edge への呼び出しをトリガーし、 `execute -> pageLoad`. すべての設定がデフォルト値に設定されている場合、カスタムコーディングは必要ありません。at.js がページに追加され、ブラウザーによって読み込まれると、Target Edge 呼び出しが実行されます。

### Web SDK の使用

Adobe Target内で作成されたコンテンツ [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) は、SDK で自動的に取得およびレンダリングできます。

Target オファーを要求して自動的にレンダリングするには、 `sendEvent` コマンドを実行し、 `renderDecisions` 選択肢 `true`. これにより、SDK は、自動レンダリングの対象となるパーソナライズされたコンテンツを自動的にレンダリングします。

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

AEP WEB SDK は、WEB SDK によって実行されたオファーと共に通知を自動的に送信します。次に、通知リクエストペイロードの例を示します。

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

## ページ読み込みターゲットオファーを要求し、自動的にレンダリングしない方法

### at.js の使用

Target Edge への呼び出しを実行して、ページ読み込み用のオファーを取得する方法は 2 つあります。

例 1:

```javascript
adobe.target.getOffer({
   mbox: "target-global-mbox", 
   success: console.log,
   error: console.error
});
```

例 2:

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

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/cmp-atjs-functions.html?lang=en)

### Web SDK の使用

の実行 `sendEvent` 下に特別な範囲を持つ命令 `decisionScopes`: `__view__`. この範囲をシグナルとして使用し、Target からすべてのページ読み込みアクティビティを取得し、すべてのビューをプリフェッチします。 また、Web SDK は、すべての VEC ビューベースのアクティビティを評価しようとします。 ビューのプリフェッチの無効化は、現在、Web SDK でサポートされていません。

パーソナライゼーションコンテンツにアクセスするには、コールバック関数を指定できます。この関数は、SDK がサーバーから正常な応答を受け取った後に呼び出されます。 コールバックには結果オブジェクトが提供され、返されたパーソナライゼーションコンテンツを含む提案プロパティを含めることができます。

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


## 特定のフォームベースのターゲット mbox をリクエストする方法


### at.js の使用

フォームベースのコンポーザーアクティビティは、 `getOffer` 関数：

例 1:

```javascript
adobe.target.getOffer({
   mbox: "hero-banner", 
   success: console.log,
   error: console.error
});
```

例 2:

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

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/cmp-atjs-functions.html?lang=en)


### Web SDK の使用

を使用して、フォームベースのコンポーザーベースのアクティビティを取得できます `sendEvent` コマンドを使用して、 `decisionScopes` オプション。 この `sendEvent` コマンドは、リクエストされたアクティビティ/提案を含むオブジェクトで解決されたプロミスを返します。これが `propositions` 配列は次のようになります。

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

Target アクティビティを適用するには、 `applyOffers` 関数： `adobe.target.applyOffer(options)`

例：

```javascript
adobe.target.getOffers({...})
  .then(response => adobe.target.applyOffers({ response: response }))
  .then(() => console.log("Success"))
  .catch(error => console.log("Error", error));
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/adobe-target-applyoffers-atjs-2.html?lang=en)

### Web SDK の使用

この機能は、現在 Web SDK ではサポートされていません。

## イベントの追跡方法

### at.js の使用

イベントを追跡するには、 `trackEvent` 関数または `sendNotifications`.

この関数は、クリック数やコンバージョン数などのユーザーのアクションをレポートするリクエストを実行します。 応答でアクティビティを配信しません。


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

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/adobe-target-trackevent.html?lang=en)

### Web SDK の使用

イベントとユーザーアクションは、 `sendEvent` コマンド、 `_experience.decisioning.propositions` XDM フィールドグループと、 `eventType` を 2 つの値のいずれかに変更します。

* `decisioning.propositionDisplay`:Target アクティビティのレンダリングを示します。
* `decisioning.propositionInteract`:マウスのクリックと同様に、ユーザーがアクティビティを操作することを示します。

この `_experience.decisioning.propositions` XDM フィールドグループは、オブジェクトの配列です。 各オブジェクトのプロパティは、 `result.propositions` が `sendEvent` コマンド： `{ id, scope, scopeDetails }`

**例 1 — トラッキング a `decisioning.propositionDisplay` アクティビティのレンダリング後のイベント**

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

**例 2 — トラッキング a `decisioning.propositionInteract` クリック指標の発生後のイベント**

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

## 単一ページトリガーでのビューの変更の表示方法

### at.js の使用

以下を使用： `adobe.target.triggerView` 関数に置き換えます。 この関数は、新しいページが読み込まれるときや、ページ上のコンポーネントが再レンダリングされるときに呼び出すことができます。Visual Experience Composer(VEC) を使用して A/B テストおよびエクスペリエンスターゲット設定 (XT) アクティビティを作成するには、 adobe.target.triggerView() をシングルページアプリケーション (SPA) に実装する必要があります。 サイトに adobe.target.triggerView() が実装されていない場合、VEC はSPAで使用できません。

**例**

```javascript
adobe.target.triggerView("homeView")
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/adobe-target-triggerview-atjs-2.html?lang=en)


### Web SDK の使用

単一ページアプリケーションの View Change をトリガーまたはシグナルにするには、 `web.webPageDetails.viewName` プロパティ `xdm` オプション `sendEvent` コマンドを使用します。 AEP WEB SDK は、ビューキャッシュをチェックします ( `viewName` 指定： `sendEvent` 実行され、表示通知イベントが送信されます。

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

Adobe Targetから返されるパーソナライゼーションコンテンツには次が含まれます [レスポンストークン](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html)：アクティビティ、オファー、エクスペリエンス、ユーザープロファイル、地域情報などに関する詳細です。 これらの詳細は、サードパーティのツールと共有したり、デバッグに使用したりできます。 レスポンストークンはAdobe Targetユーザーインターフェイスで設定できます。

### at.js の使用

at.js カスタムイベントを使用して Target の応答をリッスンし、レスポンストークンを読み取ります。

**例**

```javascript
document.addEventListener(adobe.target.event.REQUEST_SUCCEEDED, function(e) { 
  console.log("Request succeeded", e.detail); 
}); 
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=en)


### Web SDK の使用

>[!IMPORTANT]
>
>Platform Web SDK バージョン 2.6.0 以降を使用していることを確認します。

レスポンストークンは、 `propositions` これは、 `sendEvent` コマンドを使用します。 各提案には、 `items`に設定され、各項目には `meta` オブジェクトにレスポンストークンが設定されている場合、それらが Target 管理 UI で有効になっている。 [詳細情報](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=en)

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

## ちらつきの管理方法

### at.js の使用

at.js を使用すると、 `bodyHidingEnabled: true` そのため、at.js は、DOM の変更を取得して適用する前に、パーソナライズされたコンテナを事前に非表示にしておく必要があります。
パーソナライズされたコンテンツを含むページセクションは、at.js を上書きすることで、事前に非表示にすることができます `bodyHiddenStyle`.
デフォルト `bodyHiddenStyle` HTML全体を隠す `body`.
どちらの設定も、 `window.targetGlobalSettings`. `window.targetGlobalSettings` は、at.js を読み込む前に配置する必要があります。

### Web SDK の使用

顧客は Web SDK を使用して、次の例のように、設定コマンドで事前に非表示になるスタイルを設定できます。

```javascript
alloy("configure", {
  edgeConfigId: "configurationId",
  orgId: "orgId@AdobeOrg",
  debugEnabled: true,
  prehidingStyle: "body { opacity: 0 !important }"
});
```

Web SDK を非同期的に読み込む場合は、Web SDK が挿入される前に、次のスニペットをページに挿入することをお勧めします。

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

at.js の使用に対応している A4T ログには、次の 2 種類があります。

* Analytics クライアント側ログ
* Analytics Server Side ログ

#### Analytics クライアント側ログ

**例 1:Target のグローバル設定の使用**

Analytics クライアント側のログは、 `analyticsLogging: client_side` (at.js 設定内、または `window.targetglobalSettings` オブジェクト。
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

ペイロードは、その後、Data Insertion API を使用して Analytics に転送できます。

例 2:次の間隔で設定： `getOffers` 関数：

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

応答ペイロードは次のように表示されます。

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

Analytics ペイロード (`tnta` トークン ) は、 [Data Insertion API](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md).

#### Analytics Server Side ログ

Analytics Server Side のログは、 `analyticsLogging: server_side` (at.js 設定内、または `window.targetglobalSettings` オブジェクト。
その後、データフローは次のようになります。

![](assets/a4t-server-side-atjs.png)

[詳細情報](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4timplementation.html?lang=en)

### Web SDK の使用

Web SDK では、次の機能もサポートしています。

* Analytics クライアント側のログ
* Analytics Server Side のログ

#### Analytics クライアント側ログ

Analytics クライアント側のログは、Adobe Analyticsがその DataStream 設定で無効になっている場合に有効になります。

![](assets/analytics-disabled-datastream-config.png)

お客様が Analytics トークン (`tnta`) を使用して Analytics と共有する必要がある [Data Insertion API](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md)
鎖をつけて `sendEvent` コマンドを実行し、結果の提案配列を繰り返し処理します。

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

次の図は、Analytics クライアント側が有効な場合のデータフローを示しています。

![](assets/analytics-client-side-logging.png)

#### Analytics Server Side ログ

Analytics がその DataStream 設定で有効になっている場合、Analytics Server Side Logging が有効になります。

![](assets/analytics-enabled-datastream-config.png)

サーバーサイド分析ログが有効になっている場合、Analytics レポートで正しいインプレッション数とコンバージョン数が Experience Edge レベルで共有され、顧客が追加の処理をおこなう必要がなくなるように、Analytics と共有する必要がある A4T ペイロード。

サーバー側分析ログが有効な場合にシステムにデータが送られる仕組みを次に示します。

![](assets/analytics-server-side-logging.png)

## Target のグローバル設定の設定方法

### at.js の使用

Target Standard／Premium UI や REST API を使用して設定を構成する代わりに、`window.targetGlobalSettings` を使用して at.js ライブラリで設定を上書きすることができます。

上書きは、at.js が読み込まれる前、または管理/実装/ at.js 設定を編集/コード設定/ライブラリヘッダーで定義する必要があります。

例：

```javascript
window.targetGlobalSettings = {  
   timeout: 200, // using custom timeout  
   visitorApiTimeout: 500, // using custom API timeout  
   enabled: document.location.href.indexOf('https://www.adobe.com') >= 0 // enabled ONLY on adobe.com  
};
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/targetgobalsettings.html?lang=en)

### Web SDK の使用

この機能は、Web SDK ではサポートされていません。

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

Target プロファイルを更新するには、 `sendEvent` コマンドを実行し、 `data.__adobe.target` プロパティ，キー名の前に `profile`.

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
     "entity.productName": "T-shirt",
     "entity.productId": "1234"
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
            "entity.productName": "T-shirt",
            "entity.productId": "1234"
          }
        }
    }
  }
})
.then(console.log)
.catch(console.error);
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/adobe-target-getoffers-atjs-2.html?lang=en)


### Web SDK の使用

レコメンデーションデータを送信するには、 `sendEvent` コマンドを実行し、 `data.__adobe.target` プロパティ，キー名の前に `entity`.

**例**

```javascript
alloy("sendEvent", {
  renderDecisions: true,
  data: {
    __adobe: {
      target: {
        "entity.productName": "T-shirt",
        "entity.productId": "1234"
      }
    }
  }
});
```

## サードパーティ ID の使用方法

### at.js の使用

at.js を使用すると、複数の送信方法があります `mbox3rdPartyId`，次を使用 `getOffer` または `getOffers`:

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

または、 `mbox3rdPartyId` 次のいずれか `targetPageParams` または `targetPageParamsAll`.
で設定する場合 `targetPageParams`を探すと、 `target-global-mbox` 別名 `pag-lLoad`.
レコメンデーションは、 `targetPageParamsAll` すべての target リクエストで送信されるので、
を使用する利点 `targetPageParamsAll` は、 `mbox3rdPartyId` を 1 回ページに配置すると、すべての target リクエストに適切な権限が付与されます `mbox3rdPartyId`.

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

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/targetpageparams.html?lang=en)

### Web SDK の使用

Web SDK は、Target サードパーティ ID をサポートします。 ただし、その場合はさらにいくつかの手順が必要です。 ソリューションに取り組む前に、 `identityMap`.
ID マップを使用すると、顧客は複数の ID を送信できます。 すべての ID は名前空間に分けられています。 各名前空間は 1 つ以上の ID を持つことができます。 特定の ID をプライマリとしてマークできます。
この知識を念頭に置いて、Target サードパーティ ID を使用するために Web SDK を設定するために必要な手順を確認できます。

1. データストリーム設定表示で、Target サードパーティ ID を含む名前空間を設定します。

![](assets/mbox-3-party-id-setup.png)

1. 次のように、各 sendEvent コマンドで ID 名前空間を送信します。

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

at.js を使用する場合、次の 2 つの方法でプロパティトークンを設定できます。 `targetPageParams` または `targetPageParamsAll`. 使用 `targetPageParams` プロパティトークンを `target-global-mbox` を呼び出しますが、を使用します。 `targetPageParamsAll` は、すべての target 呼び出しにトークンを追加します。

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

Web SDK を使用すると、お客様は、Adobe Target名前空間でデータストリーム設定をおこなう際に、より高いレベルでプロパティを設定できます。
![](assets/at-property-setup.png)
つまり、その特定のデータストリーム設定のすべての Target 呼び出しに、そのプロパティトークンが含まれます。

## mbox のプリフェッチ方法

### at.js の使用

この機能は、at.js 2.x でのみ使用できます。at.js 2.x には、という名前の新しい関数があります。 `getOffers`. `getOffers` お客様が 1 つ以上の mbox のコンテンツをプリフェッチすることを許可します。 次に例を示します。

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

注意：を `mbox` 内 `mboxes` 配列には独自のインデックスがあります。 通常、最初の mbox には `index=0`次の `index=1`など

### Web SDK の使用

この機能は、現在、Web SDK ではサポートされていません。

## Target 実装のデバッグ方法

### at.js の使用

at.js は次のデバッグ機能を公開します。

* Mbox の無効化 — Target の取得およびレンダリングを無効にして、Target とのやり取りなしでページが壊れているかどうかを確認します
* mbox デバッグ — at.js は、すべてのアクションを記録します。
* Target Trace - Bullseye で生成された mbox トレーストークンを使用して、判定プロセスに関係する詳細を含むトレースオブジェクトを以下で利用できます。 `window.___target_trace` object

注意：これらのデバッグ機能はすべて、 [Adobe Experience Platform Debugger](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)

### Web SDK の使用

Web SDK を使用する場合、次の複数のデバッグ機能があります。

* 使用 [グリフォン](https://aep-sdks.gitbook.io/docs/beta/project-griffon)
* [Web SDK のデバッグが有効](../../../edge/fundamentals/debugging.md)
* 用途 [Web SDK 監視フック](https://github.com/adobe/alloy/wiki/Monitoring-Hooks)
* 用途 [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger/using-v2/experience-cloud-debugger.html?lang=en)
* ターゲットトレース
