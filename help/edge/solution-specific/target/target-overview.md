---
title: 'アドビターゲットとAdobe Experience Platform Web SDK。 '
seo-title: Adobe Experience Platform Web SDK and using Adobeターゲット
description: Adobeターゲットを使用してExperience Platform Web SDKを使用してパーソナライズされたコンテンツをレンダリングする方法を学びます
seo-description: Adobeターゲットを使用してExperience Platform Web SDKを使用してパーソナライズされたコンテンツをレンダリングする方法を学びます
translation-type: tm+mt
source-git-commit: db4bfec04a1116ce2b6a0be7ca0e8cb2f9639ad6

---


# ターゲットの概要

Adobe Experience Platform Web SDKは、この [パーソナライゼーション機能を通じてAdobeターゲットと統合され](../../fundamentals/rendering-personalization-content.md) 、パーソナライズされたコンテンツや意思決定を簡単に提供できます。

## アドビターゲットの有効化

ターゲットを有効にするには、次の手順を実行する必要があります。

- 適切なクライアントコードを使用して [エッジ設定のターゲットを有効にし](../../fundamentals/edge-configuration.md) ます。
- イベント追加に対する `renderDecisions` オプション。

また、オプションで次のこともできます。

- イベント `scopes` に特定のアクティビティを取得す追加る(フォームベースのコンポーザーで作成されたアクティビティに役立ちます)。
- ペ追加ージの特定の部分のみを非表示にするための [](../../fundamentals/managing-flicker.md) prehidingスニペット。

## VECの使用

SDKを使用すると、通常はVECを使用できますが、1つの例外では、 [ターゲットVECヘルパー拡張機能のインストール](https://docs.adobe.com/content/help/en/target/using/experiences/vec/troubleshoot-composer/vec-helper-browser-extension.html) とアクティブが必要です。

## フォームベースのコンポーザーの使用

フォームベースのコンポーザーは、コンテンツを返すためにも使用できます。 範囲は、ターゲットUIに場所(mBox)として表示されます。 また、スコープを使用する場合は、開発者が回答を探していることを確認する必要があります。

## XDMでのオーディエンス

ターゲット内のXDMデータは、オーディエンスビルダーにカスタムパラメーターとして表示されます。 XDMはドット表記(例： `web.webPageDetails.name`)

## 用語

__決定__ -ターゲット上、これらは、アクティビティから選択したエクスペリエンスと相関します。

__範囲__ — 決定の範囲。 ターゲットでは、これはmBoxです。 グローバルmBoxが `__view__` スコープです。

__スキーマ__ — 決定のスキーマは、ターゲットのオファーのタイプです。

__XDM__ - XDMはドット表記にシリアル化され、mBoxパラメーターとしてターゲットに挿入されます。