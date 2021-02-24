---
title: Adobe Experience PlatformWeb SDKを使用したIAB TCF 2.0のサポートの統合
description: Adobe Experience Platform Launchを使用せずにWebサイトのIAB TCF 2.0サポートを設定する方法を説明します。
seo-description: Adobe Experience PlatformWeb SDKでIAB TCF 2.0の同意を設定する方法を説明します。
translation-type: tm+mt
source-git-commit: 0b9a92f006d1ec151a0bb11c10c607ea9362f729
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 0%

---


# IAB TCF 2.0サポートとプラットフォームWeb SDKの統合

このガイドでは、Experience Platform Launchを使用せずに、Interactive Advertising Bureau Transparency &amp; Consent Frameworkバージョン2.0(IAB TCF 2.0)をAdobe Experience PlatformWeb SDKと統合する方法を示します。 IAB TCF 2.0との統合の概要については、[概要](./overview.md)を参照してください。 Experience Platform Launchとの統合方法のガイドについては、[Experience Platform Launch](./with-launch.md)のIAB TCF 2.0ガイドを参照してください。

## はじめに

このガイドは`__tcfapi`インターフェイスを使用して同意情報にアクセスします。 クラウド管理プロバイダー(CMP)との直接統合が容易になる場合があります。 ただし、CMPは通常TCF APIと同様の機能を提供するので、このガイドの情報は役に立つ場合があります。

>[!NOTE]
>
>以下の例では、コードが実行されるまでに、ページに`window.__tcfapi`が定義されていることを前提としています。 CMPは、`__tcfapi`オブジェクトの準備ができた時に、これらの関数を実行できるフックを提供できます。

IAB TCF 2.0をExperience Platform LaunchとAdobe Experience PlatformWeb SDK拡張と共に使用するには、XDMスキーマを使用できる状態にする必要があります。 いずれの設定も行っていない場合は、先に進む前にこのページを表示して開始します。

また、このガイドを使用するには、Adobe Experience PlatformWeb SDKに関する実用的な知識が必要です。 簡単なリフレッシャーについては、[Adobe Experience PlatformWeb SDKの概要](../../home.md)および[よくある質問](../../web-sdk-faq.md)のドキュメントをお読みください。

## 既定の同意の有効化

すべての不明なユーザーを同じように扱う場合は、デフォルトの同意を`pending`に設定できます。 これは、同意の基本設定を受け取るまで、エクスペリエンスイベントをキューに入れます。

デフォルトの同意について詳しくは、プラットフォームWeb SDK設定ドキュメントの[デフォルトの同意のセクション](../../fundamentals/configuring-the-sdk.md#default-consent)を参照してください。

### `gdprApplies`に基づくデフォルトの同意の設定

一部のCMPは、GDPR(General Data Protection Regulation)がお客様に適用されるかどうかを判断する機能を提供します。 GDPRが適用されないお客様に同意を求めたい場合は、TCF API呼び出しで`gdprApplies`フラグを使用できます。

次の例は、これを行う方法の1つを示しています。

```javascript
var alloyConfiguration = { ... };
window.__tcfapi('getTCData', 2, function (tcData, success) {
  if (success) {
    alloyConfiguration.defaultConsent = tcData.gdprApplies ? "pending" : "in";
    window.alloy("configure", alloyConfiguration);
  }
});
```

この例では、`configure`コマンドは、TCF APIから`tcData`を取得した後に呼び出されます。 `gdprApplies`がtrueの場合、デフォルトの同意は`pending`に設定されます。 `gdprApplies`がfalseの場合、デフォルトの同意は`in`に設定されます。 `alloyConfiguration`変数に設定を必ず入力してください。

>[!NOTE]
>
>デフォルトの同意が`in`に設定されている場合、`setConsent`コマンドを使用して、顧客の同意を希望する設定を記録できます。

## setConsentイベントの使用

IAB TCF 2.0 APIは、お客様が同意を更新した場合にイベントを提供します。 これは、顧客が最初に環境設定を行ったときと、顧客が環境設定を更新したときに発生します。

次の例は、これを行う方法の1つを示しています。

```javascript
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

このコードブロックは`useractioncomplete`イベントをリッスンし、同意文字列と`gdprApplies`フラグを渡して、同意を設定します。 お客様のカスタムIDをお持ちの場合は、`identityMap`変数を必ず入力してください。 `setConsent`を呼び出す方法の詳細については、[サポートする同意](../../consent/supporting-consent.md)のガイドを参照してください。

## sendEventに同意情報を含む

XDMスキーマ内では、エクスペリエンスイベントからの同意の環境設定情報を保存できます。 この情報をすべてのイベントに追加する方法は2つあります。

まず、`sendEvent`呼び出しのたびに関連するXDMスキーマを提供します。 次の例は、これを行う方法の1つを示しています。

```javascript
var sendEventOptions = { ... };
window.__tcfapi('getTCData', 2, function (tcData, success) {
  if (success) {
    sendEventOptions.xdm.consentStrings = [{
      consentStandard: "IAB TCF"
      consentStandardVersion: "2.0"
      consentStringValue: tcData.tcString,
      gdprApplies: tcData.gdprApplies
    }];
    window.alloy("sendEvent", sendEventOptions);
  }
});
```

次の例では、TCF APIの同意情報を取得し、XDMスキーマに追加された同意情報を含むイベントを送信します。 [コマンドオプションの内容については、`sendEvent`トラッキングイベント](../../fundamentals/tracking-events.md)ガイドを参照してください。

すべてのリクエストに同意情報を追加するもう1つの方法は、`onBeforeEventSend`コールバックを使用することです。 この方法の詳細については、トラッキングイベントドキュメント内の[イベントをグローバルに変更する](../../fundamentals/tracking-events.md#modifying-events-globally)の節を参照してください。

## 次の手順

Platform Web SDK ExtensionでのIAB TCF 2.0の使用方法を学習したので、Adobe Analyticsやリアルタイム顧客データプラットフォームなどの他のAdobeソリューションとの統合も選択できます。 詳細については、[IAB Transparency &amp; Consent Framework 2.0の概要](./overview.md)を参照してください。
