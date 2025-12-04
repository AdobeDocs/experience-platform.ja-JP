---
title: appendIdentityToUrl
description: アプリ間、web 間およびドメイン間で、パーソナライズされたエクスペリエンスをより正確に提供します。
exl-id: 09dd03bd-66d8-4d53-bda8-84fc4caadea6
source-git-commit: c2564f1b9ff036a49c9fa4b9e9ffbdbc598a07a8
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 0%

---

# `appendIdentityToUrl`

`appendIdentityToUrl` コマンドを使用すると、ユーザー識別子をクエリ文字列として URL に追加できます。 このアクションにより、ドメイン間で訪問者の ID を持ち歩き、ドメインまたはチャネルの両方を含むデータセットに対して、重複した訪問者数を防ぐことができます。 Web SDK バージョン 2.11.0 以降で使用できます。

生成され、URL に追加されたクエリ文字列は `adobe_mc` です。 Web SDKで ECID が見つからない場合は、`/acquire` エンドポイントを呼び出して ECID を生成します。

>[!NOTE]
>
>同意が指定されていない場合、このメソッドからの URL は変更されずに返されます。 このコマンドは直ちに実行され、同意の更新を待つことはありません。

URL をパラメーターとして使用して `appendIdentityToUrl` コマンドを実行します。 メソッドは、識別子がクエリ文字列として追加された URL を返します。

```js
alloy("appendIdentityToUrl",
  {
    url: document.location.href
  }
);
```

ページ上で受け取ったすべてのクリックに関するイベントリスナーを追加し、URL が目的のドメインに一致するかどうかを確認できます。 追加される場合は、URL に ID を追加し、ユーザーをリダイレクトします。

```js
document.addEventListener("click", event => {
  // Check if the click was a link
  const anchor = event.target.closest("a");
  if (!anchor || !anchor.href) return;

  // Check if the link points to the desired domain
  const url = new URL(anchor.href);
  if (!url.hostname.endsWith(".adobe.com") && !url.hostname.endsWith(".behance.com")) return;

  // Append the identity to the URL, then direct the user to the URL
  event.preventDefault();
  alloy("appendIdentityToUrl", {url: anchor.href}).then(result => { window.open(result.url, anchor.target || "_self"); });
});
```

このコマンドは、[`edgeConfigOverrides`](configure/edgeconfigoverrides.md) オブジェクトをサポートします。

## 応答オブジェクト

このコマンドを使用して [ 応答を処理 ](command-responses.md) する場合、応答オブジェクトには、ID 情報がクエリ文字列パラメーターとして追加された **`url`** という新しい URL が含まれます。

## Web SDK タグ拡張機能を使用した URL への ID の追加

このコマンドと同等の web SDK タグ拡張機能は、「ID を使用してリダイレクト [ アクション ](/help/tags/extensions/client/web-sdk/actions/redirect-with-identity.md) です。
