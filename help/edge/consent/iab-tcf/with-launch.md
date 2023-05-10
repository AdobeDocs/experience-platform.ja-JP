---
title: タグと Platform Web SDK 拡張機能を使用した IAB TCF 2.0 の統合
description: タグとAdobe Experience Platform Web SDK 拡張機能を使用して IAB TCF 2.0 同意を設定する方法について説明します。
exl-id: dc0e6b68-8257-4862-9fc4-50b370ef204f
source-git-commit: 14e3eff3ea2469023823a35ee1112568f5b5f4f7
workflow-type: tm+mt
source-wordcount: '839'
ht-degree: 0%

---

# タグと Platform Web SDK 拡張機能を使用した IAB TCF 2.0 の統合

Adobe Experience Platform Web SDK は、Interactive Advertising Bureau の Transparency &amp; Consent Framework、バージョン 2.0(IAB TCF 2.0) をサポートしています。 このガイドでは、Adobe Experience Platform Web SDK タグ拡張機能を使用して、IAB TCF 2.0 の同意情報をAdobeに送信するためのタグプロパティを設定する方法について説明します。

タグを使用しない場合は、 [タグを使用しない IAB TCF 2.0 の使用](./without-launch.md).

## はじめに

IAB TCF 2.0 をタグと Platform Web SDK 拡張機能と共に使用するには、XDM スキーマとデータセットを使用できる必要があります。

また、このガイドでは、Adobe Experience Platform Web SDK に関する十分な知識が必要です。 簡単なリフレッシャーについては、 [Adobe Experience Platform Web SDK の概要](../../home.md) そして [よくある質問](../../web-sdk-faq.md) ドキュメント。

## デフォルトの同意の設定

拡張機能の設定内に、デフォルトの同意に関する設定があります。 同意 Cookie を持たないお客様の動作を制御します。 同意 Cookie を持たない顧客のエクスペリエンスイベントをキューに入れる場合は、これをに設定します。 `pending`. 同意 Cookie を持たない顧客のエクスペリエンスイベントを破棄する場合は、これをに設定します。 `out`. また、データ要素を使用して、デフォルトの同意値を動的に設定することもできます。

デフォルトの同意の設定方法について詳しくは、 [デフォルトの同意セクション](../../fundamentals/configuring-the-sdk.md#default-consent) （ SDK 設定ガイド）を参照してください。

## 同意情報を使用したプロファイルの更新 {#consent-code-1}

を呼び出すには、 `setConsent` アクションを実行する必要があります。顧客の同意設定が変更された場合は、新しいタグルールを作成する必要があります。 まず、新しいイベントを追加し、Core 拡張機能の「Custom Code」イベントタイプを選択します。

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

このカスタムコードは次の 2 つの処理をおこないます。

* 2 つのデータ要素を設定します。1 つは同意文字列を持ち、もう 1 つは `gdprApplies` フラグ。 これは、後で「同意の設定」アクションを実行する際に役立ちます。

* トリガー：同意設定が変更された場合のルール。 同意の環境設定が変更された場合は、「同意の設定」アクションを使用する必要があります。 拡張機能に「同意の設定」アクションを追加し、次のようにフォームに入力します。

* 標準：&quot;IAB TCF&quot;
* バージョン：&quot;2.0&quot;
* 値：&quot;%IAB TCF Consent String%&quot;
* GDPR が適用される：&quot;%IAB TCF Consent GDPR%&quot;

![IAB Set Consent Action](../../assets/consent/iab-tcf/with-launch/iab-action.png)

>[!IMPORTANT]
>
>カスタムコードで作成されたデータ要素なので、データ要素セレクターを使用してこれらのデータ要素を選択することはできません。 データ要素名にはパーセント記号を付けて入力する必要があります。 このコードは、顧客が変更するたびに、その顧客のプロファイルを新しい同意設定で更新します。 また、サーバーが cookie 値を返す場合、Adobe Experience Platform Web SDK が Experience Events を記録できなくなる可能性があります。

## エクスペリエンスイベント用の XDM データ要素の作成

同意文字列は、XDM エクスペリエンスイベントに含める必要があります。 これをおこなうには、XDM オブジェクトデータ要素を使用します。 最初に、新しい XDM オブジェクトデータ要素を作成するか、イベント送信用に既に作成した要素を使用します。 「エクスペリエンスイベントプライバシースキーマ」フィールドグループをスキーマに追加している場合は、 `consentStrings` キーを使用して XDM オブジェクトに追加します。

1. 選択 **[!UICONTROL consentStrings]**.

1. 選択 **[!UICONTROL 個々の項目を指定]** を選択し、 **[!UICONTROL 項目を追加]**.

1. を展開します。 **[!UICONTROL consentString]** 見出しを開き、最初の項目を展開して、次の値を入力します。

* `consentStandard`: IAB TCF
* `consentStandardVersion`: 2.0
* `consentStringValue`:%IAB TCF Consent String%
* `gdprApplies`:%IAB TCF 同意 GDPR%

>[!IMPORTANT]
>
>カスタムコードで作成されたデータ要素なので、データ要素セレクターを使用してこれらのデータ要素を選択することはできません。 データ要素名にはパーセント記号を付けて入力する必要があります。

## IAB TCF 2.0 同意情報を含む初期エクスペリエンスイベントの送信

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

このコードは、以前のカスタムコードと同じです。ただし、 `useractioncomplete` および `tcloaded` イベントが処理されます。 この [以前のカスタムコード](#consent-code-1) トリガーは、顧客が初めて設定を選択したときにのみ有効です。 また、このコードは、顧客が既に環境設定を選択している場合にもトリガーします。 例えば、2 番目のページ読み込み時などです。

Platform Web SDK 拡張機能から「イベントを送信」アクションを追加します。 「 XDM 」フィールド内で、前の節で作成した XDM データ要素を選択します。

## IAB TCF 2.0 同意情報を使用したその他のイベントの送信

最初のエクスペリエンスイベントの後にイベントがトリガーされた場合でも、2 つのデータ要素は引き続き定義され、IAB 同意情報の送信に使用できます。 同じ XDM データ要素を使用して、今後のイベントを送信します。 IAB TCF 2.0 情報が含まれています。

## 次の手順

これで、IAB TCF 2.0 と Platform Web SDK 拡張機能の使用方法が学びました。これで、Adobe AnalyticsやAdobe Real-time Customer Data Platformなどの他のAdobeソリューションと統合することも選択できます。 詳しくは、 [IAB Transparency &amp; Consent Framework 2.0 の概要](./overview.md) を参照してください。
