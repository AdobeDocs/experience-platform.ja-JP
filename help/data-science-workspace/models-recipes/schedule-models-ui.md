---
keywords: Experience Platform;schedule a model;Data Science Workspace;popular topics
solution: Experience Platform
title: モデルのスケジュール(UI)
topic: Tutorial
translation-type: tm+mt
source-git-commit: 541acd9e1df8a53ae372f71230a705fb6a95d92b

---


# モデルのスケジュール(UI)

Adobe Experience Platform Data Science Workspaceを使用すると、機械学習サービスでのスケジュール済みスコアおよびトレーニングの実行を設定できます。 トレーニングとスコアリングプロセスを自動化すると、データ内のパターンに追い付くことで、時間をかけてサービスの効率を維持し、改善するのに役立ちます。

このチュートリアルでは、サービスギャラリーを通じて、既存のサービスに対するトレーニングとスコアリングのスケジュールを設定する手順に *ついて説明しま*&#x200B;す。 以下の主な節に分かれています。

- [スケジュール済みスコアの設定](#configure-scheduled-scoring)
- [予定されたトレーニングの設定](#configure-scheduled-training)

## はじめに

このチュートリアルを完了するには、エクスペリエンスプラットフォームにアクセスできる必要があります。 Experience PlatformのIMS組織にアクセスできない場合は、先に進む前にシステム管理者にお問い合わせください。

このチュートリアルには既存のサービスが必要です。 操作するアクセシブルなサービスがない場合は、UIチュートリアルの「モデルをサービスとしてパブリ [ッシュ」に従って作成できます](./publish-model-service-ui.md) 。

## スケジュール済みスコアの設定

モデルスコアリングは、スケジュールに基づいて自動化されたプロセスに設定できます。 サービスを作成したら、次の手順に従って、スコアリングスケジュールを設定および適用できます。

1. Adobe Experience Platformで、左側のナビゲーション **列にある** 「サービス」タブをクリックして、サービスギャラリーにア *クセスします*。 スコアリング実行のスケジュールを設定するサービスを探し、「開く」をクリックし **て** 、その概要ページを *表示します* 。
   ![](../images/models-recipes/schedule/click_to_open.png)

2. 概要ページに、サービスのスコア情報が表示されます。 「スケジュール **の更新** 」リンクをクリックして、スコアリングスケジュールを設定します。
   ![](../images/models-recipes/schedule/service_overview_score.png)

3. スコアリングスケジュールの頻度、開始日、終了日、入力データセットおよび出力データセットを設定します。 設定が完了したら、「作成」をクリックし **て** 、サービスのスコアリングスケジュールを更新します。
   ![](../images/models-recipes/schedule/14_configure_scoring_schedule.png)

4. 更新されたスコアリングスケジュールが、サービスの概要ページに表 *示されます* 。
   ![](../images/models-recipes/schedule/service_with_scoring_schedule.png)


## 予定されたトレーニングの設定

サービスでの定期トレーニングの実行を設定すると、機械学習モデルが最新のデータパターンに更新されます。 スケジュールされたトレーニングの実行が完了するたびに、トレーニングを受けたモデルが使用され、次のスケジュールされたトレーニングの実行までサービスの電源が投入されます。

サービスを作成したら、次の手順に従ってトレーニングスケジュールを設定および適用できます。

1. Adobe Experience Platformで、左側のナビゲーション **列にある** 「サービス」タブをクリックして、サービスギャラリーにア *クセスします*。 トレーニングの実行をスケジュールするサービスを探し、「 **Open** 」をクリックして *Overview* （概要）ページを表示します。
   ![](../images/models-recipes/schedule/click_to_open.png)

2. 概要ページには、サービスのトレーニング情報が表示されます。 トレーニングスケジ **ュールを設定するには** 、「スケジュールの更新」リンクをクリックします。
   ![](../images/models-recipes/schedule/service_overview_train.png)

3. トレーニングスケジュールに使用する頻度、開始日、終了日、入力データセットを設定します。 設定が完了したら、「作成」をクリックし **て** 、サービスのトレーニングスケジュールを更新します。
   ![](../images/models-recipes/schedule/12_configure_training_schedule.png)

4. Your updated training schedule is shown in the Service&#39;s *Overview* page.
   ![](../images/models-recipes/schedule/service_with_training_schedule.png)

## 次の手順

このチュートリアルに従うと、サービスでの自動トレーニングとスコアリングの実行のスケジュールが正常に完了し、Data Science Workspace Tutorial UIワークフローが完了します。 If you have not done so already, consider [restarting the tutorial](./create-retails-sales-dataset.md) and follow the API workflow to create, train, score, and publish a Model.
