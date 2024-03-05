---
title: applyResponse
description: Edge Network からの応答を使用して、Web SDK を初期化します。
source-git-commit: f75dcfc945be2f45c1638bdd4d670288aef6e1e6
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 0%

---

# `applyResponse`

The `applyResponse` コマンドを使用すると、Edge Network からの応答に基づいて様々なアクションを実行できます。 通常、サーバーが Edge ネットワークへの最初の呼び出しをおこなうハイブリッドデプロイメントで使用されます。 このコマンドは、その呼び出しから応答を取得し、ブラウザーで Web SDK を初期化します。

## Web SDK タグ拡張機能を使用して応答を適用する

応答の適用は、「 Adobe Experience Platform Data Collection Tags 」インターフェイスのルール内のアクションとして実行されます。

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL ルール]**&#x200B;を選択し、目的のルールを選択します。
1. の下 [!UICONTROL アクション]、既存のアクションを選択するか、アクションを作成します。
1. を設定します。 [!UICONTROL 拡張] ドロップダウンフィールド **[!UICONTROL Adobe Experience Platform Web SDK]**&#x200B;をクリックし、 [!UICONTROL アクションタイプ] から **[!UICONTROL 応答を適用]**.
1. 右側の目的のフィールドを設定します。
1. クリック **[!UICONTROL 変更を保持]**&#x200B;を開き、パブリッシュワークフローを実行します。

## Web SDK JavaScript ライブラリを使用した応答の適用

を実行します。 `applyResponse` コマンドを使用して、Web SDK の設定済みインスタンスを呼び出す際に呼び出すことができます。 設定オプションを含むオブジェクトは、次のフィールドをサポートします。

* **`renderDecisions`**：自動レンダリングの対象となるパーソナライズされたコンテンツを Web SDK がレンダリングするように強制するブール値です。 次と同一 [`renderDecisions`](sendevent/renderdecisions.md) （内） [`sendEvent`](sendevent/overview.md) コマンドを使用します。
* **`responseHeaders`**：文字列ヘッダー名と文字列ヘッダー値のマップ。
* **`responseBody`**：必須。Edge ネットワークへのサーバー呼び出しからの JSON 応答本文。
* **`personalization.sendDisplayEvent`**：と同じように動作するブール値 [`personalization.sendDisplayEvent`](sendevent/personalization.md) （内） `sendEvent` コマンドを使用します。

```js
allow("applyResponse",{
  "renderDecisions": true,
  "responseHeaders": {},
  "responseBody": {},
  "personalization": {
    "sendDisplayEvent": true
  }
});
```

## 応答オブジェクト

もしあなたが [応答を処理する](command-responses.md) このコマンドを使用すると、次のプロパティが応答オブジェクトで使用できます。

* **`propositions`**:Edge ネットワークから返される提案の配列。 自動的にレンダリングされる提案には、フラグが含まれます `renderAttempted` に設定 `true`.
* **`inferences`**：このユーザーに関する機械学習情報を含む、推論オブジェクトの配列。
* **`destinations`**:Edge ネットワークから返される宛先オブジェクトの配列。
