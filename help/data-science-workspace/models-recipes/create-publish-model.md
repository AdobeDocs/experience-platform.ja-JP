---
keywords: Experience Platform;machine learning model;Data Science Workspace;popular topics
solution: Experience Platform
title: 機械学習モデルのチュートリアルを作成して公開する
topic: Tutorial
translation-type: tm+mt
source-git-commit: e08460bc76d79920bbc12c7665a1416d69993f34

---


# 機械学習モデルのチュートリアルを作成して公開する

![](../images/models-recipes/model-walkthrough/objective.png)

オンラインの小売Webサイトを所有しているふりをします。 顧客が小売Webサイトで買い物をする際、ビジネスオファーに様々な他の商品を公開するために、パーソナライズされた商品のレコメンデーションを提示したいと考えます。 Webサイトが存在する間、顧客データを継続的に収集し、このデータを使用してパーソナライズされた商品レコメンデーションを生成したいと考えています。

Adobe Experience Platform Data Science Workspaceは、事前に組み込まれた [製品推奨レシピを使用して目標を達成する手段を提供します](../pre-built-recipes/product-recommendations.md)。 このチュートリアルに従って、小売データにアクセスして理解し、機械学習モデルを作成して最適化し、Data Science Workspaceでインサイトを生成する方法を確認します。

このチュートリアルは、Data Science Workspaceのワークフローを反映しており、機械学習モデルを作成するための次の手順をカバーしています。

1. [データの準備](#prepare-your-data)
2. [モデルの作成](#author-your-model)
3. [モデルのトレーニングと評価](#train-and-evaluate-your-model)
4. [モデルの操作](#operationalize-your-model)

## はじめに

このチュートリアルを開始する前に、次の前提条件を満たす必要があります。

* Adobe Experience Platformへのアクセス。 Experience PlatformのIMS組織にアクセスできない場合は、先に進む前に、システム管理者にお問い合わせください。

* 有効化アセット。 アカウント担当者に問い合わせて、次のアイテムをプロビジョニングしてください。
   * Recommendationsのレシピ
   * Recommendations入力データセット
   * レコメンデーション入力スキーマ
   * Recommendations出力データセット
   * Recommendationsの出力スキーマ
   * ゴールデンデータセットpostValues
   * ゴールデンデータセットスキーマ

* 必要な3つのJupyterノートブックファイルを <a href="https://github.com/adobe/experience-platform-dsw-reference/tree/master/Summit/2019/resources/Notebooks-Thurs" target="_blank">Adobe Public Gitリポジトリからダウンロードします</a>。これらは、Data Science WorkspaceでJupterLabワークフローを示すのに使用されます。

* このチュートリアルで使用する次の主要概念の実際の理解
   * [Experience Data Model](../../xdm/home.md): 標準化の取り組みは、Customer Experience Managementに対して、プロファイルやExperienceEventなどの標準スキーマを定義する際にアドビが主導します。
   * データセット： 実際のデータのストレージと管理の構成体。 [XDMスキーマの物理的にインスタンス化されたインスタンス](../../xdm/schema/field-dictionary.md)。
   * バッチ： データセットはバッチで構成されます。 バッチとは、ある期間に収集され、1つの単位として一緒に処理される一連のデータです。
   * JupterLab: [JupyterLab](https://blog.jupyter.org/jupyterlab-is-ready-for-users-5a6f039b8906) は、Project Jupyter向けのオープンソースのWebベースのインターフェイスで、Experience Platformに緊密に統合されています。

## データの準備 {#prepare-your-data}

機械学習モデルを作成して、顧客に対する商品の推奨事項をパーソナライズするには、Webサイトでの以前の顧客の購入を分析する必要があります。 この節では、このデータがAdobe Analyticsを通じてPlatformに取り込まれる方法、およびそのデータが機械学習モデルで使用される機能データセットにどのように変換されるかについて説明します。

### データを調べ、スキーマを理解する

1. [Adobe Experience Platform](https://platform.adobe.com/)**[!UICONTROL Datasets]** にログインし、をクリックして既存のデータセットをすべてリストし、調査するデータセットを選択します。 この場合、Analyticsデータセット **ゴールデンデータセットのpostValues**。
   ![](../images/models-recipes/model-walkthrough/datasets_110.png)
2. 右上 **[!UICONTROL Preview Dataset]** 付近にあるを選択し、サンプルレコードを確認してから、をクリックし **[!UICONTROL Close]**ます。
   ![](../images/models-recipes/model-walkthrough/golden_data_set_110.png)
3. 右側のパネルの「スキーマ」の下にあるリンクを選択して、データセットのスキーマを表示し、データセットの詳細ページに戻ります。」
   ![](../images/models-recipes/model-walkthrough/golden_schema_110.png)

その他のデータセットは、プレビュー用にバッチで事前設定されています。 上記の手順を繰り返すことで、これらのデータセットを表示できます。

| データセット名 | スキーマ | 説明 |
| ----- | ----- | ----- |
| ゴールデンデータセットpostValues | ゴールデンデータセットスキーマ | WebサイトからのAnalyticsソースデータ |
| Recommendations入力データセット | レコメンデーション入力スキーマ | Analyticsデータは、機能パイプラインを使用してトレーニングデータセットに変換されます。 このデータは、Product Recommendationsの機械学習モデルのトレーニングに使用されます。 `itemid` そして `userid` その顧客が購入した商品に対応する |
| Recommendations出力データセット | Recommendationsの出力スキーマ | スコアリング結果が格納されるデータセット。各顧客のレコメンデーション商品のリストが含まれます。 |

## モデルの作成 {#author-your-model}

Data Science Workspaceのライフサイクルの2番目の要素は、レシピとモデルの作成です。 商品レコメンデーションレシピは、過去の購入データや機械学習を利用して、商品レコメンデーションを規模の大きいもので生成するように設計されています。

レシピは、機械学習アルゴリズムと特定の問題を解決するためのロジックを含むので、モデルの基本となります。 さらに重要な点は、レシピを使用することで、組織全体の機械学習を民主化でき、他のユーザーがコードを記述することなく、さまざまな使用例に対応したモデルにアクセスできることです。

### 商品レコメンデーションレシピの参照

1. Adobe Experience Platformで左のナビゲーション列に移動し、上部 **[!UICONTROL Models]****[!UICONTROL Recipes]** のをクリックして、組織で使用可能なレシピのリストを表示します。
   ![](../images/models-recipes/model-walkthrough/browse_recipes.png)
2. 名前をクリックして、提供された **[!UICONTROL Recommendations Recipe]** ファイルを探して開きます。
   ![](../images/models-recipes/model-walkthrough/recommendations_recipe_110.png)
3. 右側のレールで、をクリックして、レシピの電源 **[!UICONTROL Recommendations Input Schema]** が入っているスキーマを表示します。 スキーマフィールド **[!UICONTROL itemId]** と **[!UICONTROL userId]** は、特定の時刻(**[!UICONTROL interactionType]**)にその顧客が購入した商品(**[!UICONTROL timestamp]**)に対応します。 同じ手順に従って、のフィールドを確認し **[!UICONTROL Recommendations Output Schema]**ます。
   ![](../images/models-recipes/model-walkthrough/preview_schemas.png)

これで、商品レコメンデーションレシピで必要な入出力スキーマを確認しました。 次のセクションに進み、商品レコメンデーションモデルの作成、トレーニング、評価の方法を見つけることができます。

## モデルのトレーニングと評価 {#train-and-evaluate-your-model}

データが準備され、レシピが使用できる状態になったので、機械学習モデルを作成、トレーニング、評価できます。

### モデルの作成

モデルはレシピのインスタンスで、データをスケールでトレーニングし、スコアを付けることができます。

1. Adobe Experience Platformで、左のナビゲーション列に移動し、ページ上部のをクリックして、組織で使用可能なすべてのレシピのリスト **[!UICONTROL Models]****[!UICONTROL Recipes]** を表示します。
   ![](../images/models-recipes/model-walkthrough/browse_recipes.png)
2. 表示されたレシピを見つけて開きます。そ **[!UICONTROL Recommendations Recipe]** のレシピの名前をクリックし、レシピの概要ページを入力します。 中央（既存のモデルがない場合） **[!UICONTROL Create a Model]** または「レシピの概要」ページの右上にあるをクリックします。
   ![](../images/models-recipes/model-walkthrough/recommendations_recipe_110.png)
3. トレーニングに使用できる入力データセットのリストが表示されます。を選択し、 **[!UICONTROL Recommendations Input Dataset]** をクリックし **[!UICONTROL Next]**ます。
   ![](../images/models-recipes/model-walkthrough/select_dataset.png)
4. モデルの名前を指定します（例：「商品レコメンデーションモデル」）。 モデルに使用可能な設定が一覧表示されます。この設定には、モデルのデフォルトのトレーニングおよびスコアリング動作の設定が含まれます。 これらの設定は組織に固有なので、変更は必要ありません。 設定を確認し、をクリックし **[!UICONTROL Finish]**ます。
   ![](../images/models-recipes/model-walkthrough/configure_model.png)
5. モデルが作成され、新しく生成されたトレーニング実行内にモデルの *概要* ページが表示されます。 モデルが作成されると、デフォルトでトレーニング実行が生成されます。
   ![](../images/models-recipes/model-walkthrough/model_post_creation.png)

トレーニングの実行が完了するのを待つか、次のセクションで新しいトレーニングの実行を作成し続けるかを選択できます。

### カスタムハイパーパラメータを使用してモデルをトレーニングする

1. 「 *モデルの概要* 」ページで、右上の **[!UICONTROL Train]** 近くにあるをクリックして、新しいトレーニング実行を作成します。 モデルの作成時に使用したのと同じ入力データセットを選択し、をクリックし **[!UICONTROL Next]**ます。
   ![](../images/models-recipes/model-walkthrough/training_select_dataset.png)
2. 「 *設定* 」ページが表示されます。 ここで、トレーニングの実行の **[!UICONTROL num_recommendations]** 値（ハイパーパラメーターとも呼ばれる）を設定できます。 トレーニングを受け、最適化されたモデルは、トレーニングの実行結果に基づいて、パフォーマンスの最も優れたハイパーパラメータを利用します。

   ハイパーパラメータは学習できないため、トレーニングを実行する前に割り当てる必要があります。 ハイパーパラメータを調整すると、トレーニングを受けたモデルの精度が変わる場合があります。 モデルの最適化は反復的なプロセスなので、十分な評価を得る前に、複数のトレーニングの実行が必要になる場合があります。

   >[!TIP] 10 **[!UICONTROL num_recommendations]** に設定します。

   ![](../images/models-recipes/model-walkthrough/configure_hyperparameter.png)
3. 新しいトレーニングの実行が完了すると、モデル評価グラフに追加のデータポイントが表示されます。これには数分かかる場合があります。
   ![](../images/models-recipes/model-walkthrough/post_training_run.png)

### モデルの評価

トレーニングの実行が完了するたびに、結果の評価指標を表示して、モデルの実行状況を判断できます。

1. トレーニングの実行をクリックして、完了した各トレーニングの実行に関する評価指標（精度と再現率）を確認します。
2. 各評価指標に対して提供される情報を調べます。 これらの指標が高いほど、モデルのパフォーマンスが向上します。
   ![](../images/models-recipes/model-walkthrough/evaluation_metrics.png)
3. 各トレーニングの実行に使用されるデータセット、スキーマおよび設定のパラメーターは、右側のパネルに表示されます。
4. 「モデル」ページに戻り、評価指標を観察して、パフォーマンスが最も高いトレーニングを実施していることを確認します。

## モデルの操作 {#operationalize-your-model}

データサイエンスワークフローの最後の手順は、データストアからのインサイトにスコアを付け、利用するために、モデルを操作することです。

### スコアを算出し、インサイトを生成する

1. 商品レコメンデーションモデル *の概要* ページで、最もパフォーマンスの高いトレーニング実行の名前をクリックし、最も再現率と精度の高い値を入力します。
2. トレーニングの実行の詳細ページの右上にあるをクリックし **[!UICONTROL Score]**&#x200B;ます。
3. スコアリング入力データセット **[!UICONTROL Recommendations Input Dataset]** としてを選択します。これは、モデルの作成時に使用したデータセットと同じで、モデルのトレーニング実行を行ったデータセットです。 Then, click **[!UICONTROL Next]**.
   ![](../images/models-recipes/model-walkthrough/scoring_input.png)
4. スコアリング出力データセット **[!UICONTROL Recommendations Output Dataset]** として「」を選択します。 スコアリング結果は、このデータセットにバッチとして保存されます。
   ![](../images/models-recipes/model-walkthrough/scoring_output.png)
5. スコア設定を確認します。 これらのパラメーターには、前に選択した入出力データセットと適切なスキーマーが含まれます。 をクリック **[!UICONTROL Finish]** して、スコアリングの実行を開始します。 実行が完了するまでに数分かかる場合があります。
   ![](../images/models-recipes/model-walkthrough/scoring_configure.png)


### 表示が洞察を得た

スコアリングの実行が正常に完了すると、生成されたインサイトの結果と表示をプレビューできます。

1. スコアリングの実行ページで、完了したスコアリングの実行をクリックし、右側のパネル **[!UICONTROL Preview Scoring Results Dataset]** のをクリックします。
   ![](../images/models-recipes/model-walkthrough/score_complete.png)
2. プレビュー表の各行には、特定の顧客に対する商品レコメンデーションが含まれ、それぞれ「」および「」とラベルが付けら **[!UICONTROL recommendations]** れ **[!UICONTROL userId]** ています。 サンプルのスクリーンショットでは **[!UICONTROL num_recommendations]** Hyperparameterが10に設定されているので、レコメンデーションの各行には、番号記号(#)で区切られた最大10個の製品IDを含めることができます。
   ![](../images/models-recipes/model-walkthrough/preview_score_results.png)

## 次の手順 {#next-steps}

商品レコメンデーションの生成が成功しました。

このチュートリアルでは、Data Science Workspaceのワークフローについて説明し、機械学習を通じて生の未処理データを有用な情報に変換する方法を示します。 Data Science Workspaceの使用方法の詳細については、次のガイドで小売売上高のスキーマとデータセットの [作成に進みます](./create-retails-sales-dataset.md)。