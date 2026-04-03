---
title: defaultConsent
description: Web プロパティのデフォルトの同意収集方法を設定します。
exl-id: 2a22fa8b-a234-4d3e-9b55-c7482a928fe6
source-git-commit: bf0bb72777cacd822fd6e887ac3ef71764784214
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 0%

---


# `defaultConsent`

`defaultConsent` プロパティは、[`setConsent`](../setconsent.md) コマンドを呼び出す前に、データ収集の同意を処理する方法を決定します。 このプロパティは、データを収集する前に同意が必要な地域に居住する個人から、誤ってデータを収集したくない場合に役立ちます。

一般データ保護規則（GDPR）の管轄外の訪問者がある場合、デフォルトの同意は`in`に設定できます。 GDPRの管轄区域の訪問者は、デフォルトの同意を`pending`に設定できます。 同意管理プラットフォーム （CMP）は、顧客の地域を検出し、IAB TCF 2.0にフラグ `gdprApplies`を提供できます。このフラグは、デフォルトの同意を設定するために使用できます。

`defaultConsent` コマンドの実行時に、`configure`文字列プロパティを目的の同意レベルに設定します。 このプロパティでは大文字と小文字が区別され、サポートされる値は`"in"`、`"out"`、`"pending"`の3つのみです。 他の値を使用しようとすると、ライブラリはエラーをスローします。 `configure` コマンドで設定されていない場合、デフォルト値は&#x200B;**`in`**&#x200B;です。

>[!IMPORTANT]
>
>`defaultConsent`の値は、ページ読み込み中に保持されません。 `configure` コマンドを呼び出すたびに、必要なデフォルトの同意を設定してください。 対照的に、訪問者の解決済みの同意（[`setConsent`](../setconsent.md)で設定）はCookieに保持され、その後のページ読み込みに自動的に適用されます。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  defaultConsent: "pending"
});
```

* **`in`**: データ収集は、ユーザーがオプトアウトするまで正常に動作します。
* **`out`**: ユーザーがオプトインするまで、データは完全に破棄されます。
* **`pending`**: ユーザーが[`setConsent`](../setconsent.md) コマンドを使用してオプトインするまで、データはローカルに保存されます。

>[!NOTE]
>
>Adobeでは、Adobeの機能と製品オファーに対応する、より堅牢な目的やカテゴリーの構築を計画していますが、現在の導入は、オプトインに対する万能のアプローチです。 この制限は、Web SDKにのみ適用され、他のAdobe JavaScript ライブラリには適用されません。

## `defaultConsent`と`setConsent`の併用 {#using-consent}

`defaultConsent`と`setConsent`を一緒に使用すると、設定された値に応じて、データ収集、Cookie設定、IDの結果が異なります。 完全なインタラクションテーブルについては、[ データ収集](/help/collection/identity/consent.md#how-consent-affects-identity)の同意とIDを参照してください。

## `gdprApplies`に基づく既定の同意の設定

一部のCMPは、一般データ保護規則（GDPR）が顧客に適用されるかどうかを判断する機能を提供しています。 GDPRが適用されない顧客の同意を取り込む場合は、TCF API呼び出しで`gdprApplies` フラグを使用できます。 例：

```js
var alloyConfiguration = { ... };
window.__tcfapi('getTCData', 2, function (tcData, success) {
  if (success) {
    alloyConfiguration.defaultConsent = tcData.gdprApplies ? "pending" : "in";
    window.alloy("configure", alloyConfiguration);
  }
});
```

上記のコードブロックでは、`configure`がTCF APIから取得された後、`tcData` コマンドが呼び出されます。 `gdprApplies`がtrueの場合、デフォルトの同意は`pending`に設定されます。 `gdprApplies`がfalseの場合、デフォルトの同意は`in`に設定されます。 設定で`alloyConfiguration`変数を必ず入力してください。

## Web SDK タグ拡張機能を使用したデフォルトの同意

タグを使用してこれらのアクションを実行する方法については、Web SDK タグ拡張機能ドキュメントの[同意設定](/help/tags/extensions/client/web-sdk/configure/consent.md)を参照してください。
