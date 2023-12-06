---
title: Edge Network Server API の概要
description: Edge Network Server API の概要と使用方法について説明します。
source-git-commit: 68174928d3b005d1e5a31b17f3f287e475b5dc86
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 9%

---


# Edge Network Server API の概要 {#overview}

Adobe Experience Platform Edge Network は、Adobe Experience CloudまたはAdobe Experience Platform Edge サービスを顧客が操作できるように最適化された方法を提供します。

The [!DNL Edge Network Server API] は、様々なデータ収集、パーソナライゼーション、広告、マーケティングの使用例に使用できます。 The [!DNL Server API] はサーバーで使用できます。 [!DNL IoT] デバイス、セットトップボックスなど、様々なデバイスを使用できます。

以降 [!DNL Server API] は、読み込みにどのライブラリにも依存せず、Adobe Experience Platform Edge Network やサポートされているソリューションとの迅速なやり取りを提供します。

の利点 [!DNL Server API] アーキテクチャには以下が含まれます。

* ページ読み込み時間の短縮
* 遅延の改善
* ファーストパーティデータ収集
* 合理化された、サーバー側でのサービス間通信

The [!DNL Server API] は、2 つの専用エンドポイントを介したインタラクティブなデータ収集とバッチデータ収集をサポートします。

1. インタラクティブエンドポイントは、高度なセグメント化、パーソナライゼーション、その他のマーケティングの使用例をサポートする、Adobe Experience PlatformおよびAdobe Experience Cloudのサービスとの通信をサポートします。
2. バッチエンドポイントを使用すると、呼び出されるアプリケーションからの応答を受信せずにデータをオンボードする必要がある場合に、リクエストをバッチで送信できます。

The [!DNL Server API] は、次のタイプのリクエストをサポートします。

* 認証済みリクエスト（経由） [Adobe Developer](https://developer.adobe.com/)、 `server.adobedc.net` endpoint.
* を介した未認証のリクエスト `edge.adobedc.net` endpoint.

これにより、組織のプライバシーポリシーに従って、機密データの安全で認証済みの収集を許可するユースケースが可能になります。 認証に加え、Server API は、API を介した認証済みの通信のみを受け入れるためのマーキングデータストリームをサポートします。

Server API の効率的な概要については、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/341448/)
