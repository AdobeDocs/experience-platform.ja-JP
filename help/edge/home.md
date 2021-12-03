---
title: Adobe Experience Platform Web SDK の概要
description: Adobe Experience Platform Web SDK を使用して、Platform 機能を Web サイトに統合する方法について説明します。
keywords: Adobe Experience Platform Web SDK;Platform Web SDK;Web SDK;edge;Visitor.js;AppMeasurement.js;AT.js;DIL.js;Web SDK;SDK;Web SDK;Launch;Launch
exl-id: 1348144a-7d25-4c27-bc40-3daee2f043a6
source-git-commit: 92344ca9c2daf603d866c8a3cc4e92b72a382fb1
workflow-type: tm+mt
source-wordcount: '664'
ht-degree: 14%

---

# Adobe Experience Platform Web SDK の概要

Adobe Experience Platform Web SDK は、Adobe Experience Cloudのお客様が [!DNL Experience Cloud] Adobe Experience Platform Edge Network を通じて JavaScript ライブラリに加えて、 [タグ拡張](./extension/web-sdk-extension-configuration.md) Web SDK の設定に役立ちます。

## Experience Edge

[!DNL Adobe Experience Platform Web SDK] は、Experience Edge を構成するコレクションの一部です。 Experience Edge は、次の 3 つのテクノロジーで構成されます。

* **[!DNL Adobe Experience Platform Web SDK]:** JavaScript SDK およびタグ拡張により、の導入を大幅に簡略化 [!DNL Adobe] 技術
* **Adobe Experience Platform Mobile SDK:** v5 モバイル SDK の拡張機能で、顧客が新しいデプロイメント手法を使用できるようになります。
* **[!DNL Adobe Experience Platform Edge Network]:** 新しい導入方法を可能にする、サーバのグローバル分散ネットワーク [!DNL Adobe] 製品

この [!DNL Adobe Experience Edge] は、低レイテンシのデータ収集、プラグ可能なコンピューティング、およびアドレス可能なすべてのチャネルにわたる迅速なデータアクティベーションのための新しいフレームワークです。

[!DNL Adobe Experience Edge] は、チャネル（JavaScript、モバイル、サーバー側）ごとに 1 つの統合 SDK を提供し、この SDK が共通のAdobeドメイン (`adobedc.net`) に送信され、データおよびエクスペリエンス配信用の単一のペイロードを受け取ります。

サーバー側では、統合エッジゲートウェイと共通のプラットフォームサービスフレームワークを使用して、新しい機能をこのリアルタイムコンピューティング環境に簡単にプラグインし導入できます。  このアーキテクチャには次の特長があります。

* 顧客の価値創出までの時間を短縮
* 「ポイント」統合の必要性を終了
* 古いライブラリと比較してパフォーマンスが向上
* コストを削減
* イノベーションのスピードを上げる
* Adobeのお客様にとって継続的な競争優位性を実現

単一の統合エッジシステムを使用すると、顧客は、統合エクスペリエンスとして、すべてのチャネルで広告、マーケティングまたはパーソナライゼーションキャンペーンを管理できます。  可能です [!DNL Adobe] お客様に対して、TCO（総所有コスト）の低いサービスを提供する。  また、リアルタイムのエッジプラグを可能にし、 [!DNL Adobe] お客様は、新しい機能とお客様が定義したロジックをリアルタイムシステムに迅速に追加できます。

## ビデオの概要

次のビデオでは、Adobe Experience Platformの概要を説明しています [!DNL Web SDK] とAdobe Experience Platform [!DNL Edge Network].

>[!VIDEO](https://video.tv.adobe.com/v/34141?quality=12&learn=on)

## Adobe Experience Platform Web SDK に置き換わる SDK

Adobe Experience Platform Web SDK は、次の SDK の代わりとなります。

* Visitor.js
* AppMeasurement.js
* AT.js
* DIL.js

これは、既存のライブラリの単なるラッパーではなく、完全なリライトです。タグを適切な順序で実行し、ライブラリのバージョン管理の課題との矛盾、依存関係の管理を改善し、課題を解決するために使用します。 これは、 [!DNL Experience Cloud] そしてそれは [オープンソース](https://github.com/adobe/alloy).

新しいライブラリに加えて、アドビのソリューションに対する HTTP 要求を整理する新しいエンドポイントが追加されました。以前は、Visitor.js は訪問者 ID サービスに対してブロック呼び出しを送信した後、AT.js は Adobe Target に呼び出しを送信、DIL.js は Adobe Audience Manager に呼び出しを送信、最後に AppMeasurement.js は Adobe Analytics に呼び出しを送信していました。この新しいライブラリとエンドポイントは、ID を取得し、 [!DNL Target] エクスペリエンス、データの送信先 [!DNL Audience Manager]を呼び出し、1 回の呼び出しでデータをAdobe Experience Platformに渡します。

次のビデオでは、Adobe Experience Platformのデモを示します。 [!DNL Web SDK] とAdobe Experience Platform [!DNL Edge Network] 実行中 ビデオの例では、1 回の呼び出しでAdobeを呼び出し、がにデータを送信します。 [!DNL Experience Platform], [!DNL Analytics], [!DNL Audience Manager]、および [!DNL Target].

>[!VIDEO](https://video.tv.adobe.com/v/34148?quality=12&learn=on)

この製品は、より多くの使用例をサポートするために、常に進化し、成長しています。 最新の情報に対応するには、 [サポートされる使用例ページ](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/supported-use-cases.html). このページでは、現在サポートされている使用例と、利用可能な場合の詳細情報へのリンクを示します。

* **まだサポートされていないユースケース：** これらは、アドビのロードマップに記載されている、将来サポートされる使用例です。
* **進行中のユースケース：** チームが現在リリースの完了に取り組んでいる使用例を以下に示します。
* **サポートされるユースケース：** これらは現在サポートされ、機能している使用例です。
* **サポートしないユースケース：** これらは、サポートしないことを決定した使用例です。
