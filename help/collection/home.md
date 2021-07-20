---
keywords: Experience Platform;ホーム;人気のトピック;データ収集;launch;web sdk
solution: Experience Platform
title: データ収集の概要
topic-legacy: overview
description: Adobe Experience Platform での顧客体験に関するデータ収集で使用される様々なテクノロジーについて説明します。
exl-id: 03ce5339-e68d-4adf-8c3c-82846a626dad
source-git-commit: da7696d288543abd21ff8a1402e81dcea32efbc2
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 84%

---

# データ収集の概要

Adobe Experience Platform は、クライアントサイドのソースから顧客体験データを収集して Adobe Experience Platform Edge Network に送信し、そのデータを強化および変換して、アドビやアドビ以外の宛先に数秒で配信できるようにするテクノロジースイートを提供します。

データ収集は、次のクライアントサイドソースに対してサポートされています。

* Web ベースのアプリケーション
* ネイティブモバイルアプリケーション
* オーバーザトップ（OTT）アプリケーション

Experience Platform が提供するデータ収集テクノロジーは、取り込まれたデータセットの検出性とアクセス性に重点を置いています。これらのテクノロジーには、以下が含まれます。

* [Adobe Experience Platform Edge Network](https://experienceleague.adobe.com/docs/web-sdk-learn/tutorials/introduction-to-web-sdk-and-edge-network.html?lang=ja)
* [Adobe Experience Platform Launch](https://experienceleague.adobe.com/docs/launch/using/home.html?lang=ja)
* [Adobe Experience Platform Web SDK](../edge/home.md)
* [エクスペリエンスデータモデル（XDM）](../xdm/home.md)

![](./images/Collection.png)

## 簡素な実装とクライアントサイドのパフォーマンスの高速化

Adobe Experience Platform Web およびモバイル SDK では、すべてのアドビ製品ライブラリを折りたたんで圧縮し、Web またはモバイルプラットフォーム向けの単一の開発キットにします。これらのライブラリを圧縮すると、データ収集が高速化され、クライアントサイドデバイスから Adobe Experience Platform Edge Network に至るまで、操作が単一のストリームに統合されます。

## アドビテクノロジーをデプロイするためのスイッチ切り替えプロセス

Platform Edge Network は、データを大規模に受信および処理できる、世界中に分散した高速で信頼性の高いサーバーのネットワークです。platform launchを使用して、Adobe Target、Adobe Audience Manager、Adobe Analyticsなどの製品に[datastreams](../edge/fundamentals/datastreams.md)を設定し、クライアント側のコードを変更せずに、サーバー側でこれらの製品をアクティブ化できます。

![](./images/deploy.png)

## データの迅速かつ安全な変換、拡張、送信

[Adobe Experience Platformのイベント転送では、任意のPlatform](../tags/ui/event-forwarding/overview.md) データストリームをタップできます。クライアントデバイスにサードパーティのコードを追加することなく、アドビ以外の任意の宛先に対して、非常に少ない遅延でデータを変換、拡張、および送信することができるため、より高速で安全なデータ収集と配信が実現します。

![](./images/launch.png)
