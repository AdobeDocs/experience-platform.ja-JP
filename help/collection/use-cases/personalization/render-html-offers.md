---
title: セレクターを使用しないHTML オファーのレンダリング
description: applyPropositions にメタデータを提供してから表示イベントを記録することで、セレクターを含まないHTMLの提案項目をレンダリングします。
keywords: パーソナライゼーション；applyPropositions；メタデータ；actionType;decisionScopes；表示イベント；
source-git-commit: e150fa51953edbb0e21de962e066deedaf8bd2d7
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# セレクターを使用しないHTML オファーのレンダリング

このパターンは、提案にHTML コンテンツが含まれているが、その適用場所（セレクター）および適用方法（アクションタイプ）を指定する必要がある場合に使用します。 これを行うには、スコープでキー設定された [`applyPropositions`](/help/collection/js/commands/applypropositions.md) オブジェクトを使用して `metadata` を呼び出します。 サポートされる `actionType` 値は、`setHtml`、`replaceHtml`、`appendHtml` です。

## 1.ちらつきの管理（オプション）

レンダリング中にコンテンツを非表示にした場合、レンダリング完了後にコンテンツを表示する責任があります。 詳しくは、「[ ちらつきの管理 ](manage-flicker.md) を参照してください。

## &#x200B;2. レンダリングする範囲の提案を要求する

```js
alloy("sendEvent", {
  personalization: {
    decisionScopes: ["discount", "salutation"]
  },
  xdm: { }
}).then(({ propositions = [] }) => {
  // Render in the next step
});
```

詳細は、[`personalization.decisionScopes`](/help/collection/js/commands/sendevent/personalization.md) を参照してください。

## &#x200B;3. `applyPropositions` メタデータを使用したオファーのレンダリング

```js
alloy("sendEvent", {
  personalization: {
    decisionScopes: ["discount", "salutation"]
  },
  xdm: { }
}).then(({ propositions = [] }) => {
  return alloy("applyPropositions", {
    propositions,
    metadata: {
      salutation: {
        selector: "#salutation",
        actionType: "setHtml"
      },
      discount: {
        selector: "#daily-special",
        actionType: "replaceHtml"
      }
    }
  }).then(({ propositions: renderedPropositions = [] }) => {
    return { renderedPropositions };
  });
});
```

## &#x200B;4. レンダリングされた提案のレコード表示イベント

`applyPropositions` を呼び出しても、表示イベントは自動的には送信されません。 レンダリングが完了したら、レンダリングされた提案を参照する `sendEvent` 呼び出しを使用します。

```js
function toDisplayPayload(propositions) {
  return propositions.map((p) => ({
    id: p.id,
    scope: p.scope,
    scopeDetails: p.scopeDetails
  }));
}

alloy("sendEvent", {
  personalization: {
    decisionScopes: ["discount", "salutation"]
  },
  xdm: { }
}).then(({ propositions = [] }) => {
  return alloy("applyPropositions", {
    propositions,
    metadata: {
      salutation: { selector: "#salutation", actionType: "setHtml" },
      discount: { selector: "#daily-special", actionType: "replaceHtml" }
    }
  }).then(({ propositions: renderedPropositions = [] }) => {
    return alloy("sendEvent", {
      xdm: {
        _experience: {
          decisioning: {
            propositions: toDisplayPayload(renderedPropositions),
            propositionEventType: { display: 1 }
          }
        }
      }
    });
  });
});
```

詳しくは、[ 表示イベントの管理 ](display-events.md) を参照してください。

>[!TIP]
>
>[ 上位および下位のページイベント ](top-bottom-page-events.md) を使用する場合、この「レコード表示」呼び出しは通常、下位の `sendEvent` ージ呼び出しに実装されます。

## 5.再レンダリング

実装で後で再レンダリングが必要な場合（単一ページアプリケーションなど）、同じ提案とメタデータで `applyPropositions` を再度呼び出します。

```js
alloy("applyPropositions", {
  propositions,
  metadata: {
    discount: { selector: "#daily-special", actionType: "replaceHtml" }
  }
});
```

その再レンダリングの表示イベントを記録する必要がある場合は、[ 表示イベントの管理 ](display-events.md) を参照してください。
