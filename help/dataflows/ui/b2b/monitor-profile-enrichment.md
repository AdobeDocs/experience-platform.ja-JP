---
description: 以下を使用： [!UICONTROL プロファイルエンリッチメント] ダッシュボードを使用して、プロファイルエンリッチメントジョブが正常に実行および完了したかどうかを把握し、エンリッチメントの効果を測定するための基本指標を表示できます。
solution: Experience Platform
title: プロファイルエンリッチメントジョブの監視
type: Tutorial
exl-id: 096a2212-ed7f-4419-8ead-fa1ca01c2804
source-git-commit: 47a6cc9b77a0591d488d5ebc3929b465e1a6e6d2
workflow-type: tm+mt
source-wordcount: '634'
ht-degree: 4%

---

# UI でのプロファイルエンリッチメントジョブの監視 {#monitor-profile-enrichment}

以下を使用： [!UICONTROL プロファイルエンリッチメント] ダッシュボードを使用して、プロファイルエンリッチメントジョブが正常に実行および完了したかどうかを把握し、エンリッチメントの効果を測定するための基本指標を表示できます。

内 [Platform UI](https://platform.adobe.com)を選択します。 **[!UICONTROL 監視]** 左側のナビゲーションから [!UICONTROL 監視] ダッシュボード。 表示セレクターで、 **B2B フロー** 特有のダッシュボード要素を表示するには [Real-Time CDP B2B](/help/rtcdp/b2b-overview.md).  この [!UICONTROL 監視] ダッシュボードには、最新の成功した実行の基本指標と、日別のジョブステータス（過去 90 日間）が含まれます。

## 関連するアカウントプロファイルエンリッチメント (#related-accounts)

この [!UICONTROL 関連するアカウント] ダッシュボードには、基本指標と、 [関連アカウント](/help/rtcdp/b2b-ai-ml-services/related-accounts.md) プロファイルエンリッチメント。

![Experience PlatformUI のプロファイルエンリッチメントジョブ監視画面にアクセスする方法を視覚的に示します。](/help/dataflows/assets/ui/b2b/monitoring-profile-enrichment-jobs.png)

のデータ **[!UICONTROL 指標]** カードには、関連アカウントジョブの最新の正常な実行の基本指標が含まれます。

関連するアカウントのプロファイルエンリッチメントジョブでは、次の指標を使用できます。

| 指標 | 説明 |
| --------- | ---------- |
| **[!UICONTROL 合計アカウントプロファイル数]** | 組織がアクセスできるアカウントプロファイルの合計数を示します。 |
| **[!UICONTROL アカウントグループ]** | 関連するアカウントの機械学習ジョブによってクラスター化されたアカウントグループの数を示します。 |
| **[!UICONTROL 単一アカウントグループ]** | 他のアカウントと一緒にグループ化されていないアカウントの数を示します。 |
| **[!UICONTROL 最大のグループサイズ]** | 関連する最大のアカウントグループのサイズを示します。 グループサイズの上限は 30 です。 |
| **[!UICONTROL 中央値グループサイズ]** | 組織内の関連するアカウントグループの中央値サイズを示します。 |
| **[!UICONTROL 前回の成功した実行]** | 最後に成功した関連アカウントジョブが実行された日時を示します。 |
| **[!UICONTROL ステータス]** | 関連するアカウントジョブのステータス（成功、失敗または処理）を示します。 |
| **[!UICONTROL メッセージ]** | 特定のジョブ実行のエラーまたは警告メッセージを示します。 |

## プロファイルエンリッチメントと一致するリード — アカウント {#lead-to-account-matching}

この [!UICONTROL リード — アカウントマッチング] ダッシュボードには、 [リード — アカウントマッチング](/help/rtcdp/b2b-ai-ml-services/lead-to-account-matching.md) プロファイルエンリッチメント。

![プロファイルエンリッチメントと一致するリード — アカウント](/help/dataflows/assets/ui/b2b/mpc-lead-to-account-matching.png)

プロファイルエンリッチメントジョブに一致するリードとアカウントの間で、次の指標を使用できます。

| 指標 | 説明 |
| --------- | ---------- |
| **[!UICONTROL アカウントを持つ人の合計]** | アカウントに関連付けられている人の合計数を示します。 |
| **[!UICONTROL 合計アカウント数]** | アカウントの合計数を示します。 |
| **[!UICONTROL アカウントを持つ既存の人]** | データソースのアカウントに既に関連付けられているユーザーの数を示します。 |
| **[!UICONTROL 一致した人物]** | あるアカウントに一致した人の数を示します。 |
| **[!UICONTROL 不一致の人物]** | アカウントに一致しなかった担当者の数を示します。 |
| **[!UICONTROL 前回の成功した実行]** | 最後に成功したリードからアカウント照合ジョブの実行の日時を示します。 |
| **[!UICONTROL ステータス]** | リードのアカウントマッチングジョブのステータス（成功、失敗、または処理）を示します。 |

## UI コントロール {#ui-controls}

この節では、監視インターフェイスの様々なユーザーインターフェイス (UI) オプションについて説明します。このオプションを使用すると、ページに表示する指標をフィルタリングできます。

矢印アイコン (![矢印アイコン](/help/dataflows/assets/ui/monitor-destinations/chevron-up.png)) をクリックして、画面上部のカードを展開または解除します。このカードには、プロファイルエンリッチメントジョブに関する一目でわかる情報が表示されます。

![矢印アイコン UI コントロールを表示する画面記録。](/help/dataflows/assets/ui/b2b/use-arrow-control.gif)

以下を使用： **[!UICONTROL 指標とグラフ]** を切り替えて、最新の指標を表示する表示を閉じます。

![指標とグラフの切り替えを表示する画面の記録。](/help/dataflows/assets/ui/b2b/metrics-and-graphs-toggle.gif)

以下を使用： **[!UICONTROL 失敗のみを表示]** 切り替えて、失敗したプロファイルエンリッチメントジョブの表示のみに切り替えます。

![「エラーのみを表示」切り替えを表示する画面の記録。](/help/dataflows/assets/ui/b2b/show-failures-only.gif)

## 次の手順 {#next-steps}

このチュートリアルに従うことで、プロファイルエンリッチメントジョブの指標を正しく監視および理解できます。 詳しくは、次のドキュメントを参照してください。

* [リアルタイム CDP B2B の関連アカウント](/help/rtcdp/b2b-ai-ml-services/related-accounts.md)
* [アカウントプロファイル UI ガイドの「関連するアカウント」タブ](/help/rtcdp/accounts/account-profile-ui-guide.md)
* [リアルタイム CDP B2B でのアカウントマッチングにつながる](/help/rtcdp/b2b-ai-ml-services/lead-to-account-matching.md)
