---
title: コマンド応答の処理
description: JavaScript promise を使用して、コマンドからの応答を処理します。
exl-id: dda98b3e-3e37-48ac-afd7-d8852b785b83
source-git-commit: f75dcfc945be2f45c1638bdd4d670288aef6e1e6
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 0%

---

# コマンド応答の処理

一部の Web SDK コマンドは、組織で役立つ可能性のあるデータを含むオブジェクトを返すことができます。 必要に応じて、そのデータの処理を選択できます。 コマンド応答は、Edge ネットワークデータを効果的に機能させる必要があるので、提案や宛先にとって役立ちます。

コマンド応答で JavaScript を使用 [promise](https://developer.mozilla.org/ja-JP/docs/Web/JavaScript/Reference/Global_Objects/Promise):promise の作成時に不明な値のプロキシとして機能します。 値が判明すると、その値を使用してプロミスが「解決」されます。

## Web SDK タグ拡張機能を使用してコマンド応答を処理する

を購読するルールを作成します。 **[!UICONTROL イベント送信完了]** イベントをルールの一部として含めることができます。

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL ルール]**&#x200B;を選択し、目的のルールを選択します。
1. の下 [!UICONTROL イベント]、既存のイベントを選択するか、イベントを作成します。
1. を設定します。 [!UICONTROL 拡張] ドロップダウンフィールド **[!UICONTROL Adobe Experience Platform Web SDK]**&#x200B;をクリックし、 [!UICONTROL イベントタイプ] から **[!UICONTROL イベント送信完了]**.
1. クリック **[!UICONTROL 変更を保持]**&#x200B;を開き、パブリッシュワークフローを実行します。

その後、次のアクションを含めることができます **[!UICONTROL 提案を適用]** または **[!UICONTROL 応答を適用]** をこのルールに追加します。

1. 上記の作成または編集されたルールを表示した場合、既存のアクションを選択するか、アクションを作成します。
1. を設定します。 [!UICONTROL 拡張] ドロップダウンフィールド **[!UICONTROL Adobe Experience Platform Web SDK]**&#x200B;をクリックし、 [!UICONTROL アクションタイプ] から **[!UICONTROL 提案を適用]** または **[!UICONTROL 応答を適用]**（目的の動作に応じて）
1. アクションの目的のフィールドを設定し、「 **[!UICONTROL 変更を保持]**.

## Web SDK JavaScript ライブラリを使用したコマンド応答の処理

以下を使用します。 `then` および `catch` コマンドの成功または失敗を判断するためのメソッド。 省略できます `then` または `catch` その目的が実装にとって重要でない場合に、を選択します。

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

コマンドから返されるすべての promise では、 `result` オブジェクト。 例えば、ライブラリ情報は、 `result` オブジェクトを [`getLibraryInfo`](getlibraryinfo.md) コマンド：

```js
alloy("getLibraryInfo")
  .then(function(result) {
    console.log(result.libraryInfo.version);
    console.log(result.libraryInfo.commands);
    console.log(result.libraryInfo.configs);
  });
```

この `result` オブジェクトは、使用するコマンドとユーザーの同意の組み合わせによって異なります。 ユーザーが特定の目的に対して同意しなかった場合、応答オブジェクトには、ユーザーが同意した内容に関して提供できる情報のみが含まれます。
