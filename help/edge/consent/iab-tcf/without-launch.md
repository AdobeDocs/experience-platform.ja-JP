---
title: Adobe Experience Platform Web SDK を使用した IAB TCF 2.0 の統合
description: タグを使用せずに Web サイトの IAB TCF 2.0 サポートを設定する方法を説明します。
seo-description: Learn how to set up IAB TCF 2.0 consent with Adobe Experience Platform Web SDK
exl-id: 14f1802a-0f8d-487f-ae17-5daaaab05162
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '663'
ht-degree: 0%

---

# Platform Web SDK との IAB TCF 2.0 の統合

このガイドでは、タグを使用せずに、Interactive Advertising Bureau の Transparency &amp; Consent Framework、バージョン 2.0(IAB TCF 2.0) をAdobe Experience Platform Web SDK に統合する方法を示します。 IAB TCF 2.0 との統合の概要については、 [概要](./overview.md). タグとの統合方法に関するガイドについては、 [タグの IAB TCF 2.0 ガイド](./with-launch.md).

## はじめに

このガイドでは、 `__tcfapi` 同意情報にアクセスするためのインターフェイス。 クラウド管理プロバイダー (CMP) と直接統合する方が簡単な場合があります。 ただし、CMP は通常、TCF API と同様の機能を提供するので、このガイドの情報は引き続き役立つ場合があります。

>[!NOTE]
>
>以下の例では、コードが実行されるまでに `window.__tcfapi` がページで定義されている。 CMP には、 `__tcfapi` オブジェクトの準備が整いました。

IAB TCF 2.0 をタグとAdobe Experience Platform Web SDK 拡張機能と共に使用するには、XDM スキーマを使用可能にする必要があります。 これらのいずれも設定していない場合は、先に進む前にこのページを表示して開始します。

また、このガイドでは、Adobe Experience Platform Web SDK に関する十分な知識が必要です。 簡単なリフレッシャーについては、 [Adobe Experience Platform Web SDK の概要](../../home.md) そして [よくある質問](../../web-sdk-faq.md) ドキュメント。

## デフォルトの同意の有効化

すべての不明なユーザーを同じように扱う場合は、 `pending` または `out`. これは、同意の環境設定を受け取るまで、エクスペリエンスイベントをキューに追加または破棄します。

デフォルトの同意について詳しくは、 [デフォルトの同意セクション](../../fundamentals/configuring-the-sdk.md#default-consent) （Platform Web SDK 設定ドキュメント）を参照してください。

### 次に基づくデフォルトの同意の設定 `gdprApplies`

一部の CMP では、EU 一般データ保護規則 (GDPR) がお客様に適用されるかどうかを判断する機能を提供しています。 GDPR が適用されないお客様の同意を前提とする場合は、 `gdprApplies` フラグを設定します。

次の例に、これをおこなう方法の 1 つを示します。

```javascript
var alloyConfiguration = { ... };
window.__tcfapi('getTCData', 2, function (tcData, success) {
  if (success) {
    alloyConfiguration.defaultConsent = tcData.gdprApplies ? "pending" : "in";
    window.alloy("configure", alloyConfiguration);
  }
});
```

この例では、 `configure` コマンドが `tcData` は TCF API から取得されます。 If `gdprApplies` が true の場合、デフォルトの同意はに設定されます。 `pending`. If `gdprApplies` が false の場合、デフォルトの同意はに設定されます。 `in`. 必ず `alloyConfiguration` 変数に設定を入力します。

>[!NOTE]
>
>デフォルトの同意がに設定されている場合 `in`、 `setConsent` コマンドを使用して、顧客の同意設定を記録できます。

## setConsent イベントの使用

IAB TCF 2.0 API は、お客様が同意を更新した場合にイベントを提供します。 これは、顧客が最初に環境設定を設定したときや、顧客が環境設定を更新したときに発生します。

次の例に、これをおこなう方法の 1 つを示します。

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

このコードブロックは、 `useractioncomplete` イベントを送信し、同意を設定します。 `gdprApplies` フラグ。 顧客のカスタム ID がある場合は、必ず `identityMap` 変数を使用します。 に関するガイドを参照してください。 [同意のサポート](../../consent/supporting-consent.md) 電話での詳細 `setConsent`.

## sendEvent に同意情報を含める

XDM スキーマ内では、エクスペリエンスイベントから同意設定情報を保存できます。 この情報を各イベントに追加する方法は 2 つあります。

まず、 `sendEvent` 呼び出し。 次の例に、これをおこなう方法の 1 つを示します。

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

この例では、TCF API の同意情報を取得し、同意情報が XDM スキーマに追加されたイベントを送信します。 詳しくは、 [イベントの追跡](../../fundamentals/tracking-events.md) 内に何があるべきかを理解するためのガイド `sendEvent` コマンドオプション。

すべてのリクエストに同意情報を追加するもう 1 つの方法は、 `onBeforeEventSend` コールバック。 の節を読む [イベントのグローバルな変更](../../fundamentals/tracking-events.md#modifying-events-globally) イベントの追跡に関するドキュメントを参照してください。

## 次の手順

これで、IAB TCF 2.0 と Platform Web SDK 拡張機能の使用方法が学びました。これで、Adobe Analyticsやリアルタイム顧客データプラットフォームなどの他のAdobeソリューションと統合することも選択できます。 詳しくは、 [IAB Transparency &amp; Consent Framework 2.0 の概要](./overview.md) を参照してください。
