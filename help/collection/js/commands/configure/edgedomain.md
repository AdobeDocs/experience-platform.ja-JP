---
title: edgeDomain
description: データの送信先となるドメインを決定します。
exl-id: 6beb5116-cd23-42fd-934c-5cf84d1d7153
source-git-commit: 2d3c31e399989652a0472bbe2174ca8d8554ba30
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 3%

---

# `edgeDomain`

`edgeDomain` プロパティを使用すると、Web SDKがデータを送信するドメインを変更できます。 カスタムドメインを使用することで、広告ブロッカーの影響を軽減することができます。

>[!NOTE]
>
>このプロパティは、Cookieの設定場所を変更しません。 Web SDKは、最終的にデータを送信する場所に関係なく、常に[&#x200B; ファーストパーティ Cookie](https://experienceleague.adobe.com/docs/core-services/interface/administration/ec-cookies/cookies-first-party.html?lang=ja)を設定します。

`edgeDomain`に使用する値は、[Adobeが管理する証明書プログラム &#x200B;](https://experienceleague.adobe.com/en/docs/core-services/interface/data-collection/adobe-managed-cert)への参加によって異なります。

**Adobeが管理する証明書プログラム**&#x200B;に参加している場合は、証明書の設定時に選択した1st パーティドメインに値を設定します。 通常、この値は組織が所有するサブドメインです。 たとえば、`data.example.com` のように設定します。組織内のCNAME レコードは、そのデータをAdobeに転送します。

**組織が証明書プログラム**&#x200B;に参加していない場合は、値を`data.adobedc.net`のサブドメインに設定します。 Adobeでは、組織のAdobeに割り当てられたIMS会社IDを使用して一貫性を保つことをお勧めします。 たとえば、`example.data.adobedc.net` のように設定します。IMS会社IDを特定するには、次の手順を実行します。

1. Adobe IDの資格情報を使用して[experience.adobe.com](https://experience.adobe.com)にログインします。
1. Experience Cloud インターフェイスの任意の場所で、`[Cmd]` + `[I]` （macOS）または`[Ctrl]` + `[I]` （Windows）を押します。
1. **[!UICONTROL User data debugger]**&#x200B;が表示されます。 「**[!UICONTROL Assigned orgs]**」タブを選択します。
1. 目的のIMS組織を展開します。
1. **[!UICONTROL Tenant]** フィールドを探します。 この値は、使用する`data.adobedc.net`の推奨サブドメインです。

`edgeDomain` コマンドの実行時に`configure`文字列を設定します。 SDKの設定時にこのプロパティを省略すると、デフォルトは`edge.adobedc.net`になります。 デフォルト値は許容されますが、Adobeでは、組織固有の値を設定することをベストプラクティスと見なします。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  edgeDomain: "data.example.com"
});
```

## Web SDK タグ拡張機能を使用したEdge ドメイン

このプロパティと同等のタグ拡張機能は、拡張機能の設定時に&#x200B;**[!UICONTROL Edge domain]** SDK インスタンスの設定[の下にある](/help/tags/extensions/client/web-sdk/configure/general.md) フィールドです。
