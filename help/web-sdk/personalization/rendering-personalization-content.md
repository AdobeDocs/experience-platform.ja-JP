---
title: Adobe Experience Platform Web SDKを使用したパーソナライズされたコンテンツのレンダリング
description: Adobe Experience Platform web SDKを使用してパーソナライズされたコンテンツをレンダリングする方法について説明します。
keywords: パーソナライゼーション；renderDecisions;sendEvent;decisionScopes；提案；
exl-id: 6a3252ca-cdec-48a0-a001-2944ad635805
source-git-commit: 35429ec2dffacb9c0f2c60b608561988ea487606
workflow-type: tm+mt
source-wordcount: '947'
ht-degree: 1%

---

# パーソナライズされたコンテンツのレンダリング

Adobe Experience Platform Web SDKでは、[Adobe Target、](https://business.adobe.com/jp/products/target/adobe-target.html)Offer Decisioning、[Adobe Journey Optimizer](https://experienceleague.adobe.com/docs/offer-decisioning/using/get-started/starting-offer-decisioning.html?lang=ja) などのAdobe パーソナライゼーションソリューションからパーソナライゼーショ [ されたコンテンツの取得をサポートしてい ](https://experienceleague.adobe.com/docs/journey-optimizer/using/get-started/get-started.html?lang=ja) す。

さらに、Web SDKは、[Adobe Target](../../destinations/catalog/personalization/adobe-target-connection.md) や [ カスタムパーソナライゼーション接続 ](../../destinations/catalog/personalization/custom-personalization.md) などのAdobe Experience Platform パーソナライゼーション宛先を通じて、同じページおよび次のページのパーソナライゼーション機能を強化します。 同じページと次のページのパーソナライゼーションに対応するExperience Platformの設定方法については、[ 専用ガイド ](../../destinations/ui/activate-edge-personalization-destinations.md) を参照してください。

Adobe Targetの [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) およびAdobe Journey Optimizerの [Web キャンペーン UI](https://experienceleague.adobe.com/docs/journey-optimizer/using/web/create-web.html) で作成されたコンテンツは、SDKによって自動的に取得され、レンダリングされます。 Adobe Target[ フォームベースの Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html)、Adobe Journey Optimizer[ コードベースの Experience Channel](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/code-based-experience/get-started-code-based) またはOffer Decisioning内で作成されたコンテンツは、SDKによって自動的にレンダリングすることはできません。 代わりに、SDKを使用してこのコンテンツをリクエストし、手動でコンテンツをレンダリングする必要があります。

## コンテンツの自動レンダリング {#automatic}

イベントをサーバーに送信する場合は、`renderDecisions` オプションを `true` に設定できます。 これにより、自動レンダリングの対象となる、パーソナライズされたコンテンツがSDKで自動的にレンダリングされます。

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

パーソナライズされたコンテンツのレンダリングは非同期なので、特定のコンテンツのレンダリングが完了するタイミングを推測しないでください。

## コンテンツの手動レンダリング {#manual}

パーソナライゼーションコンテンツにアクセスするには、コールバック関数を指定します。この関数は、SDKがサーバーから正常に応答を受け取った後に呼び出されます。 コールバックには `result` オブジェクトが指定され、このオブジェクトには、返されたパーソナライゼーションコンテンツを含む `propositions` プロパティを含めることができます。 次に、イベントを送信する際にコールバック関数を提供する方法の例を示します。

```javascript
alloy("sendEvent", {
    "xdm": {}
  }).then(function(result) {
    if (result.propositions) {
      // Manually render propositions and send "display" event
    }
  });
```

この例では、`result.propositions` が存在する場合、はイベントに関連するパーソナライゼーションの提案を含む配列です。 デフォルトでは、自動レンダリングの対象となる提案のみが含まれます。

`propositions` の配列は、次の例のようになります。

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

この例では、`renderDecisions` コマンドが実行されたときに「`true`」オプションが「`sendEvent`」に設定されていなかったので、SDKはコンテンツを自動的にレンダリングしようとしませんでした。 ただし、SDKは自動レンダリングの対象となるコンテンツを自動的に取得し、必要に応じて手動でレンダリングできるように提供しています。 各提案オブジェクトの `renderAttempted` プロパティが `false` に設定されていることに注意してください。

イベントを送信する際に代わりに `renderDecisions` オプションを `true` に設定した場合、SDKは（前述のように）自動レンダリングの対象となる提案をレンダリングしようとしたことになります。 その結果、各提案オブジェクトの `renderAttempted` プロパティが `true` に設定されます。 この場合、これらの提案を手動でレンダリングする必要はありません。

これまで、自動レンダリングの対象となるパーソナライゼーションコンテンツ（Adobe Journey Optimizerの Visual Experience Composer またはAdobe Targetの Web キャンペーン UI で作成されたすべてのコンテンツ）についてのみ説明しました。 自動レンダリングの対象となるパーソナライゼーションコンテンツ _なし_ を取得するには、イベントの送信時に `decisionScopes` オプションを設定して、コンテンツをリクエストする必要があります。 範囲は、サーバーから取得したい特定の提案を識別する文字列です。

次に例を示します。

```javascript
alloy("sendEvent", {
    "xdm": {},
    "decisionScopes": ['salutation', 'discount']
  }).then(function(result) {
    if (result.propositions) {
      // Manually render propositions and send "display" event
    }
  });
```

この例では、`salutation` または `discount` の範囲に一致する提案がサーバーで見つかった場合、その提案が返され、`result.propositions` 配列に含まれます。 自動レンダリングの条件を満たす提案は、`propositions` または `renderDecisions` のオプションの設定方法に関係なく、引き続き `decisionScopes` の配列に含まれます。 この場合、`propositions` の配列は次の例のようになります。

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

この時点で、提案コンテンツを自由にレンダリングできます。 この例では、`discount` 範囲に一致する提案が、Adobe Targetのフォームベースの Experience Composer を使用して作成されたHTMLの提案です。 ページに `daily-special` という ID を持つ要素があり、`discount` の提案のコンテンツを `daily-special` 要素にレンダリングする場合は、次の操作を行います。

1. `result` オブジェクトから提案を抽出します。
1. 各提案を反復処理し、`discount` の範囲で提案を探します。
1. 提案が見つかった場合は、提案の各項目をループして、HTML コンテンツである項目を探します。 （想定するよりも確認する方が良いです。
1. HTMLのコンテンツを含んだ項目が見つかった場合は、該当するページで `daily-special` 要素を見つけ、そのHTMLをパーソナライズされたコンテンツに置き換えます。
1. コンテンツがレンダリングされたら、`display` イベントを送信します。

コードは次のようになります。

```javascript
alloy("sendEvent", {
  "xdm": {},
  "decisionScopes": ['salutation', 'discount']
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
    // Rather than assuming there is a single item that has HTML
    // content, find the first item whose schema indicates
    // it contains HTML content.
    for (var j = 0; j < discountProposition.items.length; j++) {
      var discountPropositionItem = discountProposition.items[i];
      if (discountPropositionItem.schema === "https://ns.adobe.com/personalization/html-content-item") {
        discountHtml = discountPropositionItem.data.content;
        // Render the content
        var dailySpecialElement = document.getElementById("daily-special");
        dailySpecialElement.innerHTML = discountHtml;
        
        // For this example, we assume there is only a single place to update in the HTML.
        break;  
      }
    }
    // Send a "display" event 
    alloy("sendEvent", {
      "xdm": {
        "eventType": "decisioning.propositionDisplay",
        "_experience": {
          "decisioning": {
            "propositions": [
              {
                "id": discountProposition.id,
                "scope": discountProposition.scope,
                "scopeDetails": discountProposition.scopeDetails
              }
            ],
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


>[!TIP]
>
>Adobe Targetを使用する場合、範囲はサーバー上の mbox に対応します。ただし、個別に要求されるのではなく、すべて一度に要求される点が異なります。 グローバル mbox は常に返されます。

### フリッカーの管理

SDKには、パーソナライゼーションプロセス中に [ ちらつきの管理 ](../personalization/manage-flicker.md) を行う機能が用意されています。

## 単一ページアプリケーションで、指標を増分せずに提案をレンダリングします {#applypropositions}

`applyPropositions` コマンドを使用すると、[!DNL Target] またはAdobe Journey Optimizerからシングルページアプリケーションに提案の配列をレンダリングまたは実行できます。[!DNL Analytics] および [!DNL Target] の指標を増分する必要はありません。 これにより、レポートの精度が向上します。

>[!IMPORTANT]
>
>`__view__` 範囲（または web サーフェス）の提案がページの読み込み時にレンダリングされた場合、その `renderAttempted` フラグは `true` に設定されます。 `applyPropositions` コマンドは、`__view__` フラグを持つ `renderAttempted: true` スコープ （または web サーフェス）の提案を再レンダリングしません。

### ユースケース 1：単一ページアプリケーションビューの提案の再レンダリング

以下のサンプルで説明されているユースケースでは、表示通知を送信せずに、以前に取得してレンダリングした買い物かご表示の提案を再レンダリングします。

以下の例では、ビューの変更時に `sendEvent` コマンドがトリガーされ、結果のオブジェクトが定数に保存されます。

次に、ビューまたはコンポーネントが更新されると、`applyPropositions` コマンドが呼び出され、前の `sendEvent` コマンドの提案が表示されます。これにより、ビューの提案が再レンダリングされます。

```js
var cartPropositions = alloy("sendEvent", {
    "renderDecisions": true,
    "xdm": {
        "web": {
            "webPageDetails": {
                "viewName": "cart"
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
    "propositions": cartPropositions
});
```

### ユースケース 2：セレクターを持たない提案のレンダリング

このユースケースは、[!DNL Target Form-based Experience Composer] またはAdobe Journey Optimizer [ コードベースの Experience Channel](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/code-based-experience/get-started-code-based) を使用して作成したエクスペリエンスに当てはまります。

`applyPropositions` 呼び出しでは、セレクター、アクション、範囲を指定する必要があります。

サポートされる `actionTypes` は次のとおりです。

* `setHtml`
* `replaceHtml`
* `appendHtml`

```js
// Retrieve propositions for salutation and discount scopes
alloy("sendEvent", {
    "decisionScopes": ["salutation", "discount"]
}).then(function(result) {
    var retrievedPropositions = result.propositions;
    // Render propositions on the page by providing additional metadata

    return alloy("applyPropositions", {
        "propositions": retrievedPropositions,
        "metadata": {
            "salutation": {
                "selector": "#first-form-based-offer",
                "actionType": "setHtml"
            },
            "discount": {
                "selector": "#second-form-based-offer",
                "actionType": "replaceHtml"
            }
        }
    }).then(function(applyPropositionsResult) {
        var renderedPropositions = applyPropositionsResult.propositions;

        // Send the display notifications via sendEvent command
        function sendDisplayEvent(proposition) {
            const {
                id,
                scope,
                scopeDetails = {}
            } = proposition;

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
});
```

決定範囲のメタデータを指定しない場合、関連する提案はレンダリングされません。
