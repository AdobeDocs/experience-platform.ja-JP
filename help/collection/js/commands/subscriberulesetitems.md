---
title: subscribeRulesetItems
description: subscribeRulesetItems コマンドを使用して、特定のサーフェスのコンテンツカードを購読します。
exl-id: bc932ba5-a810-4fa6-82cc-998af39fdd34
source-git-commit: db7e6df1b1a0eb19518d9c6ccd6e6bb9131d5a3e
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 4%

---

# `subscribeRulesetItems`

`subscribeRulesetItems` コマンドを使用すると、ルールセットが満たされた結果である提案を購読できます。 これを行うには、フィルタリングの基準にするサーフェスとスキーマを指定し、コールバック関数を指定します。

ルールセットが評価されるたびに、コールバック関数は提案の配列を含んだ `result` オブジェクトを受け取ります。

>[!IMPORTANT]
>
>`subscribeRulesetItems` コマンドは、ルールセットから取得した提案を取得する唯一の方法です。これは、[`sendEvent`](sendevent/overview.md) の結果と一緒に返されないからです。


```js
alloy("subscribeRulesetItems", {
  surfaces: ["web://example.com/#welcome"],
  schemas: ["https://ns.adobe.com/personalization/message/content-card"],
  callback: (result, collectEvent) => {
    const { propositions = [] } = result;
    renderMyPropositions(propositions);
    collectEvent("display", propositions);    
  },
});
```

上記のコードは、コンテンツカードの `web://example.com/#welcome` サーフェスにサブスクライブし、`collectEvent` の便利なメソッドを使用して、すべての提案に対して `display` イベントを生成します。

## コマンドオプション {#command-options}

このコマンドは、次のプロパティを持つ `options` オブジェクトを取得します。

| プロパティ | タイプ | 説明 |
| --- | --- | --- |
| `surfaces` | 文字列配列 | サーフェスのリスト 提案は、ここで提供されるサーフェスのいずれかに一致する場合にのみ、コールバック関数によって受け取られます。 |
| `schemas` | 文字列配列 | スキーマのリスト。 提案は、ここで提供されるスキーマのいずれかと一致する場合にのみ、コールバック関数によって受け取られます。 |
| `callback` | 関数 | 提案がルールセットの満たされた結果である場合に呼び出されるコールバック関数。 コールバック関数は、呼び出されたときに 2 つのパラメーター（`result` と `collectEvent`）を受け取ります。 詳しくは、[&#x200B; コールバックパラメーター &#x200B;](#callback-parameters) を参照してください。 |

### コールバックパラメーター {#callback-parameters}

コールバック関数は、呼び出されたときに、以下の表で説明している 2 つのパラメーターを受け取ります。

| パラメーター | タイプ | 説明 |
| --- | --- | --- |
| `result` | オブジェクト | このオブジェクトには `propositions` 配列が含まれています。  これらの提案は、ルールセットが満たされている場合の直接的な結果です。 `result` オブジェクトは、[&#x200B; 句を使用して &#x200B;](command-responses.md) が返す `sendEvent`result オブジェクト `then` と同じ構造になっています。 |
| `collectEvent` | 関数 | Edge Network イベントを送信して、インタラクション、ディスプレイ、その他のイベントをトラッキングするのに使用できる便利な関数。 |

### `collectEvent` 関数 {#collectevent-function}

`collectEvent` 関数は、Edge Network イベントを送信して、インタラクション、ディスプレイ、その他のイベントをトラッキングするための便利な関数です。 次の表で説明する 2 つのパラメーターを受け入れます。

| パラメーター | タイプ | 説明 |
| --- | --- | --- |
| イベントタイプ | 文字列 | 生成する提案イベントタイプを示す文字列。 サポートされるイベントタイプは、`display`、`interact`、`dismiss` です。 |
| `propositions` | 配列 | イベントに対応する提案の配列。 |

## Web SDK タグ拡張機能を使用したコンテンツカードの購読

コマンド応答と同等の web SDK タグ拡張機能は、[**[!UICONTROL Subscribe ruleset items]**](/help/tags/extensions/client/web-sdk/event-types.md#subscribe-ruleset-items) イベントをサブスクライブするルールです。 イベントを使用して、目的のスキーマとサーフェスを指定できます。
