---
title: Adobe Experience Platform Web SDK を使用したレスポンストークンへのアクセス
description: Adobe Experience Platform Web SDK を使用してレスポンストークンにアクセスする方法について説明します。
keywords: パーソナライゼーション；target;adobe target;renderDecisions;sendEvent;decisionScopes;result.decisions，レスポンストークン；
exl-id: fc9d552a-29ba-4693-9ee2-599c7bc76cdf
source-git-commit: b6e084d2beed58339191b53d0f97b93943154f7c
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 2%

---

# レスポンストークンへのアクセス

Adobe Targetから返されるパーソナライゼーションコンテンツには次が含まれます [レスポンストークン](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html)：アクティビティ、オファー、エクスペリエンス、ユーザープロファイル、地域情報などに関する詳細です。 これらの詳細は、サードパーティのツールと共有したり、デバッグに使用したりできます。 レスポンストークンはAdobe Targetユーザーインターフェイスで設定できます。

パーソナライゼーションコンテンツにアクセスするには、イベントの送信時にコールバック関数を指定します。 このコールバックは、SDK がサーバーから成功した応答を受け取った後に呼び出されます。 コールバックには `result` オブジェクト ( `propositions` 返されたパーソナライゼーションコンテンツを含むプロパティ。 以下に、コールバック関数を指定する例を示します。

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

この例では、 `result.propositions`が存在する場合、は、イベントに関連するパーソナライゼーションの提案を含む配列です。 詳しくは、 [パーソナライゼーションコンテンツのレンダリング](../rendering-personalization-content.md) の内容に関する詳細 `result.propositions`.

Web SDK によって自動的にレンダリングされたすべての提案からすべてのアクティビティ名を収集し、単一の配列にプッシュするとします。 その後、単一の配列をサードパーティに送信できます。 この場合の解決策は、次のとおりです。

1. 提案を `result` オブジェクト。
1. 各提案をループします。
1. SDK が提案をレンダリングしたかどうかを判断します。
1. その場合は、提案の各項目をループします。
1. からアクティビティ名を取得します。 `meta` プロパティ。レスポンストークンを含むオブジェクトです。
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
