---
title: Adobe Experience Platform Web SDK のヘルプ
seo-title: Adobe Experience Platform Web SDK のヘルプ
description: Adobe Experience Platform Web SDK の概要と、その使用方法を説明します。
seo-description: Adobe Experience Cloud のお客様が　Experience Cloud　の様々なサービスを利用できるようにします.
translation-type: tm+mt
source-git-commit: f06c90d6248071894bd9929fbe1150e30c8e7385
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 39%

---


# Adobe Experience Platform Web SDKとは

Adobe Experience Platform Web SDKは、Adobe Experience Cloudのお客様がAdobe Experience Platform Edge Networkを使用してExperience Cloudの様々なサービスとやり取りを行えるクライアント側のJavaScriptライブラリです。

## Adobe Experience Platform Web SDK に置き換わる SDK

Adobe Experience Platform Web SDK は、次の SDK の代わりとなります。

* Visitor.js
* AppMeasurement.js
* AT.js
* DIL.js

これは、既存のライブラリの単なるラッパーではなく、完全なリライトです。タグを適切な順序で実行し、ライブラリのバージョン管理の課題との矛盾、依存関係の管理を改善し、課題を解決するために使用します。これは、Experience Cloudを実装する新しい方法で、 [オープンソースです](https://github.com/adobe/alloy)

新しいライブラリに加えて、アドビのソリューションに対する HTTP 要求を整理する新しいエンドポイントが追加されました。以前は、Visitor.js は訪問者 ID サービスに対してブロック呼び出しを送信した後、AT.js は Adobe Target に呼び出しを送信、DIL.js は Adobe Audience Manager に呼び出しを送信、最後に AppMeasurement.js は Adobe Analytics に呼び出しを送信していました。この新しいライブラリとエンドポイントは、IDの取得、ターゲットエクスペリエンスの取得、オーディエンスマネージャーへのデータの送信、Adobe Experience Platformへの1回の呼び出しでのデータの受け渡しを行うことができます。

## はじめに

launchの使い始め方に関するクイックチュートリアルについては、「はじめに [」ガイドを](getting-started/quick-start-with-launch.md) チェックアウトすることを強くお勧めします。

この製品は、ますます多くの使用事例をサポートするように、常に進化し、成長しています。 最新の情報を入手するために、 [サポートされているユースケースボードをチェックアウトします](https://github.com/adobe/alloy/projects/5)。 現在サポートしている使用事例や、できる限り最適な判断を下せるように取り組んでいる使用例について、この情報を最新の状態に保ちます。

* __使用事例未サポート__ — 将来行う予定の使用事例です。
* __使用事例が進行中__ — チームが現在リリースのために完了している使用例です。
* __サポートされる使用例__ — 現在サポートされ、機能する使用例です。
* __使用例サポートしない使用例__ — サポートしないと判断した使用例です。
