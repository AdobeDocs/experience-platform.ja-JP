---
title: documentUnloading
description: JavaScript sendBeacon API を使用して、Adobeにデータを送信します。
exl-id: 7683c0c4-ae2e-46ec-8471-628a10e17afc
source-git-commit: a229cec4a53ab85d13590205a008612719019ebd
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# `documentUnloading`

`documentUnloading` プロパティを使用すると、JavaScript [`sendBeacon`](https://developer.mozilla.org/ja-JP/docs/Web/API/Navigator/sendBeacon) メソッドを使用してAdobeにデータを送信できます。 一般的なリクエストの所要時間が長すぎる場合、ブラウザーはリクエストをキャンセルできます。 ページから移動した後にリクエストがバックグラウンドで実行されるように、`sendBeacon` を使用するように Web SDKに指示できます。 このプロパティを有効にすると、アンロード時にブラウザーによってデータリクエストがキャンセルされるのを防ぐことができます。

複数のブラウザーでは、`sendBeacon` と一度に送信できるデータ量に 64 KB の制限が課されています。 ペイロードが大きすぎるためにブラウザーがイベントを拒否した場合、web SDKはフォールバックして通常のトランスポート方式を使用します。 特定のブラウザーで許可されているペイロードのサイズが大きい場合でも、Adobe データ収集サーバーではペイロードが 64 KB に切り捨てられます。

`documentUnloading` コマンドを実行するときは、`sendEvent` のブール値を設定します。 デフォルト値は `false` です。 `true` メソッドを使用してデータをAdobeに送信する場合は、このプロパティを `sendBeacon` に設定します。

>[!IMPORTANT]
>
>`documentUnloading` プロパティは [`renderDecisions`](renderdecisions.md) プロパティと互換性がありません。 両方のプロパティを同時に `true` に設定しないでください。

```js
alloy("sendEvent", {
  "xdm": adobeDataLayer.getState(reference),
  "documentUnloading": true
});
```

## Web SDK タグ拡張機能を使用したドキュメントのアンロード

Web SDK タグ拡張機能を使用する場合、**[!UICONTROL Document will unload]** アクションを設定すると「[**[!UICONTROL Send event]**](/help/tags/extensions/client/web-sdk/actions/send-event.md#data-fields)」チェックボックスが使用可能になります。
