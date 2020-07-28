---
keywords: Experience Platform;train and evaluate;Data Science Workspace;popular topics
solution: Experience Platform
title: モデル（UI）のトレーニングと評価
topic: Tutorial
translation-type: tm+mt
source-git-commit: 1e5526b54f3c52b669f9f6a792eda0abfc711fdd
workflow-type: tm+mt
source-wordcount: '975'
ht-degree: 93%

---


# モデル（UI）のトレーニングと評価

Adobe Experience Platform Data Science Workspace　では、モデルの意図に適した既存のレシピを組み込むことで、機械学習モデルが作成されます。次に、モデルに関連するハイパーパラメーターを微調整することで、モデルの動作効率と有効性を最適化するようにトレーニングおよび評価します。レシピは再利用可能で、複数のモデルを作成し、単一のレシピで特定の目的に合わせてカスタマイズできます。

このチュートリアルでは、モデルの作成、トレーニング、評価の手順について説明します。

## はじめに

In order to complete this tutorial, you must have access to [!DNL Experience Platform]. If you do not have access to an IMS Organization in [!DNL Experience Platform], please speak to your system administrator before proceeding.

このチュートリアルでは、既存のレシピが必要です。レシピがない場合は、先に進む前に、「[UI へのパッケージレシピの読み込み](./import-packaged-recipe-ui.md)」チュートリアルに従ってください。

## モデルの作成

1. Adobe Experience Platform で、左側のナビゲーション列にある「**[!UICONTROL モデル]**」リンクをクリックして、既存のすべてのモデルを一覧表示します。ページの右上近くにある「**[!UICONTROL モデルの作成]**」をクリックして、モデル作成プロセスを開始します。
   ![](../images/models-recipes/train-evaluate-ui/models_browse.png)

2. 既存のレシピのリストを参照し、モデルの作成に使用するレシピを探して選択し、「**[!UICONTROL 次へ]**」をクリックします。
   ![](../images/models-recipes/train-evaluate-ui/select_recipe.png)

3. 適切な入力データセットを選択し、「**[!UICONTROL 次へ]**」をクリックします。これにより、モデルのデフォルトの入力トレーニングデータセットが設定されます。
   ![](../images/models-recipes/train-evaluate-ui/select_dataset.png)

4. モデルの名前を指定し、デフォルトのモデル設定を確認します。レシピの作成中にデフォルトの設定が適用されています。値をダブルクリックして設定値を確認および変更します。新しい設定のセットを指定するには、「**[!UICONTROL 新しい設定をアップロード]**」をクリックし、モデル構成を含む JSON ファイルをブラウザーウィンドウにドラッグします。「**[!UICONTROL 完了]**」をクリックして、モデルを作成します。
   >[!NOTE]設定は、意図されたレシピに固有です。例えば、「小売販売レシピ」の設定は、「製品レコメンデーションレシピ」に対しては機能しません。小売販売レシピ設定のリストについては「[リファレンス](#reference)」の節を照してください。

   ![](../images/models-recipes/train-evaluate-ui/name_and_configure.png)

## トレーニング実行の作成

1. Adobe Experience Platform で、左側のナビゲーション列にある「**[!UICONTROL モデル]**」リンクをクリックして、既存のすべてのモデルを一覧表示します。訓練されるモデルの名前を探し、クリックします。
   ![](../images/models-recipes/train-evaluate-ui/models_browse.png)

2. 既存のトレーニング実行と現在のトレーニングステータスが表示されます。For Models created using the [!DNL Data Science Workspace] user interface, a training run is automatically generated and executed using the default configurations and input training dataset.
   ![](../images/models-recipes/train-evaluate-ui/model_overview.png)

3. 新しいトレーニングを作成するには、**[!UICONTROL モデルの概要]**ページの右上近くにある「トレーニング」をクリックします。
   ![](../images/models-recipes/train-evaluate-ui/training_input.png)

4. トレーニング実行のトレーニング入力データセットを選択し、「**[!UICONTROL 次へ]**」をクリックします。
   ![](../images/models-recipes/train-evaluate-ui/training_configuration.png)

5. モデルの作成時に提供されたデフォルトの設定が表示されるので、値をダブルクリックして変更および修正します。「**[!UICONTROL 完了]**」をクリックして、トレーニング実行を作成し、実行します。
   >[!NOTE]設定は、意図されたレシピに固有です。例えば、「小売販売レシピ」の設定は、「製品レコメンデーションレシピ」に対しては機能しません。小売販売レシピ設定のリストについては「[リファレンス](#reference)」の節を照してください。

   ![](../images/models-recipes/train-evaluate-ui/training_configuration.png)

## モデルの評価

1. Adobe Experience Platform で、左側のナビゲーション列にある「**[!UICONTROL モデル]**」リンクをクリックして、既存のすべてのモデルを一覧表示します。評価するモデルの名前を探し、クリックします。
   ![](../images/models-recipes/train-evaluate-ui/models_browse.png)

2. 既存のトレーニング実行と現在のトレーニングステータスが表示されます。複数の完了したトレーニング実行がある場合、評価指標は、モデル評価チャートでさまざまなトレーニング実行間で比較できます。グラフの上にあるドロップダウンリストを使用して評価指標を選択します。

   絶対百分率誤差（MAPE）指標は、精度をエラーに対する割合で表します。これは、パフォーマンスが最も高い実験を特定するために使用されます。MAPE が低いほど、より良い結果が得られます。

   ![](../images/models-recipes/train-evaluate-ui/complete_training_run.png)

   「精度」指標は、*取得された*インスタンスの合計と比較した、関連するインスタンスの割合を示します。精度は、ランダムに選択した結果が正しい確率と見なすことができます。
   ![](../images/models-recipes/train-evaluate-ui/multiple_training_runs.png)

   特定のトレーニングの実行をクリックして、その実行の詳細を表示します。これは、実行が完了する前でも実行できます。実行の詳細ページでは、トレーニングの実行に固有の他の評価指標、設定パラメーターおよびビジュアライゼーションを確認できます。また、実行の詳細を確認するアクティビティログをダウンロードすることもできます。ログは、失敗した実行で何が起きたかを確認するのに特に役立ちます。
   ![](../images/models-recipes/train-evaluate-ui/activity_logs.png)

3. ハイパーパラメーターはトレーニングできず、異なる組み合わせのハイパーパラメーターをテストすることでモデルを最適化する必要があります。最適化されたモデルに到達するまで、このモデルのトレーニングと評価のプロセスを繰り返します。

## 次の手順

This tutorial walked you through creating, training, and evaluating a Model in [!DNL Data Science Workspace]. 最適モデルに到達したら、「[UI でのモデルのスコア付け](./score-model-ui.md)」チュートリアルに従って、訓練済みモデルを使用して洞察を生成できます。

## リファレンス {#reference}

### 小売販売レシピの設定

ハイパーパラメーターは、モデルのトレーニング動作を決定します。ハイパーパラメーターを修正すると、モデルの精度と精度に影響を与えます。

| Hyperparameter | 説明 | 推奨範囲 |
--- | --- | ---
| learning_rate | 学習率は、learning_rate によって各ツリーの貢献度を減らします。Learning_rate と n_estimators の間にトレードオフがあります。 | 0.1 | [2 - 10] /推定量数 |
| n_estimators | 実行するブースティングステージの数。勾配ブースティングは、過学習に対してかなり強力なので、通常、大きな数値を指定するとパフォーマンスが向上します。 | 100 | 100 ～ 1000 |
| max_depth | 個々の回帰予測の最大深さ。最大の深さは、ツリー内のノード数を制限します。最高のパフォーマンスを得るには、このパラメーターを調整します。最適な値は、入力変数の操作に依存します。 | 3 | 4 ～ 10 |

他のパラメーターは、モデルの技術的特性を決定します。

| パラメーターキー | タイプ | 説明 |
| ----- | ----- | ----- |
| `ACP_DSW_INPUT_FEATURES` | 文字列 | コンマ区切りの入力スキーマ属性のリスト |
| `ACP_DSW_TARGET_FEATURES` | 文字列 | コンマ区切りの出力スキーマ属性のリスト |
| `ACP_DSW_FEATURE_UPDATE_SUPPORT` | Boolean | 入出力機能が変更可能かどうかを特定します。 |
| `tenantId` | 文字列 | この ID は、作成するリソースの名前空間が適切に設定され、IMS 組織内に含まれるようにします。テナント ID を検索するには、[こちらの手順](../../xdm/api/getting-started.md#know-your-tenant_id)に従います。 |
| `ACP_DSW_TRAINING_XDM_SCHEMA` | 文字列 | モデルのトレーニングに使用する入力スキーマ。 |
| `evaluation.labelColumn` | 文字列 | 評価のビジュアライゼーションの列ラベル |
| `evaluation.metrics` | 文字列 | モデルの評価に使用される評価指標のカンマ区切りのリスト |
| `ACP_DSW_SCORING_RESULTS_XDM_SCHEMA` | 文字列 | モデルのスコアリングに使用される出力スキーマ。 |