---
title: Adobe Experience Platform Web SDK を使用して IAB TCF 2.0 のサポートを統合する
description: タグを使用せずに web サイトの IAB TCF 2.0 サポートを設定する方法を説明します。
seo-description: Learn how to set up IAB TCF 2.0 consent with Adobe Experience Platform Web SDK
exl-id: 14f1802a-0f8d-487f-ae17-5daaaab05162
source-git-commit: b08c6cf12a38f79e019544dea91913a77bd6490a
workflow-type: tm+mt
source-wordcount: '615'
ht-degree: 0%

---

# IAB TCF 2.0 のサポートと Platform Web SDK の統合

このガイドでは、タグを使用せずに、Interactive Advertising Bureau Transparency &amp; Consent Framework バージョン 2.0 （IAB TCF 2.0）をAdobe Experience Platform Web SDK と統合する方法について説明します。 IAB TCF 2.0 との統合の概要については、以下を参照してください [の概要](./overview.md). タグとの統合方法については、を参照してください。 [タグ用 IAB TCF 2.0 ガイド](./with-tags.md).

## はじめに

このガイドでは、を使用します `__tcfapi` 同意情報にアクセスするためのインターフェイス。 クラウド管理プロバイダー（CMP）と直接統合する方が簡単な場合があります。 ただし、CMP は一般に TCF API と同様の機能を提供するので、このガイドの情報は引き続き役に立つ場合があります。

>[!NOTE]
>
>これらの例では、コードが実行されるまでに `window.__tcfapi` ページで定義されます。 CMP は次の場合にこれらの関数を実行できるフックを提供できます `__tcfapi` オブジェクトの準備ができました。

タグおよびAdobe Experience Platform Web SDK 拡張機能で IAB TCF 2.0 を使用するには、XDM スキーマが使用可能である必要があります。 これらのどちらも設定していない場合は、続行する前にこのページを表示して開始します。

また、このガイドでは、Adobe Experience Platform Web SDK に関する十分な知識が必要です。 簡単な復習については、 [Adobe Experience Platform Web SDK の概要](../../home.md) および [よくある質問](../../faq.md) ドキュメント。

## デフォルトの同意の有効化

すべての不明なユーザーを同じように扱う場合は、次のように設定できます [`defaultConsent`](/help/web-sdk/commands/configure/defaultconsent.md) 対象： `pending` または `out`. これにより、同意設定が受信されるまでエクスペリエンスイベントがキューに入れられるか、破棄されます。

### 基づくデフォルトの同意の設定 `gdprApplies`

一部の CMP は、お客様が GDPR （一般データ保護規則）を適用されているかどうかを判断する機能を提供します。 GDPR が適用されない顧客の同意を得たい場合は、 `gdprApplies` tcf API 呼び出しのフラグ。

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

この例では、 `configure` コマンドは、 `tcData` は、TCF API から取得されます。 次の場合 `gdprApplies` が true の場合、デフォルトの同意はに設定されます `pending`. 次の場合 `gdprApplies` が false の場合、デフォルトの同意がに設定されます `in`. 必ずを入力してください。 `alloyConfiguration` ご使用の設定で変更します。

>[!NOTE]
>
>デフォルトの同意がに設定されている場合 `in`, `setConsent` コマンドは、引き続き顧客の同意環境設定を記録するために使用できます。

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

このコードブロックは、 `useractioncomplete` イベントを発生させ、同意を設定し、同意文字列と `gdprApplies` フラグ。 顧客のカスタム ID がある場合は、必ずを入力してください `identityMap` 変数。 のガイドを参照してください [setConsent](../../../web-sdk/commands/setconsent.md) を参照してください。

## sendEvent に同意情報を含める

XDM スキーマ内に、エクスペリエンスイベントから同意環境設定情報を保存できます。 すべてのイベントにこの情報を追加する方法は 2 つあります。

まず、に関連する XDM スキーマを指定できます `sendEvent` を呼び出します。 次の例は、これを行う 1 つの方法を示しています。

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

すべてのリクエストに同意情報を追加するもう 1 つの方法は、 [`onBeforeEventSend`](/help/web-sdk/commands/configure/onbeforeeventsend.md) コールバック。

## 次の手順

これで、IAB TCF 2.0 を Platform Web SDK 拡張機能と共に使用する方法を説明したので、次は、Adobe AnalyticsやAdobe Real-time Customer Data Platformなど、他のAdobeソリューションと統合することもできます。 を参照してください。 [IAB の透明性および同意フレームワーク 2.0 の概要](./overview.md) を参照してください。
