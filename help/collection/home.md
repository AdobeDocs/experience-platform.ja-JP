---
keywords: Experience Platform;ホーム;人気のトピック;データ収集;launch;web sdk
solution: Experience Platform
title: データ収集の概要
topic-legacy: overview
description: Adobe Experience Platform での顧客体験に関するデータ収集で使用される様々なテクノロジーについて説明します。
exl-id: 03ce5339-e68d-4adf-8c3c-82846a626dad
source-git-commit: f61a845b915df3d803085fbf528e014c8acd9dbd
workflow-type: ht
source-wordcount: '332'
ht-degree: 100%

---

# データ収集の概要

Adobe Experience Platform は、クライアントサイドのソースから顧客体験データを収集して Adobe Experience Platform Edge Network に送信し、そのデータを強化および変換して、アドビやアドビ以外の宛先に数秒で配信できるようにするテクノロジースイートを提供します。

データ収集は、次のクライアントサイドソースに対してサポートされています。

* Web ベースのアプリケーション
* ネイティブモバイルアプリケーション
* オーバーザトップ（OTT）アプリケーション

Experience Platform が提供するデータ収集テクノロジーは、取り込まれたデータセットの検出性とアクセス性に重点を置いています。これらのテクノロジーには、以下が含まれます。

* [Adobe Experience Platform Edge Network](https://experienceleague.adobe.com/docs/web-sdk-learn/tutorials/introduction-to-web-sdk-and-edge-network.html?lang=ja)
* [タグ](../tags/home.md)
* [イベントの転送](../tags/ui/event-forwarding/overview.md)
* [Adobe Experience Platform Web SDK](../edge/home.md)
* [エクスペリエンスデータモデル（XDM）](../xdm/home.md)

![](./images/Collection.png)

## 簡素な実装とクライアントサイドのパフォーマンスの高速化

Adobe Experience Platform Web およびモバイル SDK では、すべてのアドビ製品ライブラリを折りたたんで圧縮し、Web またはモバイルプラットフォーム向けの単一の開発キットにします。これらのライブラリを圧縮すると、データ収集が高速化され、クライアントサイドデバイスから Adobe Experience Platform Edge Network に至るまで、操作が単一のストリームに統合されます。

## アドビテクノロジーをデプロイするためのスイッチ切り替えプロセス {#edge}

Platform Edge Network は、データを大規模に受信および処理できる、世界中に分散した高速で信頼性の高いサーバーのネットワークです。タグを使用すると、Adobe Target、Adobe Audience Manager、Adobe Analytics などの製品に対して[データストリーム](../edge/fundamentals/datastreams.md)を設定できます。これにより、クライアントサイドのコードを変更せずに、サーバサイドでこれらの製品をアクティブ化できます。

![](./images/deploy.png)

>[!NOTE]
>
>Platform Edge ネットワークの詳細な説明については、次の [インタラクティブ製品ツアー](https://adobe-ideacloud.forgedx.com/adobe-adobe-edge-collection/adobe-experience-edge/public/mx?SUID=hgb1a48ICSCpbM6MzBYHbxnsh9DgjUy1) を参照してください。

## データの迅速かつ安全な変換、拡張、送信

[Adobe Experience Platform のイベント転送](../tags/ui/event-forwarding/overview.md)では、任意の Platform データストリームを利用できます。クライアントデバイスにサードパーティのコードを追加することなく、アドビ以外の任意の宛先に対して、非常に少ない遅延でデータを変換、拡張、および送信することができるため、より高速で安全なデータ収集と配信が実現します。

![](./images/launch.png)
