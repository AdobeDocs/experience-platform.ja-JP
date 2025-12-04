---
title: eventType
description: sendEvent 呼び出しのイベントのタイプを設定します。
exl-id: 9d0fae3b-827a-4084-b460-b755e478e06a
source-git-commit: f988d7665a40b589ca281d439b6fca508f23cd03
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# `eventType`

`eventType` プロパティを使用すると、web SDKを使用して送信するイベントのタイプを定義できます。 このフィールドは最終的に「`xdm.eventType`」フィールドに入力されます。 Adobeに送信するイベントタイプを区別する際に役立ちます。

Adobeには、使用可能な事前定義済みのイベントタイプがいくつか用意されています。 事前定義済みの値の完全なリストについては、XDM ユーザーガイドの [`eventType`](/help/xdm/classes/experienceevent.md#accepted-values-for-eventtype) に使用できる値」を参照してください。 必要に応じて、独自の値を使用することもできます。

`type` オブジェクトで `xdm.eventType` と [`xdm`](xdm.md) の両方を設定した場合は、このフィールドの値が優先されます。

`eventType` コマンドを実行するときは、`sendEvent` string プロパティを設定します。

```js
alloy("sendEvent", {
  "xdm": adobeDataLayer.getState(reference),
  "type": "commerce.purchases"
});
```

## Web SDK タグ拡張機能を使用したイベントタイプ

このプロパティに相当する Web SDK タグ拡張機能は、&#39;[**[!UICONTROL Type]**](/help/tags/extensions/client/web-sdk/actions/send-event.md#data-fields)&#39; アクションを構成する場合の [!UICONTROL Send event] ドロップダウン メニューです。
