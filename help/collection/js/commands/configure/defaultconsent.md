---
title: defaultConsent
description: Web プロパティのデフォルトの同意収集方法を設定します。
exl-id: 2a22fa8b-a234-4d3e-9b55-c7482a928fe6
source-git-commit: 1e272eb18fac2f59f9737756d48947a25573d772
workflow-type: tm+mt
source-wordcount: '514'
ht-degree: 5%

---


# `defaultConsent`

`defaultConsent` プロパティは、[`setConsent`](../setconsent.md) コマンドを呼び出す前にデータ収集の同意を処理する方法を決定します。 このプロパティは、データを収集する前に同意が必要なエリアに住む個人から誤ってデータを収集したくない場合に役立ちます。

EU 一般データ保護規則（GDPR）の管轄権外の訪問者がいる場合、デフォルトの同意を `in` に設定することもできます。 GDPR の管轄権内の訪問者は、デフォルトの同意が `pending` に設定されている場合があります。 同意管理プラットフォーム（CMP）は、顧客の地域を検出し、IAB TCF 2.0 に `gdprApplies` するフラグを提供できます。このフラグは、デフォルトの同意を設定するために使用できます。

`defaultConsent` コマンドを実行する際に、`configure` 文字列プロパティを目的の同意レベルに設定します。 このプロパティでは大文字と小文字が区別され、サポートされる値は `"in"`、`"out"`、`"pending"` の 3 つだけです。 その他の値を使用しようとすると、ライブラリがエラーをスローする。 `configure` コマンドで設定されていない場合、デフォルト値は **`in`** です。

>[!IMPORTANT]
>
>`defaultConsent` の値は、ページの読み込み間は保持されません。 `configure` コマンドを呼び出すたびに、必要なデフォルトの同意を必ず設定してください。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  defaultConsent: "pending"
});
```

* **`in`**：ユーザーがオプトアウトするまで、データ収集は正常に動作します。
* **`out`**: ユーザーがオプトインするまで、データは永続的に破棄されます。
* **`pending`**: ユーザーが [`setConsent`](../setconsent.md) コマンドを使用してオプトインするまで、データはローカルに保存されます。

>[!NOTE]
>
>Adobeは、Adobeの機能と提供される製品に対応する、より堅牢な目的またはカテゴリのセットを構築する予定ですが、現在の実装は、オプトインの手段としてすべてを取らないアプローチです。 この制限は、Web SDKにのみ適用され、他のAdobe JavaScript ライブラリには適用されません。

## `defaultConsent` と `setConsent` の併用 {#using-consent}

Web SDKには、2 つの補完的な同意オプションがあります。

* `defaultConsent` （このページ）: デフォルトの同意環境設定を決定します。
* [`setConsent`](../setconsent.md)：訪問者の同意環境設定を取り込みます。

これらの設定を一緒に使用すると、設定された値に応じて、異なるデータ収集および cookie 設定結果になる可能性があります。

同意設定に基づいてデータ収集が発生するタイミングと cookie が設定されるタイミングについて理解するには、次の表を参照してください。

| `defaultConsent` | `setConsent` | データ収集が発生 | Web SDKはブラウザー cookie を設定します |
|---------|----------|---------|---------|
| `in` | `in` | ○ | ○ |
| `in` | `out` | × | ○ |
| `in` | 設定なし | ○ | ○ |
| `pending` | `in` | ○ | ○ |
| `pending` | `out` | × | ○ |
| `pending` | 設定なし | × | × |
| `out` | `in` | ○ | ○ |
| `out` | `out` | × | ○ |
| `out` | 設定なし | × | × |

ライブラリによって設定される Cookie のリストについては [&#128279;](https://experienceleague.adobe.com/ja/docs/core-services/interface/data-collection/cookies/web-sdk)Adobe Experience Platform Web SDKの Cookie&rbrace; を参照してください。

>[!NOTE]
>
>訪問者が追跡をオプトアウトした場合でも、ID および同意 Cookie が設定されます。 これらの Cookie は、データ収集の環境設定に従うために必要です。

## `gdprApplies` に基づくデフォルトの同意の設定

一部の CMP は、お客様が GDPR （一般データ保護規則）を適用されているかどうかを判断する機能を提供します。 GDPR が適用されない顧客の同意を得たい場合は、TCF API 呼び出しで `gdprApplies` フラグを使用できます。 例：

```js
var alloyConfiguration = { ... };
window.__tcfapi('getTCData', 2, function (tcData, success) {
  if (success) {
    alloyConfiguration.defaultConsent = tcData.gdprApplies ? "pending" : "in";
    window.alloy("configure", alloyConfiguration);
  }
});
```

上記のコードブロックでは、TCF API から `configure` を取得した後に `tcData` コマンドが呼び出されます。 `gdprApplies` が true の場合、デフォルトの同意は `pending` に設定されます。 `gdprApplies` が false の場合、デフォルトの同意は `in` に設定されます。 必ず `alloyConfiguration` 変数に設定を入力してください。

## Web SDK タグ拡張機能を使用したデフォルトの同意

タグを使用してこれらのアクションを実行する方法については、Web SDK タグ拡張機能のドキュメントの [&#x200B; 同意設定 &#x200B;](/help/tags/extensions/client/web-sdk/configure/consent.md) を参照してください。
