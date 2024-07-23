---
title: orgId
description: orgId プロパティは、データの送信先の組織をAdobeに伝える文字列です。
exl-id: 0e04e85a-800c-4927-a165-80a5a578f4c2
source-git-commit: 8fc0fd96f13f0642f7671d0e0f4ecfae8ab6761f
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---

# `orgId`

`orgId` プロパティは、データの送信先の組織をAdobeに伝える文字列です。 **このプロパティは、Web SDK を使用して送信されるすべてのデータに必要です。**

`orgID` を見つけるには：

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. Adobe Experience Cloud内の任意の場所で、**`[Ctrl]`** + **`[I]`** キーを押します。 [!UICONTROL  ユーザーデータデバッガー ] ウィンドウが開きます。
1. [!UICONTROL  現在の組織 ID](../../assets/copy.png) の横にある「**[!UICONTROL コピー]**![ コピー ] をクリックするか、「**[!UICONTROL 割り当てられた組織]**」タブをクリックして、アクセスできる他の組織 ID を表示します。
1. 目的の情報の検索が終了したら、「閉じる **[!UICONTROL をクリックし]** す。

組織 ID は常に 24 文字の英数字から成る文字列で、末尾は常に `@AdobeOrg` になります。

## Web SDK タグ拡張機能を使用した `orgID` の設定

[ タグ拡張機能の設定 ](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md) 時に、「**[!UICONTROL IMS 組織 ID]**」テキストフィールドに組織 ID を入力します。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL 拡張機能]** に移動し、[!UICONTROL Adobe Experience Platform Web SDK **[!UICONTROL カードの]** 設定 ] をクリックします。
1. 上部の「[!UICONTROL IMS 組織 ID]」テキストフィールドに目的の組織 ID を入力します。
1. 「**[!UICONTROL 保存]**」をクリックして、変更を公開します。

## Web SDK JavaScript ライブラリを使用した `orgID` の設定

`configure` コマンドを実行するときは、`orgId` の文字列を設定します。 Web SDK の設定時にこのプロパティを省略すると、Web SDK がコンソールエラーをスローし、データがAdobeに送信されません。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
});
```
