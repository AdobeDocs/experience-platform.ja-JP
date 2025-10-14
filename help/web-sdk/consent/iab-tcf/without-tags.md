---
title: Adobe Experience Platform Web SDKを使用して IAB TCF 2.0 のサポートを統合する
description: タグを使用せずに web サイトの IAB TCF 2.0 サポートを設定する方法を説明します。
seo-description: Learn how to set up IAB TCF 2.0 consent with Adobe Experience Platform Web SDK
exl-id: 14f1802a-0f8d-487f-ae17-5daaaab05162
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '617'
ht-degree: 0%

---

# IAB TCF 2.0 のサポートとExperience Platform Web SDKの統合

このガイドでは、タグを使用せずに、Interactive Advertising Bureau Transparency &amp; Consent Framework Version 2.0 （IAB TCF 2.0）をAdobe Experience Platform Web SDKと統合する方法について説明します。 IAB TCF 2.0 との統合の概要については、[&#x200B; 概要 &#x200B;](./overview.md) を参照してください。 タグとの統合方法については、[&#x200B; タグ用 IAB TCF 2.0 ガイド &#x200B;](./with-tags.md) を参照してください。

## はじめに

このガイドでは、同意情報にアクセスするために、`__tcfapi` インターフェイスを使用します。 クラウド管理プロバイダー（CMP）と直接統合する方が簡単な場合があります。 ただし、CMP は一般に TCF API と同様の機能を提供するので、このガイドの情報は引き続き役に立つ場合があります。

>[!NOTE]
>
>これらの例では、コードが実行されるまでに、`window.__tcfapi` がページで定義されていると想定しています。 CMP は、`__tcfapi` オブジェクトの準備が整ったときにこれらの関数を実行できるフックを提供することができます。

タグおよびAdobe Experience Platform Web SDK拡張機能で IAB TCF 2.0 を使用するには、XDM スキーマが使用可能になっている必要があります。 これらのどちらも設定していない場合は、続行する前にこのページを表示して開始します。

また、このガイドでは、Adobe Experience Platform web SDKについて実際に理解している必要があります。 簡単に復習するには、[Adobe Experience Platform Web SDKの概要 &#x200B;](../../home.md) および [&#x200B; よくある質問 &#x200B;](../../faq.md) ドキュメントを参照してください。

## デフォルトの同意の有効化

すべての不明なユーザーを同じように扱う場合は、[`defaultConsent`](/help/web-sdk/commands/configure/defaultconsent.md) を `pending` または `out` に設定できます。 これにより、同意設定が受信されるまでエクスペリエンスイベントがキューに入れられるか、破棄されます。

### `gdprApplies` に基づくデフォルトの同意の設定

一部の CMP は、お客様が GDPR （一般データ保護規則）を適用されているかどうかを判断する機能を提供します。 GDPR が適用されない顧客の同意を得たい場合は、TCF API 呼び出しで `gdprApplies` フラグを使用できます。

次の例は、これを行う 1 つの方法を示しています。

```javascript
var alloyConfiguration = { ... };
window.__tcfapi('getTCData', 2, function (tcData, success) {
  if (success) {
    alloyConfiguration.defaultConsent = tcData.gdprApplies ? "pending" : "in";
    window.alloy("configure", alloyConfiguration);
  }
});
```

この例では、`configure` コマンドは、`tcData` が TCF API から取得された後に呼び出されます。 `gdprApplies` が true の場合、デフォルトの同意は `pending` に設定されます。 `gdprApplies` が false の場合、デフォルトの同意は `in` に設定されます。 必ず `alloyConfiguration` 変数に設定を入力してください。

>[!NOTE]
>
>デフォルトの同意が `in` に設定されている場合、`setConsent` コマンドを引き続き使用して、顧客の同意環境設定を記録できます。

## setConsent イベントの使用

IAB TCF 2.0 API は、顧客が同意を更新した場合にのイベントを提供します。 この問題は、顧客が最初に環境設定を行い、顧客が環境設定を更新した場合に発生します。

次の例は、これを行う 1 つの方法を示しています。

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

このコードブロックは、`useractioncomplete` イベントをリッスンして同意を設定し、同意文字列と `gdprApplies` フラグを渡します。 顧客のカスタム ID がある場合は、必ず `identityMap` 変数に入力します。 詳しくは、[setConsent](../../../web-sdk/commands/setconsent.md) に関するガイドを参照してください。

## sendEvent に同意情報を含める

XDM スキーマ内に、エクスペリエンスイベントから同意環境設定情報を保存できます。 すべてのイベントにこの情報を追加する方法は 2 つあります。

まず、`sendEvent` 呼び出しごとに関連する XDM スキーマを指定できます。 次の例は、これを行う 1 つの方法を示しています。

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

この例では、TCF API の同意情報を取得し、同意情報が XDM スキーマに追加されたイベントを送信します。

すべてのリクエストに同意情報を追加するもう 1 つの方法は、[`onBeforeEventSend`](/help/web-sdk/commands/configure/onbeforeeventsend.md) コールバックを使用することです。

## 次の手順

これで、IAB TCF 2.0 をExperience Platform Web SDK拡張機能と共に使用する方法を説明したので、次は、Adobe AnalyticsやAdobe Real-Time Customer Data Platformなどの他のAdobe ソリューションと統合することもできます。 詳しくは、[IAB Transparency &amp; Consent Framework 2.0 の概要 &#x200B;](./overview.md) を参照してください。
