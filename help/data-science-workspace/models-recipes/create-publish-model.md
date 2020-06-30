---
keywords: Experience Platform;machine learning model;Data Science Workspace;popular topics
solution: Experience Platform
title: 機械学習モデルのチュートリアルを作成して公開する
topic: Tutorial
translation-type: tm+mt
source-git-commit: c48079ba997a7b4c082253a0b2867df76927aa6d
workflow-type: tm+mt
source-wordcount: '1542'
ht-degree: 0%

---


# 機械学習モデルのチュートリアルを作成して公開する

![](../images/models-recipes/model-walkthrough/objective.png)

オンラインの小売Webサイトを所有しているふりをします。 顧客が小売Webサイトで買い物をする際、ビジネスオファーに様々な他の商品を公開するために、パーソナライズされた商品のレコメンデーションを提示したいと考えます。 Webサイトが存在する間、顧客データを継続的に収集し、このデータを使用してパーソナライズされた商品レコメンデーションを生成したいと考えています。

[!DNL Adobe Experience Platform] [!DNL Data Science Workspace] 事前に作成された [商品レコメンデーションレシピを使用して目標を達成する手段を提供します](../pre-built-recipes/product-recommendations.md)。 このチュートリアルに従って、小売データにアクセスして理解し、機械学習モデルを作成して最適化し、でインサイトを生成する方法を確認し [!DNL Data Science Workspace]ます。

このチュートリアルは、のワークフローを反映し [!DNL Data Science Workspace]ており、機械学習モデルを作成するための次の手順をカバーしています。

1. [データの準備](#prepare-your-data)
2. [モデルの作成](#author-your-model)
3. [モデルのトレーニングと評価](#train-and-evaluate-your-model)
4. [モデルの操作](#operationalize-your-model)

## はじめに

このチュートリアルを開始する前に、次の前提条件を満たす必要があります。

* へのアクセス [!DNL Adobe Experience Platform]。 でIMS組織にアクセスできない場合は、先に進む前にシステム管理者にお問い合わせ [!DNL Experience Platform]ください。

* 有効化アセット。 アカウント担当者に問い合わせて、次のアイテムをプロビジョニングしてください。
   * Recommendationsのレシピ
   * Recommendations入力データセット
   * レコメンデーション入力スキーマ
   * Recommendations出力データセット
   * Recommendationsの出力スキーマ
   * ゴールデンデータセットpostValues
   * ゴールデンデータセットスキーマ

* 必要な3つの [!DNL Jupyter Notebook] ファイルを <a href="https://github.com/adobe/experience-platform-dsw-reference/tree/master/Summit/2019/resources/Notebooks-Thurs" target="_blank">Adobeパブリック [!DNL Git] リポジトリからダウンロードします</a>。これらのファイルは、での [!DNL JupyterLab] ワークフローのデモに使用され [!DNL Data Science Workspace]ます。

* このチュートリアルで使用する次の主要概念の実際の理解
   * [!DNL Experience Data Model](../../xdm/home.md): 標準化の取り組みは、Customer Experience Managementに対して、ExperienceEventやExperienceEventなどの標準スキーマを定義する際にアドビが主導 [!DNL Profile] します。
   * データセット： 実際のデータのストレージと管理の構成体。 [XDMスキーマの物理的にインスタンス化されたインスタンス](../../xdm/schema/field-dictionary.md)。
   * バッチ： データセットはバッチで構成されます。 バッチとは、ある期間に収集され、1つの単位として一緒に処理される一連のデータです。
   * [!DNL JupyterLab]: [!DNL JupyterLab](https://blog.jupyter.org/jupyterlab-is-ready-for-users-5a6f039b8906) は、Project用のオープンソースのWebベースのインターフェイスで、に緊密に統合さ [!DNL Jupyter] れてい [!DNL Experience Platform]ます。

## データの準備 {#prepare-your-data}

機械学習モデルを作成して、顧客に対する商品の推奨事項をパーソナライズするには、Webサイトでの以前の顧客の購入を分析する必要があります。 この節では、このデータがどのように [!DNL Platform] 取り込まれ [!DNL Adobe Analytics]るか、およびそのデータが機能データセットに変換され、機械学習モデルで使用されるようにする方法について説明します。

### データを調べ、スキーマを理解する

1. [Adobe Experience Platformにログインし](https://platform.adobe.com/) 、「 **[!UICONTROL Datasets]** 」をクリックして既存のデータセットをすべてリストし、調査するデータセットを選択します。 この場合、データ [!DNL Analytics] セット **Golden Data Set postValues**。
   ![](../images/models-recipes/model-walkthrough/datasets_110.png)
2. 右上付近にある **[!UICONTROL プレビューデータセット]** (Dataset **[!UICONTROL )を選択し、サンプルレコードを調べてから、「]**閉じる」をクリックします。
   ![](../images/models-recipes/model-walkthrough/golden_data_set_110.png)
3. 右側のパネルの「スキーマ」の下にあるリンクを選択して、データセットのスキーマを表示し、データセットの詳細ページに戻ります。」
   ![](../images/models-recipes/model-walkthrough/golden_schema_110.png)

その他のデータセットは、プレビュー用にバッチで事前設定されています。 上記の手順を繰り返すことで、これらのデータセットを表示できます。

| データセット名 | スキーマ | 説明 |
| ----- | ----- | ----- |
| ゴールデンデータセットpostValues | ゴールデンデータセットスキーマ | [!DNL Analytics] webサイトのソースデータ |
| Recommendations入力データセット | レコメンデーション入力スキーマ | データは、機能パイプラインを使用してトレーニングデータセットに変換されます。 [!DNL Analytics] このデータは、Product Recommendationsの機械学習モデルのトレーニングに使用されます。 `itemid` そして `userid` その顧客が購入した商品に対応する |
| Recommendations出力データセット | Recommendationsの出力スキーマ | スコアリング結果が格納されるデータセット。各顧客のレコメンデーション商品のリストが含まれます。 |

## モデルの作成 {#author-your-model}

この [!DNL Data Science Workspace] ライフサイクルの2つ目の要素は、レシピとモデルの作成です。 商品レコメンデーションレシピは、過去の購入データや機械学習を利用して、商品レコメンデーションを規模の大きいもので生成するように設計されています。

レシピは、機械学習アルゴリズムと特定の問題を解決するためのロジックを含むので、モデルの基本となります。 さらに重要な点は、レシピを使用することで、組織全体の機械学習を民主化でき、他のユーザーがコードを記述することなく、さまざまな使用例に対応したモデルにアクセスできることです。

### 商品レコメンデーションレシピの参照

1. で、左 [!DNL Adobe Experience Platform]のナビゲーション列から[ **[!UICONTROL モデル]** ]に移動し、上部の[ **[!UICONTROL レシピ]** ]をクリックして、組織で使用可能なレシピのリストを表示します。
   ![](../images/models-recipes/model-walkthrough/browse_recipes.png)
2. 指定された **[!UICONTROL Recommendationsレシピを探して開きます]** 。レコメンデーションレシピ名はクリックします。
   ![](../images/models-recipes/model-walkthrough/recommendations_recipe_110.png)
3. 右側のパネルで、「 **[!UICONTROL Recommendations入力スキーマ]** 」をクリックして、レシピを実行するスキーマを表示します。 スキーマフィールド **[!UICONTROL itemId]** と **[!UICONTROL userId]** は、特定の時刻(**[!UICONTROL timestamp)に、その顧客が購入した商品(]** interactionType ****)に対応します。 同じ手順に従って、 **[!UICONTROL Recommendationsの出力スキーマのフィールドを確認します]**。
   ![](../images/models-recipes/model-walkthrough/preview_schemas.png)

これで、商品レコメンデーションレシピで必要な入出力スキーマを確認しました。 次のセクションに進み、商品レコメンデーションモデルの作成、トレーニング、評価の方法を見つけることができます。

## モデルのトレーニングと評価 {#train-and-evaluate-your-model}

データが準備され、レシピが使用できる状態になったので、機械学習モデルを作成、トレーニング、評価できます。

### モデルの作成

モデルはレシピのインスタンスで、データをスケールでトレーニングし、スコアを付けることができます。

1. で、左 [!DNL Adobe Experience Platform]のナビゲーション列から「 **[!UICONTROL モデル]** 」に移動し、ページ上部の「 **[!UICONTROL レシピ]** 」をクリックして、組織で使用可能なすべてのレシピのリストを表示します。
   ![](../images/models-recipes/model-walkthrough/browse_recipes.png)
2. レコメンデーションレシピを探して開きます **** 。レコメンデーションレシピの名前をクリックし、レシピの概要ページを入力します。 中央から（既存のモデルがない場合）またはレシピ概要ページの右上から **** 、「モデルを作成」をクリックします。
   ![](../images/models-recipes/model-walkthrough/recommendations_recipe_110.png)
3. トレーニングに使用できる入力データセットのリストを示します。「 **[!UICONTROL Recommendations入力データセット]** 」を選択し、「 **[!UICONTROL 次へ]**」をクリックします。
   ![](../images/models-recipes/model-walkthrough/select_dataset.png)
4. モデルの名前を指定します（例：「商品レコメンデーションモデル」）。 モデルに使用可能な設定が一覧表示されます。この設定には、モデルのデフォルトのトレーニングおよびスコアリング動作の設定が含まれます。 これらの設定は組織に固有なので、変更は必要ありません。 設定を確認し、「 **[!UICONTROL 完了]**」をクリックします。
   ![](../images/models-recipes/model-walkthrough/configure_model.png)
5. モデルが作成され、新しく生成されたトレーニング実行内にモデルの *概要* ページが表示されます。 モデルが作成されると、デフォルトでトレーニング実行が生成されます。
   ![](../images/models-recipes/model-walkthrough/model_post_creation.png)

トレーニングの実行が完了するのを待つか、次のセクションで新しいトレーニングの実行を作成し続けるかを選択できます。

### カスタムハイパーパラメータを使用してモデルをトレーニングする

1. [ *モデルの概要* ]ページで、右上の **[!UICONTROL [トレーニング]** ]をクリックして、新しいトレーニング経路を作成します。 モデルの作成時に使用したのと同じ入力データセットを選択し、「 **[!UICONTROL 次へ]**」をクリックします。
   ![](../images/models-recipes/model-walkthrough/training_select_dataset.png)
2. 「 *設定* 」ページが表示されます。 ここで、トレーニング実行の **[!UICONTROL num_recommendations]** 値（Hyperparameterとも呼ばれます）を設定できます。 トレーニングを受け、最適化されたモデルは、トレーニングの実行結果に基づいて、パフォーマンスの最も優れたハイパーパラメータを利用します。

   ハイパーパラメータは学習できないため、トレーニングを実行する前に割り当てる必要があります。 ハイパーパラメータを調整すると、トレーニングを受けたモデルの精度が変わる場合があります。 モデルの最適化は反復的なプロセスなので、十分な評価を得る前に、複数のトレーニングの実行が必要になる場合があります。

   >[!TIP] num_recommendations **[!UICONTROL を10に設定します]** 。

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
2. トレーニングの実行の詳細ページの右上にある「 **[!UICONTROL スコア]**」をクリックします。
3. スコアリング入力データセットとして **[!UICONTROL Recommendations入力データセット]** を選択します。これは、モデルの作成時に使用したものと同じデータセットで、トレーニングの実行を行います。 Then, click **[!UICONTROL Next]**.
   ![](../images/models-recipes/model-walkthrough/scoring_input.png)
4. スコアリング出力データセットとして **[!UICONTROL Recommendations出力データセット]** を選択します。 スコアリング結果は、このデータセットにバッチとして保存されます。
   ![](../images/models-recipes/model-walkthrough/scoring_output.png)
5. スコア設定を確認します。 これらのパラメーターには、前に選択した入出力データセットと適切なスキーマーが含まれます。 「 **[!UICONTROL 完了]** 」をクリックして、スコアリングの実行を開始します。 実行が完了するまでに数分かかる場合があります。
   ![](../images/models-recipes/model-walkthrough/scoring_configure.png)


### 表示が洞察を得た

スコアリングの実行が正常に完了すると、生成されたインサイトの結果と表示をプレビューできます。

1. スコアリングの実行ページで、完了したスコアリングの実行をクリックし、右側のレールの「 **[!UICONTROL プレビュースコアリング結果データセット]** 」をクリックします。
   ![](../images/models-recipes/model-walkthrough/score_complete.png)
2. プレビュー表の各行には、特定の顧客に対する商品レコメンデーションが含まれています。この商品レコメンデーションには、 **[!UICONTROL recommendations]** 、 **[!UICONTROL userId]** というラベルがそれぞれ付けられています。 サンプルのスクリーンショットでは **[!UICONTROL num_recommendations]** Hyperparameterが10に設定されているので、レコメンデーションの各行には、番号記号(#)で区切られた最大10個の製品IDを含めることができます。
   ![](../images/models-recipes/model-walkthrough/preview_score_results.png)

## 次の手順 {#next-steps}

商品レコメンデーションの生成が成功しました。

このチュートリアルでは、のワークフローを紹介し [!DNL Data Science Workspace]、機械学習を通じて生の未処理データを有用な情報に変換する方法を説明します。 このツールの使用方法の詳細につ [!DNL Data Science Workspace]いては、次の小売売上高スキーマとデータセット [の](./create-retails-sales-dataset.md)作成ガイドに進みます。