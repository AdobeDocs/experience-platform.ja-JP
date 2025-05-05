---
title: edgeDomain
description: データの送信先のルートドメインを決定します。
exl-id: 6beb5116-cd23-42fd-934c-5cf84d1d7153
source-git-commit: 8fc0fd96f13f0642f7671d0e0f4ecfae8ab6761f
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 1%

---

# `edgeDomain`

`edgeDomain` プロパティを使用すると、Web SDK がデータを送信するドメインを変更できます。 このプロパティは、[ ファーストパーティ cookie](https://experienceleague.adobe.com/docs/core-services/interface/administration/ec-cookies/cookies-first-party.html?lang=ja) を使用している組織でよく使用されます。 データは組織自身のドメインに送信され、CNAME レコードがそのデータをAdobeに転送します。

[ ファーストパーティ cookie](https://experienceleague.adobe.com/docs/core-services/interface/administration/ec-cookies/cookies-first-party.html?lang=ja) を設定する際に、組織がこのプロパティの正しい値を決定します。 通常、組織はこの目的で専用のサブドメインを使用します。 例えば、ドメイン `example.com` を使用する場合、`data.example.com` でファーストパーティ cookie を設定できます。

## Web SDK タグ拡張機能を使用したエッジドメインの設定

[ タグ拡張機能の設定 ](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md) 時に、「**[!UICONTROL Edge ドメイン]**」テキストフィールドを設定します。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL 拡張機能]** に移動し、[!UICONTROL Adobe Experience Platform Web SDK **[!UICONTROL カードの]** 設定 &#x200B;] をクリックします。
1. テキストフィールド **[!UICONTROL Edge domain]** を見つけ、目的の値を入力します。
1. 「**[!UICONTROL 保存]**」をクリックして、変更を公開します。

## Web SDK JavaScript ライブラリを使用して Edge ドメインを設定します

`configure` コマンドを実行するときは、`edgeDomain` の文字列を設定します。 SDK の設定時にこのプロパティを省略すると、デフォルトは `edge.adobedc.net` になります。 Web SDK がデータを送信するドメインを上書きする場合は、この値を設定します。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  edgeDomain: "data.example.com"
});
```
