---
keywords: Experience Platform、モデルのスケジュール、Data Science Workspace、人気の高いトピック、スコアリングのスケジュール、トレーニングのスケジュール
solution: Experience Platform
title: Data Science Workspace UI でのモデルのスケジュール
topic-legacy: tutorial
type: Tutorial
description: Adobe Experience Platform Data Science Workspace を使用すると、機械学習サービスでスコアリングおよびトレーニングの実行をスケジュール設定できます。 トレーニングとスコアリングを自動処理すると、データ内のパターンに追いつくことで、サービスの効率を時間をかけて維持および改善できます。
exl-id: 51f6f328-7c63-4de1-9184-2ba526bb82e2
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '555'
ht-degree: 18%

---

# Data Science Workspace UI でのモデルのスケジュール

Adobe Experience Platform [!DNL Data Science Workspace] では、機械学習サービスでスコアリングとトレーニングの実行をスケジュール設定できます。 トレーニングとスコアリングプロセスを自動化すると、データ内のパターンに追いつくことで、時間をかけてサービスの効率を維持し、向上させることができます。

このチュートリアルでは、[!UICONTROL  サービスギャラリー ] を通じて、既存のサービスに対するトレーニングとスコアリングのスケジュールを設定する手順について説明します。 チュートリアルは以下の主な節に分かれています。

- [スコアリングののスケジュール設定](#configure-scheduled-scoring)
- [スケジュール済みトレーニングの設定](#configure-scheduled-training)

## はじめに

このチュートリアルを完了するには、[!DNL Experience Platform] にアクセスできる必要があります。 [!DNL Experience Platform] の IMS 組織にアクセスできない場合は、先に進む前にシステム管理者にお問い合わせください。

このチュートリアルでは、既存のサービスが必要です。 使用するアクセス可能なサービスがない場合は、[ モデルをサービスとして公開する ](./publish-model-service-ui.md) 方法のチュートリアルに従って、作成できます。

## スコアリングののスケジュール設定 {#configure-scheduled-scoring}

モデルスコアリングは、スケジュールに基づいて自動処理されるように設定できます。サービスを作成したら、次の手順に従って、スコアリングのスケジュールを設定および適用できます。

Adobe Experience Platformで、左側のナビゲーション列にある「**[!UICONTROL サービス]**」タブを選択し、**[!DNL Service Gallery]** にアクセスします。 スコアリング実行のスケジュールを設定するサービスを探し、「**[!UICONTROL 開く]**」を選択して、その **[!UICONTROL 概要]** ページを表示します。

![](../images/models-recipes/schedule/select_service.png)

概要ページに、サービスのスコアリング情報が表示されます。「**[!UICONTROL スケジュールを更新]**」リンクを選択して、スコアリングのスケジュールを設定します。

![](../images/models-recipes/schedule/update_scoring.png)

スコアリングスケジュールの頻度、開始日、終了日、入力データセットおよび出力データセットを設定します。設定が完了したら、「**[!UICONTROL 作成]**」を選択して、サービスのスコアリングスケジュールを更新します。

![](../images/models-recipes/schedule/set_scoring_schedule.png)

更新されたスコアリングスケジュールが、サービスの **[!UICONTROL 概要]** ページに表示されます。

![](../images/models-recipes/schedule/scoring_set.png)

## スケジュール済みトレーニングの設定 {#configure-scheduled-training}

サービスでスケジュールされたトレーニング実行を設定すると、機械学習モデルが最新のデータパターンに更新されます。 スケジュールされたトレーニングの実行が完了するたびに、結果のトレーニング済みモデルを使用して、次のスケジュールされたトレーニングの実行までサービスを強化します。

サービスを作成したら、次の手順に従って、トレーニングスケジュールを設定および適用できます。

Adobe Experience Platformで、左側のナビゲーション列にある「**[!UICONTROL サービス]**」タブを選択し、「**[!UICONTROL サービスギャラリー]**」にアクセスします。 トレーニング実行のスケジュールを設定するサービスを探し、「**[!UICONTROL 開く]**」を選択して、その **[!UICONTROL 概要]** ページを表示します。

![](../images/models-recipes/schedule/select_service.png)

概要ページには、サービスのトレーニング情報が表示されます。 「**[!UICONTROL スケジュールを更新]**」リンクを選択して、トレーニングスケジュールを設定します。

![](../images/models-recipes/schedule/update_training.png)

トレーニングスケジュールに使用する頻度、開始日、終了日、入力データセットを設定します。設定が完了したら、「**[!UICONTROL 作成]**」を選択して、サービスのトレーニングスケジュールを更新します。

![](../images/models-recipes/schedule/set_training_schedule.png)

更新されたトレーニングスケジュールが、サービスの **[!UICONTROL 概要]** ページに表示されます。

![](../images/models-recipes/schedule/training_set.png)

## 次の手順

このチュートリアルに従うことで、サービスでの自動トレーニングとスコアリングの実行を正常にスケジュールし、[!DNL Data Science Workspace] チュートリアル UI ワークフローを完了しました。 まだおこなっていない場合は、[ チュートリアル ](./create-retails-sales-dataset.md) を再開し、API ワークフローに従ってモデルを作成、トレーニング、スコアリング、および公開することを検討してください。
