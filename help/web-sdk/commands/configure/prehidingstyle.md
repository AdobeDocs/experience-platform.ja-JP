---
title: prehidingStyle
description: ちらつくことなく、パーソナライズされたコンテンツを読み込むことができる CSS 定義を作成します。
source-git-commit: b6e084d2beed58339191b53d0f97b93943154f7c
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 0%

---

# `prehidingStyle`

The `prehidingStyle` プロパティを使用すると、読み込まれるまでパーソナライズされたコンテンツを非表示にする CSS セレクターを定義できます。 このプロパティは、同期 Web SDK 実装でちらつきを回避するために役立ちます。 Adobeは、 [スニペットを非表示にする](../../personalization/manage-flicker.md) 非同期 Web SDK 実装用。

このプロパティで定義する CSS セレクターは、最初の [`sendEvent`](../sendevent/overview.md) 」コマンドを使用して、ページ上で設定できます。 通常、パーソナライズされたコンテンツを含む、Adobeからの応答を受け取ると、コンテンツは非表示になります。 コンテンツも非表示にしない場合は、 `sendEvent` コマンドが失敗するか、タイムアウトします。

次の両方を含める場合、 `prehidingStyle` 実装内の事前非表示スニペットも、この設定プロパティよりも事前非表示スニペットの方が優先されます。

## Web SDK タグ拡張機能を使用したスタイルの事前非表示

を選択します。 **[!UICONTROL 事前非表示のスタイルを提供]** ボタンを押す [タグ拡張の設定](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md).

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、次に **[!UICONTROL 設定]** の [!UICONTROL Adobe Experience Platform Web SDK] カード。
1. 下にスクロールして、 [!UICONTROL パーソナライズ] 「 」セクションで、「 」ボタン **[!UICONTROL 事前非表示のスタイルを提供]**.
1. このボタンをクリックすると、CSS エディターと共にモーダルウィンドウが開きます。 目的の CSS セレクターと宣言ブロックを挿入し、 **[!UICONTROL 保存]** をクリックしてモーダルウィンドウを閉じます。
1. クリック **[!UICONTROL 保存]** 「拡張機能の設定」で、変更を公開します。

## Web SDK JavaScript ライブラリを使用したスタイルの事前非表示

を設定します。 `prehidingStyle` 文字列（実行時） `configure` コマンドを使用します。 Web SDK を設定する際にこのプロパティを省略した場合、最初の `sendEvent` 」コマンドを使用して、ページ上で設定できます。 同期的に読み込まれたライブラリに対して、この値を目的の CSS セレクターと宣言ブロックに設定します。

```js
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "prehidingStyle": "#container { opacity: 0 !important }"
});
```
