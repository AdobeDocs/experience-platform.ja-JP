---
title: Adobe Experience Platform Web SDK の設定
description: Web SDK を使用する場合は、configure コマンドを使用して必要な設定を行います。
exl-id: 05ba98ae-c004-4b7b-b55b-38290ca62cfa
source-git-commit: 8fc0fd96f13f0642f7671d0e0f4ecfae8ab6761f
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# Adobe Experience Platform Web SDK の設定

Web SDK の設定は、`configure` コマンドで行います。 Web SDK の設定は必須の重要な手順であり、ライブラリまたはタグ拡張機能が使用されるたびに実行する必要があります。

## タグ拡張機能を使用した Web SDK の設定 {#configure-tag-extension}

タグ拡張機能を使用して Web SDK を設定するには、次の手順に従います。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL 拡張機能]** に移動し、[!UICONTROL Adobe Experience Platform Web SDK **[!UICONTROL カードの]** 設定 &#x200B;] をクリックします。
1. すべての設定オプションについて詳しくは、[Web SDK タグ拡張機能の設定 ](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md) ページを参照してください。

これらの設定は、Adobeを使用して拡張機能にデータを送信する場合に常に指定されます。

## JavaScript ライブラリを使用した Web SDK の設定 {#configure-js}

`configure` コマンドを実行します。 このコマンドは、[`sendEvent`](../sendevent/overview.md) などの他の Web SDK コマンドを呼び出す前に必要です。

[`datastreamId`](datastreamid.md) プロパティと [`orgId`](orgid.md) プロパティは必須です。 その他のプロパティはすべてオプションです。組織の実装要件に応じて指定します。

サポートされている各コマンドの詳細については、このユーザーガイドの目次を参照してください。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  clickCollectionEnabled: true,
  clickCollection: {
    internalLinkEnabled: true,
    downloadLinkEnabled: true,
    externalLinkEnabled: true,
    eventGroupingEnabled: true,
    sessionStorageEnabled: true
  },
  context: ["web", "device", "environment", "placeContext", "highEntropyUserAgentHints"],
  debugEnabled: true,
  defaultConsent: "pending",
  downloadLinkQualifier: "\.(exe|zip|wav|mp3|mov|mpg|avi|wmv|pdf|doc|docx|xls|xlsx|ppt|pptx)$",
  edgeBasePath: "ee",
  edgeConfigOverrides: { "datastreamId": "0dada9f4-fa94-4c9c-8aaf-fdbac6c56287" },
  edgeDomain: "data.example.com",
  idMigrationEnabled: false,
  onBeforeEventSend: function(content) {
    if(content.xdm.web?.webReferrer) delete content.xdm.web.webReferrer.URL;
  },
  onBeforeLinkClickSend: function(content) {
    content.xdm.web.webPageDetails.URL = "https://example.com/current.html";
  },
  prehidingStyle: "#container { opacity: 0 !important }",
  targetMigrationEnabled: true,
  thirdPartyCookiesEnabled: false
});
```
