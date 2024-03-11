---
title: Adobe Experience Platform Web SDK の設定
description: Web SDK を使用する際には、configure コマンドを使用して必要な設定を指定します。
exl-id: 05ba98ae-c004-4b7b-b55b-38290ca62cfa
source-git-commit: 58cd6300307881c3de7c52e07c401bf2ed908517
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# Adobe Experience Platform Web SDK の設定

Web SDK の設定は、 `configure` コマンドを使用します。 Web SDK の設定は、ライブラリまたはタグ拡張が使用されるたびに実行する必要がある、重要で必須の手順です。

## タグ拡張機能を使用した Web SDK の設定 {#configure-tag-extension}

次の手順に従って、タグ拡張を通じて Web SDK を設定します。

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、次に **[!UICONTROL 設定]** の [!UICONTROL Adobe Experience Platform Web SDK] カード。
1. 次に移動： [Web SDK タグ拡張機能の設定ページ](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md) 」を参照してください。

これらの設定は、拡張機能を使用してデータをAdobeに送信する際にいつでも設定されます。

## JavaScript ライブラリを使用した Web SDK の設定 {#configure-js}

を実行します。 `configure` コマンドを使用します。 このコマンドは、他の Web SDK コマンド（例： ）を呼び出す前に必要です。 [`sendEvent`](../sendevent/overview.md).

The [`edgeConfigId`](edgeconfigid.md) および [`orgId`](orgid.md) プロパティが必要です。 組織の実装要件に応じて、その他すべてのプロパティはオプションです。

サポートされる各コマンドの詳細については、このユーザーガイドの目次を参照してください。

```js
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "clickCollectionEnabled": false,
  "context": ["web", "device", "environment", "placeContext", "highEntropyUserAgentHints"],
  "debugEnabled": true,
  "defaultConsent": "pending",
  "downloadLinkQualifier": "\.(exe|zip|wav|mp3|mov|mpg|avi|wmv|pdf|doc|docx|xls|xlsx|ppt|pptx)$",
  "edgeBasePath": "ee",
  "edgeConfigOverrides": { "datastreamId": "0dada9f4-fa94-4c9c-8aaf-fdbac6c56287" },
  "edgeDomain": "data.example.com",
  "idMigrationEnabled": false,
  "onBeforeEventSend": function(content) {
    if(content.xdm.web?.webReferrer) delete content.xdm.web.webReferrer.URL;
  },
  "onBeforeLinkClickSend": function(content) {
    content.xdm.web.webPageDetails.URL = "https://example.com/current.html";
  },
  "prehidingStyle": "#container { opacity: 0 !important }",
  "targetMigrationEnabled": true,
  "thirdPartyCookiesEnabled": false
});
```
