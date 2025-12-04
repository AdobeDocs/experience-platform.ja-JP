---
title: orgId
description: orgId プロパティは、データの送信先の組織をAdobeに伝える文字列です。
exl-id: 0e04e85a-800c-4927-a165-80a5a578f4c2
source-git-commit: 2e39a7809049c199d4778a0e17eb9e0f3b1d9775
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 0%

---

# `orgId`

`orgId` プロパティは、データの送信先をAdobeに伝える文字列です。 **このプロパティは、Web SDKを使用して送信されるすべてのデータに必要です。**

`orgID` を見つけるには：

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. Adobe Experience Cloud内の任意の場所で、**`[Ctrl]`** + **`[I]`** キーを押します。 [!UICONTROL User Data Debugger] ウィンドウが開きます。
1. **[!UICONTROL Copy]** ージの横にある「![ ージ ](../../assets/copy.png) コピー [!UICONTROL Current Org ID]」をクリックするか、「**[!UICONTROL Assigned Orgs]**」タブをクリックして、アクセス可能な他の組織 ID を表示します。
1. 目的の情報の検索が終了したら、「**[!UICONTROL Close]**」をクリックします。

組織 ID は常に 24 文字の英数字から成る文字列で、末尾は常に `@AdobeOrg` になります。

`orgId` コマンドを実行するときは、`configure` の文字列を設定します。 Web SDKの設定時にこのプロパティを省略すると、Web SDKはコンソールエラーをスローし、データがAdobeに送信されません。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
});
```

## Web SDK タグ拡張機能を使用して組織 ID を設定します

この設定は、[SDK インスタンス設定 ](/help/tags/extensions/client/web-sdk/configure/general.md) を使用して、web SDK タグ拡張機能で設定できます。 フィールドは、タグプロパティが作成された組織に基づいて自動的に入力されます。
