---
title: setConsent
description: 各ページでユーザーの同意を追跡するために使用されます。
source-git-commit: 90493d179e620604337bda96cb3b7f5401ca4a81
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 1%

---

# setConsent

The `setConsent` コマンドは、データを送信する（オプトインする）か、データを破棄する（オプトアウトする）か、 [`defaultConsent`](configure/defaultconsent.md) （同意は不明です）。

Web SDK は、次の標準をサポートしています。

* **[Adobe標準](/help/landing/governance-privacy-security/consent/adobe/overview.md)**:1.0 と 2.0 の両方の標準がサポートされています。
* **[IAB Transparency &amp; Consent Framework](/help/landing/governance-privacy-security/consent/iab/overview.md)**：この標準を使用する場合、実装が正しく設定されている場合、訪問者のリアルタイム顧客プロファイルが同意情報で更新されます。
   1. XDM 個別プロファイルスキーマには、 [IAB TCF 2.0 同意フィールドグループ](/help/xdm/field-groups/profile/iab.md).
   1. エクスペリエンスイベントスキーマには、 [IAB TCF 2.0 同意フィールドグループ](/help/xdm/field-groups/event/iab.md).
   1. イベントに IAB 同意情報を含める [XDM オブジェクト](sendevent/xdm.md). Web SDK は、イベントデータを送信する際に、同意情報を自動的に含めません。

このコマンドを使用した後、Web SDK はユーザーの環境設定を Cookie に書き込みます。 次回、ユーザーが Web サイトをブラウザーに読み込む際に、SDK は、これらの永続的な環境設定を取得し、イベントをAdobeに送信できるかどうかを判断します。

Adobeでは、同意ダイアログの環境設定を Web SDK の同意とは別に保存することをお勧めします。 Web SDK には、同意を取得する方法はありません。 ユーザーの環境設定を SDK と同期させるには、 `setConsent` コマンドを使用して、すべてのページが読み込まれます。 Web SDK は、同意が変更された場合にのみサーバー呼び出しをおこないます。

## Web SDK タグ拡張機能を使用した同意の設定

同意の設定は、Adobe Experience Platform Data Collection Tags インターフェイスのルール内のアクションとして実行されます。

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL ルール]**&#x200B;を選択し、目的のルールを選択します。
1. の下 [!UICONTROL アクション]、既存のアクションを選択するか、アクションを作成します。
1. を設定します。 [!UICONTROL 拡張] ドロップダウンフィールド **[!UICONTROL Adobe Experience Platform Web SDK]**&#x200B;をクリックし、 [!UICONTROL アクションタイプ] から **[!UICONTROL 同意の設定]**.
1. 次のように、右側に目的のフィールドを設定します。 **[!UICONTROL 標準]** および **[!UICONTROL 一般的な同意]**.
1. クリック **[!UICONTROL 変更を保持]**&#x200B;を開き、パブリッシュワークフローを実行します。

このアクションには、複数の同意オブジェクトを含めることができます。

## Web SDK JavaScript ライブラリを使用した同意の設定

を実行します。 `setConsent` コマンドを使用して、Web SDK の設定済みインスタンスを呼び出す際に呼び出すことができます。 このコマンドには、次のオブジェクトを含めることができます。

* **`consent[]`**：の配列 `consent` オブジェクト。 同意オブジェクトの形式は、選択した標準とバージョンに応じて異なります。
* **`identityMap`**:ECID の生成方法と、どの ID に同意情報が関連付けられるかを制御するオブジェクト。 Adobeでは、次の場合にこのオブジェクトを含めることをお勧めします。 `setConsent` は、他のコマンドの前に実行されます。例： [`sendEvent`](sendevent/overview.md).
* **`edgeConfigOverrides`**：を含むオブジェクト。 [datastream 設定の上書き](datastream-overrides.md).

>[!BEGINTABS]

>[!TAB ADOBE2.0]

### Adobe2.0 標準 `consent` object

* **`standard`**：選択する同意標準。 このプロパティをに設定します。 `"Adobe"` (Adobe2.0 標準 )
* **`version`**：同意標準のバージョンを表す文字列。 このプロパティをに設定します。 `"2.0"` (Adobe2.0 標準 )
* **`value`**：同意の値を含むオブジェクト。
   * **`value.collect.val`**：同意の値。 有効な値は次のとおりです。 `"y"` （オプトイン）および `"n"` （オプトアウト）。
   * **`value.metadata.time`**：ユーザーが同意値を設定したタイムスタンプ。

```js
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

### IAB TCF 2.0 標準 `consent` object

* **`standard`**：選択する同意標準。 このプロパティをに設定します。 `"IAB TCF"` （IAB TCF 2.0 標準）
* **`version`**：同意標準のバージョンを表す文字列。 このプロパティをに設定します。 `"2.0"` （IAB TCF 2.0 標準）
* **`value`**：同意値を含む文字列。
* **`gdprApplies`**:GDPR がこの同意の値に適用されるかどうかを決定するブール値。 デフォルト値は `true` です。
* **`gdprContainsPersonalData`**：このユーザーに関連付けられているイベントデータに個人データが含まれているかどうかを決定するブール値です。 デフォルト値は `false` です。

```js
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

>[!TAB ADOBE1.0]

### Adobe1.0 標準 `consent` object

* **`standard`**：選択する同意標準。 このプロパティをに設定します。 `"Adobe"` (Adobe1.0 標準 )
* **`version`**：同意標準のバージョンを表す文字列。 このプロパティをに設定します。 `"1.0"` (Adobe1.0 標準 )
* **`value.general`**：同意の値。 有効な値は次のとおりです。 `"in"` （オプトイン）および `"out"` （オプトアウト）。

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
