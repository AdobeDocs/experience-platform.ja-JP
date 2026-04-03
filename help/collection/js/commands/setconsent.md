---
title: setConsent
description: 各ページでユーザーの同意設定を追跡するために使用されます。
exl-id: d01a6ef1-4fa7-4a60-a3a1-19568b4e0d23
source-git-commit: bf0bb72777cacd822fd6e887ac3ef71764784214
workflow-type: tm+mt
source-wordcount: '1042'
ht-degree: 0%

---


# `setConsent`

`setConsent` コマンドは、データの送信（オプトイン）、データの破棄（オプトアウト）、または[`defaultConsent`](configure/defaultconsent.md)の使用（同意が不明）をWeb SDKに指示します。

Web SDKは、次の標準をサポートしています。

* **[Adobe standard](/help/landing/governance-privacy-security/consent/adobe/overview.md)**: 1.0と2.0の両方の標準がサポートされています。
* **[IABの透明性と同意フレームワーク](/help/landing/governance-privacy-security/consent/iab/overview.md)**：この標準を使用すると、実装が正しく設定されている場合、訪問者のリアルタイム顧客プロファイルが同意情報で更新されます。
   1. XDM個別プロファイルスキーマには、[IAB TCF 2.0同意フィールドグループ &#x200B;](/help/xdm/field-groups/profile/iab.md)が含まれています。
   1. Experience Event スキーマには、[IAB TCF 2.0同意フィールドグループ &#x200B;](/help/xdm/field-groups/event/iab.md)が含まれています。
   1. イベント [XDM オブジェクト &#x200B;](sendevent/xdm.md)にIAB同意情報を含めます。 Web SDKでは、イベントデータの送信時に同意情報が自動的に含まれません。

このコマンドを使用すると、Web SDKはユーザーの環境設定を[`kndctr_<orgId>_consent`](https://experienceleague.adobe.com/ja/docs/core-services/interface/data-collection/cookies/web-sdk) Cookieに書き込みます。 このCookieは、訪問者の同意設定を保存するため、訪問者の同意設定に関係なく設定されます。 ユーザーが次回ブラウザーでweb サイトを読み込むときに、SDKはこれらの永続的環境設定を取得して、イベントをAdobeに送信できるかどうかを判断します。

Adobeでは、同意ダイアログの環境設定をWeb SDKの同意とは別に保存することをお勧めします。 Web SDKでは、同意を取得する方法は提供されていません。 ユーザーの環境設定がSDKと同期するように、ページが読み込まれるたびに`setConsent` コマンドを呼び出すことができます。 Web SDKは、同意が変更されたときにのみサーバーコールを行います。

## ID同期の考慮事項 {#identity-considerations}

`setConsent` コマンドは、ID マップの`ECID`のみを使用します。このコマンドはデバイス レベルで動作します。 ID マップの他のIDは、`setConsent` コマンドでは考慮されません。

## `defaultConsent`と`setConsent`の併用 {#using-consent}

`defaultConsent`と`setConsent`を一緒に使用すると、設定された値に応じて、データ収集、Cookie設定、IDの結果が異なります。 完全なインタラクションテーブルについては、[&#x200B; データ収集](/help/collection/identity/consent.md#how-consent-affects-identity)の同意とIDを参照してください。

## `setConsent` コマンドの使用

Web SDKの設定済みインスタンスを呼び出す際に、`setConsent` コマンドを実行します。 このコマンドには、次のオブジェクトを含めることができます。

* **`consent[]`**: `consent` オブジェクトの配列。 同意オブジェクトの形式は、選択した標準とバージョンによって異なります。 同意標準に応じた各同意オブジェクトの例については、以下のタブを参照してください。
* **`identityMap`**: ECIDの生成方法と同意情報の関連付けを制御するオブジェクト。 Adobeでは、`setConsent`を[`sendEvent`](sendevent/overview.md)などの他のコマンドよりも前に実行する場合は、このオブジェクトを含めることをお勧めします。
* **`edgeConfigOverrides`**: [&#x200B; データストリーム設定オーバーライド &#x200B;](configure/edgeconfigoverrides.md)を含むオブジェクト。

>[!BEGINTABS]

>[!TAB Adobe 2.0]

### Adobe 2.0標準`consent` オブジェクト

Adobe Experience Platformにデータを送信する場合は、プロファイルスキーマにプライバシースキーマフィールドグループを含める必要があります。 Adobe 2.0標準について詳しくは、[Adobe Experience Platformのガバナンス、プライバシー、セキュリティ &#x200B;](/help/landing/governance-privacy-security/overview.md)を参照してください。 `consents` プロファイルフィールドグループの[!UICONTROL Consents and Preferences] フィールドのスキーマに対応する以下の値オブジェクト内にデータを追加できます。

* **`standard`**：選択した同意標準。 Adobe 2.0標準の場合は、このプロパティを`"Adobe"`に設定します。
* **`version`**：同意標準のバージョンを表す文字列。 Adobe 2.0標準の場合は、このプロパティを`"2.0"`に設定します。
* **`value`**：同意値を含むオブジェクト。
   * **`value.collect.val`**：同意値。 ユーザーがオプトインする場合は`"y"`に、ユーザーがオプトアウトする場合は`"n"`に設定します。
   * **`value.metadata.time`**: ユーザーが最後に同意設定を更新した際のタイムスタンプ。

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

### IAB TCF 2.0標準`consent` オブジェクト

Interactive Advertising Bureau Europe （IAB）のTransparency and Consent Framework （TCF）標準を通じて提供されたユーザーの同意設定を記録するには、次に示すように同意文字列を設定します。

このように同意が設定されると、同意情報を使用してリアルタイム顧客プロファイルが更新されます。 これを機能させるには、プロファイル XDM スキーマに[&#x200B; プロファイルプライバシースキーマフィールドグループ &#x200B;](https://github.com/adobe/xdm/blob/master/docs/reference/mixins/profile/profile-privacy.schema.md)を含める必要があります。 イベントを送信する場合、IAB同意情報をイベント XDM オブジェクトに手動で追加する必要があります。 Web SDKでは、同意に関する情報が自動的にイベントに含まれません。

イベントで同意情報を送信するには、[!DNL Profile]対応[!DNL XDM ExperienceEvent] スキーマにExperience Event Privacy フィールドグループを追加する必要があります。 これを設定する手順については、データセット準備ガイドの[ExperienceEvent スキーマの更新](/help/landing/governance-privacy-security/consent/iab/dataset.md#event-schema)の節を参照してください。

* **`standard`**：選択した同意標準。 IAB TCF 2.0標準の場合は、このプロパティを`"IAB TCF"`に設定します。
* **`version`**：同意標準のバージョンを表す文字列。 IAB TCF 2.0標準の場合は、このプロパティを`"2.0"`に設定します。
* **`value`**：同意値を含む文字列。
* **`gdprApplies`**: GDPRがこの同意値に適用されるかどうかを判断するブール値。 デフォルト値は`true`です。
* **`gdprContainsPersonalData`**：このユーザーに関連付けられているイベントデータに個人データが含まれているかどうかを判断するブール値。 デフォルト値は`false`です。

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

IAB TCF 2.0 APIは、お客様が同意を更新する際のイベントを提供します。 これは、顧客が最初に環境設定を設定し、顧客が環境設定を更新したときに発生します。 イベントリスナーを追加して、`setConsent` コマンドを実行できます。

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

上記のコードブロックは、`useractioncomplete` イベントをリッスンしてから同意を設定し、同意文字列と`gdprApplies` フラグを渡します。 顧客のカスタム IDがある場合は、必ず`identityMap`変数に入力してください。

>[!TAB Adobe 1.0]

### Adobe 1.0標準`consent` オブジェクト

* **`standard`**：選択した同意標準。 Adobe 1.0標準の場合は、このプロパティを`"Adobe"`に設定します。
* **`version`**：同意標準のバージョンを表す文字列。 Adobe 1.0標準の場合は、このプロパティを`"1.0"`に設定します。
* **`value.general`**：同意値。 ユーザーがオプトインする場合は`"in"`に、ユーザーがオプトアウトする場合は`"out"`に設定します。

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

### 1つのリクエストで複数の標準を送信する {#multiple-standards}

Web SDKでは、次の例に示すように、リクエストで複数の同意オブジェクトを送信することもサポートしています。

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

`setConsent` コマンドを使用してWeb SDKにユーザーの環境設定を伝えた後、SDKはユーザーの環境設定をCookieに保持します。 次回ユーザーがブラウザーでweb サイトを読み込むときに、Web SDKは、永続化されたこれらの環境設定を取得して使用し、イベントをAdobeに送信できるかどうかを判断します。

ユーザーの環境設定を個別に保存して、現在の環境設定で同意ダイアログを表示できるようにします。 Web SDKからユーザーの環境設定を取得する方法はありません。 ユーザーの環境設定がSDKと同期するように、ページが読み込まれるたびに`setConsent` コマンドを呼び出すことができます。 Web SDKでは、環境設定が変更された場合にのみサーバーコールが実行されます。

## Web SDKのタグ拡張機能を使用して同意を設定する

このコマンドと同等のWeb SDK タグ拡張機能は、[**[!UICONTROL Set consent]**](/help/tags/extensions/client/web-sdk/actions/set-consent.md) アクションです。
