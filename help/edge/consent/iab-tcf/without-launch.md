---
title: Adobe Experience Platform Web SDK を使用した IAB TCF 2.0 のサポートの統合
description: タグを使用せずに Web サイトの IAB TCF 2.0 サポートを設定する方法を説明します。
seo-description: Learn how to set up IAB TCF 2.0 consent with Adobe Experience Platform Web SDK
exl-id: 14f1802a-0f8d-487f-ae17-5daaaab05162
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '663'
ht-degree: 0%

---

# IAB TCF 2.0 のサポートと Platform Web SDK の統合

このガイドでは、タグを使用せずに、Interactive Advertising Bureau の透明性および同意フレームワーク、バージョン 2.0(IAB TCF 2.0) をAdobe Experience Platform Web SDK と統合する方法を説明します。 IAB TCF 2.0 との統合の概要については、[ 概要 ](./overview.md) を参照してください。 タグとの統合方法に関するガイドについては、[IAB TCF 2.0 のタグ ](./with-launch.md) に関するガイドを参照してください。

## はじめに

このガイドでは、`__tcfapi` インターフェイスを使用して同意情報にアクセスします。 クラウド管理プロバイダー (CMP) と直接統合する方が簡単な場合があります。 ただし、CMP は通常 TCF API と同様の機能を提供するので、このガイドの情報は引き続き役立つ場合があります。

>[!NOTE]
>
>以下の例では、コードが実行されるまでに、`window.__tcfapi` がページで定義されていることを想定しています。 CMP は、`__tcfapi` オブジェクトの準備ができたら、これらの関数を実行できるフックを提供できます。

タグとAdobe Experience Platform Web SDK 拡張機能で IAB TCF 2.0 を使用するには、XDM スキーマを使用できる必要があります。 これらの設定がまだ行われていない場合は、先に進む前にこのページを表示して開始します。

また、このガイドでは、Adobe Experience Platform Web SDK に関する十分な知識が必要です。 簡単なリフレッシャーについては、[Adobe Experience Platform Web SDK の概要 ](../../home.md) および [ よくある質問 ](../../web-sdk-faq.md) のドキュメントを参照してください。

## デフォルトの同意の有効化

不明なユーザーをすべて同じように扱う場合は、デフォルトの同意を `pending` または `out` に設定できます。 この設定は、同意の環境設定を受け取るまで、エクスペリエンスイベントをキューに追加または破棄します。

デフォルトの同意について詳しくは、Platform Web SDK 設定ドキュメントの [ デフォルトの同意に関する節 ](../../fundamentals/configuring-the-sdk.md#default-consent) を参照してください。

### `gdprApplies` に基づくデフォルトの同意の設定

一部の CMP は、EU 一般データ保護規則 (GDPR) がお客様に適用されるかどうかを判断する機能を提供します。 GDPR が適用されないお客様の同意を前提とする場合は、TCF API 呼び出しで `gdprApplies` フラグを使用できます。

次の例は、これをおこなう方法の 1 つを示しています。

```javascript
var alloyConfiguration = { ... };
window.__tcfapi('getTCData', 2, function (tcData, success) {
  if (success) {
    alloyConfiguration.defaultConsent = tcData.gdprApplies ? "pending" : "in";
    window.alloy("configure", alloyConfiguration);
  }
});
```

この例では、`configure` コマンドは、TCF API から `tcData` を取得した後に呼び出されます。 `gdprApplies` が true の場合、デフォルトの同意は `pending` に設定されます。 `gdprApplies` が false の場合、デフォルトの同意は `in` に設定されます。 `alloyConfiguration` 変数に必ず設定を入力してください。

>[!NOTE]
>
>デフォルトの同意が `in` に設定されている場合でも、 `setConsent` コマンドを使用して、顧客の同意設定を記録できます。

## setConsent イベントの使用

IAB TCF 2.0 API は、お客様が同意を更新した場合にイベントを提供します。 これは、顧客が最初に環境設定を行ったときと、顧客が環境設定を更新したときに発生します。

次の例は、これをおこなう方法の 1 つを示しています。

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

このコードブロックは `useractioncomplete` イベントをリッスンし、同意を設定し、コンセントストリングと `gdprApplies` フラグを渡します。 顧客のカスタム ID がある場合は、必ず `identityMap` 変数を入力してください。 `setConsent` の呼び出しの詳細については、[ 同意 ](../../consent/supporting-consent.md) のサポートに関するガイドを参照してください。

## sendEvent に同意情報を含める

XDM スキーマ内で、エクスペリエンスイベントから同意設定情報を保存できます。 この情報を各イベントに追加する方法は 2 つあります。

まず、`sendEvent` 呼び出しのたびに、関連する XDM スキーマを指定できます。 次の例は、これをおこなう方法の 1 つを示しています。

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

この例では、TCF API の同意情報を取得し、同意情報が追加されたイベントを XDM スキーマに送信します。 `sendEvent` コマンドオプションの内容については、[ トラッキングイベント ](../../fundamentals/tracking-events.md) のガイドを参照してください。

すべての要求に同意情報を追加するもう 1 つの方法は、`onBeforeEventSend` コールバックを使用することです。 この方法の詳細については、トラッキングイベントのドキュメント内から [ イベントのグローバルな変更 ](../../fundamentals/tracking-events.md#modifying-events-globally) に関する節を参照してください。

## 次の手順

これで、IAB TCF 2.0 と Platform Web SDK 拡張機能の使用方法が学びました。Adobe Analyticsやリアルタイム顧客データプラットフォームなど、他のAdobeソリューションと統合することもできます。 詳しくは、[IAB Transparency &amp; Consent Framework 2.0 の概要 ](./overview.md) を参照してください。
