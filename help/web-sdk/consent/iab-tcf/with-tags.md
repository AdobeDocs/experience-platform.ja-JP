---
title: タグとExperience Platform Web SDK拡張機能を使用して、IAB TCF 2.0 のサポートを統合します
description: タグとAdobe Experience Platform Web SDK拡張機能を使用して IAB TCF 2.0 同意を設定する方法について説明します。
exl-id: dc0e6b68-8257-4862-9fc4-50b370ef204f
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '838'
ht-degree: 0%

---

# タグとExperience Platform Web SDK拡張機能を使用して、IAB TCF 2.0 のサポートを統合します

Adobe Experience Platform Web SDKは、Interactive Advertising Bureau Transparency &amp; Consent Framework, version 2.0 （IAB TCF 2.0）をサポートしています。 このガイドでは、Adobe Experience Platform Web SDK Tag Extension を使用して IAB TCF 2.0 同意情報をAdobeに送信するためのタグプロパティを設定する方法について説明します。

タグを使用しない場合は、[&#x200B; タグを使用しない IAB TCF 2.0 の使用 &#x200B;](./without-tags.md) に関するガイドを参照してください。

## はじめに

タグおよびExperience Platform Web SDK拡張機能で IAB TCF 2.0 を使用するには、XDM スキーマとデータセットを使用できる必要があります。

また、このガイドでは、Adobe Experience Platform web SDKについて実際に理解している必要があります。 簡単に復習するには、[Adobe Experience Platform Web SDKの概要 &#x200B;](../../home.md) および [&#x200B; よくある質問 &#x200B;](../../faq.md) ドキュメントを参照してください。

## デフォルトの同意の設定

拡張機能の設定内には、デフォルトの同意に関する設定があります。 これは、同意 Cookie を持たないお客様の行動を制御します。 同意 Cookie を持っていないお客様向けにエクスペリエンスイベントをキューに入れる場合は、これを `pending` に設定します。 同意 Cookie を持たない顧客のエクスペリエンスイベントを破棄する場合は、`out` に設定します。 また、データ要素を使用して、デフォルトの同意値を動的に設定することもできます。 詳細は、[`defaultConsent`](/help/web-sdk/commands/configure/defaultconsent.md) を参照してください。

## 同意情報を使用したプロファイルの更新 {#consent-code-1}

顧客の同意環境設定が変更されたときに [`setConsent`](/help/web-sdk/commands/setconsent.md) アクションを呼び出すには、タグルールを作成します。 まず、新しいイベントを追加し、コア拡張機能の「カスタムコード」イベントタイプを選択します。

新しいイベントに次のコードサンプルを使用します。

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

このカスタムコードでは、次の 2 つを実行します。

* 2 つのデータ要素を設定します。1 つは同意文字列、もう 1 つは `gdprApplies` フラグを含みます。 これは、後で「同意を設定」アクションに入力する際に役立ちます。

* 同意環境設定が変更された場合にルールをトリガーします。 「同意を設定」アクションは、同意環境設定が変更された場合は常に使用する必要があります。 拡張機能に「同意を設定」アクションを追加し、次のフォームに入力します。

* 標準：「IAB TCF」
* バージョン：&quot;2.0&quot;
* 値：「%IAB TCF Consent String%」
* GDPR が適用されます：「%IAB TCF 同意 GDPR%」

![IAB 同意アクションの設定 &#x200B;](../../assets/consent/iab-tcf/with-launch/iab-action.png)

>[!IMPORTANT]
>
>これらのデータ要素はカスタムコードを通じて作成されているので、データ要素セレクターを使用して選択することはできません。 データ要素名にはパーセント記号を付けて入力する必要があります。 このコードは、顧客が変更されるたびに、顧客の新しい同意環境設定を使用して顧客のプロファイルを更新します。 さらに、サーバーが cookie の値を返すことで、Adobe Experience Platform Web SDKがエクスペリエンスイベントを記録できない可能性があります。

## エクスペリエンスイベント用の XDM データ要素の作成

同意文字列を XDM エクスペリエンスイベントに含める必要があります。 それには、XDM オブジェクトデータ要素を使用します。 まず、新しい XDM オブジェクトデータ要素を作成するか、イベント送信のために作成済みの要素を使用します。 エクスペリエンスイベントプライバシースキーマフィールドグループをスキーマに追加した場合は、XDM オブジェクトに `consentStrings` キーが必要です。

1. **[!UICONTROL consentStrings]** を選択します。

1. 「**[!UICONTROL 個別の項目を指定]**」を選択し、「**[!UICONTROL 項目を追加]**」を選択します。

1. **[!UICONTROL consentString]** 見出しを展開し、最初の項目を展開してから、次の値を入力します。

* `consentStandard`: IAB TCF
* `consentStandardVersion`: 2.0
* `consentStringValue`: %IAB TCF 同意文字列 %
* `gdprApplies`: %IAB TCF 同意 GDPR%

>[!IMPORTANT]
>
>これらのデータ要素はカスタムコードを通じて作成されているので、データ要素セレクターを使用して選択することはできません。 データ要素名にはパーセント記号を付けて入力する必要があります。

## IAB TCF 2.0 同意情報を使用した初期エクスペリエンスイベントの送信

ページの最初のエクスペリエンスイベントがページの読み込みイベントでトリガーされた場合、同意文字列がまだ読み込まれていない可能性があります。 このルールは、現在のページ読み込みイベントを置き換えることを目的としています。 最初に同意情報が読み込まれるようにするには、新しいルールを作成し、次のコードをカスタムコードイベントとして追加します。

```javascript
// Wait for window.__tcfapi to be defined, then trigger when there is a consent string
function addEventListener() {
  if (window.__tcfapi) {
    window.__tcfapi("addEventListener", 2, function (tcData, success) {
      if (success && (tcData.eventStatus === "useractioncomplete" || tcData.eventStatus === "tcloaded")) {
        // save the tcData.tcString in a data element
        _satellite.setVar("IAB TCF Consent String", tcData.tcString);
        _satellite.setVar("IAB TCF GDPR Applies", tcData.gdprApplies);
        trigger();
      }
    });
  } else {
    // window.__tcfapi wasn"t defined. Check again in 100 milliseconds
    setTimeout(addEventListener, 100);
  }
}
addEventListener();
```

このコードは、`useractioncomplete` イベントと `tcloaded` イベントの両方が処理される点を除いて、前のカスタムコードと同一です。 [&#x200B; 以前のカスタムコード &#x200B;](#consent-code-1) は、顧客が初めて環境設定を選択する場合にのみトリガーが表示されます。 このコードは、お客様が既に環境設定を選択している場合にもトリガーを表示します。 例えば、2 番目のページの読み込み時です。

Experience Platform Web SDK拡張機能から「イベントを送信」アクションを追加します。 XDM フィールド内で、前の節で作成した XDM データ要素を選択します。

## IAB TCF 2.0 同意情報を使用したその他のイベントの送信

最初のエクスペリエンスイベントの後にイベントがトリガーされた場合でも、2 つのデータ要素は定義されたままであり、IAB 同意情報の送信に使用できます。 同じ XDM データ要素を使用して、今後のイベントを送信します。 IAB TCF 2.0 情報が含まれています。

## 次の手順

これで、IAB TCF 2.0 をExperience Platform Web SDK拡張機能と共に使用する方法を説明したので、次は、Adobe AnalyticsやAdobe Real-Time Customer Data Platformなどの他のAdobe ソリューションと統合することもできます。 詳しくは、[IAB Transparency &amp; Consent Framework 2.0 の概要 &#x200B;](./overview.md) を参照してください。
