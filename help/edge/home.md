---
title: Adobe Experience Platform Web SDKの概要
description: Adobe Experience Platform Web SDKを使用して、Platform機能をWebサイトに統合する方法を説明します。
keywords: Adobe Experience Platform Web SDK;Platform Web SDK;Web SDK;Edge;Visitor.js;AppMeasurement.js;AT.js;DIL.js;Web SDK;SDK;Web SDK;Launch;Launch
exl-id: 1348144a-7d25-4c27-bc40-3daee2f043a6
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '664'
ht-degree: 14%

---

# Adobe Experience Platform Web SDKの概要

Adobe Experience Platform Web SDKは、Adobe Experience Cloudのお客様がAdobe Experience Platform Edgeネットワークを通じて[!DNL Experience Cloud]内の様々なサービスを操作できる、クライアントサイドJavaScriptライブラリです。 JavaScriptライブラリに加えて、Web SDKの設定に役立つ[タグ拡張](../tags/extensions/web/sdk/overview.md)があります。

## Experience Edge

[!DNL Adobe Experience Platform Web SDK] は、Experience Edgeを構成するコレクションの一部です。Experience Edgeは、次の3つのテクノロジーで構成されます。

* **[!DNL Adobe Experience Platform Web SDK]:** JavaScript SDKおよびタグ拡張により、テクノロジーの導入が大幅に簡略化さ [!DNL Adobe] れます
* **Adobe Experience Platform Mobile SDK:** v5モバイルSDKの拡張機能で、新しいデプロイメント手法を使用できます。
* **[!DNL Adobe Experience Platform Edge Network]:** 製品の導入に関する新しい方法論を可能にする、サーバーのグローバル分散ネットワ [!DNL Adobe] ーク

[!DNL Adobe Experience Edge]は、低レイテンシのデータ収集、プラグ可能なコンピューティング、およびアドレス可能なすべてのチャネルにわたる迅速なデータアクティベーションのための新しいフレームワークです。

[!DNL Adobe Experience Edge] は、各チャネル（JavaScript、モバイル、サーバー側）に対して1つの統合SDKを提供します。このSDKは、共通のAdobeドメイン(`adobedc.net`)にデータを送信し、データとエクスペリエンス配信の単一のペイロードを返します。

サーバ側では、統合エッジゲートウェイと共通のプラットフォームサービスフレームワークを使用して、新しい機能をこのリアルタイムコンピューティング環境に容易にプラグインおよび導入できます。  このアーキテクチャには次の特長があります。

* 顧客の価値創出までの時間を短縮
* 「ポイント」統合の必要性を終了
* 古いライブラリと比較してパフォーマンスが向上
* コストの削減
* イノベーションのスピードを上げる
* Adobeのお客様にとって、継続的な競争優位性を実現

単一の統合エッジシステムを使用すると、顧客は、すべてのチャネルにわたる広告、マーケティングまたはパーソナライゼーションキャンペーンを、統合エクスペリエンスとして管理できます。  [!DNL Adobe]は、TCO（総所有コスト）の低いサービスをお客様に提供できます。  また、リアルタイムエッジをプラグ可能にし、[!DNL Adobe]とお客様がリアルタイムシステムに新しい機能と顧客定義ロジックを迅速に追加できるようにすることで、製品の革新のスピードを向上させます。

## ビデオの概要

次のビデオでは、Adobe Experience Platform [!DNL Web SDK]とAdobe Experience Platform [!DNL Edge Network]の概要を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/34141?quality=12&learn=on)

## Adobe Experience Platform Web SDK に置き換わる SDK

Adobe Experience Platform Web SDK は、次の SDK の代わりとなります。

* Visitor.js
* AppMeasurement.js
* AT.js
* DIL.js

これは、既存のライブラリの単なるラッパーではなく、完全なリライトです。タグを適切な順序で実行し、ライブラリのバージョン管理の課題との矛盾、依存関係の管理を改善し、課題を解決することを目的としています。 これは[!DNL Experience Cloud]を実装する新しい方法で、[オープンソース](https://github.com/adobe/alloy)です。

新しいライブラリに加えて、アドビのソリューションに対する HTTP 要求を整理する新しいエンドポイントが追加されました。以前は、Visitor.js は訪問者 ID サービスに対してブロック呼び出しを送信した後、AT.js は Adobe Target に呼び出しを送信、DIL.js は Adobe Audience Manager に呼び出しを送信、最後に AppMeasurement.js は Adobe Analytics に呼び出しを送信していました。この新しいライブラリとエンドポイントでは、IDの取得、[!DNL Target]エクスペリエンスの取得、[!DNL Audience Manager]へのデータの送信、1回の呼び出しでのAdobe Experience Platformへのデータの渡しをおこなえます。

次のビデオでは、Adobe Experience Platform [!DNL Web SDK]とAdobe Experience Platform [!DNL Edge Network]の動作を示します。 このビデオの例では、Adobeへの1回の呼び出しを使用して、[!DNL Experience Platform]、[!DNL Analytics]、[!DNL Audience Manager]および[!DNL Target]にデータを送信します。

>[!VIDEO](https://video.tv.adobe.com/v/34148?quality=12&learn=on)

この製品は、より多くの使用例をサポートするために、常に進化し、成長しています。 最新の機能に対応するには、[サポートされる使用例のページ](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/supported-use-cases.html)を参照してください。 このページでは、現在サポートされている使用例と、利用可能な場合の詳細情報へのリンクを示します。

* **まだサポートされていないユースケース：** これらはアドビのロードマップ上にある、将来サポートされる予定の使用例です。
* **処理中の使用例：** チームが現在リリースを完了するために取り組んでいる使用例です。
* **サポートされる使用例：** これらは現在サポートされ、機能する使用例です。
* **サポートしない使用例：** サポートしないことを決定した使用例です。
