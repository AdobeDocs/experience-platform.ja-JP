---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 使用可能な指標
topic: developer guide
translation-type: tm+mt
source-git-commit: 73a492ba887ddfe651e0a29aac376d82a7a1dcc4
workflow-type: tm+mt
source-wordcount: '878'
ht-degree: 2%

---


# 使用可能な指標のリスト

次の表に、Observibility Insightsで公開されるすべての指標を、サービス別に分類したリストを示し [!DNL Platform] ます。 各指標には、説明と受け入れられたIDクエリパラメーターが含まれます。

## [!DNL Data Ingestion]

次の表に、Adobe Experience Platformの指標の概要を示 [!DNL Data Ingestion]します。 太字の指標 **は、ストリーミング取り込み指標です** 。

| インサイト指標  | 説明 | IDクエリパラメーター |
| ---- | ---- | ---- |
| timeseries.ingestion.dataset.new.count | 作成されたデータセットの合計数。 | 該当なし |
| timeseries.ingestion.dataset.size | 1つまたはすべてのデータセット用に取り込まれたすべてのデータの累積サイズ。 | データセットID（オプション） |
| timeseries.ingestion.dataset.dailysize | 1つのデータセットまたはすべてのデータセットに対して、毎日の使用量ベースで取り込まれるデータのサイズ。 | データセットID（オプション） |
| timeseries.ingestion.dataset.batchfailed.count | 1つのデータセットまたはすべてのデータセットで失敗したバッチの数。 | データセットID（オプション） |
| timeseries.ingestion.dataset.batchsuccess.count | 1つのデータセットまたはすべてのデータセットに対して取り込まれたバッチの数。 | データセットID（オプション） |
| timeseries.ingestion.dataset.recordsuccess.count | 1つのデータセットまたはすべてのデータセットに対して取り込まれたレコードの数。 | データセットID（オプション） |
| **timeseries.data.collection.validation.total.messages.rate** | 1つのデータセットまたはすべてのデータセットに対するメッセージの合計数。 | データセットID（オプション） |
| **timeseries.data.collection.validation.valid.messages.rate** | 1つのデータセットまたはすべてのデータセットに対する有効なメッセージの合計数。 | データセットID（オプション） |
| **timeseries.data.collection.validation.invalid.messages.rate** | 1つのデータセットまたはすべてのデータセットに対する無効なメッセージの合計数。 | データセットID（オプション） |
| **timeseries.data.collection.validation.type.count** | 1つのデータセットまたはすべてのデータセットに対する無効な「タイプ」メッセージの合計数。 | データセットID（オプション） |
| **timeseries.data.collection.validation.カテゴリ.range.count** | 1つのデータセットまたはすべてのデータセットに対する無効な「範囲」メッセージの合計数。 | データセットID（オプション） |
| **timeseries.data.collection.validation.them.format.count** | 1つのデータセットまたはすべてのデータセットに対する無効な「フォーマット」メッセージの合計数。 | データセットID（オプション） |
| **timeseries.data.collection.validation.them.pattern.count** | 1つのデータセットまたはすべてのデータセットに対する無効な「パターン」メッセージの合計数。 | データセットID（オプション） |
| **timeseries.data.collection.validation.カテゴリ.presence.count** | 1つのデータセットまたはすべてのデータセットに対する無効な「存在」メッセージの合計数。 | データセットID（オプション） |
| **timeseries.data.collection.validation.them.enum.count** | 1つのデータセットまたはすべてのデータセットに対する無効な「列挙」メッセージの合計数。 | データセットID（オプション） |
| **timeseries.data.collection.validation.カテゴリ.unclassified.count** | 1つのデータセットまたはすべてのデータセットに対する無効な「未分類」メッセージの合計数。 | データセットID（オプション） |
| **timeseries.data.collection.validation.them.unknown.count** | 1つのデータセットまたはすべてのデータセットに対する無効な「不明」メッセージの合計数。 | データセットID（オプション） |
| **timeseries.data.collection.inlet.total.messages.received** | 1つのデータインレットまたはすべてのデータインレットに対して受信したメッセージの合計数です。 | インレットID（オプション） |
| **timeseries.data.collection.inlet.total.messages.size.received** | 1つのデータインレットまたはすべてのデータインレットに対して受信したデータの合計サイズです。 | インレットID（オプション） |
| **timeseries.data.collection.inlet.success** | 1つのデータインレットまたはすべてのデータインレットに対する成功したHTTP呼び出しの合計数です。 | インレットID（オプション） |
| **timeseries.data.collection.inlet.failure** | 1つのデータインレットまたはすべてのデータインレットに対する、失敗したHTTP呼び出しの合計数です。 | インレットID（オプション） |

## [!DNL Identity Service]

次の表に、Adobe Experience Platformの指標の概要を示 [!DNL Identity Service]します。

| インサイト指標  | 説明 | IDクエリパラメーター |
| ---- | ---- | ---- |
| timeseries.identity.dataset.recordsuccess.count | 1つのデータセットまたはすべてのデータセットに対して、データソースに書き込ま [!DNL Identity Service]れたレコードの数。 | データセットID（オプション） |
| timeseries.identity.dataset.recordfailed.count | 1つのデータセットまたはすべてのデータセット [!DNL Identity Service]に対する、失敗したレコード数。 | データセットID（オプション） |
| timeseries.identity.dataset.namespacecode.recordsuccess.count | 名前空間に正常に取り込まれたIDレコードの数です。 | 名前空間ID(**必須**) |
| timeseries.identity.dataset.namespacecode.recordfailed.count | 名前空間が失敗したIDレコードの数です。 | 名前空間ID(**必須**) |
| timeseries.identity.dataset.namespacecode.recordskipped.count | 名前空間がスキップしたIDレコードの数です。 | 名前空間ID(**必須**) |
| timeseries.identity.graph.imsorg.uniqueidentities.count | IMS組織のIDグラフに格納される一意のIDの数。 | 該当なし |
| timeseries.identity.graph.imsorg.namespacecode.uniqueidentities.count | 名前空間のIDグラフに格納される一意のIDの数。 | 名前空間ID(**必須**) |
| timeseries.identity.graph.imsorg.numidgraphs.count | IMS組織のIDグラフに格納される一意のグラフIDの数。 | 該当なし |
| timeseries.identity.graph.imsorg.graphstrength.uniqueidentities.count | 特定のグラフの強さ（「不明」、「弱」または「強」）のIMS組織のIDグラフに格納される一意のIDの数。 | グラフの強さ(**必須**) |

## [!DNL Privacy Service]

次の表に、Adobe Experience Platformの指標の概要を示 [!DNL Privacy Service]します。

| インサイト指標  | 説明 | IDクエリパラメーター |
| ---- | ---- | ---- |
| timeseries.gdpr.jobs.totaljobs.count | GDPRから作成されたジョブの合計数。 | ENV(**必須**) |
| timeseries.gdpr.jobs.completedjobs.count | GDPRから完了したジョブの合計数。 | ENV(**必須**) |
| timeseries.gdpr.jobs.errorjobs.count | GDPRからのエラー・ジョブの合計数。 | ENV(**必須**) |

## [!DNL Query Service]

次の表に、Adobe Experience Platformの指標の概要を示 [!DNL Query Service]します。

| インサイト指標  | 説明 | IDクエリパラメーター |
| ---- | ---- | ---- |
| timeseries.queryservice.query.scheduleonce.count | 定期的でないスケジュール済みクエリの合計数です。 | 該当なし |
| timeseries.queryservice.query.scheduledrecurring.count | 定期的にスケジュールされたクエリの合計数です。 | 該当なし |
| timeseries.queryservice.query.batchquery.count | 実行されたバッチクエリの合計数。 | 該当なし |
| timeseries.queryservice.query.scheduledquery.count | 実行されたスケジュール済みクエリの合計数。 | 該当なし |
| timeseries.queryservice.query.interactivequery.count | 実行されたインタラクティブクエリの合計数です。 | 該当なし |
| timeseries.queryservice.query.batchfrompsqlquery.count | PSQLから実行されたバッチクエリの合計数です。 | 該当なし |

## [!DNL Real-time Customer Profile]

次の表に、の指標の概要を示し [!DNL Real-time Customer Profile]ます。

| インサイト指標  | 説明 | IDクエリパラメーター |
| ---- | ---- | ---- |
| timeseries.profiles.dataset.recordread.count | 1つのデータセットまたはすべてのデータセット [!DNL Data Lake] に対して、BYから読み取ったレコードの数 [!DNL Profile]。 | データセットID（オプション） |
| timeseries.profiles.dataset.recordsuccess.count | 1つのデータセットまたはすべてのデータセットに対して、データソースに書き込ま [!DNL Profile]れたレコードの数。 | データセットID（オプション） |
| timeseries.profiles.dataset.recordfailed.count | 1つのデータセットまたはすべてのデータセット [!DNL Profile]に対する、失敗したレコード数。 | データセットID（オプション） |
| timeseries.profiles.dataset.batchsuccess.count | データセットまたはすべてのデータセットに取り込む [!DNL Profile] バッチの数。 | データセットID（オプション） |
| timeseries.profiles.dataset.batchfailed.count | 1つのデータセットまたはすべてのデータセットで失敗した [!DNL Profile] バッチの数。 | データセットID（オプション） |
| platform.ups.ingest.streaming.request.m1_rate | 着信要求率。 | IMS組織 |
| platform.ups.ingest.streaming.access.put.success.m1_rate | インジェストの成功率。 | IMS組織 |
| platform.ups.ingest.streaming.records.created.m15_rate | データセットに取り込まれた新しいレコードの割合。 | データセットID |
| platform.ups.ingest.streaming.request.error.created.outOfOrder.m1_rate | データセットに対する作成リクエストの順番が正しくないタイムスタンプレコードの割合。 | データセットID |
| platform.ups.profile-commons.ingest.streaming.dataSet.record.created.timestamp | データセットに対する最後の作成レコードリクエストのタイムスタンプ。 | データセットID |
| platform.ups.ingest.streaming.request.error.updated.outOfOrder.m1_rate | データセットの更新リクエストに対する順番が正しくないタイムスタンプレコードの割合。 | データセットID |
| platform.ups.profile-commons.ingest.streaming.dataSet.record.updated.timestamp | データセットに対する最後の更新レコードリクエストのタイムスタンプ。 | データセットID |
| platform.ups.ingest.streaming.record.size.m1_rate | 平均レコードサイズ。 | IMS組織 |
| platform.ups.ingest.streaming.records.updated.m15_rate | データセットに取り込まれたレコードに対する更新要求の割合。 | データセットID |
