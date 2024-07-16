---
keywords: Experience Platform；モデルの公開；Data Science Workspace；人気のトピック；サービスのスコアリング
solution: Experience Platform
title: Data Science Workspace UI でのサービスとしてのモデルのPublish
type: Tutorial
description: Adobe Experience Platform Data Science Workspaceを使用すると、トレーニング済みおよび評価済みのモデルをサービスとして公開でき、組織内のユーザーが独自のモデルを作成しなくてもデータのスコアを取得できます。
exl-id: ebbec1b1-20d3-43b5-82d3-89c79757625a
source-git-commit: d6a4b149b911cd6e7dbbd6c1289fce64be76b506
workflow-type: tm+mt
source-wordcount: '505'
ht-degree: 37%

---

# データサイエンスワークスペース UI でのサービスとしてのモデルの公開 {#publish-a-model-as-a-service}

>[!CONTEXTUALHELP]
>id="platform_intelligentservices_publishmodel"
>title="サービスとしてのモデルの公開"
>abstract=""

Adobe Experience Platform Data Science Workspaceを使用すると、トレーニング済みおよび評価済みのモデルをサービスとして公開でき、組織内のユーザーが独自のモデルを作成しなくてもデータのスコアを取得できます。

## はじめに

このチュートリアルを完了するには、[!DNL Experience Platform] へのアクセス権が必要です。 [!DNL Experience Platform] の組織にアクセスする権限がない場合は、続行する前にシステム管理者に問い合わせてください。

このチュートリアルでは、トレーニングを正常に実行できる既存のモデルが必要です。公開できるモデルがない場合は、「[UI でのモデルの訓練と評価](./train-evaluate-model-ui.md)」チュートリアルに従って操作を続行します。

Sensei 機械学習 API を使用してモデルを公開する場合は、[API のチュートリアル](./publish-model-service-api.md)を参照してください。

## モデルの公開 {#publish-a-model}

Adobe Experience Platformで、左側のナビゲーション列にある **[!UICONTROL モデル]** を選択し、「**[!UICONTROL 参照]** タブを選択して、既存のすべてのモデルをリストします。 サービスとして公開するモデルの名前を選択します。

![](../images/models-recipes/publish-model/browse_model.png)

モデルの概要ページの右上付近にある ]**0}Publish} を選択して、サービス作成プロセスを開始します。**[!UICONTROL 

![](../images/models-recipes/publish-model/view_training.png)

サービスの名前を入力し、必要に応じてサービスの説明を入力して、終了したら「**[!UICONTROL 次へ]**」を選択します。

![](../images/models-recipes/publish-model/configure_training.png)

モデルに対する実行が成功したトレーニングがすべて表示されます。新しいサービスは、選択したトレーニングの実行からトレーニングとスコアの設定を継承します。

![](../images/models-recipes/publish-model/select_training_run.png)

「**[!UICONTROL 完了]**」を選択してサービスを作成し、「**[!UICONTROL サービスギャラリー]**」にリダイレクトして、新しく作成されたサービスを含む、使用可能なすべてのサービスを表示します。

![](../images/models-recipes/publish-model/service_gallery.png)

## サービスを使用したスコア {#access-a-service}

Adobe Experience Platformで、左側のナビゲーション列にある「**[!UICONTROL サービス]**」タブを選択し、**[!UICONTROL サービスギャラリー]** にアクセスします。 使用するサービスを見つけて **[!UICONTROL 開く]** を選択します。

![](../images/models-recipes/publish-model/open_service.png)

サービスの概要ページ内で、「**[!UICONTROL スコア]**」を選択します。

![](../images/models-recipes/publish-model/score_service.png)

スコアリング実行に適した入力データセットを選択し、「**[!UICONTROL 次へ]**」を選択します。 スコアリングデータセットに対して同じ手順を実行するように求められます。 入力および出力データセットを選択したら、設定を更新できます。

![](../images/models-recipes/publish-model/select_datasets.png)

サービスを作成すると、デフォルトのスコア設定が継承されます。これらの設定を確認し、必要に応じて値をダブルクリックして調整できます。設定に問題がなければ、「**[!UICONTROL 完了]**」を選択してスコアリング実行を開始します。

![](../images/models-recipes/publish-model/scoring_configs.png)

サービスの&#x200B;**概要**&#x200B;ページに、新しいスコアリングジョブとその進行状況の詳細が表示されます。ジョブが完了すると、**[!UICONTROL スコアリング]** コンテナ内の **[!UICONTROL 最新]** ヘッダーが更新されます。

![](../images/models-recipes/publish-model/pending_scoring.png)

## 次の手順 {#next-steps}

このチュートリアルでは、[!UICONTROL サービスギャラリー]を通じてアクセス可能なサービスとしてモデルを公開し、新しいサービスを使用してデータをスコアリングしました。次のチュートリアルに進み、[サービスで自動トレーニングとスコアリングの実行をスケジュールする](./schedule-models-ui.md)方法を学びます。
