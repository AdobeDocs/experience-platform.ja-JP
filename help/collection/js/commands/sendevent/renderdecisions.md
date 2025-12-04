---
title: renderDecisions
description: 自動レンダリングの対象となる、パーソナライズされたコンテンツをレンダリングします。
exl-id: 6f7a3531-c2b6-4e90-a7ad-9f0fe4dc39e9
source-git-commit: c6a2b9700f0a688f65fec9febf5622c6c7b6aafa
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---

# `renderDecisions`

`renderDecisions` プロパティを使用すると、自動レンダリングの対象となるパーソナライズされたコンテンツを web SDKで強制的にレンダリングできます。

`renderDecisions` コマンドを実行するときは、`sendEvent` のブール値を設定します。 省略すると、このプロパティは既定で `false` に設定されます。 パーソナライズされたコンテンツを自動的にレンダリングする場合は、このプロパティを `true` に設定します。

>[!IMPORTANT]
>
>`renderDecisions` プロパティは [`documentUnloading`](documentunloading.md) プロパティと互換性がありません。 両方のプロパティを同時に `true` に設定しないでください。

```js
alloy("sendEvent", {
  "xdm": adobeDataLayer.getState(reference),
  "renderDecisions": true
});
```

このプロパティを `true` に設定する場合は、関連するスコープまたはサーフェスも含めてください。 これらのスコープまたはサーフェスは、自動的に要求することも（ページの最初の `sendEvent` コマンドなど）、明示的に要求することもできます（[`personalization.decisionScopes`](personalization.md#personalizationdecisionscopes) または [`personalization.surfaces`](personalization.md#personalizationsurfaces) を使用）。 パーソナライゼーションのレンダリング時によくある問題は、次のようなシナリオです。

1. `sendEvent` コマンドは、設定されていない（デフォルトは `renderDecisions`）デフォルトのパーソナライゼーションを要求 `false` るページの早い段階で実行されます。 提案は取得されますが、レンダリングされません。
1. ページの後半で、`sendEvent` が `renderDecisions` に設定され、スコープやサーフェスが含まれていない別の `true` トリガーが表示されます。 この 2 番目の呼び出しにはスコープまたはサーフェスがないので、何もレンダリングされません。

この問題は、次のいずれかの方法で回避できます。

* 最初の `renderDecisions` 呼び出しで `true` を `sendEvent` に設定する。または
* `decisionScopes` を `surfaces` に設定する場合、後続の `sendEvent` 呼び出しで `renderDecisions` または `true` を明示的に設定します。

## Web SDK タグ拡張機能を使用して決定をレンダリング

このプロパティに相当する Web SDK タグ拡張機能は、「[**」アクション内の**](/help/tags/extensions/client/web-sdk/actions/send-event.md#personalization-fields) ビジュアルパーソナライゼーションの決定をレンダリング [!UICONTROL Send event] チェックボックスです。
