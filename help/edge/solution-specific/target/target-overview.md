---
title: 'アドビターゲットとAdobe Experience Platform Web SDK。 '
seo-title: Adobe Experience Platform Web SDK and using Adobeターゲット
description: Adobeターゲットを使用してExperience Platform Web SDKを使用してパーソナライズされたコンテンツをレンダリングする方法を学びます
seo-description: Adobeターゲットを使用してExperience Platform Web SDKを使用してパーソナライズされたコンテンツをレンダリングする方法を学びます
translation-type: tm+mt
source-git-commit: 4bff4b20ccc1913151aa1783d5123ffbb141a7d0
workflow-type: tm+mt
source-wordcount: '651'
ht-degree: 2%

---


# ターゲットの概要

Adobe Experience Platform Web SDKは、Adobeターゲットで管理されるパーソナライズされたエクスペリエンスをWebチャネルに配信し、レンダリングできます。 Visual Experience Composer [(VEC)と呼ばれるWYSIWYGエディター、または](https://docs.adobe.com/content/help/en/target/using/experiences/vec/visual-experience-composer.html) フォームベースのExperience Composer [](https://docs.adobe.com/content/help/en/target/using/experiences/form-experience-composer.html)（非ビジュアルインターフェイス）を使用して、アクティビティとパーソナライズエクスペリエンスを作成、アクティブ化、配信できます。

## アドビターゲットの有効化

ターゲットを有効にするには、次の操作を行う必要があります。

1. ターゲットUIでアクティビティ.idとexperience.idの応答トークンをオンにします。

![ターゲット_応答_トークン](../../solution-specific/target/assets/target_response_token.png)

1. 適切なクライアントコードを使用して [エッジ設定のターゲットを有効にし](../../fundamentals/edge-configuration.md) ます。
1. イベント追加に対する `renderDecisions` オプション。

その後、オプションで次のこともできます。

* 追加 `decisionScopes` をイベントに送信して、特定のアクティビティを取得します(フォームベースのコンポーザーで作成されたアクティビティに役立ちます)。
* ペ追加ージの特定の部分のみを非表示にする [](../../solution-specific/target/flicker-management.md) ためのプレヒードスニペット。

## AdobeターゲットVECの使用

SDKを使用すると、次の1つの例外を除き、通常どおりVECを使用できます。 [ターゲットVECヘルパー拡張機能がインストールされ](https://docs.adobe.com/content/help/en/target/using/experiences/vec/troubleshoot-composer/vec-helper-browser-extension.html) 、アクティブになっている必要があります。

## VECアクティビティを自動レンダリング

AEP Web SDKは、AdobeターゲットのWeb上でのVEC経由で定義されたエクスペリエンスを、ユーザーに合わせて自動的にレンダリングする機能を備えています。 VECアクティビティを自動レンダリングするAEP Web SDKに通知するには、次のイベントを送信します `renderDecisions = true`。

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

フォームベースのExperience Composerは、JSON、HTML、画像などの応答タイプごとに異なるA/Bテスト、エクスペリエンスターゲット設定、自動パーソナライゼーション、Recommendationsの各アクティビティを設定するのに役立つ、非視覚的なインターフェイスです。 アドビのターゲットが返す応答のタイプや決定に応じて、コアビジネスロジックを実行できます。 フォームベースのComposerアクティビティの判断を取得するには、決定を取得するすべての「decisionScopes」と共にイベントを送信します。

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

`decisionScopes` パーソナライズされたエクスペリエンスをレンダリングするページのセクション、場所または部分を定義します。 これら `decisionScopes` はカスタマイズ可能で、ユーザー定義です。 現在のターゲットのお客様 `decisionScopes` は、「mbox」とも呼ばれます。 ターゲットUIでは、「場所」と `decisionScopes` して表示されます。

## __表示__ 範囲

AEP Web SDKは、AEP Web SDKに依存せずにVECアクションを取得し、VECアクションをレンダリングする機能を提供します。 として `__view__` 定義されたイベントを送信し `decisionScopes`ます。

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

AEP Web SDKを介して配信されるターゲットアクティビティのオーディエンスを定義する場合、 [XDM](https://docs.adobe.com/content/help/en/experience-platform/xdm/home.html) を定義して使用する必要があります。 XDMスキーマ、クラス、ミックスインを定義した後、ターゲティング用のXDMデータで定義されたターゲットオーディエンスルールを作成できます。 ターゲット内では、XDMデータはカスタムオーディエンスーとしてパラメータービルダーに表示されます。 XDMはドット表記(例えば、 `web.webPageDetails.name`)を使用してシリアル化されます。

ターゲットアクティビティに、カスタムパラメーターまたはユーザープロファイルを使用する定義済みのオーディエンスがある場合は、AEP Web SDKを介して正しく配信されないことに注意してください。 カスタムパラメータやユーザプロファイルを使用する代わりに、XDMを使用する必要があります。 ただし、XDMを必要としないAEP Web SDKを介してサポートされる、すぐに使用できるオーディエンスのターゲット設定フィールドがあります。 ターゲットUIで使用できるXDMを必要としないフィールドは次のとおりです。

* ターゲットライブラリ
* 地域
* ネットワーク
* オペレーティングシステム
* サイトのページ
* ブラウザー
* トラフィックソース
* 時間枠

## 用語

__決定__ -ターゲット上、これらは、アクティビティから選択したエクスペリエンスとの関係を示します。

__範囲__ — 決定の範囲。 ターゲットでは、これはmBoxです。 グローバルmBoxが `__view__` スコープです。

__スキーマ__ — 決定のスキーマは、ターゲットのオファーのタイプです。

__XDM__ - XDMはドット表記にシリアル化され、mBoxパラメーターとしてターゲットに挿入されます。
