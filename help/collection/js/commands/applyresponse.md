---
title: applyResponse
description: Edge Networkからの応答を使用して、web SDKを初期化します。
exl-id: 0653b8f7-33f0-43a1-97f5-59a51270f660
source-git-commit: db7e6df1b1a0eb19518d9c6ccd6e6bb9131d5a3e
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# `applyResponse`

`applyResponse` コマンドを使用すると、Edge Networkからの応答に基づいて様々なアクションを実行できます。 これは、通常、サーバーがEdge Networkに対して最初の呼び出しを行うハイブリッドデプロイメントで使用されます。 このコマンドは、その呼び出しから応答を受け取り、ブラウザーで web SDKを初期化します。

設定済みの Web SDK インスタンスを呼び出す際に、`applyResponse` コマンドを実行します。 設定オプションを含むオブジェクトは、次のフィールドをサポートしています。

* **`renderDecisions`**：自動レンダリングの対象となるパーソナライズされたコンテンツを web SDKで強制的にレンダリングするブール値です。 [`renderDecisions`](sendevent/renderdecisions.md) コマンドの [`sendEvent`](sendevent/overview.md) と同じです。
* **`responseHeaders`**：文字列ヘッダー名から文字列ヘッダー値へのマップ。
* **`responseBody`**：必須。 Edge Networkへのサーバーコールからの JSON 応答本文。
* **`personalization.sendDisplayEvent`**:[`personalization.sendDisplayEvent`](sendevent/personalization.md) コマンドで `sendEvent` と同じように動作するブール値。

```js
alloy("applyResponse",{
  "renderDecisions": true,
  "responseHeaders": {},
  "responseBody": {},
  "personalization": {
    "sendDisplayEvent": true
  }
});
```

## 応答オブジェクト

このコマンドを使用して [&#x200B; 応答を処理 &#x200B;](command-responses.md) する場合、応答オブジェクトで次のプロパティを使用できます。

* **`propositions`**:Edge Networkから返される提案の配列。 自動的にレンダリングされる提案には、`renderAttempted` に設定され `true` フラグが含まれます。
* **`inferences`**：このユーザーに関する機械学習情報を含む、推論オブジェクトの配列。
* **`destinations`**:Edge Networkによって返される宛先オブジェクトの配列。

## Web SDK タグ拡張機能を使用して応答を適用

このコマンドと同等の web SDK タグ拡張機能は、[**[!UICONTROL Apply response]**](/help/tags/extensions/client/web-sdk/actions/apply-response.md) のアクションです。
