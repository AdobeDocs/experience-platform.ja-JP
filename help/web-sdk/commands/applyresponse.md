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

この `applyResponse` コマンドを使用すると、Edge Networkからの応答に基づいて様々なアクションを実行できます。 通常、Edge Networkがサーバーに対して最初の呼び出しを行うハイブリッド環境で使用されます。 このコマンドは、その呼び出しから応答を受け取り、ブラウザーで Web SDK を初期化します。

## Web SDK タグ拡張機能を使用して応答を適用

応答の適用は、Adobe Experience Platform Data Collection タグインターフェイスのルール内のアクションとして実行されます。

1. へのログイン [experience.adobe.com](https://experience.adobe.com) Adobe IDの資格情報を使用します。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL ルール]**&#x200B;を選択してから、目的のルールを選択します。
1. 次の下 [!UICONTROL アクション]、既存のアクションを選択するか、アクションを作成します。
1. を [!UICONTROL 拡張機能] ドロップダウンフィールドの移動先 **[!UICONTROL Adobe Experience Platform Web SDK]**、を設定します。 [!UICONTROL アクションタイプ] 対象： **[!UICONTROL 応答を適用]**.
1. 目的のフィールドを右側に設定します。
1. クリック **[!UICONTROL 変更を保持]**&#x200B;次に、公開ワークフローを実行します。

## Web SDK JavaScript ライブラリを使用して応答を適用

を実行 `applyResponse` コマンドは、設定した Web SDK のインスタンスを呼び出す際に使用します。 設定オプションを含むオブジェクトは、次のフィールドをサポートしています。

* **`renderDecisions`**：自動レンダリングの対象となるパーソナライズされたコンテンツを Web SDK で強制的にレンダリングするブール値です。 次と同じ [`renderDecisions`](sendevent/renderdecisions.md) が含まれる [`sendEvent`](sendevent/overview.md) コマンド。
* **`responseHeaders`**：文字列ヘッダー名から文字列ヘッダー値へのマップ。
* **`responseBody`**：必須。 Edge Networkに対するサーバーコールからの JSON 応答本文。
* **`personalization.sendDisplayEvent`**：と同じように動作するブール値 [`personalization.sendDisplayEvent`](sendevent/personalization.md) が含まれる `sendEvent` コマンド。

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

以下を行う場合 [応答を処理](command-responses.md) このコマンドを使用すると、応答オブジェクトで次のプロパティを使用できます。

* **`propositions`**:Edge Networkによって返される提案の配列。 自動的にレンダリングされる提案にはフラグが含まれます `renderAttempted` をに設定 `true`.
* **`inferences`**：推論オブジェクトの配列。このオブジェクトには、このユーザーに関する機械学習情報が含まれています。
* **`destinations`**:Edge Networkによって返される宛先オブジェクトの配列。
