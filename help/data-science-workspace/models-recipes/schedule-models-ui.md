---
keywords: Experience Platform；モデルのスケジュール；Data Science Workspace；人気の高いトピック；スコアのスケジュール；トレーニングのスケジュール
solution: Experience Platform
title: Data Science Workspace UIでのモデルのスケジュール
topic-legacy: tutorial
type: Tutorial
description: Adobe Experience Platformデータサイエンスワークスペースでは、機械学習サービスでのスケジュール済みスコアおよびトレーニングの実行を設定できます。 トレーニングとスコアリングを自動処理すると、データ内のパターンに追いつくことで、サービスの効率を時間をかけて維持および改善できます。
exl-id: 51f6f328-7c63-4de1-9184-2ba526bb82e2
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '555'
ht-degree: 18%

---

# Data Science Workspace UIでのモデルのスケジュール

Adobe Experience Platform[!DNL Data Science Workspace]では、機械学習サービスでのスケジュール済みスコアおよびトレーニングの実行を設定できます。 トレーニングとスコアリングプロセスを自動化すると、データ内のパターンに対応し、時間をかけてサービスの効率性を維持、向上させるのに役立ちます。

このチュートリアルでは、[!UICONTROL サービスギャラリー]を通じて、既存のサービスに対するトレーニングとスコアリングのスケジュールを設定する手順について説明します。 チュートリアルは以下の主な節に分かれています。

- [スコアリングのスケジュール設定](#configure-scheduled-scoring)
- [スケジュール済みトレーニングの設定](#configure-scheduled-training)

## はじめに

このチュートリアルを完了するには、[!DNL Experience Platform]にアクセスできる必要があります。 [!DNL Experience Platform]のIMS組織にアクセスできない場合は、先に進む前に、システム管理者にお問い合わせください。

このチュートリアルでは、既存のサービスが必要です。 使用するアクセス可能なサービスがない場合は、[サービス](./publish-model-service-ui.md)としてモデルをパブリッシュするためのチュートリアルに従って作成できます。

## スコアリングののスケジュール設定 {#configure-scheduled-scoring}

モデルスコアリングは、スケジュールに基づいて自動処理されるように設定できます。サービスを作成したら、次の手順に従ってスコアリングスケジュールを設定および適用できます。

Adobe Experience Platformで、左側のナビゲーション列にある&#x200B;**[!UICONTROL サービス]**&#x200B;タブを選択し、**[!DNL Service Gallery]**&#x200B;にアクセスします。 スコアリングの実行をスケジュールするサービスを探し、**[!UICONTROL 開く]**&#x200B;を選択して&#x200B;**[!UICONTROL 概要]**&#x200B;ページを表示します。

![](../images/models-recipes/schedule/select_service.png)

概要ページに、サービスのスコアリング情報が表示されます。「**[!UICONTROL スケジュールを更新]**」リンクを選択して、スコアリングスケジュールを設定します。

![](../images/models-recipes/schedule/update_scoring.png)

スコアリングスケジュールの頻度、開始日、終了日、入力データセットおよび出力データセットを設定します。設定に満足したら、「**[!UICONTROL 作成]**」を選択して、サービスのスコアリングスケジュールを更新します。

![](../images/models-recipes/schedule/set_scoring_schedule.png)

更新されたスコアリングスケジュールがサービスの&#x200B;**[!UICONTROL 概要]**&#x200B;ページに表示されます。

![](../images/models-recipes/schedule/scoring_set.png)

## スケジュール済みトレーニングの設定 {#configure-scheduled-training}

サービス上で予定されたトレーニングを実行する設定を行うと、機械学習モデルが最新のデータパターンに更新されます。 予定されたトレーニングの実行が完了するたびに、結果のトレーニングモデルを使用して、次の予定されたトレーニングの実行までサービスに電力を供給します。

サービスを作成したら、次の手順に従ってトレーニングスケジュールを設定および適用できます。

Adobe Experience Platformで、左のナビゲーション列にある「**[!UICONTROL サービス]**」タブを選択し、**[!UICONTROL サービスギャラリー]**&#x200B;にアクセスします。 トレーニングの実行をスケジュールするサービスを探し、**[!UICONTROL 開く]**&#x200B;を選択して&#x200B;**[!UICONTROL 概要]**&#x200B;ページを表示します。

![](../images/models-recipes/schedule/select_service.png)

概要ページには、サービスのトレーニング情報が表示されます。 トレーニングスケジュールを設定するには、「**[!UICONTROL スケジュールを更新]**」リンクを選択します。

![](../images/models-recipes/schedule/update_training.png)

トレーニングスケジュールに使用する頻度、開始日、終了日、入力データセットを設定します。設定が完了したら、「**[!UICONTROL 作成]**」を選択して、サービスのトレーニングスケジュールを更新します。

![](../images/models-recipes/schedule/set_training_schedule.png)

更新されたトレーニングスケジュールは、サービスの&#x200B;**[!UICONTROL 概要]**&#x200B;ページに表示されます。

![](../images/models-recipes/schedule/training_set.png)

## 次の手順

このチュートリアルに従うと、サービスでの自動トレーニングとスコアリングの実行を正しくスケジュールし、[!DNL Data Science Workspace]チュートリアルUIワークフローを完了できます。 まだ実行していない場合は、チュートリアル](./create-retails-sales-dataset.md)を再開し、APIワークフローに従って、モデルの作成、トレーニング、スコアリング、および公開を行ってください。[
