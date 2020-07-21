---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: カフカコネクタ
topic: overview
translation-type: tm+mt
source-git-commit: bfbf2074a9dcadd809de043d62f7d2ddaa7c7b31
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 0%

---


# [!DNL Kafka] Adobe Experience Platform用コネクタ

Adobe Experience Platform用ストリームコネクタは、に基づいてい [!DNL Apache Kafka Connect]ます。 このライブラリは、データセンターのトピックからJSONイベントを直接リアルタイムにストリーミングするため [!DNL Kafka][!DNL Experience Platform] に使用できます。

ストリームコネクタはシンク（一方向）コネクタで、トピックからのデータを上の登録されたエンドポイントに配信 [!DNL Kafka] し [!DNL Experience Platform]ます。 このコネクタを使用するには、ライブラリをダウンロードし、既存の [!DNL Kafka] デプロイメントに追加して、Adobe Streaming HTTP URLにト [!DNL Kafka] ピックを設定する必要があります。 追加のコードは **不要です** 。 コネクタは、次の機能をサポートしています。

- 認証済みのデータ収集
- メッセージのバッチ処理によるネットワーク呼び出しの削減とスループットの向上

コネクタの設定手順など、 [!DNL Kafka] コネクタの詳細については、『 [はじめに](https://github.com/adobe/experience-platform-streaming-connect)』を参照してください。 ワークフローについて詳しくは、 [開発者ガイドを参照してください](https://github.com/adobe/experience-platform-streaming-connect/blob/master/DEVELOPER_GUIDE.md)。