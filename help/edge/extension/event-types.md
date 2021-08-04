---
title: Adobe Experience Platform Web SDK拡張機能のイベントタイプ
description: Adobe Experience Platform LaunchのAdobe Experience Platform Web SDK拡張機能で提供されるイベントタイプを使用する方法について説明します。
solution: Experience Platform
feature: Web SDK
source-git-commit: 4bddd9f23ae885468148d1592af219290d6fafd9
workflow-type: tm+mt
source-wordcount: '1026'
ht-degree: 1%

---

# イベントタイプ

このページでは、Adobe Experience Platform Web SDKタグ拡張機能が提供するAdobe Experience Platformイベントタイプについて説明します。 これらは[ルール](https://experienceleague.adobe.com/docs/launch-learn/tutorials/fundamentals/building-rules-in-launch.html)の構築に使用され、XDM](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/tracking-events.html?lang=ja)の[`eventType`フィールドと混同しないでください。

## [!UICONTROL イベントの送信完了]

通常、プロパティには、[[!UICONTROL イベント]を送信](action-types.md#send-event)アクションを使用して、Adobe Experience Platform Edgeネットワークにイベントを送信する1つ以上のルールがあります。 イベントがEdgeネットワークに送信されるたびに、応答が役立つデータと共にブラウザーに返されます。 [!UICONTROL Send event complete]イベントタイプがないと、返されたこのデータにアクセスできません。

返されたデータにアクセスするには、別のルールを作成し、[!UICONTROL Send event complete]イベントをルールに追加します。 このルールは、[!UICONTROL 「Send event]」アクションの結果、成功応答がサーバーから受信されるたびにトリガーされます。

[!UICONTROL Send event complete]イベントがルールをトリガーすると、サーバーから返されるデータが提供され、特定のタスクを実行するのに役立つ場合があります。 通常は、[!UICONTROL Send event complete]イベントを含む同じルールに（[!UICONTROL Core]拡張子から）[!UICONTROL カスタムコード]アクションを追加します。 [!UICONTROL カスタムコード]アクションでは、カスタムコードは`event`という名前の変数にアクセスできます。 この`event`変数には、サーバーから返されたデータが格納されます。

Edgeネットワークから返されたデータを処理するルールは、次のようになります。

![](./assets/send-event-complete.png)

以下に、このルールの[!UICONTROL カスタムコード]アクションを使用して特定のタスクを実行する方法の例を示します。

### パーソナライズされたコンテンツの手動レンダリング

応答データの処理ルールに含まれる「Custom Code」アクションで、サーバーから返されたパーソナライゼーション提案にアクセスできます。 これをおこなうには、次のカスタムコードを入力します。

```javascript
var propositions = event.propositions;
```

`event.propositions`が存在する場合は、パーソナライゼーション提案オブジェクトを含む配列です。 配列に含まれる提案は、大部分、イベントがサーバーに送信された方法によって決定されます。

この最初のシナリオでは、「 [!UICONTROL 決定をレンダリング] 」チェックボックスをオフにし、イベントを送信する[!UICONTROL イベント]を送信アクション内に[!UICONTROL 決定範囲]を指定していないとします。

![img.png](assets/send-event-render-unchecked-without-scopes.png)

この例では、`propositions`配列には、自動レンダリングの資格があるイベントに関連する提案のみが含まれます。

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
    "renderAttempted": false
  }
]
```

イベントを送信する際に、「 [!UICONTROL レンダリングの決定] 」チェックボックスがオンになっていないので、SDKはコンテンツの自動レンダリングを試みませんでした。 ただし、SDKは、自動レンダリングの対象となるコンテンツを自動的に取得し、必要に応じて手動でレンダリングするようユーザーに提供します。 各提案オブジェクトの`renderAttempted`プロパティが`false`に設定されていることに注意してください。

代わりに、イベントの送信時に「 [!UICONTROL レンダリングの決定] 」チェックボックスをオンにした場合、SDKは自動レンダリングの対象となる提案をレンダリングしようとしていました。 その結果、各提案オブジェクトの`renderAttempted`プロパティが`true`に設定されます。 この場合、これらの提案を手動でレンダリングする必要はありません。

これまでは、自動レンダリングの対象となるパーソナライゼーションコンテンツ(例えば、Adobe Target Visual Experience Composerを使用してVisual Experience Composerで作成されたコンテンツ)のみを確認してきました。 自動レンダリングの対象とならないパーソナライゼーションコンテンツ&#x200B;_を取得するには、[!UICONTROL イベント]を送信アクションの[!UICONTROL 判定範囲]フィールドを使用して判定範囲を指定し、コンテンツを要求します。_&#x200B;スコープとは、サーバーから取得する特定の提案を識別する文字列です。

[!UICONTROL イベント]を送信アクションは次のようになります。

![img.png](assets/send-event-render-unchecked-with-scopes.png)

この例では、提案が`salutation`スコープまたは`discount`スコープに一致するサーバー上で見つかった場合、提案が返され、`propositions`配列に含まれます。 自動レンダリングの条件を満たす提案は、[!UICONTROL Send event]アクションの[!UICONTROL Render decisions]フィールドまたは[!UICONTROL Decision scopes]フィールドの設定方法に関係なく、引き続き`propositions`配列に含まれます。 `propositions`配列の例を次に示します。

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
    "renderAttempted": false
  }
]
```

この時点で、適切に提案コンテンツをレンダリングできます。 この例では、`discount`範囲に一致する提案は、Adobe TargetのフォームベースのExperience Composerを使用して構築されたHTML提案です。 ページ上にIDが`daily-special`の要素があり、`discount`提案から`daily-special`要素にコンテンツをレンダリングするとします。 以下の操作を実行します。

1. `event`オブジェクトから提案を抽出します。
1. 各提案をループし、スコープ`discount`の提案を探します。
1. 提案が見つかった場合は、提案の各項目をループし、HTMLコンテンツの項目を探します。 （想定するよりも確認する方が良い）。
1. HTMLコンテンツを含む項目が見つかった場合は、ページ上で`daily-special`要素を探し、そのHTMLをパーソナライズされたコンテンツに置き換えます。

[!UICONTROL カスタムコード]アクション内のカスタムコードは、次のようになります。

```javascript
var propositions = event.propositions;

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
      break;
    }
  }
}

if (discountHtml) {
  // Discount HTML exists. Time to render it.
  var dailySpecialElement = document.getElementById("daily-special");
  dailySpecialElement.innerHTML = discountHtml;
}
```

### Adobe Targetレスポンストークンへのアクセス

Adobe Targetから返されるパーソナライゼーションコンテンツには、[レスポンストークン](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html)が含まれます。これは、アクティビティ、オファー、エクスペリエンス、ユーザープロファイル、地域情報などに関する詳細です。 これらの詳細は、サードパーティのツールと共有したり、デバッグに使用したりできます。 レスポンストークンはAdobe Targetユーザーインターフェイスで設定できます。

応答データの処理ルールに含まれる「Custom Code」アクションで、サーバーから返されたパーソナライゼーション提案にアクセスできます。 それには、次のカスタムコードを入力します。

```javascript
var propositions = event.propositions;
```

`event.propositions`が存在する場合は、パーソナライゼーション提案オブジェクトを含む配列です。 `result.propositions`のコンテンツについて詳しくは、[パーソナライズされたコンテンツ](#manually-render-personalized-content)を手動でレンダリングを参照してください。

Web SDKによって自動的にレンダリングされたすべての提案からすべてのアクティビティ名を収集し、単一の配列にプッシュするとします。 その後、単一のアレイをサードパーティに送信できます。 この場合、[!UICONTROL カスタムコード]アクション内にカスタムコードを記述して、次の操作を実行します。

1. `event`オブジェクトから提案を抽出します。
1. 各提案をループスルーします。
1. SDKが提案をレンダリングしたかどうかを特定します。
1. その場合は、提案の各項目をループします。
1. レスポンストークンを含むオブジェクトの`meta`プロパティからアクティビティ名を取得します。
1. アクティビティ名を配列にプッシュします。
1. アクティビティ名をサードパーティに送信します。

```javascript
var propositions = event.propositions;
if (propositions) {
  var activityNames = [];
  propositions.forEach(function(proposition) {
    if (proposition.renderAttempted) {
      proposition.items.forEach(function(item) {
        if (item.meta) {
          // item.meta contains the response tokens.
          var activityName = item.meta["activity.name"];
          // Ignore duplicates
          if (activityNames.indexOf(activityName) === -1) {
            activityNames.push(activityName);  
          }
        }
      });
    }
  });
  // Now that activity names are in an array,
  // you can send them to a third party or use
  // them in some other way.
}
```





