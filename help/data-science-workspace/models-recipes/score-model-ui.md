---
keywords: Experience Platform;score a model;Data Science Workspace;popular topics
solution: Experience Platform
title: モデルのスコアリング（UI）
topic: Tutorial
translation-type: tm+mt
source-git-commit: 1e5526b54f3c52b669f9f6a792eda0abfc711fdd
workflow-type: tm+mt
source-wordcount: '573'
ht-degree: 86%

---


# モデルのスコアリング（UI）

Scoring in Adobe Experience Platform [!DNL Data Science Workspace] can be achieved by feeding input data into an existing trained Model. 次に、スコアリング結果が保存され、新しいバッチとして指定した出力データセットで表示可能になります。

This tutorial demonstrates the steps required to score a Model in the [!DNL Data Science Workspace] user interface.

## はじめに

In order to complete this tutorial, you must have access to [!DNL Experience Platform]. If you do not have access to an IMS Organization in [!DNL Experience Platform], please speak to your system administrator before proceeding.

このチュートリアルには、トレーニング済みのモデルが必要です。トレーニング済みモデルがない場合は、続行する前に、『[UI でのモデルのトレーニングと評価](./train-evaluate-model-ui.md)』チュートリアルに従ってください。

## 新しいスコアリングの実行の作成

スコアリングの実行は、以前に完了し評価されたトレーニングの実行の最適化された設定を使用して作成されます。モデルの最適な設定のセットは、通常、トレーニングの実行評価指標を見直すことで決定されます。

1. 最も最適なトレーニング実行を見つけて、その設定をスコアリングに使用します。目的のトレーニング実行を開き、名前をクリックします。

2. 「トレーニング実行の&#x200B;**[!UICONTROL 評価]**」タブで、画面右上の「**[!UICONTROL スコア]**」ボタンをクリックします。これにより、新しい&#x200B;*スコアリングの実行*ワークフローが開始されます。
   ![](../images/models-recipes/score/training_run_overview.png)

3. 入力スコアリングデータセットを選択し、「**[!UICONTROL 次へ]**」をクリックします。
   ![](../images/models-recipes/score/scoring_input.png)

4. 出力スコアリングデータセットを選択します。これは、スコアリング結果が保存される専用の出力データセットです。選択を確定し、「**[!UICONTROL 次へ]**」をクリックします。
   ![](../images/models-recipes/score/scoring_results.png)

5. ワークフローの最後の手順で、スコアリングの実行を設定するよう求められます。これらの設定は、スコアリングの実行にモデルで使用されます。
モデルの作成時に設定された継承パラメーターは削除できないことに注意してください。継承されていないパラメーターは、値をダブルクリックするか、エントリにカーソルを合わせて元に戻すアイコンをクリックすると編集するまたは元に戻すことができます。
   ![](../images/models-recipes/score/configuration.png)
スコアリング設定を確認して、「**[!UICONTROL 完了]**」をクリックし、スコアリングの実行を作成して実行します。「*スコアリングの実行*」タブに移動すると、新しいスコアリングの実行にステータスが表示されます。
   ![](../images/models-recipes/score/scoring_runs_tab.png)
スコアリングの実行には、「保留中」、「完了」、「失敗」、「実行中」の 4 つのステータスのいずれかが表示され、自動的に更新されます。ステータスが「完了」または「失敗」の場合は、次の手順に進みます。

## スコアリング結果の表示

1. スコアリングの実行に使用されたトレーニングの実行を見つけ、名前をクリックして、「**[!UICONTROL 評価]**」ページを表示します。

2. トレーニングの実行の評価ページの上部近くにある「**[!UICONTROL スコアリングの実行]**」タブをクリックし 、既存のスコアリングの実行のリストを表示します。スコアリングリストをクリックすると、右の列に詳細が表示されます。
   ![](../images/models-recipes/score/view_details.png)

3. 選択したスコアリングの実行のステータスが「完了」または「失敗」の場合、右の列にある&#x200B;**[!UICONTROL 表示アクティビティログ]**リンクがアクティブになります。リンクをクリックして、実行ログを表示またはダウンロードします。スコアリングの実行が失敗した場合、実行ログには、失敗の理由を判断する際に役立つ情報が記録されます。
   ![](../images/models-recipes/score/activity_logs.png)

4. 右側の列にある「**[!UICONTROL スコアリング結果データセットのプレビュー]**」リンクをクリックします。スコアリングの実行の出力データセットのプレビューを確認できるようになります。
   ![](../images/models-recipes/score/preview_results.png)

5. スコアリング結果の完全なセットについては、右側の列にある「**[!UICONTROL スコアリング結果データセット]**」リンクをクリックします。

## 次の手順

This tutorial walked you through the steps to score data using a trained Model in [!DNL Data Science Workspace]. [UI でモデルをサービスとして公開](./publish-model-service-ui.md)する方法のチュートリアルに従って、組織内のユーザーが機械学習サービスに簡単にアクセスしてデータをスコアリングできるようにします。
