---
title: プレーンJavaScriptを使用したクイック開始
seo-title: 'Adobe Experience PlatformWeb SDKクイック開始 '
description: Experience PlatformWeb SDKを使用してデータを収集するクイック開始ガイド
seo-description: Experience PlatformWeb SDKを使用してデータを収集するクイック開始ガイド
keywords: 1st-party domain;CNAME;schema;create schema;configuration id;configuration tool;data element;create data element;XDM Object;sendEvent;send Event;install sdk;install web sdk;configure;configure web sdk;
translation-type: tm+mt
source-git-commit: 7aa0e654300f10db774794d66ece9d0d040cb2ba
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 15%

---


# ようこそ

このガイドでは、Adobe Experience PlatformWeb SDKの様々な設定方法を順を追って説明します。 この機能を使用するには、ホワイトリストに登録する必要があります。 待機中のリストに移動したい場合は、CSMに連絡してください。

- [ファーストパーティドメイン（CNAME）](https://docs.adobe.com/content/help/ja-JP/core-services/interface/ec-cookies/cookies-first-party.html)が有効になっている。既に Analytics 用 CNAME をお持ちの場合は、その CNAME を使用する必要があります。CNAMEを使用しない開発環境でのテストは可能ですが、実稼働環境に移行する前に必要です。
- Adobe Experience Platform　を使用する資格がある.  プラットフォームを購入していない場合、Adobeは、限られた方法でPlatform Data Services Foundationを提供し、SDKを使用して無償で利用できます。
- 訪問者 ID サービスの最新バージョンを使用している.

## スキーマの準備

はXDMとしてデータを [!DNL Experience Platform Edge Network] 受け取ります。 XDMは、スキーマを定義できるデータ形式です。 スキーマは、データの形式設定方法 [!DNL Edge Network] を定義します。 データを送信するには、スキーマを定義する必要があります。

- [スキーマ](../../xdm/tutorials/create-schema-ui.md)
- Add the Adobe Experience Platform [!DNL Web SDK] mixin to the schema you created

次のビデオでは、データ用のスキーマ、データセット、ストリーミングソースコネクタの作成をサポートします。 [!DNL Web SDK]

>[!VIDEO](https://video.tv.adobe.com/v/35395?quality=12&learn=on)

## 設定IDの作成

tag managementの機能を使用していない場合でも、Adobe起動の [エッジ設定ツール](../fundamentals/edge-configuration.md) (Edge Configuration Tool)を使用して設定IDを作成できます。 これにより、様々なソリューション [!DNL Edge Network] にデータを送信できるようになります。 各オプションの検索方法について詳しくは、「 [Edge Configuration Tool](../fundamentals/edge-configuration.md) 」ページを参照してください。

>[!NOTE]
>
>組織がこの機能を使用する許可リスト上に存在する必要があります。 許可リストを使用するには、CSMに問い合わせてください。

## SDKのインストール

SDKをインストールするには、以下の「ベースコード」を、HTMLのタグ内でできる限り高い位置にコピー&amp;ペーストします。 `<head>`

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["alloy"]);
</script>
<script src="https://cdn1.adoberesources.net/alloy/2.1.0/alloy.min.js" async></script>
```

これを行うための様々なオプションについて詳しくは、SDKの [インストールを参照してください](../fundamentals/installing-the-sdk.md)。

## SDKの設定

次に、SDKに設定を指定します。 これは、 `configure` コマンドを使用して行います。 これは、各ページで最初に呼び出されるコマンドです。

```javascript
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93:dev",
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

- [スキーマの構築](https://docs.adobe.com/content/help/ja-JP/experience-platform/xdm/schema/composition.html)
- [デバッグについて](../fundamentals/debugging.md)
- エクスペリエンスを [パーソナライズする方法を説明します。](../fundamentals/rendering-personalization-content.md)
- 複数のソリューションにデータを送信する方法について説明します。
   - [Adobe Analytics](../solution-specific/analytics/analytics-overview.md)
   - [Adobe Audience Manager](../solution-specific/audience-manager/audience-manager-overview.md)
   - [Adobe Target](../solution-specific/target/target-overview.md)
