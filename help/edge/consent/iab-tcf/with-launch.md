---
title: platform launchとプラットフォームWeb SDK Extensionを使用したIAB TCF 2.0サポートの統合
description: IAB TCF 2.0の同意を、Adobe Experience Platform LaunchおよびAdobe Experience PlatformWeb SDK拡張と共に設定する方法について説明します。
translation-type: tm+mt
source-git-commit: 1a51ce92eb5c41ff65ebcf4c652640dd0782487f
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---


# platform launchとプラットフォームWeb SDK拡張を使用してIAB TCF 2.0サポートを統合

Adobe Experience PlatformWeb SDKは、Interactive Advertising Bureau Transparency &amp; Consent Frameworkバージョン2.0(IAB TCF 2.0)をサポートしています。 このガイドでは、Experience Platform Launch用のAdobe Experience PlatformWeb SDK拡張を使用して、IAB TCF 2.0の同意情報をAdobeに送信するためのAdobe Experience Platform Launchプロパティを設定する方法を説明します。

Experience Platform Launchを使用したくない場合は、[Experience Platform Launch](./without-launch.md)を使用せずにIAB TCF 2.0を使用する方法のガイドを参照してください。

## はじめに

IAB TCF 2.0をExperience Platform LaunchとPlatform Web SDK拡張と共に使用するには、XDMスキーマとデータセットを利用できる状態にする必要があります。

また、このガイドを使用するには、Adobe Experience PlatformWeb SDKに関する実用的な知識が必要です。 簡単なリフレッシャーについては、[Adobe Experience PlatformWeb SDKの概要](../../home.md)および[よくある質問](../../web-sdk-faq.md)のドキュメントをお読みください。

## 既定の同意の設定

拡張機能の設定には、デフォルトの同意の設定があります。 これにより、同意Cookieを持たない顧客の動作が制御されます。 同意Cookieを持たない顧客のエクスペリエンスイベントをキューに入れる場合は、`pending`に設定します。 また、データ要素を使用して、デフォルトの同意値を動的に設定することもできます。

デフォルトの同意の設定方法について詳しくは、SDK設定ガイドの[デフォルトの同意のセクション](../../fundamentals/configuring-the-sdk.md#default-consent)を参照してください。

## 同意情報{#consent-code-1}を使用してプロファイルを更新中

顧客の同意の環境設定が変更されたときに`setConsent`アクションを呼び出すには、新しいExperience Platform Launchルールを作成する必要があります。 開始するには、新しいイベントを追加し、コア拡張機能の「カスタムコード」イベントタイプを選択します。

新しいイベントには、次のコードサンプルを使用します。

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

このカスタムコードは2つの処理を行います。

* 2つのデータ要素を設定します。1つは同意文字列、もう1つは`gdprApplies`フラグを持ちます。 これは、後で「同意を設定」アクションに入力する場合に役立ちます。

* 同意の環境設定が変更された場合にルールをトリガーします。 「同意の設定」アクションは、同意の環境設定が変更された場合に必ず使用する必要があります。 内追加部に「同意を設定」アクションを挿入し、次のようにフォームに入力します。

* 標準：&quot;IAB TCF&quot;
* バージョン：&quot;2.0&quot;
* 値：&quot;%IAB TCF同意文字列%&quot;
* GDPR適用：&quot;%IAB TCF Consent GDPR%&quot;

![IABの同意の設定](../../images/consent/iab-tcf/with-launch/iab-action.png)

>[!IMPORTANT]
>
>これらのデータ要素は、カスタムコードを使用して作成されたものなので、データ要素セレクターを使用して選択することはできません。 データ要素名にはパーセント記号を付けて入力する必要があります。 このコードは、顧客が変更を加えるたびに、顧客のプロファイルを新しい同意プリファレンスで更新します。 また、サーバーがcookieの値を返すので、Adobe Experience PlatformWeb SDKがエクスペリエンスイベントを記録できない可能性があります。

## エクスペリエンスイベント用のXDMデータ要素の作成

同意文字列は、XDMエクスペリエンスイベントに含める必要があります。 これを行うには、XDM Objectデータ要素を使用します。 新しいXDMオブジェクトデータ要素を作成して開始するか、既に作成したイベント送信用のXDMオブジェクト要素を使用します。 スキーマにエクスペリエンスイベントのプライバシーミックスインを追加した場合は、XDMオブジェクトに`consentStrings`キーが必要です。

1. **[!UICONTROL consentStrings]**&#x200B;を選択します。

1. 「**[!UICONTROL 個々の項目を指定]**」を選択し、「**[!UICONTROL 追加項目]**」を選択します。

1. **[!UICONTROL consentString]**&#x200B;見出しを展開し、最初の項目を展開して、次の値を入力します。

* `consentStandard`:IAB TCF
* `consentStandardVersion`:2.0
* `consentStringValue`:%IAB TCF同意文字列%
* `gdprApplies`:%IAB TCF Consent GDPR%

>[!IMPORTANT]
>
>これらのデータ要素は、カスタムコードを使用して作成されたものなので、データ要素セレクターを使用して選択することはできません。 データ要素名にはパーセント記号を付けて入力する必要があります。

## IAB TCF 2.0の同意情報を含む初期エクスペリエンスイベントの送信

ページ上の最初のエクスペリエンスイベントがページ読み込みイベントでトリガーされた場合、同意文字列がまだ読み込まれていない可能性があります。 このルールは、現在のページ読み込みイベントを置き換えることを目的としています。 同意情報が最初に読み込まれるようにするには、新しいルールを作成し、次のコードをカスタムコードイベントとして追加します。

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

このコードは、`useractioncomplete`と`tcloaded`の両方のイベントが処理される点を除いて、前のカスタムコードと同じです。 [以前のカスタムコード](#consent-code-1)は、お客様が初めて好みを選択した場合にのみトリガーします。 このコードは、ユーザーが既に環境設定を選択している場合にもトリガーします。 例えば、2回目のページ読み込み時など。

Platform Web SDK 追加 Extensionから「イベントを送信」アクションを送信します。 XDMフィールド内で、前の節で作成したXDMデータ要素を選択します。

## IAB TCF 2.0同意情報を使用した他のイベントの送信

最初のエクスペリエンスイベントの後にイベントがトリガーされると、2つのデータ要素は引き続き定義され、IABの同意情報の送信に使用できます。 同じXDMデータ要素を使用して、今後のイベントを送信します。 IAB TCF 2.0情報が含まれます。

## 次の手順

Platform Web SDK ExtensionでのIAB TCF 2.0の使用方法を学習したので、Adobe Analyticsやリアルタイム顧客データプラットフォームなどの他のAdobeソリューションとの統合も選択できます。 詳細については、[IAB Transparency &amp; Consent Framework 2.0の概要](./overview.md)を参照してください。
