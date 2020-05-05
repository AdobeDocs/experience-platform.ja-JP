---
keywords: Experience Platform;schedule a model;Data Science Workspace;popular topics
solution: Experience Platform
title: モデルのスケジュール(UI)
topic: Tutorial
translation-type: tm+mt
source-git-commit: e08460bc76d79920bbc12c7665a1416d69993f34

---


# モデルのスケジュール(UI)

Adobe Experience Platform Data Science Workspaceを使用すると、機械学習サービスでのスコアおよびトレーニングの実行をスケジュール設定できます。 トレーニングとスコアリングプロセスを自動化すると、データ内のパターンに対応し、時間をかけてサービスの効率を維持、向上させるのに役立ちます。

このチュートリアルでは、 *サービスギャラリーを通じて、既存のサービスに対するトレーニングとスコアリングのスケジュールを設定する手順について説明します*。 以下の主な節に分かれています。

- [スケジュール済みスコアの設定](#configure-scheduled-scoring)
- [予定されたトレーニングの設定](#configure-scheduled-training)

## はじめに

このチュートリアルを完了するには、エクスペリエンスプラットフォームにアクセスできる必要があります。 Experience PlatformのIMS組織にアクセスできない場合は、先に進む前に、システム管理者にお問い合わせください。

このチュートリアルでは、既存のサービスが必要です。 操作するアクセス可能なサービスがない場合は、UI [チュートリアルの「モデルをサービスとして](./publish-model-service-ui.md) 公開」に従って作成できます。

## スケジュール済みスコアの設定 {#configure-scheduled-scoring}

モデルスコアリングは、スケジュールに基づいて自動化されたプロセスに設定できます。 サービスを作成したら、次の手順に従って、スコアリングスケジュールを設定および適用できます。

1. Adobe Experience Platformで、左側のナビゲーション列にある **[!UICONTROL Services]** タブをクリックして、 *サービスギャラリーにアクセスします*。 スコアリングの実行をスケジュールするサービスを探し、をクリックして **[!UICONTROL Open]** 概要 ** ページを表示します。
   ![](../images/models-recipes/schedule/click_to_open.png)

2. 概要ページには、サービスのスコアリング情報が表示されます。 リンクをクリックして、スコアリングスケジュールを設定します。 **[!UICONTROL Update Schedule]**
   ![](../images/models-recipes/schedule/service_overview_score.png)

3. スコアリングスケジュールの頻度、開始日、終了日、入力データセット、出力データセットを設定します。 設定が完了したら、をクリックして、サービス **[!UICONTROL Create]** のスコアリングスケジュールを更新します。
   ![](../images/models-recipes/schedule/14_configure_scoring_schedule.png)

4. 更新したスコアリングスケジュールがサービスの *概要* ページに表示されます。
   ![](../images/models-recipes/schedule/service_with_scoring_schedule.png)


## 予定されたトレーニングの設定 {#configure-scheduled-training}

サービスで予定されたトレーニングを実行する設定を行うと、機械学習モデルが最新のデータパターンに更新されます。 予定されたトレーニングの実行が完了するたびに、次の予定されたトレーニングの実行まで、このトレーニングを受けたモデルを使用してサービスに電力を供給します。

サービスを作成したら、次の手順に従ってトレーニングスケジュールを設定および適用できます。

1. Adobe Experience Platformで、左側のナビゲーション列にある **[!UICONTROL Services]** タブをクリックして、 *サービスギャラリーにアクセスします*。 トレーニングの実施をスケジュールするサービスを探し、「 **[!UICONTROL Open]** 概要 ** 」ページをクリックして表示します。
   ![](../images/models-recipes/schedule/click_to_open.png)

2. 概要ページには、サービスのトレーニング情報が表示されます。 トレーニングスケジュールを設定するには、 **[!UICONTROL Update Schedule]** リンクをクリックします。
   ![](../images/models-recipes/schedule/service_overview_train.png)

3. トレーニングスケジュールに使用する頻度、開始日、終了日、入力データセットを設定します。 設定が完了したら、をクリックして、サービス **[!UICONTROL Create]** のトレーニングスケジュールを更新します。
   ![](../images/models-recipes/schedule/12_configure_training_schedule.png)

4. 更新されたトレーニングスケジュールが、サービスの *概要* ページに表示されます。
   ![](../images/models-recipes/schedule/service_with_training_schedule.png)

## 次の手順

このチュートリアルに従うと、サービスでの自動トレーニングとスコアの実行を正しくスケジュールし、Data Science WorkspaceチュートリアルのUIワークフローを完了しました。 まだモデルを作成、トレーニング、スコアリングおよび公開していない場合は、チュートリアルを [再起動し](./create-retails-sales-dataset.md) 、APIワークフローに従ってモデルを作成、トレーニング、スコアリングおよび公開します。
