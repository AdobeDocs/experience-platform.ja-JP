---
keywords: Experience Platform；モデルの公開；Data Science Workspace；人気の高いトピック；サービスのスコア付け
solution: Experience Platform
title: Data Science Workspace UI でモデルをサービスとして公開する
type: Tutorial
description: Adobe Experience Platform Data Science Workspace を使用すると、トレーニング済みおよび評価済みのモデルをサービスとして公開でき、組織内のユーザーは、独自のモデルを作成する必要なく、データをスコアリングできます。
exl-id: ebbec1b1-20d3-43b5-82d3-89c79757625a
source-git-commit: 81f48de908b274d836f551bec5693de13c5edaf1
workflow-type: tm+mt
source-wordcount: '499'
ht-degree: 34%

---

# Data Science Workspace UI でモデルをサービスとして公開する

Adobe Experience Platform Data Science Workspace を使用すると、トレーニング済みおよび評価済みのモデルをサービスとして公開でき、組織内のユーザーは、独自のモデルを作成する必要なく、データをスコアリングできます。

## はじめに

このチュートリアルを完了するには、 [!DNL Experience Platform]. の組織に対するアクセス権がない場合、 [!DNL Experience Platform]続行する前に、システム管理者にお問い合わせください。

このチュートリアルでは、トレーニングを正常に実行できる既存のモデルが必要です。公開できるモデルがない場合は、「[UI でのモデルの訓練と評価](./train-evaluate-model-ui.md)」チュートリアルに従って操作を続行します。

Sensei 機械学習 API を使用してモデルを公開する場合は、[API のチュートリアル](./publish-model-service-api.md)を参照してください。

## モデルの公開 {#publish-a-model}

Adobe Experience Platformで、 **[!UICONTROL モデル]** 左側のナビゲーション列に配置され、「 **[!UICONTROL 参照]** 」タブをクリックして、既存のすべてのモデルを一覧表示します。 サービスとして公開するモデルの名前を選択します。

![](../images/models-recipes/publish-model/browse_model.png)

選択 **[!UICONTROL 公開]** 「モデルの概要」ページの右上近くにあるサービス作成プロセスを開始します。

![](../images/models-recipes/publish-model/view_training.png)

サービスの名前を入力し、必要に応じてサービスの説明を入力して、 **[!UICONTROL 次へ]** 終了したとき。

![](../images/models-recipes/publish-model/configure_training.png)

モデルに対する実行が成功したトレーニングがすべて表示されます。新しいサービスは、選択したトレーニングの実行からトレーニングとスコアの設定を継承します。

![](../images/models-recipes/publish-model/select_training_run.png)

選択 **[!UICONTROL 完了]** サービスを作成し、 **[!UICONTROL サービスギャラリー]** ：新しく作成されたサービスを含む、使用可能なすべてのサービスを表示します。

![](../images/models-recipes/publish-model/service_gallery.png)

## サービスを使用したスコア {#access-a-service}

Adobe Experience Platformで、 **[!UICONTROL サービス]** 左側のナビゲーション列にあるタブで、 **[!UICONTROL サービスギャラリー]**. 使用するサービスを見つけ、「 」を選択します。 **[!UICONTROL 開く]**.

![](../images/models-recipes/publish-model/open_service.png)

サービスの概要ページで、「 」を選択します。 **[!UICONTROL スコア]**.

![](../images/models-recipes/publish-model/score_service.png)

スコアリング実行に適した入力データセットを選択し、「 」を選択します。 **[!UICONTROL 次へ]**. スコアリングデータセットに対しても同じ手順を実行するように求められます。 入出力データセットを選択したら、設定を更新できます。

![](../images/models-recipes/publish-model/select_datasets.png)

サービスを作成すると、デフォルトのスコア設定が継承されます。これらの設定を確認し、必要に応じて値をダブルクリックして調整できます。設定が完了したら、「 」を選択します。 **[!UICONTROL 完了]** をクリックして、スコアリングの実行を開始します。

![](../images/models-recipes/publish-model/scoring_configs.png)

サービスの&#x200B;**概要**&#x200B;ページに、新しいスコアリングジョブとその進行状況の詳細が表示されます。ジョブが完了したら、 **[!UICONTROL 最新]** ヘッダー内 **[!UICONTROL スコア]** コンテナが更新されます。

![](../images/models-recipes/publish-model/pending_scoring.png)

## 次の手順 {#next-steps}

このチュートリアルでは、[!UICONTROL サービスギャラリー]を通じてアクセス可能なサービスとしてモデルを公開し、新しいサービスを使用してデータをスコアリングしました。次のチュートリアルに進み、[サービスで自動トレーニングとスコアリングの実行をスケジュールする](./schedule-models-ui.md)方法を学びます。
