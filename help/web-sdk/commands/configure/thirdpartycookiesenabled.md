---
title: thirdPartyCookiesEnabled
description: 訪問者を識別するためのサードパーティ Cookie の使用を許可する。
exl-id: f241a9ae-a892-46a5-b0dd-5ac72a44d4ac
source-git-commit: 8fc0fd96f13f0642f7671d0e0f4ecfae8ab6761f
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---


# `thirdPartyCookiesEnabled`

>[!IMPORTANT]
>
>Google[ 発表しました ](https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout) は、2024 年後半にサードパーティ cookie に対するChromeのサポートを廃止する計画です。 その結果、主要なブラウザーでサードパーティ cookie がサポートされなくなります。
>
>この変更が実装されると、Adobeでは、Web SDK で現在サポートされている `demdex` Cookie のサポートを停止します。


`thirdPartyCookiesEnabled` プロパティは、Web SDK がサードパーティコンテキストで cookie を設定するかどうかを決定するブール値です。 このオプションを有効にすると、組織が所有するサブドメイン間またはドメイン間で訪問者を識別する場合に役立ちます。 ただし、最新のブラウザーの多くは、サードパーティ cookie の設定と有効期限を制限しています。

このオプションを有効にすると、Web SDK はAdobe Audience Managerを使用して訪問者の特定に役立ちます。 このオプションを無効にすると、コールトゥAudience Managerは無効になります。 詳しくは、Audience Managerユーザーガイドの [Demdex ドメインの呼び出しについて ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/demdex-calls.html?lang=ja) を参照してください。

## Web SDK タグ拡張機能を使用してサードパーティ Cookie を有効にする

[ タグ拡張機能の設定 ](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md) の場合は、「**[!UICONTROL サードパーティ Cookie を使用]**」チェックボックスを選択します。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL 拡張機能]** に移動し、[!UICONTROL Adobe Experience Platform Web SDK **[!UICONTROL カードの]** 設定 ] をクリックします。
1. 「[!UICONTROL ID]」セクションまでスクロールし、「**[!UICONTROL サードパーティ Cookie を使用]**」チェックボックスを選択します。
1. 「**[!UICONTROL 保存]**」をクリックして、変更を公開します。

## Web SDK JavaScript ライブラリを使用してサードパーティ Cookie を有効にする

`configure` コマンドを実行するときは、`thirdPartyCookiesEnabled` のブール値を設定します。 Web SDK の設定時にこのプロパティを省略すると、デフォルトは `true` になります。 Web SDK で訪問者の特定にAudience Managerを使用しない場合は、この値を `false` に設定します。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  thirdPartyCookiesEnabled: false
});
```
