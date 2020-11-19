---
title: 'Adobe TargetとAdobe Experience PlatformのWeb SDK '
seo-title: Adobe Experience PlatformウェブSDKとAdobe Targetの使用
description: Adobe Targetを使用してExperience PlatformWeb SDKを使用し、パーソナライズされたコンテンツをレンダリングする方法を学びます
seo-description: Adobe Targetを使用してExperience PlatformWeb SDKを使用し、パーソナライズされたコンテンツをレンダリングする方法を学びます
keywords: target;adobe target;activity.id;experience.id;renderDecisions;decisionScopes;prehiding snippet;vec;Form-Based Experience Composer;xdm;audiences;decisions;scope;schema;
translation-type: tm+mt
source-git-commit: 0928dd3eb2c034fac14d14d6e53ba07cdc49a6ea
workflow-type: tm+mt
source-wordcount: '629'
ht-degree: 3%

---


# [!DNL Target] 概要

Adobe Experience Platform [!DNL Web SDK] は、Adobe Targetで管理されているパーソナライズされたエクスペリエンスをWebチャネルーに配信し、レンダリングできます。 Visual Experience Composer [(VEC)と呼ばれるWYSIWYGエディター、または](https://docs.adobe.com/content/help/en/target/using/experiences/vec/visual-experience-composer.html) フォームベースのExperience Composer [](https://docs.adobe.com/content/help/en/target/using/experiences/form-experience-composer.html)（非ビジュアルインターフェイス）を使用して、アクティビティとパーソナライズエクスペリエンスを作成、アクティブ化、配信できます。

## Adobe Targetを有効にする

有効にするに [!DNL Target]は、次の操作を行う必要があります。

1. 適切なクライアントコードを使用して [エッジ設定のターゲットを有効にし](../../fundamentals/edge-configuration.md) ます。
1. イベント追加に対する `renderDecisions` オプション。

その後、オプションで次のこともできます。

* 追加 `decisionScopes` をイベントに送信して、特定のアクティビティを取得します(フォームベースのコンポーザーで作成されたアクティビティに役立ちます)。
* ペ追加ージの特定の部分のみを非表示にする [](../manage-flicker.md) ためのプレヒードスニペット。

## Adobe TargetVECの使用

プラットフォームWeb SDK実装でVECを使用するには、 [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-target-vec-helper/) または [Chrome](https://chrome.google.com/webstore/detail/adobe-target-vec-helper/ggjpideecfnbipkacplkhhaflkdjagak) VEC Helper Extensionをインストールしてアクティブ化する必要があります。

## VECアクティビティを自動レンダリング

Adobe Experience PlatformWeb SDKは、Adobe TargetのVEC on the webで定義したエクスペリエンスを、ユーザーに合わせて自動的にレンダリングする機能を備えています。 VECアクティビティを自動レンダリングするAdobe Experience PlatformWeb SDKに示すには、次のイベントを送信し `renderDecisions = true`ます。

```javascript
alloy
("sendEvent", 
  { 
  "renderDecisions": true, 
  "xdm": {
    "commerce": { 
      "order": {
        "purchaseID": "a8g784hjq1mnp3", 
         "purchaseOrderNumber": "VAU3123", 
         "currencyCode": "USD", 
         "priceTotal": 999.98 
         } 
      } 
    }
  }
);
```

## フォームベースのコンポーザーの使用

フォームベースのExperience Composerは、JSON、HTML、画像など、様々な応答タイプを持つA/Bテスト、 [!DNL Experience Targeting]Automated Personalization、Recommendationsのアクティビティを設定するのに役立つ、非視覚的なインターフェイスです。 Adobe Targetが返す応答のタイプや決定に応じて、コアビジネスロジックを実行できます。 フォームベースのComposerアクティビティの判断を取得するには、決定を取得するすべての「decisionScopes」と共にイベントを送信します。

```javascript
alloy
  ("sendEvent", { 
    decisionScopes: [
      "foo", "bar"], 
      "xdm": {
        "commerce": { 
          "order": { 
            "purchaseID": "a8g784hjq1mnp3", 
            "purchaseOrderNumber": "VAU3123", 
            "currencyCode": "USD", 
            "priceTotal": 999.98 
          } 
        } 
      } 
    }
  );
```

## 決定範囲

`decisionScopes` パーソナライズされたエクスペリエンスをレンダリングするページのセクション、場所または部分を定義します。 これら `decisionScopes` はカスタマイズ可能で、ユーザー定義です。 現在の [!DNL Target] お客様 `decisionScopes` の場合は、「mbox」とも呼ばれます。 UIでは、「場所」と [!DNL Target]`decisionScopes` して表示されます。

## 範囲 `__view__`

Adobe Experience PlatformWeb SDKは、SDKに依存せずにVECアクションを取得し、VECアクションをレンダリングする機能を提供します。 として `__view__` 定義されたイベントを送信し `decisionScopes`ます。

```javascript
alloy("sendEvent", {
      "decisionScopes": ["__view__", "foo", "bar"], 
      "xdm": { 
        "web": { 
          "webPageDetails": { 
            "name": "Home Page"
          }
        } 
      }
    }
  ).then(function(results) {
    for (decision of results.decisions) {
      if (decision.decisionScope === "__view__") {
        console.log(decision.content)
      }
    }
  });
```

## XDMでのオーディエンス

Adobe Experience PlatformWeb SDKを介して配信されるターゲットアクティビティのオーディエンスを定義する場合は、 [XDM](https://docs.adobe.com/content/help/ja-JP/experience-platform/xdm/home.html) を定義して使用する必要があります。 XDMスキーマ、クラス、ミックスインを定義した後、ターゲティング用のXDMデータで定義されたターゲットオーディエンスルールを作成できます。 ターゲット内では、XDMデータはカスタムオーディエンスーとしてパラメータービルダーに表示されます。 XDMはドット表記(例えば、 `web.webPageDetails.name`)を使用してシリアル化されます。

ターゲットアクティビティに、カスタムパラメーターまたはユーザープロファイルを使用する定義済みのオーディエンスがある場合は、SDKを介して正しく配信されないことに注意してください。 カスタムパラメータやユーザプロファイルを使用する代わりに、XDMを使用する必要があります。 ただし、XDMを必要としない、Adobe Experience PlatformWeb SDKを介してサポートされる、すぐに使用できるオーディエンスターゲットフィールドがあります。 ターゲットUIで使用できるXDMを必要としないフィールドは次のとおりです。

* ターゲットライブラリ
* 地域
* ネットワーク
* Operating System
* サイトのページ
* Browser
* トラフィックソース
* 時間枠

## 用語

__判断：__ では、アクティビティ [!DNL Target]から選択したエクスペリエンスとの関連を示します。

__スキーマ:__ 決定のスキーマは、でのオファーの種類で [!DNL Target]す。

__範囲：__ 決定の範囲。 で [!DNL Target]は、mBoxです。 グローバルmBoxが `__view__` スコープです。

__XDM:__ XDMはドット表記にシリアライズされ、mBoxパラメーター [!DNL Target] としてに挿入されます。
