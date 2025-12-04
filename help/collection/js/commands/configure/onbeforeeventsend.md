---
title: onBeforeEventSend
description: Web SDKを設定して、Adobeに送信する直前に送信したデータを変更できるJavaScript関数を登録する方法を説明します。
exl-id: 945f4fa1-380c-46aa-a92a-bbcfd6644751
source-git-commit: 2e39a7809049c199d4778a0e17eb9e0f3b1d9775
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 0%

---

# `onBeforeEventSend`

`onBeforeEventSend` コールバックを使用すると、JavaScript関数を登録して、そのデータがAdobeに送信される直前に送信したデータを変更できます。 このコールバックを使用すると、要素の追加、編集、削除の機能を含め、`xdm` オブジェクトまたは `data` オブジェクトを操作できます。 クライアントサイドのボットトラフィックが検出された場合など、データの送信を完全に条件付きでキャンセルすることもできます。

>[!WARNING]
>
>このコールバックを使用すると、カスタムコードを使用できます。 コールバックに含めるコードがキャッチされない例外をスローした場合、イベントの処理が停止し、**データはAdobeに送信されません**。

`onBeforeEventSend` コマンドの実行時に `configure` コールバックを登録します。 インライン関数内のパラメーター変数を変更することで、`content` 変数の名前を任意の値に変更できます。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  onBeforeEventSend: function(content) {
    // Use nullish coalescing assignments to add a new value
    content.xdm._experience ??= {};
    content.xdm._experience.analytics ??= {};
    content.xdm._experience.analytics.customDimensions ??= {};
    content.xdm._experience.analytics.customDimensions.eVars ??= {};
    content.xdm._experience.analytics.customDimensions.eVars.eVar1 = "Analytics custom value";
    
    // Use optional chaining to change an existing value
    if(content.xdm.web?.webPageDetails) content.xdm.web.webPageDetails.URL = content.xdm.web.webPageDetails.URL.toLowerCase();
    
    // Remove an existing value
    if(content.xdm.web?.webReferrer) delete content.xdm.web.webReferrer.URL;
    
    // Return true to immediately send data
    if (sendImmediate == true) {
      return true;
    }
    
    // Return false to immediately cancel sending data
    if(myBotDetector.isABot()){
      return false;
    }
    
    // Assign the value in the 'cid' query string to the tracking code XDM element
    content.xdm.marketing ??= {};
    content.xdm.marketing.trackingCode = new URLSearchParams(window.location.search).get('cid');
  }
});
```

インライン関数の代わりに独自の関数を登録することもできます。

```js
function lastChanceLogic(content) {
  content.xdm.application ??= {};
  content.xdm.application.name = "App name";
}

alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  onBeforeEventSend: lastChanceLogic
});    
```

## Web SDK タグ拡張機能を使用して、イベント送信前コールバックでを設定します

これらの設定は、web SDK タグ拡張機能で [Data collection configuration settings](/help/tags/extensions/client/web-sdk/configure/data-collection.md#on-before-event-send-callback) を使用して設定できます。
