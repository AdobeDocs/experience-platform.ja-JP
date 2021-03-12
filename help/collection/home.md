---
keywords: Experience Platform；ホーム；人気のあるトピック；データ収集；起動；Web sdk
solution: Experience Platform
title: データ収集の概要
topic: 概要
description: データ収集は、データ収集用にページのタグ付けに重点を置いたツールを提供することで、Adobe Experience Platformをサポートします。
translation-type: tm+mt
source-git-commit: 323bfc6b5eae5463a86761589fe384aedc40a4d6
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 4%

---


# [!DNL Collection] の概要

Adobe Experience Platformコレクションは、Adobe Experience Platformエッジネットワークのクライアント側のデータ収集を可能にするテクノロジースイートです。

* Webサイト
* ネイティブモバイルアプリケーション
* OTT

<!-- * Servers -->

データは、Adobeまたは非Adobeの宛先に、数秒でリンチ、変換、送信できます。

[!DNL Data Collection] で取り込まれたデータセットの検出性とアクセス性に重点を置いたツールを提供することで、Adobe Experience Platformをサポート [!DNL Experience Platform]します。

Adobe Experience Platformコレクション開発者は、次のテクノロジーを備えています。

* Adobe Experience Platformデータストリーム
* [Adobe Experience Platform Edge Network](https://experienceleague.adobe.com/docs/web-sdk-learn/tutorials/introduction-to-web-sdk-and-edge-network.html)
* [Adobe Experience Platformエクスペリエンスデータモデル(XDM)](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html)
* [Adobe Experience Platform Launchクライアント側](https://experienceleague.adobe.com/docs/launch.html)
* [Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html)

![](./images/Collection.png)

## 実装のシンプル化、クライアント側のパフォーマンスの高速化

Adobe Experience PlatformWebおよびモバイルSDKは、すべてのAdobe製品ライブラリを折りたたみ、圧縮して1つのWebまたはモバイルSDKにします。 ライブラリを圧縮すると、これらのデバイスからAdobe Experience Platformエッジネットワークへのデータ収集の速度が上がり、単一のストリームに簡単になります。

Adobe Experience PlatformサーバSDKは、任意のサーバからAdobe Experience Platformエッジネットワークにデータをストリーミングします。

## スイッチを切り替えてAdobeテクノロジを導入するプロセス

Adobe Experience Platformエッジネットワークは、データを大幅に受信および処理できる、グローバルな分散、高速、信頼性の高いサーバネットワークです。 Adobe Experience Platformデータストリームを使用すると、クライアント側のコードを変更せずに、Adobe製品をサーバー側で有効にできます。

![](./images/deploy.png)

## データの迅速かつ安全な変換、拡張、送信

Adobe Experience Platform Launch・サーバ側は、任意のAdobe Experience Platform・データ・ストリームをタップできます。 クライアントデバイスにサードパーティのコードを追加することなく、極端な遅延の少ない、Adobe以外の任意の宛先に対して、データの変換、拡張および送信を行うことができ、より高速で安全なデータ収集と配信を実現できます。

![](./images/launch.png)