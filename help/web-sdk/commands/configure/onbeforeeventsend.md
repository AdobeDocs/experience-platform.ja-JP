---
title: onBeforeEventSend
description: データの送信直前に実行されるコールバック。
source-git-commit: b6e084d2beed58339191b53d0f97b93943154f7c
workflow-type: tm+mt
source-wordcount: '447'
ht-degree: 0%

---

# `onBeforeEventSend`

The `onBeforeEventSend` callback を使用すると、JavaScript 関数を登録できます。この関数は、データがAdobeに送信される直前に送信するデータを変更できます。 このコールバックを使用すると、 `xdm` または `data` オブジェクトに含まれます。要素の追加、編集または削除を行う機能も含まれます。 また、検出されたクライアント側ボットトラフィックなど、データの送信を完全に条件付きでキャンセルすることもできます。

>[!WARNING]
>
>このコールバックを使用すると、カスタムコードを使用できます。 コールバックに含めたコードがキャッチできない例外をスローした場合、イベントの処理が停止します。 データはAdobeに送信されません。

## イベントの前に、 Web SDK タグ拡張を使用してコールバックを送信します。

を選択します。 **[!UICONTROL イベント送信コールバックコードの前にを指定します。]** ボタンを押す [タグ拡張の設定](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md). このボタンをクリックするとモーダルウィンドウが開き、目的のコードを挿入できます。

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、次に **[!UICONTROL 設定]** の [!UICONTROL Adobe Experience Platform Web SDK] カード。
1. 下にスクロールして、 [!UICONTROL データ収集] 「 」セクションで、「 」ボタン **[!UICONTROL イベント送信コールバックコードの前にを指定します。]**.
1. このボタンをクリックすると、コードエディターでモーダルウィンドウが開きます。 目的のコードを挿入し、「 **[!UICONTROL 保存]** をクリックしてモーダルウィンドウを閉じます。
1. クリック **[!UICONTROL 保存]** 「拡張機能の設定」で、変更を公開します。

コードエディター内で、 `content` オブジェクト。 このオブジェクトには、Adobeに送信されるペイロードが含まれます。 この `content` オブジェクトを指定するか、関数内で任意のコードを囲みます。 以外で定義された変数 `content` を使用できますが、Adobeに送信されるペイロードには含まれません。

>[!TIP]
>
>オブジェクト `content.xdm` および `content.data` は常にこのコンテキストで定義されるので、存在するかどうかを確認する必要はありません。 これらのオブジェクト内の一部の変数は、実装とデータレイヤーによって異なります。 Adobeでは、JavaScript エラーを防ぐために、これらのオブジェクト内の未定義の値を確認することをお勧めします。

例えば、次のような場合に、

* XDM 要素の追加 `xdm.commerce.order.purchaseID`
* 内のすべての文字を強制 `xdm.marketing.trackingCode` 小文字
* `xdm.environment.operatingSystemVersion` を削除します
* イベントタイプがリンククリックの場合、下のコードに関係なく、すぐにデータを送信します
* ボットが検出された場合にAdobeへのデータ送信をキャンセルする

モーダルウィンドウ内の同等のコードは次のとおりです。

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

>[!NOTE]
>
>戻らない `false` をクリックします。 リピート `false` 最初のイベントが発生すると、パーソナライゼーションに悪影響が及ぶ場合があります。

## イベントの前に、Web SDK JavaScript ライブラリを使用してコールバックを送信します

を登録します。 `onBeforeEventSend` 実行時のコールバック `configure` コマンドを使用します。 次の項目を変更できます。 `content` 変数名を任意の値に変更します。

```js
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "onBeforeEventSend": function(content) {
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
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "onBeforeEventSend": lastChanceLogic
});    
```
