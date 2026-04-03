---
title: ドメイン間でIDを共有する
description: 組織が所有するドメインをまたいでIDの継続性を維持し、パーソナライゼーションとレポート機能を向上させます。
source-git-commit: bf0bb72777cacd822fd6e887ac3ef71764784214
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 0%

---

# ドメイン間でIDを共有する

組織が所有するドメイン間で訪問者が移動すると、各ドメインはデフォルトで独自の訪問者IDを維持します。 明示的な引き継ぎがなければ、ドメインの1つから別のドメインへのクリックを行った訪問者は、移行先サイトで新しい未知の人物として扱われます。 このタイプの実装フラグメントは、レポートを作成し、パーソナライゼーションを再起動します。

クロスドメイン ID共有は、訪問者がリンクをクリックするかリダイレクトされたときに、宛先URLに`adobe_mc` クエリ文字列パラメーターを追加することで、この問題を解決します。 このパラメーターには、訪問者の[Experience Cloud ID （ECID） &#x200B;](./overview.md)、組織ID、およびタイムスタンプが含まれます。 宛先ページが有効な`adobe_mc` パラメーターで読み込まれると、Web SDKはそれを自動的に読み取り、最初のEdge Network リクエストにハンドオフ IDを適用するため、両方のドメインが同じ訪問者を共有します。 `adobe_mc` パラメーターは5分後に有効期限が切れるため、リダイレクト後に宛先ページを迅速に読み込む必要があります。

このユースケースでは、異なるドメイン上のweb サイト間でのID共有について説明します。 モバイルアプリからWebViewまたはモバイル web ページにIDを渡す場合は、代わりに[&#x200B; モバイル web ID共有](./mobile-to-web.md)を使用します。

## 前提条件

開始する前に、実装が次の要件を満たしていることを確認してください。

* **Web SDK**: [Web SDK](/help/collection/js/js-overview.md) バージョン **2.11.0以降**、またはWeb SDK タグ拡張機能が、ソース ドメインと宛先ドメインの両方にインストールされています。
* **一致する設定**:Web SDKの設定時に、すべての参加ドメインで同じ[`orgId`](../js/commands/configure/orgid.md)が使用されます。
* **URL コントロール**：コードは、ドメイン間のリンクまたはリダイレクトを制御して、クエリ文字列パラメーターを宛先URLに追加できるようにします。

## クロスドメイン共有の導入

クロスドメインのハンドオフでソースとして機能するすべてのドメインでID共有を設定する必要があります。 訪問者が2つのドメイン間で両方の方向に移動できる場合は、両方のドメインをソースとして設定します。

>[!BEGINTABS]

>[!TAB JavaScript library]

[`appendIdentityToUrl`](/help/collection/js/commands/appendidentitytourl.md) コマンドを使用して、`adobe_mc` パラメーターを送信リンクに追加します。 次の例では、アンカー要素のクリックをリッスンし、目的のドメインを指すリンクにIDを追加します。

```js
document.addEventListener("click", event => {
  // Check if the click was a link
  const anchor = event.target.closest("a");
  if (!anchor || !anchor.href) return;

  // Check if the link points to a domain you want to share identity with
  const url = new URL(anchor.href);
  if (!url.hostname.endsWith(".example.com") && !url.hostname.endsWith(".example.org")) return;

  // Append the identity to the URL, then navigate
  event.preventDefault();
  alloy("appendIdentityToUrl", { url: anchor.href }).then(result => {
    window.open(result.url, anchor.target || "_self");
  });
});
```

>[!TAB Web SDK タグ拡張機能]

[**[!UICONTROL Redirect with identity]**](/help/tags/extensions/client/web-sdk/actions/redirect-with-identity.md) アクションを使用して、`adobe_mc` パラメーターをアウトバウンドリンクに追加します。 次の条件を使用してルールを作成すると、目的の動作を実現できます。

1. **イベント**：拡張機能を&#x200B;**[!UICONTROL Core]**&#x200B;に、イベントタイプを&#x200B;**[!UICONTROL Click]**&#x200B;に設定します。 **[!UICONTROL Elements matching the CSS selector]**&#x200B;の下に、`a[href]`と入力します。
2. **条件**：拡張機能を&#x200B;**[!UICONTROL Core]**&#x200B;に、条件タイプを&#x200B;**[!UICONTROL Value Comparison]**&#x200B;に設定します。 **[!UICONTROL Left Operand]**&#x200B;を`%this.hostname%`に、**[!UICONTROL Operator]**&#x200B;を&#x200B;**[!UICONTROL Matches Regex]**&#x200B;に、**[!UICONTROL Right Operand]**&#x200B;を宛先ドメインに一致する正規表現に設定します（例：`example\.com$|example\.org$`）。
3. **アクション**：拡張機能を&#x200B;**[!UICONTROL Adobe Experience Platform Web SDK]**&#x200B;に、アクションタイプを&#x200B;**[!UICONTROL Redirect with identity]**&#x200B;に設定します。

>[!ENDTABS]

## 宛先ドメインでIDを受信する

宛先ドメインに追加のコードは必要ありません。 Web SDKがページに存在し、URLに有効な`adobe_mc` パラメーターが含まれている場合、SDKはECIDを自動的に抽出し、最初のEdge Network リクエストで訪問者のID マップに適用します。

宛先ドメインが次の条件を満たしていることを確認します。

* Web SDKまたはWeb SDK タグ拡張機能がインストールされ、ソースドメインと同じ[`orgId`](../js/commands/configure/orgid.md)で設定されています。 JavaScript ライブラリとWeb SDK タグ拡張機能は、同じ`orgId`を共有している限り、ドメイン間で同じ意味で使用できます。
* ページは、**パラメーターが期限切れになるまでに、リダイレクトから** 5分`adobe_mc`以内に最初のEdge Network リクエストを読み込んで送信します。
