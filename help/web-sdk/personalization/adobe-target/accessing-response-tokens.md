---
title: Adobe Experience Platform Web SDK を使用したレスポンストークンへのアクセス
description: Adobe Experience Platform Web SDK を使用して応答トークンにアクセスする方法を説明します。
keywords: パーソナライゼーション；Target;Adobe Target;renderDecisions;sendEvent;decisionScopes;result.decisions、応答トークン；
exl-id: fc9d552a-29ba-4693-9ee2-599c7bc76cdf
source-git-commit: b6e084d2beed58339191b53d0f97b93943154f7c
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 1%

---

# レスポンストークンへのアクセス

Adobe Targetから返されるPersonalization コンテンツには、アクティビティ、オファー、エクスペリエンス、ユーザープロファイル、地域情報などに関する詳細である [ レスポンストークン ](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=ja) が含まれます。 これらの詳細は、サードパーティのツールと共有したり、デバッグに使用したりできます。 レスポンストークンは、Adobe Target ユーザーインターフェイスで設定できます。

任意のパーソナライゼーションコンテンツにアクセスするには、イベントの送信時にコールバック関数を指定します。 このコールバックは、SDK がサーバーから正常に応答を受信した後に呼び出されます。 コールバックには `result` オブジェクトが提供され、このオブジェクトには、返されたパーソナライゼーションコンテンツを含む `propositions` プロパティを含めることができます。 コールバック関数の提供例を以下に示します。

```javascript
alloy("sendEvent", {
    renderDecisions: true,
    xdm: {}
  }).then(function(result) {
    if (result.propositions) {
      // Manually render propositions
    }
  });
```

この例では、`result.propositions` が存在する場合、はイベントに関連するパーソナライゼーションの提案を含む配列です。 パーソナライゼーションコンテンツのコンテンツについて詳しくは、[ パーソナライゼーションコンテ `result.propositions` のレンダリング ](../rendering-personalization-content.md) を参照してください。

Web SDK によって自動的にレンダリングされたすべての提案からすべてのアクティビティ名を収集し、1 つの配列にプッシュするとします。 その後、単一のアレイをサードパーティに送信できます。 この場合の解決策は、次のとおりです。

1. `result` オブジェクトから提案を抽出します。
1. 各提案をループ処理します。
1. SDK によって提案がレンダリングされたかどうかを判断します。
1. その場合は、提案の各項目をループします。
1. `meta` プロパティからアクティビティ名を取得します。これは、応答トークンを含むオブジェクトです。
1. アクティビティ名を配列にプッシュします。
1. アクティビティ名をサードパーティに送信します。

コードは次のようになります。

```javascript
alloy("sendEvent", {
    renderDecisions: true,
    xdm: {}
  }).then(function(result) {
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
  });
```
