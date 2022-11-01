---
title: データハイジーンの概要
description: Adobe Experience Platform のデータハイジーンを使用すると、古くなったレコードや不正確なレコードを更新またはパージして、データのライフサイクルを管理できます。
exl-id: 104a2bb8-3242-4a20-b98d-ad6df8071a16
source-git-commit: 6453ec6c98d90566449edaa0804ada260ae12bf6
workflow-type: tm+mt
source-wordcount: '758'
ht-degree: 97%

---

# Adobe Experience Platform のデータハイジーン

>[!IMPORTANT]
>
>データの衛生状態は、現在、を購入した組織でのみ利用できます **Adobeヘルスケアシールド** または **Adobeプライバシーとセキュリティシールド**.

Adobe Experience Platform では、カスタマーエクスペリエンスを調整するために、大規模で複雑なデータ操作を管理するための堅牢なツールのセットを提供しています。長い期間をかけてデータがシステムに取り込まれるにつれて、データが期待通りに使用され、間違ったデータを修正する必要がある場合は更新され、組織のポリシーで必要と判断された場合は削除されるように、データストアを管理することがますます重要になります。

Platform のデータハイジーン機能を使用すると、以下を通じて保存済みの消費者データを管理できます。

* 自動化されたデータセット有効期限切れのスケジュール設定
* 取り込んだ ID に基づく消費者データの削除

これらのアクティビティは、[[!UICONTROL データハイジーン] UI ワークスペース](#ui)または [Data Hygiene API](#api)を使用して実行できます。データハイジーンジョブが実行されると、システムはプロセスの各ステップで透明性を更新します。各ジョブタイプがシステム上でどのように表現されるかについて詳しくは、[タイムラインと透明性](#timelines-and-transparency)の節を参照してください。

## [!UICONTROL データハイジーン] UI ワークスペース {#ui}

Platform UI の[!UICONTROL データハイジーン]ワークスペースを使用すると、データハイジーン操作の設定とスケジュール設定ができ、レコードが期待どおりに維持されていることを確認するのに便利です。

UI でのデータハイジーンタスクの管理手順については、[データハイジーン UI ガイド](./ui/overview.md)を参照してください。

## Data Hygiene API {#api}

[!UICONTROL データハイジーン] UI は、Data Hygiene API をベースに構築されており、そのエンドポイントは、データハイジーン活動を自動化したい場合に、直接使用できます。詳しくは、[Data Hygiene API ガイド](./api/overview.md)を参照してください。

## タイムラインと透明性

消費者データ削除リクエストとデータセット有効期限切れリクエストには、それぞれ独自の処理タイムラインがあり、それぞれのワークフローの主要なポイントで透明性を更新します。各ジョブタイプの詳細については、以下の節を参照してください。

### データセット有効期限 {#dataset-expiration-transparency}

[データセット有効期限切れリクエスト](./ui/dataset-expiration.md)が作成されると、次のプロセスが実行されます。

| 段階 | スケジュールされた有効期限後の経過時間 | 説明 |
| --- | --- | --- |
| リクエストが送信される | 0 時間 | データセットが指定の時間に有効期限切れになるように求めるリクエストをデータスチュワードまたはプライバシーアナリストが送信します。リクエストは送信後、[!UICONTROL データハイジーン UI] に表示され、スケジュールされた有効期限まで保留状態のままになり、期限後にリクエストが実行されます。 |
| データセットがドロップされる | 1 時間 | UI の[データセットインベントリページ](../catalog/datasets/user-guide.md)からデータセットがドロップされます。データレイク内のデータはソフト削除されるだけで、プロセスの終わりまでそのまま残り、プロセスの終了後にハード削除されます。 |
| プロファイル数が更新される | 30 時間 | データセットの有効期限切れに起因するプロファイル数の変更が、[ダッシュボードウィジェット](../dashboards/guides/profiles.md#profile-count-trend)やその他のレポートに反映されます。 |
| セグメントが更新される | 48 時間 | プロファイルが削除されると、関連するすべての[セグメント](../segmentation/home.md)が更新され、新しいサイズが反映されます。 |
| ジャーニーと宛先の更新 | 50 時間 | 関連するセグメントの変更に従って、[ジャーニー](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/about-journeys/journey.html?lang=ja)、[キャンペーン](https://experienceleague.adobe.com/docs/journey-optimizer/using/campaigns/get-started-with-campaigns.html?lang=ja)および[宛先](../destinations/home.md)が更新されます。 |
| ハード削除が完了する | 14 日 | データセットに関連するすべてのデータが、データレイクからハード削除されます。 [ハイジーンジョブのステータス](./ui/browse.md#view-details)が、データセットの削除を反映するように更新されます。 |

{style=&quot;table-layout:auto&quot;}

### 消費者データ削除 {#consumer-delete-transparency}

[消費者データ削除リクエスト](./ui/delete-consumer.md)が作成されると、次のプロセスが実行されます。

| 段階 | リクエスト送信後の経過時間 | 説明 |
| --- | --- | --- |
| リクエストが送信される | 0 時間 | データスチュワードまたはプライバシーアナリストが、消費者データ削除リクエストを送信します。 このリクエストは、送信後に[!UICONTROL データハイジーン UI] に表示されます。 |
| プロファイル参照が更新される | 3 時間 | 削除された ID によるプロファイル数の変更は、[ダッシュボードウィジェット](../dashboards/guides/profiles.md#profile-count-trend)やその他のレポートに反映されます。 |
| セグメントが更新される | 24 時間 | プロファイルを削除すると、関連するすべての[セグメント](../segmentation/home.md)が更新され、新しいサイズが反映されます。 |
| ジャーニーと宛先の更新 | 26 時間 | [ジャーニー](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/about-journeys/journey.html)、[キャンペーン](https://experienceleague.adobe.com/docs/journey-optimizer/using/campaigns/get-started-with-campaigns.html)および[宛先](../destinations/home.md)は、関連するセグメントの変更に従ってが更新されます。 |
| データレイクでのレコードのソフト削除 | 7 日 | データは、データレイクからソフト削除されます。 |
| データのバキューム処理完了 | 14 日 | [ハイジーンのジョブのステータス](./ui/browse.md#view-details)が更新され、ジョブの完了が示されます。これは、データレイク上でデータのバキューム処理が完了し、関連レコードがハード削除されたことを意味します。 |

{style=&quot;table-layout:auto&quot;}

## 次の手順

このドキュメントでは、Platform データのハイジーン機能の概要を説明します。UI でのデータハイジーンリクエストの実行を開始するには、[UI ガイド](./ui/overview.md)を参照してください。データハイジーンジョブをプログラムで作成する方法については、[Data Hygiene API ガイド](./api/overview.md)を参照してください。
