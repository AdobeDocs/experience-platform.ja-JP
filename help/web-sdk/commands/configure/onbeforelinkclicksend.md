---
title: onBeforeLinkClickSend
description: リンクトラッキングデータの送信直前に実行されるコールバック。
source-git-commit: b6e084d2beed58339191b53d0f97b93943154f7c
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 0%

---

# `onBeforeLinkClickSend`

The `onBeforeLinkClickSend` callback を使用すると、JavaScript 関数を登録できます。この関数は、データがAdobeに送信される直前に送信するリンクトラッキングデータを変更できます。 このコールバックを使用すると、 `xdm` または `data` オブジェクトに含まれます。要素の追加、編集または削除を行う機能も含まれます。 また、検出されたクライアント側ボットトラフィックなど、データの送信を完全に条件付きでキャンセルすることもできます。 Web SDK 2.15.0以降でサポートされています。

このコールバックは、 [`clickCollectionEnabled`](clickcollectionenabled.md) が有効になっている。 次の場合 `clickCollectionEnabled` が無効の場合、このコールバックは実行されません。 両方の `onBeforeEventSend` および `onBeforeLinkClickSend` には、登録された関数が含まれます。 `onBeforeLinkClickSend` 関数が最初に実行されます。 1 回 `onBeforeLinkClickSend` 関数が終了すると、 `onBeforeEventSend` 関数を実行します。

>[!WARNING]
>
>このコールバックを使用すると、カスタムコードを使用できます。 コールバックに含めたコードがキャッチできない例外をスローした場合、イベントの処理が停止します。 データはAdobeに送信されません。

## リンク前にオンにし、 Web SDK タグ拡張を使用してコールバックを送信をクリックします。

を選択します。 **[!UICONTROL リンククリックイベント送信コールバックコードの前にを指定します。]** ボタンを押す [タグ拡張の設定](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md). このボタンをクリックするとモーダルウィンドウが開き、目的のコードを挿入できます。

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、次に **[!UICONTROL 設定]** の [!UICONTROL Adobe Experience Platform Web SDK] カード。
1. 下にスクロールして、 [!UICONTROL データ収集] 「 」セクションで、「 」チェックボックスをオンにします。 **[!UICONTROL クリックデータの収集を有効にする]**.
1. ラベルの付いたボタンを選択 **[!UICONTROL リンククリックイベント送信コールバックコードの前にを指定します。]**.
1. このボタンをクリックすると、コードエディターでモーダルウィンドウが開きます。 目的のコードを挿入し、「 **[!UICONTROL 保存]** をクリックしてモーダルウィンドウを閉じます。
1. クリック **[!UICONTROL 保存]** 「拡張機能の設定」で、変更を公開します。

コードエディター内で、 `content` オブジェクト。 このオブジェクトには、Adobeに送信されるペイロードが含まれます。 この `content` オブジェクトを指定するか、関数内で任意のコードを囲みます。 以外で定義された変数 `content` を使用できますが、Adobeに送信されるペイロードには含まれません。

>[!TIP]
>
>オブジェクト `content.xdm`, `content.data`、および `content.clickedElement` は常にこのコンテキストで定義されるので、存在するかどうかを確認する必要はありません。 これらのオブジェクト内の一部の変数は、実装とデータレイヤーによって異なります。 Adobeでは、JavaScript エラーを防ぐために、これらのオブジェクト内の未定義の値を確認することをお勧めします。

例えば、次のアクションを実行するとします。

* 現在のページ URL を変更
* Adobe AnalyticseVarのクリックされた要素を取り込む
* リンクタイプを「その他」から「ダウンロード」に変更します。

モーダルウィンドウ内の同等のコードは次のとおりです。

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

同様に [`onBeforeEventSend`](onbeforeeventsend.md)を使用する場合、 `return true` 関数をすぐに完了する、または `return false` ：データの送信を即座にキャンセルします。 データの送信をキャンセルする場合は、 `onBeforeLinkClickSend` 両方の場合 `onBeforeEventSend` および `onBeforeLinkClickSend` には、登録された関数が含まれます。 `onBeforeEventSend` 関数が実行されない。

## リンク前にオンにし、「 Web SDK JavaScript ライブラリを使用したコールバックの送信」をクリックします。

を登録します。 `onBeforeLinkClickSend` 実行時のコールバック `configure` コマンドを使用します。 次の項目を変更できます。 `content` 変数名を任意の値に変更します。

```js
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "onBeforeLinkClickSend": function(content) {
    // Add, modify, or delete values
    content.xdm.web.webPageDetails.URL = "https://example.com/current.html";
    
    // Return true to immediately complete the function
    if (sendImmediate == true) {
      return true;
    }
    
    // Return false to immediately cancel sending data
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
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "onBeforeLinkClickSend": lastChanceLinkLogic
});    
```
