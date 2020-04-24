---
keywords: Experience Platform;publish a model;Data Science Workspace;popular topics
solution: Experience Platform
title: モデルをサービスとして公開(UI)
topic: Tutorial
translation-type: tm+mt
source-git-commit: 19823c7cf0459e045366f0baae2bd8a98416154c

---


# モデルをサービスとして公開(UI)

Adobe Experience Platform Data Science Workspaceを使用すると、トレーニングを受けた評価済みのモデルをサービスとして公開でき、IMS組織内のユーザーは、独自のモデルを作成する必要なくデータをスコアリングできます。

このチュートリアルでは、モデルをサービスとして公開する手順と、サービスギャラリーを通じてサービスを使用してデータにスコアを付ける手順 *について説明しま*&#x200B;す。 以下の主な節に分かれています。

- [モデルの公開](#publish-a-model)
- [サービスを使用したスコア](#access-a-service)

## はじめに

このチュートリアルを完了するには、エクスペリエンスプラットフォームにアクセスできる必要があります。 Experience PlatformのIMS組織にアクセスできない場合は、先に進む前にシステム管理者にお問い合わせください。

このチュートリアルでは、トレーニングを正常に実行できる既存のモデルが必要です。 パブリッシュ可能なモデルがない場合は、UIチュートリ [アルの「モデルをトレーニングし、評価する](./train-evaluate-model-ui.md) 」に従って操作を続行します。

Sensei Machine Learning APIを使用してモデルを公開する場合は、 [APIのチュートリアルを参照してください](./publish-model-service-api.md)。

## モデルの公開 {#publish-a-model}

1. Adobe Experience Platformで、左側のナビゲーション列にあるリ **[!UICONTROL Models]** ンクをクリックして、既存のすべてのモデルをリストします。 サービスとしてパブリッシュするモデルの名前を探し、クリックします。
   ![](../images/models-recipes/publish-model/1_browse_model.png)
2. 「モデ **[!UICONTROL Publish]** ルの概要」ページの右上近くにあるをクリックして、サービス作成プロセスを開始します。
   ![](../images/models-recipes/publish-model/2_view_training_runs.png)
3. サービスの名前を入力し、必要に応じて「サービスの説明」を入力します。完了したら、をク **[!UICONTROL Next]** リックします。
   ![](../images/models-recipes/publish-model/3_configure_service.png)
4. モデルに対する成功したトレーニングの実行がすべて表示されます。 新しいサービスは、選択したトレーニング実行からトレーニングとスコアの設定を継承します。
   ![](../images/models-recipes/publish-model/4_select_training_run.png)
5. をクリッ **[!UICONTROL Finish]** クして、サービスを作成し、新しく作成したサー **[!UICONTROL Service Gallery]** ビスを含む、使用可能なすべてのサービスを表示するには、にリダイレクトします。
   ![](../images/models-recipes/publish-model/service_gallery.png)

## サービスを使用したスコア {#access-a-service}

1. Adobe Experience Platformで、左側のナビゲーション **[!UICONTROL Services]** 列にあるタブをクリックして、「サービスギャラリー」にア *クセスします*。 使用するサービスを探し、をクリックします **[!UICONTROL Score]**。
   ![](../images/models-recipes/publish-model/click_to_score.png)
2. スコアリング実行に適した入力データセットを選択し、をクリックしま **[!UICONTROL Next]**す。
   ![](../images/models-recipes/publish-model/6_scoring_input.png)
3. スコアリング結果に適した出力データセットを選択し、をクリックしま **[!UICONTROL Next]**す。
   ![](../images/models-recipes/publish-model/7_scoring_output.png)
4. サービスを作成すると、デフォルトのスコア設定が継承されます。 これらの設定を確認し、必要に応じて値を重複クリックして調整できます。 設定が完了したら、をクリックしてスコア **[!UICONTROL Finish]** リングの実行を開始します。
   ![](../images/models-recipes/publish-model/8_scoring_configure.png)
5. サービスの概要ページ *に* 、新しいスコアリングジョブとその進行状況の詳細が表示されます。 ジョブが完了すると、スコア **[!UICONTROL Most Recent]** リングジョブが更新されます。
   ![](../images/models-recipes/publish-model/score_pending.png)

## 次の手順 {#next-steps}

このチュートリアルに従うと、アクセシブルなサービスとしてモデルを正常に公開し、サービスギャラリーを通じて新しいサービスを使用してデータをスコアリ *ングできま*&#x200B;す。 次のチュートリアルに進み、サービスで自動トレーニングとス [コアリングの実行をスケジュールする方法を学びます](./schedule-models-ui.md)。
