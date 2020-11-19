---
title: Adobe Experience Platform Web SDK のヘルプ
seo-title: Adobe Experience Platform Web SDK のヘルプ
description: Adobe Experience Platform Web SDK の概要と、その使用方法を説明します。
seo-description: Adobe Experience Cloudのお客様がExperience Cloudの様々なサービスを利用できるようにする方法を学びます。
keywords: Adobe Experience Platform Web SDK;Platform Web SDK;Web SDK;edge;Visitor.js;AppMeasurement.js;AT.js;DIL.js;web sdk;SDK;web SDK;Launch;launch
translation-type: tm+mt
source-git-commit: bdd80b15258bf4e3c0dee1e260fd3469c76d5885
workflow-type: tm+mt
source-wordcount: '689'
ht-degree: 20%

---


# Adobe Experience PlatformWeb SDKとは

Adobe Experience Platform Web SDK is a client-side JavaScript library that allows customers of Adobe Experience Cloud to interact with the various services in the [!DNL Experience Cloud] through the Adobe Experience Platform Edge Network. JavaScriptライブラリに加え、Web SDKの設定を支援する [Experience Platform Launch拡張](https://docs.adobe.com/content/help/ja-JP/launch/using/extensions-ref/adobe-extension/aep-extension/overview.html) 。

## エクスペリエンスエッジ

[!DNL Adobe Experience Platform Web SDK] は、Experience Edgeを構成するコレクションの一部です。 Experience Edgeは、次の3つのテクノロジーで構成されています。

* **[!DNL Adobe Experience Platform Web SDK]:** テクノロジーの導入を大幅にシンプル化するJavaScript SDKと [!DNL Experience Platform Launch][!DNL Adobe] 拡張機能
* **Adobe Experience PlatformモバイルSDK:** v5モバイルSDKの拡張機能により、お客様は新しい導入方法を使用できます。
* **[!DNL Adobe Experience Platform Edge Network]:** サーバのグローバルな分散ネットワーク。製品の導入に関する新しい方法論を可能にし [!DNL Adobe] ます。

The [!DNL Adobe Experience Edge] is a new framework for low-latency data collection, pluggable computing and rapid data activation across all addressable channels.

[!DNL Adobe Experience Edge] は、すべてのチャネル（JavaScript、モバイル、サーバー側）に対して単一の統合SDKを提供します。このSDKは、共通のAdobeドメイン(`adobedc.net`)にデータを送信し、データとエクスペリエンスの配信に対して単一のペイロードを受け取ります。

サーバ側では、統合エッジゲートウェイと共通プラットフォームサービスフレームワークを使用して、新しい機能をこのリアルタイムコンピューティング環境に容易にプラグインおよび導入できます。  このアーキテクチャには次の特長があります。

* 顧客の価値までの時間を短縮
* 「ポイント」統合の必要性を終了
* 古いライブラリと比較してパフォーマンスが向上
* コストの削減
* 革新のスピードを上げる
* Adobeのお客様にとって、継続的な競争優位性を提供

統合されたエッジシステムを1つ使用することで、顧客は広告、マーケティング、パーソナライズの各キャンペーンを、統合されたエクスペリエンスとしてすべてのチャネルにわたって管理できます。  TCO（総所有コスト） [!DNL Adobe] の低いサービスをお客様に提供できます。  また、リアルタイムエッジをプラグ可能にし、お客様が新しい機能やお客様定義のロジックをより迅速にそのリアルタイムシステムに追加できるようにすることで、製品の革新の迅速化にも役立ちます。 [!DNL Adobe]

## ビデオの概要

以下のビデオでは、Adobe Experience PlatformとAdobe Experience Platformの概要 [!DNL Web SDK] を説明 [!DNL Edge Network]します。

>[!VIDEO](https://video.tv.adobe.com/v/34141?quality=12&learn=on)

## Adobe Experience Platform Web SDK に置き換わる SDK

Adobe Experience Platform Web SDK は、次の SDK の代わりとなります。

* Visitor.js
* AppMeasurement.js
* AT.js
* DIL.js

これは、既存のライブラリの単なるラッパーではなく、完全なリライトです。目的は、タグを適切な順序で実行する必要があり、ライブラリのバージョン管理の課題との矛盾、依存関係の管理の改善によって、課題を解決することです。 これは、を実装する新しい方法で [!DNL Experience Cloud] あり、 [オープンソースです](https://github.com/adobe/alloy)。

新しいライブラリに加えて、アドビのソリューションに対する HTTP 要求を整理する新しいエンドポイントが追加されました。以前は、Visitor.js は訪問者 ID サービスに対してブロック呼び出しを送信した後、AT.js は Adobe Target に呼び出しを送信、DIL.js は Adobe Audience Manager に呼び出しを送信、最後に AppMeasurement.js は Adobe Analytics に呼び出しを送信していました。This new library and endpoint can retrieve an ID, fetch a [!DNL Target] experience, send data to [!DNL Audience Manager], and pass the data to Adobe Experience Platform in a single call.

次のビデオでは、Adobe Experience Platform [!DNL Web SDK] とAdobe Experience Platformの実際 [!DNL Edge Network] の動作を実演します。 このビデオの例では、 [!DNL Experience Platform]、、、およびにデータを送信する、Adobeへの1回の呼び出しを使用し [!DNL Analytics][!DNL Audience Manager][!DNL Target]ます。

>[!VIDEO](https://video.tv.adobe.com/v/34148?quality=12&learn=on)

この製品は、ますます多くの使用事例をサポートするように、常に進化し、成長しています。 最新のバージョンに対応するために、 [サポートされているユースケースボードをご覧ください](https://github.com/adobe/alloy/projects/5)。 現在サポートしている使用事例や、可能な限り最適な判断を下すために取り組んでいる使用事例について、この情報を最新の状態に保ちます。

* **まだサポートされていない使用例：** これらは、将来サポートされる予定の使用例です。
* **使用事例が進行中：** これらは、チームが現在リリースに向けて完了している使用例です。
* **サポートされる使用例：** これらは、現在サポートされていて機能する使用例です。
* **サポートしない使用例：** これらは、サポートしないと決めた使用例です。
