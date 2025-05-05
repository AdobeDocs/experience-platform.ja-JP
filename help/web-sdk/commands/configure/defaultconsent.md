---
title: defaultConsent
description: Web プロパティのデフォルトの同意収集方法を設定します。
exl-id: 2a22fa8b-a234-4d3e-9b55-c7482a928fe6
source-git-commit: 8fc0fd96f13f0642f7671d0e0f4ecfae8ab6761f
workflow-type: tm+mt
source-wordcount: '799'
ht-degree: 5%

---


# `defaultConsent`

`defaultConsent` プロパティは、[`setConsent`](../setconsent.md) コマンドを呼び出す前にデータ収集の同意を処理する方法を決定します。 このプロパティは、データを収集する前に同意が必要なエリアに住む個人から誤ってデータを収集したくない場合に役立ちます。

デフォルトでは、ユーザーはすべての目的に対してオプトインされ、Web SDK は次のタスクを実行できます。

* アドビのサーバーとの間でデータを送信する。
* Cookie または web ストレージ項目の読み取りと書き込み。

ユーザーがすべての目的をオプトアウトした場合、Web SDK はこれらのタスクを実行しません。

`defaultConsent` プロパティは、次の 3 つの値をサポートします。

* **`in`**：ユーザーがオプトアウトするまで、データ収集は通常どおり進行します。
* **`out`**: ユーザーがオプトインするまで、データは永続的に破棄されます。
* **`pending`**: ユーザーが [`setConsent`](../setconsent.md) コマンドを使用してオプトインするまで、データはローカルに保存されます。 汎用目的のデフォルトの同意が `pending` に設定されている場合、ユーザーのオプトイン環境設定に依存するコマンド（[`sendEvent`](../sendevent/overview.md) コマンドなど）を実行しようとすると、コマンドが Web SDK のキューに入れられます。 キューに入れられたコマンドは、ユーザーのオプトイン環境設定を Web SDK に伝えるまで処理されません。

>[!NOTE]
>
> 同意データは、ページの読み込み間は保持されません。

EU 一般データ保護規則（GDPR）の管轄権外の訪問者がいる場合、デフォルトの同意を `in` に設定することもできます。 GDPR の管轄権内の訪問者は、デフォルトの同意が `pending` に設定されている場合があります。 同意管理プラットフォーム（CMP）は、顧客の地域を検出し、IAB TCF 2.0 に `gdprApplies` するフラグを提供できます。このフラグは、デフォルトの同意を設定するために使用できます。

ユーザーのオプトイン環境設定が設定される前に発生したイベントを収集しない場合は、Web SDK の設定時に `"defaultConsent": "out"` を渡すことができます。 ユーザーのオプトイン環境設定に依存するコマンドを実行しようとしても、ユーザーのオプトイン環境設定を Web SDK に伝えるまでは効果がありません。

>[!NOTE]
>
>現在、Web SDK は、すべて許可または禁止の単一の目的のみをサポートしています。 アドビでは、様々なAdobe機能や提供される製品に対応した、より堅牢な目的やカテゴリのセットを構築する予定ですが、現在の実装は、オプトインの手段としてすべてを取らないアプローチです。  これは [!DNL Web SDK] にのみ適用され、他のAdobeのJavaScript ライブラリには適用されません。

## `defaultConsent` と `setConsent` の併用 {#using-consent}

Web SDK には、2 つの補完的な同意設定コマンドがあります。

* [`defaultConsent`](defaultconsent.md)：このコマンドは、Web SDK を使用して、Adobeのお客様の同意環境設定を取得することを目的としています。
* [`setConsent`](../setconsent.md)：このコマンドは、サイト訪問者の同意環境設定を取り込むためのものです。

これらの設定を一緒に使用すると、設定された値に応じて、異なるデータ収集および cookie 設定結果になる可能性があります。

同意設定に基づいてデータ収集が発生するタイミングと cookie が設定されるタイミングについて理解するには、次の表を参照してください。

| defaultConsent | setConsent | データ収集が発生 | Web SDK はブラウザー cookie を設定します |
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

同意設定で許可されている場合、次の Cookie が設定されます。

| 名前 | 最大経過年数 | 説明 |
|---|---|---|
| **AMCV_###@AdobeOrg** | 34128000 （395 日） | [`idMigrationEnabled`](../configure/idmigrationenabled.md) が有効な場合に表示されます。 サイトの一部がまだ `visitor.js` を使用している場合に Web SDK に移行すると役立ちます。 |
| **Demdex cookie** | 15552000 （180 日間） | ID 同期が有効な場合に存在します。 Audience Managerは、この cookie を設定して、サイト訪問者に一意の ID を割り当てます。 demdex cookie は、Audience Manager が訪問者識別、ID 同期、セグメント化、モデリング、レポートなどの基本的な機能を実行するのに役立ちます。 |
| **kndctr_orgid_cluster** | 1800 （30 分） | 現在のユーザーのリクエストに対応するEdge Networkリージョンを格納します。 Edge Networkがリクエストを正しいリージョンにルーティングできるように、リージョンは URL パスで使用されます。 ユーザーが別の IP アドレスで接続する場合や、別のセッションで接続する場合は、リクエストは再び最も近いリージョンにルーティングされます。 |
| **kndct_orgid_identity** | 34128000 （395 日） | ECID と、ECID に関連するその他の情報を保存します。 |
| **kndctr_orgid_consent** | 15552000 （180 日間） | Web サイトのユーザー同意設定を格納します。 |
| **s_ecid** | 63115200 （2 年） | Experience CloudID （[!DNL ECID]）または MID のコピーが含まれます。 MID は、`s_ecid=MCMID\|<ECID>` という構文に従うキーと値のペアとして保存されます。 |

## Web SDK タグ拡張機能を使用したデフォルトの同意の設定

[ タグ拡張機能の設定 ](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md) 時に、**[!UICONTROL デフォルトの同意]** の下で目的のラジオボタンを選択します。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL 拡張機能]** に移動し、[!UICONTROL Adobe Experience Platform Web SDK **[!UICONTROL カードの]** 設定 &#x200B;] をクリックします。
1. 「[!UICONTROL &#x200B; プライバシー &#x200B;]」セクションまでスクロールし、目的の **[!UICONTROL デフォルトの同意]** を選択します。
1. 「**[!UICONTROL 保存]**」をクリックして、変更を公開します。

## Web SDK JavaScript ライブラリを使用したデフォルトの同意の設定

`configure` コマンドを実行する際に、`defaultConsent` 文字列プロパティを目的の同意レベルに設定します。 このプロパティでは大文字と小文字が区別され、サポートされる値は `"in"`、`"out"`、`"pending"` の 3 つだけです。 その他の値を使用しようとすると、ライブラリがエラーをスローする。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  defaultConsent: "pending"
});
```
