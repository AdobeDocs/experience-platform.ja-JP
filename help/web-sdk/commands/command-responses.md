---
title: コマンド応答の処理
description: JavaScript promises を使用してコマンドからの応答を処理します。
exl-id: dda98b3e-3e37-48ac-afd7-d8852b785b83
source-git-commit: f75dcfc945be2f45c1638bdd4d670288aef6e1e6
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 0%

---

# コマンド応答の処理

一部の Web SDK コマンドでは、組織で役立つ可能性のあるデータを含むオブジェクトを返す場合があります。 必要に応じて、そのデータの処理方法を選択できます。 コマンドの応答は、効果的に機能するためにEdge Networkデータが必要なので、提案と宛先に役立ちます。

コマンド応答は、プロミスの作成時には不明な値のプロキシとして機能するJavaScript[promises](https://developer.mozilla.org/ja-JP/docs/Web/JavaScript/Reference/Global_Objects/Promise) を使用します。 値がわかると、プロミスはその値で「解決」されます。

## Web SDK タグ拡張機能を使用してコマンド応答を処理します

**[!UICONTROL イベント完了を送信]** イベントをルールの一部として登録するルールを作成します。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL ルール]** に移動し、目的のルールを選択します。
1. [!UICONTROL  イベント ] で、既存のイベントを選択するか、イベントを作成します。
1. [!UICONTROL  拡張機能 ] ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、[!UICONTROL  イベントタイプ ] を **[!UICONTROL イベント完了を送信]** に設定します。
1. 「**[!UICONTROL 変更を保持]**」をクリックして、公開ワークフローを実行します。

その後、アクション **[!UICONTROL 提案を適用]** または **[!UICONTROL 応答を適用]** をこのルールに含めることができます。

1. 上記で作成または編集したルールを表示する際に、既存のアクションを選択するか、アクションを作成します。
1. [!UICONTROL  拡張機能 ] ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、目的の動作に応じて [!UICONTROL  アクションタイプ ] を **[!UICONTROL 提案を適用]** または **[!UICONTROL 応答を適用]** に設定します。
1. アクションの目的のフィールドを設定し、「**[!UICONTROL 変更を保持]**」をクリックします。

## Web SDK JavaScript ライブラリを使用してコマンド応答を処理します

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

コマンドから返されるすべてのプロミスは、`result` オブジェクトを使用します。 例えば、[`getLibraryInfo`](getlibraryinfo.md) のコマンドを使用して、`result` オブジェクトからライブラリ情報を取得できます。

```js
alloy("getLibraryInfo")
  .then(function(result) {
    console.log(result.libraryInfo.version);
    console.log(result.libraryInfo.commands);
    console.log(result.libraryInfo.configs);
  });
```

この `result` オブジェクトの内容は、使用するコマンドとユーザーの同意の組み合わせによって異なります。 ユーザーが特定の目的で同意していない場合、応答オブジェクトには、ユーザーが同意した内容のコンテキストで提供できる情報のみが含まれます。
