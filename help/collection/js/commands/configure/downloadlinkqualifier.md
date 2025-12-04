---
title: downloadLinkQualifier
description: 自動リンクトラッキングがダウンロードリンクを選定する方法を決定するのに役立ちます。
exl-id: ef4f0ed4-83fc-485f-866d-f9ca15447ac8
source-git-commit: 2e39a7809049c199d4778a0e17eb9e0f3b1d9775
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---

# `downloadLinkQualifier`

[`clickCollectionEnabled`](clickcollectionenabled.md) を使用して自動リンクトラッキングを有効にすると、`downloadLinkQualifier` プロパティが、ダウンロードリンクと見なされる URL の条件を決定するのに役立ちます。

このプロパティは正規表現文字列です。 クリックされた URL がこの正規表現に一致する場合、`xdm.web.webInteraction.type` は `"download"` に設定されます。 リンクに `download` HTML属性が含まれる場合、リンクは直ちにダウンロードリンクとして分類されます。 `clickCollectionEnabled` が有効になっていない場合、このプロパティは何もしません。

`downloadLinkQualifier` コマンドを実行するときは、`configure` の文字列を設定します。 このプロパティーを省略すると、デフォルト値は次のようになります。

`\.(exe|zip|wav|mp3|mov|mpg|avi|wmv|pdf|doc|docx|xls|xlsx|ppt|pptx)$`

別の条件を使用してダウンロードリンクを選定する場合は、このプロパティを目的の正規表現値に設定します。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  clickCollectionEnabled: true,
  downloadLinkQualifier: "\.(exe|zip|wav|mp3|mov|mpg|avi|wmv|pdf|doc|docx|xls|xlsx|ppt|pptx)$"
});
```

## Web SDK タグ拡張機能を使用したリンク修飾子のダウンロード

タグ拡張機能を設定する際に、このフィールドと同等の Web SDK タグ拡張機能を [Data collection configuration settings](/help/tags/extensions/client/web-sdk/configure/data-collection.md#download-link-qualifier) の下に配置します。
