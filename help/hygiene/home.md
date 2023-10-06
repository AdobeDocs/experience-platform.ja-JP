---
title: 高度なデータライフサイクル管理の概要
description: 高度なデータライフサイクル管理を使用すると、古いレコードや不正確なレコードを更新またはパージして、データのライフサイクルを管理できます。
exl-id: 104a2bb8-3242-4a20-b98d-ad6df8071a16
source-git-commit: 566f1b6478cd0de0691cfb2301d5b86fbbfece52
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 62%

---

# Adobe Experience Platformの高度なデータライフサイクル管理

Adobe Experience Platform では、カスタマーエクスペリエンスを調整するために、大規模で複雑なデータ操作を管理するための堅牢なツールのセットを提供しています。長い期間をかけてデータがシステムに取り込まれるにつれて、データが期待通りに使用され、間違ったデータを修正する必要がある場合は更新され、組織のポリシーで必要と判断された場合は削除されるように、データストアを管理することがますます重要になります。

<!-- Platform's data lifecycle capabilities allow you to manage your stored data through the following:

* Scheduling automated dataset expirations
* Deleting individual records from one or all datasets

>[!IMPORTANT]
>
>Record deletes are meant to be used for data cleansing, removing anonymous data, or data minimization. They are **not** to be used for data subject rights requests (compliance) as pertaining to privacy regulations like the General Data Protection Regulation (GDPR). For all compliance use cases, use [Adobe Experience Platform Privacy Service](../privacy-service/home.md) instead. -->

これらのアクティビティは、 [[!UICONTROL データのライフサイクル] UI ワークスペース](#ui) または [データ衛生 API](#api). データライフサイクルジョブを実行すると、プロセスの各ステップで透明度の更新が行われます。 各ジョブタイプがシステム上でどのように表現されるかについて詳しくは、[タイムラインと透明性](#timelines-and-transparency)の節を参照してください。

## [!UICONTROL データのライフサイクル] UI ワークスペース {#ui}

The [!UICONTROL データのライフサイクル] Platform UI のワークスペースを使用すると、データのライフサイクル操作を設定およびスケジュールでき、レコードが期待どおりに維持されるようになります。

UI でデータライフサイクルタスクを管理する手順について詳しくは、 [データライフサイクル UI ガイド](./ui/overview.md).

## Data Hygiene API {#api}

The [!UICONTROL データのライフサイクル] UI は、データライフサイクル API をベースに構築されています。データライフサイクルアクティビティを自動化する場合に、エンドポイントを直接使用できます。 詳しくは、[Data Hygiene API ガイド](./api/overview.md)を参照してください。

## タイムラインと透明性

[レコードの削除リクエストとデータセット有効期限切れリクエストには、それぞれ独自の処理タイムラインがあり、それぞれのワークフローの主要なポイントで透明性を更新します。](./ui/record-delete.md)

<!-- ### Dataset expirations {#dataset-expiration-transparency} -->

[データセット有効期限切れリクエスト](./ui/dataset-expiration.md)が作成されると、次のプロセスが実行されます。

| 段階 | スケジュールされた有効期限後の経過時間 | 説明 |
| --- | --- | --- |
| リクエストが送信される | 0 時間 | データセットが指定の時間に有効期限切れになるように求めるリクエストをデータスチュワードまたはプライバシーアナリストが送信します。リクエストは、 [!UICONTROL データライフサイクル UI] 送信後は、スケジュールされた有効期限まで保留状態のままになり、その後、リクエストが実行されます。 |
| データセットがドロップされる | 1 時間 | UI の[データセットインベントリページ](../catalog/datasets/user-guide.md)からデータセットがドロップされます。データレイク内のデータはソフト削除されるだけで、プロセスの終わりまでそのまま残り、プロセスの終了後にハード削除されます。 |
| プロファイル数が更新される | 30 時間 | 削除するデータセットの内容に応じて、すべてのコンポーネント属性がそのデータセットに関連付けられている場合、一部のプロファイルがシステムから削除されることがあります。 データセットが削除されてから 30 時間が経過すると、結果として生じるプロファイル数全体の変更が、 [ダッシュボードウィジェット](../dashboards/guides/profiles.md#profile-count-trend)やその他のレポートに反映されます。 |
| オーディエンスが更新される | 48 時間 | 影響を受けるすべてのプロファイルが更新されると、関連するすべての[オーディエンス](../segmentation/home.md)が更新されて、新しいサイズが反映されます。削除したデータセットとセグメント化しようとしている属性に応じて、削除の結果、各オーディエンスのサイズが増減する場合があります。 |
| ジャーニーと宛先の更新 | 50 時間 | 関連するセグメントの変更に従って、[ジャーニー](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/about-journeys/journey.html?lang=ja)、[キャンペーン](https://experienceleague.adobe.com/docs/journey-optimizer/using/campaigns/get-started-with-campaigns.html?lang=ja)および[宛先](../destinations/home.md)が更新されます。 |
| ハード削除が完了する | 14 日 | データセットに関連するすべてのデータが、データレイクからハード削除されます。 The [データライフサイクルジョブのステータス](./ui/browse.md#view-details) 削除されたデータセットは、これを反映するように更新されます。 |

{style="table-layout:auto"}

<!-- ### Record deletes {#record-delete-transparency}

The following takes place when a [record delete request](./ui/record-delete.md) is created:

| Stage | Time after request submission | Description |
| --- | --- | --- |
| Request is submitted | 0 hours | A data steward or privacy analyist submits a record delete request. The request is visible in the [!UICONTROL Data Lifecycle UI] after it has been submitted. |
| Profile lookups updated | 3 hours | The change in profile counts caused by the deleted identity are reflected in [dashboard widgets](../dashboards/guides/profiles.md#profile-count-trend) and other reports. |
| Segments updated | 24 hours | Once profiles are removed, all related [segments](../segmentation/home.md) are updated to reflect their new size. |
| Journeys and destinations updated | 26 hours | [Journeys](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/about-journeys/journey.html), [campaigns](https://experienceleague.adobe.com/docs/journey-optimizer/using/campaigns/get-started-with-campaigns.html), and [destinations](../destinations/home.md) are updated according to changes in related segments. |
| Records soft deleted in data lake | 7 days | The data is soft deleted from the data lake. |
| Data vacuuming completed | 14 days | The [status of the lifecycle job](./ui/browse.md#view-details) updates to indicate that the job has completed, meaning that data vacuuming has been completed on the data lake and the relevant records have been hard deleted. |

{style="table-layout:auto"} -->

## 次の手順

このドキュメントでは、Platform のデータライフサイクル機能の概要を説明しました。 UI でのデータハイジーンリクエストの実行を開始するには、[UI ガイド](./ui/overview.md)を参照してください。データライフサイクルジョブをプログラムで作成する方法については、 [データ衛生 API ガイド](./api/overview.md)
