---
title: Adobe Experience Platform Web SDKを使用したパーソナライズされたコンテンツのレンダリング
description: Adobe Experience Platform Web SDKを使用してパーソナライズされたコンテンツをレンダリングする方法について説明します。
keywords: パーソナライゼーション；renderDecisions;sendEvent;decisionScopes;propositions;
exl-id: 6a3252ca-cdec-48a0-a001-2944ad635805
source-git-commit: 0246de5810c632134288347ac7b35abddf2d4308
workflow-type: tm+mt
source-wordcount: '701'
ht-degree: 2%

---

# パーソナライズされたコンテンツのレンダリング

Adobe Experience Platform Web SDKは、[Adobe Target](https://business.adobe.com/products/target/adobe-target.html)や[Offer decisioning](https://experienceleague.adobe.com/docs/offer-decisioning/using/get-started/starting-offer-decisioning.html?lang=ja)など、Adobe時のパーソナライゼーションソリューションからのパーソナライズされたコンテンツの取得をサポートします。 [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html)を使用してAdobe Target内で作成されたコンテンツは、SDKで自動的に取得およびレンダリングできます。 [フォームベースのExperience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html)またはOffer decisioningを使用してAdobe Target内で作成されたコンテンツは、SDKで自動的にレンダリングできません。 代わりに、SDKを使用してこのコンテンツを要求し、手動でコンテンツをレンダリングする必要があります。

## コンテンツの自動レンダリング

イベントをサーバーに送信する際に、`renderDecisions`オプションを`true`に設定できます。 これにより、SDKは、自動レンダリングの対象となるパーソナライズされたコンテンツを自動的にレンダリングします。

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

パーソナライズされたコンテンツのレンダリングは非同期的なので、特定のコンテンツのレンダリングが完了するタイミングを前提にしないでください。

## コンテンツの手動レンダリング

パーソナライゼーションコンテンツにアクセスするには、コールバック関数を指定できます。この関数は、SDKがサーバーから正常な応答を受け取った後に呼び出されます。 コールバックには`result`オブジェクトが提供され、このオブジェクトには、返されたパーソナライゼーションコンテンツを含む`propositions`プロパティを含めることができます。 イベントを送信する際にコールバック関数を提供する方法の例を以下に示します。

```javascript
alloy("sendEvent", {
    xdm: {}
  }).then(function(result) {
    if (result.propositions) {
      // Manually render propositions and send "display" event
    }
  });
```

この例では、`result.propositions`が存在する場合、はイベントに関連するパーソナライゼーションの提案を含む配列です。 デフォルトでは、自動レンダリングの対象となる提案のみが含まれます。

`propositions`配列は次の例のようになります。

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

この例では、`renderDecisions`コマンドの実行時に`true`オプションが設定されていないので、SDKはコンテンツの自動レンダリングを試みませんでした。 `sendEvent`ただし、SDKは、自動レンダリングの対象となるコンテンツを自動的に取得し、必要に応じて手動でレンダリングするようにユーザーに提供します。 各提案オブジェクトの`renderAttempted`プロパティが`false`に設定されていることに注意してください。

代わりに`renderDecisions`オプションを`true`に設定してイベントを送信した場合、SDKは（前述のように）自動レンダリングの対象となる提案をレンダリングしようとしました。 その結果、各提案オブジェクトの`renderAttempted`プロパティが`true`に設定されます。 この場合、これらの提案を手動でレンダリングする必要はありません。

これまでは、自動レンダリングの対象となるパーソナライゼーションコンテンツ(Adobe TargetのVisual Experience Composerを使用して作成されたすべてのコンテンツ)についてのみ説明しました。 自動レンダリングの対象とならないパーソナライゼーションコンテンツ&#x200B;_を取得するには、イベントの送信時に`decisionScopes`オプションを設定して、コンテンツをリクエストする必要があります。_&#x200B;スコープとは、サーバーから取得する特定の提案を識別する文字列です。

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

この例では、提案が`salutation`スコープまたは`discount`スコープに一致するサーバー上で見つかった場合、提案が返され、`result.propositions`配列に含まれます。 `renderDecisions`オプションや`decisionScopes`オプションの設定方法に関係なく、自動レンダリングの条件を満たす提案は引き続き`propositions`配列に含まれます。 `propositions`配列の例を次に示します。

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

この時点で、適切に提案コンテンツをレンダリングできます。 この例では、`discount`範囲に一致する提案は、Adobe TargetのフォームベースのExperience Composerを使用して構築されたHTML提案です。 ページ上にIDが`daily-special`の要素があり、`discount`提案から`daily-special`要素にコンテンツをレンダリングする場合は、次の手順を実行します。

1. `result`オブジェクトから提案を抽出します。
1. 各提案をループし、スコープ`discount`の提案を探します。
1. 提案が見つかった場合は、提案の各項目をループし、HTMLコンテンツの項目を探します。 （想定するよりも確認する方が良い）。
1. HTMLコンテンツを含む項目が見つかった場合は、ページ上で`daily-special`要素を探し、そのHTMLをパーソナライズされたコンテンツに置き換えます。
1. コンテンツがレンダリングされたら、`display`イベントを送信します。

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
        eventType: "display",
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
>Adobe Targetを使用する場合、スコープはサーバー上のmboxに対応しますが、個々にリクエストされるのではなく、すべて同時にリクエストされる点が異なります。 グローバルmboxは常に返されます。

### フリッカーの管理

SDKは、パーソナライゼーションプロセス中に[ちらつき](../personalization/manage-flicker.md)を管理する機能を提供します。
