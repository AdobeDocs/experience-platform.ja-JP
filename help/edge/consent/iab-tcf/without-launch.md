---
title: Adobe Experience Platform Web SDKを使用したIAB TCF 2.0のサポートの統合
description: タグを使用せずにWebサイトのIAB TCF 2.0サポートを設定する方法を説明します。
seo-description: Adobe Experience Platform Web SDKでIAB TCF 2.0の同意を設定する方法を説明します
exl-id: 14f1802a-0f8d-487f-ae17-5daaaab05162
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '677'
ht-degree: 0%

---

# IAB TCF 2.0のサポートとPlatform Web SDKの統合

このガイドでは、タグを使用せずに、Interactive Advertising Bureau Transparency &amp; Consent Frameworkバージョン2.0(IAB TCF 2.0)をAdobe Experience Platform Web SDKに統合する方法を説明します。 IAB TCF 2.0との統合の概要については、[概要](./overview.md)を参照してください。 タグとの統合方法に関するガイドについては、タグ](./with-launch.md)の[IAB TCF 2.0ガイドを参照してください。

## はじめに

このガイドでは、`__tcfapi`インターフェイスを使用して同意情報にアクセスします。 クラウド管理プロバイダー(CMP)と直接統合する方が簡単な場合があります。 ただし、CMPは通常、TCF APIと同様の機能を提供するので、このガイドの情報は引き続き役立つ場合があります。

>[!NOTE]
>
>以下の例では、コードが実行されるまでに、`window.__tcfapi`がページ上で定義されることを前提としています。 CMPには、`__tcfapi`オブジェクトの準備が整ったら、これらの関数を実行できるフックが用意されています。

IAB TCF 2.0をタグとAdobe Experience Platform Web SDK拡張機能と共に使用するには、XDMスキーマを使用可能にする必要があります。 これらのいずれも設定していない場合は、先に進む前にこのページを表示して開始します。

また、このガイドでは、Adobe Experience Platform Web SDKを実際に使用するうえで理解している必要があります。 簡単なリフレッシャーについては、[Adobe Experience Platform Web SDKの概要](../../home.md)および[よくある質問](../../web-sdk-faq.md)のドキュメントを参照してください。

## デフォルトの同意の有効化

すべての不明なユーザーを同じように扱う場合は、デフォルトの同意を`pending`または`out`に設定できます。 この設定は、同意の環境設定を受け取るまで、エクスペリエンスイベントをキューに追加または破棄します。

デフォルトの同意について詳しくは、Platform Web SDK設定ドキュメントの[デフォルトの同意に関する節](../../fundamentals/configuring-the-sdk.md#default-consent)を参照してください。

### `gdprApplies`に基づくデフォルトの同意の設定

一部のCMPは、EU一般データ保護規則(GDPR)がお客様に適用されるかどうかを判断する機能を提供します。 GDPRが適用されない顧客の同意を前提とする場合は、TCF API呼び出しで`gdprApplies`フラグを使用できます。

次の例は、これをおこなう方法の1つを示しています。

```javascript
var alloyConfiguration = { ... };
window.__tcfapi('getTCData', 2, function (tcData, success) {
  if (success) {
    alloyConfiguration.defaultConsent = tcData.gdprApplies ? "pending" : "in";
    window.alloy("configure", alloyConfiguration);
  }
});
```

この例では、`configure`コマンドは、TCF APIから`tcData`を取得した後に呼び出されます。 `gdprApplies`がtrueの場合、デフォルトの同意は`pending`に設定されます。 `gdprApplies`がfalseの場合、デフォルトの同意は`in`に設定されます。 必ず`alloyConfiguration`変数に設定を入力してください。

>[!NOTE]
>
>デフォルトの同意が`in`に設定されている場合でも、 `setConsent`コマンドを使用して、顧客の同意設定を記録できます。

## setConsentイベントの使用

IAB TCF 2.0 APIは、お客様が同意を更新した場合にイベントを提供します。 これは、顧客が最初に環境設定を設定したときと、顧客が環境設定を更新したときに発生します。

次の例は、これをおこなう方法の1つを示しています。

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

このコードブロックは、`useractioncomplete`イベントをリッスンし、同意文字列と`gdprApplies`フラグを渡して設定します。 顧客のカスタムIDがある場合は、必ず`identityMap`変数を入力してください。 `setConsent`の呼び出しについて詳しくは、[同意](../../consent/supporting-consent.md)のサポートに関するガイドを参照してください。

## sendEventに同意情報を含める

XDMスキーマ内で、エクスペリエンスイベントから同意設定情報を保存できます。 この情報を各イベントに追加する方法は2つあります。

まず、`sendEvent`呼び出しのたびに関連するXDMスキーマを指定できます。 次の例は、これをおこなう方法の1つを示しています。

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

この例では、TCF APIの同意情報を取得し、XDMスキーマに同意情報が追加されたイベントを送信します。 `sendEvent`コマンドオプションの内容については、[トラッキングイベント](../../fundamentals/tracking-events.md)のガイドを参照してください。

すべての要求に同意情報を追加するもう1つの方法は、`onBeforeEventSend`コールバックを使用することです。 この方法の詳細については、トラッキングイベントのドキュメント内の[イベントのグローバルな変更](../../fundamentals/tracking-events.md#modifying-events-globally)に関する節を参照してください。

## 次の手順

これで、IAB TCF 2.0とPlatform Web SDK拡張機能の使用方法が学びました。Adobe Analyticsやリアルタイム顧客データプラットフォームなど、他のAdobeソリューションとの統合も選択できます。 詳しくは、 [IAB Transparency &amp; Consent Framework 2.0の概要](./overview.md)を参照してください。
