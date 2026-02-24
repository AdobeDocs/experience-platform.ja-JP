---
title: 高度なデータ・ライフサイクル管理の概要
description: 高度なデータライフサイクル管理を使用すると、古くなったレコードや不正確なレコードを更新またはパージして、データのライフサイクルを管理できます。
exl-id: 104a2bb8-3242-4a20-b98d-ad6df8071a16
source-git-commit: fc71e61fd33fe216f8cd326b9df048958c07077a
workflow-type: tm+mt
source-wordcount: '691'
ht-degree: 32%

---

# Adobe Experience Platformの Advanced Data Lifecycle Management

Adobe Experience Platform では、カスタマーエクスペリエンスを調整するために、大規模で複雑なデータ操作を管理するための堅牢なツールのセットを提供しています。長い期間をかけてデータがシステムに取り込まれるにつれて、データが期待通りに使用され、間違ったデータを修正する必要がある場合は更新され、組織のポリシーで必要と判断された場合は削除されるように、データストアを管理することがますます重要になります。

これらのアクティビティは、[[!UICONTROL Data Lifecycle] UI ワークスペース ](#ui) または [Data Hygiene API](#api) を使用して実行できます。 データ・ライフサイクル・ジョブが実行されると、システムはプロセスの各ステップで透明性を更新します。 各ジョブタイプがシステム上でどのように表現されるかについて詳しくは、[タイムラインと透明性](#timelines-and-transparency)の節を参照してください。

>[!NOTE]
>
>Advanced Data Lifecycle Management では、[ データセット有効期限エンドポイント ](./api/dataset-expiration.md) を介したデータセット削除、および [workorder エンドポイント ](./api/workorder.md) を介したプライマリ ID を使用した ID 削除（行レベルデータ）をサポートしています。 また、Experience Platform UI を使用して [ データセットの有効期限 ](./ui/dataset-expiration.md) および [ レコードの削除 ](./ui/record-delete.md) を管理することもできます。 詳しくは、リンクされたドキュメントを参照してください。 なお、データライフサイクルはバッチ削除をサポートしていません。

## [!UICONTROL Data Lifecycle] UI ワークスペース {#ui}

Experience Platform UI の [!UICONTROL Data Lifecycle] ワークスペースを使用すると、データライフサイクルの設定とスケジュール設定ができ、レコードが期待どおりにメンテナンスされていることを確認するのに役立ちます。

UI でデータライフサイクルタスクを管理する手順について詳しくは、[ データライフサイクル UI ガイド ](./ui/overview.md) を参照してください。

## Data Hygiene API {#api}

[!UICONTROL Data Lifecycle] UI は、Data Hygiene API をベースに構築されており、そのエンドポイントは、データのライフサイクルアクティビティを自動化したい場合に、直接使用できます。 詳しくは、[Data Hygiene API ガイド](./api/overview.md)を参照してください。

## タイムラインと透明性 {#timelines-and-transparency}

[ レコードの削除 ](./ui/record-delete.md) リクエストとデータセット有効期限リクエストには、それぞれ独自の処理タイムラインがあり、それぞれのワークフローの主要なポイントで透明性を更新します。

>[!TIP]
>
>クォータ制限に対する現在の使用量を監視するには、[ クォータ参照ガイド ](./api/quota.md) を参照してください。\
>使用権限ルール、月別キャップ、SLA タイムライン、例外処理ポリシーについては、[ レコード削除（UI） ](./ui/record-delete.md#quotas) および [ 作業指示（API） ](./api/workorder.md#quotas) ドキュメントを参照してください。

[データセット有効期限切れリクエスト](./ui/dataset-expiration.md)が作成されると、次のプロセスが実行されます。

| 段階 | スケジュールされた有効期限後の経過時間 | 説明 |
| --- | --- | --- |
| リクエストが送信される | 0 時間 | データセットが指定の時間に有効期限切れになるように求めるリクエストをデータスチュワードまたはプライバシーアナリストが送信します。 リクエストは送信後、[!UICONTROL Data Lifecycle UI] に表示され、スケジュールされた有効期限まで保留状態のままになり、期限後にリクエストが実行されます。 |
| データセットがデータレイクからドロップされる | 1 時間 | UI の[データセットインベントリページ](../catalog/datasets/user-guide.md)からデータセットがドロップされます。データレイク内のデータはソフト削除されるだけで、プロセスの終わりまでそのまま残り、プロセスの終了後にハード削除されます。 |
| データセットがプロファイルサービスからドロップされます | 3 時間 | これ以降、バッチおよびストリーミングのセグメント化、プレビューまたは見積もり、書き出しおよびエンティティアクセスを含む操作は、このデータセットからデータを読み取らなくなります。 プロファイルサービス内のデータはソフト削除されるだけで、プロセスの終了までそのまま残り、プロセスの終了後にハード削除されます。 |
| プロファイル数とオーディエンスの更新 | 48 時間 | 影響を受けるすべてのプロファイルが更新されると、関連するすべての[オーディエンス](../segmentation/home.md)が更新されて、新しいサイズが反映されます。削除したデータセットとセグメント化している属性に応じて、削除が原因で各オーディエンスのサイズが増減する場合があります。 この時点で、結果として生じるプロファイル数全体の変更が [ ダッシュボードウィジェット ](../dashboards/guides/profiles.md#profile-count-trend) やその他のレポートに反映されます。 |
| ジャーニーと宛先の更新 | 50 時間 | 関連するセグメントの変更に従って、[ジャーニー](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/about-journeys/journey.html?lang=ja)、[キャンペーン](https://experienceleague.adobe.com/docs/journey-optimizer/using/campaigns/get-started-with-campaigns.html?lang=ja)および[宛先](../destinations/home.md)が更新されます。 |
| ハード削除が完了する | 15 日 | データセットに関連するすべてのデータが、データレイクとプロファイルサービスからハード削除されます。 データセットが削除された [ データライフサイクルジョブのステータス ](./ui/browse.md#view-details) が、これを反映するように更新されます。 |

{style="table-layout:auto"}

## 次の手順

このドキュメントでは、Experience Platformのデータライフサイクル機能の概要を説明しました。 UI でのデータハイジーンリクエストの実行を開始するには、[UI ガイド](./ui/overview.md)を参照してください。データライフサイクルジョブをプログラムで作成する方法については、[Data Hygiene API ガイド ](./api/overview.md) を参照してください。
