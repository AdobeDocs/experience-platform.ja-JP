---
keywords: Experience Platform;publish a model;Data Science Workspace;popular topics
solution: Experience Platform
title: モデルをサービスとして発行(UI)
topic: Tutorial
translation-type: tm+mt
source-git-commit: 19823c7cf0459e045366f0baae2bd8a98416154c
workflow-type: tm+mt
source-wordcount: '460'
ht-degree: 0%

---


# モデルをサービスとして発行(UI)

Adobe Experience Platform Data Science Workspaceを使用すると、トレーニングを受けた評価済みのモデルをサービスとして公開でき、IMS組織内のユーザーは、独自のモデルを作成することなくデータをスコアできます。

このチュートリアルでは、モデルをサービスとして公開する手順を順を追って説明し、サー *ビスギャラリーを通じてサービスを使用してデータにスコアを付けます*。 以下の主な節に分かれています。

- [モデルのパブリッシュ](#publish-a-model)
- [サービスを使用したスコア](#access-a-service)

## はじめに

このチュートリアルを完了するには、エクスペリエンスプラットフォームにアクセスできる必要があります。 Experience PlatformのIMS組織にアクセスできない場合は、先に進む前に、システム管理者にお問い合わせください。

このチュートリアルでは、トレーニングを正常に実施した既存のモデルが必要です。 パブリッシュ可能なモデルがない場合は、先に進む前に、UI [(UI](./train-evaluate-model-ui.md) )チュートリアルのモデルをトレーニングして評価してください。

Senesie Machine Learning APIを使用してモデルを公開する場合は、 [APIチュートリアルを参照してください](./publish-model-service-api.md)。

## モデルのパブリッシュ {#publish-a-model}

1. Adobe Experience Platformで、左側のナビゲーション列にある「 **[!UICONTROL モデル]** 」リンクをクリックして、既存のすべてのモデルをリストします。 サービスとしてパブリッシュするモデルの名前を探してクリックします。
   ![](../images/models-recipes/publish-model/1_browse_model.png)
2. モデルの概要ページの右上近くにある **[!UICONTROL 公開]** (Publish)をクリックして、サービス作成プロセスを開始します。
   ![](../images/models-recipes/publish-model/2_view_training_runs.png)
3. サービスに付ける名前を入力し、必要に応じてサービスの説明を入力します。終了したら、 **[!UICONTROL 「次へ]** 」をクリックします。
   ![](../images/models-recipes/publish-model/3_configure_service.png)
4. モデルに対する成功したトレーニングの実行がすべて表示されます。 新しいサービスは、選択したトレーニング実行からトレーニングとスコア設定を継承します。
   ![](../images/models-recipes/publish-model/4_select_training_run.png)
5. 「 **[!UICONTROL 完了]** 」をクリックしてサービスを作成し、 **** サービスギャラリーにリダイレクトして、新しく作成したサービスを含む使用可能なすべてのサービスを表示します。
   ![](../images/models-recipes/publish-model/service_gallery.png)

## サービスを使用したスコア {#access-a-service}

1. Adobe Experience Platformで、左のナビゲーション列にある「 **[!UICONTROL サービス]** 」タブをクリックして、 *サービスギャラリーにアクセスします*。 使用するサービスを見つけ、「 **[!UICONTROL スコア]**」をクリックします。
   ![](../images/models-recipes/publish-model/click_to_score.png)
2. スコアリング実行に適した入力データセットを選択し、「 **[!UICONTROL 次へ]**」をクリックします。
   ![](../images/models-recipes/publish-model/6_scoring_input.png)
3. スコアリング結果に適した出力データセットを選択し、「 **[!UICONTROL 次へ]**」をクリックします。
   ![](../images/models-recipes/publish-model/7_scoring_output.png)
4. サービスを作成すると、デフォルトのスコア設定が継承されます。 これらの設定を確認し、必要に応じて値を重複クリックして調整できます。 設定が完了したら、「 **[!UICONTROL 完了]** 」をクリックしてスコアリングの実行を開始します。
   ![](../images/models-recipes/publish-model/8_scoring_configure.png)
5. サービスの *概要* ページに、新しいスコアリングジョブとその進行状況の詳細が表示されます。 ジョブが完了すると、 **[!UICONTROL 最新のスコアリングジョブが更新されます]** 。
   ![](../images/models-recipes/publish-model/score_pending.png)

## 次の手順 {#next-steps}

このチュートリアルに従うと、アクセシブルなサービスとしてモデルを正常に公開し、 *サービスギャラリーを通じて新しいサービスを使用してデータにスコアを割り当てることができます*。 次のチュートリアルに進み、サービスでの自動トレーニングとスコアリングの実行を [スケジュールする方法を学習します](./schedule-models-ui.md)。
