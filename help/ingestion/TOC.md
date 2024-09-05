---
audience: user
user-guide-title: Adobe Experience Platformでのデータ取得のヘルプ
breadcrumb-title: データ取り込みガイド
user-guide-description: バッチまたはストリーミング取り込みを使用して、データを Experience Platform に取り込みます。
feature: Data Ingestion
role: Developer
source-git-commit: 1070c34bcd4577fcc5f0ac160196450db3aab9b0
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 95%

---


# Adobe Experience Platform のデータ取り込み {#ingestion}

- [データ取り込みの概要](home.md)
- ストリーミング取得 {#streaming}
   - [概要](streaming-ingestion/overview.md)
   - [Kafka コネクタ](streaming-ingestion/kafka.md)
   - [トラブルシューティング](streaming-ingestion/troubleshooting.md)
- バッチ取得 {#batch}
   - [バッチ取得 API の概要](batch-ingestion/getting-started.md)
   - [API の概要](batch-ingestion/overview.md)
   - [API デベロッパーガイド](batch-ingestion/api-overview.md)
   - [部分バッチ取得](batch-ingestion/partial.md)
   - [トラブルシューティング](batch-ingestion/troubleshooting.md)
- チュートリアル {#tutorials}
   - CSV ファイルの XDM {#map-csv} へのマッピング
      - [概要](./tutorials/map-csv/overview.md)
      - [既存のスキーマへの CSV ファイルのマッピング](./tutorials/map-csv/existing-schema.md)
      - [AI で生成されたレコメンデーションを使用した CSV ファイルのマッピング](./tutorials/map-csv/recommendations.md)
   - [UI を使用したバッチデータ取得](tutorials/ingest-batch-data.md)
   - [認証済みストリーミング接続の作成](tutorials/create-authenticated-streaming-connection.md)
   - [ストリーミング接続の作成（API）](tutorials/create-streaming-connection.md)
   - [ストリーミング接続の作成（UI）](tutorials/create-streaming-connection-ui.md)
   - [レコードデータのストリーミング](tutorials/streaming-record-data.md)
   - [時系列データのストリーミング](tutorials/streaming-time-series-data.md)
   - [複数のメッセージのストリーミング](tutorials/streaming-multiple-messages.md)
- データ品質と監視 {#quality}
   - [概要](quality/overview.md)
   - [データ取得の監視](quality/monitor-data-ingestion.md)
   - [エラー診断の取得](quality/error-diagnostics.md)
   - [失敗したバッチの取得](quality/retrieve-failed-batches.md)
   - [ストリーミング取得検証](quality/streaming-validation.md)
- [データ取り込みのガードレール](guardrails.md)
- [ソースコネクタ](source-connectors.md)
- [バッチ取り込み API リファレンス](https://developer.adobe.com/experience-platform-apis/references/batch-ingestion/)
- [ストリーミング取得 API リファレンス](https://developer.adobe.com/experience-platform-apis/references/streaming-ingestion/)
- [Platform リリースノート](https://experienceleague.adobe.com/ja/docs/experience-platform/release-notes/latest)
