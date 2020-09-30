---
keywords: Experience Platform;home;popular topics;kafka;kafka connector;Kafka;
solution: Experience Platform
title: Kafka コネクタ
topic: overview
description: Adobe Experience Platform のストリームコネクタは、Apache Kafka Connect をベースにしています。このライブラリを使用して、データセンターの Kafka トピックから Experience Platform に JSON イベントを直接リアルタイムにストリーミングできます。
translation-type: tm+mt
source-git-commit: 4b2df39b84b2874cbfda9ef2d68c4b50d00596ac
workflow-type: tm+mt
source-wordcount: '181'
ht-degree: 48%

---


# [!DNL Kafka]Adobe Experience Platform 用 コネクタ

The stream connector for Adobe Experience Platform is based on [!DNL Apache Kafka Connect]. This library can be used to stream JSON events from [!DNL Kafka] topics in your data center directly to [!DNL Experience Platform] in real-time.

The stream connector is a sink (one-way) connector, delivering data from [!DNL Kafka] topics to a registered endpoint on [!DNL Experience Platform]. To use this connector, you must download the library, add it to your existing [!DNL Kafka] deployment, and configure the [!DNL Kafka] topic(s) to the Adobe Streaming HTTP URL. 追加のコードは必要&#x200B;**ありません**。コネクタでは、次の機能がサポートされています。

- 認証済みのデータ収集
- メッセージのバッチ処理によるネットワーク呼び出しの削減とスループットの向上

For more information on the [!DNL Kafka] connector, including instructions on how to set up the connector, please read the [getting started guide](https://github.com/adobe/experience-platform-streaming-connect). ワークフローについて詳しくは、「[Developer Guide](https://github.com/adobe/experience-platform-streaming-connect/blob/master/DEVELOPER_GUIDE.md)」を参照してください。