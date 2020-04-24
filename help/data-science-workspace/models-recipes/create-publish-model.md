---
keywords: Experience Platform;machine learning model;Data Science Workspace;popular topics
solution: Experience Platform
title: 機械学習モデルのチュートリアルの作成と公開
topic: Tutorial
translation-type: tm+mt
source-git-commit: 19823c7cf0459e045366f0baae2bd8a98416154c

---


# 機械学習モデルのチュートリアルの作成と公開

![](../images/models-recipes/model-walkthrough/objective.png)

オンラインの小売サイトを所有しているフリをします。 顧客が小売Webサイトで買い物をする際に、ビジネスオファーの他の様々な製品を公開するために、パーソナライズされた製品レコメンデーションを提示したいとします。 Webサイトの存在期間全体にわたって、顧客データを継続的に収集し、このデータを使用してパーソナライズされた商品レコメンデーションを生成したいと考えています。

Adobe Experience Platform Data Science Workspaceは、事前に作成された製品推奨レシピを使用して目標を達成する手 [段を提供します](../pre-built-recipes/product-recommendations.md)。 このチュートリアルに従って、小売データにアクセスし、理解し、機械学習モデルを作成し、最適化し、Data Science Workspaceでインサイトを生成する方法を確認します。

このチュートリアルは、Data Science Workspaceのワークフローを反映し、機械学習モデルを作成する次の手順をカバーしています。

1. [データの準備](#prepare-your-data)
2. [モデルのオーサリング](#author-your-model)
3. [モデルのトレーニングと評価](#train-and-evaluate-your-model)
4. [モデルの操作](#operationalize-your-model)

## はじめに

このチュートリアルを開始する前に、次の前提条件が必要です。

* Adobe Experience Platformへのアクセス。 Experience PlatformのIMS組織にアクセスできない場合は、先に進む前にシステム管理者にお問い合わせください。

* 有効化アセット 次のアイテムをプロビジョニングするには、アカウント担当者にお問い合わせください。
   * Recommendationsのレシピ
   * Recommendations入力データセット
   * Recommendations入力スキーマ
   * Recommendations出力データセット
   * Recommendationsの出力スキーマ
   * ゴールデンデータセットpostValues
   * ゴールデンデータセットスキーマ

* 必要な3つのJupyterノートブックファイルを <a href="https://github.com/adobe/experience-platform-dsw-reference/tree/master/Summit/2019/resources/Notebooks-Thurs" target="_blank">Adobe Public Gitリポジトリからダウンロードします</a>。これらのファイルは、Data Science WorkspaceのJupyterLabワークフローのデモに使用されます。

* このチュートリアルで使用する次の主要概念に関する実用的な理解
   * [Experience Data Model](../../xdm/home.md):Customer Experience Management用の標準スキーマ(プロファイルやExperienceEventなど)を定義するためにアドビが導く標準化の取り組み。
   * データセット：実際のストレージのデータと管理の構成。 XDMインスタンスの物理的なインスタ [ンススキーマ](../../xdm/schema/field-dictionary.md)。
   * バッチ：データセットは、バッチで構成されます。 バッチとは、一定期間に収集され、1つの単位として一緒に処理される一連のデータです。
   * JupyterLab:JupyterLabは [](https://blog.jupyter.org/jupyterlab-is-ready-for-users-5a6f039b8906) 、Project JupyterのオープンソースWebベースのインターフェイスで、Experience Platformに緊密に統合されています。

## データの準備 {#prepare-your-data}

顧客に対してパーソナライズされた商品レコメンデーションを作成する機械学習モデルを作成するには、Webサイトでの以前の顧客の購入を分析する必要があります。 この節では、このデータがAdobe Analyticsを通じてPlatformに取り込まれる方法と、そのデータが機能データセットに変換され、機械学習モデルで使用される方法について説明します。

### データを調べ、スキーマ

1. [Adobe Experience Platformにログインし](https://platform.adobe.com/) 、「 **Datasets** 」をクリックして既存のデータセットをすべてリストし、調査するデータセットを選択します。 この場合、Analyticsデータセット **のゴールデンデータセットpostValues**。
   ![](../images/models-recipes/model-walkthrough/datasets_110.png)
2. 右上近くにある **プレビュー** ・データセットを選択し、サンプル・レコードを確認して、「閉じる」をクリッ **クします**。
   ![](../images/models-recipes/model-walkthrough/golden_data_set_110.png)
3. 右側のレールの「スキーマ」の下のリンクを選択して、データセットのスキーマを表示し、データセットの詳細ページに戻ります。」
   ![](../images/models-recipes/model-walkthrough/golden_schema_110.png)

その他のデータセットは、プレビュー用にバッチで事前設定されています。 これらのデータセットを表示するには、上記の手順を繰り返します。

| データセット名 | スキーマ | 説明 |
| ----- | ----- | ----- |
| ゴールデンデータセットpostValues | ゴールデンデータセットスキーマ | WebサイトのAnalyticsソースデータ |
| Recommendations入力データセット | Recommendations入力スキーマ | Analyticsデータは、機能パイプラインを使用してトレーニングデータセットに変換されます。 このデータは、Product Recommendationsの機械学習モデルのトレーニングに使用されます。 `itemid` そして、 `userid` その顧客が購入した製品に対応しています。 |
| Recommendations出力データセット | Recommendationsの出力スキーマ | スコアリング結果が保存されるデータセットには、各顧客のレコメンデーション商品のリストが含まれます。 |

## モデルのオーサリング {#author-your-model}

Data Science Workspaceのライフサイクルの2番目の要素は、レシピとモデルの作成です。 商品レコメンデーションレシピは、過去の購入データと機械学習を利用して、商品レコメンデーションをスケールで生成するように設計されています。

レシピは、特定の問題を解決するために設計された機械学習アルゴリズムとロジックを含む、モデルの基盤です。 さらに重要な点は、レシピを使用すると、組織全体の機械学習を民主化でき、他のユーザーがコードを書かずに様々な用途のモデルにアクセスできるようになることです。

### 商品レコメンデーションのレシピの参照

1. Adobe Experience Platformで、左のナビゲーション列から **Models** （モデル）に移動し、上部の「 **Recipes** （レシピ）」をクリックして、組織で使用可能なレシピのリストを表示します。
   ![](../images/models-recipes/model-walkthrough/browse_recipes.png)
2. 提供された **Recommendations Recipeを探し、名前をクリ** ックして開きます。
   ![](../images/models-recipes/model-walkthrough/recommendations_recipe_110.png)
3. 右側のレールで、「 **Recommendations入力スキーマ** 」をクリックして、レシピを実行するスキーマを表示します。 スキーマフィールド **itemId** と **userIdは** 、特定の時間(**timestampId)にその顧客が購入した製品(** interactionType ****)に対応します。 同じ手順に従って、 **Recommendationsの出力スキーマ**。
   ![](../images/models-recipes/model-walkthrough/preview_schemas.png)

これで、商品レコメンデーションレシピで必要な入出力スキーマを確認しました。 次のセクションに進み、商品レコメンデーションモデルの作成、トレーニング、評価方法を見つけることができます。

## モデルのトレーニングと評価 {#train-and-evaluate-your-model}

データの準備が完了し、レシピを使用する準備が整ったら、機械学習モデルを作成、トレーニング、評価できます。

### モデルの作成

モデルはレシピのインスタンスで、データをスケールでトレーニングし、スコアを付けることができます。

1. Adobe Experience Platformで、左側のナビゲーション列の **Models** （モデル）に移動し、ページの上部にある「 **Recipes** （レシピ）」をクリックして、組織で使用可能なすべてのレシピのリストを表示します。
   ![](../images/models-recipes/model-walkthrough/browse_recipes.png)
2. 提供された **Recommendationsレシピを探して** 、名前をクリックし、レシピの概要ページを入力して開きます。 「モデ **ルを作成」をクリックします** （既存のモデルがない場合）。または、「レシピの概要」ページの右上にある「モデルを作成」をクリックします。
   ![](../images/models-recipes/model-walkthrough/recommendations_recipe_110.png)
3. トレーニングに使用できる入力データセットのリストが表示されます。「 **Recommendations入力データセット** 」を選択し、「次へ」をクリ **ックします**。
   ![](../images/models-recipes/model-walkthrough/select_dataset.png)
4. モデルの名前を指定します（例：「Product Recommendations Model」）。 モデルのデフォルトのトレーニングおよびスコアリング動作の設定を含む、モデルで使用可能な設定が表示されます。 これらの設定は組織に固有なので、変更は必要ありません。 設定を確認し、「完了」をクリ **ックしま**す。
   ![](../images/models-recipes/model-walkthrough/configure_model.png)
5. モデルが作成され、新しく生成されたトレーニング *実行内に* 、モデルの概要ページが表示されます。 トレーニングの実行は、モデルの作成時にデフォルトで生成されます。
   ![](../images/models-recipes/model-walkthrough/model_post_creation.png)

トレーニングの実行が完了するまで待つか、次のセクションで新しいトレーニングの実行の作成を続行するかを選択できます。

### カスタムハイパーパラメータを使用してモデルをトレーニングする

1. [モデルの *概要* ]ページで、右上 **部の** [トレーニング]をクリックし、新しいトレーニングを作成します。 モデルの作成時に使用したのと同じ入力データセットを選択し、「次へ」をクリ **ックしま**す。
   ![](../images/models-recipes/model-walkthrough/training_select_dataset.png)
2. 設定ペ *ージ* が表示されます。 ここで、トレーニング実行の **num_recommendations** （ハイパーパラメーター）値を設定できます。 トレーニングを受け、最適化されたモデルは、トレーニングの実行結果に基づいて、パフォーマンスが最も高いハイパーパラメータを利用します。

   ハイパーパラメータは学習できないので、トレーニングを実行する前に割り当てる必要があります。 ハイパーパラメータを調整すると、トレーニングモデルの精度が変わる場合があります。 モデルの最適化は反復的なプロセスなので、満足のいく評価を得る前に複数のトレーニングの実行が必要になる場合があります。

   >[!TIP] num_recommendations **を** 10に設定します。

   ![](../images/models-recipes/model-walkthrough/configure_hyperparameter.png)
3. 新しいトレーニングの実行が完了すると、モデル評価グラフに追加のデータポイントが表示されます。この処理には数分かかる場合があります。
   ![](../images/models-recipes/model-walkthrough/post_training_run.png)

### モデルの評価

トレーニングの実行が完了するたびに、結果の評価指標を表示して、モデルのパフォーマンスを判断できます。

1. トレーニングの実行をクリックして、完了した各トレーニングの評価指標（精度と再現）を確認します。
2. 各評価指標に対して提供される情報を調べます。 これらの指標が高いほど、モデルのパフォーマンスが向上します。
   ![](../images/models-recipes/model-walkthrough/evaluation_metrics.png)
3. 各トレーニングの実行に使用されるスキーマセット、データセット、設定パラメーターは、右側のレールで確認できます。
4. 「モデル」ページに戻り、評価指標を観察して、パフォーマンスが最も高いトレーニングを特定します。

## モデルの操作 {#operationalize-your-model}

Data Scienceワークフローの最後の手順は、データストアからの洞察をスコア化し、利用するためにモデルを操作することです。

### スコアを付けてインサイトを生成する

1. 商品レコメンデーションの「モ *デルの概要* 」ページで、最もパフォーマンスの高いトレーニング実行の名前をクリックし、リコールと精度の値を最も高くします。
2. トレーニングの実行の詳細ページの右上にある「スコア」をクリック **します**。
3. Recommendations入力データセ **ットをスコアリング入力データセットとして選択します** 。これは、モデルの作成時に使用し、そのトレーニング実行時に実行したデータセットと同じです。 Then, click **Next**.
   ![](../images/models-recipes/model-walkthrough/scoring_input.png)
4. スコアリング出 **力データセットとして** 、Recommendations出力データセットを選択します。 スコアリング結果は、このデータセットにバッチとして保存されます。
   ![](../images/models-recipes/model-walkthrough/scoring_output.png)
5. スコア設定を確認します。 これらのパラメーターには、前に選択した入力および出力データセットと、適切なスキーマが含まれます。 「完了 **** 」をクリックして、スコアの実行を開始します。 実行が完了するまで数分かかる場合があります。
   ![](../images/models-recipes/model-walkthrough/scoring_configure.png)


### 表示が洞察を得た

スコアリングの実行が正常に完了すると、生成された結果とインサイトの表示をプレビューできます。

1. スコアリングの実行ページで、完了したスコアリングの実行をクリックし、右側のレールの「 **プレビュースコアリング結果データセット** 」をクリックします。
   ![](../images/models-recipes/model-walkthrough/score_complete.png)
2. プレビューの表では、各行に特定の顧客の商品レコメンデーションが含まれ、それぞれrecommendationsと **userId** というラベルが付 **けられます** 。 サンプルスクリーンショットで **num_recommendations** Hyperparameterが10に設定されているので、レコメンデーションの各行には、番号記号(#)で区切られた最大10個の製品IDを含めることができます。
   ![](../images/models-recipes/model-walkthrough/preview_score_results.png)

製品のレコメンデーションが正常に生成されました。

このチュートリアルでは、Data Science Workspaceのワークフローを紹介し、機械学習を通じて生の未処理のデータを有用な情報に変換する方法を説明しました。 Data Science Workspaceの使用方法の詳細については、次のガイドで小売売上のスキーマとデ [ータセットの作成に進みます](./create-retails-sales-dataset.md)。