---
title: defaultConsent
description: Web プロパティのデフォルトの同意収集方法を設定します。
exl-id: 2a22fa8b-a234-4d3e-9b55-c7482a928fe6
source-git-commit: d3591053939147589dae24e1e4c20d53b1f87dd3
workflow-type: tm+mt
source-wordcount: '799'
ht-degree: 5%

---


# `defaultConsent`

この `defaultConsent` プロパティは、 [`setConsent`](../setconsent.md) コマンド。 このプロパティは、データを収集する前に同意が必要なエリアに住む個人から誤ってデータを収集したくない場合に役立ちます。

デフォルトでは、ユーザーはすべての目的に対してオプトインされ、Web SDK は次のタスクを実行できます。

* アドビのサーバーとの間でデータを送信する。
* Cookie または web ストレージ項目の読み取りと書き込み。

ユーザーがすべての目的をオプトアウトした場合、Web SDK はこれらのタスクを実行しません。

この `defaultConsent` プロパティは、次の 3 つの値をサポートします。

* **`in`**：ユーザーがオプトアウトするまで、データ収集は通常どおり進行します。
* **`out`**：ユーザーがオプトインするまで、データは永続的に破棄されます。
* **`pending`**：ユーザーがを使用することをにオプトインするまで、データはローカルに保存されます [`setConsent`](../setconsent.md) コマンド。 一般的な目的に対する既定の同意が `pending`、ユーザーのオプトイン環境設定に依存するコマンド（例： [`sendEvent`](../sendevent/overview.md) command）を指定すると、コマンドは Web SDK のキューに入れられます。 キューに入れられたコマンドは、ユーザーのオプトイン環境設定を Web SDK に伝えるまで処理されません。

>[!NOTE]
>
> 同意データは、ページの読み込み間は保持されません。

EU 一般データ保護規則（GDPR）の管轄権外の訪問者がいる場合、デフォルトの同意を次のように設定できます。 `in`. GDPR の管轄区域内の訪問者は、デフォルトの同意が次のように設定されている場合があります。 `pending`. 同意管理プラットフォーム（CMP）は、顧客の地域を検出し、フラグを提供できます `gdprApplies` IAB TCF 2.0 にアップグレードします。このフラグは、デフォルトの同意を設定するために使用できます。

ユーザーのオプトイン環境設定が設定される前に発生したイベントを収集しない場合は、を渡すことができます `"defaultConsent": "out"` web SDK の設定中に発生します。 ユーザーのオプトイン環境設定に依存するコマンドを実行しようとしても、ユーザーのオプトイン環境設定を Web SDK に伝えるまでは効果がありません。

>[!NOTE]
>
>現在、Web SDK は、すべて許可または禁止の単一の目的のみをサポートしています。 アドビでは、様々なAdobe機能や提供される製品に対応した、より堅牢な目的やカテゴリのセットを構築する予定ですが、現在の実装は、オプトインの手段としてすべてを取らないアプローチです。  次の場合にのみ適用されます [!DNL Web SDK] 他のAdobeJavaScript ライブラリではありません。

## 使用 `defaultConsent` ～と共に `setConsent` {#using-consent}

Web SDK には、2 つの補完的な同意設定コマンドがあります。

* [`defaultConsent`](defaultconsent.md)：このコマンドは、Web SDK を使用して、Adobeのお客様の同意環境設定を取り込むためのものです。
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
| **AMCV_###@AdobeOrg** | 34128000 （395 日） | 次の場合に存在 [`idMigrationEnabled`](../configure/idmigrationenabled.md) が有効になっています。 サイトの一部がまだを使用している状態で Web SDK に移行する場合に役立ちます `visitor.js`. |
| **Demdex cookie** | 15552000 （180 日間） | ID 同期が有効な場合に存在します。 Audience Managerは、この cookie を設定して、サイト訪問者に一意の ID を割り当てます。 demdex cookie は、Audience Manager が訪問者識別、ID 同期、セグメント化、モデリング、レポートなどの基本的な機能を実行するのに役立ちます。 |
| **kndctr_orgid_cluster** | 1800 （30 分） | 現在のユーザーのリクエストに対応するEdge Networkリージョンを格納します。 Edge Networkがリクエストを正しいリージョンにルーティングできるように、リージョンは URL パスで使用されます。 ユーザーが別の IP アドレスで接続する場合や、別のセッションで接続する場合は、リクエストは再び最も近いリージョンにルーティングされます。 |
| **knd_orgid_identity** | 34128000 （395 日） | ECID と、ECID に関連するその他の情報を保存します。 |
| **kndctr_orgid_consent** | 15552000 （180 日間） | Web サイトのユーザー同意設定を格納します。 |
| **s_ecid** | 63115200 （2 年） | EXPERIENCE CLOUDID （[!DNL ECID]）または MID。 MID は、`s_ecid=MCMID\|<ECID>` という構文に従うキーと値のペアとして保存されます。 |

## Web SDK タグ拡張機能を使用したデフォルトの同意の設定

で目的のラジオボタンを選択します **[!UICONTROL デフォルトの同意]** 条件 [タグ拡張機能の設定](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md).

1. へのログイン [experience.adobe.com](https://experience.adobe.com) Adobe IDの資格情報を使用します。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、 **[!UICONTROL 設定]** 日 [!UICONTROL Adobe Experience Platform Web SDK] カード。
1. にスクロール ダウンします。 [!UICONTROL プライバシー] セクションで、目的のを選択します **[!UICONTROL デフォルトの同意]**.
1. クリック **[!UICONTROL 保存]**&#x200B;を作成してから、変更を公開します。

## Web SDK JavaScript ライブラリを使用したデフォルトの同意の設定

を `defaultConsent` を実行する際に、文字列プロパティを目的の同意レベルに変更する `configure` コマンド。 このプロパティでは大文字と小文字が区別され、次の 3 つの値のみがサポートされます。 `"in"`, `"out"`、および `"pending"`. その他の値を使用しようとすると、ライブラリがエラーをスローする。

```js
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "defaultConsent": "pending"
});
```
