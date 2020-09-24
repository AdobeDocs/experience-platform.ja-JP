---
keywords: Experience Platform;schedule a model;Data Science Workspace;popular topics;schedule scoring;schedule training
solution: Experience Platform
title: モデルのスケジュール設定（UI）
topic: tutorial
type: Tutorial
description: Adobe Experience Platformデータサイエンスワークスペースでは、機械学習サービスでのスケジュール済みスコアおよびトレーニングの実行を設定できます。 トレーニングとスコアリングを自動処理すると、データ内のパターンに追いつくことで、サービスの効率を時間をかけて維持および改善できます。
translation-type: tm+mt
source-git-commit: 97dfd3a9a66fe2ae82cec8954066bdf3b6346830
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 79%

---


# モデルのスケジュール設定（UI）

Adobe Experience Platform [!DNL Data Science Workspace] allows you to set up scheduled scoring and training runs on a machine learning Service. トレーニングとスコアリングを自動処理すると、データ内のパターンに追いつくことで、サービスの効率を時間をかけて維持および改善できます。

このチュートリアルでは、**[!UICONTROL サービスギャラリー]**&#x200B;を通じて、既存のサービスに対するトレーニングとスコアリングのスケジュールを設定する手順について説明します。チュートリアルは以下の主な節に分かれています。

- [スコアリングのスケジュール設定](#configure-scheduled-scoring)
- [スケジュール済みトレーニングの設定](#configure-scheduled-training)

## はじめに

In order to complete this tutorial, you must have access to [!DNL Experience Platform]. If you do not have access to an IMS Organization in [!DNL Experience Platform], please speak to your system administrator before proceeding.

このチュートリアルには既存のサービスが必要です。使用するアクセス可能なサービスがない場合は、『[UI でのサービスとしてのモデルの公開](./publish-model-service-ui.md)』チュートリアルに従ってサービスを作成できます。

## スコアリングののスケジュール設定 {#configure-scheduled-scoring}

モデルスコアリングは、スケジュールに基づいて自動処理されるように設定できます。サービスを作成したら、次の手順に従って、スコアリングのスケジュールを設定および適用できます。

1. In Adobe Experience Platform, click the **[!UICONTROL Services]** tab located in the left navigation column to access the *[!DNL Service Gallery]*. スコアリング実行のスケジュールを設定するサービスを探し、「**[!UICONTROL 開く]**」をクリックして、その&#x200B;*概要*ページを表示します。
   ![](../images/models-recipes/schedule/click_to_open.png)

2. 概要ページに、サービスのスコアリング情報が表示されます。「**[!UICONTROL スケジュールの更新]**」リンクをクリックして、スコアリングのスケジュールを設定します。
   ![](../images/models-recipes/schedule/service_overview_score.png)

3. スコアリングスケジュールの頻度、開始日、終了日、入力データセットおよび出力データセットを設定します。設定が完了したら、「**[!UICONTROL 作成]**」をクリックして、サービスのスコアリングスケジュールを更新します。
   ![](../images/models-recipes/schedule/14_configure_scoring_schedule.png)

4. 更新されたスコアリングスケジュールが、サービスの&#x200B;*概要*ページに表示されます。
   ![](../images/models-recipes/schedule/service_with_scoring_schedule.png)


## スケジュール済みトレーニングの設定 {#configure-scheduled-training}

サービスでトレーニング実行をスケジュール設定すると、機械学習モデルが最新のデータパターンに更新されます。スケジュールされたトレーニングの実行が完了するたびに、結果のトレーニング済みモデルは、次のスケジュール済みトレーニングの実行までサービスを強化するために使用されます。

サービスを作成したら、次の手順に従ってトレーニングスケジュールを設定および適用できます。

1. Adobe Experience Platform で、左側のナビゲーション列にある「**[!UICONTROL サービス]**」タブをクリックして、**[!UICONTROL サービスギャラリー]**&#x200B;にアクセスします。トレーニング実行のスケジュールを設定するサービスを探し、「**[!UICONTROL 開く]**」をクリックして、その&#x200B;*概要*ページを表示します。
   ![](../images/models-recipes/schedule/click_to_open.png)

2. 概要ページには、サービスのトレーニング情報が表示されます。「**[!UICONTROL スケジュールの更新]**」リンクをクリックして、トレーニングスケジュールを設定します。
   ![](../images/models-recipes/schedule/service_overview_train.png)

3. トレーニングスケジュールに使用する頻度、開始日、終了日、入力データセットを設定します。設定が完了したら、「**[!UICONTROL 作成]**」をクリックして、サービスのトレーニングスケジュールを更新します。
   ![](../images/models-recipes/schedule/12_configure_training_schedule.png)

4. 更新されたトレーニングスケジュールが、サービスの&#x200B;*概要*ページに表示されます。
   ![](../images/models-recipes/schedule/service_with_training_schedule.png)

## 次の手順

By following this tutorial, you have successfully scheduled automated training and scoring runs on a Service, and completed the [!DNL Data Science Workspace] tutorial UI workflow. まだおこなっていない場合は、[チュートリアルを再開](./create-retails-sales-dataset.md)して、API ワークフローに従ってモデルを作成、トレーニング、スコアリング、および公開することを検討してください。
