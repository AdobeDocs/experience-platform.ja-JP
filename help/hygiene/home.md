---
title: 高度なデータ・ライフサイクル管理の概要
description: 高度なデータライフサイクル管理を使用すると、古くなったレコードや不正確なレコードを更新またはパージして、データのライフサイクルを管理できます。
exl-id: 104a2bb8-3242-4a20-b98d-ad6df8071a16
source-git-commit: a1502e8f1515ff73840b2926f5be355032dd4bab
workflow-type: tm+mt
source-wordcount: '815'
ht-degree: 27%

---

# Adobe Experience Platformの Advanced Data Lifecycle Management

Adobe Experience Platform では、カスタマーエクスペリエンスを調整するために、大規模で複雑なデータ操作を管理するための堅牢なツールのセットを提供しています。長い期間をかけてデータがシステムに取り込まれるにつれて、データが期待通りに使用され、間違ったデータを修正する必要がある場合は更新され、組織のポリシーで必要と判断された場合は削除されるように、データストアを管理することがますます重要になります。

<!-- Experience Platform's data lifecycle capabilities allow you to manage your stored data through the following:

* Scheduling automated dataset expirations
* Deleting individual records from one or all datasets

>[!IMPORTANT]
>
>Record deletes are meant to be used for data cleansing, removing anonymous data, or data minimization. They are **not** to be used for data subject rights requests (compliance) as pertaining to privacy regulations like the General Data Protection Regulation (GDPR). For all compliance use cases, use [Adobe Experience Platform Privacy Service](../privacy-service/home.md) instead. -->

これらのアクティビティは、[[!UICONTROL Data Lifecycle] UI ワークスペース &#x200B;](#ui) または [Data Hygiene API](#api) を使用して実行できます。 データ・ライフサイクル・ジョブが実行されると、システムはプロセスの各ステップで透明性を更新します。 各ジョブタイプがシステム上でどのように表現されるかについて詳しくは、[タイムラインと透明性](#timelines-and-transparency)の節を参照してください。

>[!NOTE]
>
>Advanced Data Lifecycle Management では、[&#x200B; データセット有効期限エンドポイント &#x200B;](./api/dataset-expiration.md) を介したデータセット削除、および [workorder エンドポイント &#x200B;](./api/workorder.md) を介したプライマリ ID を使用した ID 削除（行レベルデータ）をサポートしています。 また、Experience Platform UI を使用して [&#x200B; データセットの有効期限 &#x200B;](./ui/dataset-expiration.md) および [&#x200B; レコードの削除 &#x200B;](./ui/record-delete.md) を管理することもできます。 詳しくは、リンクされたドキュメントを参照してください。 なお、データライフサイクルはバッチ削除をサポートしていません。

## [!UICONTROL Data Lifecycle] UI ワークスペース {#ui}

Experience Platform UI の [!UICONTROL Data Lifecycle] ワークスペースを使用すると、データライフサイクルの設定とスケジュール設定ができ、レコードが期待どおりにメンテナンスされていることを確認するのに役立ちます。

UI でデータライフサイクルタスクを管理する手順について詳しくは、[&#x200B; データライフサイクル UI ガイド &#x200B;](./ui/overview.md) を参照してください。

## Data Hygiene API {#api}

[!UICONTROL Data Lifecycle] UI は、Data Hygiene API をベースに構築されており、そのエンドポイントは、データのライフサイクルアクティビティを自動化したい場合に、直接使用できます。 詳しくは、[Data Hygiene API ガイド](./api/overview.md)を参照してください。

## タイムラインと透明性 {#timelines-and-transparency}

[&#x200B; レコードの削除 &#x200B;](./ui/record-delete.md) リクエストとデータセット有効期限リクエストには、それぞれ独自の処理タイムラインがあり、それぞれのワークフローの主要なポイントで透明性を更新します。

>[!TIP]
>
>クォータ制限に対する現在の使用量を監視するには、[&#x200B; クォータ参照ガイド &#x200B;](./api/quota.md) を参照してください。\
>使用権限ルール、月別キャップ、SLA タイムライン、例外処理ポリシーについては、[&#x200B; レコード削除（UI） &#x200B;](./ui/record-delete.md#quotas) および [&#x200B; 作業指示（API） &#x200B;](./api/workorder.md#quotas) ドキュメントを参照してください。

[データセット有効期限切れリクエスト](./ui/dataset-expiration.md)が作成されると、次のプロセスが実行されます。

| 段階 | スケジュールされた有効期限後の経過時間 | 説明 |
| --- | --- | --- |
| リクエストが送信される | 0 時間 | データセットが指定の時間に有効期限切れになるように求めるリクエストをデータスチュワードまたはプライバシーアナリストが送信します。 リクエストは送信後、[!UICONTROL Data Lifecycle UI] に表示され、スケジュールされた有効期限まで保留状態のままになり、期限後にリクエストが実行されます。 |
| データセットがデータレイクからドロップされる | 1 時間 | UI の[データセットインベントリページ](../catalog/datasets/user-guide.md)からデータセットがドロップされます。データレイク内のデータはソフト削除されるだけで、プロセスの終わりまでそのまま残り、プロセスの終了後にハード削除されます。 |
| データセットがプロファイルサービスからドロップされます | 3 時間 | これ以降、バッチおよびストリーミングのセグメント化、プレビューまたは見積もり、書き出しおよびエンティティアクセスを含む操作は、このデータセットからデータを読み取らなくなります。 プロファイルサービス内のデータはソフト削除されるだけで、プロセスの終了までそのまま残り、プロセスの終了後にハード削除されます。 |
| プロファイル数とオーディエンスの更新 | 48 時間 | 影響を受けるすべてのプロファイルが更新されると、関連するすべての[オーディエンス](../segmentation/home.md)が更新されて、新しいサイズが反映されます。削除したデータセットとセグメント化している属性に応じて、削除が原因で各オーディエンスのサイズが増減する場合があります。 この時点で、結果として生じるプロファイル数全体の変更が [&#x200B; ダッシュボードウィジェット &#x200B;](../dashboards/guides/profiles.md#profile-count-trend) やその他のレポートに反映されます。 |
| ジャーニーと宛先の更新 | 50 時間 | 関連するセグメントの変更に従って、[ジャーニー](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/about-journeys/journey.html?lang=ja)、[キャンペーン](https://experienceleague.adobe.com/docs/journey-optimizer/using/campaigns/get-started-with-campaigns.html?lang=ja)および[宛先](../destinations/home.md)が更新されます。 |
| ハード削除が完了する | 15 日 | データセットに関連するすべてのデータが、データレイクとプロファイルサービスからハード削除されます。 データセットが削除された [&#x200B; データライフサイクルジョブのステータス &#x200B;](./ui/browse.md#view-details) が、これを反映するように更新されます。 |

{style="table-layout:auto"}

>[!IMPORTANT]
>
>Amazon Web Services（AWS）のデータセット削除は、変更が完全に適用されるまで、約 3 時間の遅延の影響を受けます。 これには、データセットに削除のフラグが立てられるまでの最大 2 時間と、その後システムから完全に削除されるまでの追加の 1 時間が含まれます。 これに対し、Azure Data Lake を使用するExperience Platform インスタンスに対する削除リクエストでは、ビジネス機能全体に即時の変化が生じます。
>
>AWS ユーザーの場合、この遅延は、バッチセグメント化、ストリーミングセグメント化、プレビュー、予測、書き出し、データアクセスに影響を与える可能性があります。 Azure Data Lake のユーザーは即時に更新を行うので、この待ち時間は、AWSを使用しているお客様にのみ影響します。 AWS ユーザーの場合、影響を受けたすべてのシステムに削除リクエストが完全に反映されるまで、最大 3 時間かかる場合があります。 それに応じて期待値を調整します。


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

このドキュメントでは、Experience Platformのデータライフサイクル機能の概要を説明しました。 UI でのデータハイジーンリクエストの実行を開始するには、[UI ガイド](./ui/overview.md)を参照してください。データライフサイクルジョブをプログラムで作成する方法については、[Data Hygiene API ガイド &#x200B;](./api/overview.md) を参照してください。
