---
title: Web SDKでの表示イベントの管理
description: 表示イベントの概要と、Web SDKでの使用方法について説明します。
exl-id: 7150ad6e-7693-4f4d-917e-8d08a39a0b41
keywords: パーソナライゼーション；イベントの表示；sendEvent;renderDecisions;applyPropositions；提案；
source-git-commit: e150fa51953edbb0e21de962e066deedaf8bd2d7
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 0%

---

# Web SDKでの表示イベントの管理

表示イベントは、パーソナライズされた特定のコンテンツがユーザーに表示されたことをパーソナライゼーションまたは分析サービスに知らせます。 表示イベントを送信すると、ダウンストリームシステムが、*リクエストされた* コンテンツと *実際に表示された* コンテンツを区別できるので、レポートの精度が向上します。

## 表示イベントの自動送信

自動表示イベントは、通常、最も簡単なオプションです。 これらは、Web SDKが `sendEvent` 応答から適格なコンテンツのレンダリングを完了した直後に送信されるので、レポートの精度を向上させることができます。

表示イベントを自動的に送信するには、`sendEvent` を `renderDecisions` に設定し、`true` を `personalization.sendDisplayEvent` に設定する `true` 呼び出しを使用します（`true` がデフォルトなので、省略します）。

```js
alloy("sendEvent", {
  renderDecisions: true,
  personalization: { }, // sendDisplayEvent defaults to true
  xdm: {
    web: {
      webPageDetails: {
        name: "home"
      }
    }
  }
});
```

>[!NOTE]
>
>自動表示イベントは、SDKが管理するレンダリングによって異なります。 コンテンツを手動でレンダリングする場合（`applyPropositions` を使用する場合を含む）、`sendEvent` を使用して表示イベントを明示的に送信する必要があります。

## 後続の `sendEvent` 呼び出しで表示イベントを送信

後の `sendEvent` 呼び出しに表示イベントを含めると、パーソナライゼーションのリクエスト時には利用できない追加のページ読み込みデータを添付する場合に役立ちます。 一般的に、[&#x200B; 上位および下位のページイベント &#x200B;](/help/collection/use-cases/personalization/top-bottom-page-events.md) を実装する際に使用されます。 この方法でディスプレイイベントを正しく実装すると、Adobe Analyticsの [&#x200B; バウンス率 &#x200B;](https://experienceleague.adobe.com/en/docs/analytics/components/metrics/bounce-rate) に関する問題を回避できます。

1. 最初の `sendEvent` 呼び出し（多くの場合、ページの上部）で、コンテンツをリクエストしてレンダリングしますが、`renderDecisions` を `true` に、`personalization.sendDisplayEvent` を `false` に設定すると、自動表示イベントが抑制されます。

   ```js
   alloy("sendEvent", {
     renderDecisions: true,
     personalization: { sendDisplayEvent: false },
     xdm: {
       web: {
         webPageDetails: {
            name: "home"
         }
       }
     }
   });
   ```

1. 後で（多くの場合、ページの下部で）、`sendEvent` を [`personalization.includeRenderedPropositions`](/help/collection/js/commands/sendevent/personalization.md) に設定して、前のリクエスト以降にレンダリングされた提案の表示イベントを含む XDM ペイロードで `true` を呼び出します。

   ```js
   alloy("sendEvent", {
     personalization: { includeRenderedPropositions: true },
     xdm: {
       // Add any additional page load telemetry you want to send here
       web: {
         webPageDetails: {
           name: "home"
         }
       }
     }
   });
   ```

>[!NOTE]
>
>`includeRenderedPropositions` の使用時には、表示が抑制された、自動的にレンダリングされた提案のみが含まれます。

## 手動でレンダリングされた提案の表示イベントを送信

コンテンツを自分でレンダリングする場合（完全に手動でレンダリングするか、`applyPropositions` を使用する場合）、`sendEvent` コマンドを使用して表示イベントを明示的に送信する必要があります。 次のプロパティを含む XDM ペイロードを使用して `sendEvent` を呼び出します。

* レンダリングされた提案の `_experience.decisioning.propositions`、`id` および `scope` を含む `scopeDetails`
* `_experience.decisioning.propositionEventType.display` を `1` に設定

次の 2 つの例では、このヘルパー関数を使用して、表示イベント XDM ペイロードを作成します。

```js
function buildDisplayEventXdm(renderedPropositions) {
  return {
    eventType: "decisioning.propositionDisplay",
    _experience: {
      decisioning: {
        propositions: renderedPropositions.map(({ id, scope, scopeDetails }) => ({
          id,
          scope,
          scopeDetails
        })),
        propositionEventType: { display: 1 }
      }
    }
  };
}
```

次の例では、表示イベントで手動レンダリングを使用しています。

```js
function renderExample(propositions) {
  // Your custom logic here. Return ONLY the propositions that were actually rendered.
  // For example: return [propositions[0]];
  return [];
}

alloy("sendEvent", {
  personalization: { decisionScopes: ["discount"] },
  xdm: { }
}).then(({ propositions = [] }) => {
  const renderedPropositions = renderExample(propositions);
  if (!renderedPropositions.length) { return; }
  return alloy("sendEvent", { xdm: buildDisplayEventXdm(renderedPropositions) });
});
```

次の例では、`applyPropositions` コマンドを表示イベントと共に使用します。 `sendEvent`、`applyPropositions` と連結され、次に別の `sendEvent` が連結されます。

```js
alloy("sendEvent", {
  personalization: { decisionScopes: ["discount", "salutation"] },
  xdm: { }
}).then(({ propositions = [] }) => {
  return alloy("applyPropositions", {
    propositions,
    metadata: {
      salutation: { selector: "#salutation", actionType: "setHtml" },
      discount: { selector: "#daily-special", actionType: "replaceHtml" }
    }
  });
}).then(({ propositions: renderedPropositions = [] }) => {
  if (!renderedPropositions.length) { return; }
  return alloy("sendEvent", { xdm: buildDisplayEventXdm(renderedPropositions) });
});
```

## よく避けるべき間違い

* **レンダリングが完了する前に表示イベントを送信**：自動レンダリングが完了した後、`applyPropositions` が解決した後、または手動レンダリングロジックが完了した後に表示イベントを送信します。
* **レンダリングしなかった提案の表示イベントを送信**：実際にユーザーに表示された提案のみを含めます。
* **`scopeDetails`** のドロップ：表示イベントを送信する際に、提案オブジェクトから `scopeDetails` を含めます。
