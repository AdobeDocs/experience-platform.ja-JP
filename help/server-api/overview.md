---
title: Edge Network Server API の概要
description: Edge Network Server API の概要と使用方法について説明します。
exl-id: 46bd8798-d7f9-405b-9ca8-856ad4aa688c
source-git-commit: 041a1782442df5f08bb52e4e450734a51c7781ea
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 9%

---

# Edge Network Server API の概要 {#overview}

Adobe Experience Platform Edge Networkは、Adobe Experience CloudまたはAdobe Experience Platform Edge サービスの最適な利用方法をお客様に提供します。

[!DNL Edge Network Server API] は、様々なデータ収集、パーソナライゼーション、広告、マーケティングのユースケースに使用できます。 [!DNL Server API] は、サーバー、[!DNL IoT] デバイス、セットトップボックス、その他の様々なデバイスで使用できます。

この [!DNL Server API] は、読み込むライブラリに依存しないので、Adobe Experience Platform Edge Networkやサポートされているソリューションを素早く操作できます。

[!DNL Server API] アーキテクチャのメリットには、次のようなものがあります。

* ページの読み込み時間の短縮
* 待ち時間の改善
* ファーストパーティデータ収集
* サービス間の合理化されたサーバーサイドの通信

[!DNL Server API] は、次の 2 つの専用エンドポイントを介したインタラクティブなバッチのデータ収集をサポートしています。

1. インタラクティブエンドポイントは、高度なセグメント化、パーソナライゼーション、その他のマーケティングユースケースをサポートするAdobe Experience PlatformおよびAdobe Experience Cloud サービスとのコミュニケーションをサポートします。
2. バッチエンドポイントを使用すると、呼び出されるアプリケーションから応答を受け取ることなくデータをオンボードする必要がある場合、リクエストをバッチで送信できます。

[!DNL Server API] は、次のタイプのリクエストをサポートしています。

* `server.adobedc.net` エンドポイントを使用して、[Adobe Developer](https://developer.adobe.com/) 経由で認証済みのリクエスト。
* `edge.adobedc.net` エンドポイントを介した未認証のリクエスト。

これにより、組織のプライバシーポリシーに従って、機密データの安全で認証された収集を可能にするユースケースが可能になります。 認証に加えて、Server API は、API 経由での認証済み通信のみを受け入れるようにデータストリームのマークをサポートしています。

Server API の合理化された概要については、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/341448/)
