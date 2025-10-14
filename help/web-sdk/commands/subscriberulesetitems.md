---
title: subscribeRulesetItems
description: subscribeRulesetItems コマンドを使用して、特定のサーフェスのコンテンツカードを購読します。
source-git-commit: 01cba985e22e4673fb60721c810ac9cbee287386
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 3%

---


# `subscribeRulesetItems`

`subscribeRulesetItems` コマンドを使用すると、ルールセットが満たされた結果である提案を購読できます。 これを行うには、フィルタリングの基準にするサーフェスとスキーマを指定し、コールバック関数を指定します。

ルールセットが評価されるたびに、コールバック関数は提案の配列を含んだ `result` オブジェクトを受け取ります。

>[!IMPORTANT]
>
>`subscribeRulesetItems` コマンドは、ルールセットから取得した提案を取得する唯一の方法です。これは、[`sendEvent`](sendevent/overview.md) の結果と一緒に返されないからです。

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
| `result` | オブジェクト | このオブジェクトには `propositions` 配列が含まれています。  これらの提案は、ルールセットが満たされている場合の直接的な結果です。 `result` オブジェクトは、`then` 句を使用して `sendEvent` が返す [result オブジェクト &#x200B;](command-responses.md) と同じ構造になっています。 |
| `collectEvent` | 関数 | Edge Networkイベントを送信して、インタラクション、ディスプレイ、その他のイベントをトラッキングするために使用できる便利な関数。 |

### `collectEvent` 関数 {#collectevent-function}

`collectEvent` 関数は、Edge Networkイベントを送信して、インタラクション、ディスプレイ、その他のイベントをトラッキングするための便利な関数です。 次の表で説明する 2 つのパラメーターを受け入れます。

| パラメーター | タイプ | 説明 |
| --- | --- | --- |
| イベントタイプ | 文字列 | 生成する提案イベントタイプを示す文字列。 サポートされるイベントタイプは、`display`、`interact`、`dismiss` です。 |
| `propositions` | 配列 | イベントに対応する提案の配列。 |

## Web SDK タグ拡張機能を使用したコンテンツカードの購読 {#tag-extension}

次の手順に従って、タグユーザーインターフェイスを通じてコンテンツカードを購読します。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL ルール]** に移動し、目的のルールを選択します。
1. [!UICONTROL &#x200B; イベント &#x200B;] の下で、既存のイベントを選択するか、新しいイベントを作成します。
1. [!UICONTROL &#x200B; 拡張機能 &#x200B;] ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、**[!UICONTROL イベントタイプ]** を **[!UICONTROL 購読ルールセット項目]** に設定します。
1. 画面の右側で、コンテンツカードを購読するスキーマとサーフェスを選択します。
1. 「**[!UICONTROL 変更を保持]**」を選択して、公開ワークフローを実行します。

## Web SDK JavaScript ライブラリを使用したコンテンツカードのサブスクライブ {#library}

次のサンプルコードでは、コンテンツカードの `web://mywebsite.com/#welcome` サーフェスをサブスクライブし、`collectEvent` の便利なメソッドを使用して、すべての提案に対して `display` イベントを生成します。

```js
alloy("subscribeRulesetItems", {
  surfaces: ["web://mywebsite.com/#welcome"],
  schemas: ["https://ns.adobe.com/personalization/message/content-card"],
  callback: (result, collectEvent) => {
    const { propositions = [] } = result;
    renderMyPropositions(propositions);
    collectEvent("display", propositions);    
  },
});
```
