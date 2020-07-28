---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 使用可能な指標
topic: developer guide
translation-type: tm+mt
source-git-commit: 73a492ba887ddfe651e0a29aac376d82a7a1dcc4
workflow-type: tm+mt
source-wordcount: '878'
ht-degree: 82%

---


# 使用可能な指標のリスト

The following tables list all of the metrics that are exposed by Observability Insights, broken down by [!DNL Platform] service. 各指標には、説明と受け入れられた ID クエリーパラメーターが含まれます。

## [!DNL Data Ingestion]

The following table outlines metrics for Adobe Experience Platform [!DNL Data Ingestion]. **太字**&#x200B;の指標はストリーミング取得指標です。

| インサイト指標 | 説明 | ID クエリーパラメーター |
| ---- | ---- | ---- |
| timeseries.ingestion.dataset.new.count | 作成されたデータセットの合計数。 | なし |
| timeseries.ingestion.dataset.size | 1 つまたはすべてのデータセット用に取得されたすべてのデータの累積サイズ。 | データセット ID（オプション） |
| timeseries.ingestion.dataset.dailysize | 1 つのデータセットまたはすべてのデータセットに対して毎日の使用量ベースで取得されるデータのサイズ。 | データセット ID（オプション） |
| timeseries.ingestion.dataset.batchfailed.count | 1 つのデータセットまたはすべてのデータセットで失敗したバッチの数。 | データセット ID（オプション） |
| timeseries.ingestion.dataset.batchsuccess.count | 1 つのデータセットまたはすべてのデータセットに対して取得されたバッチの数。 | データセット ID（オプション） |
| timeseries.ingestion.dataset.recordsuccess.count | 1 つのデータセットまたはすべてのデータセットに対して取得されたレコードの数。 | データセット ID（オプション） |
| **timeseries.data.collection.validation.total.messages.rate** | 1 つのデータセットまたはすべてのデータセットのメッセージの合計数。 | データセット ID（オプション） |
| **timeseries.data.collection.validation.valid.messages.rate** | 1 つのデータセットまたはすべてのデータセットに対する有効なメッセージの合計数。 | データセット ID（オプション） |
| **timeseries.data.collection.validation.invalid.messages.rate** | 1 つのデータセットまたはすべてのデータセットに対する無効なメッセージの合計数。 | データセット ID（オプション） |
| **timeseries.data.collection.validation.category.type.count** | 1 つのデータセットまたはすべてのデータセットに対する無効な「タイプ」メッセージの合計数。 | データセット ID（オプション） |
| **timeseries.data.collection.validation.category.range.count** | 1 つのデータセットまたはすべてのデータセットに対する無効な「範囲」メッセージの合計数。 | データセット ID（オプション） |
| **timeseries.data.collection.validation.category.format.count** | 1 つのデータセットまたはすべてのデータセットに対する無効な「形式」メッセージの合計数。 | データセット ID（オプション） |
| **timeseries.data.collection.validation.category.pattern.count** | 1 つのデータセットまたはすべてのデータセットに対する無効な「パターン」メッセージの合計数。 | データセット ID（オプション） |
| **timeseries.data.collection.validation.category.presence.count** | 1 つのデータセットまたはすべてのデータセットに対する無効な「存在」メッセージの合計数。 | データセット ID（オプション） |
| **timeseries.data.collection.validation.category.enum.count** | 1 つのデータセットまたはすべてのデータセットに対する無効な「enum」メッセージの合計数。 | データセット ID（オプション） |
| **timeseries.data.collection.validation.category.unclassified.count** | 1 つのデータセットまたはすべてのデータセットに対する無効な「未分類」メッセージの合計数。 | データセット ID（オプション） |
| **timeseries.data.collection.validation.category.unknown.count** | 1 つのデータセットまたはすべてのデータセットに対する無効な「不明」メッセージの合計数。 | データセット ID（オプション） |
| **timeseries.data.collection.inlet.total.messages.received** | 1 つのデータインレットまたはすべてのデータインレットに対して受信したメッセージの合計数。 | インレット ID（オプション） |
| **timeseries.data.collection.inlet.total.messages.size.received** | 1 つのデータインレットまたはすべてのデータインレットに対して受信したデータの合計サイズ。 | インレット ID（オプション） |
| **timeseries.data.collection.inlet.success** | 1 つのデータインレットまたはすべてのデータインレットに対する成功した HTTP 呼び出しの合計数。 | インレット ID（オプション） |
| **timeseries.data.collection.inlet.failure** | 1 つのデータインレットまたはすべてのデータインレットに対する失敗した HTTP 呼び出しの合計数。 | インレット ID（オプション） |

## [!DNL Identity Service]

The following table outlines metrics for Adobe Experience Platform [!DNL Identity Service].

| インサイト指標 | 説明 | ID クエリーパラメーター |
| ---- | ---- | ---- |
| timeseries.identity.dataset.recordsuccess.count | Number of records written to their data source by [!DNL Identity Service], for one dataset or all datasets. | データセット ID（オプション） |
| timeseries.identity.dataset.recordfailed.count | Number of records failed by [!DNL Identity Service], for one dataset or for all datasets. | データセット ID（オプション） |
| timeseries.identity.dataset.namespacecode.recordsuccess.count | ユーザーが正常に取得した ID レコードの名前空間数。 | 名前空間 ID（**必須**） |
| timeseries.identity.dataset.namespacecode.recordfailed.count | 名前空間が失敗した ID レコードの数。 | 名前空間 ID（**必須**） |
| timeseries.identity.dataset.namespacecode.recordskipped.count | 名前空間がスキップした ID レコードの数。 | 名前空間 ID（**必須**） |
| timeseries.identity.graph.imsorg.uniqueidentities.count | IMS 組織の ID グラフに保存される一意の ID の数。 | なし |
| timeseries.identity.graph.imsorg.namespacecode.uniqueidentities.count | 名前空間の ID グラフに保存される一意の ID の数。 | 名前空間 ID（**必須**） |
| timeseries.identity.graph.imsorg.numidgraphs.count | IMS 組織 の ID グラフに保存される一意のグラフ ID の数。 | なし |
| timeseries.identity.graph.imsorg.graphstrength.uniqueidentities.count | 特定のグラフの強さ（「不明」、「弱」または「強」）の IMS 組織の ID グラフに保存される一意の ID の数。 | グラフの強さ（**必須**） |

## [!DNL Privacy Service]

The following table outlines metrics for Adobe Experience Platform [!DNL Privacy Service].

| インサイト指標 | 説明 | ID クエリーパラメーター |
| ---- | ---- | ---- |
| timeseries.gdpr.jobs.totaljobs.count | GDPR から作成されたジョブの合計数。 | ENV(必&#x200B;**須**) |
| timeseries.gdpr.jobs.completedjobs.count | GDPR から完了したジョブの合計数。 | ENV(必&#x200B;**須**) |
| timeseries.gdpr.jobs.errorjobs.count | GDPR からのエラージョブの合計数。 | ENV(必&#x200B;**須**) |

## [!DNL Query Service]

The following table outlines metrics for Adobe Experience Platform [!DNL Query Service].

| インサイト指標 | 説明 | ID クエリーパラメーター |
| ---- | ---- | ---- |
| timeseries.queryservice.query.scheduleonce.count | 定期的でないスケジュール済みクエリの合計数。 | なし |
| timeseries.queryservice.query.scheduledrecurring.count | 定期的なスケジュール済みクエリの合計数。 | なし |
| timeseries.queryservice.query.batchquery.count | 実行されたバッチクエリの合計数。 | なし |
| timeseries.queryservice.query.scheduledquery.count | 実行されたスケジュール済みクエリの合計数。 | なし |
| timeseries.queryservice.query.interactivequery.count | 実行されたインタラクティブクエリの合計数。 | なし |
| timeseries.queryservice.query.batchfrompsqlquery.count | PSQL から実行されたバッチクエリの合計数。 | なし |

## [!DNL Real-time Customer Profile]

The following table outlines metrics for [!DNL Real-time Customer Profile].

| インサイト指標 | 説明 | ID クエリーパラメーター |
| ---- | ---- | ---- |
| timeseries.profiles.dataset.recordread.count | Number of records read from the [!DNL Data Lake] by [!DNL Profile], for one dataset or for all datasets. | データセット ID（オプション） |
| timeseries.profiles.dataset.recordsuccess.count | Number of records written to their data source by [!DNL Profile], for one dataset or for all datasets. | データセット ID（オプション） |
| timeseries.profiles.dataset.recordfailed.count | Number of records failed by [!DNL Profile], for one dataset or for all datasets. | データセット ID（オプション） |
| timeseries.profiles.dataset.batchsuccess.count | Number of [!DNL Profile] batches ingested for a dataset or for all datasets. | データセット ID（オプション） |
| timeseries.profiles.dataset.batchfailed.count | Number of [!DNL Profile] batches failed for one dataset or for all datasets. | データセット ID（オプション） |
| platform.ups.ingest.streaming.request.m1_rate | 受信要求の速度。 | IMS 組織 |
| platform.ups.ingest.streaming.access.put.success.m1_rate | 取得成功率。 | IMS 組織 |
| platform.ups.ingest.streaming.records.created.m15_rate | データセットに対して取得された新しいレコードの割合。 | データセット ID |
| platform.ups.ingest.streaming.request.error.created.outOfOrder.m1_rate | データセットの作成要求のタイムスタンプが正しくないレコードの割合。 | データセット ID |
| platform.ups.profile-commons.ingest.streaming.dataSet.record.created.timestamp | データセットに対する最後のレコード作成要求のタイムスタンプ。 | データセット ID |
| platform.ups.ingest.streaming.request.error.updated.outOfOrder.m1_rate | データセットの更新要求のタイムスタンプが正しくないレコードの割合。 | データセット ID |
| platform.ups.profile-commons.ingest.streaming.dataSet.record.updated.timestamp | データセットに対する最後のレコード更新要求のタイムスタンプ。 | データセット ID |
| platform.ups.ingest.streaming.record.size.m1_rate | 平均レコードサイズ。 | IMS 組織 |
| platform.ups.ingest.streaming.records.updated.m15_rate | データセットに対して取得された更新要求の割合。 | データセット ID |
