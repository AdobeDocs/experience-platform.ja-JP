---
keywords: Experience Platform；モデルの発行；Data Science Workspace；人気の高いトピック；サービスのスコア
solution: Experience Platform
title: モデルのサービスとしての公開（UI）
topic: tutorial
type: Tutorial
description: Adobe Experience Platform Data Science Workspace　を使用すると、訓練を受けた評価済みのモデルをサービスとして公開でき、IMS 組織内のユーザーは、独自のモデルを作成する必要なくデータをスコアリングできます。
translation-type: tm+mt
source-git-commit: 8c94d3631296c1c3cc97501ccf1a3ed995ec3cab
workflow-type: tm+mt
source-wordcount: '469'
ht-degree: 87%

---


# モデルのサービスとしての公開（UI）

Adobe Experience Platform Data Science Workspace　を使用すると、訓練を受けた評価済みのモデルをサービスとして公開でき、IMS 組織内のユーザーは、独自のモデルを作成する必要なくデータをスコアリングできます。

## はじめに

このチュートリアルを完了するには、[!DNL Experience Platform]にアクセスできる必要があります。 [!DNL Experience Platform]のIMS組織にアクセスできない場合は、先に進む前に、システム管理者にお問い合わせください。

このチュートリアルでは、トレーニングを正常に実行できる既存のモデルが必要です。公開できるモデルがない場合は、「[UI でのモデルの訓練と評価](./train-evaluate-model-ui.md)」チュートリアルに従って操作を続行します。

Sensei 機械学習 API を使用してモデルを公開する場合は、[API のチュートリアル](./publish-model-service-api.md)を参照してください。

## モデルの公開 {#publish-a-model}

1. Adobe Experience Platform で、左側のナビゲーション列にある「**[!UICONTROL モデル]**」リンクをクリックして、既存のすべてのモデルを一覧表示します。サービスとして公開するモデルの名前を探し、クリックします。
   ![](../images/models-recipes/publish-model/1_browse_model.png)
2. サービス作成プロセスを開始するには、モデル概要ページの右上近くにある「**[!UICONTROL 公開]**」をクリックします。
   ![](../images/models-recipes/publish-model/2_view_training_runs.png)
3. サービスの名前を入力し、必要に応じてサービスの説明を入力します。完了したら、「**[!UICONTROL 次へ]**」をクリックします。
   ![](../images/models-recipes/publish-model/3_configure_service.png)
4. モデルに対する実行が成功したトレーニングがすべて表示されます。新しいサービスは、選択したトレーニングの実行からトレーニングとスコアの設定を継承します。
   ![](../images/models-recipes/publish-model/4_select_training_run.png)
5. 「**[!UICONTROL 完了]**」をクリックしてサービスを作成し、「**[!UICONTROL サービスギャラリー]**」にリダイレクトして、新しく作成したサービスを含む、使用可能なすべてのサービスを表示します。
   ![](../images/models-recipes/publish-model/service_gallery.png)

## サービスを使用したスコア  {#access-a-service}

1. Adobe Experience Platform で、左側のナビゲーション列にある「**[!UICONTROL サービス]**」タブをクリックして、**[!UICONTROL サービスギャラリー]**&#x200B;にアクセスします。使用するサービスを見つけ、「**[!UICONTROL スコア]**」をクリックします。
   ![](../images/models-recipes/publish-model/click_to_score.png)
2. スコアリング実行に適した入力データセットを選択し、「**[!UICONTROL 次へ]**」をクリックします。
   ![](../images/models-recipes/publish-model/6_scoring_input.png)
3. スコアリング結果に適した出力データセットを選択し、「**[!UICONTROL 次へ]**」をクリックします。
   ![](../images/models-recipes/publish-model/7_scoring_output.png)
4. サービスを作成すると、デフォルトのスコア設定が継承されます。これらの設定を確認し、必要に応じて値をダブルクリックして調整できます。設定が完了したら、「**[!UICONTROL 完了]**」をクリックして、スコアリングの実行を開始します。
   ![](../images/models-recipes/publish-model/8_scoring_configure.png)
5. サービスの&#x200B;**概要**&#x200B;ページに、新しいスコアリングジョブとその進行状況の詳細が表示されます。ジョブが完了すると、**[!UICONTROL スコア]**&#x200B;コンテナ内の&#x200B;**[!UICONTROL 最新]**ヘッダーが更新されます。
   ![](../images/models-recipes/publish-model/score_pending.png)

## 次の手順 {#next-steps}

このチュートリアルでは、[!UICONTROL サービスギャラリー]を通じてアクセス可能なサービスとしてモデルを公開し、新しいサービスを使用してデータをスコアリングしました。次のチュートリアルに進み、[サービスで自動トレーニングとスコアリングの実行をスケジュールする](./schedule-models-ui.md)方法を学びます。
