---
keywords: Experience Platform;JupyterLab；レシピ；ノートブック；データサイエンスWorkspace；人気のトピック；レシピの作成
solution: Experience Platform
title: JupyterLab Notebooks を使用したモデルの作成
type: Tutorial
description: このチュートリアルでは、JupyterLab ノートブックレシピビルダー テンプレートを使用してレシピを作成するために必要な手順について説明します。
exl-id: d3f300ce-c9e8-4500-81d2-ea338454bfde
source-git-commit: 5d98dc0cbfaf3d17c909464311a33a03ea77f237
workflow-type: tm+mt
source-wordcount: '2106'
ht-degree: 29%

---

# JupyterLab Notebook を使用したモデル作成

>[!NOTE]
>
>データサイエンスワークスペースは購入できなくなりました。
>
>このドキュメントは、以前に データ Science ワークスペース の利用資格を持つ既存のお客様を対象としています。

このチュートリアルでは、JupyterLab ノートブックレシピビルダーテンプレートを使用してモデルを作成するために必要な手順について説明します。

## 導入された概念：

- **レシピ：** レシピとは、モデル仕様を表すAdobeの用語で、トレーニング済みのモデルを作成して実行するために必要な、特定の機械学習、AI アルゴリズムまたはアルゴリズムのアンサンブル、処理ロジックおよび設定を表す最上位のコンテナです。
- **モデル**：モデルは、履歴データと構成を使用してビジネスユースケースを解決するためにトレーニングされる機械学習レシピのインスタンスです。
- **トレーニング**：トレーニングは、ラベル付きのデータからパターンやインサイトを学習するプロセスです。
- **スコアリング**：スコアリングは、トレーニング済みモデルを使用してデータからインサイトを生成するプロセスです。

## 必要なアセットのダウンロード {#assets}

このチュートリアルを進める前に、必要なスキーマとデータセットを作成する必要があります。 [Luma 傾向モデルスキーマとデータセットの作成 ](../models-recipes/create-luma-data.md) に関するチュートリアルにアクセスして、必要なアセットをダウンロードし、前提条件を設定します。

## [!DNL JupyterLab] notebook 環境の基本を学ぶ

レシピをゼロから作成するには、[!DNL Data Science Workspace] 内で行います。 開始するには、[Adobe Experience Platform](https://platform.adobe.com) に移動し、左側の **[!UICONTROL ノートブック]** タブを選択します。 新しいノートブックを作成するには、[!DNL JupyterLab Launcher] ールバーからレシピビルダーテンプレートを選択します。

[!UICONTROL  レシピビルダー ] ノートブックを使用すると、ノートブック内でトレーニング実行とスコアリング実行を実行できます。 これにより、トレーニングデータとスコアリングデータで実験を実行する間に、`train()` メソッドと `score()` メソッドを柔軟に変更できます。トレーニングとスコアリングの出力に満足したら、レシピを作成し、さらにレシピを使用して機能をモデル化し、モデルとして公開できます。

>[!NOTE]
>
>[!UICONTROL  レシピビルダー ] ノートブックはすべてのファイル形式での作業をサポートしていますが、現在、レシピを作成機能は [!DNL Python] のみをサポートしています。

![](../images/jupyterlab/create-recipe/recipe_builder-new.png)

ランチャーから [!UICONTROL  レシピビルダー ] ノートブックを選択すると、ノートブックが新しいタブで開きます。

上部にある新しいノートブックタブに、追加の 3 つのアクション（**[!UICONTROL トレーニング]**、**[!UICONTROL スコア]**、**[!UICONTROL レシピを作成]** を含むツールバーが読み込まれます。 これらのアイコンは、「レシピビルダー [!UICONTROL  ノートブックにのみ表示さ ] ます。 ノートブックでレシピを作成した後、これらのアクションについて詳しくは [ トレーニングとスコアリングの節 ](#training-and-scoring) を参照してください。

![](../images/jupyterlab/create-recipe/toolbar_actions.png)

## [!UICONTROL レシピビルダー]ノートブックの概要

提供された アセット フォルダーには、Luma 傾向モデル `propensity_model.ipynb`があります。 JupyterLab の [ノートブックアップロード] オプションを使用して、指定されたモデルアップロード、ノートブックを開きます。

![アップロードノートブック](../images/jupyterlab/create-recipe/upload_notebook.png)

このチュートリアルの残りの部分では、傾向モデル ノートブックで事前に定義されている次のファイルについて説明します。

- [要件ファイル](#requirements-file)
- [設定ファイル](#configuration-files)
- [トレーニングデータローダー](#training-data-loader)
- [スコアリングデータローダー](#scoring-data-loader)
- [パイプラインファイル](#pipeline-file)
- [評価ファイル](#evaluator-file)
- [データセーバーファイル](#data-saver-file)

次のビデオチュートリアルでは、Luma 傾向モデル ノートブックについて説明します。

>[!VIDEO](https://video.tv.adobe.com/v/333570)

### 要件ファイル {#requirements-file}

要件ファイルは、モデルで使用する追加のライブラリを宣言するために使用されます。 依存関係がある場合は、バージョン番号を指定できます。その他のライブラリを探すには、[anaconda.org](https://anaconda.org) にアクセスしてください。 要件ファイルのフォーマット方法については、[Conda](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-file-manually) を参照してください。 既に使用されている主なライブラリのリストは次のとおりです。

```JSON
python=3.6.7
scikit-learn
pandas
numpy
data_access_sdk_python
```

>[!NOTE]
>
>追加するライブラリまたは特定のバージョンは、上記のライブラリと互換性がない場合があります。 また、環境 ファイルを手動で作成する場合は、 `name` フィールドを上書きできません。

Luma 傾向モデルノートブックの場合、要件を更新する必要はありません。

### 設定ファイル {#configuration-files}

設定ファイル（`training.conf` および `scoring.conf`）は、トレーニングとスコアリングに使用するデータセットを指定し、ハイパーパラメーターを追加するために使用されます。トレーニングとスコアリングには別々の設定があります。

モデルがトレーニングを実行するには、`trainingDataSetId`、`ACP_DSW_TRAINING_XDM_SCHEMA`、`tenantId` を指定する必要があります。 スコアリングの場合は、さらに `scoringDataSetId`、`tenantId`、`scoringResultsDataSetId ` を指定する必要があります。

データセット ID とスキーマ ID を見つけるには、左側のナビゲーションバー（フォルダーアイコンの下 ](../images/jupyterlab/create-recipe/dataset-tab.png) のノートブック内の「データ」タブ ![ 「データ」タブ）に移動します。 3 つの異なるデータセット ID を指定する必要があります。 `scoringResultsDataSetId` は、モデルのスコアリング結果の保存に使用され、空のデータセットにする必要があります。 これらのデータセットは、以前 [ 必須アセット ](#assets) 手順で作成されたものです。

![](../images/jupyterlab/create-recipe/dataset_tab.png)

同じ情報は、「[Adobe Experience Platform](https://platform.adobe.com/)」の「**[スキーマ](https://platform.adobe.com/schema)**」タブと「**[データセット](https://platform.adobe.com/dataset/overview)**」タブにあります。

競争すると、トレーニングとスコアの構成は次のスクリーンショットのようになります。

![ 設定 ](../images/jupyterlab/create-recipe/config.png)

デフォルトでは、データのトレーニングとスコアリングを行う際に、次の設定パラメーターが設定されます。

- `ML_FRAMEWORK_IMS_USER_CLIENT_ID`
- `ML_FRAMEWORK_IMS_TOKEN`
- `ML_FRAMEWORK_IMS_ML_TOKEN`
- `ML_FRAMEWORK_IMS_TENANT_ID`

## トレーニングデータローダーについて {#training-data-loader}

トレーニングデータローダーの目的は、機械学習モデルの作成に使用するデータをインスタンス化することです。通常、データローダーのトレーニングには、次の 2 つのタスクがあります。

- [!DNL Platform] からデータを読み込み中
- データの準備と特徴量エンジニアリング

以下の 2 つの節で、データの読み込みとデータの準備について説明します。

### データの読み込み {#loading-data}

この手順では、[pandas データフレーム](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html)を使用します。データは、[!DNL Platform] SDK(`platform_sdk`)を使用して[!DNL Adobe Experience Platform]内のファイルからロードすることも、パンダの`read_csv()`または`read_json()`機能を使用して外部ソースからロードすることもできます。

- [[!DNL Platform SDK]](#platform-sdk)
- [外部ソース](#external-sources)

>[!NOTE]
>
>レシピビルダーノートブックでは、データは `platform_sdk` データローダーを使用して読み込まれます。

### [!DNL Platform] SDK {#platform-sdk}

`platform_sdk` データローダーの使用に関する詳細なチュートリアルについては、『[Platform SDK ガイド](../authoring/platform-sdk.md)』を参照してください。このチュートリアルでは、認証の構築、データの基本読み取り、およびデータの基本的な書き込みに関する情報を提供します。

### 外部ソース {#external-sources}

この節では、JSON または CSV ファイルを pandas オブジェクトにインポートする方法を示します。Pandas ライブラリの公式ドキュメントについては、次の URL を参照してください。
- [read_csv](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html)
- [read_json](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_json.html)

まず、CSV ファイルのインポート例を示します。`data` 引数は CSV ファイルへのパスです。この変数は、[前の節](#configuration-files)の `configProperties` からインポートされました。

```PYTHON
df = pd.read_csv(data)
```

また、JSON ファイルからインポートすることもできます。`data` 引数は CSV ファイルへのパスです。この変数は、[前の節](#configuration-files)の `configProperties` からインポートされました。

```PYTHON
df = pd.read_json(data)
```

これで、データはデータフレームオブジェクトに含まれ、[次の節](#data-preparation-and-feature-engineering)で分析および操作できます。

## トレーニング データ ローダーファイル

この例では、Platform SDK を使用してデータが読み込まれます。 ライブラリは、次の行を含めることで、ページの上部にインポートできます。

`from platform_sdk.dataset_reader import DatasetReader`

その後、`load()` メソッドを使用して、設定（`recipe.conf`）ファイルで設定したトレーニングデータセットを `trainingDataSetId` から取得できます。

```PYTHON
def load(config_properties):
    print("Training Data Load Start")

    #########################################
    # Load Data
    #########################################    
    client_context = get_client_context(config_properties)
    dataset_reader = DatasetReader(client_context, dataset_id=config_properties['trainingDataSetId'])
```

>[!NOTE]
>
>[ 設定ファイルの節 ](#configuration-files) で説明したように、`client_context = get_client_context(config_properties)` を使用してExperience Platformからデータにアクセスする場合、次の設定パラメーターが設定されます。
> - `ML_FRAMEWORK_IMS_USER_CLIENT_ID`
> - `ML_FRAMEWORK_IMS_TOKEN`
> - `ML_FRAMEWORK_IMS_ML_TOKEN`
> - `ML_FRAMEWORK_IMS_TENANT_ID`

データが揃ったら、データの準備と特徴のエンジニアリングから始めます。

### データの準備と特徴量エンジニアリング {#data-preparation-and-feature-engineering}

データを読み込んだ後、データをクリーンアップしてデータを準備する必要があります。 この例では、モデルの目標は、顧客が製品を注文するかどうかを予測することです。 モデルは特定の製品を表示しないので、`productListItems` は不要なので、列は削除されます。 次に、1 つの列に 1 つの値または 2 つの値のみを含む追加の列が削除されます。 モデルトレーニングときは、目標の予測に役立つ有用なデータのみを保持することが重要です。

![データ準備の例](../images/jupyterlab/create-recipe/data_prep.png)

不要なデータを削除したら、特徴エンジニアリングを開始できます。 この例で使用するデモ データには、セッション情報は含まれていません。 通常は、特定の顧客の現在および過去のセッションに関するデータが必要です。 セッション情報が不足しているため、この例では、代わりに ジャーニー で区切って現在および過去のセッションを模倣します。

![ジャーニーの境界](../images/jupyterlab/create-recipe/journey_demarcation.png)

区切りが完了すると、データにラベルが付けられ、ジャーニーが作成されます。

![ データにラベルを付ける ](../images/jupyterlab/create-recipe/label_data.png)

次に、特徴を作成し、過去と現在に分けます。 その後、不要な列はすべて削除され、Luma の顧客に対する過去のジャーニーと現在のジャーニーの両方が残ります。 これらのジャーニーには、顧客が商品を購入したかどうか、購入に至るまでのジャーニーなどの情報が含まれています。

![ 最終現在のトレーニング ](../images/jupyterlab/create-recipe/final_journey.png)

## スコアリングデータローダー {#scoring-data-loader}

スコアリング用のデータのロード手順は、トレーニング・データのロード手順と似ています。 コードをよく見ると、`dataset_reader` の `scoringDataSetId` を除いてすべてが同じであることがわかります。 これは、トレーニングとスコアリングの両方に同じ Luma データソースが使用されるからです。

様々なデータ・ファイルをトレーニングとスコアリングに使用する場合、トレーニングとスコアリングのデータ・ローダーは個別になります。 これにより、必要に応じて、トレーニングデータをスコアリングデータにマッピングするなど、追加の前処理を実行できます。

## パイプラインファイル {#pipeline-file}

`pipeline.py` ファイルには、トレーニングとスコアリングのロジックが含まれています。

トレーニングの目的は、トレーニング データセットの特徴とラベルを使用してモデルを作成することです。 トレーニング モデルを選択したら、x と y のトレーニング データセットをモデルに適合させる必要があり、関数はトレーニング済みのモデルを返します。

>[!NOTE]
> 
>特徴とは、ラベルを予測するために機械学習モデルによって使用される入力変数を指します。

![デフトレイン](../images/jupyterlab/create-recipe/def_train.png)

`score()` 関数には、スコアリングアルゴリズムを含め、モデルの成功度を示す測定値を返す必要があります。`score()` 関数は、スコアリングデータセットラベルとトレーニング済みモデルを使用して、予測された特徴のセットを生成します。次に、これらの予測値が、スコアリングデータセットの実際の特徴と比較されます。この例では、`score()` 関数は、トレーニング済みモデルを使用して、スコアリングデータセットのラベルを使用して特徴を予測します。予測された特徴が返されます。

![def スコア ](../images/jupyterlab/create-recipe/def_score.png)

## 評価ファイル {#evaluator-file}

`evaluator.py` ファイルには、トレーニング済みレシピを評価する方法と、トレーニングデータを分割する方法に関するロジックが含まれています。

### データセットの分割 {#split-the-dataset}

トレーニングのデータ準備段階では、トレーニングとテストに使用するデータセットを分割する必要があります。この `val` データは、トレーニング後にモデルを評価するために暗黙的に使用されます。 このプロセスはスコアリングとは別のものです。

この節では、ノートブックにデータを読み込んだ後、データセット内の関連のない列を削除してデータをクリーンアップする `split()` 関数について説明します。 ここからフィーチャーエンジニアリングを実行できます。これは、データ内の既存の未加工フィーチャーから追加の関連フィーチャーを作成するプロセスです。

![Split 関数 ](../images/jupyterlab/create-recipe/split.png)

### トレーニング済みモデルの評価 {#evaluate-the-trained-model}

`evaluate()` 関数は、モデルのトレーニング後に実行され、モデルの実行の成功を示す指標を返します。`evaluate()` 関数は、テスト データセット ラベルとトレーニング済みモデルを使用して、一連の特徴を予測します。これらの予測値は、テストデータセットの実際の特徴と比較されます。この例で使用されている指標は、 `precision`、 `recall`、 `f1`および `accuracy`です。 この関数は、評価指標の配列を含む `metric` オブジェクトを返します。これらのメトリックは、トレーニング済みモデルのパフォーマンスを評価するために使用されます。

![ 評価 ](../images/jupyterlab/create-recipe/evaluate.png)

`print(metric)` を追加すると、指標の結果を表示できます。

![ 指標の結果 ](../images/jupyterlab/create-recipe/evaluate_metric.png)

## データセーバーファイル {#data-saver-file}

`datasaver.py`ファイルには`save()`関数が含まれており、スコアのテスト中に予測を保存するために使用されます。`save()`関数は予測を受け取り、[!DNL Experience Platform Catalog] API を使用して、`scoring.conf` ファイルで指定した`scoringResultsDataSetId`にデータを書き込みます。あなたはできる

![データセーバー](../images/jupyterlab/create-recipe/data_saver.png)

## トレーニングとスコアリング {#training-and-scoring}

ノートブックの変更が完了し、レシピをトレーニングする場合は、バーの上部にある関連ボタンを選択して、セルでトレーニング実行を作成できます。 ボタンを選択すると、トレーニングスクリプトからのコマンドと出力のログがノートブック（`evaluator.py` のセルの下）に表示されます。 Conda は、最初にすべての依存関係をインストールし、その後トレーニングを開始します。

スコアリングを実行する前に、少なくとも 1 回はトレーニングを実行する必要があります。「**[!UICONTROL スコアリングを実行]**」ボタンを選択すると、トレーニング中に生成されたトレーニング済みモデルにスコアが付けられます。 スコア付けスクリプトが `datasaver.py` の下に表示されます。

デバッグの目的で、非表示の出力を表示する場合は、出力セルの末尾に `debug` を追加し、再実行します。

![ トレーニングとスコア ](../images/jupyterlab/create-recipe/toolbar_actions.png)

## レシピの作成 {#create-recipe}

レシピの編集が完了し、トレーニング/スコアリング出力に満足したら、右上の **[!UICONTROL レシピを作成]** を選択して、ノートブックからレシピを作成できます。

![](../images/jupyterlab/create-recipe/create-recipe.png)

「**[!UICONTROL レシピの作成]**」を選択すると、レシピ名を入力するよう求められます。 この名前は、[!DNL Platform] で作成された実際のレシピを表します。

![](../images/jupyterlab/create-recipe/enter_recipe_name.png)

「**[!UICONTROL OK]**」を選択すると、レシピの作成プロセスが開始されます。 これには時間がかかり、「レシピを作成」ボタンの代わりに進行状況バーが表示されます。 完了したら、「**[!UICONTROL レシピを表示]**」ボタンを選択すると、「**[!UICONTROL ML モデル**[!UICONTROL  の下の「レシピ ]**」タブに移動]** ます。

![](../images/jupyterlab/create-recipe/recipe_creation_started.png)

>[!CAUTION]
>
> - ファイルのセルを削除しないでください
> - ファイルのセルの先頭の `%%writefile` 行を編集しないでください
> - 異なるノートブックに同時にレシピを作成しないでください

## 次の手順 {#next-steps}

このチュートリアルを完了すると、 [!UICONTROL レシピビルダー] ノートブックで機械学習モデルを作成する方法を学習しました。 また、ノートブックを行使してレシピ ワークフローする方法も学びました。

[!DNL Data Science Workspace] 内でリソースを操作する方法を引き続き学習するには、[!DNL Data Science Workspace] のレシピとモデル ドロップダウンを参照してください。