---
keywords: Experience Platform;ホーム;人気のトピック;日付範囲
title: 標準アラートルール
description: このドキュメントでは、Experience Platform が提供する事前定義済みのアラートルールについて説明します。
feature: Alerts
exl-id: b4af1c15-b1bc-4e4b-a447-09cc17a63988
source-git-commit: d8ada2de0ee0408e4e10f0dc45652af6eb6352cf
workflow-type: tm+mt
source-wordcount: '728'
ht-degree: 16%

---

# 標準アラートルール

Adobe Experience Platform には、組織に対して有効にできる定義済みのアラートルールがいくつか用意されています。 このドキュメントでは、これらのAdobeが提供するアラートルールの詳細を説明します。 Experience Platform のアラートに関する一般的な情報については、[アラートの概要](./overview.md)を参照してください。

条件 [Platform UI でのアラートルールの表示](./ui.md)を使用すると、各ルールを個別に購読できます。 を通じてアラートを購読する場合 [I/O イベント通知](./subscribe.md)ただし、アラートルールは、異なるサブスクリプションパッケージに整理されます。 以下の表では、各ルールに対応する I/O イベント購読名が表示されます。

## データ取得

次のアラートルールは、 [データ取り込み](../../ingestion/home.md) および  [ソース](../../sources/home.md):

| I/O イベントの購読 | アラートルール | 説明 |
| --- | --- | --- |
| ソースフロー実行情報 | ソースフロー実行開始 | このアラートは、ソース接続がデータの処理を開始したときにトリガーします。 |
| ソースフロー実行情報 | ソースフロー実行の成功 | このアラートは、データがソース接続から正常に取り込まれたときにトリガーします。 |
| ソースフロー実行の遅延、失敗、エラー | ソースフロー実行の失敗 | このアラートは、ソース接続からデータを取り込む際にエラーが発生した場合にトリガーします。 |
| ソースフロー実行の遅延、失敗、エラー | 取り込み遅延 | このアラートは、バッチ取得フローの実行が処理に 150 分以上かかる場合にトリガーします。 |

{style=&quot;table-layout:auto&quot;}

## ID サービス

次のアラートルールは、 [ID サービス](../../identity-service/home.md):

| I/O イベントの購読 | アラートルール | 説明 |
| --- | --- | --- |
| ID 取り込み情報 | ID サービスフロー実行開始 | このアラートは、ID サービスフローの実行がデータの処理を開始したときにトリガーします。 つまり、取り込んだデータはデータレイクから ID サービスに読み込まれています。 |
| ID 取り込み情報 | ID サービスフロー実行成功 | このアラートは、データがデータレイクから ID サービスに正常に読み込まれた場合にトリガーします。 |
| ID 取り込みの遅延、エラー、エラー | ID サービスフロー実行遅延 | このアラートは、ID サービスのフローの実行が処理に 150 分以上かかった場合にトリガーします。 |
| ID 取り込みの遅延、エラー、エラー | ID サービスフロー実行エラー | このアラートは、データを ID サービスに取り込む際にエラーが発生した場合にトリガーします。 |

{style=&quot;table-layout:auto&quot;}

## リアルタイム顧客プロファイル

次のアラートルールは、 [リアルタイム顧客プロファイル](../../profile/home.md):

| I/O イベントの購読 | アラートルール | 説明 |
| --- | --- | --- |
| プロファイル取り込み情報 | プロファイルフロー実行開始 | このアラートは、プロファイルフローの実行がデータの処理を開始したときにトリガーします。 |
| プロファイル取り込み情報 | プロファイルフロー実行成功 | このアラートは、データがデータレイクからプロファイルに正常に読み込まれた場合にトリガーします。 |
| プロファイル取り込みの遅延、エラーおよびエラー | プロファイルフロー実行遅延 | このアラートは、データレイクからプロファイルにデータを読み込む際にトリガーし、処理に 150 分以上かかります。 |
| プロファイル取り込みの遅延、エラーおよびエラー | プロファイルフロー実行エラー | このアラートは、データをプロファイルに取り込む際にエラーが発生した場合にトリガーします。 |

{style=&quot;table-layout:auto&quot;}

## セグメント化

次のアラートルールは、 [セグメント化サービス](../../segmentation/home.md):

| I/O イベントの購読 | アラートルール | 説明 |
| --- | --- | --- |
| セグメント評価ジョブ情報 | セグメントジョブ開始 | このアラートは、セグメント評価ジョブがデータの処理を開始したときにトリガーします。 |
| セグメント評価ジョブ情報 | セグメントジョブ成功 | このアラートは、セグメント評価ジョブが正常に完了した場合にトリガーします。 |
| セグメント評価ジョブの遅延、エラーおよびエラー | セグメントジョブの遅延 | このアラートトリガーは、セグメント評価ジョブの完了に 150 分以上かかった場合に発生します。 |
| セグメント評価ジョブの遅延、エラーおよびエラー | セグメントジョブエラー | このアラートは、セグメント評価ジョブでエラーが発生した場合にトリガーします。 |
| セグメント評価ジョブの遅延、エラーおよびエラー | セグメント定義が無効 | このアラートは、内部エラーが原因でセグメント定義が無効になった場合にトリガーします。 これは、トリガーエンジニアリングチームが問題を調査するためのAdobeルームを自動的に作成します。 このアラートは、情報を提供する目的でのみ作成され、ユーザーからのアクションは必要ありません。 |

{style=&quot;table-layout:auto&quot;}

## 宛先

次のアラートルールは、 [宛先](../../destinations/home.md):

| I/O イベントの購読 | アラートルール | 説明 |
| --- | --- | --- |
| 宛先フロー実行情報 | 宛先フロー実行開始 | このアラートは、宛先フローの実行がセグメントのアクティブ化を開始したときにトリガーします。 |
| 宛先フロー実行情報 | 宛先フロー実行成功 | このアラートは、宛先に対してセグメントが正常にアクティブ化された場合にトリガーします。 |
| 宛先フロー実行の遅延、失敗、エラー | 宛先フロー実行遅延 | このアラートは、宛先フローの実行がセグメントのアクティブ化に 150 分以上かかった場合にトリガーします。 |
| 宛先フロー実行の遅延、失敗、エラー | 宛先フロー実行の失敗 | このアラートは、宛先へのセグメントのアクティブ化中にエラーが発生した場合にトリガーします。 |

{style=&quot;table-layout:auto&quot;}

<!-- (Definitions to be added once available)
| Segment Job Delay | This alert triggers when a segment job takes longer than 150 minutes to complete. | N/A | 30 seconds | 3 hours |
| No Ingestion Activity in Past 24 Hours | This alert triggers when no new data has been ingested in the last 24-hour period. | N/A | 1 day | 1 day |
| Ingestion Error Rate Exceeded | This alert triggers when the error rate for data ingestion exceeds the allotted threshold. | 20% | 30 seconds | 30 seconds |
| Entitlement Threshold Exceeded | This alert triggers when the number of created profiles exceeds 80% of your organization's entitlement. | 30 seconds | N/A |
| SFTP source has not ingested data | This alert triggers when an [SFTP source](../../sources/connectors/cloud-storage/sftp.md) has not ingested any data within a certain time period. | 1 day | 1 day |
| Feed Message | This alert when an identity sharing feed message has been sent to a user using [Segment Match](../../segmentation/ui/segment-match.md). | N/A | N/A |
| Feed Access Revoked | This alert triggers when another Platform user revokes access to an identity sharing feed using [Segment Match](../../segmentation/ui/segment-match.md). | N/A | N/A |
| Feed Modified | This alert triggers when an identity sharing feed is modified by a user using [Segment Match](../../segmentation/ui/segment-match.md). | N/A | N/A |
| Feed Shared | This alert triggers when a user shares a new feed in [Segment Match](../../segmentation/ui/segment-match.md). | N/A | N/A |
| Link Request | This alert triggers when a user requests to connect for partner sharing. | N/A | N/A |
| Link Action | This alert triggers when a user accepts a request to connect for partner sharing. | N/A | N/A |
-->
