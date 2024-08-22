---
title: Adobe Experience Platform Web SDK 拡張機能のイベントタイプ
description: Adobe Experience Platform LaunchのAdobe Experience Platform Web SDK 拡張機能で提供されるイベントタイプの使用方法について説明します。
solution: Experience Platform
exl-id: b3162406-c5ce-42ec-ab01-af8ac8c63560
source-git-commit: 666e8c6fcccf08d0841c5796677890409b22d794
workflow-type: tm+mt
source-wordcount: '1127'
ht-degree: 0%

---

# イベントタイプ

ここでは、Adobe Experience Platform Web SDK タグ拡張機能で提供されるAdobe Experience Platform イベントタイプについて説明します。 これらは [ ルールの作成 ](https://experienceleague.adobe.com/docs/platform-learn/data-collection/tags/build-rules.html?lang=ja) に使用されるので、[`xdm` オブジェクトの `eventType` フィールドと混同しないでください ](/help/web-sdk/commands/sendevent/xdm.md)。

## [!UICONTROL  送信イベントの完了 ]

通常、Adobe Experience Platform Edge Networkにイベントを送信するには、プロパティに [[!UICONTROL  イベントを送信 ] アクション ](action-types.md#send-event) を使用する 1 つ以上のルールがあります。 イベントがEdge Networkに送信されるたびに、役立つデータが記載されたレスポンスがブラウザーに返されます。 [!UICONTROL  イベント完了を送信 ] イベントタイプがないと、返されたデータにアクセスできません。

返されたデータにアクセスするには、別のルールを作成してから、[!UICONTROL  イベント完了を送信 ] イベントをルールに追加します。 このルールは、[!UICONTROL  イベントを送信 ] アクションの結果として、サーバーから正常な応答を受信するたびにトリガーされます。

[!UICONTROL Send event complete] イベントがルールをトリガーすると、特定のタスクを実行するのに役立つ可能性のあるデータがサーバーから返されます。 通常は、[!UICONTROL  イベント完了を送信 ] イベントを含む同じルールに（[!UICONTROL  コア ] 拡張機能からの [!UICONTROL  カスタムコード ] アクションを追加します。 [!UICONTROL  カスタムコード ] アクションでは、カスタムコードは `event` という名前の変数にアクセスできます。 この `event` 変数には、サーバーから返されたデータが含まれます。

Edge Networkから返されるデータを処理するルールは、次のようになります。

![](assets/send-event-complete.png)

このルールで [!UICONTROL  カスタムコード ] アクションを使用して特定のタスクを実行する方法の例を以下に示します。

### パーソナライズされたコンテンツの手動レンダリング

応答データを処理するルールにあるカスタムコードアクションでは、サーバーから返されたパーソナライゼーションの提案にアクセスできます。 それには、次のカスタムコードを入力します。

```javascript
var propositions = event.propositions;
```

`event.propositions` が存在する場合、パーソナライゼーションの提案オブジェクトを含む配列です。 配列に含まれる提案の大部分は、イベントがサーバーにどのように送信されたかによって決定されます。

この最初のシナリオでは、「[!UICONTROL  決定をレンダリング ]」チェックボックスをオンにしておらず、イベントの送信を担当する [!UICONTROL  イベントを送信 ] アクション内に [!UICONTROL  決定範囲 ] を提供していないとします。

![img.png](assets/send-event-render-unchecked-without-scopes.png)

この例では、`propositions` 配列には、自動レンダリングの対象となるイベントに関連する提案のみが含まれています。

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

イベントを送信する際に、「[!UICONTROL  決定をレンダリング ]」チェックボックスがオフになっていたので、SDK はコンテンツを自動的にレンダリングしようとしませんでした。 ただし、SDK は自動レンダリングの対象となるコンテンツを引き続き自動的に取得し、必要に応じて手動でレンダリングするための手段を提供します。 各提案オブジェクトの `renderAttempted` プロパティが `false` に設定されていることに注意してください。

イベントを送信する際に代わりに「[!UICONTROL  決定をレンダリング ]」チェックボックスをオンにした場合、SDK は自動レンダリングの対象となる提案をレンダリングしようとしたことになります。 その結果、各提案オブジェクトの `renderAttempted` プロパティが `true` に設定されます。 この場合、これらの提案を手動でレンダリングする必要はありません。

これまで、自動レンダリングの対象となるパーソナライゼーションコンテンツ（例えば、Adobe Targetを使用して Visual Experience Composer で作成されたコンテンツ）のみを確認しました。 自動レンダリングの対象となるパーソナライゼーションコンテンツ _なし_ を取得するには、[!UICONTROL  イベントを送信 ] アクションの [!UICONTROL  決定範囲 ] フィールドを使用して決定範囲を指定してコンテンツをリクエストします。 範囲は、サーバーから取得したい特定の提案を識別する文字列です。

[!UICONTROL  イベントを送信 ] アクションは次のようになります。

![img.png](assets/send-event-render-unchecked-with-scopes.png)

この例では、`salutation` または `discount` の範囲に一致する提案がサーバーで見つかった場合、その提案が返され、`propositions` 配列に含まれます。 自動レンダリングの条件を満たす提案は、「[!UICONTROL  イベントを送信 ]」アクションの「[!UICONTROL  レンダリング決定 ]」フィールドまたは「[!UICONTROL  決定範囲 ]」フィールドをどのように設定したかに関係なく、引き続き `propositions` 配列に含まれることに注意してください。 この場合、`propositions` の配列は次の例のようになります。

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

この時点で、提案コンテンツを自由にレンダリングできます。 この例では、`discount` 範囲に一致する提案が、Adobe Targetのフォームベースの Experience Composer を使用して作成されたHTMLの提案です。 ページ上に `daily-special` という ID を持つ要素があり、`discount` の提案のコンテンツを `daily-special` 要素にレンダリングするとします。 次の手順を実行します。

1. `event` オブジェクトから提案を抽出します。
1. 各提案を反復処理し、`discount` の範囲で提案を探します。
1. 提案が見つかった場合は、提案の各項目をループして、HTMLコンテンツである項目を探します。 （想定するよりも確認する方が良いです。
1. HTMLコンテンツを含む項目が見つかった場合は、ページ上で `daily-special` 要素を見つけ、そのHTMLをパーソナライズされたコンテンツに置き換えます。

[!UICONTROL  カスタムコード ] アクション内のカスタムコードは、次のように表示される場合があります。

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

### Adobe Target応答トークンへのアクセス

Adobe Targetから返されるPersonalization コンテンツには、アクティビティ、オファー、エクスペリエンス、ユーザープロファイル、地域情報などに関する詳細である [ レスポンストークン ](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html) が含まれます。 これらの詳細は、サードパーティのツールと共有したり、デバッグに使用したりできます。 レスポンストークンは、Adobe Target ユーザーインターフェイスで設定できます。

応答データを処理するルールにあるカスタムコードアクションでは、サーバーから返されたパーソナライゼーションの提案にアクセスできます。 それには、次のカスタムコードを入力します。

```javascript
var propositions = event.propositions;
```

`event.propositions` が存在する場合、パーソナライゼーションの提案オブジェクトを含む配列です。 コンテン `result.propositions` のコンテンツについて詳しくは、[ パーソナライズされたコンテンツの手動レンダリング ](#manually-render-personalized-content) を参照してください。

Web SDK によって自動的にレンダリングされたすべての提案からすべてのアクティビティ名を収集し、1 つの配列にプッシュするとします。 その後、単一のアレイをサードパーティに送信できます。 この場合、[!UICONTROL  カスタムコード ] アクション内にカスタムコードを記述して、次の操作を行います。

1. `event` オブジェクトから提案を抽出します。
1. 各提案をループ処理します。
1. SDK によって提案がレンダリングされたかどうかを判断します。
1. その場合は、提案の各項目をループします。
1. `meta` プロパティからアクティビティ名を取得します。これは、応答トークンを含むオブジェクトです。
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

## [!UICONTROL  ルールセット項目を登録 ] {#subscribe-ruleset-items}

**[!UICONTROL ルールセット項目を登録]** イベントタイプを使用すると、サーフェスのAdobe Journey Optimizer コンテンツカードを登録できます。 ルールセットが評価されるたびに、このコマンドに提供されるコールバックは、コンテンツカードデータを保持する提案を含む結果オブジェクトを受け取ります。

![ 「購読ルールセット項目」イベントタイプを示すExperience Platformタグのユーザーインターフェイスの画像。](assets/subscribe-ruleset-items.png)

このイベントタイプは、次の設定可能なプロパティをサポートしています。

* **[!UICONTROL スキーマ]**：コンテンツカードを購読するスキーマの配列。 スキーマは手動で、またはデータ要素を指定して入力できます。
* **[!UICONTROL サーフェス]**：コンテンツカードを購読するサーフェスの配列。 サーフェスは、手動で入力することも、データ要素を指定して入力することもできます。
