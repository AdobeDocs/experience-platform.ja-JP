---
description: '[!UICONTROL &#x200B; プロファイルエンリッチメント &#x200B;] ダッシュボードを使用すると、プロファイルエンリッチメントジョブが正常に実行および完了したかどうかを把握し、エンリッチメントの有効性を測定するための基本指標を表示できます。'
solution: Experience Platform
title: プロファイルエンリッチメントジョブの監視
type: Tutorial
exl-id: 096a2212-ed7f-4419-8ead-fa1ca01c2804
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '765'
ht-degree: 4%

---

# UI でのプロファイルエンリッチメントジョブの監視 {#monitor-profile-enrichment}

[!UICONTROL &#x200B; プロファイルエンリッチメント &#x200B;] ダッシュボードを使用すると、プロファイルエンリッチメントジョブが正常に実行および完了したかどうかを把握し、エンリッチメントの有効性を測定するための基本指標を表示できます。

[Experience Platform UI](https://platform.adobe.com) で、左側のナビゲーションから **[!UICONTROL モニタリング]** を選択して、[!UICONTROL &#x200B; モニタリング &#x200B;] ダッシュボードにアクセスします。 表示セレクターで「**B2B フロー**」を選択して、[Real-Time CDP B2B](/help/rtcdp/b2b-overview.md) に固有のダッシュボード要素を表示します。  [!UICONTROL &#x200B; モニタリング &#x200B;] ダッシュボードには、最新の成功実行からの基本指標と、最大 90 日前の毎日のジョブステータスが含まれます。

## 関連するアカウントのプロファイルエンリッチメント {#related-accounts}

[!UICONTROL &#x200B; 関連するアカウント &#x200B;] ダッシュボードには、基本的な指標と、[ 関連するアカウント ](/help/rtcdp/b2b-ai-ml-services/related-accounts.md) プロファイルエンリッチメントに固有の毎日のジョブのステータスが表示されます。

![Experience Platform UI のプロファイルエンリッチメントジョブ監視画面に移動する方法を示す視覚的な指標。](/help/dataflows/assets/ui/b2b/monitoring-profile-enrichment-jobs.png)

**[!UICONTROL 指標]** カードのデータには、関連アカウントジョブが最後に正常に実行されたときの基本的な指標が含まれます。

関連するアカウントプロファイルエンリッチメントジョブでは、次の指標を使用できます。

| 指標 | 説明 |
| --------- | ---------- |
| **[!UICONTROL 合計アカウントプロファイル数]** | 組織がアクセスできるアカウントプロファイルの合計を示します。 |
| **[!UICONTROL アカウントグループ]** | 関連するアカウントの機械学習ジョブによってクラスター化されたアカウントグループの数を示します。 |
| **[!UICONTROL 単一アカウントグループ]** | 他のアカウントとグループ化されていないアカウントの数を示します。 |
| **[!UICONTROL 最大グループサイズ]** | 関連する最大のアカウントグループのサイズを示します。 許可される最大グループサイズは 30 です。 |
| **[!UICONTROL 中央値グループのサイズ]** | 組織内の関連アカウントグループのサイズの中央値を示します。 |
| **[!UICONTROL 前回成功した実行]** | 関連アカウントジョブが最後に正常に実行された日時を示します。 |
| **[!UICONTROL ステータス]** | 関連アカウントジョブのステータス（成功、失敗または処理中）を示します。 |
| **[!UICONTROL メッセージ]** | 特定のジョブ実行に関するエラーまたは警告メッセージを示します。 |

## リードとアカウントのマッチングプロファイルのエンリッチメント {#lead-to-account-matching}

[!UICONTROL &#x200B; リードからアカウントへのマッチング &#x200B;] ダッシュボードには、「リードからアカウントへのマッチング [ プロファイルエンリッチメントに固有の基本指標と毎日のジョブ実行ステータスが表示さ ](/help/rtcdp/b2b-ai-ml-services/lead-to-account-matching.md) ます。

![ リードとアカウントのマッチングプロファイルのエンリッチメント ](/help/dataflows/assets/ui/b2b/mpc-lead-to-account-matching.png)

リードとアカウントのマッチングプロファイルエンリッチメントジョブでは、次の指標を使用できます。

| 指標 | 説明 |
| --------- | ---------- |
| **[!UICONTROL 全口数]** | アカウントに関連付けられているユーザーの合計数を示します。 |
| **[!UICONTROL アカウント総数]** | アカウントの合計数を示します。 |
| **[!UICONTROL 既存のアカウントを持つユーザー]** | データソースから既にアカウントに関連付けられているユーザーの数を示します。 |
| **[!UICONTROL 一致した人物]** | アカウントと一致した人物の数を示します。 |
| **[!UICONTROL 一致しなかった人物]** | アカウントと一致しなかった人物の数を示します。 |
| **[!UICONTROL 前回成功した実行]** | 最後に成功したリードとアカウントの一致するジョブの実行の日時を示します。 |
| **[!UICONTROL ステータス]** | リードとアカウントのマッチングジョブのステータス（成功、失敗または処理中）を示します。 |

## リードおよびアカウントの予測スコアリングプロファイルのエンリッチメント {#predictive-lead-to-account-scoring}

[!UICONTROL &#x200B; リードおよびアカウントの予測スコアリング &#x200B;] ダッシュボードには、[ リードおよびアカウントの予測スコアリング ](/help/rtcdp/b2b-ai-ml-services/predictive-lead-and-account-scoring.md) プロファイルエンリッチメントに固有の基本指標と毎日のジョブ実行ステータスが表示されます。

![ リードおよびアカウントの予測スコアリングプロファイルエンリッチメント ](/help/dataflows/assets/ui/b2b/predictive-lead-and-account-scoring.png)

リードおよびアカウントの予測スコアリングプロファイルエンリッチメントジョブでは、次の指標を使用できます。

| 指標 | 説明 |
| --------- | ---------- |
| **[!UICONTROL ジョブ開始]** | リードおよびアカウントの予測スコアリングジョブ実行の開始日時を示します。 |
| **[!UICONTROL 処理時間]** | ジョブが完了するまでにかかった合計時間。 |
| **[!UICONTROL スコア名]** | ジョブのスコア名。 |
| **[!UICONTROL プロファイルタイプ]** | スコアのタイプ： <ul><li>人物</li><li>アカウント</li></ul>。 |
| **[!UICONTROL ジョブタイプ]** | ジョブのタイプ。<ul><li>スコアリング</li><li>トレーニング</li>。 |
| **[!UICONTROL ステータス]** | リードおよびアカウントの予測スコアリングジョブのステータス（成功、失敗または処理中）を示します。 |

## UI コントロール {#ui-controls}

この節では、モニタリングインターフェイスの様々なユーザーインターフェイス（UI）オプションについて説明します。これらのオプションを使用すると、ページに表示される指標をフィルタリングできます。

矢印アイコン（![ 矢印アイコン ](/help/images/icons/chevron-up.png)）を使用すると、画面上部のカードを展開したり展開解除したりできます。このカードには、プロファイルエンリッチメントジョブに関する情報が一目で確認できます。

![ 矢印アイコン UI コントロールを表示する画面録画。](/help/dataflows/assets/ui/b2b/use-arrow-control.gif)

**[!UICONTROL 指標とグラフ]** トグルを使用して、最新の指標を表示するビューを閉じます。

![ 「指標とグラフ」トグルを表示する画面録画。](/help/dataflows/assets/ui/b2b/metrics-and-graphs-toggle.gif)

**[!UICONTROL 失敗のみを表示]** 切替スイッチを使用すると、失敗したプロファイルエンリッチメントジョブのみを表示できます。

![ 「失敗のみを表示」切替スイッチを表示する画面録画。](/help/dataflows/assets/ui/b2b/show-failures-only.gif)

## 次の手順 {#next-steps}

このチュートリアルに従うと、プロファイルエンリッチメントジョブの指標を正常に監視し、理解できます。 詳しくは、次のドキュメントを参照してください。

* [Real-Time CDP B2B の関連するアカウント](/help/rtcdp/b2b-ai-ml-services/related-accounts.md)
* [アカウントプロファイル UI ガイドの「関連するアカウント」タブ](/help/rtcdp/accounts/account-profile-ui-guide.md)
* [Real-Time CDP B2B でのリードとアカウントのマッチング](/help/rtcdp/b2b-ai-ml-services/lead-to-account-matching.md)
* [Real-Time CDP B2B でのリードおよびアカウントの予測スコアリング](/help/rtcdp/b2b-ai-ml-services/predictive-lead-and-account-scoring.md)
