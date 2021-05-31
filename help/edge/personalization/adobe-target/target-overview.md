---
title: Platform Web SDKでのAdobe Targetの使用
description: Adobe Targetを使用してExperience PlatformWeb SDKでパーソナライズされたコンテンツをレンダリングする方法を説明します
keywords: target;adobe target;activity.id;experience.id;renderDecisions;decisionScopes；事前非表示スニペット；vec；フォームベースのExperience Composer;xdm；オーディエンス；決定；スコープ；スキーマ；
exl-id: 021171ab-0490-4b27-b350-c37d2a569245
source-git-commit: c3d66e50f647c2203fcdd5ad36ad86ed223733e3
workflow-type: tm+mt
source-wordcount: '652'
ht-degree: 3%

---

# Platform Web SDKでのAdobe Targetの使用

Adobe Experience Platform [!DNL Web SDK]は、Adobe Targetで管理されるパーソナライズされたエクスペリエンスをWebチャネルに配信し、レンダリングできます。 [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html)(VEC)、または非ビジュアルインターフェイスである[フォームベースのExperience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html)と呼ばれるWYSIWYGエディターを使用して、アクティビティとパーソナライゼーションエクスペリエンスを作成、アクティブ化、配信できます。

次の機能はテスト済みで、現在Targetでサポートされています。

* A/Bテスト
* A4Tインプレッションおよびコンバージョンレポート
* Automated Personalization
* エクスペリエンスのターゲット設定
* 多変量分析テスト
* ネイティブのTargetインプレッションおよびコンバージョンレポート
* VECサポート

## Adobe Targetの有効化

[!DNL Target]を有効にするには、次の操作を行います。

1. [datastream](../../fundamentals/datastreams.md)で適切なクライアントコードを使用してTargetを有効にします。
1. イベントに`renderDecisions`オプションを追加します。

その後、オプションで、次のオプションも追加できます。

* `decisionScopes`:イベントにこのオプションを追加して、特定のアクティビティ（フォームベースのコンポーザーで作成されたアクティビティで役立つ）を取得します。
* [スニペットの事前非表示](../manage-flicker.md):ページの特定の部分のみを非表示にします。

## Adobe Target VECの使用

Platform Web SDK実装でVECを使用するには、[Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-target-vec-helper/)または[Chrome](https://chrome.google.com/webstore/detail/adobe-target-vec-helper/ggjpideecfnbipkacplkhhaflkdjagak)VECヘルパー拡張機能をインストールしてアクティブ化します。

## VECアクティビティの自動レンダリング

Adobe Experience Platform Web SDKは、Adobe Target Web上のVECで定義されたエクスペリエンスを、ユーザーに対して自動的にレンダリングする機能を備えています。 VECアクティビティを自動レンダリングするようAdobe Experience Platform Web SDKに指示するには、`renderDecisions = true`を含むイベントを送信します。

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

フォームベースのExperience Composerは、JSON、HTML、画像など、様々な応答タイプを持つA/Bテスト、[!DNL Experience Targeting]、Automated Personalization、Recommendationsアクティビティを設定するのに役立つ、非視覚的なインターフェイスです。 Adobe Targetから返される応答のタイプや決定に応じて、コアビジネスロジックを実行できます。 フォームベースのComposerアクティビティの決定を取得するには、決定を取得するすべての「decisionScopes」を含むイベントを送信します。

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

## 判定範囲

`decisionScopes` は、パーソナライズされたエクスペリエンスをレンダリングするページのセクション、場所または部分を定義します。これらの`decisionScopes`はカスタマイズ可能で、ユーザー定義です。 現在の[!DNL Target]のお客様の場合、`decisionScopes`は「mbox」とも呼ばれます。 [!DNL Target] UIでは、`decisionScopes`が「場所」として表示されます。

## `__view__`スコープ

Adobe Experience Platform Web SDKは、VECアクションをレンダリングするSDKに依存せずにVECアクションを取得できる機能を提供します。 `__view__`を`decisionScopes`として定義したイベントを送信します。

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

## XDMのオーディエンス

Adobe Experience Platform Web SDKを介して配信されるTargetアクティビティ用のオーディエンスを定義する場合は、[XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html?lang=ja)を定義して使用する必要があります。 XDMスキーマ、クラス、スキーマフィールドグループを定義したら、XDMデータで定義されたターゲットオーディエンスルールを作成してターゲティングできます。 Target内では、XDMデータはカスタムパラメーターとしてAudience Builderに表示されます。 XDMは、ドット表記（例：`web.webPageDetails.name`）を使用してシリアル化されます。

カスタムパラメーターまたはユーザープロファイルを使用する事前定義済みのオーディエンスを持つTargetアクティビティは、SDKを介して正しく配信されません。 カスタムパラメーターやユーザープロファイルを使用する代わりに、XDMを使用する必要があります。 ただし、XDMを必要としない、Adobe Experience Platform Web SDKを介してサポートされる標準のオーディエンスターゲティングフィールドがあります。 これらのフィールドは、XDMを必要としないTarget UIで使用できます。

* ターゲットライブラリ
* 地域
* ネットワーク
* Operating System
* サイトのページ
* Browser
* トラフィックソース
* 時間枠

## 用語

__決定：__ では、 [!DNL Target]決定はアクティビティから選択されたエクスペリエンスと相関関係にあります。

__スキーマ：__ 決定のスキーマは、でのオファーのタイプで [!DNL Target]す。

__範囲：__ 決定の範囲。[!DNL Target]では、スコープはmBoxです。 グローバルmBoxは`__view__`スコープです。

__XDM:__ XDMはドット表記にシリアル化され、mboxパラメーター [!DNL Target] としてに配置されます。
