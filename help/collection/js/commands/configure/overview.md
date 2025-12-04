---
title: Adobe Experience Platform Web SDKの設定
description: Web SDKを使用する場合は、configure コマンドを使用して必要な設定を行います。
exl-id: 05ba98ae-c004-4b7b-b55b-38290ca62cfa
source-git-commit: db7e6df1b1a0eb19518d9c6ccd6e6bb9131d5a3e
workflow-type: tm+mt
source-wordcount: '82'
ht-degree: 0%

---

# Adobe Experience Platform Web SDKの設定

Web SDKの設定は、**`configure`** コマンドで行います。 このコマンドは、[`sendEvent`](../sendevent/overview.md) などの他の web SDK コマンドを呼び出す前に、ページが読み込まれるたびに必要になります。

**[`datastreamId`](datastreamid.md) プロパティと [`orgId`](orgid.md) プロパティが必要です。** その他のプロパティはすべてオプションです。組織の実装要件に応じて指定します。 次の例は、ほとんどの使用可能なプロパティを使用した設定オブジェクトを示しています。

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
    filterClickDetails: function(content) {
      content.linkUrl = "https://example.com/current.html";
    },
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
  prehidingStyle: "#container { opacity: 0 !important }",
  targetMigrationEnabled: true,
  thirdPartyCookiesEnabled: false
});
```
