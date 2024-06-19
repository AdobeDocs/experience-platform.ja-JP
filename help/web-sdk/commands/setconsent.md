---
title: setConsent
description: 各ページでユーザーの同意環境設定を追跡するために使用されます。
exl-id: d01a6ef1-4fa7-4a60-a3a1-19568b4e0d23
source-git-commit: d3591053939147589dae24e1e4c20d53b1f87dd3
workflow-type: tm+mt
source-wordcount: '1372'
ht-degree: 3%

---


# `setConsent`

この `setConsent` コマンドは、データを送信（オプトイン）、データを破棄（オプトアウト）または使用する必要があるかどうかを Web SDK に指示します [`defaultConsent`](configure/defaultconsent.md) （同意は不明）。

Web SDK は、次の標準をサポートしています。

* **[Adobe標準](/help/landing/governance-privacy-security/consent/adobe/overview.md)**:1.0 と 2.0 の両方の標準がサポートされています。
* **[IAB の透明性および同意フレームワーク](/help/landing/governance-privacy-security/consent/iab/overview.md)**：この標準を使用すると、実装が正しく設定されていれば、訪問者のリアルタイム顧客プロファイルは同意情報で更新されます。
   1. XDM 個人プロファイルスキーマには、次が含まれます [IAB TCF 2.0 同意フィールドグループ](/help/xdm/field-groups/profile/iab.md).
   1. エクスペリエンスイベントスキーマには、が含まれます [IAB TCF 2.0 同意フィールドグループ](/help/xdm/field-groups/event/iab.md).
   1. IAB 同意情報をイベントに含めます [XDM オブジェクト](sendevent/xdm.md). Web SDK は、イベントデータを送信する際に、同意情報を自動的に含めません。

このコマンドを使用すると、Web SDK はユーザーの環境設定を Cookie に書き込みます。 ユーザーが次にブラウザーで web サイトを読み込むときに、SDK はこれらの永続的な環境設定を取得し、イベントをAdobeに送信できるかどうかを判断します。

Adobeでは、Web SDK の同意とは別に、同意ダイアログの環境設定を保存することをお勧めします。 Web SDK には、同意を取得する方法はありません。 ユーザー環境設定と SDK の同期が維持されるようにするには、を呼び出します。 `setConsent` ページを読み込むたびにコマンドを実行します。 Web SDK は、同意が変更された場合にのみサーバー呼び出しを行います。

## 使用 `defaultConsent` ～と共に `setConsent` {#using-consent}

Web SDK には、2 つの補完的な同意設定コマンドがあります。

* [`defaultConsent`](configure/defaultconsent.md)：このコマンドは、Web SDK を使用して、Adobeのお客様の同意環境設定を取り込むためのものです。
* [`setConsent`](setconsent.md)：このコマンドは、サイト訪問者の同意環境設定を取り込むためのものです。

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
| **AMCV_###@AdobeOrg** | 34128000 （395 日） | 次の場合に存在 [`idMigrationEnabled`](configure/idmigrationenabled.md) が有効になっています。 サイトの一部がまだを使用している状態で Web SDK に移行する場合に役立ちます `visitor.js`. |
| **Demdex cookie** | 15552000 （180 日間） | ID 同期が有効な場合に存在します。 Audience Managerは、この cookie を設定して、サイト訪問者に一意の ID を割り当てます。 demdex cookie は、Audience Manager が訪問者識別、ID 同期、セグメント化、モデリング、レポートなどの基本的な機能を実行するのに役立ちます。 |
| **kndctr_orgid_cluster** | 1800 （30 分） | 現在のユーザーのリクエストに対応するEdge Networkリージョンを格納します。 Edge Networkがリクエストを正しいリージョンにルーティングできるように、リージョンは URL パスで使用されます。 ユーザーが別の IP アドレスで接続する場合や、別のセッションで接続する場合は、リクエストは再び最も近いリージョンにルーティングされます。 |
| **knd_orgid_identity** | 34128000 （395 日） | ECID と、ECID に関連するその他の情報を保存します。 |
| **kndctr_orgid_consent** | 15552000 （180 日間） | Web サイトのユーザー同意設定を格納します。 |
| **s_ecid** | 63115200 （2 年） | EXPERIENCE CLOUDID （[!DNL ECID]）または MID。 MID は、`s_ecid=MCMID\|<ECID>` という構文に従うキーと値のペアとして保存されます。 |

## Web SDK タグ拡張機能を使用した同意の設定

同意の設定は、Adobe Experience Platform Data Collection タグインターフェイスのルール内のアクションとして実行されます。

1. へのログイン [experience.adobe.com](https://experience.adobe.com) Adobe IDの資格情報を使用します。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL ルール]**&#x200B;を選択してから、目的のルールを選択します。
1. 次の下 [!UICONTROL アクション]、既存のアクションを選択するか、アクションを作成します。
1. を [!UICONTROL 拡張機能] ドロップダウンフィールドの移動先 **[!UICONTROL Adobe Experience Platform Web SDK]**、を設定します。 [!UICONTROL アクションタイプ] 対象： **[!UICONTROL 同意を設定]**.
1. 以下を含め、右側に目的のフィールドを設定します **[!UICONTROL 標準]** および **[!UICONTROL 一般的な同意]**.
1. クリック **[!UICONTROL 変更を保持]**&#x200B;次に、公開ワークフローを実行します。

このアクション内に複数の同意オブジェクトを含めることができます。

## Web SDK JavaScript ライブラリを使用して同意を設定

を実行 `setConsent` コマンドは、設定した Web SDK のインスタンスを呼び出す際に使用します。 このコマンドには、次のオブジェクトを含めることができます。

* **`consent[]`**：の配列 `consent` オブジェクト。 同意オブジェクトの形式は、選択した標準とバージョンに応じて異なります。 同意標準に応じた、各同意オブジェクトの例については、以下のタブを参照してください。
* **`identityMap`**:ECID の生成方法と、同意情報が関連付けられている ID を制御するオブジェクト。 Adobeでは、次の場合にこのオブジェクトを含めることをお勧めします `setConsent` 次のような他のコマンドの前に実行されます [`sendEvent`](sendevent/overview.md).
* **`edgeConfigOverrides`**：を含むオブジェクト [データストリーム設定の上書き](datastream-overrides.md).

>[!BEGINTABS]

>[!TAB Adobe 2.0]

### Adobe 2.0 標準 `consent` オブジェクト

Adobe Experience Platformを使用している場合は、プロファイルスキーマにプライバシースキーマフィールドグループを含める必要があります。 参照： [Adobe Experience Platformにおけるガバナンス、プライバシー、セキュリティ](../../landing/governance-privacy-security/overview.md) Adobe 2.0 標準の詳細については、こちらを参照してください。 のスキーマに対応する以下の値オブジェクト内にデータを追加できます `consents` のフィールド [!UICONTROL 同意および環境設定] プロファイルフィールドグループ。

* **`standard`**：選択する同意標準。 このプロパティをに設定 `"Adobe"` Adobe 2.0 標準の場合。
* **`version`**：同意標準のバージョンを表す文字列。 このプロパティをに設定 `"2.0"` Adobe 2.0 標準の場合。
* **`value`**：同意値を含むオブジェクト。
   * **`value.collect.val`**：同意値。 これを次に設定 `"y"` ユーザーがおよびをオプトインする場合 `"n"` ユーザーがオプトアウトする場合。
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

この方法で同意が設定されると、リアルタイム顧客プロファイルが同意情報で更新されます。 これを機能させるには、プロファイル XDM スキーマにを含める必要があります [プロファイルプライバシースキーマフィールドグループ](https://github.com/adobe/xdm/blob/master/docs/reference/mixins/profile/profile-privacy.schema.md). イベントを送信する場合、IAB 同意情報をイベント XDM オブジェクトに手動で追加する必要があります。 Web SDK は、イベントに同意情報を自動的に含めません。

イベントで同意情報を送信するには、Experience Event Privacy フィールドグループをユーザーに追加する必要があります [!DNL Profile]-enabled [!DNL XDM ExperienceEvent] スキーマ。 の節を参照してください。 [experienceEvent スキーマの更新](../../landing/governance-privacy-security/consent/iab/dataset.md#event-schema) 設定方法の手順については、データセット準備ガイドを参照してください。

* **`standard`**：選択する同意標準。 このプロパティをに設定 `"IAB TCF"` （IAB TCF 2.0 標準用）
* **`version`**：同意標準のバージョンを表す文字列。 このプロパティをに設定 `"2.0"` （IAB TCF 2.0 標準用）
* **`value`**：同意値を含む文字列。
* **`gdprApplies`**:GDPR がこの同意値に適用されるかどうかを決定するブール値。 デフォルト値はです `true`.
* **`gdprContainsPersonalData`**：このユーザーに関連付けられたイベントデータに個人データが含まれているかどうかを決定するブール値。 デフォルト値はです `false`.

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

* **`standard`**：選択する同意標準。 このプロパティをに設定 `"Adobe"` Adobe 1.0 標準の場合。
* **`version`**：同意標準のバージョンを表す文字列。 このプロパティをに設定 `"1.0"` Adobe 1.0 標準の場合。
* **`value.general`**：同意値。 これを次に設定 `"in"` ユーザーがおよびをオプトインする場合 `"out"` ユーザーがオプトアウトする場合。

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

また、Web SDK は、次の例に示すように、リクエストでの複数の同意オブジェクトの送信もサポートしています。

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

を使用して、ユーザーの環境設定を Web SDK に伝えた後、 `setConsent` コマンドを使用する場合、SDK はユーザーの環境設定を cookie に保持します。 ユーザーが次回ブラウザーで web サイトを読み込むと、Web SDK はこれらの永続化された環境設定を取得し、使用して、イベントをAdobeに送信できるかどうかを決定します。

現在の環境設定で同意ダイアログを表示するには、ユーザーの環境設定を個別に保存する必要があります。 Web SDK からユーザー環境設定を取得する方法はありません。 ユーザー環境設定と SDK の同期が維持されるようにするには、を呼び出します。 `setConsent` ページを読み込むたびにコマンドを実行します。 Web SDK は、環境設定が変更された場合にのみ、サーバー呼び出しを行います。

## 同意の設定中の ID の同期 {#sync-identities}

デフォルトの同意（ [defaultConsent](configure/defaultconsent.md) パラメーター）がに設定されています。 `pending` または `out`, `setConsent` 設定は、最初に送信されるリクエストで、ID を確立する場合があります。 このため、最初のリクエストで ID を同期することが重要な場合があります。 ID マップをに追加できます `setConsent` 上と同じようにコマンド `sendEvent` コマンド。 参照： [identityMap の使用](../identity/overview.md#using-identitymap) コマンドに ID マップを含める方法の例は、次のとおりです。
