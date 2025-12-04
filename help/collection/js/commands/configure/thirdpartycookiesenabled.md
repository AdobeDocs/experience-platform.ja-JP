---
title: thirdPartyCookiesEnabled
description: 訪問者を識別するためのサードパーティ Cookie の使用を許可する。
exl-id: f241a9ae-a892-46a5-b0dd-5ac72a44d4ac
source-git-commit: c6a2b9700f0a688f65fec9febf5622c6c7b6aafa
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---

# `thirdPartyCookiesEnabled`

`thirdPartyCookiesEnabled` プロパティは、Web SDKがサードパーティコンテキストで cookie を設定するかどうかを指定するブール値です。 このオプションを有効にすると、組織が所有するサブドメイン間またはドメイン間で訪問者を識別する場合に役立ちます。 ただし、最新のブラウザーの多くは、サードパーティ cookie の設定と有効期限を制限しています。 訪問者のブラウザーがサードパーティ cookie をサポートしていない場合、このプロパティは何もしません。

`thirdPartyCookiesEnabled` プロパティは、[`CORE ID`](/help/collection/use-cases/identity/id-overview.md#tracking-coreid-web-sdk) 呼び出しで [`getIdentity`](../getidentity.md) を要求できるかどうかを制御します。

このオプションを有効にすると、web SDKがAdobe Audience Managerを使用して訪問者の特定に役立ちます。 このオプションを無効にすると、Audience Managerの呼び出しは無効になります。 詳しくは、Audience Manager ユーザーガイドの [Demdex ドメインの呼び出しについて &#x200B;](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/demdex-calls.html?lang=ja) を参照してください。

`thirdPartyCookiesEnabled` コマンドを実行するときは、`configure` のブール値を設定します。 Web SDKの設定時にこのプロパティを省略した場合、デフォルトは `true` になります。 Web SDKで訪問者の特定にAudience Managerを使用しない場合は、この値を `false` に設定します。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  thirdPartyCookiesEnabled: false
});
```

## Web SDK タグ拡張機能を使用してサードパーティ Cookie を有効にする

この設定は、Web SDK タグ拡張機能で [ID 設定 &#x200B;](/help/tags/extensions/client/web-sdk/configure/identity.md#use-third-party-cookies) を使用して設定できます。
