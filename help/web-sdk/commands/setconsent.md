---
title: setConsent
description: 各ページでユーザーの同意環境設定を追跡するために使用されます。
exl-id: d01a6ef1-4fa7-4a60-a3a1-19568b4e0d23
source-git-commit: 83b4745693749c5f50791d6efeb3a7ba02a4cce5
workflow-type: tm+mt
source-wordcount: '1330'
ht-degree: 3%

---


# `setConsent`

`setConsent` コマンドは、データを送信（オプトイン）、データを破棄（オプトアウト）または使用（同意は不明）する必要があ [`defaultConsent`](configure/defaultconsent.md) かどうかを Web SDKに指示します。

Web SDKは、次の標準をサポートしています。

* **[Adobe標準](/help/landing/governance-privacy-security/consent/adobe/overview.md)**: 1.0 と 2.0 の両方の標準がサポートされています。
* **[IAB の透明性および同意フレームワーク](/help/landing/governance-privacy-security/consent/iab/overview.md)**：この標準を使用すると、実装が正しく設定されている場合、訪問者のリアルタイム顧客プロファイルが同意情報で更新されます。
   1. XDM 個人プロファイルスキーマには、[IAB TCF 2.0 同意フィールドグループ ](/help/xdm/field-groups/profile/iab.md) が含まれます。
   1. エクスペリエンスイベントスキーマには、[IAB TCF 2.0 同意フィールドグループ ](/help/xdm/field-groups/event/iab.md) が含まれています。
   1. IAB 同意情報をイベント [XDM オブジェクト ](sendevent/xdm.md) に含めます。 Web SDKは、イベントデータを送信する際に、同意情報を自動的に含めません。

このコマンドを使用すると、Web SDKはユーザーの環境設定を Cookie に書き込みます。 ユーザーが次回ブラウザーで web サイトを読み込むときに、SDKはこれらの永続的な環境設定を取得し、イベントをAdobeに送信できるかどうかを決定します。

Adobeでは、同意ダイアログの環境設定を Web SDKの同意とは別に保存することをお勧めします。 Web SDKでは、同意を取得する方法は提供していません。 ユーザーの環境設定とSDKの同期が保たれるように、ページが読み込まれるたびに `setConsent` コマンドを呼び出すことができます。 Web SDKは、同意が変更された場合にのみサーバーコールを行います。

## ID 同期に関する考慮事項 {#identity-considerations}

`setConsent` コマンドはデバイスレベルで動作するので、ID マップの `ECID` のみを使用します。 ID マップ内の他の ID は、`setConsent` コマンドでは考慮されません。

## `defaultConsent` と `setConsent` の併用 {#using-consent}

Web SDKには、2 つの補完的な同意設定コマンドがあります。

* [`defaultConsent`](configure/defaultconsent.md)：このコマンドは、Web SDKを使用してAdobeのお客様の同意環境設定を取り込むためのものです。
* [`setConsent`](setconsent.md)：このコマンドは、サイト訪問者の同意環境設定を取り込むためのものです。

これらの設定を一緒に使用すると、設定された値に応じて、異なるデータ収集および cookie 設定結果になる可能性があります。

同意設定に基づいてデータ収集が発生するタイミングと cookie が設定されるタイミングについて理解するには、次の表を参照してください。

| defaultConsent | setConsent | データ収集が発生 | Web SDKはブラウザー cookie を設定します |
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
| **AMCV_###@AdobeOrg** | 34128000 （395 日） | [`idMigrationEnabled`](configure/idmigrationenabled.md) が有効な場合に表示されます。 これは、サイトの一部がまだ `visitor.js` を使用している間に web SDKに移行する場合に役立ちます。 |
| **Demdex cookie** | 15552000 （180 日間） | ID 同期が有効な場合に存在します。 Audience Managerは、この cookie を設定してサイト訪問者に一意の ID を割り当てます。 demdex cookie は、Audience Manager が訪問者識別、ID 同期、セグメント化、モデリング、レポートなどの基本的な機能を実行するのに役立ちます。 |
| **kndctr_orgid_cluster** | 1800 （30 分） | 現在のユーザーのリクエストに対応するEdge Network リージョンを格納します。 Edge Networkでリクエストを正しいリージョンにルーティングできるように、リージョンは URL パスで使用されます。 ユーザーが別の IP アドレスで接続する場合や、別のセッションで接続する場合は、リクエストは再び最も近いリージョンにルーティングされます。 |
| **kndct_orgid_identity** | 34128000 （395 日） | ECID と、ECID に関連するその他の情報を保存します。 |
| **kndctr_orgid_consent** | 15552000 （180 日間） | Web サイトのユーザー同意設定を格納します。 |
| **s_ecid** | 63115200 （2 年） | Experience Cloud ID （[!DNL ECID]）または MID のコピーが含まれます。 MID は、`s_ecid=MCMID\|<ECID>` という構文に従うキーと値のペアとして保存されます。 |

## Web SDK タグ拡張機能を使用して同意を設定

同意の設定は、Adobe Experience Platform Data Collection タグインターフェイスのルール内のアクションとして実行されます。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL ルール]** に移動し、目的のルールを選択します。
1. [!UICONTROL  アクション ] で、既存のアクションを選択するか、アクションを作成します。
1. [!UICONTROL  拡張機能 ] ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、[!UICONTROL  アクションタイプ ] を **[!UICONTROL 同意を設定]** に設定します。
1. **[!UICONTROL 標準]** および **[!UICONTROL 一般同意]** を含む、右側の目的のフィールドを設定します。
1. 「**[!UICONTROL 変更を保持]**」をクリックして、公開ワークフローを実行します。

このアクション内に複数の同意オブジェクトを含めることができます。

## Web SDK JavaScript ライブラリを使用して同意を設定

設定済みの Web SDK インスタンスを呼び出す際に、`setConsent` コマンドを実行します。 このコマンドには、次のオブジェクトを含めることができます。

* **`consent[]`**: `consent` オブジェクトの配列。 同意オブジェクトの形式は、選択した標準とバージョンに応じて異なります。 同意標準に応じた、各同意オブジェクトの例については、以下のタブを参照してください。
* **`identityMap`**:ECID の生成方法と、同意情報が関連付けられている ID を制御するオブジェクト。 Adobeでは、[`sendEvent`](sendevent/overview.md) などの他のコマンドの前に `setConsent` を実行する場合、このオブジェクトを含めることをお勧めします。
* **`edgeConfigOverrides`**: [ データストリーム設定の上書き ](datastream-overrides.md) を含むオブジェクト。

>[!BEGINTABS]

>[!TAB Adobe 2.0]

### Adobe 2.0 標準 `consent` オブジェクト

Adobe Experience Platformを使用している場合は、プロファイルスキーマにプライバシースキーマフィールドグループを含める必要があります。 Adobe 2.0 標準について詳しくは、[Adobe Experience Platformにおけるガバナンス、プライバシー、セキュリティ ](../../landing/governance-privacy-security/overview.md) を参照してください。 [!UICONTROL  同意および環境設定 ] プロファイルフィールドグループの `consents` フィールドのスキーマに対応する、以下の値オブジェクト内にデータを追加できます。

* **`standard`**：選択する同意標準。 このプロパティをAdobe 2.0 標準の `"Adobe"` に設定します。
* **`version`**：同意標準のバージョンを表す文字列。 このプロパティをAdobe 2.0 標準の `"2.0"` に設定します。
* **`value`**：同意値を含むオブジェクト。
   * **`value.collect.val`**：同意値。 これを「ユーザーがオプトインする際に `"y"`」および「ユーザーがオプトアウトする際に `"n"`」に設定します。
   * **`value.metadata.time`**：ユーザーが同意設定を最後に更新した際のタイムスタンプ。

```js
// Set consent using the Adobe 2.0 standard
alloy("setConsent", {
  "consent": [{
    "standard": "Adobe",
    "version": "2.0",
    "value": {
      "collect": {
        "val": "y"
      },
      "metadata": {
        "time": "YYYY-03-17T15:48:42-07:00"
      }
    }
  }]
});
```

>[!TAB IAB TCF 2.0]

### IAB TCF 2.0 標準 `consent` オブジェクト

Interactive Advertising Bureau Europe （IAB）の Transparency and Consent Framework （TCF）標準で提供されるユーザー同意環境設定を記録するには、以下に示すように同意文字列を設定します。

この方法で同意が設定されると、リアルタイム顧客プロファイルが同意情報で更新されます。 これを機能させるには、プロファイル XDM スキーマに [ プロファイルプライバシースキーマフィールドグループ ](https://github.com/adobe/xdm/blob/master/docs/reference/mixins/profile/profile-privacy.schema.md) を含める必要があります。 イベントを送信する場合、IAB 同意情報をイベント XDM オブジェクトに手動で追加する必要があります。 Web SDKは、イベントに同意情報を自動的に含めません。

イベントで同意情報を送信するには、[!DNL Profile] 対応の [!DNL XDM ExperienceEvent] スキーマに Experience Event Privacy フィールドグループを追加する必要があります。 これを設定する手順については、データセット準備ガイドの [ExperienceEvent スキーマの更新 ](../../landing/governance-privacy-security/consent/iab/dataset.md#event-schema) に関する節を参照してください。

* **`standard`**：選択する同意標準。 IAB TCF 2.0 標準の場合、このプロパティを `"IAB TCF"` に設定します。
* **`version`**：同意標準のバージョンを表す文字列。 IAB TCF 2.0 標準の場合、このプロパティを `"2.0"` に設定します。
* **`value`**：同意値を含む文字列。
* **`gdprApplies`**：この同意値に GDPR が適用されるかどうかを決定するブール値。 デフォルト値は `true` です。
* **`gdprContainsPersonalData`**：このユーザーに関連付けられたイベントデータに個人データが含まれているかどうかを判断するブール値。 デフォルト値は `false` です。

```js
// Set consent using the IAB TCF 2.0 standard
alloy("setConsent", {
  consent: [{
    "standard": "IAB TCF",
    "version": "2.0",
    "value": "CO052l-O052l-DGAMBFRACBgAIBAAAAABIYgEawAQEagAAAA",
    "gdprApplies": true,
    "gdprContainsPersonalData": true
  }]
});
```

>[!TAB Adobe 1.0]

### Adobe 1.0 標準 `consent` オブジェクト

* **`standard`**：選択する同意標準。 Adobe 1.0 標準の場合、このプロパティを `"Adobe"` に設定します。
* **`version`**：同意標準のバージョンを表す文字列。 Adobe 1.0 標準の場合、このプロパティを `"1.0"` に設定します。
* **`value.general`**：同意値。 これを「ユーザーがオプトインする際に `"in"`」および「ユーザーがオプトアウトする際に `"out"`」に設定します。

```js
// Set consent using the Adobe 1.0 standard
alloy("setConsent", {
  "consent": [{
    "standard": "Adobe",
    "version": "1.0",
    "value": {
      "general": "in"
    }
  }]
});
```

>[!ENDTABS]

### 1 回の要求で複数の規格を送信する {#multiple-standards}

また、Web SDKは、次の例に示すように、リクエストでの複数の同意オブジェクトの送信もサポートしています。

```js
alloy("setConsent", {
    consent: [{
        standard: "Adobe",
        version: "2.0",
        value: {
            collect: {
                val: "y"
            },
            metadata: {
                time: "2021-03-17T15:48:42-07:00"
            }
        }
    }, {
        standard: "IAB TCF",
        version: "2.0",
        value: "CO1Z4yuO1Z4yuAcABBENArCsAP_AAH_AACiQGCNX_T5eb2vj-3Zdt_tkaYwf55y3o-wzhhaIse8NwIeH7BoGP2MwvBX4JiQCGBAkkiKBAQdtHGhcCQABgIhRiTKMYk2MjzNKJLJAilsbe0NYCD9mnsHT3ZCY70--u__7P3fAwQgkwVLwCRIWwgJJs0ohTABCOICpBwCUEIQEClhoACAnYFAR6gAAAIDAACAAAAEEEBAIABAAAkIgAAAEBAKACIBAACAEaAhAARIEAsAJEgCAAVA0JACKIIQBCDgwCjlACAoAAAAA.YAAAAAAAAAAA",
        gdprApplies: true
    }]
});
```

## 同意設定の保持 {#persistence}

`setConsent` コマンドを使用して web SDKにユーザーの環境設定を伝えると、SDKはユーザーの環境設定を Cookie に保持します。 ユーザーが次にブラウザーで web サイトを読み込むとき、Web SDKはこれらの永続化された環境設定を取得し、使用して、イベントをAdobeに送信できるかどうかを判断します。

現在の環境設定で同意ダイアログを表示するには、ユーザーの環境設定を個別に保存する必要があります。 Web SDKからユーザーの環境設定を取得する方法はありません。 ユーザーの環境設定とSDKの同期が保たれるように、ページが読み込まれるたびに `setConsent` コマンドを呼び出すことができます。 Web SDKは、環境設定が変更された場合にのみ、サーバー呼び出しを行います。
