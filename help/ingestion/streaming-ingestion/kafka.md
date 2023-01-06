---
keywords: Experience Platform；ホーム；人気のトピック；kafka;kafka コネクタ；Kafka;
solution: Experience Platform
title: Kafka コネクタ
description: Adobe Experience Platform のストリームコネクタは、Apache Kafka Connect をベースにしています。このライブラリを使用して、データセンターの Kafka トピックからExperience Platformに JSON イベントを直接リアルタイムでストリーミングできます。
exl-id: 062963e5-c727-4c2c-97db-8a9a5a7d903c
source-git-commit: e802932dea38ebbca8de012a4d285eab691231be
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 29%

---

# [!DNL Kafka]Adobe Experience Platform 用 コネクタ

Adobe Experience Platformのストリームコネクタは、 [!DNL Apache Kafka Connect]. このライブラリを使用して、次の JSON イベントをストリーミングできます： [!DNL Kafka] のトピックを [!DNL Experience Platform] リアルタイムで。

ストリームコネクタは、シンク（一方向）コネクタで、 [!DNL Kafka] 次の登録済みエンドポイントへのトピック： [!DNL Experience Platform]. このコネクタを使用するには、ライブラリをダウンロードし、既存のに追加する必要があります [!DNL Kafka] デプロイメントと設定 [!DNL Kafka] トピックからAdobeストリーミング HTTP URL へ 追加のコードは必要&#x200B;**ありません**。コネクタでは、次の機能がサポートされています。

- 認証済みのデータ収集
- メッセージのバッチ処理によるネットワーク呼び出しの削減とスループットの向上

詳しくは、 [!DNL Kafka] コネクタ（コネクタの設定方法の説明を含む） [入門ガイド](https://github.com/adobe/experience-platform-streaming-connect). ワークフローについて詳しくは、「[Developer Guide](https://www.adobe.com/go/kafka-connector-developer-guide)」を参照してください。
