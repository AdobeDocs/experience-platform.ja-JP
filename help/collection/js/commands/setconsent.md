---
title: setConsent
description: 各ページでユーザーの同意環境設定を追跡するために使用されます。
exl-id: d01a6ef1-4fa7-4a60-a3a1-19568b4e0d23
source-git-commit: 66105ca19ff1c75f1185b08b70634b7d4a6fd639
workflow-type: tm+mt
source-wordcount: '1117'
ht-degree: 2%

---


# `setConsent`

`setConsent` コマンドは、データを送信（オプトイン）、データを破棄（オプトアウト）または使用（同意は不明）する必要があ [`defaultConsent`](configure/defaultconsent.md) かどうかを Web SDKに指示します。

Web SDKは、次の標準をサポートしています。

* **[Adobe標準](/help/landing/governance-privacy-security/consent/adobe/overview.md)**: 1.0 と 2.0 の両方の標準がサポートされています。
* **[IAB の透明性および同意フレームワーク](/help/landing/governance-privacy-security/consent/iab/overview.md)**：この標準を使用すると、実装が正しく設定されている場合、訪問者のリアルタイム顧客プロファイルが同意情報で更新されます。
   1. XDM 個人プロファイルスキーマには、[IAB TCF 2.0 同意フィールドグループ ](/help/xdm/field-groups/profile/iab.md) が含まれます。
   1. エクスペリエンスイベントスキーマには、[IAB TCF 2.0 同意フィールドグループ ](/help/xdm/field-groups/event/iab.md) が含まれています。
   1. IAB 同意情報をイベント [XDM オブジェクト ](sendevent/xdm.md) に含めます。 Web SDKは、イベントデータを送信する際に、同意情報を自動的に含めません。

このコマンドを使用すると、Web SDKはユーザーの環境設定を [`kndctr_<orgId>_consent`](https://experienceleague.adobe.com/en/docs/core-services/interface/data-collection/cookies/web-sdk) Cookie に書き込みます。 この cookie は訪問者の同意設定を保存するため、その訪問者の同意設定に関係なく設定されます。 ユーザーが次回ブラウザーで web サイトを読み込むときに、SDKはこれらの永続的な環境設定を取得し、イベントをAdobeに送信できるかどうかを決定します。

Adobeでは、同意ダイアログの環境設定を Web SDKの同意とは別に保存することをお勧めします。 Web SDKでは、同意を取得する方法は提供していません。 ユーザーの環境設定とSDKの同期が保たれるように、ページが読み込まれるたびに `setConsent` コマンドを呼び出すことができます。 Web SDKは、同意が変更された場合にのみサーバーコールを行います。

## ID 同期に関する考慮事項 {#identity-considerations}

`setConsent` コマンドはデバイスレベルで動作するので、ID マップの `ECID` のみを使用します。 ID マップ内の他の ID は、`setConsent` コマンドでは考慮されません。

## `defaultConsent` と `setConsent` の併用 {#using-consent}

Web SDKには、2 つの補完的な同意設定コマンドがあります。

* [`defaultConsent`](configure/defaultconsent.md)：このコマンドは、`setConsent` を呼び出す前に、訪問者のデフォルトの同意環境設定を自動的に設定します。
* `setConsent` （現在のページ）：このコマンドは、訪問者の同意環境設定を明示的に設定します。

これらの設定を一緒に使用すると、設定された値に応じて、異なるデータ収集および cookie 設定の結果になる可能性があります。

| `defaultConsent` | `setConsent` | データ収集が発生 | Web SDKはブラウザー cookie を設定します |
| --- | --- | --- | --- |
| `in` | `in` | ○ | ○ |
| `in` | `out` | × | ○ |
| `in` | 設定なし | ○ | ○ |
| `pending` | `in` | ○ | ○ |
| `pending` | `out` | × | ○ |
| `pending` | 設定なし | × | × |
| `out` | `in` | ○ | ○ |
| `out` | `out` | × | ○ |
| `out` | 設定なし | × | × |

設定可能な Cookie の完全なリストについては、コアサービスガイドの [Adobe Experience Platform Web SDKの Cookie](https://experienceleague.adobe.com/en/docs/core-services/interface/data-collection/cookies/web-sdk) を参照してください。

## `setConsent` コマンドの使用

設定済みの Web SDK インスタンスを呼び出す際に、`setConsent` コマンドを実行します。 このコマンドには、次のオブジェクトを含めることができます。

* **`consent[]`**: `consent` オブジェクトの配列。 同意オブジェクトの形式は、選択した標準とバージョンに応じて異なります。 同意標準に応じた、各同意オブジェクトの例については、以下のタブを参照してください。
* **`identityMap`**:ECID の生成方法と、同意情報が関連付けられている ID を制御するオブジェクト。 Adobeでは、`setConsent` などの他のコマンドの前に [`sendEvent`](sendevent/overview.md) を実行する場合、このオブジェクトを含めることをお勧めします。
* **`edgeConfigOverrides`**: [ データストリーム設定の上書き ](configure/edgeconfigoverrides.md) を含むオブジェクト。

>[!BEGINTABS]

>[!TAB Adobe 2.0]

### Adobe 2.0 標準 `consent` オブジェクト

Adobe Experience Platformにデータを送信する場合、プロファイルスキーマにプライバシースキーマフィールドグループを含める必要があります。 Adobe 2.0 標準について詳しくは、[Adobe Experience Platformにおけるガバナンス、プライバシー、セキュリティ ](/help/landing/governance-privacy-security/overview.md) を参照してください。 `consents` プロファイルフィールドグループの [!UICONTROL Consents and Preferences] フィールドのスキーマに対応する、以下の値オブジェクト内にデータを追加できます。

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

イベントで同意情報を送信するには、[!DNL Profile] 対応の [!DNL XDM ExperienceEvent] スキーマに Experience Event Privacy フィールドグループを追加する必要があります。 これを設定する手順については、データセット準備ガイドの [ExperienceEvent スキーマの更新 ](/help/landing/governance-privacy-security/consent/iab/dataset.md#event-schema) に関する節を参照してください。

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

IAB TCF 2.0 API は、顧客が同意を更新した場合にのイベントを提供します。 この問題は、顧客が最初に環境設定を行い、顧客が環境設定を更新した場合に発生します。 イベントリスナーを追加して、`setConsent` のコマンドを実行できます。

```js
const identityMap = { ... };
window.__tcfapi('addEventListener', 2, function (tcData, success) {
  if (success && tcData.eventStatus === 'useractioncomplete') {
    window.alloy("setConsent", {
      identityMap,
      consent: [
        {
          standard: "IAB TCF",
          version: "2.0",
          value: tcData.tcString,
          gdprApplies: tcData.gdprApplies
        }
      ]
    });
  }
});
```

上記のコードブロックは `useractioncomplete` イベントをリッスンして同意を設定し、同意文字列と `gdprApplies` フラグを渡します。 顧客のカスタム ID がある場合は、必ず `identityMap` 変数に入力します。

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
                time: "YYYY-03-17T15:48:42-07:00"
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

`setConsent` コマンドを使用して web SDKにユーザーの環境設定を伝えると、SDKはユーザーの環境設定を Cookie に保持します。 ユーザーが次にブラウザーで web サイトを読み込むときに、Web SDKはこれらの永続化された環境設定を取得し、使用してイベントをAdobeに送信できるかどうかを判断します。

ユーザーの環境設定を個別に保存して、現在の環境設定を使用して同意ダイアログを表示できます。 Web SDKからユーザーの環境設定を取得する方法はありません。 ユーザーの環境設定とSDKの同期が保たれるように、ページが読み込まれるたびに `setConsent` コマンドを呼び出すことができます。 Web SDKは、環境設定が変更された場合にのみサーバーコールを行います。

## Web SDK タグ拡張機能を使用して同意を設定

このコマンドと同等の web SDK タグ拡張機能は、[**[!UICONTROL Set consent]**](/help/tags/extensions/client/web-sdk/actions/set-consent.md) のアクションです。
