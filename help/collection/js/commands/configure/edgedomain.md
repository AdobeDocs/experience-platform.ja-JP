---
title: edgeDomain
description: データの送信先のルートドメインを決定します。
exl-id: 6beb5116-cd23-42fd-934c-5cf84d1d7153
source-git-commit: 09799847c61d82ed5b7cd372d92aa436697d54f3
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 3%

---

# `edgeDomain`

`edgeDomain` プロパティを使用すると、Web SDKがデータを送信するドメインを変更できます。 このプロパティは、[&#x200B; ファーストパーティ cookie](https://experienceleague.adobe.com/docs/core-services/interface/administration/ec-cookies/cookies-first-party.html?lang=ja) を使用している組織でよく使用されます。 データは組織自身のドメインに送信され、CNAME レコードがそのデータをAdobeに転送します。

`edgeDomain` に使用する値は、[Adobeが管理する証明書プログラム &#x200B;](https://experienceleague.adobe.com/ja/docs/core-services/interface/data-collection/adobe-managed-cert) への参加によって異なります。

**組織がAdobe管理の証明書プログラムに参加している場合**、証明書の設定時に選択したファーストパーティドメインに値を設定します。 通常、この値は組織が所有するサブドメインです。 たとえば、`data.example.com` のように設定します。組織内の CNAME レコードは、そのデータをAdobeにリダイレクトします。

**証明書プログラムに参加していない場合**、値を `data.adobedc.net` のサブドメインに設定します。 Adobeでは、一貫性を保つため、組織の会社 ID を使用することをお勧めします。 たとえば、`example.data.adobedc.net` のように設定します。以下の手順を使用して、会社 ID を決定します。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. Experience Cloud インターフェイスの任意の場所で、`[Cmd]` + `[I]` （iOS）または `[Ctrl]` + `[I]` （Windows）を押します。
1. **[!UICONTROL User data debugger]** が表示されます。 「**[!UICONTROL Assigned orgs]**」タブを選択します。
1. 目的の IMS 組織を展開します。
1. 「**[!UICONTROL Tenant]**」フィールドを見つけます。 この値は、使用する `data.adobedc.net` の推奨サブドメインです。

`edgeDomain` コマンドを実行するときは、`configure` の文字列を設定します。 SDKの設定時にこのプロパティを省略した場合、デフォルトは `edge.adobedc.net` になります。 デフォルト値は許容できますが、Adobeでは、組織固有の値を設定することがベストプラクティスだと考えています。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  edgeDomain: "data.example.com"
});
```

## Web SDK タグ拡張機能を使用したEdge ドメイン

このプロパティと同等のタグ拡張機能は、拡張機能を設定する際の **[!UICONTROL Edge domain]** SDK インスタンス設定 [&#x200B; の &#x200B;](/help/tags/extensions/client/web-sdk/configure/general.md) フィールドです。
