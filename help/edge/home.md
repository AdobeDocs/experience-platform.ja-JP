---
title: Adobe Experience Platform Web SDK の概要
description: Adobe Experience Platform Web SDK を使用して、Platform 機能を web サイトに統合する方法を説明します。
keywords: Adobe Experience Platform Web SDK;Platform Web SDK;Web SDK;エッジ;Visitor.js;AppMeasurement.js;AT.js;DIL.js;web sdk;SDK;web SDK;Launch;launch
exl-id: 1348144a-7d25-4c27-bc40-3daee2f043a6
source-git-commit: 71857ffc5e671f4d9a0502fb95d89d30fdec1f15
workflow-type: ht
source-wordcount: '622'
ht-degree: 100%

---

# Adobe Experience Platform Web SDK の概要

Adobe Experience Platform Web SDK は、Adobe Experience Cloud のお客様が Adobe Experience Platform Edge Network を通じて [!DNL Experience Cloud] の様々なサービスを操作できるようにする、クライアントサイド JavaScript ライブラリです。JavaScript ライブラリに加えて、Web SDK 設定に役立つ[タグ拡張](./extension/web-sdk-extension-configuration.md)があります。

**タグを使用して Web SDK を設定し、ソリューションにデータを送信するためのステップごとのガイドについては、[Web SDK を使用した Adobe Experience Cloud の実装チュートリアル](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html?lang=ja)を参照してください。**

## Experience Edge

[!DNL Adobe Experience Platform Web SDK] は、Experience Edge を構成するコレクションの一部です。Experience Edge は、次の 3 つのテクノロジーで構成されます。

* **[!DNL Adobe Experience Platform Web SDK]：**[!DNL Adobe] テクノロジーのデプロイを大幅にシンプル化するための JavaScript SDK およびタグ拡張
* **Adobe Experience Platform Mobile SDK：**&#x200B;お客様が新しいデプロイメント手法を使用できるようにするための v5 モバイル SDK の拡張機能
* **[!DNL Adobe Experience Platform Edge Network]：**[!DNL Adobe] 製品の新しいデプロイ手法を可能にする、サーバーのグローバル分散ネットワーク

[!DNL Adobe Experience Edge] は、すべてのアドレス可能なチャネルにわたる、低遅延のデータ収集、プラグ可能なコンピューティング、迅速なデータアクティブ化のための新しいフレームワークです。

[!DNL Adobe Experience Edge] は、すべてのチャネル（JavaScript、モバイル、サーバーサイド）に対して単一の統合 SDK を提供し、共通のアドビドメイン（`adobedc.net`）にデータを送信し、データとエクスペリエンスの配信のために単一のペイロードを返信します。

サーバーサイドでは、統一されたエッジゲートウェイと共通のプラットフォームサービスフレームワークにより、このリアルタイムコンピューティング環境に新しい機能を簡単にプラグインおよびデプロイできます。このアーキテクチャには次の特長があります。

* お客様の価値創出にかかる時間を短縮
* 「ポイント」統合の必要性をなくす
* 古いライブラリと比較してパフォーマンスを向上
* コストを削減
* イノベーションのスピードを上げる
* アドビのお客様に持続的な競争上の優位性を提供

単一の統合エッジシステムにより、お客様は、すべてのチャネルをまたいで、広告、マーケティングまたはパーソナライゼーションキャンペーンを統合されたエクスペリエンスとして管理できます。これにより、[!DNL Adobe] はお客様により低い総所有コストでサービスを提供できます。また、リアルタイムエッジをプラグ可能にして、[!DNL Adobe] とそのお客様がそのリアルタイムシステムに新しい機能およびお客様定義のロジックをより迅速に追加できるようにすることで、製品イノベーションのスピードを上げるのに役立ちます。

## ビデオの概要

次のビデオでは、Adobe Experience Platform [!DNL Web SDK] および Adobe Experience Platform [!DNL Edge Network] の概要を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/34141?quality=12&learn=on)

## Adobe Experience Platform Web SDK に置き換わる SDK

Adobe Experience Platform Web SDK は、次の SDK の代わりとなります。

* Visitor.js
* AppMeasurement.js
* AT.js
* DIL.js

これは、既存のライブラリの単なるラッパーではなく、完全なリライトです。その目的は、正しい順序で実行する必要があるタグ、ライブラリのバージョン管理との不整合、依存関係の管理などの課題を解決することです。[!DNL Experience Cloud] を実装するための新しい方法であり、[オープンソース](https://github.com/adobe/alloy)です。

新しいライブラリに加えて、アドビのソリューションに対する HTTP 要求を整理する新しいエンドポイントが追加されました。以前は、Visitor.js は訪問者 ID サービスに対してブロック呼び出しを送信した後、AT.js は Adobe Target に呼び出しを送信、DIL.js は Adobe Audience Manager に呼び出しを送信、最後に AppMeasurement.js は Adobe Analytics に呼び出しを送信していました。この新しいライブラリおよびエンドポイントでは、ID の取得、[!DNL Target] エクスペリエンスの取得、[!DNL Audience Manager] へのデータの送信、1 回の呼び出しでの Adobe Experience Platform へのデータの受け渡しが可能です。

次のビデオでは、Adobe Experience Platform [!DNL Web SDK] および Adobe Experience Platform [!DNL Edge Network] を実際に動作させています。ビデオの例では、アドビへの 1 回の呼び出しを使用して、[!DNL Experience Platform]、[!DNL Analytics]、[!DNL Audience Manager] および [!DNL Target] にデータを送信しています。

>[!VIDEO](https://video.tv.adobe.com/v/34148?quality=12&learn=on)

この製品は、より多くのユースケースに対応できるよう、常に進化を続け、成長しています。最新情報を入手し、現在サポートしているものを確認するには、[サポートされるユースケースページ](https://github.com/orgs/adobe/projects/18/views/1)を参照してください。
