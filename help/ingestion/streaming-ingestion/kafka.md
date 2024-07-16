---
keywords: Experience Platform；ホーム；人気のトピック；kafka;kafka コネクタ；Kafka;
solution: Experience Platform
title: Kafka コネクタ
description: Adobe Experience Platform のストリームコネクタは、Apache Kafka Connect をベースにしています。このライブラリを使用すると、データセンター内の Kafka トピックから JSON イベントをリアルタイムで直接Experience Platformにストリーミングできます。
exl-id: 062963e5-c727-4c2c-97db-8a9a5a7d903c
source-git-commit: e802932dea38ebbca8de012a4d285eab691231be
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 26%

---

# Adobe Experience Platform用 [!DNL Kafka] コネクタ

Adobe Experience Platformのストリームコネクタは、[!DNL Apache Kafka Connect] に基づいています。 このライブラリを使用すると、データセンター内の [!DNL Kafka] トピックから JSON イベントをリアルタイムで直接 [!DNL Experience Platform] にストリーミングできます。

ストリームコネクタはシンク（一方向）コネクタであり、[!DNL Kafka] トピックから [!DNL Experience Platform] 上の登録されたエンドポイントにデータを配信します。 このコネクタを使用するには、ライブラリをダウンロードして既存の [!DNL Kafka] デプロイメントに追加し、Adobeのストリーミング HTTP URL に対して [!DNL Kafka] トピックを設定する必要があります。 追加のコードは必要&#x200B;**ありません**。コネクタでは、次の機能がサポートされています。

- 認証済みのデータ収集
- メッセージのバッチ処理によるネットワーク呼び出しの削減とスループットの向上

コネクタの設定方法など、[!DNL Kafka] コネクタの詳細については、[ はじめる前に ](https://github.com/adobe/experience-platform-streaming-connect) を参照してください。 ワークフローについて詳しくは、「[Developer Guide](https://www.adobe.com/go/kafka-connector-developer-guide)」を参照してください。
