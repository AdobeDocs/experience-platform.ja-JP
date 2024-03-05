---
title: edgeDomain
description: データの送信先のルートドメインを決定します。
source-git-commit: f75dcfc945be2f45c1638bdd4d670288aef6e1e6
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 1%

---

# `edgeDomain`

The `edgeDomain` プロパティを使用すると、Web SDK がデータを送信するドメインを変更できます。 このプロパティは、 [ファーストパーティ Cookie](https://experienceleague.adobe.com/docs/core-services/interface/administration/ec-cookies/cookies-first-party.html?lang=ja). データは組織自体のドメインに送信され、その後、CNAME レコードがそのデータをAdobeに転送します。

組織で、の設定時にこのプロパティの正しい値を決定します [ファーストパーティ Cookie](https://experienceleague.adobe.com/docs/core-services/interface/administration/ec-cookies/cookies-first-party.html?lang=ja). 組織は通常、この目的で専用のサブドメインを使用します。 例えば、 `example.com`に設定されている場合、 `data.example.com`.

## Web SDK タグ拡張機能を使用したエッジドメインの設定

を設定します。 **[!UICONTROL Edge ドメイン]** 次の場合のテキストフィールド [タグ拡張の設定](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md).

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、次に **[!UICONTROL 設定]** の [!UICONTROL Adobe Experience Platform Web SDK] カード。
1. テキストフィールドを見つけます。 **[!UICONTROL Edge ドメイン]**&#x200B;をクリックし、必要な値を入力します。
1. クリック **[!UICONTROL 保存]**&#x200B;をクリックし、変更を公開します。

## Web SDK JavaScript ライブラリを使用したエッジドメインの設定

を設定します。 `edgeDomain` 文字列（実行時） `configure` コマンドを使用します。 SDK を設定する際にこのプロパティを省略した場合、デフォルトはになります。 `edge.adobedc.net`. Web SDK がデータを送信するドメインを上書きする場合は、この値を設定します。

```js
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "edgeDomain": "data.example.com"
});
```
