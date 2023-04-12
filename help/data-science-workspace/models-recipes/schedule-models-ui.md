---
keywords: Experience Platform、モデルのスケジュール、Data Science Workspace、人気の高いトピック、スコアリングのスケジュール、トレーニングのスケジュールを設定
solution: Experience Platform
title: Data Science Workspace UI でのモデルのスケジュール
type: Tutorial
description: Adobe Experience Platform Data Science Workspace を使用すると、機械学習サービスでスコアリングとトレーニングの実行をスケジュール設定できます。 トレーニングとスコアリングを自動処理すると、データ内のパターンに追いつくことで、サービスの効率を時間をかけて維持および改善できます。
exl-id: 51f6f328-7c63-4de1-9184-2ba526bb82e2
source-git-commit: 81f48de908b274d836f551bec5693de13c5edaf1
workflow-type: tm+mt
source-wordcount: '554'
ht-degree: 18%

---

# Data Science Workspace UI でのモデルのスケジュール

Adobe Experience Platform [!DNL Data Science Workspace] では、機械学習サービスでスコアリングとトレーニングの実行をスケジュールに沿って設定できます。 トレーニングとスコアリングのプロセスを自動化すると、データ内のパターンに追いつくことで、サービスの効率を時間をかけて維持および改善できます。

このチュートリアルでは、 [!UICONTROL サービスギャラリー]. チュートリアルは以下の主な節に分かれています。

- [スコアリングののスケジュール設定](#configure-scheduled-scoring)
- [スケジュール済みトレーニングの設定](#configure-scheduled-training)

## はじめに

このチュートリアルを完了するには、 [!DNL Experience Platform]. の組織に対するアクセス権がない場合、 [!DNL Experience Platform]続行する前に、システム管理者にお問い合わせください。

このチュートリアルでは、既存のサービスが必要です。 使用するアクセス可能なサービスがない場合は、次のチュートリアルに従ってサービスを作成できます。 [サービスとしてのモデルの公開](./publish-model-service-ui.md).

## スコアリングののスケジュール設定 {#configure-scheduled-scoring}

モデルスコアリングは、スケジュールに基づいて自動処理されるように設定できます。サービスを作成したら、次の手順に従って、スコアリングのスケジュールを設定および適用できます。

Adobe Experience Platformで、 **[!UICONTROL サービス]** 左側のナビゲーション列にあるタブで、 **[!DNL Service Gallery]**. スコアリング実行のスケジュールを設定するサービスを見つけ、「 」を選択します。 **[!UICONTROL 開く]** 見る **[!UICONTROL 概要]** ページ。

![](../images/models-recipes/schedule/select_service.png)

概要ページに、サービスのスコアリング情報が表示されます。を選択します。 **[!UICONTROL スケジュールを更新]** リンクをクリックして、スコアリングのスケジュールを設定します。

![](../images/models-recipes/schedule/update_scoring.png)

スコアリングスケジュールの頻度、開始日、終了日、入力データセットおよび出力データセットを設定します。設定が完了したら、「 」を選択します。 **[!UICONTROL 作成]** サービスのスコアリングスケジュールを更新する場合。

![](../images/models-recipes/schedule/set_scoring_schedule.png)

更新されたスコアリングスケジュールが、サービスの **[!UICONTROL 概要]** ページ。

![](../images/models-recipes/schedule/scoring_set.png)

## スケジュール済みトレーニングの設定 {#configure-scheduled-training}

サービスでトレーニング実行をスケジュール設定すると、機械学習モデルが最新のデータパターンに更新されます。 スケジュールされたトレーニングの実行が完了するたびに、結果のトレーニング済みモデルは、次のスケジュール済みトレーニングの実行までサービスを強化するために使用されます。

サービスを作成したら、次の手順に従ってトレーニングスケジュールを設定および適用できます。

Adobe Experience Platformで、 **[!UICONTROL サービス]** 左側のナビゲーション列にあるタブで、 **[!UICONTROL サービスギャラリー]**. トレーニング実行のスケジュールを設定するサービスを見つけ、「 」を選択します。 **[!UICONTROL 開く]** 見る **[!UICONTROL 概要]** ページ。

![](../images/models-recipes/schedule/select_service.png)

概要ページには、サービスのトレーニング情報が表示されます。 を選択します。 **[!UICONTROL スケジュールを更新]** トレーニングスケジュールを設定するリンク

![](../images/models-recipes/schedule/update_training.png)

トレーニングスケジュールに使用する頻度、開始日、終了日、入力データセットを設定します。設定が完了したら、「 」を選択します。 **[!UICONTROL 作成]** サービスのトレーニングスケジュールを更新する場合。

![](../images/models-recipes/schedule/set_training_schedule.png)

更新されたトレーニングスケジュールが、サービスの **[!UICONTROL 概要]** ページ。

![](../images/models-recipes/schedule/training_set.png)

## 次の手順

このチュートリアルに従うことで、サービスで自動トレーニングとスコアリングの実行が正常にスケジュールされ、 [!DNL Data Science Workspace] tutorial UI のワークフロー まだおこなっていない場合は、 [チュートリアルの再起動](./create-retails-sales-dataset.md) API ワークフローに従って、モデルの作成、トレーニング、スコアリング、公開をおこないます。
