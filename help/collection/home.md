---
keywords: Experience Platform;ホーム;人気のトピック;データ収集;launch;web sdk
solution: Experience Platform
title: データ収集の概要
topic-legacy: overview
description: Adobe Experience Platform での顧客体験に関するデータ収集で使用される様々なテクノロジーについて説明します。
exl-id: 03ce5339-e68d-4adf-8c3c-82846a626dad
source-git-commit: a150c23dffde9431953a019509e9554159052d21
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 100%

---

# データ収集の概要

Adobe Experience Platform は、クライアントサイドのソースから顧客体験データを収集して Adobe Experience Platform Edge Network に送信し、そのデータを強化および変換して、アドビやアドビ以外の宛先に数秒で配信できるようにするテクノロジースイートを提供します。

データ収集は、次のクライアントサイドソースに対してサポートされています。

* Web ベースのアプリケーション
* ネイティブモバイルアプリケーション
* オーバーザトップ（OTT）アプリケーション

Experience Platform が提供するデータ収集テクノロジーは、取り込まれたデータセットの検出性とアクセス性に重点を置いています。 これらのテクノロジーには、以下が含まれます。

* [Adobe Experience Platform Edge Network](https://experienceleague.adobe.com/docs/web-sdk-learn/tutorials/introduction-to-web-sdk-and-edge-network.html?lang=ja)
* [Adobe Experience Platform Launch](https://adobe.com/go/launch_help_en)
* [Adobe Experience Platform Web SDK](../edge/home.md)
* [エクスペリエンスデータモデル（XDM）](../xdm/home.md)

![](./images/Collection.png)

## 簡素な実装とクライアントサイドのパフォーマンスの高速化

Adobe Experience Platform Web およびモバイル SDK では、すべてのアドビ製品ライブラリを折りたたんで圧縮し、Web またはモバイルプラットフォーム向けの単一の開発キットにします。 これらのライブラリを圧縮すると、データ収集が高速化され、クライアントサイドデバイスから Adobe Experience Platform Edge Network に至るまで、操作が単一のストリームに統合されます。

## アドビテクノロジーをデプロイするためのスイッチ切り替えプロセス

Platform Edge Network は、データを大規模に受信および処理できる、世界中に分散した高速で信頼性の高いサーバーのネットワークです。 Platform Launch を使用すると、Adobe Target、Adobe Audience Manager、Adobe Analytics などの製品に対して[エッジ構成](../edge/fundamentals/datastreams.md)を設定できます。これにより、クライアントサイドのコードを変更せずに、サーバサイドでこれらの製品をアクティブ化できます。

![](./images/deploy.png)

## データの迅速かつ安全な変換、拡張、送信

[Adobe Experience Platform Launch サーバーサイド](https://experienceleague.adobe.com/docs/launch/using/server-side-info/server-side-overview.html?lang=ja)では、任意の Platform データストリームを活用できます。クライアントデバイスにサードパーティのコードを追加することなく、アドビ以外の任意の宛先に対して、非常に少ない遅延でデータを変換、拡張、および送信することができるため、より高速で安全なデータ収集と配信が実現します。

![](./images/launch.png)
