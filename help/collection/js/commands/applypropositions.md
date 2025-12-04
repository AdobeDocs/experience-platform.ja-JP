---
title: applyPropositions
description: sendEvent で既にレンダリングされている提案を再レンダリングします。
exl-id: 6b79f334-4ea6-4ba4-8640-d35b7f90df98
source-git-commit: db7e6df1b1a0eb19518d9c6ccd6e6bb9131d5a3e
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# `applyPropositions`

`applyPropositions` コマンドを使用すると、[`sendEvent`](sendevent/overview.md) コマンドを使用して既にレンダリングされた提案を再レンダリングできます。 このコマンドは、単一ページアプリケーションで作業しているときにページの一部が再レンダリングされると、ページに適用済みのパーソナライゼーションが上書きされる可能性があるので便利です。

このコマンドは、次のフィールドをサポートしています。

* **提案**：再レンダリングする提案オブジェクトの配列。
* **ビュー名**: レンダリングするビューの名前。 これらの決定に対する表示通知はキャッシュされ、`sendEvent` オプションを使用して、後続の `personalization.includeRenderedPropositions` コマンドに含めることができます。
* **Meta データ**:HTML オファーの適用方法を決定するオブジェクト。 これには、次のプロパティが含まれます。
   * 範囲
   * セレクター
   * アクションタイプ

設定済みの Web SDK インスタンスを呼び出す際に、`applyPropositions` コマンドを実行します。 設定オプションを含むオブジェクトは、次のフィールドをサポートしています。

* **`propositions`**：再レンダリングする提案オブジェクトの配列。 `propositionScopes` フィールドは通常、どのスコープまたはサーフェスを再レンダリングするかを決定するので、通常、このオブジェクトは使用されません。
* **`metadata`**:HTML オファーの適用方法を決定します。 キーがスコープまたはサーフェスであり、値がキー `selector` と `actionType` を含むオブジェクトであるマップです。
   * `selector`:HTMLの適用先の CSS セレクターを含む文字列
   * `actionType`:HTMLで実行するアクション。 有効な値には、`setHtml`、`replaceHtml`、`appendHtml` などがあります。
* **`viewName`**：単一ページアプリケーションでレンダリングするビューの名前。 これらの決定に対する表示通知はキャッシュされ、`sendEvent` を使用して後続の `personalization.includeRenderedPropositions` コマンドに含めることができます。

```js
alloy("applyPropositions",{
  "propositions": [],
  "metadata": {},
  "viewName": ""
});
```

## Web SDK タグ拡張機能を使用して提案を適用します

このコマンドと同等の web SDK タグ拡張機能は、[**[!UICONTROL Apply propositions]**](/help/tags/extensions/client/web-sdk/actions/apply-propositions.md) のアクションです。
