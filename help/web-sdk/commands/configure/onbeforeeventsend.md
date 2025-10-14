---
title: onBeforeEventSend
description: Web SDK を設定して、データがAdobeに送信される直前に送信したデータを変更できるJavaScript関数を登録する方法を説明します。
exl-id: 945f4fa1-380c-46aa-a92a-bbcfd6644751
source-git-commit: d3be2a9e75514023a7732a1c3460f8695ef02e68
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---

# `onBeforeEventSend`

`onBeforeEventSend` コールバックを使用すると、データがAdobeに送信される直前に送信したデータを変更できるJavaScript関数を登録できます。 このコールバックを使用すると、要素の追加、編集、削除の機能を含め、`xdm` オブジェクトまたは `data` オブジェクトを操作できます。 クライアントサイドのボットトラフィックが検出された場合など、データの送信を完全に条件付きでキャンセルすることもできます。

>[!WARNING]
>
>このコールバックを使用すると、カスタムコードを使用できます。 コールバックに含めるコードがキャッチされない例外をスローした場合、イベントの処理が停止します。 データはAdobeに送信されません。

## Web SDK タグ拡張機能を使用して、イベント送信コールバックの前にを設定します {#tag-extension}

[&#x200B; タグ拡張機能の設定 &#x200B;](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md) 時に、「**[!UICONTROL イベントの前に提供するコールバックコード]**」ボタンを選択します。 このボタンをクリックすると、目的のコードを挿入できるモーダルウィンドウが開きます。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL 拡張機能]** に移動し、[!UICONTROL Adobe Experience Platform Web SDK **[!UICONTROL カードの]** 設定 &#x200B;] をクリックします。
1. 「[!UICONTROL &#x200B; データ収集 &#x200B;]」セクションまでスクロールし、「**[!UICONTROL イベント送信前に提供コールバックコード]**」ボタンを選択します。
1. このボタンをクリックすると、コードエディターでモーダルウィンドウが開きます。 目的のコードを挿入し、「**[!UICONTROL 保存]**」をクリックしてモーダルウィンドウを閉じます。
1. 拡張機能設定の **[!UICONTROL 保存]** をクリックして、変更を公開します。

コードエディター内で、次の変数にアクセスできます。

* **`content.xdm`**：イベントの [XDM](../sendevent/xdm.md) ペイロード。
* **`content.data`**：イベントの [data](../sendevent/data.md) オブジェクトペイロード。
* **`return true`**：直ちにコールバックを終了し、`content` オブジェクト内の現在の値を使用してデータをAdobeに送信します。
* **`return false`**：直ちにコールバックを終了し、Adobeへのデータの送信を中止します。

`content` 外で定義された変数は使用できますが、Adobeに送信されるペイロードには含まれません。

```js
// Use nullish coalescing assignments to add objects if they don't yet exist
content.xdm.commerce ??= {};
content.xdm.commerce.order ??= {};

// Then add the purchase ID
content.xdm.commerce.order.purchaseID = "12345";

// Use optional chaining to prevent undefined errors when setting tracking code to lower case
if(content.xdm.marketing?.trackingCode) content.xdm.marketing.trackingCode = content.xdm.marketing.trackingCode.toLowerCase();

// Delete operating system version
if(content.xdm.environment) delete content.xdm.environment.operatingSystemVersion;

// Immediately end onBeforeEventSend logic and send the data to Adobe for this event type
if (content.xdm.eventType === "web.webInteraction.linkClicks") {
  return true;
}

// Cancel sending data if it is a known bot
if (myBotDetector.isABot()) {
  return false;
}
```

>[!TIP]
>ページの最初のイベントで `false` を返さないようにします。 最初のイベントで `false` を返すと、パーソナライゼーションに悪影響を与える可能性があります。

## Web SDK JavaScript ライブラリを使用して、イベント送信コールバックの前にを設定します {#library}

`configure` コマンドの実行時に `onBeforeEventSend` コールバックを登録します。 インライン関数内のパラメーター変数を変更することで、`content` 変数の名前を任意の値に変更できます。

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
