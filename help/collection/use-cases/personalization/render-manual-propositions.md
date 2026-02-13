---
title: 提案を手動でレンダリング
description: 独自の UI ロジック（HTML、JSON、カスタムスキーマ）を使用して提案コンテンツをレンダリングした後、表示イベントをレコードします。
keywords: パーソナライゼーション；提案；手動レンダリング；sendEvent;decisionScopes；表示イベント；
source-git-commit: e150fa51953edbb0e21de962e066deedaf8bd2d7
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# 提案を手動でレンダリング

提案項目の適用方法を完全に制御する必要がある場合は、このパターンを使用します。 例えば、JSON コンテンツから複雑な UI を構成している場合や、レンダリングの前にカスタムのビジネスルールを適用する場合などです。

## &#x200B;1. リクエストの提案

```js
alloy("sendEvent", {
  personalization: {
    decisionScopes: ["salutation", "discount"]
  },
  xdm: { }
}).then(({ propositions = [] }) => {
  // Render in the next step
});
```

詳細については、[`personalization`](/help/collection/js/commands/sendevent/personalization.md) コマンドの [`sendEvent`](/help/collection/js/commands/sendevent/overview.md) オブジェクトを参照してください。

## 2.提案項目のレンダリング（例）

提案を手動でレンダリングする場合、様々なフォームや形状を取ることができます。 次に、範囲ごとに提案を見つけ、見つかった最初のHTML コンテンツ項目を適用する最小限の例を示します。

```js
function findPropositionByScope(propositions, scope) {
  return propositions.find((p) => p.scope === scope);
}

function renderHtmlInto(selector, html) {
  const el = document.querySelector(selector);
  if (!el) return;
  el.innerHTML = html;
}

alloy("sendEvent", {
  personalization: { decisionScopes: ["discount"] },
  xdm: { }
}).then(({ propositions = [] }) => {
  const discount = findPropositionByScope(propositions, "discount");
  if (!discount) return { propositions, rendered: [] };

  const htmlItem = discount.items?.find(
    (i) => i.schema === "https://ns.adobe.com/personalization/html-content-item"
  );

  if (!htmlItem) return { propositions, rendered: [] };

  renderHtmlInto("#daily-special", htmlItem.data.content);
  return { propositions, rendered: [discount] };
});
```

>[!IMPORTANT]
>
>HTMLをレンダリングする場合は、ご利用の環境で安全であることを確認してください。 コンテンツのレンダリングをセキュリティ境界（サニタイズ、信頼できるソース、CSP に関する考慮事項）として扱う。

## &#x200B;3. レンダリング内容に関する表示イベントを記録する

手動でレンダリングした提案の場合、表示イベントは、レンダリングされた提案 `sendEvent` 参照する呼び出しを通じて記録されます。

```js
function toDisplayPayload(propositions) {
  return propositions.map((p) => ({
    id: p.id,
    scope: p.scope,
    scopeDetails: p.scopeDetails
  }));
}

// Example: record display for the propositions you actually rendered.
alloy("sendEvent", {
  xdm: {
    _experience: {
      decisioning: {
        propositions: toDisplayPayload(renderedPropositions),
        propositionEventType: { display: 1 }
      }
    }
  }
});
```

詳しくは、[&#x200B; 表示イベントの管理 &#x200B;](display-events.md) を参照してください。

## 4.再レンダリング

UI の変更に再レンダリングが必要な場合は、キャッシュした提案データに対して手動レンダリングロジックを再実行します（必要に応じて再度取得します）。 シナリオを再レンダリングするために表示を記録する必要がある場合は、[&#x200B; 表示イベントの管理 &#x200B;](display-events.md) を参照してください。
