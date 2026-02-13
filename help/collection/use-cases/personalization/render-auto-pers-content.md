---
title: DOM アクションの提案を自動的にレンダリング
description: Web SDKを使用して、適格な DOM アクションの提案を自動的にレンダリングし、一般的な SPA リレンダリングシナリオを処理します。
keywords: パーソナライゼーション；renderDecisions;dom-action;sendEvent;applyPropositions；単一ページアプリケーション；
source-git-commit: e150fa51953edbb0e21de962e066deedaf8bd2d7
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# DOM アクションの提案を自動的にレンダリング

パーソナライゼーションの応答に提案項目がスキーマに含まれる場合は、このパターンを使用します。

**`https://ns.adobe.com/personalization/dom-action`**

これらの項目には通常、セレクターとアクションタイプ（`setHtml` など）が含まれ、`renderDecisions` が有効な場合に web SDKによって自動的に適用されます。

## 1.ちらつきの管理（オプション）

パーソナライズされたコンテンツの適用中にちらつきを防止する必要がある場合は、実装に推奨されるちらつき管理手法を使用します。 使用可能なオプションについては、[&#x200B; ちらつきの管理 &#x200B;](manage-flicker.md) を参照してください。

## 2.自動レンダリングで記録された決定のリクエストとレンダリング

`renderDecisions` コマンドを呼び出す場合は、`true` を `sendEvent` に設定します。 省略すると、`renderDecisions` プロパティの既定値は false になります。

```js
alloy("sendEvent", {
  renderDecisions: true,
  xdm: {
    web: {
      webPageDetails: {
        name: "home"
      }
    }
  }
});
```

オプションで、特定のプレースメントをリクエストする必要がある場合は、`personalization.decisionScopes` を含めます。

```js
alloy("sendEvent", {
  renderDecisions: true,
  personalization: {
    decisionScopes: ["hero-banner", "recommendations"]
  },
  xdm: { }
});
```

詳細については、[`personalization`](/help/collection/js/commands/sendevent/personalization.md) コマンドの [`sendEvent`](/help/collection/js/commands/sendevent/overview.md) オブジェクトを参照してください。

## &#x200B;3. イベントの表示

`renderDecisions` を `true` に設定し、`personalization.sendDisplayEvent` を `true` に設定するか省略すると、Web SDKはパーソナライゼーションがレンダリングされた直後に表示イベントを送信します。

```js
alloy("sendEvent", {
  renderDecisions: true,
  personalization: {
    // sendDisplayEvent defaults to true when omitted
  },
  xdm: { }
});
```

[&#x200B; 上位および下位のページイベント &#x200B;](display-events.md) を使用する場合など、実装のニーズを満たす代替オプションについては、[&#x200B; 表示イベントの管理 &#x200B;](top-bottom-page-events.md) を参照してください。

## &#x200B;4. SPA ビューの変更と再レンダリング

単一ページアプリケーションの場合は、ビュー変更イベントの `viewName` を含めます。

```js
alloy("sendEvent", {
  renderDecisions: true,
  xdm: {
    web: {
      webPageDetails: {
        viewName: "cart"
      }
    }
  }
});
```

SPA が、新しい決定フェッチを行わずに、同じビューに対して UI を再レンダリングした場合は、以前に返された提案を再適用できます。

```js
let lastPropositions = [];

alloy("sendEvent", {
  renderDecisions: true,
  xdm: {
    web: { webPageDetails: { viewName: "cart" } }
  }
}).then(({ propositions = [] }) => {
  lastPropositions = propositions;
});

// Later, after a UI re-render:
alloy("applyPropositions", {
  propositions: lastPropositions
});
```

詳細は、[`applyPropositions`](/help/collection/js/commands/applypropositions.md) を参照してください。

>[!NOTE]
>
>`applyPropositions` コマンドは、表示イベントを自動的に送信しません。 シナリオを再レンダリングするために「表示」を記録する必要がある場合は、[&#x200B; 表示イベントの管理 &#x200B;](display-events.md) を参照してください。
