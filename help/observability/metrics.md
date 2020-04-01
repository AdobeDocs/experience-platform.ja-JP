---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 使用可能な指標
topic: developer guide
translation-type: tm+mt
source-git-commit: 947955403a270914437d9172bca458f9c49ccd8f

---


# リスト可能な指標

以下に、監視可能性インサイトによって公開される指標のリストを示します。各指標には、関連するプラットフォームサービス、説明、および受け入れられたIDクエリパラメーターが含まれます。

| インサイト指標 | プラットフォームサービス | 説明 | IDクエリ |
| ---- | ---- | ---- | ---- |
| timeseries.ingestion.dataset.new.count | データ収集 | 作成されたデータセットの合計数。 | なし |
| timeseries.ingestion.dataset.size | データ収集 | 1つまたはすべてのデータセット用に取り込まれたすべてのデータの累積サイズ。 | データセットID（オプション） |
| timeseries.ingestion.dataset.dailysize | データ収集 | 1つのデータセットまたはすべてのデータセットに対して毎日の使用量ベースで取り込まれるデータのサイズ。 | データセットID（オプション） |
| timeseries.ingestion.dataset.batchfailed.count | データ収集 | 1つのデータセットまたはすべてのデータセットで失敗したバッチの数。 | データセットID（オプション） |
| timeseries.ingestion.dataset.batchsuccess.count | データ収集 | 1つのデータセットまたはすべてのデータセットに対して取り込まれたバッチの数。 | データセットID（オプション） |
| timeseries.ingestion.dataset.recordsuccess.count | データ収集 | 1つのデータセットまたはすべてのデータセットに対して取り込まれたレコードの数。 | データセットID（オプション） |
| timeseries.data.collection.validation.total.messages.rate | データ取り込み（ストリーミング） | 1つのデータセットまたはすべてのデータセットのメッセージの合計数。 | データセットID（オプション） |
| timeseries.data.collection.validation.valid.messages.rate | データ取り込み（ストリーミング） | 1つのデータセットまたはすべてのデータセットに対する有効なメッセージの合計数。 | データセットID（オプション） |
| timeseries.data.collection.validation.invalid.messages.rate | データ取り込み（ストリーミング） | 1つのデータセットまたはすべてのデータセットに対する無効なメッセージの合計数。 | データセットID（オプション） |
| timeseries.data.collection.validation.type.count | データ取り込み（ストリーミング） | 1つのデータセットまたはすべてのデータセットに対する無効な「タイプ」メッセージの合計数。 | データセットID（オプション） |
| timeseries.data.collection.validation.them.range.count | データ取り込み（ストリーミング） | 1つのデータセットまたはすべてのデータセットに対する無効な「範囲」メッセージの合計数。 | データセットID（オプション） |
| timeseries.data.collection.validation.them.format.count | データ取り込み（ストリーミング） | 1つのデータセットまたはすべてのデータセットに対する無効な「形式」メッセージの合計数。 | データセットID（オプション） |
| timeseries.data.collection.validation.them.pattern.count | データ取り込み（ストリーミング） | 1つのデータセットまたはすべてのデータセットに対する無効な「パターン」メッセージの合計数。 | データセットID（オプション） |
| timeseries.data.collection.validation.them.presence.count | データ取り込み（ストリーミング） | 1つのデータセットまたはすべてのデータセットに対する無効な「存在」メッセージの合計数。 | データセットID（オプション） |
| timeseries.data.collection.validation.them.enum.count | データ取り込み（ストリーミング） | 1つのデータセットまたはすべてのデータセットに対する無効な「enum」メッセージの合計数。 | データセットID（オプション） |
| timeseries.data.collection.validation.them.unclassified.count | データ取り込み（ストリーミング） | 1つのデータセットまたはすべてのデータセットに対する無効な「未分類」メッセージの合計数。 | データセットID（オプション） |
| timeseries.data.collection.validation.them.unknown.count | データ取り込み（ストリーミング） | 1つのデータセットまたはすべてのデータセットに対する無効な「不明」メッセージの合計数。 | データセットID（オプション） |
| timeseries.data.collection.inlet.total.messages.received | データ取り込み（ストリーミング） | 1つのデータインレットまたはすべてのデータインレットに対して受信したメッセージの合計数。 | インレットID（オプション） |
| timeseries.data.collection.inlet.total.messages.size.received | データ取り込み（ストリーミング） | 1つのデータインレットまたはすべてのデータインレットに対して受信したデータの合計サイズ。 | インレットID（オプション） |
| timeseries.data.collection.inlet.success | データ取り込み（ストリーミング） | 1つのデータインレットまたはすべてのデータインレットに対する成功したHTTP呼び出しの合計数。 | インレットID（オプション） |
| timeseries.data.collection.inlet.failure | データ取り込み（ストリーミング） | 1つのデータインレットまたはすべてのデータインレットに対する、失敗したHTTP呼び出しの合計数です。 | インレットID（オプション） |
| timeseries.dataset.recordread.count | リアルタイム顧客プロファイル | データレークから読み取ったレコードの数(プロファイル別)、1つのデータセットまたはすべてのデータセット用。 | データセットID（オプション） |
| timeseries.dataset.recordsuccess.count | リアルタイム顧客プロファイル | データソース、1つのデータセット、またはすべてのプロファイルセットに対してデータソースに書き込まれたレコードの数。 | データセットID（オプション） |
| timeseries.dataset.recordfailed.count | リアルタイム顧客プロファイル | 1つのデータセットまたはすべてのプロファイルセットに対して、失敗したレコードの数（データベース別）。 | データセットID（オプション） |
| timeseries.dataset.batchsuccess.count | リアルタイム顧客プロファイル | データセットまたはすべてのプロファイルセットに対して取り込まれたデータバッチの数。 | データセットID（オプション） |
| timeseries.dataset.batchfailed.count | リアルタイム顧客プロファイル | 1つのプロファイルセットまたはすべてのデータセットで失敗したデータバッチの数。 | データセットID（オプション） |
| timeseries.identity.dataset.recordsuccess.count | ID サービス | 1つのデータセットまたはすべてのデータセットに対して、IDサービスによってデータソースに書き込まれたレコードの数。 | データセットID（オプション） |
| timeseries.identity.dataset.recordfailed.count | ID サービス | 1つのデータセットまたはすべてのデータセットに対して、IDサービスが失敗したレコードの数。 | データセットID（オプション） |
| timeseries.identity.dataset.namespacecode.recordsuccess.count | ID サービス | ユーザーが正常に取り込んだIDレコードの名前空間数。 | 名前空間ID(**必須**) |
| timeseries.identity.dataset.namespacecode.recordfailed.count | ID サービス | 名前空間が失敗したIDレコードの数。 | 名前空間ID(**必須**) |
| timeseries.identity.dataset.namespacecode.recordskipped.count | ID サービス | ユーザーがスキップしたIDレコードの名前空間数。 | 名前空間ID(**必須**) |
| timeseries.identity.graph.imsorg.uniqueidentities.count | ID サービス | IMS組織のIDグラフに保存される一意のIDの数。 | なし |
| timeseries.identity.graph.imsorg.namespacecode.uniqueidentities.count | ID サービス | ユーザーのIDグラフに保存される一意のIDの数です。名前空間 | 名前空間ID(**必須**) |
| timeseries.identity.graph.imsorg.numidgraphs.count | ID サービス | IMS組織のIDグラフに保存される一意のグラフIDの数。 | なし |
| timeseries.identity.graph.imsorg.graphstrength.uniqueidentities.count | ID サービス | 特定のグラフの強さ（「不明」、「弱」または「強」）のIMS組織のIDグラフに保存される一意のIDの数。 | グラフの強さ(**必須**) |
| timeseries.gdpr.jobs.totaljobs.count | GDPR | GDPRから作成されたジョブの合計数。 | ENV(必&#x200B;**須**) |
| timeseries.gdpr.jobs.completedjobs.count | GDPR | GDPRから完了したジョブの合計数。 | ENV(必&#x200B;**須**) |
| timeseries.gdpr.jobs.errorjobs.count | GDPR | GDPRからのエラー・ジョブの合計数。 | ENV(必&#x200B;**須**) |
| timeseries.queryservice.them.scheduleonce.count | クエリサービス | 定期的でないスケジュール済みクエリの合計数。 | なし |
| timeseries.queryservice.them.scheduledercurring.count | クエリサービス | 定期的なスケジュールされたクエリの合計数。 | なし |
| timeseries.queryservice.クエリ.batchquery.count | クエリサービス | 実行されたバッチクエリの合計数。 | なし |
| timeseries.queryservice.them.scheduledquery.count | クエリサービス | 実行されたスケジュール済みクエリの合計数。 | なし |
| timeseries.queryservice.them.クエリ.interactivequery.count | クエリサービス | 実行されたインタラクティブクエリの合計数。 | なし |
| timeseries.queryservice.クエリ.batchfrompsqlquery.count | クエリサービス | PSQLから実行されたバッチクエリの合計数。 | なし |