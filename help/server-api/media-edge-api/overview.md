---
solution: Experience Platform
title: Media Edge API
description: Media Edge API の概要
exl-id: 55c952de-caab-4301-acf2-f7b64cebbb1c
source-git-commit: e300e57df998836a8c388511b446e90499185705
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 86%

---

# Media Edge API の概要

Media Edge API は、Adobe Experience Platform上に作成され、[XDM スキーマ](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html#:~:text=Experience%20Data%20Model%20(XDM)%2C,the%20power%20of%20digital%20experiences)のフレームワーク内でメディアイベントトラッキングデータを提供します。これにより、Media Analytics のお客様は次の機能が利用できるようになります。

* [Adobe Customer Journey Analytics](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-overview/cja-overview.html?lang=ja) を使用すると、お客様は期間、開始、停止の詳細をほぼリアルタイムで取得し、メディア指標を評価して組み合わせることができます。Adobe Analytics から移行するお客様は、Adobe Customer Journey Analytics ですべてのレポート指標を利用できます。

* [Adobe Real-Time Customer Data Platform](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html?lang=ja) を使用すると、お客様はメディア消費データでリアルタイムのプロファイルを強化できます。

* [Adobe Journey Optimizer](https://experienceleague.adobe.com/docs/journey-optimizer/using/get-started/get-started.html?lang=ja) を使用すると、お客様はオムニチャネルキャンペーンを最適化し、メディア使用シグナルに基づいてジャーニーを自動化できます。


## メディアトラッキングデータフローの最適化

[Media Collection API](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/streaming-media-apis/mc-api-overview.html&amp;media-tracking-data-flows) と Media Edge API は両方とも、メディアトラッキングデータを RESTful サービスとして提供します。ただし、Media Edge サービスを使用すると、次のような利点があります。

* これは、XDM スキーマをデータフローに組み込む最も簡単な方法です。

* 呼び出しは、メディアプレーヤーから [Experience Platformエッジネットワーク](https://experienceleague.adobe.com/docs/experience-platform/edge-network-server-api/overview.html?lang=ja).

* 最小限のクロスサーバー呼び出しでメディアイベントを効率的に追跡します。

次の表に、様々なメディア分析ケースで使用可能な Adobe API サービスを示します。

| ユースケース | API サービス |
| -------- | ----------- |
| Adobe Experience Platform ソリューション | Media Edge |
| Real-Time CDP +Customer Journey Analytics | Media Edge |
| Adobe Analytics + Adobe Experience Platform ソリューション | Media Edge |
| Adobe Analytics のみ（既にトラッキング中） | メディアコレクション |

>[!NOTE]
>
> Analtyics の Media Collection API サービスは引き続き XDM データを受信しますが、Media Edge サービスほど最適化されていません。Media Player から送信されるデータによっては、一部の Analytics 専用データを Media Edge API サービス経由でルーティングすることもできます。

次の図に、2 つの API サービスのデータフローを示します。

![Media Analytics データフロー](../assets/edge-api-dataflow.png)

## 次の手順

* Media Edge API の使用について詳しくは、[基本を学ぶドキュメント](getting-started.md)を参照してください。

* Platform Edge の操作について詳しくは、[Experience Platform Edge を使用した Media Analytics のインストール](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/implementation-edge.html)を参照してください。
