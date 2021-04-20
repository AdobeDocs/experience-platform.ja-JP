---
keywords: Experience Platform；ホーム；人気のあるトピック；kafka;kafkaコネクタ；Kafka;
solution: Experience Platform
title: カフカコネクタ
topic: overview
description: Adobe Experience Platform のストリームコネクタは、Apache Kafka Connect をベースにしています。このライブラリは、データセンターのKafkaトピックからJSONイベントを直接Experience Platformにリアルタイムでストリーミングするのに使用できます。
translation-type: tm+mt
source-git-commit: 126b3d1cf6d47da73c6ab045825424cf6f99e5ac
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 30%

---


# [!DNL Kafka]Adobe Experience Platform 用 コネクタ

Adobe Experience Platformのストリームコネクタは[!DNL Apache Kafka Connect]を基にしています。 このライブラリは、データセンターの[!DNL Kafka]トピックから[!DNL Experience Platform]にJSONイベントをリアルタイムで直接ストリーミングするのに使用できます。

ストリームコネクタはシンク（一方向）コネクタで、[!DNL Kafka]トピックから[!DNL Experience Platform]の登録済みエンドポイントにデータを配信します。 このコネクタを使用するには、ライブラリをダウンロードし、既存の[!DNL Kafka]デプロイメントに追加して、AdobeストリーミングHTTP URLに[!DNL Kafka]トピックを設定する必要があります。 追加のコードは必要&#x200B;**ありません**。コネクタでは、次の機能がサポートされています。

- 認証済みのデータ収集
- メッセージのバッチ処理によるネットワーク呼び出しの削減とスループットの向上

[!DNL Kafka]コネクタの設定方法など、コネクタの詳細については、[はじめに](https://github.com/adobe/experience-platform-streaming-connect)をお読みください。 ワークフローについて詳しくは、「[Developer Guide](https://www.adobe.com/go/kafka-connector-developer-guide)」を参照してください。