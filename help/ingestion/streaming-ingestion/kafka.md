---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: カフカコネクタ
topic: overview
translation-type: tm+mt
source-git-commit: f80b2e1d787d1f8d9fe8ac306422aa7744a69cd3

---


# Kafka connector for Adobe Experience Platform

Adobe Experience Platformのストリームコネクターは、Apache Kafka Connectをベースとしています。 このライブラリを使用して、データセンターのKafkaトピックからExperience PlatformにJSONイベントをリアルタイムで直接ストリーミングできます。

ストリームコネクターはシンク（一方向）コネクターで、Kafkaトピックからエクスペリエンスプラットフォーム上の登録済みエンドポイントにデータを配信します。 このコネクタを使用するには、ライブラリをダウンロードし、既存のKafkaデプロイメントに追加し、KafkaトピックをAdobe Streaming HTTP URLに設定する必要があります。 追加のコードは **不要** 。 コネクタは、次の機能をサポートしています。

- 認証済みのデータ収集
- メッセージのバッチ処理によるネットワーク呼び出しの削減とスループットの向上

Kafkaコネクタの詳細については、コネクタの設定方法を含め、『はじめに』ガイドを参照し [てください](https://github.com/adobe/experience-platform-streaming-connect)。 ワークフローの詳細については、開発者ガイドを参照し [てください](https://github.com/adobe/experience-platform-streaming-connect/blob/master/DEVELOPER_GUIDE.md)。