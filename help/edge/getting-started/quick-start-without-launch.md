---
title: プレーンJavaScriptを使用したクイック開始
seo-title: 'Adobe Experience Platform Web SDKのクイック開始 '
description: Experience Platform Web SDKを使用してデータを収集するクイック開始ガイド
seo-description: Experience Platform Web SDKを使用してデータを収集するクイック開始ガイド
translation-type: tm+mt
source-git-commit: 7c5d4306f9964553cf48a208166fce265dcdd94d
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 14%

---


# ようこそ

このガイドでは、Adobe Experience Platform Web SDKを設定する様々な方法の手順を説明します。 この機能を使用するには、ホワイトリストに登録する必要があります。 待機中のリストに移動したい場合は、CSMに連絡してください。

- [ファーストパーティドメイン（CNAME）](https://docs.adobe.com/content/help/ja-JP/core-services/interface/ec-cookies/cookies-first-party.html)が有効になっている。既に Analytics 用 CNAME をお持ちの場合は、その CNAME を使用する必要があります。CNAMEを使用しない開発環境でのテストは可能ですが、実稼働環境に移行する前に必要です
- Adobe Experience Platform Data Platformの権利を付与されます。  プラットフォームを購入していない場合は、Experience Platform Data Services Foundationをご利用いただき、SDKでの利用を制限し、無償で提供します。
- 訪問者 ID サービスの最新バージョンを使用している

## 設定IDの作成

Adobe Launchの [エッジ設定ツールを使用して設定IDを作成できます](../fundamentals/edge-configuration.md) （tag management機能を使用していない場合も含む）。 これにより、Edge Networkで様々なソリューションにデータを送信できるようになります。 各オプションの検索方法について詳しくは、「 [Edge Configuration Tool](../fundamentals/edge-configuration.md) 」ページを参照してください。

>[!NOTE]
>
>この機能を使用するには、組織がホワイトリストに登録されている必要があります。 最終的なホワイトリスト登録のために、CSMにリストに登録するようにお問い合わせください。

## スキーマの準備

Experience Platform Edge Networkは、データをXDMとして受け取ります。 XDMは、スキーマを定義できるデータ形式です。 スキーマは、Edge Networkでデータの形式設定方法を定義します。 データを送信するには、スキーマを定義する必要があります。

- [スキーマの作成](../../xdm/tutorials/create-schema-ui.md)
- 作成したスキーマに Adobe Experience Platform Web SDK mixin を追加します。

## SDKのインストール

SDKをインストールするには、以下の「ベースコード」を、HTMLのタグ内でできる限り高い位置にコピー&amp;ペーストします。 `<head>`

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["alloy"]);
</script>
<script src="https://cdn1.adoberesources.net/alloy/1.0.0/alloy.min.js" async></script>
```

これを行うための様々なオプションについて詳しくは、SDKの [インストールを参照してください](../fundamentals/installing-the-sdk.md)。

## SDKの設定

次に、SDKに設定を指定します。 これは、 `configure` コマンドを使用して行います。 これは、各ページで最初に呼び出されるコマンドです。

```javascript
alloy("configure", {
  "configId": "ebebf826-a01f-4458-8cec-ef61de241c93:dev",
  "orgId":"ADB3LETTERSANDNUMBERS@AdobeOrg"
});
```

ここでは、上記で作成した設定IDと組織IDを指定します。 これら2つの必須フィールドのみです。 ただし、その他の [設定オプションは多数あります](../fundamentals/configuring-the-sdk.md)。

## イベントの送信

configureコマンドを呼び出すと、自由に開始トラッキングイベントをできます。

```javascript
alloy("sendEvent", {});
```

通常、イベントにはデータをいくつか送信します。 このようにXDMデータを送信できます。 送信するデータは、XDMで作成したスキーマに存在する必要があります。

```javascript
alloy("sendEvent", {
  "xdm": {
    "web":{
      "webPageDetails":{
        "name":"Home Page"
      }
    }
  }
});
```

イベントの追跡方法について詳しくは、[イベントのトラッキング](../fundamentals/tracking-events.md)を参照してください。

## 次の手順

データの流し込みが完了したら、次の操作を実行できます。

- [スキーマの構築](https://docs.adobe.com/content/help/en/experience-platform/xdm/schema/composition.html)
- [デバッグについて](../fundamentals/debugging.md)
- エクスペリエンスを [パーソナライズする方法を説明します。](../fundamentals/rendering-personalization-content.md)
- 複数のソリューションにデータを送信する方法について説明します。
   - [Adobe Analytics](../solution-specific/analytics/analytics-overview.md)
   - [Adobe Audience Manager](../solution-specific/audience-manager/audience-manager-overview.md)
   - [Adobe Target](../solution-specific/target/target-overview.md)
