---
title: コマンド応答の処理
description: JavaScript promises を使用してコマンドからの応答を処理します。
exl-id: dda98b3e-3e37-48ac-afd7-d8852b785b83
source-git-commit: 8abdd206f5fcff0e1cec7bb6ecd25c5eb9354286
workflow-type: tm+mt
source-wordcount: '224'
ht-degree: 0%

---

# コマンド応答の処理

一部の Web SDK コマンドでは、組織で役立つ可能性のあるデータを含むオブジェクトを返すことがあります。 必要に応じて、そのデータの処理方法を選択できます。 コマンドの応答は、Edge Network データが効果的に機能する必要があるので、提案と宛先にとって価値があります。

コマンド応答は、プロミスの作成時には不明な値のプロキシとして機能するJavaScript[promises](https://developer.mozilla.org/ja-JP/docs/Web/JavaScript/Reference/Global_Objects/Promise) を使用します。 値がわかると、プロミスはその値で「解決」されます。

`then` メソッドと `catch` メソッドを使用して、コマンドが成功するか失敗するかを判断します。 実装にとって目的が重要でない場合は、`then` または `catch` を省略できます。

```javascript
alloy("sendEvent", {
  "xdm": {
    "commerce": {
      "order": {
        "purchaseID": "a8g784hjq1mnp3",
        "purchaseOrderNumber": "VAU3123",
        "currencyCode": "USD",
        "priceTotal": 999.98
      }
    }
  }
}).then(function(result) {
    console.log("The sendEvent command succeeded.");
  })
  .catch(function(error) {
    console.log("The sendEvent command failed.");
  });
```

コマンドから返されるすべてのプロミスは、`result` オブジェクトを使用します。 例えば、`result` のコマンドを使用して、[`getLibraryInfo`](getlibraryinfo.md) オブジェクトからライブラリ情報を取得できます。

```js
alloy("getLibraryInfo")
  .then(function(result) {
    console.log(result.libraryInfo.version);
    console.log(result.libraryInfo.commands);
    console.log(result.libraryInfo.configs);
  });
```

この `result` オブジェクトの内容は、使用するコマンドとユーザーの同意の組み合わせによって異なります。 ユーザーが特定の目的で同意していない場合、応答オブジェクトには、ユーザーが同意した内容のコンテキストで提供できる情報のみが含まれます。

## Web SDK タグ拡張機能を使用したコマンド応答

コマンド応答と同等の web SDK タグ拡張機能は、[**[!UICONTROL Send event complete]**](/help/tags/extensions/client/web-sdk/event-types.md#send-event-complete) イベントをサブスクライブするルールです。 その後、[**[!UICONTROL Apply propositions]**](/help/tags/extensions/client/web-sdk/actions/apply-propositions.md) や [**[!UICONTROL Apply response]**](/help/tags/extensions/client/web-sdk/actions/apply-response.md) などのアクションをこのルールに含めることができます。
