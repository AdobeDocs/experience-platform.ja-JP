---
title: applyResponse
description: Edge Networkからの応答を使用して、Web SDK を初期化します。
exl-id: 0653b8f7-33f0-43a1-97f5-59a51270f660
source-git-commit: 74725546163f0807d3188aff5b5ffda9b8d6350b
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 0%

---

# `applyResponse`

`applyResponse` コマンドを使用すると、Edge Networkからの応答に基づいて様々なアクションを実行できます。 通常、Edge Networkがサーバーに対して最初の呼び出しを行うハイブリッド環境で使用されます。 このコマンドは、その呼び出しから応答を受け取り、ブラウザーで Web SDK を初期化します。

## Web SDK タグ拡張機能を使用して応答を適用

応答の適用は、Adobe Experience Platform Data Collection タグインターフェイスのルール内のアクションとして実行されます。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL ルール]** に移動し、目的のルールを選択します。
1. [!UICONTROL  アクション ] で、既存のアクションを選択するか、アクションを作成します。
1. [!UICONTROL  拡張機能 ] ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、[!UICONTROL  アクションタイプ ] を **[!UICONTROL Apply response]** に設定します。
1. 目的のフィールドを右側に設定します。
1. 「**[!UICONTROL 変更を保持]**」をクリックして、公開ワークフローを実行します。

## Web SDK JavaScript ライブラリを使用して応答を適用

設定済みの Web SDK インスタンスを呼び出す際に、`applyResponse` コマンドを実行します。 設定オプションを含むオブジェクトは、次のフィールドをサポートしています。

* **`renderDecisions`**：自動レンダリングの対象となるパーソナライズされたコンテンツを Web SDK で強制的にレンダリングするブール値です。 [`sendEvent`](sendevent/overview.md) コマンドの [`renderDecisions`](sendevent/renderdecisions.md) と同じです。
* **`responseHeaders`**：文字列ヘッダー名から文字列ヘッダー値へのマップ。
* **`responseBody`**：必須。 Edge Networkに対するサーバーコールからの JSON 応答本文。
* **`personalization.sendDisplayEvent`**:`sendEvent` コマンドで [`personalization.sendDisplayEvent`](sendevent/personalization.md) と同じように動作するブール値。

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

このコマンドを使用して [ 応答を処理 ](command-responses.md) する場合、応答オブジェクトで次のプロパティを使用できます。

* **`propositions`**:Edge Networkによって返される提案の配列。 自動的にレンダリングされる提案には、`true` に設定され `renderAttempted` フラグが含まれます。
* **`inferences`**：このユーザーに関する機械学習情報を含む、推論オブジェクトの配列。
* **`destinations`**:Edge Networkによって返される対象オブジェクトの配列。
