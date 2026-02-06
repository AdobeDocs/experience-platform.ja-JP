---
description: '[!UICONTROL Profile Enrichment] ダッシュボードを使用して、プロファイルエンリッチメントジョブが正常に実行および完了したかどうかを把握し、エンリッチメントの効果を測定する基本指標を表示します。'
solution: Experience Platform
title: プロファイルエンリッチメントジョブの監視
type: Tutorial
badgeB2B: label="B2B edition" type="Informative" url="https://experienceleague.adobe.com/docs/experience-platform/rtcdp/intro/rtcdp-intro/overview.html?lang=ja#rtcdp-editions" newtab=true
exl-id: 096a2212-ed7f-4419-8ead-fa1ca01c2804
source-git-commit: 0e993f7d0791f5f6f9dce63eb3848609d892e788
workflow-type: tm+mt
source-wordcount: '696'
ht-degree: 4%

---

# UI でのプロファイルエンリッチメントジョブの監視 {#monitor-profile-enrichment}

[!UICONTROL Profile Enrichment] ダッシュボードを使用して、プロファイルエンリッチメントジョブが正常に実行および完了したかどうかを把握し、エンリッチメントの効果を測定する基本指標を表示します。

[Experience Platform UI](https://platform.adobe.com) で、左側のナビゲーションから **[!UICONTROL Monitoring]** を選択して、[!UICONTROL Monitoring] ダッシュボードにアクセスします。 表示セレクターで「**B2B フロー**」を選択して、[Real-Time CDP B2B](/help/rtcdp/b2b-overview.md) に固有のダッシュボード要素を表示します。  [!UICONTROL Monitoring] ダッシュボードには、最新の成功した実行からの基本指標と、最大 90 日前の毎日のジョブステータスが含まれます。

## 関連するアカウントのプロファイルエンリッチメント {#related-accounts}

[!UICONTROL Related accounts] ダッシュボードには、基本指標と、（関連するアカウント [&#x200B; プロファイルエンリッチメントに固有の毎日のジョブのステータス &#x200B;](/help/rtcdp/b2b-ai-ml-services/related-accounts.md) 表示されます。

![Experience Platform UI のプロファイルエンリッチメントジョブ監視画面に移動する方法を示す視覚的な指標。](/help/dataflows/assets/ui/b2b/monitoring-profile-enrichment-jobs.png)

**[!UICONTROL Metrics]** カードのデータには、関連アカウントジョブが最後に正常に実行されたときの基本的な指標が含まれます。

関連するアカウントプロファイルエンリッチメントジョブでは、次の指標を使用できます。

| 指標 | 説明 |
| --------- | ---------- |
| **[!UICONTROL Total account profiles]** | 組織がアクセスできるアカウントプロファイルの合計を示します。 |
| **[!UICONTROL Account groups]** | 関連するアカウントの機械学習ジョブによってクラスター化されたアカウントグループの数を示します。 |
| **[!UICONTROL Single-account groups]** | 他のアカウントとグループ化されていないアカウントの数を示します。 |
| **[!UICONTROL Largest group size]** | 関連する最大のアカウントグループのサイズを示します。 許可される最大グループサイズは 30 です。 |
| **[!UICONTROL Median group size]** | 組織内の関連アカウントグループのサイズの中央値を示します。 |
| **[!UICONTROL Last successful run]** | 関連アカウントジョブが最後に正常に実行された日時を示します。 |
| **[!UICONTROL Status]** | 関連アカウントジョブのステータス（成功、失敗または処理中）を示します。 |
| **[!UICONTROL Message]** | 特定のジョブ実行に関するエラーまたは警告メッセージを示します。 |

## リードとアカウントのマッチングプロファイルのエンリッチメント {#lead-to-account-matching}

[!UICONTROL Lead to account matching] ダッシュボードには、（リードとアカウントのマッチング [&#x200B; プロファイルエンリッチメントに固有の基本指標と毎日のジョブ実行ステータスが表示 &#x200B;](/help/rtcdp/b2b-ai-ml-services/lead-to-account-matching.md) れます。

![&#x200B; リードとアカウントのマッチングプロファイルのエンリッチメント &#x200B;](/help/dataflows/assets/ui/b2b/mpc-lead-to-account-matching.png)

リードとアカウントのマッチングプロファイルエンリッチメントジョブでは、次の指標を使用できます。

| 指標 | 説明 |
| --------- | ---------- |
| **[!UICONTROL Total persons with accounts]** | アカウントに関連付けられているユーザーの合計数を示します。 |
| **[!UICONTROL Total accounts]** | アカウントの合計数を示します。 |
| **[!UICONTROL Existing persons with accounts]** | データソースから既にアカウントに関連付けられているユーザーの数を示します。 |
| **[!UICONTROL Persons matched]** | アカウントと一致した人物の数を示します。 |
| **[!UICONTROL Persons unmatched]** | アカウントと一致しなかった人物の数を示します。 |
| **[!UICONTROL Last successful run]** | 最後に成功したリードとアカウントの一致するジョブの実行の日時を示します。 |
| **[!UICONTROL Status]** | リードとアカウントのマッチングジョブのステータス（成功、失敗または処理中）を示します。 |

## リードおよびアカウントの予測スコアリングプロファイルのエンリッチメント {#predictive-lead-to-account-scoring}

[!UICONTROL Predictive lead and account scoring] ダッシュボードには、プロファイルエンリッチメント [&#x200B; リードおよびアカウントの予測スコアリング &#x200B;](/help/rtcdp/b2b-ai-ml-services/predictive-lead-and-account-scoring.md) に固有の基本指標と毎日のジョブ実行ステータスが表示されます。

![&#x200B; リードおよびアカウントの予測スコアリングプロファイルエンリッチメント &#x200B;](/help/dataflows/assets/ui/b2b/predictive-lead-and-account-scoring.png)

リードおよびアカウントの予測スコアリングプロファイルエンリッチメントジョブでは、次の指標を使用できます。

| 指標 | 説明 |
| --------- | ---------- |
| **[!UICONTROL Job start]** | リードおよびアカウントの予測スコアリングジョブ実行の開始日時を示します。 |
| **[!UICONTROL Processing time]** | ジョブが完了するまでにかかった合計時間。 |
| **[!UICONTROL Score name]** | ジョブのスコア名。 |
| **[!UICONTROL Profile type]** | スコアのタイプ： <ul><li>人物</li><li>アカウント</li></ul>。 |
| **[!UICONTROL Job type]** | ジョブのタイプ。<ul><li>スコアリング</li><li>トレーニング</li>。 |
| **[!UICONTROL Status]** | リードおよびアカウントの予測スコアリングジョブのステータス（成功、失敗または処理中）を示します。 |

## UI コントロール {#ui-controls}

この節では、モニタリングインターフェイスの様々なユーザーインターフェイス（UI）オプションについて説明します。これらのオプションを使用すると、ページに表示される指標をフィルタリングできます。

矢印アイコン（![&#x200B; 矢印アイコン &#x200B;](/help/images/icons/chevron-up.png)）を使用すると、画面上部のカードを展開したり展開解除したりできます。このカードには、プロファイルエンリッチメントジョブに関する情報が一目で確認できます。

![&#x200B; 矢印アイコン UI コントロールを表示する画面録画。](/help/dataflows/assets/ui/b2b/use-arrow-control.gif)

「**[!UICONTROL Metrics and graphs]**」トグルを使用して、最新の指標を表示するビューを解除します。

![&#x200B; 「指標とグラフ」トグルを表示する画面録画。](/help/dataflows/assets/ui/b2b/metrics-and-graphs-toggle.gif)

**[!UICONTROL Show failures only]** 切替スイッチを使用して、失敗したプロファイルエンリッチメントジョブのみを表示します。

![&#x200B; 「失敗のみを表示」切替スイッチを表示する画面録画。](/help/dataflows/assets/ui/b2b/show-failures-only.gif)

## 次の手順 {#next-steps}

このチュートリアルに従うと、プロファイルエンリッチメントジョブの指標を正常に監視し、理解できます。 詳しくは、次のドキュメントを参照してください。

* [Real-Time CDP B2B の関連するアカウント](/help/rtcdp/b2b-ai-ml-services/related-accounts.md)
* [アカウントプロファイル UI ガイドの「関連するアカウント」タブ](/help/rtcdp/accounts/account-profile-ui-guide.md)
* [Real-Time CDP B2B でのリードとアカウントのマッチング](/help/rtcdp/b2b-ai-ml-services/lead-to-account-matching.md)
* [Real-Time CDP B2B でのリードおよびアカウントの予測スコアリング](/help/rtcdp/b2b-ai-ml-services/predictive-lead-and-account-scoring.md)
