---
title: onBeforeLinkClickSend
description: リンクトラッキングデータが送信される直前に実行されるコールバック。
exl-id: 8c73cb25-2648-4cf7-b160-3d06aecde9b4
source-git-commit: 660d4e72bd93ca65001092520539a249eae23bfc
workflow-type: tm+mt
source-wordcount: '463'
ht-degree: 0%

---


# `onBeforeLinkClickSend`

>[!IMPORTANT]
>
>このコールバックは非推奨（廃止予定）です。 使用方法 [`filterClickDetails`](clickcollection.md) その代わり。

この `onBeforeLinkClickSend` callback を使用すると、データがAdobeに送信される直前に送信したリンクトラッキングデータを変更できるJavaScript関数を登録できます。 このコールバックを使用すると、 `xdm` または `data` オブジェクト （要素を追加、編集、削除する機能を含む）。 クライアントサイドのボットトラフィックが検出された場合など、データの送信を完全に条件付きでキャンセルすることもできます。 Web SDK 2.15.0 以降でサポートされています。

このコールバックは、次の場合にのみ実行されます [`clickCollectionEnabled`](clickcollectionenabled.md) が有効になっており、 [`filterClickDetails`](clickcollection.md) 登録済みの関数が含まれていません。 次の場合 `clickCollectionEnabled` が無効になっている、または `filterClickDetails` 登録済みの関数が含まれると、このコールバックは実行されません。 次の場合 `onBeforeEventSend` および `onBeforeLinkClickSend` どちらも登録済みの関数を含んでいます。 `onBeforeLinkClickSend` が最初に実行されます。

>[!WARNING]
>
>このコールバックを使用すると、カスタムコードを使用できます。 コールバックに含めるコードがキャッチされない例外をスローした場合、イベントの処理が停止します。 データはAdobeに送信されません。

## Web SDK タグ拡張機能を使用して、「リンクの前に設定」クリックでコールバックを送信 {#tag-extension}

「」を選択します **[!UICONTROL リンククリックイベント送信時のコールバックコードを提供]** ボタン条件 [タグ拡張機能の設定](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md). このボタンをクリックすると、目的のコードを挿入できるモーダルウィンドウが開きます。

1. へのログイン [experience.adobe.com](https://experience.adobe.com) Adobe IDの資格情報を使用します。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、 **[!UICONTROL 設定]** 日 [!UICONTROL Adobe Experience Platform Web SDK] カード。
1. にスクロール ダウンします。 [!UICONTROL データ収集] 「」セクションで、「」チェックボックスを選択します **[!UICONTROL クリックデータ収集の有効化]**.
1. というラベルの付いたボタンを選択します **[!UICONTROL リンククリックイベント送信時のコールバックコードを提供]**.
1. このボタンをクリックすると、コードエディターでモーダルウィンドウが開きます。 目的のコードを挿入し、をクリックします **[!UICONTROL 保存]** をクリックして、モーダルウィンドウを閉じます。
1. クリック **[!UICONTROL 保存]** 「拡張機能設定」で、変更を公開します。

コードエディター内で、次の変数にアクセスできます。

* **`content.clickedElement`**：クリックされた DOM 要素。
* **`content.xdm`**：イベントの XDM ペイロード。
* **`content.data`**：イベントのデータオブジェクトペイロード。
* **`return true`**：現在の変数値でコールバックを直ちに終了します。 この `onBeforeEventSend` コールバックは、登録済みの関数が含まれている場合に実行されます。
* **`return false`**：直ちにコールバックを終了し、Adobeへのデータの送信を中止します。 この `onBeforeEventSend` コールバックは実行されません。

の外部で定義された変数 `content` を使用できますが、Adobeに送信されるペイロードには含まれません。

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

次と同様 [`onBeforeEventSend`](onbeforeeventsend.md)の場合は、次のことができます `return true` 関数をすぐに完了する場合、または `return false` を指定して、Adobeへのデータの送信を中止します。 でのデータの送信を中止した場合 `onBeforeLinkClickSend` 両方の場合 `onBeforeEventSend` および `onBeforeLinkClickSend` 登録済みの関数を含む `onBeforeEventSend` 関数が実行されません。

## Web SDK JavaScript ライブラリを使用して、「リンクの前にコールバックを送信」をクリックして設定します。 {#library}

を登録 `onBeforeLinkClickSend` を実行するときのコールバック `configure` コマンド。 を変更できます `content` インライン関数内のパラメーター変数を変更して、必要な任意の値に変数名を変更します。

```js
alloy("configure", {
  edgeConfigId: "ebebf826-a01f-4458-8cec-ef61de241c93",
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
  edgeConfigId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  onBeforeLinkClickSend: lastChanceLinkLogic
});    
```
