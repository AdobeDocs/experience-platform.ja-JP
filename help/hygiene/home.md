---
title: 高度なデータ・ライフサイクル管理の概要
description: 高度なデータライフサイクル管理を使用すると、古くなったレコードや不正確なレコードを更新またはパージして、データのライフサイクルを管理できます。
exl-id: 104a2bb8-3242-4a20-b98d-ad6df8071a16
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '832'
ht-degree: 34%

---

# Adobe Experience Platformの Advanced Data Lifecycle Management

Adobe Experience Platform では、カスタマーエクスペリエンスを調整するために、大規模で複雑なデータ操作を管理するための堅牢なツールのセットを提供しています。長い期間をかけてデータがシステムに取り込まれるにつれて、データが期待通りに使用され、間違ったデータを修正する必要がある場合は更新され、組織のポリシーで必要と判断された場合は削除されるように、データストアを管理することがますます重要になります。

<!-- Experience Platform's data lifecycle capabilities allow you to manage your stored data through the following:

* Scheduling automated dataset expirations
* Deleting individual records from one or all datasets

>[!IMPORTANT]
>
>Record deletes are meant to be used for data cleansing, removing anonymous data, or data minimization. They are **not** to be used for data subject rights requests (compliance) as pertaining to privacy regulations like the General Data Protection Regulation (GDPR). For all compliance use cases, use [Adobe Experience Platform Privacy Service](../privacy-service/home.md) instead. -->

これらのアクティビティは、[[!UICONTROL  データライフサイクル ] UI ワークスペース ](#ui) または [Data Hygiene API](#api) を使用して実行できます。 データ・ライフサイクル・ジョブが実行されると、システムはプロセスの各ステップで透明性を更新します。 各ジョブタイプがシステム上でどのように表現されるかについて詳しくは、[タイムラインと透明性](#timelines-and-transparency)の節を参照してください。

>[!NOTE]
>
>Advanced Data Lifecycle Management では、[ データセット有効期限エンドポイント ](./api/dataset-expiration.md) を介したデータセット削除、および [workorder エンドポイント ](./api/workorder.md) を介したプライマリ ID を使用した ID 削除（行レベルデータ）をサポートしています。 また、Experience Platform UI を使用して [ データセットの有効期限 ](./ui/dataset-expiration.md) および [ レコードの削除 ](./ui/record-delete.md) を管理することもできます。 詳しくは、リンクされたドキュメントを参照してください。 なお、データライフサイクルはバッチ削除をサポートしていません。

## [!UICONTROL  データライフサイクル ] UI ワークスペース {#ui}

Experience Platform UI の [!UICONTROL  データライフサイクル ] ワークスペースを使用すると、データライフサイクルの設定とスケジュール設定ができ、レコードが期待どおりにメンテナンスされていることを確認するのに役立ちます。

UI でデータライフサイクルタスクを管理する手順について詳しくは、[ データライフサイクル UI ガイド ](./ui/overview.md) を参照してください。

## Data Hygiene API {#api}

[!UICONTROL  データライフサイクル ] UI は、Data Hygiene API をベースに構築されており、そのエンドポイントは、データライフサイクルアクティビティを自動化したい場合に、直接使用できます。 詳しくは、[Data Hygiene API ガイド](./api/overview.md)を参照してください。

## タイムラインと透明性 {#timelines-and-transparency}

[ レコードの削除 ](./ui/record-delete.md) リクエストとデータセット有効期限リクエストには、それぞれ独自の処理タイムラインがあり、それぞれのワークフローの主要なポイントで透明性を更新します。

[データセット有効期限切れリクエスト](./ui/dataset-expiration.md)が作成されると、次のプロセスが実行されます。

| 段階 | スケジュールされた有効期限後の経過時間 | 説明 |
| --- | --- | --- |
| リクエストが送信される | 0 時間 | データセットが指定の時間に有効期限切れになるように求めるリクエストをデータスチュワードまたはプライバシーアナリストが送信します。 リクエストは送信後、[!UICONTROL  データライフサイクル UI] に表示され、スケジュールされた有効期限まで保留状態のままになり、期限後にリクエストが実行されます。 |
| データセットに削除フラグが設定されています | 0 ～ 2 時間 | リクエストが実行されると、データセットに削除のフラグが付けられます。 Amazon Web Services（AWS）のデータストレージを使用する場合、このプロセスには最大 2 時間かかります。 この間、バッチやストリーミングのセグメント化、プレビューや見積もり、書き出し、アクセスなどの操作では、このデータセットが無視されます。 |
| データセットがドロップされる | 3 時間 | **データセットに削除のフラグが付けられてから 1 時間後**、データセットはシステムから完全に削除されます。 この時点で、UI の [ データセットインベントリページ ](../catalog/datasets/user-guide.md) からデータセットがドロップされます。 ただし、データレイク内のデータは、この段階でソフト削除されるだけで、ハード削除プロセスが完了するまで残ります。 |
| プロファイル数が更新される | 30 時間 | 削除するデータセットの内容に応じて、すべてのコンポーネント属性がそのデータセットに関連付けられている場合、一部のプロファイルがシステムから削除されることがあります。 データセットが削除されてから 30 時間が経過すると、結果として生じるプロファイル数全体の変更が、 [ダッシュボードウィジェット](../dashboards/guides/profiles.md#profile-count-trend)やその他のレポートに反映されます。 |
| オーディエンスが更新される | 48 時間 | 影響を受けるすべてのプロファイルが更新されると、関連するすべての[オーディエンス](../segmentation/home.md)が更新されて、新しいサイズが反映されます。削除したデータセットとセグメント化しようとしている属性に応じて、削除の結果、各オーディエンスのサイズが増減する場合があります。 |
| ジャーニーと宛先の更新 | 50 時間 | 関連するセグメントの変更に従って、[ジャーニー](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/about-journeys/journey.html?lang=ja)、[キャンペーン](https://experienceleague.adobe.com/docs/journey-optimizer/using/campaigns/get-started-with-campaigns.html?lang=ja)および[宛先](../destinations/home.md)が更新されます。 |
| ハード削除が完了する | 15 日 | データセットに関連するすべてのデータが、データレイクからハード削除されます。 データセットが削除された [ データライフサイクルジョブのステータス ](./ui/browse.md#view-details) が、これを反映するように更新されます。 |

{style="table-layout:auto"}

>[!IMPORTANT]
>
>Amazon Web Services（AWS）のデータセット削除は、変更が完全に適用されるまで、約 3 時間の遅延の影響を受けます。 これには、データセットに削除のフラグが立てられるまでの最大 2 時間と、その後システムから完全に削除されるまでの追加の 1 時間が含まれます。 これに対し、Azure Data Lake を使用するExperience Platform インスタンスの削除リクエストでは、ビジネス機能全体に即時の変化が生じます。
>
>AWS ユーザーの場合、この遅延は、バッチセグメント化、ストリーミングセグメント化、プレビュー、予測、書き出し、データアクセスに影響を与える可能性があります。 Azure Data Lake ユーザーは即時に更新を行うので、この待ち時間はAWSを使用しているお客様にのみ影響します。 AWS ユーザーの場合、影響を受けたすべてのシステムに削除リクエストが完全に反映されるまで、最大 3 時間かかる場合があります。 それに応じて期待値を調整します。


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

このドキュメントでは、Experience Platformのデータライフサイクル機能の概要を説明しました。 UI でのデータハイジーンリクエストの実行を開始するには、[UI ガイド](./ui/overview.md)を参照してください。データライフサイクルジョブをプログラムで作成する方法については、[Data Hygiene API ガイド ](./api/overview.md) を参照してください。
