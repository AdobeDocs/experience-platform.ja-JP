---
title: Adobe Experience PlatformWeb SDKの概要
description: Adobe Experience PlatformWeb SDKを使用して、プラットフォーム機能をWebサイトに統合する方法を説明します。
keywords: Adobe Experience PlatformWeb SDK；プラットフォームWeb SDK;Web SDK；エッジ；訪問者.js;AppMeasurement.js;AT.js;DIL.js;Web sdk;Web SDK；起動；起動
translation-type: tm+mt
source-git-commit: 69f2e6069546cd8b913db453dd9e4bc3f99dd3d9
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 15%

---


# Adobe Experience PlatformウェブSDKの概要

Adobe Experience PlatformWeb SDKは、Adobe Experience Cloudのお客様がAdobe Experience Platformエッジネットワークを介して[!DNL Experience Cloud]の様々なサービスとやり取りできるクライアント側のJavaScriptライブラリです。 JavaScriptライブラリに加えて、Web SDKの設定に役立つ[Experience Platform Launch拡張子](https://experienceleague.adobe.com/docs/launch/using/extensions-ref/adobe-extension/aep-extension/overview.html)があります。

## エクスペリエンスエッジ

[!DNL Adobe Experience Platform Web SDK] は、Experience Edgeを構成するコレクションの一部です。Experience Edgeは、次の3つのテクノロジーで構成されています。

* **[!DNL Adobe Experience Platform Web SDK]:** JavaScript SDKと [!DNL Experience Platform Launch] 拡張機能により、 [!DNL Adobe] テクノロジの導入を大幅にシンプル化
* **Adobe Experience PlatformモバイルSDK:** v5モバイルSDKの拡張機能で、新しい展開方法を使用できるようになります。
* **[!DNL Adobe Experience Platform Edge Network]:** サーバのグローバルな分散ネットワーク。新しい [!DNL Adobe] 製品導入方法を可能にします。

[!DNL Adobe Experience Edge]は、低レイテンシのデータ収集、プラグ可能なコンピューティング、およびすべてのアドレス可能なチャネルにわたる迅速なデータアクティベーションのための新しいフレームワークです。

[!DNL Adobe Experience Edge] は、すべてのチャネル（JavaScript、モバイル、サーバー側）に対して単一の統合SDKを提供します。このSDKは、共通のAdobeドメイン(`adobedc.net`)にデータを送信し、データとエクスペリエンスの配信に対して単一のペイロードを受け取ります。

サーバ側では、統合エッジゲートウェイと共通プラットフォームサービスフレームワークを使用して、新しい機能をこのリアルタイムコンピューティング環境に容易にプラグインおよび導入できます。  このアーキテクチャには次の特長があります。

* 顧客の価値までの時間を短縮
* 「ポイント」統合の必要性を終了
* 古いライブラリと比較してパフォーマンスが向上
* コストの削減
* 革新のスピードを上げる
* Adobeのお客様にとって、継続的な競争優位性を提供

統合されたエッジシステムを1つ使用することで、顧客は広告、マーケティング、パーソナライズの各キャンペーンを、統合されたエクスペリエンスとしてすべてのチャネルにわたって管理できます。  [!DNL Adobe]は、TCO（総所有コスト）が低いサービスをお客様に提供できます。  また、リアルタイムエッジをプラグ可能にし、[!DNL Adobe]お客様が新しい機能やお客様定義のロジックをリアルタイムシステムに迅速に追加できるようにすることで、製品の革新の迅速化にも役立ちます。

## ビデオの概要

次のビデオでは、Adobe Experience Platform[!DNL Web SDK]とAdobe Experience Platform[!DNL Edge Network]の概要を説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/34141?quality=12&learn=on)

## Adobe Experience Platform Web SDK に置き換わる SDK

Adobe Experience Platform Web SDK は、次の SDK の代わりとなります。

* Visitor.js
* AppMeasurement.js
* AT.js
* DIL.js

これは、既存のライブラリの単なるラッパーではなく、完全なリライトです。目的は、タグを適切な順序で実行する必要があり、ライブラリのバージョン管理の課題との矛盾、依存関係の管理の改善によって、課題を解決することです。 [!DNL Experience Cloud]を実装する新しい方法で、[オープンソース](https://github.com/adobe/alloy)です。

新しいライブラリに加えて、アドビのソリューションに対する HTTP 要求を整理する新しいエンドポイントが追加されました。以前は、Visitor.js は訪問者 ID サービスに対してブロック呼び出しを送信した後、AT.js は Adobe Target に呼び出しを送信、DIL.js は Adobe Audience Manager に呼び出しを送信、最後に AppMeasurement.js は Adobe Analytics に呼び出しを送信していました。この新しいライブラリとエンドポイントは、IDを取得し、[!DNL Target]エクスペリエンスを取得し、[!DNL Audience Manager]にデータを送信し、1回の呼び出しでデータをAdobe Experience Platformに渡すことができます。

次のビデオでは、Adobe Experience Platform[!DNL Web SDK]とAdobe Experience Platform[!DNL Edge Network]の実際の動作を実演します。 このビデオの例では、Adobeへの1回の呼び出しを使用して、[!DNL Experience Platform]、[!DNL Analytics]、[!DNL Audience Manager]および[!DNL Target]にデータを送信します。

>[!VIDEO](https://video.tv.adobe.com/v/34148?quality=12&learn=on)

この製品は、ますます多くの使用事例をサポートするように、常に進化し、成長しています。 最新の情報を入手するには、[サポートされているユースケースボード](https://github.com/adobe/alloy/projects/5)をご覧ください。 現在サポートしている使用事例や、可能な限り最適な判断を下すために取り組んでいる使用事例について、この情報を最新の状態に保ちます。

* **まだサポートされていない使用例：** これらの使用例は、将来サポートされる予定です。
* **進行中の使用例：** チームが現在リリース用に完了している使用例です。
* **サポートされる使用例：** これらは、現在サポートされ、機能する使用例です。
* **サポートしない使用例：サポートしな** いと判断した使用例を示します。
