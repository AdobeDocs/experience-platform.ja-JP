---
title: 同意を設定
description: 訪問者に必要な同意を設定します。
source-git-commit: 8dd658c46fc92ae6d450213ab79f2766f4211c53
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 0%

---

# 同意を設定

**[!UICONTROL Set consent]** のアクションは、タグ拡張機能がデータを送信する（オプトイン）、データを破棄する（オプトアウト）、または使用する [ デフォルトの同意 ](../configure/consent.md) （同意が不明）かどうかを決定します。 ユーザーがサイトで同意を許可または拒否する場合、このアクションを使用して、ユーザーの環境設定をタグ拡張機能と同期できます。 このアクションと同等のJavaScript ライブラリが [`setConsent`](/help/collection/js/commands/setconsent.md) コマンドです。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Rules]** に移動して、目的のルールを選択します。
1. 「[!UICONTROL Actions]」で、既存のアクションを選択するか、アクションを作成します。
1. [!UICONTROL Extension] ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、[!UICONTROL Action type] を **[!UICONTROL Set consent]** に設定します。

タグ拡張機能は、次の標準をサポートしています。

* **[Adobe標準](/help/landing/governance-privacy-security/consent/adobe/overview.md)**: 1.0 と 2.0 の両方の標準がサポートされています。
* **[IAB の透明性および同意フレームワーク](/help/landing/governance-privacy-security/consent/iab/overview.md)**：この標準を使用すると、実装が正しく設定されている場合、訪問者のリアルタイム顧客プロファイルが同意情報で更新されます。
   1. XDM 個人プロファイルスキーマには、[IAB TCF 2.0 同意フィールドグループ ](/help/xdm/field-groups/profile/iab.md) が含まれます。
   1. エクスペリエンスイベントスキーマには、[IAB TCF 2.0 同意フィールドグループ ](/help/xdm/field-groups/event/iab.md) が含まれています。

Adobeでは、同意ダイアログの環境設定をデータ要素など、個別に保存することをお勧めします。 タグ拡張機能には、同意を取得する方法はありません。 ユーザーの環境設定とタグ拡張機能の同期が維持されるように、ページが読み込まれるたびにそのアクションを実行できます。

## 使用可能なフィールド

このアクションタイプは、次の設定オプションをサポートしています。

* **[!UICONTROL Instance]**：アクションが適用されるSDK インスタンス。 実装で 1 つのSDK インスタンスを使用している場合、このドロップダウンメニューは無効になります。
* **[!UICONTROL Identity map]**:ECID の生成方法と、同意情報が関連付けられている ID を制御するデータ要素。
* **[!UICONTROL Consent information]**：フォームに入力するか、同意情報を含むデータ要素を指定するかを決定します。
* **[!UICONTROL Standard]**：使用する同意標準。 利用可能なオプションには&#39;[!UICONTROL Adobe]&#39;と&#39;[!UICONTROL IAB TCF]&#39;があります。
* **[!UICONTROL Version]**：使用する同意標準のバージョン。
* **[!UICONTROL Datastream configuration overrides]**：このコマンドは、データストリーム設定の上書きをサポートし、このデータを受信するアプリやサービスを制御できます。 個々のコマンドとタグ拡張機能設定内の両方でデータストリーム設定の上書きを設定した場合、個々のコマンドが優先されます。 詳しくは、[ データストリーム設定の上書き ](../configure/configuration-overrides.md) を参照してください。

## 同意情報を更新するルールの作成

このアクションを使用する最適なタイミングは、顧客の同意環境設定が変更された場合です。 この変更をリッスンするタグルールを作成できます。

1. タグプロパティ内で、**[!UICONTROL Rules]** に移動し、「**[!UICONTROL Add rule]**」を選択します。
1. ルールに必要な名前を付け、`+` の横にある「**[!UICONTROL Events]**」アイコンを選択します。
1. 左側で次のプロパティを設定します。
   * **[!UICONTROL Extension]**：[!UICONTROL Core]
   * **[!UICONTROL EVent type]**：[!UICONTROL Custom code]
1. 右側のエディターを開き、次のコードをテンプレートとして使用します。

```javascript
// Wait for window.__tcfapi to be defined, then trigger when the customer has completed their consent and preferences.
function addEventListener() {
  if (window.__tcfapi) {
    window.__tcfapi("addEventListener", 2, function (tcData, success) {
      if (success && tcData.eventStatus === "useractioncomplete") {
        // save the tcData.tcString in a data element
        _satellite.setVar("IAB TCF Consent String", tcData.tcString);
        _satellite.setVar("IAB TCF Consent GDPR", tcData.gdprApplies);
        trigger();
      }
    });
  } else {
    // window.__tcfapi wasn't defined. Check again in 100 milliseconds
    setTimeout(addEventListener, 100);
  }
}
addEventListener();
```

1. **[!UICONTROL Keep changes]** を選択します。

上記のカスタムコードブロックでは、次の 2 つを実行します。

* 同意環境設定が変更された場合にルールをトリガーします。
* **IAB TCF 同意文字列** および **IAB TCF 同意 GDPR** の 2 つのデータ要素を設定します。

次のデータ要素は、&#39;[!UICONTROL Set Consent]&#39; アクションを設定するときに役立ちます：

1. `+` の横にある「**[!UICONTROL Actions]**」アイコンを選択します。
1. 左側で次のプロパティを設定します。
   * **[!UICONTROL Extension]**：[!UICONTROL Adobe Experience Platform Web SDK]
   * **[!UICONTROL Action type]**：[!UICONTROL Set consent]
1. 右側で次のプロパティを設定します。
   * **[!UICONTROL Standard]**：[!UICONTROL IAB TCF]
   * **[!UICONTROL Version]**：[!UICONTROL 2.0]
   * **[!UICONTROL Value]**：`%IAB TCF Consent String%`
   * **[!UICONTROL Does GDPR apply to this consent value]**: [!UICONTROL Provide a data element] （値は `%IAB TCF Consent GDPR%`）

![IAB 同意アクションの設定 ](../assets/iab-action.png)

>[!NOTE]
>
>これらのデータ要素はカスタムコードを通じて作成されているので、データ要素セレクターを使用して選択することはできません。 データ要素名にはパーセント記号を付けて入力する必要があります。
