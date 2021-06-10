---
title: Platform Web SDKでのAdobe Targetの使用
description: Adobe Targetを使用してExperience PlatformWeb SDKでパーソナライズされたコンテンツをレンダリングする方法を説明します
keywords: target;adobe target;activity.id;experience.id;renderDecisions;decisionScopes；事前非表示スニペット；vec；フォームベースのExperience Composer;xdm；オーディエンス；決定；スコープ；スキーマ；
exl-id: 021171ab-0490-4b27-b350-c37d2a569245
source-git-commit: e7f5074ef776fc6ccd4f9951722861d771a371de
workflow-type: tm+mt
source-wordcount: '918'
ht-degree: 5%

---

# [!DNL Adobe Target]と[!DNL Platform Web SDK]の使用

[!DNL Adobe Experience Platform] [!DNL Web SDK] では、で管理されるパーソナライズされたエクスペリエンスを配 [!DNL Adobe Target] 信し、レンダリングできます。[Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html)(VEC)、または非ビジュアルインターフェイスである[フォームベースのExperience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html)と呼ばれるWYSIWYGエディターを使用して、アクティビティとパーソナライゼーションエクスペリエンスを作成、アクティブ化、配信できます。

次の機能はテスト済みで、現在[!DNL Target]でサポートされています。

* [A/Bテスト](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html)
* [A4Tインプレッションおよびコンバージョンレポート](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html)
* [Automated Personalizationアクティビティ](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html)
* [エクスペリエンスのターゲット設定アクティビティ](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html)
* [多変量分析テスト(MVT)](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html)
* [Recommendationsアクティビティ](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html)
* [ネイティブのTargetインプレッションおよびコンバージョンレポート](https://experienceleague.adobe.com/docs/target/using/reports/reports.html)
* [VECサポート](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html)

## [!DNL Adobe Target]を有効にする

[!DNL Target]を有効にするには、次の操作を行います。

1. [datastream](../../fundamentals/datastreams.md)の[!DNL Target]を適切なクライアントコードで有効にします。
1. イベントに`renderDecisions`オプションを追加します。

その後、オプションで、次のオプションも追加できます。

* **`decisionScopes`**:イベントにこのオプションを追加して、特定のアクティビティ（フォームベースのコンポーザーで作成されたアクティビティで役立つ）を取得します。
* **[スニペットの事前非表示](../manage-flicker.md)**:ページの特定の部分のみを非表示にします。

## Adobe Target VECの使用

[!DNL Platform Web SDK]実装でVECを使用するには、[Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-target-vec-helper/)または[Chrome](https://chrome.google.com/webstore/detail/adobe-target-vec-helper/ggjpideecfnbipkacplkhhaflkdjagak) VECヘルパー拡張機能をインストールして有効にします。

詳しくは、『*Adobe Targetガイド*』の「[Visual Experience Composerヘルパー拡張機能](https://experienceleague.adobe.com/docs/target/using/experiences/vec/troubleshoot-composer/vec-helper-browser-extension.html)」を参照してください。

## VECアクティビティの自動レンダリング

[!DNL Adobe Experience Platform Web SDK]は、ユーザーに対して、Web上の[!DNL Adobe Target]のVECを介して定義されたエクスペリエンスを自動的にレンダリングする機能を備えています。 VECアクティビティを自動レンダリングするように[!DNL Experience Platform Web SDK]に指示するには、`renderDecisions = true`を含むイベントを送信します。

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

[フォームベースのExperience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html)は、JSONなどの応答タイプの異なる[!UICONTROL A/Bテスト]、[!UICONTROL エクスペリエンスターゲット設定]、[!UICONTROL Automated Personalization]、[!UICONTROL Recommendations]アクティビティの設定に役立つ非視覚的なインターフェイスですHTML、画像など [!DNL Target]から返される応答のタイプや決定に応じて、コアビジネスロジックを実行できます。 フォームベースのComposerアクティビティの決定を取得するには、決定を取得するすべての「decisionScopes」を含むイベントを送信します。

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

`decisionScopes` パーソナライズされたエクスペリエンスをレンダリングするページのセクション、場所または部分を定義します。これらの`decisionScopes`はカスタマイズ可能で、ユーザー定義です。 現在の[!DNL Target]のお客様の場合、`decisionScopes`は「mbox」とも呼ばれます。 [!DNL Target] UIでは、`decisionScopes`が「場所」として表示されます。

## `__view__`スコープ

[!DNL Experience Platform Web SDK]は、VECアクションをレンダリングするSDKに依存せずにVECアクションを取得する機能を提供します。 `__view__`を`decisionScopes`として定義したイベントを送信します。

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

[!DNL Platform Web SDK]を介して配信される[!DNL Target]アクティビティのオーディエンスを定義する場合は、[XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html?lang=ja)を定義して使用する必要があります。 XDMスキーマ、クラス、スキーマフィールドグループを定義したら、XDMデータで定義される[!DNL Target]オーディエンスルールを作成してターゲティングをおこなうことができます。 [!DNL Target]内では、XDMデータは、カスタムパラメーターとして[!UICONTROL Audience Builder]に表示されます。 XDMは、ドット表記（例：`web.webPageDetails.name`）を使用してシリアル化されます。

カスタムパラメーターまたはユーザープロファイルを使用する事前定義済みのオーディエンスを持つ[!DNL Target]アクティビティがある場合、SDKを介して正しく配信されません。 カスタムパラメーターやユーザープロファイルを使用する代わりに、XDMを使用する必要があります。 ただし、[!DNL Platform Web SDK]経由でサポートされる標準のオーディエンスターゲティングフィールドは、XDMは不要です。 これらのフィールドは、XDMを必要としない[!DNL Target] UIで使用できます。

* ターゲットライブラリ
* 地域
* ネットワーク
* Operating System
* サイトのページ
* Browser
* トラフィックソース
* 時間枠

詳しくは、『*Adobe Targetガイド*』の[オーディエンスのカテゴリ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/target-rules.html?lang=en)を参照してください。

### 単一プロファイルの更新

[!DNL Platform Web SDK]を使用すると、プロファイルを[!DNL Target]プロファイルに更新し、さらに[!DNL Platform Web SDK]にエクスペリエンスイベントとして更新できます。

[!DNL Target]プロファイルを更新するには、次の情報を使用してプロファイルデータが渡されていることを確認します。

* 「`“data {“`
* 「`“__adobe.target”`
* プレフィックス`“profile.”`(例：

| キー | タイプ | 説明 |
| --- | --- | --- |
| `renderDecisions` | Boolean | パーソナライゼーションコンポーネントにDOMアクションを解釈するかどうかを指定します |
| `decisionScopes` | 配列 `<String>` | 決定を取得するスコープのリスト |
| `xdm` | オブジェクト | XDMで形式設定されたデータで、エクスペリエンスイベントとしてPlatform Web SDKに格納される |
| `data` | オブジェクト | ターゲットクラスの[!DNL Target]ソリューションに送信される任意のキーと値のペア。 |

このコマンドを使用する一般的な[!DNL Platform Web SDK]コードは次のようになります。

**`sendEvent`プロファイルデータと共に**

```
alloy("sendEvent", {
   renderDecisions: true|false,
   xdm: { // Experience Event XDM data },
   data: { // Freeform stuff (event & profile) }
});
```

**サンプルコード**

```
alloy("sendEvent", {
  renderDecisions: true,
  xdm: {
    device: {
      screenWidth: 9999
    }
  },
  data: {
    __adobe: {
      target: {
        "profile.gender": "female",
        "profile.age": 30,
	"entity.id" : "123",
	"entity.genre" : "Drama"
      }
    }
  }
}) 
.then(console.log);
```

## 推奨のリクエスト

次の表に、[!DNL Recommendations]属性と、それぞれが[!DNL Platform Web SDK]を介してサポートされているかどうかを示します。

| カテゴリ | 属性 | サポート状況 |
| --- | --- | --- |
| Recommendations — デフォルトのエンティティ属性 | entity.id | サポートあり |
|  | entity.name | サポートあり |
|  | entity.categoryId | サポートあり |
|  | entity.pageUrl | サポートあり |
|  | entity.thumbnailUrl | サポートあり |
|  | entity.message | サポートあり |
|  | entity.value | サポートあり |
|  | entity.inventory | サポートあり |
|  | entity.brand | サポートあり |
|  | entity.margin | サポートあり |
|  | entity.event.detailsOnly | サポートあり |
| Recommendations — カスタムエンティティの属性 | entity.yourCustomAttributeName | サポートあり |
| Recommendations — 予約mbox/ページパラメーター | excludedIds | サポートあり |
|  | cartIds | サポートあり |
|  | productPurchasedId | サポートあり |
| カテゴリ親和性のページまたは品目カテゴリ | user.categoryId | サポートあり |

## デバッグ

mboxTraceとmboxDebugは非推奨（廃止予定）となりました。 [[!DNL Platform Web SDK] デバッグ](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/debugging.html)を使用します。

## 用語

__決定：__ では、 [!DNL Target]決定はアクティビティから選択されたエクスペリエンスと相関関係にあります。

__スキーマ：__ 決定のスキーマは、でのオファーのタイプで [!DNL Target]す。

__範囲：__ 決定の範囲。[!DNL Target]では、スコープはmBoxです。 グローバルmBoxは`__view__`スコープです。

__XDM:__ XDMはドット表記にシリアル化され、mboxパラメーター [!DNL Target] としてに配置されます。
