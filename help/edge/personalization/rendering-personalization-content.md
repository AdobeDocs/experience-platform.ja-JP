---
title: Adobe Experience Platform Web SDK を使用したパーソナライズされたコンテンツのレンダリング
description: Adobe Experience Platform Web SDK を使用してパーソナライズされたコンテンツをレンダリングする方法について説明します。
keywords: パーソナライゼーション；renderDecisions;sendEvent;decisionScopes;propositions;
exl-id: 6a3252ca-cdec-48a0-a001-2944ad635805
source-git-commit: c75a8bdeaba67259b5f4b4ce025d5e128d763040
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 3%

---

# パーソナライズされたコンテンツのレンダリング

Adobe Experience Platform Web SDK は、次のようなパーソナライゼーションソリューションからのパーソナライズされたコンテンツのAdobeをサポートします。 [Adobe Target](https://business.adobe.com/jp/products/target/adobe-target.html), [offer decisioning](https://experienceleague.adobe.com/docs/offer-decisioning/using/get-started/starting-offer-decisioning.html?lang=ja) および [Adobe Journey Optimizer](https://experienceleague.adobe.com/docs/journey-optimizer/using/get-started/get-started.html?lang=ja).

さらに、Web SDK を使用すると、Adobe Experience Platformのパーソナライゼーションの宛先 ( [Adobe Target](../../destinations/catalog/personalization/adobe-target-connection.md) そして [カスタムパーソナライゼーション接続](../../destinations/catalog/personalization/custom-personalization.md). 同じページと次のページのパーソナライゼーション用にExperience Platformを設定する方法については、 [専用ガイド](../../destinations/ui/configure-personalization-destinations.md).

Adobe Target内で作成されたコンテンツ [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) とAdobe Journey Optimizer [ウェブキャンペーン UI](https://experienceleague.adobe.com/docs/journey-optimizer/using/web/create-web.html) は、SDK で自動的に取得およびレンダリングできます。 Adobe Target内で作成されたコンテンツ [フォームベースの Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html) またはOffer decisioningは、SDK で自動的にレンダリングできません。 代わりに、SDK を使用してこのコンテンツを要求し、自分で手動でコンテンツをレンダリングする必要があります。

## コンテンツの自動レンダリング

イベントをサーバーに送信する際に、 `renderDecisions` 選択肢 `true`. これにより、SDK は、自動レンダリングの対象となるパーソナライズされたコンテンツを自動的にレンダリングします。

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

パーソナライズされたコンテンツのレンダリングは非同期的なので、特定のコンテンツがいつレンダリングを完了するかについて前提にしないでください。

## コンテンツの手動レンダリング

パーソナライゼーションコンテンツにアクセスするには、コールバック関数を指定できます。この関数は、SDK がサーバーから正常な応答を受け取った後に呼び出されます。 コールバックは `result` オブジェクト ( `propositions` 返されたパーソナライゼーションコンテンツを含むプロパティ。 イベントの送信時にコールバック関数を指定する方法の例を以下に示します。

```javascript
alloy("sendEvent", {
    xdm: {}
  }).then(function(result) {
    if (result.propositions) {
      // Manually render propositions and send "display" event
    }
  });
```

この例では、 `result.propositions`が存在する場合、は、イベントに関連するパーソナライゼーションの提案を含む配列です。 デフォルトでは、自動レンダリングの対象となる提案のみが含まれます。

この `propositions` 配列は次の例のようになります。

```json
[
  {
    "id": "AT:eyJhY3Rpdml0eUlkIjoiMTI3MDE5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
    "scope": "__view__",
    "items": [
      {
        "id": "11223344",
        "schema": "https://ns.adobe.com/personalization/dom-action",
        "data": {
          "content": "<h2 style=\"color: yellow\">An HTML proposition.</h2>",
          "selector": "#hero",
          "type": "setHtml"
        },
        "meta": {}
      }
    ],
    "scopeDetails": {
      ...
    },
    "renderAttempted": false
  },
  {
    "id": "AT:PyJhY3Rpdml0eUlkIjoiMTI3MDE5IiwiZXhwZXJpZW5jZUlkIjoiMCJ8",
    "scope": "__view__",
    "items": [
      {
        "id": "11223345",
        "schema": "https://ns.adobe.com/personalization/dom-action",
        "data": {
          "content": "<h2 style=\"color: yellow\">Another HTML proposition.</h2>",
          "selector": "#sidebar",
          "type": "setHtml"
        },
        "meta": {}
      }
    ],
    "scopeDetails": {
      ...
    },
    "renderAttempted": false
  }
]
```

この例では、 `renderDecisions` オプションが設定されていません `true` ( `sendEvent` コマンドが実行されたので、SDK はコンテンツの自動レンダリングを試みませんでした。 ただし、SDK は、自動レンダリングの対象となるコンテンツを自動的に取得します。自動レンダリングをおこなう場合は、この値を手動でレンダリングする必要があります。 各提案オブジェクトには、 `renderAttempted` プロパティを `false`.

代わりに `renderDecisions` 選択肢 `true` イベントを送信すると、SDK は（前述のように）自動レンダリングの対象となる提案をレンダリングしようとしました。 その結果、各提案オブジェクトには、 `renderAttempted` プロパティを `true`. この場合、これらの提案を手動でレンダリングする必要はありません。

これまでは、自動レンダリングの対象となるパーソナライゼーションコンテンツ (Adobe Targetの Visual Experience Composer またはAdobe Journey Optimizerの Web Campaign UI を使用して作成されたコンテンツ ) についてのみ説明しました。 パーソナライゼーションコンテンツを取得するには _not_ 自動レンダリングの対象となる場合は、 `decisionScopes` 」オプションを使用して設定できます。 スコープとは、サーバーから取得する特定の提案を識別する文字列です。

次に例を示します。

```javascript
alloy("sendEvent", {
    xdm: {},
    decisionScopes: ['salutation', 'discount']
  }).then(function(result) {
    if (result.propositions) {
      // Manually render propositions and send "display" event
    }
  });
```

この例では、提案が `salutation` または `discount` スコープ、返され、 `result.propositions` 配列。 自動レンダリングの条件を満たす提案は、引き続き `propositions` 配列を、設定方法に関係なく `renderDecisions` または `decisionScopes` オプション。 この `propositions` 配列の場合、この例は次のようになります。

```json
[
  {
    "id": "AT:cZJhY3Rpdml0eUlkIjoiMTI3MDE5IiwiZXhwZXJpZW5jZUlkIjoiMCJ2",
    "scope": "salutation",
    "items": [
      {
        "schema": "https://ns.adobe.com/personalization/json-content-item",
        "data": {
          "id": "4433221",
          "content": {
            "salutation": "Welcome, esteemed visitor!"
          }
        },
        "meta": {}
      }
    ],
    "scopeDetails": {
      "id": "AT:cZJhY3Rpdml0eUlkIjoiMTI3MDE5IiwiZXhwZXJpZW5jZUlkIjoiMCJ2",
      "activity": {
        "id": "384456"
      }
    },  
    "renderAttempted": false
  },
  {
    "id": "AT:FZJhY3Rpdml0eUlkIjoiMTI3MDE5IiwiZXhwZXJpZW5jZUlkIjoiMCJ0",
    "scope": "discount",
    "items": [
      {
        "schema": "https://ns.adobe.com/personalization/html-content-item",
        "data": {
          "id": "4433222",
          "content": "<div>50% off your order!</div>",
          "format": "text/html"
        },
        "meta": {}
      }
    ],
    "scopeDetails": {
      "id": "AT:FZJhY3Rpdml0eUlkIjoiMTI3MDE5IiwiZXhwZXJpZW5jZUlkIjoiMCJ0",
      "activity": {
        "id": "384457"
      }
    },
    "renderAttempted": false
  },
  {
    "id": "AT:eyJhY3Rpdml0eUlkIjoiMTI3MDE5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
    "scope": "__view__",
    "items": [
      {
        "id": "11223344",
        "schema": "https://ns.adobe.com/personalization/dom-action",
        "data": {
          "content": "<h2 style=\"color: yellow\">An HTML proposition.</h2>",
          "selector": "#hero",
          "type": "setHtml"
        },
        "meta": {}
      }
    ],
    "scopeDetails": {
      "id": "AT:PyJhY3Rpdml0eUlkIjoiMTI3MDE5IiwiZXhwZXJpZW5jZUlkIjoiMCJ8",
      "activity": {
        "id": "384459"
      }
    },
    "renderAttempted": false
  },
  {
    "id": "AT:PyJhY3Rpdml0eUlkIjoiMTI3MDE5IiwiZXhwZXJpZW5jZUlkIjoiMCJ8",
    "scope": "__view__",
    "items": [
      {
        "id": "11223345",
        "schema": "https://ns.adobe.com/personalization/dom-action",
        "data": {
          "content": "<h2 style=\"color: yellow\">Another HTML proposition.</h2>",
          "selector": "#sidebar",
          "type": "setHtml"
        },
        "meta": {}
      }
    ],
    "scopeDetails": {
      "id": "AT:PyJhY3Rpdml0eUlkIjoiMTI3MDE5IiwiZXhwZXJpZW5jZUlkIjoiMCJ8",
      "activity": {
        "id": "384459"
      }
    },
    "renderAttempted": false
  }
]
```

この時点で、適切に提案コンテンツをレンダリングできます。 この例では、 `discount` 範囲は、Adobe Targetのフォームベースの Experience Composer を使用して構築されたHTMLの提案です。 ページ上に、という ID を持つ要素があるとします。 `daily-special` 次の場所からコンテンツをレンダリングしたい場合、 `discount` 提案 `daily-special` 要素を使用する場合は、次の操作を行います。

1. 提案を `result` オブジェクト。
1. 各提案をループし、範囲を持つ提案を探します。 `discount`.
1. 提案が見つかった場合は、提案内の各項目をループし、HTMLコンテンツの項目を探します。 （想定するよりも確認する方が良い）。
1. HTMLコンテンツを含む項目が見つかった場合は、 `daily-special` 要素を作成し、その要素をパーソナライズされたコンテンツにHTMLで置き換えます。
1. コンテンツがレンダリングされた後、 `display` イベント。

コードは次のようになります。

```javascript
alloy("sendEvent", {
  xdm: {},
  decisionScopes: ['salutation', 'discount']
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

  var discountHtml;
  if (discountProposition) {
    // Find the item from proposition that should be rendered.
    // Rather than assuming there a single item that has HTML
    // content, find the first item whose schema indicates
    // it contains HTML content.
    for (var j = 0; j < discountProposition.items.length; j++) {
      var discountPropositionItem = discountProposition.items[i];
      if (discountPropositionItem.schema === "https://ns.adobe.com/personalization/html-content-item") {
        discountHtml = discountPropositionItem.data.content;
        // Render the content
        var dailySpecialElement = document.getElementById("daily-special");
        dailySpecialElement.innerHTML = discountHtml;
        
        // For this example, we assume there is only a signle place to update in the HTML.
        break;  
      }
    }
      // Send a "display" event 
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


>[!TIP]
>
>Adobe Targetを使用する場合、スコープはサーバー上の mbox に対応しますが、すべて個別にリクエストされるのではなく、一度にリクエストされる点が異なります。 グローバル mbox は常に返されます。

### フリッカーの管理

SDK は、次の機能を提供します。 [ちらつきを制御](../personalization/manage-flicker.md) パーソナライゼーションプロセス中に

## 指標を増分せずに単一ページアプリケーションで提案をレンダリング {#applypropositions}

この `applyPropositions` コマンドを使用すると、次の提案の配列をレンダリングまたは実行できます： [!DNL Target] を単一ページのアプリケーションに追加するときに、 [!DNL Analytics] および [!DNL Target] 指標。 これにより、レポートの精度が向上します。

>[!IMPORTANT]
>
>If Propositions for the `__view__` 範囲（または Web サーフェス）は、ページ読み込み時にレンダリングされ、 `renderAttempted` フラグが `true`. この `applyPropositions` コマンドは再レンダリングしません `__view__` 次の条件を持つ範囲（または Web 表面）の提案 `renderAttempted: true` フラグ。

### 使用例 1:単一ページのアプリケーションビューの提案を再レンダリング

以下のサンプルで説明する使用例では、表示通知を送信せずに、以前に取得され、レンダリングされた買い物かご表示の提案を再レンダリングします。

次の例では、 `sendEvent` コマンドは、ビューの変更時にトリガーされ、結果のオブジェクトを定数で保存します。

次に、ビューまたはコンポーネントが更新されると、 `applyPropositions` 前の `sendEvent` 」コマンドを使用して、ビューの提案を再レンダリングします。

```js
var cartPropositions = alloy("sendEvent", {
    renderDecisions: true,
    xdm: {
        web: {
            webPageDetails: {
                viewName: "cart"
            }
        }
    }
}).then(function(result) {
    var propositions = result.propositions;

    // Collect response tokens, etc.
    return propositions;
});

// Call applyPropositions to re-render the view propositions from the previous sendEvent command.
alloy("applyPropositions", {
    propositions: cartPropositions
});
```

### 使用例 2:セレクターのない提案をレンダリング

この使用例は、 [!DNL Target Form-based Experience Composer].

セレクター、アクションおよび範囲は、 `applyPropositions` 呼び出し。

サポート `actionTypes` 次の場合：

* `setHtml`
* `replaceHtml`
* `appendHtml`

```js
// Retrieve propositions for salutation and discount scopes
alloy("sendEvent", {
    decisionScopes: ["salutation", "discount"]
}).then(function(result) {
    var retrievedPropositions = result.propositions;
    // Render propositions on the page by providing additional metadata

    return alloy("applyPropositions", {
        propositions: retrievedPropositions,
        metadata: {
            salutation: {
                selector: "#first-form-based-offer",
                actionType: "setHtml"
            },
            discount: {
                selector: "#second-form-based-offer",
                actionType: "replaceHtml"
            }
        }
    }).then(function(applyPropositionsResult) {
        var renderedPropositions = applyPropositionsResult.propositions;

        // Send the display notifications via sendEvent command
        alloy("sendEvent", {
            xdm: {
                eventType: "decisioning.propositionDisplay",
                _experience: {
                    decisioning: {
                        propositions: renderedPropositions
                    }
                }
            }
        });
    });
});
```

決定範囲にメタデータを指定しない場合、関連する提案はレンダリングされません。
