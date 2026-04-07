---
title: subscribeRulesetItems
description: subscribeRulesetItems コマンドを使用して、特定のサーフェスのコンテンツカードをサブスクライブします。
exl-id: bc932ba5-a810-4fa6-82cc-998af39fdd34
source-git-commit: 3ecfc2258e63a34a739ab8b296437c357d1dd9d1
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 3%

---

# `subscribeRulesetItems`

`subscribeRulesetItems` コマンドを使用すると、満たされたルールセットの結果である提案をサブスクライブできます。 これを行うには、フィルタリングするサーフェスとスキーマを指定し、コールバック関数を指定します。

ルールセットは、[`sendEvent`](sendevent/overview.md) コマンドが送信されるたびに評価されます。 コールバック関数は、その中に提案の配列を持つ`result` オブジェクトを受け取ります。

>[!IMPORTANT]
>
>`subscribeRulesetItems` コマンドは、ルールセットから提案を取得する唯一の方法です。提案は[`sendEvent`](sendevent/overview.md)件の結果と同時に返されないからです。 提案がキャプチャされるように、`sendEvent`を呼び出す前にサブスクリプションを設定する必要があります。


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

上記のコードは、コンテンツカードの`web://example.com/#welcome` サーフェスをサブスクライブし、すべての提案に`collectEvent`の便利なメソッドを使用して`display`のイベントを発生させます。

## コマンドオプション {#command-options}

このコマンドは、次のプロパティを持つ`options` オブジェクトを取ります。

| プロパティ | タイプ | 説明 |
| --- | --- | --- |
| `surfaces` | 文字列配列 | サーフェスのリスト。 提案は、ここで提供されているサーフェスのいずれかに一致する場合にのみ、コールバック関数で受け取られます。 |
| `schemas` | 文字列配列 | スキーマのリスト 提案は、ここで提供されているスキーマのいずれかに一致する場合にのみ、コールバック関数で受け取られます。 |
| `callback` | 関数 | 提案が満たされたルールセットの結果である場合に呼び出されるコールバック関数。 コールバック関数は、呼び出されたときに2つのパラメーター（`result`と`collectEvent`）を受け取ります。 詳しくは、[&#x200B; コールバックパラメーター](#callback-parameters)を参照してください。 |

>[!TIP]
>
>追加の値を`surfaces`および`schemas`配列に渡すことにより、1つのコマンドで複数のサーフェスとスキーマをサブスクライブできます。

### コールバックパラメーター {#callback-parameters}

コールバック関数は、呼び出されたときに、以下の表に記載されている2つのパラメーターを受け取ります。

| パラメーター | タイプ | 説明 |
| --- | --- | --- |
| `result` | オブジェクト | このオブジェクトには`propositions`配列が含まれています。  これらの提案は、満たされたルールセットの直接の結果です。 `result` オブジェクトは、[句を使用して](command-responses.md)が返した`sendEvent`結果オブジェクト `then`と同じ構造になっています。 |
| `collectEvent` | 関数 | Edge Network イベントを送信して、インタラクション、ディスプレイ、その他のイベントを追跡するために使用できる利便性関数。 |

### `collectEvent` 関数 {#collectevent-function}

`collectEvent`関数は、Edge Network イベントを送信して、インタラクション、ディスプレイ、その他のイベントをトラッキングするために使用できる利便性関数です。 次の表に示す2つのパラメーターを受け入れます。

| パラメーター | タイプ | 説明 |
| --- | --- | --- |
| イベントタイプ | 文字列 | 生成する提案イベントタイプを示す文字列。 サポートされているイベントタイプは`display`、`interact`または`dismiss`です。 |
| `propositions` | 配列 | イベントに対応する提案の配列。 |


`collectEvent`関数は、コールバックの外部で独立して呼び出すことができます。 この関数の呼び出しは、ユーザーのアクションに対する応答など、後でインタラクションまたは却下を追跡する場合に便利です。

```js
collectEvent("interact", propositions);
```

## Web SDK タグ拡張機能を使用したコンテンツカードの購読

コマンド応答と同等のWeb SDK タグ拡張機能は、[**[!UICONTROL Subscribe ruleset items]**](/help/tags/extensions/client/web-sdk/event-types.md#subscribe-ruleset-items) イベントをサブスクライブするルールです。 このイベントを使用すると、目的のスキーマとサーフェスを指定できます。
