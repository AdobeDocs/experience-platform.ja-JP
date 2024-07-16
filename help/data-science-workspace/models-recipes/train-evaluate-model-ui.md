---
keywords: Experience Platform；トレーニングと評価；Data Science Workspace；人気のトピック；モデルの作成；トレーニング実行の作成
solution: Experience Platform
title: Data Science Workspace UI でのモデルのトレーニングと評価
type: Tutorial
description: Adobe Experience Platform Data Science Workspace　では、モデルの意図に適した既存のレシピを組み込むことで、機械学習モデルが作成されます。次に、モデルに関連するハイパーパラメーターを微調整することで、モデルの動作効率と有効性を最適化するようにトレーニングおよび評価します。レシピは再利用可能で、複数のモデルを作成し、単一のレシピで特定の目的に合わせてカスタマイズできます。
exl-id: 6f674cfa-c123-46a3-80e2-9342fe687976
source-git-commit: 81f48de908b274d836f551bec5693de13c5edaf1
workflow-type: tm+mt
source-wordcount: '1084'
ht-degree: 55%

---

# Data Science Workspace UI でのモデルのトレーニングと評価

Adobe Experience Platform Data Science Workspace　では、モデルの意図に適した既存のレシピを組み込むことで、機械学習モデルが作成されます。次に、モデルに関連するハイパーパラメーターを微調整することで、モデルの動作効率と有効性を最適化するようにトレーニングおよび評価します。レシピは再利用可能で、複数のモデルを作成し、単一のレシピで特定の目的に合わせてカスタマイズできます。

このチュートリアルでは、モデルの作成、トレーニング、評価の手順について説明します。

## はじめに

このチュートリアルを完了するには、[!DNL Experience Platform] へのアクセス権が必要です。 [!DNL Experience Platform] の組織にアクセスする権限がない場合は、続行する前にシステム管理者に問い合わせてください。

このチュートリアルでは、既存のレシピが必要です。レシピがない場合は、先に進む前に、「[UI へのパッケージレシピの読み込み](./import-packaged-recipe-ui.md)」チュートリアルに従ってください。

## モデルの作成

Experience Platformで、左側のナビゲーションにある「**[!UICONTROL モデル]**」タブを選択し、「参照」タブを選択して既存のモデルを表示します。 ページ右上付近の **[!UICONTROL モデルを作成]** を選択して、モデル作成プロセスを開始します。

![](../images/models-recipes/train-evaluate-ui/models_browse.png)

既存のレシピのリストを参照し、モデルの作成に使用するレシピを見つけて選択し、「**[!UICONTROL 次へ]**」を選択します。
![](../images/models-recipes/train-evaluate-ui/select_recipe.png)

適切な入力データセットを選択して、「**[!UICONTROL 次へ]**」を選択します。 これにより、モデルのデフォルトの入力トレーニングデータセットが設定されます。
![](../images/models-recipes/train-evaluate-ui/select_dataset.png)

モデルの名前を指定し、デフォルトのモデル設定を確認します。レシピの作成時にデフォルト設定が適用され、値をダブルクリックして設定値を確認および変更しました。

設定の新しいセットを指定するには、「**[!UICONTROL 新しい設定をアップロード]** を選択し、モデル設定を含む JSON ファイルをブラウザーウィンドウにドラッグします。 「**[!UICONTROL 終了]**」を選択して、モデルを作成します。

>[!NOTE]
>
>コンフィギュレーションは、意図したレシピに固有で固有です。つまり、小売販売レシピのコンフィギュレーションは、商品のRecommendationsレシピでは機能しません。 小売販売レシピ設定のリストについては「[リファレンス](#reference)」の節を照してください。

![](../images/models-recipes/train-evaluate-ui/name_and_configure.png)

## トレーニング実行の作成

Experience Platformで、左側のナビゲーションにある「**[!UICONTROL モデル]**」タブを選択し、「参照」タブを選択して既存のモデルを表示します。 トレーニングするモデルの名前に付けられたハイパーリンクを検索して選択します。

![](../images/models-recipes/train-evaluate-ui/model-hyperlink.png)

既存のトレーニング実行と現在のトレーニングステータスが表示されます。[!DNL Data Science Workspace] ユーザーインターフェイスを使用して作成されたモデルの場合、トレーニング実行は、デフォルト設定と入力トレーニングデータセットを使用して、自動的に生成および実行されます。

モデルの概要ページの右上付近にある **[!UICONTROL トレーニング]** を選択して、新しいトレーニング実行を作成します。

![](../images/models-recipes/train-evaluate-ui/model_overview.png)

トレーニング実行のトレーニング入力データセットを選択し、「**[!UICONTROL 次へ]**」を選択します。

![](../images/models-recipes/train-evaluate-ui/training_input.png)

モデルの作成時に提供されたデフォルトの設定が表示されるので、値をダブルクリックして変更および修正します。**[!UICONTROL 終了]** を選択し、トレーニング実行を作成して実行します。

>[!NOTE]
>
>コンフィギュレーションは、意図したレシピに固有で固有です。つまり、小売販売レシピのコンフィギュレーションは、商品のRecommendationsレシピでは機能しません。 小売販売レシピ設定のリストについては「[リファレンス](#reference)」の節を照してください。

![](../images/models-recipes/train-evaluate-ui/training_configuration.png)


## モデルの評価

Experience Platformで、左側のナビゲーションにある「**[!UICONTROL モデル]**」タブを選択し、「参照」タブを選択して既存のモデルを表示します。 評価するモデルの名前にアタッチされているハイパーリンクを探して選択します。

![ モデルを選択 ](../images/models-recipes/train-evaluate-ui/model-hyperlink.png)

既存のトレーニング実行と現在のトレーニングステータスが表示されます。完了したトレーニング実行が複数ある場合、モデル評価グラフで、様々なトレーニング実行にわたって評価指標を比較できます。 グラフ上のドロップダウンリストを使用して、評価指標を選択します。

絶対百分率誤差（MAPE）指標は、精度をエラーに対する割合で表します。これは、パフォーマンスが最も高い実験を特定するために使用されます。MAPE が低いほど、より良い結果が得られます。

![ トレーニング実行の概要 ](../images/models-recipes/train-evaluate-ui/complete_training_run.png)

「精度」指標は、*取得された*&#x200B;インスタンスの合計と比較した、関連するインスタンスの割合を示します。精度は、ランダムに選択した結果が正しい確率と見なすことができます。

![ 複数の実行の実行 ](../images/models-recipes/train-evaluate-ui/multiple_training_runs.png)

特定のトレーニング実行を選択すると、評価ページを開いて、その実行の詳細が表示されます。 これは、実行が完了する前でも実行できます。評価ページでは、トレーニング実行に固有の、その他の評価指標、設定パラメーターおよびビジュアライゼーションを表示できます。

![ ログをプレビュー ](../images/models-recipes/train-evaluate-ui/evaluate_training.png)

また、実行の詳細を確認するアクティビティログをダウンロードすることもできます。ログは、失敗した実行で何が起きたかを確認するのに特に役立ちます。

![ アクティビティログ ](../images/models-recipes/train-evaluate-ui/activity_logs.png)

ハイパーパラメーターはトレーニングできず、異なる組み合わせのハイパーパラメーターをテストすることでモデルを最適化する必要があります。最適化されたモデルに到達するまで、このモデルのトレーニングと評価のプロセスを繰り返します。

## 次の手順

このチュートリアルでは、[!DNL Data Science Workspace] でのモデルの作成、トレーニング、評価について順を追って説明しました。 最適モデルに到達したら、「[UI でのモデルのスコア付け](./score-model-ui.md)」チュートリアルに従って、訓練済みモデルを使用して洞察を生成できます。

## リファレンス {#reference}

### 小売販売レシピの設定

ハイパーパラメーターは、モデルのトレーニング動作を決定します。ハイパーパラメーターを修正すると、モデルの精度と精度に影響を与えます。

| Hyperparameter | 説明 | 推奨範囲 |
| --- | --- | --- |
| learning_rate | 学習率は、learning_rate によって各ツリーの貢献度を減らします。Learning_rate と n_estimators の間にトレードオフがあります。 | 0.1 |
| n_estimators | 実行するブースティングステージの数。勾配ブースティングは、過学習に対してかなり強力なので、通常、大きな数値を指定するとパフォーマンスが向上します。 | 100 |
| max_depth | 個々の回帰予測の最大深さ。最大の深さは、ツリー内のノード数を制限します。最高のパフォーマンスを得るには、このパラメーターを調整します。最適な値は、入力変数の操作に依存します。 | 3 |

他のパラメーターは、モデルの技術的特性を決定します。

| パラメーターキー | タイプ | 説明 |
| ----- | ----- | ----- |
| `ACP_DSW_INPUT_FEATURES` | 文字列 | コンマ区切りの入力スキーマ属性のリスト |
| `ACP_DSW_TARGET_FEATURES` | 文字列 | コンマ区切りの出力スキーマ属性のリスト |
| `ACP_DSW_FEATURE_UPDATE_SUPPORT` | Boolean | 入出力機能が変更可能かどうかを特定します。 |
| `tenantId` | 文字列 | この ID により、作成したリソースの名前空間が適切に設定され、組織内に含まれるようになります。 テナント ID を検索するには、[こちらの手順](../../xdm/api/getting-started.md#know-your-tenant_id)に従います。 |
| `ACP_DSW_TRAINING_XDM_SCHEMA` | 文字列 | モデルのトレーニングに使用する入力スキーマ。 |
| `evaluation.labelColumn` | 文字列 | 評価のビジュアライゼーションの列ラベル |
| `evaluation.metrics` | 文字列 | モデルの評価に使用される評価指標のカンマ区切りのリスト |
| `ACP_DSW_SCORING_RESULTS_XDM_SCHEMA` | 文字列 | モデルのスコアリングに使用される出力スキーマ。 |
