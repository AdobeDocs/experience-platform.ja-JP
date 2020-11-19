---
title: IAB TCF 2.0をExperience Platform Launchなしで使用
seo-title: IAB TCF 2.0とAdobe Experience PlatformWeb SDKの同意の設定
description: Adobe Experience PlatformWeb SDKでIAB TCF 2.0の同意を設定する方法を説明します。
seo-description: Adobe Experience PlatformWeb SDKでIAB TCF 2.0の同意を設定する方法を説明します。
translation-type: tm+mt
source-git-commit: 1b5ee9b1f9bdc7835fa8de59020b3eebb4f59505
workflow-type: tm+mt
source-wordcount: '687'
ht-degree: 0%

---


# AEP Web SDK拡張とIAB TCF 2.0の使用

このガイドでは、Experience Platform Launchを使用せずに、Interactive Advertising Bureau Transparency &amp; Consent Frameworkバージョン2.0(IAB TCF 2.0)をAdobe Experience PlatformWeb SDKと統合する方法を示します。 IAB TCF 2.0との統合の概要については、 [概要を参照してください](./overview.md)。 Experience Platform Launchとの統合方法のガイドについては、Experience Platform Launch用 [IAB TCF 2.0ガイドを参照してください](./with-launch.md)。

## はじめに

このガイドでは、同意情報にアクセスするための `__tcfapi` インターフェイスを使用します。 クラウド管理プロバイダー(CMP)との直接統合が容易になる場合があります。 ただし、CMPは通常TCF APIと同様の機能を提供するので、このガイドの情報は役に立つ場合があります。

>[!NOTE]
>
>以下の例では、コードが実行されるまでに、がページで定義されてい `window.__tcfapi` ることを前提としています。 CMPは、オブジェクトの準備ができた時にこれらの機能を実行できるフックを提供でき `__tcfapi` ます。

IAB TCF 2.0をExperience Platform LaunchとAEP Web SDK拡張と共に使用するには、XDMスキーマを使用できる状態にする必要があります。 いずれの設定も行っていない場合は、先に進む前にこのページを表示して開始します。

また、このガイドを使用するには、Adobe Experience PlatformWeb SDKに関する実用的な知識が必要です。 簡単なリフレッシャーについては、 [Adobe Experience PlatformWeb SDKの概要](../../home.md) 、および [よくある質問](../../web-sdk-faq.md) (FAQ)に関するドキュメントをお読みください。

## 既定の同意の有効化

すべての不明なユーザーを同じように扱う場合は、デフォルトの同意をに設定でき `pending`ます。 これは、同意の基本設定を受け取るまで、エクスペリエンスイベントをキューに入れます。

デフォルトの同意について詳しくは、プラットフォームWeb SDK設定ドキュメントの [デフォルトの同意の節](../../fundamentals/configuring-the-sdk.md#default-consent) を参照してください。

### デフォルトの同意の設定 `gdprApplies`

一部のCMPは、GDPR(General Data Protection Regulation)がお客様に適用されるかどうかを判断する機能を提供します。 GDPRが適用されないお客様に同意を求める場合は、TCF API呼び出しで `gdprApplies` フラグを使用できます。

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

この例では、TCF APIからコマンドを取得した後に `configure` コマンド `tcData` を呼び出します。 がtrueの場合、デフォルト `gdprApplies` の同意はに設定され `pending`ます。 がfalse `gdprApplies` の場合、デフォルトの同意はに設定され `in`ます。 必ず、 `alloyConfiguration` 変数に設定を入力してください。

>[!NOTE]
>
>デフォルトの同意がに設定されている場合 `in`も、この `setConsent` コマンドを使用して、顧客の同意を希望する設定を記録できます。

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

このコードブロックは `useractioncomplete` イベントをリッスンし、同意文字列とフ `gdprApplies` ラグを渡して同意を設定します。 顧客のカスタムIDを持っている場合は、必ず `identityMap` 変数を入力してください。 呼び出しの詳細については、 [同意の](../../consent/supporting-consent.md) サポートに関するガイドを参照してくだ `setConsent`さい。

## sendEventに同意情報を含む

XDMスキーマ内では、エクスペリエンスイベントからの同意の環境設定情報を保存できます。 この情報をすべてのイベントに追加する方法は2つあります。

まず、各 `sendEvent` 呼び出しで関連するXDMスキーマを提供できます。 次の例は、これを行う方法の1つを示しています。

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

次の例では、TCF APIの同意情報を取得し、XDMスキーマに追加された同意情報を含むイベントを送信します。 コマンドオプションの内容については、『 [トラッキングイベント](../../fundamentals/tracking-events.md)`sendEvent` 』ガイドを参照してください。

すべてのリクエストに同意情報を追加するもう1つの方法は、 `onBeforeEventSend` コールバックを使用することです。 イベントをグローバルに [変更する方法の詳細については、トラッキングイベントのドキュメントから](../../fundamentals/tracking-events.md#modifying-events-globally) 、のグローバルな変更に関する節を参照してください。

## 次の手順

AEP Web SDK ExtensionでのIAB TCF 2.0の使用方法を学習したので、Adobe Analyticsやリアルタイム顧客データプラットフォームなどの他のAdobeソリューションとの統合も選択できます。 詳細は、 [IAB Transparency &amp; Consent Framework 2.0の概要](./overview.md) を参照してください。
