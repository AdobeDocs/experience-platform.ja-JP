---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 使用可能な指標
topic: developer guide
translation-type: tm+mt
source-git-commit: 6a988c990d3d4df706a44cdbc82f77bef20c2ea1

---


# リスト可能な指標

次の表に、Observatibility Insightsで公開されるすべての指標を、プラットフォームサービス別に分類したリストを示します。 各指標には、説明と受け入れられたIDクエリーが含まれます。

## データ収集

次の表に、Adobe Experience Platform Data Ingestの指標の概要を示します。 太字の指標は **ストリーミング** 取り込み指標です。

| インサイト指標 | 説明 | IDクエリ |
| ---- | ---- | ---- | ---- |
| timeseries.ingestion.dataset.new.count | 作成されたデータセットの合計数。 | なし |
| timeseries.ingestion.dataset.size | 1つまたはすべてのデータセット用に取り込まれたすべてのデータの累積サイズ。 | データセットID（オプション） |
| timeseries.ingestion.dataset.dailysize | 1つのデータセットまたはすべてのデータセットに対して毎日の使用量ベースで取り込まれるデータのサイズ。 | データセットID（オプション） |
| timeseries.ingestion.dataset.batchfailed.count | 1つのデータセットまたはすべてのデータセットで失敗したバッチの数。 | データセットID（オプション） |
| timeseries.ingestion.dataset.batchsuccess.count | 1つのデータセットまたはすべてのデータセットに対して取り込まれたバッチの数。 | データセットID（オプション） |
| timeseries.ingestion.dataset.recordsuccess.count | 1つのデータセットまたはすべてのデータセットに対して取り込まれたレコードの数。 | データセットID（オプション） |
| **timeseries.data.collection.validation.total.messages.rate** | 1つのデータセットまたはすべてのデータセットのメッセージの合計数。 | データセットID（オプション） |
| **timeseries.data.collection.validation.valid.messages.rate** | 1つのデータセットまたはすべてのデータセットに対する有効なメッセージの合計数。 | データセットID（オプション） |
| **timeseries.data.collection.validation.invalid.messages.rate** | 1つのデータセットまたはすべてのデータセットに対する無効なメッセージの合計数。 | データセットID（オプション） |
| **timeseries.data.collection.validation.type.count** | 1つのデータセットまたはすべてのデータセットに対する無効な「タイプ」メッセージの合計数。 | データセットID（オプション） |
| **timeseries.data.collection.validation.them.range.count** | 1つのデータセットまたはすべてのデータセットに対する無効な「範囲」メッセージの合計数。 | データセットID（オプション） |
| **timeseries.data.collection.validation.them.format.count** | 1つのデータセットまたはすべてのデータセットに対する無効な「形式」メッセージの合計数。 | データセットID（オプション） |
| **timeseries.data.collection.validation.them.pattern.count** | 1つのデータセットまたはすべてのデータセットに対する無効な「パターン」メッセージの合計数。 | データセットID（オプション） |
| **timeseries.data.collection.validation.them.presence.count** | 1つのデータセットまたはすべてのデータセットに対する無効な「存在」メッセージの合計数。 | データセットID（オプション） |
| **timeseries.data.collection.validation.them.enum.count** | 1つのデータセットまたはすべてのデータセットに対する無効な「enum」メッセージの合計数。 | データセットID（オプション） |
| **timeseries.data.collection.validation.them.unclassified.count** | 1つのデータセットまたはすべてのデータセットに対する無効な「未分類」メッセージの合計数。 | データセットID（オプション） |
| **timeseries.data.collection.validation.them.unknown.count** | 1つのデータセットまたはすべてのデータセットに対する無効な「不明」メッセージの合計数。 | データセットID（オプション） |
| **timeseries.data.collection.inlet.total.messages.received** | 1つのデータインレットまたはすべてのデータインレットに対して受信したメッセージの合計数。 | インレットID（オプション） |
| **timeseries.data.collection.inlet.total.messages.size.received** | 1つのデータインレットまたはすべてのデータインレットに対して受信したデータの合計サイズ。 | インレットID（オプション） |
| **timeseries.data.collection.inlet.success** | 1つのデータインレットまたはすべてのデータインレットに対する成功したHTTP呼び出しの合計数。 | インレットID（オプション） |
| **timeseries.data.collection.inlet.failure** | 1つのデータインレットまたはすべてのデータインレットに対する、失敗したHTTP呼び出しの合計数です。 | インレットID（オプション） |

## ID サービス

次の表に、Adobe Experience Platform IDサービスの指標の概要を示します。

| インサイト指標 | 説明 | IDクエリ |
| ---- | ---- | ---- | ---- |
| timeseries.identity.dataset.recordsuccess.count | 1つのデータセットまたはすべてのデータセットに対して、IDサービスによってデータソースに書き込まれたレコードの数。 | データセットID（オプション） |
| timeseries.identity.dataset.recordfailed.count | 1つのデータセットまたはすべてのデータセットに対して、IDサービスが失敗したレコードの数。 | データセットID（オプション） |
| timeseries.identity.dataset.namespacecode.recordsuccess.count | ユーザーが正常に取り込んだIDレコードの名前空間数。 | 名前空間ID(**必須**) |
| timeseries.identity.dataset.namespacecode.recordfailed.count | 名前空間が失敗したIDレコードの数。 | 名前空間ID(**必須**) |
| timeseries.identity.dataset.namespacecode.recordskipped.count | ユーザーがスキップしたIDレコードの名前空間数。 | 名前空間ID(**必須**) |
| timeseries.identity.graph.imsorg.uniqueidentities.count | IMS組織のIDグラフに保存される一意のIDの数。 | なし |
| timeseries.identity.graph.imsorg.namespacecode.uniqueidentities.count | ユーザーのIDグラフに保存される一意のIDの数です。名前空間 | 名前空間ID(**必須**) |
| timeseries.identity.graph.imsorg.numidgraphs.count | IMS組織のIDグラフに保存される一意のグラフIDの数。 | なし |
| timeseries.identity.graph.imsorg.graphstrength.uniqueidentities.count | 特定のグラフの強さ（「不明」、「弱」または「強」）のIMS組織のIDグラフに保存される一意のIDの数。 | グラフの強さ(**必須**) |

## プライバシーサービス

次の表に、Adobe Experience Platformプライバシーサービスの指標の概要を示します。

| インサイト指標 | 説明 | IDクエリ |
| ---- | ---- | ---- | ---- |
| timeseries.gdpr.jobs.totaljobs.count | GDPRから作成されたジョブの合計数。 | ENV(必&#x200B;**須**) |
| timeseries.gdpr.jobs.completedjobs.count | GDPRから完了したジョブの合計数。 | ENV(必&#x200B;**須**) |
| timeseries.gdpr.jobs.errorjobs.count | GDPRからのエラー・ジョブの合計数。 | ENV(必&#x200B;**須**) |

## クエリサービス

次の表に、Adobe Experience Platformプラットフォームクエリサービスの指標の概要を示します。

| インサイト指標 | 説明 | IDクエリ |
| ---- | ---- | ---- | ---- |
| timeseries.queryservice.query.scheduleonce.count | 定期的でないスケジュール済みクエリの合計数。 | なし |
| timeseries.queryservice.query.scheduledrecurring.count | 定期的なスケジュールされたクエリの合計数。 | なし |
| timeseries.queryservice.query.batchquery.count | 実行されたバッチクエリの合計数。 | なし |
| timeseries.queryservice.query.scheduledquery.count | 実行されたスケジュール済みクエリの合計数。 | なし |
| timeseries.queryservice.query.interactivequery.count | 実行されたインタラクティブクエリの合計数。 | なし |
| timeseries.queryservice.query.batchfrompsqlquery.count | PSQLから実行されたバッチクエリの合計数。 | なし |

## リアルタイム顧客プロファイル

次の表に、リアルタイム顧客プロファイルの指標

| インサイト指標 | 説明 | IDクエリ |
| ---- | ---- | ---- | ---- |
| timeseries.profiles.dataset.recordread.count | データレークから読み取ったレコードの数(プロファイル別)、1つのデータセットまたはすべてのデータセット用。 | データセットID（オプション） |
| timeseries.profiles.dataset.recordsuccess.count | データソース、1つのデータセット、またはすべてのプロファイルセットに対してデータソースに書き込まれたレコードの数。 | データセットID（オプション） |
| timeseries.profiles.dataset.recordfailed.count | 1つのデータセットまたはすべてのプロファイルセットに対して、失敗したレコードの数（データベース別）。 | データセットID（オプション） |
| timeseries.profiles.dataset.batchsuccess.count | データセットまたはすべてのプロファイルセットに対して取り込まれたデータバッチの数。 | データセットID（オプション） |
| timeseries.profiles.dataset.batchfailed.count | 1つのプロファイルセットまたはすべてのデータセットで失敗したデータバッチの数。 | データセットID（オプション） |
| platform.ups.ingest.streaming.request.m1_rate | 受信要求の速度。 | IMS組織 |
| platform.ups.ingest.streaming.access.put.success.m1_rate | インジェストの成功率。 | IMS組織 |
| platform.ups.ingest.streaming.records.created.m15_rate | データセットに取り込まれた新しいレコードの割合。 | データセットID |
| platform.ups.ingest.streaming.request.error.created.outOfOrder.m1_rate | データセットの作成リクエストに対する、順不同のタイムスタンプレコードの割合。 | データセットID |
| platform.ups.profile-commons.ingest.streaming.dataSet.record.created.timestamp | データセットに対する最後のレコード作成リクエストのタイムスタンプ。 | データセットID |
| platform.ups.ingest.streaming.request.error.updated.outOfOrder.m1_rate | データセットの更新要求のタイムスタンプが正しくないレコードの割合。 | データセットID |
| platform.ups.profile-commons.ingest.streaming.dataSet.record.updated.timestamp | データセットに対する最後の更新レコードリクエストのタイムスタンプ。 | データセットID |
| platform.ups.ingest.streaming.record.size.m1_rate | 平均レコードサイズ。 | IMS組織 |
| platform.ups.ingest.streaming.records.updated.m15_rate | データセットに取り込まれたレコードの更新要求の割合。 | データセットID |
