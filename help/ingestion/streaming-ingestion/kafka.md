---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: カフカコネクタ
topic: overview
translation-type: tm+mt
source-git-commit: f80b2e1d787d1f8d9fe8ac306422aa7744a69cd3
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---


# Kafka connector for Adobe Experience Platform

Adobe Experience Platform用のストリームコネクターは、Apache Kafka Connectに基づいています。 このライブラリは、データセンターのKafkaトピックからエクスペリエンスプラットフォームにJSONイベントをリアルタイムで直接ストリーミングするのに使用できます。

ストリームコネクターはシンク（一方向）コネクターで、Kafkaトピックからエクスペリエンスプラットフォームの登録済みエンドポイントにデータを配信します。 このコネクタを使用するには、ライブラリをダウンロードし、既存のKafkaデプロイメントに追加して、Adobe Streaming HTTP URLにKafkaトピックを設定する必要があります。 追加のコードは **不要です** 。 コネクタは、次の機能をサポートしています。

- 認証済みのデータ収集
- メッセージのバッチ処理によるネットワーク呼び出しの削減とスループットの向上

Kafkaコネクタの設定手順など、コネクタの詳細については、『 [はじめに](https://github.com/adobe/experience-platform-streaming-connect)』を参照してください。 ワークフローについて詳しくは、 [開発者ガイドを参照してください](https://github.com/adobe/experience-platform-streaming-connect/blob/master/DEVELOPER_GUIDE.md)。