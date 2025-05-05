---
title: onBeforeLinkClickSend
description: リンクトラッキングデータが送信される直前に実行されるコールバック。
exl-id: 8c73cb25-2648-4cf7-b160-3d06aecde9b4
source-git-commit: 8fc0fd96f13f0642f7671d0e0f4ecfae8ab6761f
workflow-type: tm+mt
source-wordcount: '463'
ht-degree: 0%

---


# `onBeforeLinkClickSend`

>[!IMPORTANT]
>
>このコールバックは非推奨（廃止予定）です。 代わりに [`filterClickDetails`](clickcollection.md) を使用します。

`onBeforeLinkClickSend` コールバックを使用すると、JavaScript関数を登録して、データがAdobeに送信される直前に送信したリンクトラッキングデータを変更できます。 このコールバックを使用すると、要素の追加、編集、削除の機能を含め、`xdm` オブジェクトまたは `data` オブジェクトを操作できます。 クライアントサイドのボットトラフィックが検出された場合など、データの送信を完全に条件付きでキャンセルすることもできます。 Web SDK 2.15.0 以降でサポートされています。

このコールバックは、[`clickCollectionEnabled`](clickcollectionenabled.md) が有効で、登録済みの関数が含まれ [`filterClickDetails`](clickcollection.md) いない場合にのみ実行されます。 `clickCollectionEnabled` が無効になっている場合、または `filterClickDetails` に登録済みの関数が含まれている場合、このコールバックは実行されません。 `onBeforeEventSend` と `onBeforeLinkClickSend` の両方に登録済みの関数が含まれている場合、`onBeforeLinkClickSend` が最初に実行されます。

>[!WARNING]
>
>このコールバックを使用すると、カスタムコードを使用できます。 コールバックに含めるコードがキャッチされない例外をスローした場合、イベントの処理が停止します。 データはAdobeに送信されません。

## Web SDK タグ拡張機能を使用して、「リンクの前に設定」クリックでコールバックを送信 {#tag-extension}

[ タグ拡張機能の設定 ](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md) 時に、「**[!UICONTROL リンククリック前に提供イベント コールバックコードを送信]**」ボタンを選択します。 このボタンをクリックすると、目的のコードを挿入できるモーダルウィンドウが開きます。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL 拡張機能]** に移動し、[!UICONTROL Adobe Experience Platform Web SDK **[!UICONTROL カードの]** 設定 &#x200B;] をクリックします。
1. 「[!UICONTROL &#x200B; データ収集 &#x200B;]」セクションまでスクロールし、「**[!UICONTROL クリックデータ収集を有効にする]**」チェックボックスを選択します。
1. **[!UICONTROL リンククリックイベントの前に提供するコールバックコードの送信]** というラベルの付いたボタンを選択します。
1. このボタンをクリックすると、コードエディターでモーダルウィンドウが開きます。 目的のコードを挿入し、「**[!UICONTROL 保存]**」をクリックしてモーダルウィンドウを閉じます。
1. 拡張機能設定の **[!UICONTROL 保存]** をクリックして、変更を公開します。

コードエディター内で、次の変数にアクセスできます。

* **`content.clickedElement`**：クリックされた DOM 要素。
* **`content.xdm`**：イベントの XDM ペイロード。
* **`content.data`**：イベントのデータオブジェクトペイロード。
* **`return true`**：現在の変数値でコールバックを直ちに終了します。 `onBeforeEventSend` コールバックは、登録済みの関数が含まれている場合に実行されます。
* **`return false`**：直ちにコールバックを終了し、Adobeへのデータの送信を中止します。 `onBeforeEventSend` コールバックは実行されません。

`content` 外で定義された変数は使用できますが、Adobeに送信されるペイロードには含まれません。

```js
// Set an already existing value to something else
content.xdm.web.webPageDetails.URL = "https://example.com/current.html";

// Use nullish coalescing assignments to create objects if they don't yet exist, preventing undefined errors. 
// Can be omitted if you are certain that the object is already defined
content.xdm._experience ??= {};
content.xdm._experience.analytics ??= {};
content.xdm._experience.analytics.customDimensions ??= {};
content.xdm._experience.analytics.customDimensions.eVars ??= {};

// Then set the property to the clicked element
content.xdm._experience.analytics.customDimensions.eVars.eVar1 = content.clickedElement;

// Use optional chaining to check if each object is defined, preventing undefined errors
if(content.xdm.web?.webInteraction?.type === "other") content.xdm.web.webInteraction.type = "download";
```

[`onBeforeEventSend`](onbeforeeventsend.md) と同様に、関数を直ちに終了する `return true`、Adobeへのデータの送信を中止する `return false` を指定できます。 `onBeforeEventSend` と `onBeforeLinkClickSend` の両方に登録済みの関数が含まれている場合に `onBeforeLinkClickSend` でのデータの送信を中止すると、`onBeforeEventSend` の関数は実行されません。

## Web SDK JavaScript ライブラリを使用して、「リンクの前にコールバックを送信」をクリックして設定します。 {#library}

`configure` コマンドの実行時に `onBeforeLinkClickSend` コールバックを登録します。 インライン関数内のパラメーター変数を変更することで、`content` 変数の名前を任意の値に変更できます。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  onBeforeLinkClickSend: function(content) {
    // Add, modify, or delete values
    content.xdm.web.webPageDetails.URL = "https://example.com/current.html";
    
    // Return true to complete the function immediately
    if (sendImmediate == true) {
      return true;
    }
    
    // Return false to cancel sending data immediately
    if(myBotDetector.isABot()){
      return false;
    }
  }
});
```

インライン関数の代わりに独自の関数を登録することもできます。

```js
function lastChanceLinkLogic(content) {
  content.xdm.application ??= {};
  content.xdm.application.name = "App name";
}

alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  onBeforeLinkClickSend: lastChanceLinkLogic
});    
```
