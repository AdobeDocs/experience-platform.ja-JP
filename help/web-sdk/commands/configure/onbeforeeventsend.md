---
title: onBeforeEventSend
description: Web SDK を設定して、データがAdobeに送信される直前に送信したデータを変更できるJavaScript関数を登録する方法を説明します。
source-git-commit: 660d4e72bd93ca65001092520539a249eae23bfc
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---


# `onBeforeEventSend`

この `onBeforeEventSend` callback を使用すると、データがAdobeに送信される直前に送信したデータを変更できるJavaScript関数を登録できます。 このコールバックを使用すると、 `xdm` または `data` オブジェクト （要素を追加、編集、削除する機能を含む）。 クライアントサイドのボットトラフィックが検出された場合など、データの送信を完全に条件付きでキャンセルすることもできます。

>[!WARNING]
>
>このコールバックを使用すると、カスタムコードを使用できます。 コールバックに含めるコードがキャッチされない例外をスローした場合、イベントの処理が停止します。 データはAdobeに送信されません。

## Web SDK タグ拡張機能を使用して、イベント送信コールバックの前にを設定します {#tag-extension}

「」を選択します **[!UICONTROL イベント送信前にコールバックコードを提供]** ボタン条件 [タグ拡張機能の設定](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md). このボタンをクリックすると、目的のコードを挿入できるモーダルウィンドウが開きます。

1. へのログイン [experience.adobe.com](https://experience.adobe.com) Adobe IDの資格情報を使用します。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、 **[!UICONTROL 設定]** 日 [!UICONTROL Adobe Experience Platform Web SDK] カード。
1. にスクロール ダウンします。 [!UICONTROL データ収集] 「」セクションで、「」ボタンを選択します **[!UICONTROL イベント送信前にコールバックコードを提供]**.
1. このボタンをクリックすると、コードエディターでモーダルウィンドウが開きます。 目的のコードを挿入し、をクリックします **[!UICONTROL 保存]** をクリックして、モーダルウィンドウを閉じます。
1. クリック **[!UICONTROL 保存]** 「拡張機能設定」で、変更を公開します。

コードエディター内で、次の変数にアクセスできます。

* **`content.xdm`**：です [XDM](../sendevent/xdm.md) イベントのペイロード。
* **`content.data`**：です [データ](../sendevent/data.md) イベントのオブジェクトペイロード。
* **`return true`**：すぐにコールバックを終了し、現在の値を使用してデータをAdobeに送信します。 `content` オブジェクト。
* **`return false`**：直ちにコールバックを終了し、Adobeへのデータの送信を中止します。

の外部で定義された変数 `content` を使用できますが、Adobeに送信されるペイロードには含まれません。

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
>戻らない `false` ページの最初のイベントで。 復帰 `false` 最初のイベントでを実行すると、パーソナライゼーションに悪影響を及ぼす可能性があります。

## Web SDK JavaScript ライブラリを使用して、イベント送信コールバックの前にを設定します {#library}

を登録 `onBeforeEventSend` を実行するときのコールバック `configure` コマンド。 を変更できます `content` インライン関数内のパラメーター変数を変更して、必要な任意の値に変数名を変更します。

```js
alloy("configure", {
  edgeConfigId: "ebebf826-a01f-4458-8cec-ef61de241c93",
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
  edgeConfigId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  onBeforeEventSend: lastChanceLogic
});    
```
