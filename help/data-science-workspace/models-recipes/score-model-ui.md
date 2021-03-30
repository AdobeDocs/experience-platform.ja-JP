---
keywords: Experience Platform；モデルのスコア；データサイエンスワークスペース；人気の高いトピック；ui；スコアリングの実行；スコアリングの結果
solution: Experience Platform
title: Data Science Workspace UIでのモデルのスコア設定
topic: チュートリアル
type: チュートリアル
description: 'Adobe Experience Platform Data Science Workspace　でのスコアリングは、既存のトレーニング済みモデルに入力データを送ることで達成できます。次に、スコアリング結果が保存され、新しいバッチとして指定した出力データセットで表示可能になります。 '
translation-type: tm+mt
source-git-commit: 7feda18351061600b05dc7be8afbbfb9a0fa7ec1
workflow-type: tm+mt
source-wordcount: '650'
ht-degree: 31%

---


# Data Science Workspace UIでモデルにスコアを付ける

Adobe Experience Platform[!DNL Data Science Workspace]でのスコアは、既存のトレーニングを受けたモデルに入力データを送ることで達成できます。 次に、スコアリング結果が保存され、新しいバッチとして指定した出力データセットで表示可能になります。

このチュートリアルでは、[!DNL Data Science Workspace]ユーザーインターフェイスでモデルにスコアを付けるために必要な手順を説明します。

## はじめに

このチュートリアルを完了するには、[!DNL Experience Platform]にアクセスできる必要があります。 [!DNL Experience Platform]のIMS組織にアクセスできない場合は、先に進む前に、システム管理者にお問い合わせください。

このチュートリアルには、トレーニング済みのモデルが必要です。トレーニング済みモデルがない場合は、続行する前に、『[UI でのモデルのトレーニングと評価](./train-evaluate-model-ui.md)』チュートリアルに従ってください。

## 新しいスコアリングの実行の作成

スコアリングの実行は、以前に完了し評価されたトレーニングの実行の最適化された設定を使用して作成されます。モデルの最適な設定のセットは、通常、トレーニングの実行評価指標を見直すことで決定されます。

最も最適なトレーニング実行を見つけて、その設定をスコアリングに使用します。次に、目的のトレーニングを開き、名前に付けられたハイパーリンクを選択します。

![トレーニングの実行を選択](../images/models-recipes/score/select-run.png)

トレーニングの「**[!UICONTROL 評価]**」タブから、画面の右上にある「**[!UICONTROL スコア]**」を選択します。 新しいスコアリングワークフローが開始されます。

![](../images/models-recipes/score/training_run_overview.png)

入力スコアリングデータセットを選択し、「**[!UICONTROL 次へ]**」を選択します。

![](../images/models-recipes/score/scoring_input.png)

出力スコアリングデータセットを選択します。これは、スコアリング結果が保存される専用の出力データセットです。選択内容を確認し、「**[!UICONTROL 次へ]**」を選択します。

![](../images/models-recipes/score/scoring_results.png)

ワークフローの最後の手順で、スコアリングの実行を設定するよう求められます。これらの設定は、スコアリング実行のモデルで使用されます。
モデルの作成中に設定された継承パラメータは削除できません。 継承されていないパラメーターは、重複が値をクリックするか、エントリの上にカーソルを置いた状態で「元に戻す」アイコンを選択することで編集または元に戻すことができます。

![設定](../images/models-recipes/score/configuration.png)

スコアリングの設定を確認し、「**[!UICONTROL 完了]**」を選択して、スコアリング実行を作成し実行します。 「**[!UICONTROL スコアリングの実行]**」タブに移動し、**[!UICONTROL 保留]**&#x200B;ステータスの新しいスコアリングの実行が表示されます。

![スコアリング実行タブ](../images/models-recipes/score/scoring_runs_tab.png)

スコアリングの実行は、次のいずれかのステータスで表示できます。
- 保留中
- Complete
- Failed
- 実行中

ステータスは自動的に更新されます。 ステータスが&#x200B;**[!UICONTROL 完了]**&#x200B;または&#x200B;**[!UICONTROL 失敗]**&#x200B;の場合は、次の手順に進みます。

## スコアリング結果の表示

表示スコアリング結果を開始するには、トレーニングランを選択します。

![トレーニングの実行を選択](../images/models-recipes/score/select-run.png)

トレーニングの実行&#x200B;**[!UICONTROL 評価]**&#x200B;ページにリダイレクトされます。 トレーニング実行の評価ページの上部近くにある「**[!UICONTROL スコアリング実行]**」タブを選択し、既存のスコアリング実行のリストを表示します。

![評価ページ](../images/models-recipes/score/view_scoring_runs.png)

次に、実行の詳細を表示するためのスコアリングの実行を選択します。

![詳細を実行する](../images/models-recipes/score/view_details.png)

選択したスコアリング実行のステータスが「完了」または「失敗」の場合、**[!UICONTROL 表示アクティビティログ]**&#x200B;リンクが使用可能になります。 スコアリングの実行が失敗した場合、実行ログには、失敗の理由を判断するのに役立つ情報が記録されます。 実行ログをダウンロードするには、[**[!UICONTROL 表示アクティビティログ]**]を選択します。

![表示ログの選択](../images/models-recipes/score/view_logs.png)

**[!UICONTROL 表示アクティビティlogs]**&#x200B;プロバーが表示されます。 URLを選択して、関連付けられたログを自動的にダウンロードします。

![](../images/models-recipes/score/activity_logs.png)

また、**[!UICONTROL プレビュースコアリング結果データセット]**&#x200B;を選択して、スコアリング結果の表示を行うこともできます。

![プレビュー結果の選択](../images/models-recipes/score/view_results.png)

出力データセットのプレビューが提供されます。

![プレビュー結果](../images/models-recipes/score/preview_results.png)

スコアリング結果の完全なセットについては、右の列にある&#x200B;**[!UICONTROL スコアリング結果データセット]**&#x200B;リンクを選択します。

## 次の手順

このチュートリアルでは、[!DNL Data Science Workspace]のトレーニングを受けたモデルを使用してデータにスコアを付ける手順を順を追って説明しました。 [UI でモデルをサービスとして公開](./publish-model-service-ui.md)する方法のチュートリアルに従って、組織内のユーザーが機械学習サービスに簡単にアクセスしてデータをスコアリングできるようにします。
