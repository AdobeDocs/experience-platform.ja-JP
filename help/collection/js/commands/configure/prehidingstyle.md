---
title: prehidingStyle
description: パーソナライズされたコンテンツをちらつきなく読み込むことができる CSS 定義を作成します。
exl-id: 3693542a-69d3-4ad8-bea4-4cabf7d80563
source-git-commit: bb90bbddf33bc4b0557026a0f34965ac37475c65
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 0%

---

# `prehidingStyle`

`prehidingStyle` プロパティを使用すると、CSS セレクターを定義して、パーソナライズされたコンテンツを読み込むまで非表示にすることができます。 このプロパティは、ちらつきを避けるために、同期 web SDK実装で役立ちます。 Adobeでは、非同期 web SDK実装に [&#x200B; 事前非表示スニペット &#x200B;](/help/collection/use-cases/personalization/manage-flicker.md) を使用することをお勧めします。

ページ上で最初の [`sendEvent`](../sendevent/overview.md) コマンドを実行すると、このプロパティで定義する CSS セレクターによってコンテンツの非表示が開始されます。 Adobeからの応答を受信すると、コンテンツが再表示されます。これには、通常、パーソナライズされたコンテンツが含まれます。 `sendEvent` コマンドが失敗またはタイムアウトした場合も、コンテンツは再表示されます。

`prehidingStyle` と事前非表示スニペットの両方を実装に含める場合は、事前非表示スニペットがこの設定プロパティよりも優先されます。

`prehidingStyle` コマンドを実行するときは、`configure` の文字列を設定します。 Web SDKの設定時にこのプロパティを省略すると、ページ上で最初の `sendEvent` コマンドを実行したときに隠しコマンドは何も表示されません。 この値を、同期して読み込まれるライブラリに必要な CSS セレクターおよび宣言ブロックに設定します。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  prehidingStyle: "#container { opacity: 0 !important }"
});
```

## Web SDK タグ拡張機能を使用したスタイルの事前非表示

これらの設定は、[Personalization configuration settings](/help/tags/extensions/client/web-sdk/configure/personalization.md) を使用して web SDK タグ拡張機能で設定できます。
