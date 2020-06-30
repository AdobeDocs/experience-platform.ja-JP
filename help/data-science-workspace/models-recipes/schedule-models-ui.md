---
keywords: Experience Platform;schedule a model;Data Science Workspace;popular topics
solution: Experience Platform
title: モデルのスケジュール(UI)
topic: Tutorial
translation-type: tm+mt
source-git-commit: 4b0f0dda97f044590f55eaf75a220f631f3313ee
workflow-type: tm+mt
source-wordcount: '488'
ht-degree: 0%

---


# モデルのスケジュール(UI)

Adobe Experience Platform [!DNL Data Science Workspace] を使用すると、機械学習サービスでのスケジュール済みスコアおよびトレーニングの実行を設定できます。 トレーニングとスコアリングプロセスを自動化すると、データ内のパターンに対応し、時間をかけてサービスの効率を維持、向上させるのに役立ちます。

このチュートリアルでは、 *[!UICONTROL サービスギャラリーを通じて、既存のサービスに対するトレーニングとスコアリングのスケジュールを設定する手順について説明します]*。 以下の主な節に分かれています。

- [スケジュール済みスコアの設定](#configure-scheduled-scoring)
- [予定されたトレーニングの設定](#configure-scheduled-training)

## はじめに

このチュートリアルを完了するには、にアクセスする必要があり [!DNL Experience Platform]ます。 でIMS組織にアクセスできない場合は、先に進む前にシステム管理者にお問い合わせ [!DNL Experience Platform]ください。

このチュートリアルでは、既存のサービスが必要です。 操作するアクセス可能なサービスがない場合は、UI [チュートリアルの「モデルをサービスとして](./publish-model-service-ui.md) 公開」に従って作成できます。

## スケジュール済みスコアの設定 {#configure-scheduled-scoring}

モデルスコアリングは、スケジュールに基づいて自動化されたプロセスに設定できます。 サービスを作成したら、次の手順に従って、スコアリングスケジュールを設定および適用できます。

1. Adobe Experience Platformで、左のナビゲーション列にある「 **[!UICONTROL サービス]** 」タブをクリックして、にアクセス *[!DNL Service Gallery]*&#x200B;します。 スコアリングの実行をスケジュールするサービスを探し、 **[!UICONTROL 「開く]** 」をクリックして *概要* ページを表示します。
   ![](../images/models-recipes/schedule/click_to_open.png)

2. 概要ページには、サービスのスコアリング情報が表示されます。 「スケジュールを **[!UICONTROL 更新]** 」リンクをクリックして、スコアリングスケジュールを設定します。
   ![](../images/models-recipes/schedule/service_overview_score.png)

3. スコアリングスケジュールの頻度、開始日、終了日、入力データセット、出力データセットを設定します。 設定が完了したら、「 **[!UICONTROL 作成]** 」をクリックして、サービスのスコアリングスケジュールを更新します。
   ![](../images/models-recipes/schedule/14_configure_scoring_schedule.png)

4. 更新したスコアリングスケジュールがサービスの *概要* ページに表示されます。
   ![](../images/models-recipes/schedule/service_with_scoring_schedule.png)


## 予定されたトレーニングの設定 {#configure-scheduled-training}

サービスで予定されたトレーニングを実行する設定を行うと、機械学習モデルが最新のデータパターンに更新されます。 予定されたトレーニングの実行が完了するたびに、次の予定されたトレーニングの実行まで、このトレーニングを受けたモデルを使用してサービスに電力を供給します。

サービスを作成したら、次の手順に従ってトレーニングスケジュールを設定および適用できます。

1. 「Adobe Experience Platform」で、左のナビゲーション列にある「 **[!UICONTROL サービス]** 」タブをクリックし、「 *[!UICONTROL サービスギャラリー]*」にアクセスします。 トレーニングの実行スケジュールを設定するサービスを探し、 **[!UICONTROL 「開く]** 」をクリックして表示の *概要* ページを表示します。
   ![](../images/models-recipes/schedule/click_to_open.png)

2. 概要ページには、サービスのトレーニング情報が表示されます。 トレーニングスケジュールを設定するには、 **[!UICONTROL 「スケジュールを]** 更新」リンクをクリックします。
   ![](../images/models-recipes/schedule/service_overview_train.png)

3. トレーニングスケジュールに使用する頻度、開始日、終了日、入力データセットを設定します。 設定が完了したら、「 **[!UICONTROL 作成]** 」をクリックして、サービスのトレーニングスケジュールを更新します。
   ![](../images/models-recipes/schedule/12_configure_training_schedule.png)

4. 更新されたトレーニングスケジュールが、サービスの *概要* ページに表示されます。
   ![](../images/models-recipes/schedule/service_with_training_schedule.png)

## 次の手順

このチュートリアルに従うと、サービスでの自動トレーニングとスコアリングの実行を正常にスケジュールし、チュートリアルの [!DNL Data Science Workspace] UIワークフローを完了できます。 まだモデルを作成、トレーニング、スコアリングおよび公開していない場合は、チュートリアルを [再起動し](./create-retails-sales-dataset.md) 、APIワークフローに従ってモデルを作成、トレーニング、スコアリングおよび公開します。
