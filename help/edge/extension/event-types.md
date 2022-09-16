---
title: Adobe Experience Platform Web SDK 拡張機能のイベントタイプ
description: Adobe Experience Platform LaunchのAdobe Experience Platform Web SDK 拡張機能で提供されるイベントタイプを使用する方法について説明します。
solution: Experience Platform
exl-id: b3162406-c5ce-42ec-ab01-af8ac8c63560
source-git-commit: 5218e6cf82b74efbbbcf30495395a4fe2ad9fe14
workflow-type: tm+mt
source-wordcount: '1024'
ht-degree: 1%

---

# イベントタイプ

このページでは、 Adobe Experience Platform Web SDK タグ拡張機能で提供されるAdobe Experience Platformイベントタイプについて説明します。 これらは、 [ルールの作成](https://experienceleague.adobe.com/docs/platform-learn/data-collection/tags/build-rules.html) そして、 [`eventType` XDM のフィールド](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/tracking-events.html?lang=ja).

## [!UICONTROL イベント送信完了]

通常、プロパティには、 [[!UICONTROL イベントを送信] アクション](action-types.md#send-event) イベントをAdobe Experience Platform Edge Network に送信する場合。 イベントが Edge ネットワークに送信されるたびに、応答が役立つデータと共にブラウザーに返されます。 を使用しない [!UICONTROL イベント送信完了] イベントタイプの場合、この戻り値のデータに対するアクセス権はありません。

返されるデータにアクセスするには、別のルールを作成し、 [!UICONTROL イベント送信完了] イベントをルールに追加します。 このルールは、 [!UICONTROL イベントを送信] アクション。

When a [!UICONTROL イベント送信完了] イベントトリガールールを使用すると、サーバーから返されるデータを提供します。このデータは、特定のタスクを実行するのに役立つ場合があります。 通常、 [!UICONTROL カスタムコード] アクション ( [!UICONTROL コア] ) を [!UICONTROL イベント送信完了] イベント。 内 [!UICONTROL カスタムコード] アクションを使用すると、カスタムコードは `event`. この `event` 変数には、サーバーから返されたデータが含まれます。

Edge Network から返されたデータを処理するルールは、次のようになります。

![](./assets/send-event-complete.png)

以下に、 [!UICONTROL カスタムコード] アクションを設定してください。

### パーソナライズされたコンテンツを手動でレンダリング

応答データの処理ルールに含まれる「 Custom Code 」アクションで、サーバーから返されたパーソナライゼーションの提案にアクセスできます。 これをおこなうには、次のカスタムコードを入力します。

```javascript
var propositions = event.propositions;
```

If `event.propositions` が存在する場合、パーソナライゼーションの提案オブジェクトを含む配列です。 配列に含まれる提案は、大部分、イベントがサーバーに送信された方法によって決定されます。

この最初のシナリオでは、 [!UICONTROL 決定をレンダリング] チェックボックスをオンにし、何も指定していない [!UICONTROL 決定範囲] 内側 [!UICONTROL イベントを送信] イベントの送信を担当するアクション。

![img.png](assets/send-event-render-unchecked-without-scopes.png)

この例では、 `propositions` 配列には、自動レンダリングの対象となるイベントに関連する提案のみが含まれます。

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

イベントを送信する際に、 [!UICONTROL 決定をレンダリング] のチェックボックスがオフになっていたので、SDK はコンテンツの自動レンダリングを試みませんでした。 ただし、SDK は、自動レンダリングの対象となるコンテンツを自動的に取得し、自動レンダリングをおこなう場合は手動でレンダリングするように指定しました。 各提案オブジェクトには、 `renderAttempted` プロパティを `false`.

代わりに [!UICONTROL 決定をレンダリング] 「 」チェックボックスをオンにすると、SDK は、イベントを送信する際に、自動レンダリングの対象となる提案をレンダリングしようとしました。 その結果、各提案オブジェクトには、 `renderAttempted` プロパティを `true`. この場合、これらの提案を手動でレンダリングする必要はありません。

これまでは、自動レンダリングの対象となるパーソナライゼーションコンテンツ ( 例えば、Adobe Targetの Visual Experience Composer で作成されたコンテンツ ) のみを確認してきました。 パーソナライゼーションコンテンツを取得するには _not_ 自動レンダリングの対象となる場合は、 [!UICONTROL 決定範囲] フィールド [!UICONTROL イベントを送信] アクション。 スコープとは、サーバーから取得する特定の提案を識別する文字列です。

この [!UICONTROL イベントを送信] アクションは次のようになります。

![img.png](assets/send-event-render-unchecked-with-scopes.png)

この例では、提案が `salutation` または `discount` スコープ、返され、 `propositions` 配列。 自動レンダリングの条件を満たす提案は、引き続き `propositions` 配列を作成します。 [!UICONTROL 決定をレンダリング] または [!UICONTROL 決定範囲] フィールド [!UICONTROL イベントを送信] アクション。 この `propositions` 配列の場合、この例は次のようになります。

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

この時点で、適切に提案コンテンツをレンダリングできます。 この例では、 `discount` 範囲は、Adobe Targetのフォームベースの Experience Composer を使用して構築されたHTMLの提案です。 ページ上に、という ID を持つ要素があるとします。 `daily-special` 次の場所からコンテンツをレンダリングしたい場合、 `discount` 提案 `daily-special` 要素。 次の手順を実行します。

1. 提案を `event` オブジェクト。
1. 各提案をループし、範囲を持つ提案を探します。 `discount`.
1. 提案が見つかった場合は、提案内の各項目をループし、HTMLコンテンツの項目を探します。 （想定するよりも確認する方が良い）。
1. HTMLコンテンツを含む項目が見つかった場合は、 `daily-special` 要素を作成し、その要素をパーソナライズされたコンテンツにHTMLで置き換えます。

カスタムコードを [!UICONTROL カスタムコード] アクションは次のように表示されます。

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

Adobe Targetから返されるパーソナライゼーションコンテンツには次が含まれます [レスポンストークン](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html)：アクティビティ、オファー、エクスペリエンス、ユーザープロファイル、地域情報などに関する詳細です。 これらの詳細は、サードパーティのツールと共有したり、デバッグに使用したりできます。 レスポンストークンはAdobe Targetユーザーインターフェイスで設定できます。

応答データの処理ルールに含まれる「 Custom Code 」アクションで、サーバーから返されたパーソナライゼーションの提案にアクセスできます。 これをおこなうには、次のカスタムコードを入力します。

```javascript
var propositions = event.propositions;
```

If `event.propositions` が存在する場合、パーソナライゼーションの提案オブジェクトを含む配列です。 詳しくは、 [パーソナライズされたコンテンツを手動でレンダリング](#manually-render-personalized-content) 内容の詳細 `result.propositions`.

Web SDK によって自動的にレンダリングされたすべての提案からすべてのアクティビティ名を収集し、単一の配列にプッシュするとします。 その後、単一の配列をサードパーティに送信できます。 この場合、 [!UICONTROL カスタムコード] アクション：

1. 提案を `event` オブジェクト。
1. 各提案をループします。
1. SDK が提案をレンダリングしたかどうかを判断します。
1. その場合は、提案の各項目をループします。
1. からアクティビティ名を取得します。 `meta` プロパティ。レスポンストークンを含むオブジェクトです。
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
