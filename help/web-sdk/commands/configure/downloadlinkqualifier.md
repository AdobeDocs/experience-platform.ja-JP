---
title: downloadLinkQualifier
description: 自動リンクトラッキングがダウンロードリンクを選定する方法を決定するのに役立ちます。
exl-id: ef4f0ed4-83fc-485f-866d-f9ca15447ac8
source-git-commit: 8be502c9eea67119dc537a5d63a6c71e0bff1697
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# `downloadLinkQualifier`

[`clickCollectionEnabled`](clickcollectionenabled.md) を使用して自動リンクトラッキングを有効にすると、`downloadLinkQualifier` プロパティが、ダウンロードリンクと見なされる URL の条件を決定するのに役立ちます。

このプロパティは正規表現文字列です。 クリックされた URL がこの正規表現に一致する場合、`xdm.web.webInteraction.type` は `"download"` に設定されます。 リンクに `download` しいHTML属性が含まれる場合、リンクは直ちにダウンロードリンクとして分類されます。 `clickCollectionEnabled` が有効になっていない場合、このプロパティは何もしません。

## Web SDK タグ拡張機能を使用したリンク修飾子のダウンロード

「**[!UICONTROL クリックデータ収集を有効にする]**」チェックボックスを有効にし、「タグ拡張機能の設定 [ 時に **[!UICONTROL リンク修飾子をダウンロード]** の下に目的のテキストを入力し ](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md) す。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL 拡張機能]** に移動し、[!UICONTROL Adobe Experience Platform Web SDK **[!UICONTROL カードの]** 設定 ] をクリックします。
1. 「[!UICONTROL  データ収集 ]」セクションまでスクロールし、「**[!UICONTROL クリックデータ収集を有効にする]**」チェックボックスを選択します。
1. 有効にすると、「**[!UICONTROL リンク修飾子をダウンロード]**」テキストボックスが表示されます。 目的の値を入力します。 正規表現をテストしてデフォルト値を復元するためのボタンも使用できます。
1. 「**[!UICONTROL 保存]**」をクリックして、変更を公開します。

## Web SDK JavaScript ライブラリを使用したリンク修飾子のダウンロード

`configure` コマンドを実行するときは、`downloadLinkQualifier` の文字列を設定します。 このプロパティーを省略すると、デフォルト値は次のようになります。

`\.(exe|zip|wav|mp3|mov|mpg|avi|wmv|pdf|doc|docx|xls|xlsx|ppt|pptx)$`

別の条件を使用してダウンロードリンクを選定する場合は、このプロパティを目的の正規表現値に設定します。

```js
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "clickCollectionEnabled": true,
  "downloadLinkQualifier": "\.(exe|zip|wav|mp3|mov|mpg|avi|wmv|pdf|doc|docx|xls|xlsx|ppt|pptx)$"
});
```
