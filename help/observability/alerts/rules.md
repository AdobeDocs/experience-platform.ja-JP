---
keywords: Experience Platform;ホーム;人気のトピック;日付範囲
title: 標準のアラートルール
description: このドキュメントでは、Experience Platformが提供する事前定義済みのアラート・ルールについて説明します。
feature: Alerts
exl-id: b4af1c15-b1bc-4e4b-a447-09cc17a63988
source-git-commit: d82487f34c0879ed27ac55e42d70346f45806131
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 17%

---

# 標準のアラートルール

Adobe Experience Platformには、組織に対して有効にできる定義済みのアラートルールがいくつか用意されています。 次の表に、これらのAdobe提供のアラートルールの詳細を示します。 Experience Platformのアラートに関する一般的な情報については、[アラートの概要](./overview.md)を参照してください。

| 規則 | 説明 | 評価頻度 | 繰り返しウィンドウ |
| --- | --- | --- | --- |
| ソースフロー実行の成功 | このアラートは、データがソース接続から正常に取り込まれたときにトリガーします。 | なし | なし |
| ソースフロー実行エラー | このアラートは、ソース接続からデータを取り込む際にエラーが発生した場合にトリガーします。 | なし | なし |
| セグメントジョブの遅延 | このアラートトリガーは、セグメントジョブの完了に150分以上かかった場合に発生します。 | 30 秒 | 3 時間 |
| セグメント定義が無効 | このアラートは、セグメント定義が無効な場合にトリガーします。 | なし | なし |

{style=&quot;table-layout:auto&quot;}

<!-- (Definitions to be added once available)
| Entitlement Threshold Exceeded | This alert triggers when the number of created profiles exceeds 80% of your organization's entitlement. | 30 seconds | N/A |
| No ingestion activity in past 24 hours | This alert triggers when no new data has been ingested in the last 24-hour period. | 1 day | 1 day |
| SFTP source has not ingested data | This alert triggers when an [SFTP source](../../sources/connectors/cloud-storage/sftp.md) has not ingested any data within a certain time period. | 1 day | 1 day |
| Ingestion error rate exceeded | This alert triggers when the error rate for data ingestion exceeds 20%. | 30 seconds | 30 seconds |
| Feed Message | This alert when an identity sharing feed message has been sent to a user using [Segment Match](../../segmentation/ui/segment-match.md). | N/A | N/A |
| Feed Access Revoked | This alert triggers when another Platform user revokes access to an identity sharing feed using [Segment Match](../../segmentation/ui/segment-match.md). | N/A | N/A |
| Feed Modified | This alert triggers when an identity sharing feed is modified by a user using [Segment Match](../../segmentation/ui/segment-match.md). | N/A | N/A |
| Feed Shared | This alert triggers when a user shares a new feed in [Segment Match](../../segmentation/ui/segment-match.md). | N/A | N/A |
| Link Request | This alert triggers when a user requests to connect for partner sharing. | N/A | N/A |
| Link Action | This alert triggers when a user accepts a request to connect for partner sharing. | N/A | N/A |
-->
