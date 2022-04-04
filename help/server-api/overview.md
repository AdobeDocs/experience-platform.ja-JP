---
title: Edge Network Server API
description: Edge Network Server API の概要と使用方法について説明します。
seo-description: Learn what the Edge Network Server API is and how you can use it.
keywords: データ収集；収集；Adobe Experience Platform Edge Network;server api;
exl-id: 46bd8798-d7f9-405b-9ca8-856ad4aa688c
source-git-commit: 422f859bef8faf292fd7e5fd8b6a8d31967421c1
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Edge Network Server API の概要 {#overview}

Adobe Experience Platform Edge Network は、Adobe Experience CloudまたはAdobe Experience Platform Edge サービスを顧客が操作できるように最適化された方法を提供します。

この [!DNL Edge Network Server API] は、様々なデータ収集、パーソナライゼーション、広告、マーケティングの使用例に使用できます。 この [!DNL Server API] はサーバーで使用できます。 [!DNL IoT] デバイス、セットトップボックス、およびその他の様々なデバイス。

以降 [!DNL Server API] は、読み込みにどのライブラリにも依存せず、Adobe Experience Platform Edge Network やサポートされているソリューションとの迅速なやり取りを提供します。

の利点 [!DNL Server API] アーキテクチャには以下が含まれます。

* ページ読み込み時間の短縮
* 遅延の改善
* ファーストパーティデータ収集
* 合理化された、サーバー側でのサービス間通信

この [!DNL Server API] は、2 つの専用エンドポイントを介したインタラクティブなデータ収集とバッチデータ収集をサポートします。

1. インタラクティブエンドポイントは、高度なセグメント化、パーソナライゼーション、その他のマーケティングの使用例をサポートする、Adobe Experience PlatformおよびAdobe Experience Cloudのサービスとの通信をサポートします。
2. バッチエンドポイントを使用すると、呼び出されるアプリケーションからの応答を受信せずにデータをオンボードする必要がある場合に、リクエストをバッチで送信できます。

この [!DNL Server API] は、次のタイプのリクエストをサポートします。

* 認証済みリクエスト（経由） [Adobe I/O](https://developer.adobe.com/)、新しい `server.adobedc.net` endpoint.
* を介した未認証のリクエスト `edge.adobedc.net` endpoint.

これにより、組織のプライバシーポリシーに従って、機密データの安全で認証済みの収集を許可するユースケースが可能になります。 認証に加え、Server API は、API を介した認証済みの通信のみを受け入れるためのマーキングデータストリームをサポートします。

Server API の効率的な概要については、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/341448/)
