---
title: タグとPlatform Web SDK拡張機能を使用したIAB TCF 2.0のサポートの統合
description: タグとAdobe Experience Platform Web SDK拡張機能を使用してIAB TCF 2.0の同意を設定する方法について説明します。
exl-id: dc0e6b68-8257-4862-9fc4-50b370ef204f
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '838'
ht-degree: 0%

---

# タグとPlatform Web SDK拡張機能を使用したIAB TCF 2.0のサポートの統合

Adobe Experience Platform Web SDKは、Interactive Advertising Bureau Transparency &amp; Consent Frameworkバージョン2.0(IAB TCF 2.0)をサポートしています。 このガイドでは、Adobe Experience Platform Web SDKタグ拡張を使用して、IAB TCF 2.0の同意情報をAdobeに送信するためのタグプロパティを設定する方法について説明します。

タグを使用しない場合は、](./without-launch.md)タグなしでIAB TCF 2.0を使用する[のガイドを参照してください。

## はじめに

IAB TCF 2.0をタグとPlatform Web SDK拡張機能と共に使用するには、XDMスキーマとデータセットを使用できる必要があります。

また、このガイドでは、Adobe Experience Platform Web SDKを実際に使用するうえで理解している必要があります。 簡単なリフレッシャーについては、[Adobe Experience Platform Web SDKの概要](../../home.md)および[よくある質問](../../web-sdk-faq.md)のドキュメントを参照してください。

## デフォルトの同意の設定

拡張機能の設定には、デフォルトの同意の設定があります。 同意Cookieを持たないお客様の動作を制御します。 同意Cookieを持たない顧客のエクスペリエンスイベントをキューに入れる場合は、これを`pending`に設定します。 同意Cookieを持たない顧客のエクスペリエンスイベントを破棄する場合は、これを`out`に設定します。 また、データ要素を使用して、デフォルトの同意値を動的に設定することもできます。

デフォルトの同意の設定方法について詳しくは、SDK設定ガイドの[デフォルトの同意に関する節](../../fundamentals/configuring-the-sdk.md#default-consent)を参照してください。

## 同意情報を使用したプロファイルの更新 {#consent-code-1}

顧客の同意設定が変更されたときに`setConsent`アクションを呼び出すには、新しいタグルールを作成する必要があります。 まず、新しいイベントを追加し、コア拡張機能の「カスタムコード」イベントタイプを選択します。

新しいイベントに対して次のコードサンプルを使用します。

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

このカスタムコードは、次の2つの処理をおこないます。

* 2つのデータ要素を設定します。1つは同意文字列で、もう1つは`gdprApplies`フラグです。 これは、後で「同意の設定」アクションを入力する際に役立ちます。

* トリガー：同意設定が変更された場合のルール 同意設定が変更された場合は、「同意の設定」アクションを使用する必要があります。 拡張機能に「同意の設定」アクションを追加し、次のようにフォームに入力します。

* 標準：&quot;IAB TCF&quot;
* バージョン：&quot;2.0&quot;
* 値：&quot;%IAB TCF Consent String%&quot;
* GDPRの適用：&quot;%IAB TCF Consent GDPR%&quot;

![IAB Set Consent Action](../../images/consent/iab-tcf/with-launch/iab-action.png)

>[!IMPORTANT]
>
>データ要素セレクターを使用してこれらのデータ要素を選択することはできません。これらのデータ要素は、カスタムコードを使用して作成されたものです。 データ要素名にはパーセント記号を付けて入力する必要があります。 このコードは、顧客が変更するたびに、顧客のプロファイルを新しい同意設定で更新します。 さらに、サーバーがcookie値を返す場合、Adobe Experience Platform Web SDKがエクスペリエンスイベントを記録できなくなる可能性があります。

## エクスペリエンスイベント用のXDMデータ要素の作成

XDMエクスペリエンスイベントにコンセントストリングを含める必要があります。 これをおこなうには、XDMオブジェクトデータ要素を使用します。 まず、新しいXDMオブジェクトデータ要素を作成するか、または、イベント送信用に作成済みの要素を使用します。 エクスペリエンスイベントプライバシースキーマフィールドグループをスキーマに追加した場合は、XDMオブジェクトに`consentStrings`キーが必要です。

1. **[!UICONTROL consentStrings]**&#x200B;を選択します。

1. 「**[!UICONTROL 個々の項目を指定]**」を選択し、「**[!UICONTROL 項目を追加]**」を選択します。

1. **[!UICONTROL consentString]**&#x200B;の見出しを展開し、最初の項目を展開して、次の値を入力します。

* `consentStandard`: IAB TCF
* `consentStandardVersion`:2.0
* `consentStringValue`:%IAB TCF Consent String%
* `gdprApplies`:%IAB TCFの同意GDPR%

>[!IMPORTANT]
>
>データ要素セレクターを使用してこれらのデータ要素を選択することはできません。これらのデータ要素は、カスタムコードを使用して作成されたものです。 データ要素名にはパーセント記号を付けて入力する必要があります。

## IAB TCF 2.0の同意情報を使用した初期エクスペリエンスイベントの送信

ページ上の最初のエクスペリエンスイベントがページ読み込みイベントでトリガーされる場合、コンセントストリングがまだ読み込まれていない可能性があります。 このルールは、現在のページ読み込みイベントを置き換えることを目的としています。 同意情報が最初に読み込まれるようにするには、新しいルールを作成し、次のコードをカスタムコードイベントとして追加します。

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

このコードは、`useractioncomplete`イベントと`tcloaded`イベントの両方が処理される点を除いて、以前のカスタムコードと同じです。 [以前のカスタムコード](#consent-code-1)は、顧客が初めて環境設定を選択した場合にのみトリガーします。 このコードは、顧客が既に環境設定をトリガーした場合にも設定されます。 例えば、2番目のページ読み込み時などです。

Platform Web SDK拡張機能から「イベントを送信」アクションを追加します。 XDMフィールド内で、前の節で作成したXDMデータ要素を選択します。

## IAB TCF 2.0の同意情報を使用したその他のイベントの送信

最初のエクスペリエンスイベントの後にイベントがトリガーされた場合でも、2つのデータ要素は引き続き定義され、IAB同意情報の送信に使用できます。 同じXDMデータ要素を使用して、今後のイベントを送信します。 IAB TCF 2.0情報が含まれています。

## 次の手順

これで、IAB TCF 2.0とPlatform Web SDK拡張機能の使用方法が学びました。Adobe Analyticsやリアルタイム顧客データプラットフォームなど、他のAdobeソリューションとの統合も選択できます。 詳しくは、 [IAB Transparency &amp; Consent Framework 2.0の概要](./overview.md)を参照してください。
