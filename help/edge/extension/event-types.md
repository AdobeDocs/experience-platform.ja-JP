---
title: Adobe Experience Platform Web SDK 拡張機能のイベントタイプ
description: Adobe Experience Platform LaunchのAdobe Experience Platform Web SDK 拡張機能で提供されるイベントタイプを使用する方法について説明します。
solution: Experience Platform
feature: Web SDK
exl-id: b3162406-c5ce-42ec-ab01-af8ac8c63560
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '1024'
ht-degree: 1%

---

# イベントタイプ

このページでは、 Adobe Experience Platform Web SDK タグ拡張機能が提供するAdobe Experience Platformイベントタイプについて説明します。 これらは [ ルール ](https://experienceleague.adobe.com/docs/launch-learn/tutorials/fundamentals/building-rules-in-launch.html) の作成に使用され、XDM](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/tracking-events.html?lang=ja) の [`eventType` フィールドと混同しないでください。

## [!UICONTROL イベントの送信完了]

通常、プロパティには、[[!UICONTROL  イベント ] を送信 ](action-types.md#send-event) アクションを使用して、Adobe Experience Platform Edge ネットワークにイベントを送信する 1 つ以上のルールがあります。 イベントが Edge ネットワークに送信されるたびに、応答が役立つデータと共にブラウザーに返されます。 [!UICONTROL Send event complete] イベントタイプがないと、返されたこのデータにアクセスできません。

返されたデータにアクセスするには、別のルールを作成し、[!UICONTROL Send event complete] イベントをルールに追加します。 このルールは、[!UICONTROL 「Send event]」アクションの結果、成功した応答がサーバーから受信されるたびにトリガーされます。

[!UICONTROL Send event complete] イベントがルールをトリガーすると、サーバーから返されるデータが提供され、特定のタスクを実行するのに役立つ場合があります。 通常は、[!UICONTROL Send event complete] イベントを含む同じルールに（[!UICONTROL Core] 拡張子から）[!UICONTROL Custom code] アクションを追加します。 [!UICONTROL  カスタムコード ] アクションでは、カスタムコードは `event` という名前の変数にアクセスできます。 この `event` 変数には、サーバーから返されたデータが格納されます。

Edge ネットワークから返されたデータを処理するルールは、次のようになります。

![](./assets/send-event-complete.png)

以下は、このルールの [!UICONTROL Custom code] アクションを使用して特定のタスクを実行する方法の例です。

### パーソナライズされたコンテンツの手動レンダリング

応答データの処理ルールに含まれる「Custom Code」アクションで、サーバーから返されたパーソナライゼーション提案にアクセスできます。 これをおこなうには、次のカスタムコードを入力します。

```javascript
var propositions = event.propositions;
```

`event.propositions` が存在する場合は、パーソナライゼーション提案オブジェクトを含む配列です。 配列に含まれる提案は、大部分、イベントがサーバーに送信された方法によって決定されます。

最初のシナリオでは、「 [!UICONTROL  決定をレンダリング ] 」チェックボックスをオフにし、イベントを送信する [!UICONTROL  イベントを送信 ] アクション内に [!UICONTROL  決定範囲 ] を指定していないとします。

![img.png](assets/send-event-render-unchecked-without-scopes.png)

この例では、 `propositions` 配列には、自動レンダリングの資格があるイベントに関連する提案のみが含まれます。

`propositions` 配列は次の例のようになります。

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

イベントを送信する際に、「 [!UICONTROL  レンダリングの決定 ] 」チェックボックスはオフになっているので、SDK はコンテンツの自動レンダリングを試みませんでした。 ただし、SDK は、自動レンダリングの対象となるコンテンツを自動的に取得し、必要に応じて手動でレンダリングするようユーザーに提供します。 各提案オブジェクトの `renderAttempted` プロパティが `false` に設定されていることに注意してください。

代わりに、イベントの送信時に「 [!UICONTROL  レンダリングの決定 ] 」チェックボックスをオンにした場合、SDK は、自動レンダリングの対象となる提案をレンダリングしようとしていました。 その結果、各提案オブジェクトの `renderAttempted` プロパティが `true` に設定されます。 この場合、これらの提案を手動でレンダリングする必要はありません。

これまでは、自動レンダリングの対象となるパーソナライゼーションコンテンツ ( 例えば、Adobe Target Visual Experience Composer を使用して作成されたコンテンツ ) のみを確認してきました。 自動レンダリングの対象とならないパーソナライゼーションコンテンツ __ を取得するには、[!UICONTROL  イベント ] を送信アクションの [!UICONTROL  判定範囲 ] フィールドを使用して判定範囲を指定し、コンテンツを要求します。 スコープとは、サーバーから取得する特定の提案を識別する文字列です。

[!UICONTROL Send event] アクションは次のようになります。

![img.png](assets/send-event-render-unchecked-with-scopes.png)

この例では、提案が `salutation` スコープまたは `discount` スコープに一致するサーバー上で見つかった場合、提案は返され、`propositions` 配列に含まれます。 [!UICONTROL  イベント ] を送信アクションの [!UICONTROL  レンダリングの決定 ] フィールドまたは [!UICONTROL  決定範囲 ] フィールドの設定方法に関係なく、自動レンダリングの条件を満たす提案は、引き続き `propositions` 配列に含まれます。 `propositions` 配列は、この例のようになります。

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

この時点で、適切な位置に提案コンテンツをレンダリングできます。 この例では、`discount` スコープに一致する提案は、Adobe Targetのフォームベースの Experience Composer を使用して構築された HTML 提案です。 ページ上に ID が `daily-special` の要素があり、`discount` 提案から `daily-special` 要素にコンテンツをレンダリングするとします。 以下の操作を実行します。

1. `event` オブジェクトから提案を抽出します。
1. 各提案をループし、`discount` の範囲を持つ提案を探します。
1. 提案が見つかった場合は、提案内の各項目をループし、HTML コンテンツの項目を探します。 （確かめる方がよい）
1. HTML コンテンツを含む項目が見つかった場合は、ページで `daily-special` 要素を探し、その HTML をパーソナライズされたコンテンツに置き換えます。

[!UICONTROL Custom code] アクション内のカスタムコードは、次のようになります。

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

Adobe Targetから返されるパーソナライゼーションコンテンツには、[ レスポンストークン ](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html) が含まれます。これは、アクティビティ、オファー、エクスペリエンス、ユーザープロファイル、地域情報などに関する詳細です。 これらの詳細は、サードパーティのツールと共有したり、デバッグに使用したりできます。 レスポンストークンは、Adobe Targetユーザーインターフェイスで設定できます。

応答データの処理ルールに含まれる「Custom Code」アクションで、サーバーから返されたパーソナライゼーション提案にアクセスできます。 これをおこなうには、次のカスタムコードを入力します。

```javascript
var propositions = event.propositions;
```

`event.propositions` が存在する場合は、パーソナライゼーション提案オブジェクトを含む配列です。 `result.propositions` のコンテンツについて詳しくは、[ パーソナライズされたコンテンツを手動でレンダリング ](#manually-render-personalized-content) を参照してください。

Web SDK によって自動的にレンダリングされたすべての提案からすべてのアクティビティ名を収集し、1 つの配列にプッシュするとします。 その後、単一のアレイをサードパーティに送信できます。 この場合、[!UICONTROL  カスタムコード ] アクション内にカスタムコードを記述して、次の操作を行います。

1. `event` オブジェクトから提案を抽出します。
1. 各提案をループします。
1. SDK が提案をレンダリングしたかどうかを特定します。
1. その場合は、提案内の各項目をループします。
1. レスポンストークンを含むオブジェクトの `meta` プロパティからアクティビティ名を取得します。
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
