---
title: prehidingStyle
description: パーソナライズされたコンテンツをちらつきなく読み込むことができる CSS 定義を作成します。
exl-id: 3693542a-69d3-4ad8-bea4-4cabf7d80563
source-git-commit: 8fc0fd96f13f0642f7671d0e0f4ecfae8ab6761f
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 0%

---

# `prehidingStyle`

`prehidingStyle` プロパティを使用すると、CSS セレクターを定義して、パーソナライズされたコンテンツを読み込むまで非表示にすることができます。 このプロパティは、ちらつきを避けるために、同期 Web SDK 実装で役立ちます。 Adobeでは、非同期 Web SDK 実装には [&#x200B; 事前非表示スニペット &#x200B;](../../personalization/manage-flicker.md) を使用することをお勧めします。

ページ上で最初の [`sendEvent`](../sendevent/overview.md) コマンドを実行すると、このプロパティで定義する CSS セレクターによってコンテンツの非表示が開始されます。 Adobe（通常、パーソナライズされたコンテンツを含む）からの応答を受信すると、コンテンツが再表示されます。 `sendEvent` コマンドが失敗またはタイムアウトした場合も、コンテンツは再表示されます。

`prehidingStyle` と事前非表示スニペットの両方を実装に含める場合は、事前非表示スニペットがこの設定プロパティよりも優先されます。

## Web SDK タグ拡張機能を使用したスタイルの事前非表示

[&#x200B; タグ拡張機能の設定 &#x200B;](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md) 時に **[!UICONTROL 事前非表示スタイルを設定]** ボタンを選択します。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL 拡張機能]** に移動し、[!UICONTROL Adobe Experience Platform Web SDK **[!UICONTROL カードの]** 設定 &#x200B;] をクリックします。
1. 「[!UICONTROL Personalization]」セクションまでスクロールダウンし、「**[!UICONTROL 事前非表示スタイルを設定]** ボタンを選択します。
1. このボタンをクリックすると、CSS エディターを含むモーダルウィンドウが開きます。 目的の CSS セレクターと宣言ブロックを挿入し、「**[!UICONTROL 保存]**」をクリックしてモーダルウィンドウを閉じます。
1. 拡張機能設定の **[!UICONTROL 保存]** をクリックして、変更を公開します。

## Web SDK JavaScript ライブラリを使用したスタイルの事前非表示

`configure` コマンドを実行するときは、`prehidingStyle` の文字列を設定します。 Web SDK の設定時にこのプロパティを省略した場合、ページで最初の `sendEvent` コマンドを実行したときに非表示になっているものはありません。 この値を、同期して読み込まれるライブラリに必要な CSS セレクターおよび宣言ブロックに設定します。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  prehidingStyle: "#container { opacity: 0 !important }"
});
```
