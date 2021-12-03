---
keywords: Experience Platform;ホーム;人気のトピック;データ収集;launch;web sdk
solution: Experience Platform
title: データ収集の概要
topic-legacy: overview
description: Adobe Experience Platform での顧客体験に関するデータ収集で使用される様々なテクノロジーについて説明します。
exl-id: 03ce5339-e68d-4adf-8c3c-82846a626dad
source-git-commit: 0926f0a6dc005b1bf278e7a0fa0afe4296d8ad80
workflow-type: ht
source-wordcount: '458'
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
* [データストリーム](../edge/fundamentals/datastreams.md)
* [イベントの転送](../tags/ui/event-forwarding/overview.md)
* [Adobe Experience Platform Web SDK](../edge/home.md)
* [Adobe Experience Platform モバイル SDK](https://aep-sdks.gitbook.io/docs/)
* [Adobe Experience Platform デバッガー](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob?hl=ja)
* [エクスペリエンスデータモデル（XDM）](../xdm/home.md)
* [Adobe Experience Platform ID サービス](../identity-service/home.md)

このガイドでは、データ収集フレームワークの概要と、このフレームワークが Platform Edge Network を介して Adobe Experience Cloud 製品やアドビ以外のアプリケーションにデータを送信する仕組みについて説明します。

## タグ、Web SDK および Mobile SDK

Platform Web SDK と Platform Mobile SDK には、すべてのアドビ製品ライブラリが、それぞれ web とモバイルのプラットフォーム用開発キットとしてまとめられています。 これらは、ローコードを使用するか、データ収集 UI から[タグ](../tags/home.md)を使用して実装できます。

これらのライブラリを圧縮すると、データ収集が高速化され、クライアントサイドデバイスから Platform Edge Network へという単一のストリームに複数の処理が統合されます。

![タグ、Web SDK、Mobile SDK](./images/home/tags-sdks.png)

## Platform Edge Network とデータストリーム {#edge}

Platform Edge Network とは、世界中に分散された信頼性の高い高速サーバーネットワークであり、データを極めて大規模に受信および処理できます。タグを使用すると、Adobe Target、Adobe Audience Manager、Adobe Analytics などの製品に対して[データストリーム](../edge/fundamentals/datastreams.md)を設定できます。これにより、クライアントサイドのコードを変更せずに、サーバサイドでこれらの製品をアクティブ化できます。

![データストリームおよびアドビのソリューション](./images/home/adobe-solutions.png)

>[!NOTE]
>
>Platform Edge Network について詳しくは、次の[インタラクティブ製品ツアー](https://adobe-ideacloud.forgedx.com/adobe-adobe-edge-collection/adobe-experience-edge/public/mx?SUID=hgb1a48ICSCpbM6MzBYHbxnsh9DgjUy1)を参照してください。

## イベント転送

[イベント転送](../tags/ui/event-forwarding/overview.md)では、任意の Experience Platform データストリームを利用できます。そのため、データの変換、エンリッチメントおよびアドビ以外の宛先への送信が可能で、しかも待ち時間は極めて短く、サードパーティのコードをクライアントデバイスに追加する必要もありません。

![イベント転送](./images/home/event-forwarding.png)

## 次の手順

このドキュメントでは、収集した顧客体験データをアドビ製品やサードパーティの宛先に送信するプロセスを自動化するための、Platform のデータ収集技術の仕組みについて概説しました。

![データ収集フレームワーク](./images/home/collection.png)

Edge Network を介したイベントデータの送信に関する一般的なワークフローについて詳しくは、 [データ収集のエンドツーエンドの概要](./e2e.md)を参照してください。
