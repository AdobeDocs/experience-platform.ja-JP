---
title: 'Adobe TargetとAdobe Experience PlatformWeb SDK '
seo-title: Adobe Experience PlatformウェブSDKとAdobe Targetの使用
description: Adobe Targetを使用してExperience PlatformWeb SDKを使用し、パーソナライズされたコンテンツをレンダリングする方法を学びます
seo-description: Adobe Targetを使用してExperience PlatformWeb SDKを使用し、パーソナライズされたコンテンツをレンダリングする方法を学びます
keywords: target;adobe target;activity.id;experience.id;renderDecisions;decisionScopes;prehiding snippet;vec;Form-Based Experience Composer;xdm;audiences;decisions;scope;schema;
translation-type: tm+mt
source-git-commit: f08452fa9a6ece93e40ef8ca811530feb0620969
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 3%

---


# [!DNL Target] 概要

Adobe Experience Platform [!DNL Web SDK] は、Adobe Targetで管理されるパーソナライズされたエクスペリエンスをWebチャネルに配信し、レンダリングできます。 Visual Experience Composer [(VEC)と呼ばれるWYSIWYGエディター、または](https://docs.adobe.com/content/help/en/target/using/experiences/vec/visual-experience-composer.html) フォームベースのExperience Composer [](https://docs.adobe.com/content/help/en/target/using/experiences/form-experience-composer.html)（非ビジュアルインターフェイス）を使用して、アクティビティとパーソナライズエクスペリエンスを作成、アクティブ化、配信できます。

## Adobe Targetを有効にする

有効にするに [!DNL Target]は、次の操作を行う必要があります。

1. 適切なクライアントコードを使用して [エッジ設定のターゲットを有効にし](../../fundamentals/edge-configuration.md) ます。
1. イベント追加に対する `renderDecisions` オプション。

その後、オプションで次のこともできます。

* 追加 `decisionScopes` をイベントに送信して、特定のアクティビティを取得します(フォームベースのコンポーザーで作成されたアクティビティに役立ちます)。
* ペ追加ージの特定の部分のみを非表示にする [](../manage-flicker.md) ためのプレヒードスニペット。

## Adobe TargetVECの使用

SDKを使用すると、次の1つの例外を除き、通常どおりVECを使用できます。 [ターゲットVECヘルパー拡張機能がインストールされ](https://docs.adobe.com/content/help/en/target/using/experiences/vec/troubleshoot-composer/vec-helper-browser-extension.html) 、アクティブになっている必要があります。

## VECアクティビティを自動レンダリング

AEP Web SDKは、Adobe TargetのWeb上でのVECを使用して定義したエクスペリエンスを、ユーザーに合わせて自動的にレンダリングする機能を備えています。 VECアクティビティを自動レンダリングするAEP Web SDKに通知するには、次のイベントを送信し `renderDecisions = true`ます。

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

AEP [!DNL Web SDK] は、VECアクションをレンダリングするAEPに依存せずにVECアクションを取得できる機能 [!DNL Web SDK] を提供します。 として `__view__` 定義されたイベントを送信し `decisionScopes`ます。

```javascript
alloy("sendEvent", {
  decisionScopes: [“__view__”,"foo", "bar"], 
  "xdm": { 
    "web": { 
      "webPageDetails": { 
        "name": "Home Page"
       }
      } 
     }
    }
   ).then(results){
  for (decision of results.decisions){
     if(decision.decisionScope == "__view__")
       console.log(decision.content)
}
};
```

## XDMでのオーディエンス

AEP Web SDKを介して配信されるターゲットアクティビティのオーディエンスを定義する場合、 [XDM](https://docs.adobe.com/content/help/ja-JP/experience-platform/xdm/home.html) を定義して使用する必要があります。 XDMスキーマ、クラス、ミックスインを定義した後、ターゲティング用のXDMデータで定義されたターゲットオーディエンスルールを作成できます。 ターゲット内では、XDMデータはカスタムオーディエンスーとしてパラメータービルダーに表示されます。 XDMはドット表記(例えば、 `web.webPageDetails.name`)を使用してシリアル化されます。

ターゲットアクティビティに、カスタムパラメーターまたはユーザープロファイルを使用する定義済みのオーディエンスがある場合は、AEP Web SDKを介して正しく配信されないことに注意してください。 カスタムパラメータやユーザプロファイルを使用する代わりに、XDMを使用する必要があります。 ただし、XDMを必要としないAEP Web SDKを介してサポートされる、すぐに使用できるオーディエンスのターゲット設定フィールドがあります。 ターゲットUIで使用できるXDMを必要としないフィールドは次のとおりです。

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
