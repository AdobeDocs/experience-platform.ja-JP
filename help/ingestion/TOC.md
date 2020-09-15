---
product: experience-platform
audience: user
user-guide-title: Adobe Experience Platformでのデータ取得のヘルプ
breadcrumb-title: Data Ingestion Guide
user-guide-description: Adobe Experience Platform brings data from multiple sources together in order to help marketers better understand the behavior of their customers. Adobe Experience Platform Data Ingestion represents the multiple methods by which Platform ingests data from these sources, as well as how that data is persisted within the Data Lake for use by downstream Platform services.
translation-type: tm+mt
source-git-commit: 2a5d6a9462950007d00f321ca9f3a3c457a3243e
workflow-type: tm+mt
source-wordcount: '111'
ht-degree: 94%

---


# Adobe Experience Platformでのデータ取得 {#ingestion}

- [データ取得の概要](home.md)
- ストリーミング取得 {#streaming}
   - [概要](streaming-ingestion/overview.md)
   - [Kafka コネクタ](streaming-ingestion/kafka.md)
   - [トラブルシューティング](streaming-ingestion/troubleshooting.md)
- バッチ取得 {#batch}
   - [概要](batch-ingestion/overview.md)
   - [バッチ取得 API](batch-ingestion/api-overview.md)
   - [部分的なバッチ取得](batch-ingestion/partial.md)
   - [トラブルシューティング](batch-ingestion/troubleshooting.md)
- チュートリアル {#tutorials}
   - [XDM への CSV ファイルのマッピング](tutorials/map-a-csv-file.md)
   - [UI を使用したバッチデータ取得](tutorials/ingest-batch-data.md)
   - [認証済みストリーミング接続の作成](tutorials/create-authenticated-streaming-connection.md)
   - [ストリーミング接続の作成（API）](tutorials/create-streaming-connection.md)
   - [ストリーミング接続の作成（UI）](tutorials/create-streaming-connection-ui.md)
   - [レコードデータのストリーミング](tutorials/streaming-record-data.md)
   - [時系列データのストリーミング](tutorials/streaming-time-series-data.md)
   - [複数のメッセージのストリーミング](tutorials/streaming-multiple-messages.md)
- データ取得の品質と監視 {#quality}
   - [概要](quality/overview.md)
   - [データフローの監視](quality/monitor-data-flows.md)
   - [エラー診断の取得](quality/error-diagnostics.md)
   - [失敗したバッチの取得](quality/retrieve-failed-batches.md)
   - [ストリーミング取得の検証](quality/streaming-validation.md)
   - [データ取得イベントへのサブスクリプション](quality/subscribe-events.md)
- [ソースコネクタ](source-connectors.md)
- [API リファレンス](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml)
- [プラットフォームのリリースノート](https://docs.adobe.com/content/help/ja-JP/experience-platform/release-notes/latest.html)