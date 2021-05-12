---
keywords: Experience Platform；ホーム；人気のあるトピック；データ収集；起動；Web sdk
solution: Experience Platform
title: データ収集の概要
topic-legacy: overview
description: Adobe Experience Platformでの顧客体験のデータ収集に関連する様々なテクノロジーについて説明します。
exl-id: 03ce5339-e68d-4adf-8c3c-82846a626dad
source-git-commit: a150c23dffde9431953a019509e9554159052d21
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 6%

---

# データ収集の概要

Adobe Experience Platformは、顧客側のソースから顧客体験データを収集し、Adobe Experience Platformエッジネットワークに送信して、ソースを強化、変換し、AdobeやAdobe以外の宛先に数秒で配信できるテクノロジースイートを提供します。

データ収集は、次のクライアント側ソースに対してサポートされています。

* Webベースのアプリケーション
* ネイティブモバイルアプリケーション
* オーバーザトップ(OTT)アプリケーション

Experience Platformが提供するデータ収集テクノロジーは、取り込まれたデータセットの検出性とアクセス性に重点を置いています。 これらのテクノロジーは、次のものを含みます。

* [Adobe Experience Platform Edge Network](https://experienceleague.adobe.com/docs/web-sdk-learn/tutorials/introduction-to-web-sdk-and-edge-network.html)
* [Adobe Experience Platform Launch](https://adobe.com/go/launch_help_en)
* [Adobe Experience Platform Web SDK](../edge/home.md)
* [エクスペリエンスデータモデル（XDM）](../xdm/home.md)

![](./images/Collection.png)

## 実装のシンプル化、クライアント側のパフォーマンスの高速化

Adobe Experience PlatformWebおよびモバイルSDKは、すべてのAdobe製品ライブラリを折りたたみ、圧縮して、Webまたはモバイルプラットフォーム向けの単一の開発キットにします。 これらのライブラリを圧縮すると、データ収集が高速化され、クライアント側デバイスからAdobe Experience Platformエッジネットワークに至るまで、操作が単一のストリームに統合されます。

## スイッチを切り替えてAdobeテクノロジを導入するプロセス

Platform Edge Networkは、データを大幅に受信および処理できる、グローバルな分散、高速、信頼性の高いサーバのネットワークです。 platform launchを使用すると、Adobe Target、Adobe Audience Manager、Adobe Analyticsなどの製品に対して[エッジ構成](../edge/fundamentals/datastreams.md)を設定できます。これにより、クライアント側のコードを変更せずに、サーバ側でこれらの製品をアクティブ化できます。

![](./images/deploy.png)

## データの迅速かつ安全な変換、拡張、送信

[Adobe Experience Platform Launchサーバー](https://experienceleague.adobe.com/docs/launch/using/server-side-info/server-side-overview.html) サイドカンは、任意のプラットフォームデータストリームをタップします。クライアントデバイスにサードパーティのコードを追加することなく、極端な遅延の少ない、Adobe以外の任意の宛先に対して、データの変換、拡張および送信を行うことができ、より高速で安全なデータ収集と配信を実現できます。

![](./images/launch.png)
