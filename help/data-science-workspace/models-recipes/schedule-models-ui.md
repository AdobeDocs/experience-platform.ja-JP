---
keywords: Experience Platform;モデルをスケジュールする。データ科学ワークスペース;人気のあるトピック;スケジュールスコアリングスケジュールトレーニング
solution: Experience Platform
title: データ科学ワークスペース UIのモデルスケジュール
type: Tutorial
description: Adobe Experience Platform データ Science ワークスペースでは、機械学習サービスでスケジュールされたスコアリングとトレーニングの実行を設定できます。 トレーニングとスコアリングを自動処理すると、データ内のパターンに追いつくことで、サービスの効率を時間をかけて維持および改善できます。
exl-id: 51f6f328-7c63-4de1-9184-2ba526bb82e2
source-git-commit: 5d98dc0cbfaf3d17c909464311a33a03ea77f237
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 17%

---

# データ科学ワークスペース UIのモデルスケジュール

>[!NOTE]
>
>データサイエンスワークスペースは購入できなくなりました。
>
>このドキュメントは、以前に データ Science ワークスペース の利用資格を持つ既存のお客様を対象としています。

Adobe Experience Platform [!DNL Data Science Workspace] を使用すると、機械学習サービスでスケジュールされたスコアリングとトレーニングの実行を設定できます。 トレーニングとスコアリングのプロセスを自動化すると、データ内のパターンに追いつくことで、サービスの効率を長期にわたって維持および向上させることができます。

このチュートリアルでは、 [!UICONTROL サービス ギャラリー]を使用して既存のサービスのトレーニングスケジュールとスコアリング スケジュールを構成する手順について説明します。 チュートリアルは以下の主な節に分かれています。

- [スコアリングののスケジュール設定](#configure-scheduled-scoring)
- [スケジュール済みトレーニングの設定](#configure-scheduled-training)

## はじめに

このチュートリアルを完了するには、 [!DNL Experience Platform]へのアクセス権が必要です。 [!DNL Experience Platform] の組織にアクセスできない場合は、続行する前にシステム管理者に問い合わせてください。

このチュートリアルには既存のサービスが必要です。 使用するアクセス可能なサービスがない場合は、モデルをサービスとして [公開](./publish-model-service-ui.md)チュートリアルに従って作成できます。

## スコアリングののスケジュール設定 {#configure-scheduled-scoring}

モデルスコアリングは、スケジュールに基づいて自動処理されるように設定できます。サービスを作成したら、次の手順フォローする、スコアリングスケジュールを設定および適用できます。

Adobe Experience Platformで、左側のナビゲーション列にある **[!UICONTROL サービス]** タブを選択して **[!DNL Service Gallery]**&#x200B;にアクセスします。 スコアリング実行をスケジュールするサービス検索文字列、 **[!UICONTROL 開く]** を選択してその **[!UICONTROL 概要]** ページ表示します。

![](../images/models-recipes/schedule/select_service.png)

概要ページに、サービスのスコアリング情報が表示されます。「 **[!UICONTROL 更新」スケジュール]** リンクを選択して、スコアリングスケジュールを設定します。

![](../images/models-recipes/schedule/update_scoring.png)

スコアリングスケジュールの頻度、開始日、終了日、入力データセットおよび出力データセットを設定します。構成に問題がなければ、 [ **[!UICONTROL 作成]** ] を選択してサービスのスコアリング スケジュールを更新します。

![](../images/models-recipes/schedule/set_scoring_schedule.png)

更新されたスコアリング スケジュールがサービスの **[!UICONTROL 概要]** ページに表示されます。

![](../images/models-recipes/schedule/scoring_set.png)

## スケジュール済みトレーニングの設定 {#configure-scheduled-training}

サービスでスケジュールされたトレーニングの実行を構成すると、機械学習モデルが最新のデータ パターンに更新されます。 スケジュールされたトレーニングの実行が完了するたびに、結果のトレーニング済みモデルを使用して、次にスケジュールされたトレーニングの実行までサービスを強化します。

サービスを作成したら、フォローする手順に従って、トレーニングスケジュールを設定および適用できます。

Adobe Experience Platformで、左側のナビゲーション列にある **[!UICONTROL サービス]** タブを選択して **[!UICONTROL サービス ギャラリー]**&#x200B;にアクセスします。 トレーニング実行をスケジュールするサービス検索文字列、 **[!UICONTROL 開く]** を選択してその **[!UICONTROL 概要]** ページ表示します。

![](../images/models-recipes/schedule/select_service.png)

概要 ページには、サービスのトレーニング情報が表示されます。 トレーニングスケジュールを設定するには、 **[!UICONTROL 更新スケジュール]** リンクを選択します。

![](../images/models-recipes/schedule/update_training.png)

トレーニングスケジュールに使用する頻度、開始日、終了日、入力データセットを設定します。構成に問題がなければ、 [ **[!UICONTROL 作成]** ] を選択してサービスのトレーニングスケジュールを更新します。

![](../images/models-recipes/schedule/set_training_schedule.png)

更新されたトレーニングスケジュールがサービスの **[!UICONTROL 概要]** ページに表示されます。

![](../images/models-recipes/schedule/training_set.png)

## 次の手順

このチュートリアルに従うことで、サービスでの自動トレーニングとスコアリングの実行が正常にスケジュールされ、 [!DNL Data Science Workspace] チュートリアル UI ワークフローが完了しました。 まだ行っていない場合は、チュートリアル[&#128279;](./create-retails-sales-dataset.md)を再起動し、API ワークフローフォローするしてモデルを作成、トレーニング、スコア付け、公開するすることを検討してください。
