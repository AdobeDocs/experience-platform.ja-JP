---
title: データの衛生状態の概要
description: Adobe Experience Platform のデータハイジーンを使用すると、古くなったレコードや不正確なレコードを更新またはパージして、データのライフサイクルを管理できます。
exl-id: 104a2bb8-3242-4a20-b98d-ad6df8071a16
source-git-commit: 51181dccbd37df60e438f34090ebaeb9e327c4ce
workflow-type: tm+mt
source-wordcount: '756'
ht-degree: 26%

---

# Adobe Experience Platformでのデータの衛生状態

Adobe Experience Platform では、カスタマーエクスペリエンスを調整するために、大規模で複雑なデータ操作を管理するための堅牢なツールのセットを提供しています。長い期間をかけてデータがシステムに取り込まれるにつれて、データが期待通りに使用され、間違ったデータを修正する必要がある場合は更新され、組織のポリシーで必要と判断された場合は削除されるように、データストアを管理することがますます重要になります。

プラットフォームのデータ衛生機能を使用すると、保存した消費者データを次の方法で管理できます。

* 自動データセット有効期限のスケジュール設定
* 取り込んだ ID に基づく消費者データの削除

>[!NOTE]
>
>消費者の削除要求は、Adobeのヘルスケアシールドまたはプライバシーシールドを購入した組織でのみ使用できます。

これらのアクティビティは、 [[!UICONTROL データの衛生状態] UI ワークスペース](#ui) または [データ衛生 API](#api). データの衛生ジョブが実行されると、プロセスの各ステップで透明度の更新が行われます。 詳しくは、 [タイムラインと透明性](#timelines-and-transparency) を参照してください。

## [!UICONTROL データハイジーン] UI ワークスペース {#ui}

Platform UI の[!UICONTROL データハイジーン]ワークスペースを使用すると、データハイジーン操作の設定とスケジュール設定ができ、レコードが期待どおりに維持されていることを確認するのに便利です。

UI でのデータハイジーンタスクの管理手順については、[データハイジーン UI ガイド](./ui/overview.md)を参照してください。

## Data Hygiene API {#api}

[!UICONTROL データハイジーン] UI は、Data Hygiene API をベースに構築されており、そのエンドポイントは、データハイジーン活動を自動化したい場合に、直接使用できます。詳しくは、[Data Hygiene API ガイド](./api/overview.md)を参照してください。

## タイムラインと透明度

消費者の削除リクエストとデータセットの有効期限リクエストにはそれぞれ独自の処理タイムラインがあり、それぞれのワークフローの主要なポイントで透明性の更新を提供します。 各ジョブタイプの詳細については、以下の節を参照してください。

### データセット有効期限 {#dataset-expiration-transparency}

次の処理は、 [データセット有効期限リクエスト](./ui/dataset-expiration.md) が作成されました。

| ステージ | スケジュールされた有効期限後の時間 | 説明 |
| --- | --- | --- |
| リクエストが送信されました | 0 時間 | データ管理者またはプライバシー分析者は、データセットの有効期限が所定の時間に切れるようにリクエストを送信します。 リクエストは、 [!UICONTROL データ衛生 UI] 送信後は、スケジュールされた有効期限まで保留状態のままになり、その後、リクエストが実行されます。 |
| データセットが削除されました | 1 時間 | データセットが [データセットインベントリページ](../catalog/datasets/user-guide.md) を使用します。 データレイク内のデータはソフト削除のみで、プロセスの終わりまでそのままで、その後はハード削除されます。 |
| 更新されたプロファイル数 | 30 時間 | データセットの有効期限によるプロファイル数の変更は、 [ダッシュボードウィジェット](../dashboards/guides/profiles.md#profile-count-trend) その他のレポート |
| 更新されたセグメント | 48 時間 | プロファイルを削除すると、 [セグメント](../segmentation/home.md) が更新され、新しいサイズが反映されます。 |
| ジャーニーと宛先の更新 | 50 時間 | [ジャーニー](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/about-journeys/journey.html), [campaigns](https://experienceleague.adobe.com/docs/journey-optimizer/using/campaigns/get-started-with-campaigns.html)、および [宛先](../destinations/home.md) 関連するセグメントの変更に従ってが更新されます。 |
| ハード削除が完了しました | 14 日 | データセットに関連するすべてのデータは、データレイクからハード削除されます。 この [衛生業務の状況](./ui/browse.md#view-details) 削除されたデータセットは、これを反映するように更新されます。 |

{style=&quot;table-layout:auto&quot;}

### 消費者の削除 {#consumer-delete-transparency}

次の処理は、 [消費者削除リクエスト](./ui/delete-consumer.md) が作成されました。

| ステージ | リクエスト送信後の時間 | 説明 |
| --- | --- | --- |
| リクエストが送信されました | 0 時間 | データ管理者またはプライバシー分析者が、消費者の削除リクエストを送信します。 リクエストは、 [!UICONTROL データ衛生 UI] 送信後 |
| 更新されたプロファイル参照 | 3 時間 | 削除された ID によるプロファイル数の変更は、 [ダッシュボードウィジェット](../dashboards/guides/profiles.md#profile-count-trend) その他のレポート |
| 更新されたセグメント | 24 時間 | プロファイルを削除すると、 [セグメント](../segmentation/home.md) が更新され、新しいサイズが反映されます。 |
| ジャーニーと宛先の更新 | 26 時間 | [ジャーニー](https://experienceleague.adobe.com/docs/journey-optimizer/using/orchestrate-journeys/about-journeys/journey.html), [campaigns](https://experienceleague.adobe.com/docs/journey-optimizer/using/campaigns/get-started-with-campaigns.html)、および [宛先](../destinations/home.md) 関連するセグメントの変更に従ってが更新されます。 |
| データレイクでソフト削除されたレコード | 7 日 | データはデータレイクからソフト削除されます。 |
| データのバキュームが完了しました | 14 日 | この [衛生業務の状況](./ui/browse.md#view-details) ジョブが完了したこと、つまり、データレイクでデータのバキュームが完了し、関連するレコードがハード削除されたことを示す更新です。 |

{style=&quot;table-layout:auto&quot;}

## 次の手順

このドキュメントでは、Platform のデータ衛生機能の概要を説明しました。 UI でのデータ衛生リクエストの実行を開始するには、 [UI ガイド](./ui/overview.md). データ衛生ジョブをプログラムで作成する方法については、 [データ衛生 API ガイド](./api/overview.md)
