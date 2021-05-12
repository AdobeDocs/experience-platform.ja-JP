---
title: Adobe TargetとプラットフォームWeb SDKの連携
description: Adobe Targetを使用してExperience PlatformWeb SDKを使用し、パーソナライズされたコンテンツをレンダリングする方法を学びます
keywords: ターゲット;adobeターゲット;アクティビティ.id；エクスペリエンス.id；レンダリング決定；決定範囲；スニペットの事前非表示；vec；フォームベースのExperience Composer;xdm;オーディエンス；決定；スコープ；スキーマ;
exl-id: 021171ab-0490-4b27-b350-c37d2a569245
source-git-commit: 20adb26fbd55302ac8005978968a0d69bdda8755
workflow-type: tm+mt
source-wordcount: '658'
ht-degree: 4%

---

# Adobe TargetとプラットフォームWeb SDKの連携

Adobe Experience Platform[!DNL Web SDK]は、Adobe Targetで管理されるパーソナライズされたエクスペリエンスをWebチャネルに配信およびレンダリングできます。 [Visual Experience Composer](https://docs.adobe.com/content/help/en/target/using/experiences/vec/visual-experience-composer.html)(VEC)と呼ばれるWYSIWYGエディター、または非ビジュアルインターフェイスである[フォームベースのExperience Composer](https://docs.adobe.com/content/help/en/target/using/experiences/form-experience-composer.html)を使用して、アクティビティとパーソナライズエクスペリエンスを作成、アクティブ化、配信できます。

次の機能がテスト済みで、現在ターゲットでサポートされています。

* A/Bテスト
* A4Tインプレッションとコンバージョンのレポート
* Automated Personalization
* エクスペリエンスのターゲット設定
* 多変量分析テスト
* ネイティブのターゲットインプレッションとコンバージョンレポート
* VECのサポート

## Adobe Targetを有効にする

[!DNL Target]を有効にするには、次の手順を実行します。

1. [datastream](../../fundamentals/datastreams.md)のターゲットを適切なクライアントコードで有効にします。
1. イベント追加に対する`renderDecisions`オプション。

次のオプションも追加できます。

* `decisionScopes`:このアクティビティをイベントに追加して、特定のオプション(フォームベースのコンポーザーで作成されたアクティビティに役立ちます)を取得します。
* [スニペットのプレヒード](../manage-flicker.md):ページの特定の部分のみを非表示にします。

## Adobe TargetVECの使用

プラットフォームWeb SDK実装でVECを使用するには、[Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-target-vec-helper/)または[Chrome](https://chrome.google.com/webstore/detail/adobe-target-vec-helper/ggjpideecfnbipkacplkhhaflkdjagak) VEC Helper Extensionをインストールしてアクティブ化します。

## VECアクティビティを自動レンダリング

Adobe Experience PlatformWeb SDKは、Adobe TargetのVEC on the webで定義したエクスペリエンスを、ユーザーに合わせて自動的にレンダリングする機能を備えています。 VECアクティビティを自動レンダリングするようにAdobe Experience PlatformWeb SDKに指示するには、`renderDecisions = true`と共にイベントを送信します。

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

フォームベースのExperience Composerは、JSON、HTML、画像など、様々な応答タイプを持つA/Bテスト、[!DNL Experience Targeting]、Automated Personalization、Recommendationsのアクティビティを設定するのに役立つ、非視覚的なインターフェイスです。 Adobe Targetが返す応答のタイプや決定に応じて、コアビジネスロジックを実行できます。 フォームベースのComposerアクティビティの判断を取得するには、決定を取得するすべての「decisionScopes」と共にイベントを送信します。

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

`decisionScopes` パーソナライズされたエクスペリエンスをレンダリングするページのセクション、場所または部分を定義します。これらの`decisionScopes`はカスタマイズ可能で、ユーザー定義です。 現在の[!DNL Target]ユーザーの場合、`decisionScopes`は「mbox」とも呼ばれます。 [!DNL Target] UIでは、`decisionScopes`は「場所」として表示されます。

## `__view__`スコープ

Adobe Experience PlatformWeb SDKは、SDKに依存せずにVECアクションを取得し、VECアクションをレンダリングする機能を提供します。 `__view__`を`decisionScopes`として定義したイベントを送信します。

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

Adobe Experience PlatformWeb SDK経由で配信されるターゲットアクティビティのオーディエンスを定義する場合は、[XDM](https://docs.adobe.com/content/help/ja-JP/experience-platform/xdm/home.html)を定義して使用する必要があります。 XDMスキーマ、クラス、スキーマフィールドグループを定義した後で、ターゲティング用のXDMデータで定義されたターゲットオーディエンスルールを作成できます。 ターゲット内では、XDMデータはカスタムオーディエンスーとしてパラメータービルダーに表示されます。 XDMはドット表記（例えば`web.webPageDetails.name`）を使用してシリアル化されます。

ターゲットアクティビティに、カスタムパラメーターまたはユーザープロファイルを使用する定義済みのオーディエンスがある場合、それらはSDKを介して正しく配信されません。 カスタムパラメータやユーザプロファイルを使用する代わりに、XDMを使用する必要があります。 ただし、XDMを必要としない、Adobe Experience PlatformWeb SDKを介してサポートされる、すぐに使用できるオーディエンスターゲットフィールドがあります。 XDMを必要としないターゲットUIでは、次のフィールドを使用できます。

* ターゲットライブラリ
* 地域
* ネットワーク
* Operating System
* サイトのページ
* Browser
* トラフィックソース
* 時間枠

## 用語

__決定事項：__ で [!DNL Target]は、アクティビティから選択したエクスペリエンスとの関連を決定します。

__スキーマ：決定__ のスキーマは、でのオファーのタイプ [!DNL Target]です。

__範囲：決定__ の範囲。[!DNL Target]では、範囲はmBoxです。 グローバルmBoxは`__view__`スコープです。

__XDM:XDM__ はドット表記にシリアライズされ、mBoxパラメーター [!DNL Target] としてに挿入されます。
