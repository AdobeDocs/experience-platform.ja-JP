---
title: Adobe Experience Platform Web SDK のヘルプ
seo-title: Adobe Experience Platform Web SDK のヘルプ
description: Adobe Experience Platform Web SDK の概要と、その使用方法を説明します。
seo-description: Adobe Experience Cloud のお客様が　Experience Cloud　の様々なサービスを利用できるようにします 
translation-type: tm+mt
source-git-commit: 9b8bddf39301cdc39bfa5370ef98d99434fc64f8
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 29%

---


# Adobe Experience PlatformWeb SDKとは

Adobe Experience PlatformWeb SDKは、Adobe Experience Cloudのお客様がAdobe Experience Platformエッジネットワークを介してExperience Cloud内の様々なサービスとやり取りすることを可能にする、クライアント側のJavaScriptライブラリです。

次のビデオでは、Adobe Experience PlatformWeb SDKとEdge Networkの概要を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/34141?quality=12&learn=on)

## Adobe Experience Platform Web SDK に置き換わる SDK

Adobe Experience Platform Web SDK は、次の SDK の代わりとなります。

* Visitor.js
* AppMeasurement.js
* AT.js
* DIL.js

これは、既存のライブラリの単なるラッパーではなく、完全なリライトです。目的は、タグを適切な順序で実行する必要があり、ライブラリのバージョン管理の課題との矛盾、依存関係の管理の改善によって、課題を解決することです。 これはExperience Cloudを実装する新しい方法で、 [オープンソースです](https://github.com/adobe/alloy)。

新しいライブラリに加えて、アドビのソリューションに対する HTTP 要求を整理する新しいエンドポイントが追加されました。以前は、Visitor.js は訪問者 ID サービスに対してブロック呼び出しを送信した後、AT.js は Adobe Target に呼び出しを送信、DIL.js は Adobe Audience Manager に呼び出しを送信、最後に AppMeasurement.js は Adobe Analytics に呼び出しを送信していました。This new library and endpoint can retrieve an ID, fetch a [!DNL Target] experience, send data to Audience Manager, and pass the data to the Adobe Experience Platform in a single call.

次のビデオでは、Adobe Experience PlatformWeb SDKとEdge Networkの動作を示します。 このビデオの例では、アドビへの1回の呼び出しを使用して、Experience Platform、Analytics、Audience ManagerおよびTargetにデータを送信します。

>[!VIDEO](https://video.tv.adobe.com/v/34148?quality=12&learn=on)


## はじめに

Adobe Launchの使い始め方に関するクイックチュートリアルについては、「はじめに [」ガイドを](getting-started/quick-start-with-launch.md) 参照することを強くお勧めします。

この製品は、ますます多くの使用事例をサポートするように、常に進化し、成長しています。 最新のバージョンに対応するために、 [サポートされているユースケースボードをご覧ください](https://github.com/adobe/alloy/projects/5)。 現在サポートしている使用事例や、可能な限り最適な判断を下すために取り組んでいる使用事例について、この情報を最新の状態に保ちます。

* __使用事例未サポート__ — これらは、将来サポートされる予定のロードマップ上の使用例です。
* __使用事例が進行中__ — チームが現在リリースのために完了している使用例です。
* __サポートされる使用例__ — 現在サポートされ、機能する使用例です。
* __使用例サポートしない使用例__ — サポートしないと判断した使用例です。
