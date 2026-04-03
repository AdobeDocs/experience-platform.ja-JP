---
title: thirdPartyCookiesEnabled
description: 訪問者を識別するためのサードパーティ Cookieの使用を許可します。
exl-id: f241a9ae-a892-46a5-b0dd-5ac72a44d4ac
source-git-commit: b292b9243816b1eed7fd3939096ddc30d6be0606
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---

# `thirdPartyCookiesEnabled`

`thirdPartyCookiesEnabled` プロパティは、Web SDKがサードパーティのコンテキストでCookieを設定するかどうかを判断するブール値です。 このオプションを有効にすると、組織が所有するサブドメインまたはドメイン間で訪問者を識別する場合に便利です。 しかし、多くの最新のブラウザは、サードパーティ Cookieの設定と有効期限を制限します。 訪問者のブラウザーがサードパーティ Cookieをサポートしていない場合、このプロパティは何も実行しません。

`thirdPartyCookiesEnabled` プロパティは、[`CORE ID`](/help/collection/identity/overview.md#core-id-and-third-party-identity)回の呼び出しで[`getIdentity`](../getidentity.md)を要求できるかどうかも制御します。

このオプションを有効にすると、Web SDKはAdobe Audience Managerを使用して訪問者の識別に役立ちます。 このオプションを無効にすると、Audience Managerへの呼び出しは無効になります。 詳しくは、Audience Manager ユーザーガイドの「[Demdex ドメインへの呼び出しについて](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/demdex-calls.html?lang=ja)」を参照してください。

`thirdPartyCookiesEnabled` コマンドの実行時に`configure` ブール値を設定します。 Web SDKの設定時にこのプロパティを省略すると、デフォルトは`true`になります。 Web SDKでAudience Managerを使用して訪問者を識別しない場合は、この値を`false`に設定します。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  thirdPartyCookiesEnabled: false
});
```

## Web SDK タグ拡張機能を使用してサードパーティ Cookieを有効にする

この設定は、[ID設定](/help/tags/extensions/client/web-sdk/configure/identity.md#use-third-party-cookies)を使用して、Web SDK タグ拡張機能で設定できます。
