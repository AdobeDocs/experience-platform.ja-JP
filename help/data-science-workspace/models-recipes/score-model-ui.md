---
keywords: Experience Platform；モデルのスコアリング；Data Science Workspace；人気のトピック；ui；スコアリング実行；スコアリング結果
solution: Experience Platform
title: Data Science Workspace UI でのモデルのスコアリング
type: Tutorial
description: Adobe Experience Platform Data Science Workspace　でのスコアリングは、既存のトレーニング済みモデルに入力データを送ることで達成できます。次に、スコアリング結果が保存され、新しいバッチとして指定した出力データセットで表示可能になります。
exl-id: 00d6a872-d71a-47f4-8625-92621d4eed56
source-git-commit: 81f48de908b274d836f551bec5693de13c5edaf1
workflow-type: tm+mt
source-wordcount: '647'
ht-degree: 31%

---

# Data Science Workspace UI でのモデルのスコアリング

Adobe Experience Platform [!DNL Data Science Workspace] でのスコアリングは、入力データを既存のトレーニング済みモデルにフィードすることで実現できます。 次に、スコアリング結果が保存され、新しいバッチとして指定した出力データセットで表示可能になります。

このチュートリアルでは、[!DNL Data Science Workspace] ユーザーインターフェイスでモデルのスコアを取得するために必要な手順を示します。

## はじめに

このチュートリアルを完了するには、[!DNL Experience Platform] へのアクセス権が必要です。 [!DNL Experience Platform] の組織にアクセスする権限がない場合は、続行する前にシステム管理者に問い合わせてください。

このチュートリアルには、トレーニング済みのモデルが必要です。トレーニング済みモデルがない場合は、続行する前に、『[UI でのモデルのトレーニングと評価](./train-evaluate-model-ui.md)』チュートリアルに従ってください。

## 新しいスコアリングの実行の作成

スコアリングの実行は、以前に完了し評価されたトレーニングの実行の最適化された設定を使用して作成されます。モデルの最適な設定のセットは、通常、トレーニングの実行評価指標を見直すことで決定されます。

最も最適なトレーニング実行を見つけて、その設定をスコアリングに使用します。次に、名前に添付されているハイパーリンクを選択して、目的のトレーニング実行を開きます。

![ トレーニング実行を選択 ](../images/models-recipes/score/select-run.png)

トレーニング実行 **[!UICONTROL 評価]** タブで、画面右上の **[!UICONTROL スコア]** を選択します。 新しいスコアリングワークフローが開始されます。

![](../images/models-recipes/score/training_run_overview.png)

入力スコアリングデータセットを選択し、「**[!UICONTROL 次へ]**」を選択します。

![](../images/models-recipes/score/scoring_input.png)

出力スコアリングデータセットを選択します。これは、スコアリング結果が保存される専用の出力データセットです。選択内容を確認し、「**[!UICONTROL 次へ]**」を選択します。

![](../images/models-recipes/score/scoring_results.png)

ワークフローの最後の手順で、スコアリングの実行を設定するよう求められます。これらの設定は、スコアリング実行のモデルで使用されます。
モデルの作成中に設定した継承されたパラメータは削除できません。 値をダブルクリックするか、エントリの上にカーソルを置いたときに元に戻すアイコンを選択すると、継承されていないパラメーターを編集または元に戻すことができます。

![ 設定 ](../images/models-recipes/score/configuration.png)

スコアリング設定を確認して確認し、「**[!UICONTROL 完了]**」を選択してスコアリング実行を作成して実行します。 「**[!UICONTROL スコアリング実行]**」タブに移動し、「**[!UICONTROL 保留]** ステータスの新規スコアリング実行が表示されます。

![ 「スコアリング実行」タブ ](../images/models-recipes/score/scoring_runs_tab.png)

スコアリング実行は、次のいずれかのステータスで表示できます。
- 保留中
- Complete
- 失敗
- 実行中

ステータスは自動的に更新されます。 ステータスが **[!UICONTROL 完了]** または **[!UICONTROL 失敗]** の場合は、次の手順に進みます。

## スコアリング結果の表示

スコアリング結果を表示するには、まずトレーニング実行を選択します。

![ トレーニング実行を選択 ](../images/models-recipes/score/select-run.png)

トレーニング実行 **[!UICONTROL 評価]** ページにリダイレクトされます。 トレーニング実行評価ページの上部付近にある「**[!UICONTROL スコアリング実行]**」タブを選択して、既存のスコアリング実行のリストを表示します。

![ 評価ページ ](../images/models-recipes/score/view_scoring_runs.png)

次に、スコアリング実行を選択して実行の詳細を表示します。

![ 実行の詳細 ](../images/models-recipes/score/view_details.png)

選択したスコアリング実行のステータスが「完了」または「失敗」の場合は、「アクティビティ・ログの表示 **[!UICONTROL リンクが使用可能]** なります。 スコアリング実行が失敗した場合、実行ログは失敗の理由の判断に役立つ情報を提供できます。 実行ログをダウンロードするには、「**[!UICONTROL アクティビティログを表示]**」を選択します。

![ ログを表示を選択 ](../images/models-recipes/score/view_logs.png)

**[!UICONTROL アクティビティログを表示]** ポップオーバーが表示されます。 関連するログを自動的にダウンロードする URL を選択します。

![](../images/models-recipes/score/activity_logs.png)

また、「**[!UICONTROL スコアリング結果データセットをプレビュー]**」を選択してスコアリング結果を表示することもできます。

![ プレビュー結果を選択 ](../images/models-recipes/score/view_results.png)

出力データセットのプレビューが表示されます。

![ 結果をプレビュー ](../images/models-recipes/score/preview_results.png)

スコアリング結果の完全なセットについては、右側の列にある **[!UICONTROL スコアリング結果データセット]** リンクを選択します。

## 次の手順

このチュートリアルでは、[!DNL Data Science Workspace] のトレーニング済みモデルを使用してデータにスコアを付ける手順を説明しました。 [UI でモデルをサービスとして公開](./publish-model-service-ui.md)する方法のチュートリアルに従って、組織内のユーザーが機械学習サービスに簡単にアクセスしてデータをスコアリングできるようにします。
