---
keywords: Experience Platform;train and evaluate;Data Science Workspace;popular topics
solution: Experience Platform
title: モデル(UI)のトレーニングと評価
topic: Tutorial
translation-type: tm+mt
source-git-commit: e08460bc76d79920bbc12c7665a1416d69993f34

---


# モデル(UI)のトレーニングと評価

Adobe Experience Platform Data Science Workspaceでは、機械学習モデルは、モデルの意図に適した既存のレシピを組み込むことで作成されます。 次に、モデルのトレーニングと評価を行い、関連するハイパーパラメーターを微調整して、その動作効率と有効性を最適化します。 レシピは再利用可能です。つまり、複数のモデルを作成し、1つのレシピで特定の目的に合わせてカスタマイズできます。

このチュートリアルでは、モデルの作成、トレーニング、評価の手順について説明します。

## はじめに

このチュートリアルを完了するには、エクスペリエンスプラットフォームにアクセスできる必要があります。 Experience PlatformのIMS組織にアクセスできない場合は、先に進む前に、システム管理者にお問い合わせください。

このチュートリアルでは、既存のレシピが必要です。 レシピがない場合は、先に進む前に、UI [チュートリアルの「パッケージ化されたレシピを](./import-packaged-recipe-ui.md) 読み込む」に従ってください。

## モデルの作成

1. Adobe Experience Platformで、左側のナビゲーション列にある **[!UICONTROL Models]** リンクをクリックして、既存のすべてのモデルをリストします。 ページの右上 **[!UICONTROL Create Model]** 近くにあるをクリックして、モデル作成プロセスを開始します。
   ![](../images/models-recipes/train-evaluate-ui/models_browse.png)

2. 既存のレシピのリストを参照し、モデルの作成に使用するレシピを探して選択し、をクリックし **[!UICONTROL Next]**ます。
   ![](../images/models-recipes/train-evaluate-ui/select_recipe.png)

3. 適切な入力データセットを選択し、をクリックし **[!UICONTROL Next]**ます。 これにより、モデルのデフォルトの入力トレーニングデータセットが設定されます。
   ![](../images/models-recipes/train-evaluate-ui/select_dataset.png)

4. モデルの名前を指定し、デフォルトのモデル設定を確認します。 レシピの作成時にデフォルトの設定が適用され、重複が値をクリックして、設定値を確認および変更しました。 新しい設定のセットを提供するには、をクリックし **[!UICONTROL Upload New Config]** て、モデル設定を含むJSONファイルをブラウザーウィンドウにドラッグします。 をクリック **[!UICONTROL Finish]** して、モデルを作成します。
   >[!NOTE]設定は、意図したレシピに固有であり、小売売上高レシピの設定は、商品レコメンデーションレシピでは機能しません。 小売売上手法設定のリストについては、 [リファレンス](#reference) 「」の項を参照してください。

   ![](../images/models-recipes/train-evaluate-ui/name_and_configure.png)

## トレーニング実行の作成

1. Adobe Experience Platformで、左側のナビゲーション列にある **[!UICONTROL Models]** リンクをクリックして、既存のすべてのモデルをリストします。 トレーニングを受けるモデルの名前を見つけ、クリックします。
   ![](../images/models-recipes/train-evaluate-ui/models_browse.png)

2. 既存のトレーニングの実行が、現在のトレーニングのステータスですべて表示されます。 Data Science Workspaceユーザーインターフェイスを使用して作成されたモデルの場合、トレーニング実行は自動的に生成され、デフォルトの設定と入力トレーニングデータセットを使用して実行されます。
   ![](../images/models-recipes/train-evaluate-ui/model_overview.png)

3. 「モデルの概要」ページの右上 **[!UICONTROL Train]** 近くにあるをクリックして、新しいトレーニングを作成します。
   ![](../images/models-recipes/train-evaluate-ui/training_input.png)

4. トレーニング実行のトレーニング入力データセットを選択し、をクリックし **[!UICONTROL Next]**ます。
   ![](../images/models-recipes/train-evaluate-ui/training_configuration.png)

5. モデルの作成時に提供されるデフォルトの設定が表示され、値を重複クリックして、それに応じて変更および変更します。 をクリック **[!UICONTROL Finish]** して、トレーニングの実行を作成および実行します。
   >[!NOTE]設定は、意図したレシピに固有であり、小売売上高レシピの設定は、商品レコメンデーションレシピでは機能しません。 小売売上手法設定のリストについては、 [リファレンス](#reference) 「」の項を参照してください。

   ![](../images/models-recipes/train-evaluate-ui/training_configuration.png)

## モデルの評価

1. Adobe Experience Platformで、左側のナビゲーション列にある **[!UICONTROL Models]** リンクをクリックして、既存のすべてのモデルをリストします。 評価するモデルの名前を探してクリックします。
   ![](../images/models-recipes/train-evaluate-ui/models_browse.png)

2. 既存のトレーニングの実行が、現在のトレーニングのステータスですべて表示されます。 複数のトレーニングの実行が完了した場合、モデル評価グラフの異なるトレーニングの実行間で評価指標を比較できます。グラフの上にあるドロップダウンリストを使用して、評価指標を選択します。

   平均絶対誤差率(MAPE)指標は、精度をエラーに対する割合で表します。 これは、パフォーマンスの最も高いテストを特定するために使用します。 MAPEが低いほど良い。

   ![](../images/models-recipes/train-evaluate-ui/complete_training_run.png)

   「精度」指標は、 *取得されたインスタンスの合計と比較した、関連するインスタンスの割合を示します* 。 精度は、ランダムに選択した結果が正しい確率と見なすことができます。
   ![](../images/models-recipes/train-evaluate-ui/multiple_training_runs.png)

   特定のトレーニングの実行をクリックして、その実行の詳細を表示します。 これは、実行が完了する前でも実行できます。 実行の詳細ページでは、トレーニングの実行に固有の他の評価指標、設定パラメーターおよびビジュアライゼーションを確認できます。 アクティビティログをダウンロードして実行の詳細を確認することもできます。 ログは、失敗した実行で何が起きたかを確認するのに特に役立ちます。
   ![](../images/models-recipes/train-evaluate-ui/activity_logs.png)

3. ハイパーパラメータはトレーニングできず、異なる組み合わせのハイパーパラメータをテストすることでモデルを最適化する必要があります。 最適化されたモデルに到達するまで、このモデルのトレーニングと評価のプロセスを繰り返します。

## 次の手順

このチュートリアルでは、Data Science Workspaceでのモデルの作成、トレーニング、評価に関する手順を説明しました。 最適化されたモデルに到達したら、UI [チュートリアルの「モデルの](./score-model-ui.md) スコア」に従って、トレーニングを受けたモデルを使用してインサイトを生成できます。

## リファレンス {#reference}

### 小売販売レシピの設定

ハイパーパラメータは、モデルのトレーニング動作を決定し、ハイパーパラメータを修正すると、モデルの精度と精度に影響を与えます。

| Hyperparameter | 説明 | 推奨範囲 |
--- | --- | ---
| learning_rate | 学習率は、learning_rateによって各ツリーの貢献度を縮小します。 learning_rateとn_estimatorsの間にトレードオフがあります。 | 0.1 | [2 - 10] /評価者数 |
| n_estimators | 実行する昇格ステージの数。 グラデーションのブーストは、過剰な調整に対してかなり堅牢なので、通常、大きな数値を指定するとパフォーマンスが向上します。 | 100 | 100 - 1000 |
| max_depth | 個々の回帰予測の最大深さ。 深さの最大値は、ツリー内のノード数を制限します。 最高のパフォーマンスを得るために、このパラメータを調整します。 最善の値は、入力変数の操作に依存します。 | 3 | 4 - 10 |

その他のパラメーターは、モデルの技術的特性を決定します。

| パラメータキー | タイプ | 説明 |
| ----- | ----- | ----- |
| `ACP_DSW_INPUT_FEATURES` | 文字列 | 入力スキーマ属性をコンマで区切ったリスト。 |
| `ACP_DSW_TARGET_FEATURES` | 文字列 | カンマ区切りの出力スキーマ属性のリスト。 |
| `ACP_DSW_FEATURE_UPDATE_SUPPORT` | Boolean | 入出力フィーチャを変更可能にするかどうかを指定します |
| `tenantId` | 文字列 | このIDを使用すると、作成するリソースの名前が適切に付けられ、IMS組織内に含まれるようになります。 [テナントIDを検索するには](../../xdm/api/getting-started.md#know-your-tenant_id) 、ここの手順に従います。 |
| `ACP_DSW_TRAINING_XDM_SCHEMA` | 文字列 | モデルのトレーニングに使用する入力スキーマ。 |
| `evaluation.labelColumn` | 文字列 | 評価のビジュアライゼーションの列ラベル。 |
| `evaluation.metrics` | 文字列 | モデルの評価に使用する評価指標のコンマ区切りリスト。 |
| `ACP_DSW_SCORING_RESULTS_XDM_SCHEMA` | 文字列 | モデルのスコアリングに使用する出力スキーマ。 |