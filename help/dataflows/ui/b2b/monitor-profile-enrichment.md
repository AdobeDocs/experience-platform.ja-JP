---
description: 以下を使用： [!UICONTROL プロファイルエンリッチメント] ダッシュボードを使用して、プロファイルエンリッチメントジョブが正常に実行および完了したかどうかを把握し、エンリッチメントの効果を測定するための基本指標を表示できます。
solution: Experience Platform
title: プロファイルエンリッチメントジョブの監視
type: Tutorial
source-git-commit: f3389ef2c2bd9ff52ecde2a4f5fd55e5b86783fc
workflow-type: tm+mt
source-wordcount: '466'
ht-degree: 1%

---

# UI でのプロファイルエンリッチメントジョブの監視

以下を使用： [!UICONTROL プロファイルエンリッチメント] ダッシュボードを使用して、プロファイルエンリッチメントジョブが正常に実行および完了したかどうかを把握し、エンリッチメントの効果を測定するための基本指標を表示できます。

内 [Platform UI](https://platform.adobe.com)を選択します。 **[!UICONTROL 監視]** 左側のナビゲーションから [!UICONTROL 監視] ダッシュボード。 表示セレクターで、 **B2B フロー** 特有のダッシュボード要素を表示するには [Real-Time CDP B2B](/help/rtcdp/b2b-overview.md).  この [!UICONTROL 監視] ダッシュボードには、最新の成功した実行の基本指標と、日別のジョブステータス（過去 90 日間）が含まれます。 この [!UICONTROL 関連するアカウント] ダッシュボードには、 [関連アカウント](/help/rtcdp/b2b-ai-ml-services/related-accounts.md) プロファイルエンリッチメント。

![Experience PlatformUI のプロファイルエンリッチメントジョブ監視画面にアクセスする方法を視覚的に示します。](/help/dataflows/assets/ui/b2b/monitoring-profile-enrichment-jobs.png)

のデータ **[!UICONTROL 指標]** カードには、関連アカウントジョブの最新の正常な実行の基本指標が含まれます。

関連するアカウントのプロファイルエンリッチメントジョブでは、次の指標を使用できます。

| 指標 | 説明 |
---------|----------|
| **[!UICONTROL 合計アカウントプロファイル数]** | 組織がアクセスできるアカウントプロファイルの合計数を示します。 |
| **[!UICONTROL アカウントグループ]** | 関連アカウントの機械学習ジョブによってクラスター化されたアカウントグループの数を示します。 |
| **[!UICONTROL 単一アカウントグループ]** | 他のアカウントと一緒にグループ化されていないアカウントの数を示します。 |
| **[!UICONTROL 最大のグループサイズ]** | 関連する最大のアカウントグループのサイズを示します。 グループサイズの上限は 30 です。 |
| **[!UICONTROL 中央値グループサイズ]** | 組織内の関連するアカウントグループの中央値サイズを示します。 |
| **[!UICONTROL 前回の成功した実行]** | 最後に成功した関連アカウントジョブの実行日時を示します。 |
| **[!UICONTROL ステータス]** | 関連アカウントジョブのステータス（成功、失敗または処理）を示します。 |
| **[!UICONTROL メッセージ]** | 特定のジョブ実行のエラーまたは警告メッセージを示します。 |

## UI コントロール {#ui-controls}

この節では、監視インターフェイスの様々なユーザーインターフェイス (UI) オプションについて説明します。このオプションを使用すると、ページに表示する指標をフィルタリングできます。

矢印アイコン (![矢印アイコン](/help/dataflows/assets/ui/monitor-destinations/chevron-up.png)) をクリックして、画面上部のカードを展開または解除します。このカードには、プロファイルエンリッチメントジョブに関する一目でわかる情報が表示されます。

![矢印アイコン UI コントロールを表示する画面記録。](/help/dataflows/assets/ui/b2b/use-arrow-control.gif)

以下を使用： **[!UICONTROL 指標とグラフ]** を切り替えて、最新の指標を表示する表示を閉じます。

![指標とグラフの切り替えを表示する画面の記録。](/help/dataflows/assets/ui/b2b/metrics-and-graphs-toggle.gif)

以下を使用： **[!UICONTROL 失敗のみを表示]** 切り替えて、失敗したプロファイルエンリッチメントジョブの表示のみに切り替えます。

![「エラーのみを表示」切り替えを表示する画面の記録。](/help/dataflows/assets/ui/b2b/show-failures-only.gif)

## 次の手順 {#next-steps}

このチュートリアルに従うことで、関連するアカウントのプロファイルエンリッチメントジョブの指標を正しく監視および理解できます。 詳しくは、次のドキュメントを参照してください。

* [リアルタイム CDP B2B の関連アカウント](/help/rtcdp/b2b-ai-ml-services/related-accounts.md)
* [アカウントプロファイル UI ガイドの「関連アカウント」タブ](/help/rtcdp/accounts/account-profile-ui-guide.md)