---
title: orgId
description: orgId プロパティは、データの送信先の組織をAdobeに伝える文字列です。
source-git-commit: f75dcfc945be2f45c1638bdd4d670288aef6e1e6
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---

# `orgId`

The `orgId` プロパティは、データの送信先の組織をAdobeに伝える文字列です。 **このプロパティは、Web SDK を使用して送信されるすべてのデータに必要です。**

次の場所に `orgID`:

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. Adobe Experience Cloud内の任意の場所で、 **`[Ctrl]`** + **`[I]`**. A [!UICONTROL ユーザーデータデバッガー] ウィンドウが開きます。
1. クリック **[!UICONTROL コピー]** ![コピー](../../assets/copy.png) の横 [!UICONTROL 現在の組織 ID]または、 **[!UICONTROL 割り当てられた組織]** タブをクリックして、アクセスできる他の組織 ID を表示します。
1. 目的の情報の検索が終了したら、 **[!UICONTROL 閉じる]**.

組織 ID は常に 24 文字の英数字文字列で、常にで終わります。 `@AdobeOrg`.

## の設定 `orgID` Web SDK タグ拡張機能の使用

に組織 ID を入力します。 **[!UICONTROL IMS 組織 ID]** 次の場合のテキストフィールド [タグ拡張の設定](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md).

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、次に **[!UICONTROL 設定]** の [!UICONTROL Adobe Experience Platform Web SDK] カード。
1. 目的の組織 ID を [!UICONTROL IMS 組織 ID] 上部付近のテキストフィールド。
1. クリック **[!UICONTROL 保存]**&#x200B;をクリックし、変更を公開します。

## の設定 `orgID` Web SDK JavaScript ライブラリの使用

を設定します。 `orgId` 文字列（実行時） `configure` コマンドを使用します。 Web SDK を設定する際にこのプロパティを省略した場合、Web SDK はコンソールエラーをスローし、データはAdobeに送信されません。

```js
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
});
```
