---
keywords: Experience Platform;score a model;Data Science Workspace;popular topics
solution: Experience Platform
title: モデルにスコアを付ける(UI)
topic: Tutorial
translation-type: tm+mt
source-git-commit: e08460bc76d79920bbc12c7665a1416d69993f34

---


# モデルにスコアを付ける(UI)

Adobe Experience Platform Data Science Workspaceでのスコアリングは、既存のトレーニングを受けたモデルに入力データを入力することで達成できます。 次に、スコアリング結果が新しいバッチとして指定した出力データセットに保存され、表示可能になります。

このチュートリアルでは、Data Science Workspaceユーザーインターフェイスでモデルにスコアを付けるために必要な手順を説明します。

## はじめに

このチュートリアルを完了するには、エクスペリエンスプラットフォームにアクセスできる必要があります。 Experience PlatformのIMS組織にアクセスできない場合は、先に進む前に、システム管理者にお問い合わせください。

このチュートリアルには、トレーニングを受けたモデルが必要です。 トレーニングを受けたモデルがない場合は、先に進む前に、UI [チュートリアルで](./train-evaluate-model-ui.md) トレーニングに従ってモデルを評価してください。

## 新しいスコアリング実行の作成

スコアリング実行は、以前に完了し評価されたトレーニング実行の最適化された設定を使用して作成されます。 モデルの最適な設定は、通常、トレーニングの実行評価指標を見直すことで決定されます。

1. スコアリングに設定を使用するための最適なトレーニングの実施を見つけます。 目的のトレーニングを開き、名前をクリックします。

2. 「Training Run」 **[!UICONTROL Evaluation]** タブで、画面の右上にある **[!UICONTROL Score]** ボタンをクリックします。 これにより、新しい *実行スコアリング* ワークフローが開始されます。
   ![](../images/models-recipes/score/training_run_overview.png)

3. 入力スコアリングデータセットを選択し、をクリックし **[!UICONTROL Next]**ます。
   ![](../images/models-recipes/score/scoring_input.png)

4. 出力スコアリングデータセットを選択します。これは、スコアリング結果が保存される専用の出力データセットです。 選択を確定し、をクリックし **[!UICONTROL Next]**ます。
   ![](../images/models-recipes/score/scoring_results.png)

5. ワークフローの最後の手順に従って、スコアリングの実行を設定するよう求められます。 これらの設定は、スコアリング実行のモデルで使用されます。
モデルの作成時に設定された継承パラメータは削除できないことに注意してください。 継承されていないパラメーターは、重複が値をクリックするか、エントリの上にカーソルを置いた状態で元に戻すアイコンをクリックすることで、編集または元に戻すことができます。
   ![](../images/models-recipes/score/configuration.png)
スコアリングの設定を確認し、をクリックしてスコアリング実行 **[!UICONTROL Finish]** を作成および実行します。 「 *スコアリング実行* 」タブに移動し、新しいスコアリング実行にステータスが表示されます。
   ![](../images/models-recipes/score/scoring_runs_tab.png)
スコアリングの実行には、次の4つのステータスのいずれかが表示されます。 保留中、完了、失敗、または実行中で、自動的に更新されます。 ステータスが「完了」または「失敗」の場合は、次の手順に進みます。

## 表示スコアリング結果

1. スコアリングの実行に使用されたトレーニングの実行を探し、名前をクリックしてその **[!UICONTROL Evaluation]** ページを表示します。

2. トレーニング実行の評価ページの上部近くにあるタブをクリックし、既存のスコアリング実行のリストを表示し **[!UICONTROL Scoring Runs]** ます。 スコアリングのリストをクリックして、右側の列に詳細を表示します。
   ![](../images/models-recipes/score/view_details.png)

3. 選択したスコアリング実行のステータスが「完了」または「失敗」の場合、右列の **[!UICONTROL View Activity Logs]** リンクはアクティブになります。 表示へのリンクをクリックするか、実行ログをダウンロードします。 スコアリングの実行が失敗した場合、実行ログには、失敗の理由を判断する際に役立つ情報が記録されます。
   ![](../images/models-recipes/score/activity_logs.png)

4. 右側の列にある **[!UICONTROL Preview Scoring Results Dataset]** リンクをクリックします。 スコアリング実行の出力データセットのプレビューを確認できます。
   ![](../images/models-recipes/score/preview_results.png)

5. スコアリング結果の完全なセットについては、右側の列にある **[!UICONTROL Scoring Results Dataset]** リンクをクリックします。

## 次の手順

このチュートリアルでは、Data Science Workspaceのトレーニングを受けたモデルを使用してデータにスコアを付ける手順を順を追って説明しました。 機械学習サービスに簡単にアクセスできるようにして、組織内のユーザーがデータをスコアできるように、UI [](./publish-model-service-ui.md) でモデルをサービスとして公開する方法に関するチュートリアルに従ってください。
