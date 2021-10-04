---
keywords: Experience Platform；ホーム；人気のあるトピック；kafka;kafka コネクタ；Kafka;
solution: Experience Platform
title: Kafka コネクタ
topic-legacy: overview
description: Adobe Experience Platform のストリームコネクタは、Apache Kafka Connect をベースにしています。このライブラリを使用して、データセンターの Kafka トピックから JSON イベントをリアルタイムで直接Experience Platformにストリーミングできます。
exl-id: 062963e5-c727-4c2c-97db-8a9a5a7d903c
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 29%

---

# [!DNL Kafka]Adobe Experience Platform 用 コネクタ

Adobe Experience Platformのストリームコネクタは、[!DNL Apache Kafka Connect] に基づいています。 このライブラリを使用して、データセンターの [!DNL Kafka] トピックから [!DNL Experience Platform] に JSON イベントをリアルタイムで直接ストリーミングできます。

ストリームコネクタはシンク（一方向）コネクタで、[!DNL Kafka] トピックのデータを [!DNL Experience Platform] の登録済みエンドポイントに配信します。 このコネクタを使用するには、ライブラリをダウンロードし、既存の [!DNL Kafka] デプロイメントに追加して、[!DNL Kafka] トピックをAdobeストリーミングの HTTP URL に設定する必要があります。 追加のコードは必要&#x200B;**ありません**。コネクタでは、次の機能がサポートされています。

- 認証済みのデータ収集
- メッセージのバッチ処理によるネットワーク呼び出しの削減とスループットの向上

コネクタの設定手順など、[!DNL Kafka] コネクタの詳細については、[ 入門ガイド ](https://github.com/adobe/experience-platform-streaming-connect) を参照してください。 ワークフローについて詳しくは、「[Developer Guide](https://www.adobe.com/go/kafka-connector-developer-guide)」を参照してください。
