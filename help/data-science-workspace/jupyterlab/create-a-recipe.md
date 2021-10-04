---
keywords: Experience Platform;JupyterLab；レシピ；ノートブック；Data Science Workspace；よく読まれるトピック；レシピの作成
solution: Experience Platform
title: Jupyter ノートブックを使用したレシピの作成
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、2 つの主なセクションについて説明します。まず、JupyterLab ノートブック内のテンプレートを使用して機械学習モデルを作成します。次に、JupyterLab 内でノートブックをレシピワークフローに導き、Data Science Workspace 内でレシピを作成します。
exl-id: d3f300ce-c9e8-4500-81d2-ea338454bfde
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '2364'
ht-degree: 79%

---

# Jupyter ノートブックを使用したレシピの作成

このチュートリアルでは、2 つの主なセクションについて説明します。まず、[!DNL JupyterLab Notebook] 内のテンプレートを使用して機械学習モデルを作成します。 次に、[!DNL JupyterLab] 内でノートブックをレシピワークフローに導き、[!DNL Data Science Workspace] 内でレシピを作成します。

## 導入された概念：

- **レシピ**：レシピは、アドビ用語でモデル仕様を意味します。レシピは、トレーニング済みモデルの構築と実行に必要な特定の機械学習、AI アルゴリズムまたはアルゴリズムのアンサンブル、処理ロジック、および構成を表す最上位のコンテナであり、特定のビジネス問題の解決に役立ちます。
- **モデル**：モデルは、履歴データと構成を使用してビジネスユースケースを解決するためにトレーニングされる機械学習レシピのインスタンスです。
- **トレーニング**：トレーニングは、ラベル付きのデータからパターンやインサイトを学習するプロセスです。
- **スコアリング**：スコアリングは、トレーニング済みモデルを使用してデータからインサイトを生成するプロセスです。

## [!DNL JupyterLab] ノートブック環境の概要

[!DNL Data Science Workspace] 内では、最初からレシピを作成することができます。 まず、[Adobe Experience Platform](https://platform.adobe.com) に移動し、左側の「**[!UICONTROL ノートブック]**」タブをクリックします。 [!DNL JupyterLab Launcher] から Recipe Builder テンプレートを選択して、新しいノートブックを作成します。

[!UICONTROL Recipe Builder] ノートブックを使用すると、ノートブック内でトレーニングとスコアリングの実行を実行できます。 これにより、トレーニングデータとスコアリングデータで実験を実行する間に、`train()` メソッドと `score()` メソッドを柔軟に変更できます。トレーニングとスコアリングの出力に満足したら、Recipe Builder ノートブックに組み込まれたノートブックからレシピへの機能を使用して、[!DNL Data Science Workspace] で使用するレシピを作成できます。

>[!NOTE]
>
>Recipe Builder ノートブックは、すべてのファイル形式での作業をサポートしていますが、現在のところ、レシピの作成機能は [!DNL Python] のみをサポートしています。

![](../images/jupyterlab/create-recipe/recipe_builder.png)

ランチャーから Recipe Builder ノートブックをクリックすると、タブでノートブックが開きます。 ノートブックで使用されるテンプレートは、[こちらのパブリックリポジトリー](https://github.com/adobe/experience-platform-dsw-reference/tree/master/recipes/python/retail/)にもある Python 小売売上予測レシピです。

ツールバーには、「**[!UICONTROL トレーニング]**」、「**[!UICONTROL スコア]**」、「**[!UICONTROL レシピの作成]**」の 3 つのアクションがあります。 これらのアイコンは、[!UICONTROL Recipe Builder] ノートブックにのみ表示されます。 これらのアクションに関する詳細は、ノートブックでレシピを作成した後に、[トレーニングとスコアリング](#training-and-scoring)の節で説明します。

![](../images/jupyterlab/create-recipe/toolbar_actions.png)

## レシピファイルの編集

レシピファイルを編集するには、ファイルパスに対応する Jupyter 内のセルに移動します。例えば、`evaluator.py` に変更を加える場合は、`%%writefile demo-recipe/evaluator.py`を探します 。

セルに必要な変更を加え、終了したらセルを実行します。`%%writefile filename.py` コマンドは、セルの内容を `filename.py` に書き込みます。変更を加えたファイルごとに手動でセルを実行する必要があります。

>[!NOTE]
>
> 該当する場合は、セルを手動で実行する必要があります。

## Recipe Builder ノートブックの概要

[!DNL JupyterLab] ノートブック環境の基本を理解したので、機械学習モデルのレシピを構成するファイルの確認を開始できます。 ここで説明するファイルは次のとおりです。

- [要件ファイル](#requirements-file)
- [設定ファイル](#configuration-files)
- [トレーニングデータローダー](#training-data-loader)
- [スコアリングデータローダー](#scoring-data-loader)
- [パイプラインファイル](#pipeline-file)
- [評価ファイル](#evaluator-file)
- [データセーバーファイル](#data-saver-file)

### 要件ファイル {#requirements-file}

要件ファイルは、レシピで使用する追加のライブラリを宣言するために使用されます。依存関係がある場合は、バージョン番号を指定できます。追加のライブラリを探すには、[anaconda.org](https://anaconda.org) を参照してください。 要件ファイルのフォーマット方法については、[Conda](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-file-manually) を参照してください。 既に使用されている主なライブラリのリストは次のとおりです。

```JSON
python=3.6.7
scikit-learn
pandas
numpy
data_access_sdk_python
```

>[!NOTE]
>
> 追加したライブラリまたは特定のバージョンは、上記のライブラリと互換性がない場合があります。また、環境ファイルを手動で作成する場合、 `name` フィールドは上書きできません。

### 設定ファイル {#configuration-files}

設定ファイル（`training.conf` および `scoring.conf`）は、トレーニングとスコアリングに使用するデータセットを指定し、ハイパーパラメーターを追加するために使用されます。トレーニングとスコアリングには別々の設定があります。

トレーニングとスコアリングを実行する前に、次の変数を入力する必要があります。
- `trainingDataSetId`
- `ACP_DSW_TRAINING_XDM_SCHEMA`
- `scoringDataSetId`
- `ACP_DSW_SCORING_RESULTS_XDM_SCHEMA`
- `scoringResultsDataSetId`

データセットとスキーマ ID を確認するには、左側のナビゲーションバー（フォルダーアイコンの下）にあるノートブック内の「データ」タブ ![「データ」タブ ](../images/jupyterlab/create-recipe/dataset-tab.png) に移動します。

![](../images/jupyterlab/create-recipe/dataset_tab.png)

同じ情報は、「[Adobe Experience Platform](https://platform.adobe.com/)」の「**[スキーマ](https://platform.adobe.com/schema)**」タブと「**[データセット](https://platform.adobe.com/dataset/overview)**」タブにあります。

デフォルトでは、データにアクセスする際に次の設定パラメーターが設定されます。

- `ML_FRAMEWORK_IMS_USER_CLIENT_ID`
- `ML_FRAMEWORK_IMS_TOKEN`
- `ML_FRAMEWORK_IMS_ML_TOKEN`
- `ML_FRAMEWORK_IMS_TENANT_ID`

## トレーニングデータローダー {#training-data-loader}

トレーニングデータローダーの目的は、機械学習モデルの作成に使用するデータをインスタンス化することです。通常、トレーニングデータローダーが達成するタスクは 2 つあります。
- [!DNL Platform] からデータを読み込む
- データの準備と特徴量エンジニアリング

以下の 2 つの節で、データの読み込みとデータの準備について説明します。

### データの読み込み {#loading-data}

この手順では、[pandas データフレーム](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html)を使用します。データは、 [!DNL Platform] SDK (`platform_sdk`) を使用して [!DNL Adobe Experience Platform] のファイルから、または pandas の `read_csv()` 関数または `read_json()` 関数を使用して外部ソースから読み込むことができます。

- [[!DNL Platform SDK]](#platform-sdk)
- [外部ソース](#external-sources)

>[!NOTE]
>
> Recipe Builder ノートブックでは、データは `platform_sdk` データローダーを介して読み込まれます。

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

### Platform SDK から

Platform SDK を使用してデータを読み込むことができます。 ライブラリは、次の行を含めることで、ページの上部にインポートできます。

`from platform_sdk.dataset_reader import DatasetReader`

次に、`load()` メソッドを使用して、設定（`recipe.conf`）ファイルに設定されたとおりに `trainingDataSetId` からトレーニングデータセットを取得します。

```PYTHON
def load(config_properties):
    print("Training Data Load Start")

    #########################################
    # Load Data
    #########################################    
    client_context = get_client_context(config_properties)
    
    dataset_reader = DatasetReader(client_context, config_properties['trainingDataSetId'])
    
    timeframe = config_properties.get("timeframe")
    tenant_id = config_properties.get("tenant_id")
```

>[!NOTE]
>
>「[ 設定ファイル ](#configuration-files)」の節で説明したように、`client_context` を使用してExperience Platformからデータにアクセスする場合は、次の設定パラメーターが設定されます。
> - `ML_FRAMEWORK_IMS_USER_CLIENT_ID`
> - `ML_FRAMEWORK_IMS_TOKEN`
> - `ML_FRAMEWORK_IMS_ML_TOKEN`
> - `ML_FRAMEWORK_IMS_TENANT_ID`


データが揃ったら、データの準備と特徴のエンジニアリングから始めます。

### データの準備と特徴量エンジニアリング {#data-preparation-and-feature-engineering}

データは読み込み後に準備されて、`train` データセットと `val` データセットに分割されます。サンプルコードを以下に示します。

```PYTHON
#########################################
# Data Preparation/Feature Engineering
#########################################
dataframe.date = pd.to_datetime(dataframe.date)
dataframe['week'] = dataframe.date.dt.week
dataframe['year'] = dataframe.date.dt.year

dataframe = pd.concat([dataframe, pd.get_dummies(dataframe['storeType'])], axis=1)
dataframe.drop('storeType', axis=1, inplace=True)
dataframe['isHoliday'] = dataframe['isHoliday'].astype(int)

dataframe['weeklySalesAhead'] = dataframe.shift(-45)['weeklySales']
dataframe['weeklySalesLag'] = dataframe.shift(45)['weeklySales']
dataframe['weeklySalesDiff'] = (dataframe['weeklySales'] - dataframe['weeklySalesLag']) / dataframe['weeklySalesLag']
dataframe.dropna(0, inplace=True)

dataframe = dataframe.set_index(dataframe.date)
dataframe.drop('date', axis=1, inplace=True) 
```

この例では、元のデータセットに対して次の 5 つの処理が実行されています。
- `week` 列と `year` 列を追加する
- `storeType` を指数変数に変換する
- `isHoliday` を数値変数に変換する
- 将来の売上高と過去の売上高を得るために `weeklySales` をオフセットする
- 日付別にデータを `train` と `val` データセットに分割する

まず、`week` 列と `year` 列を作成し、元の `date` 列を [!DNL Python] [datetime](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.to_datetime.html) に変換します。 週と年の値は、datetime オブジェクトから抽出されます。

次に、`storeType` が 3 つの異なる店舗タイプ（`A`、`B`、`C`）を表す 3 つの列に変換されます。それぞれにはどちらの `storeType` が true かを示すブール値が含まれます。`storeType` 列が削除されます。

同様に、`weeklySales` が `isHoliday` ブール値を数値表現（1 または 0）に変更します。

このデータは、`train` と `val` データセットの間で分割されます。

`load()` 関数は `train` と `val` データセットを出力として完了する必要があります。

### スコアリングデータローダー {#scoring-data-loader}

スコアリングのデータを読み込む手順は、`split()` 関数でトレーニングデータを読み込む手順と似ています。データアクセス SDK を使用して、`recipe.conf` ファイルにある `scoringDataSetId` からデータを読み込みます。

```PYTHON
def load(config_properties):

    print("Scoring Data Load Start")

    #########################################
    # Load Data
    #########################################
    client_context = get_client_context(config_properties)

    dataset_reader = DatasetReader(client_context, config_properties['scoringDataSetId'])
    timeframe = config_properties.get("timeframe")
    tenant_id = config_properties.get("tenant_id")
```

データの読み込み後、データの準備と特徴量エンジニアリングがおこなわれます。

```PYTHON
    #########################################
    # Data Preparation/Feature Engineering
    #########################################
    if '_id' in dataframe.columns:
        #Rename columns to strip tenantId
        dataframe = dataframe.rename(columns = lambda x : str(x)[str(x).find('.')+1:])
        #Drop id, eventType and timestamp
        dataframe.drop(['_id', 'eventType', 'timestamp'], axis=1, inplace=True)

    dataframe.date = pd.to_datetime(dataframe.date)
    dataframe['week'] = dataframe.date.dt.week
    dataframe['year'] = dataframe.date.dt.year

    dataframe = pd.concat([dataframe, pd.get_dummies(dataframe['storeType'])], axis=1)
    dataframe.drop('storeType', axis=1, inplace=True)
    dataframe['isHoliday'] = dataframe['isHoliday'].astype(int)

    dataframe['weeklySalesAhead'] = dataframe.shift(-45)['weeklySales']
    dataframe['weeklySalesLag'] = dataframe.shift(45)['weeklySales']
    dataframe['weeklySalesDiff'] = (dataframe['weeklySales'] - dataframe['weeklySalesLag']) / dataframe['weeklySalesLag']
    dataframe.dropna(0, inplace=True)

    dataframe = dataframe.set_index(dataframe.date)
    dataframe.drop('date', axis=1, inplace=True)

    print("Scoring Data Load Finish")

    return dataframe
```

このモデルの目的は将来の毎週の売上を予測することなので、モデルの予測がどれだけうまく機能するかを評価するために使用されるスコアリングデータセットを作成する必要があります。

この Recipe Builder ノートブックは、週間売り上げを前に 7 日間オフセットすることでこれを実現しています。毎週 45 店舗の測定値があるので、`weeklySales` 値を 45 データセット分、前にシフトして、新しい列（`weeklySalesAhead`）に転送できます。

```PYTHON
df['weeklySalesAhead'] = df.shift(-45)['weeklySales']
```

同様に、`weeklySalesLag` 列を後ろに 45 シフトして作成できます。これを使用して、週別の売上高の差を計算し、`weeklySalesDiff` 列に格納することもできます。

```PYTHON
df['weeklySalesLag'] = df.shift(45)['weeklySales']
df['weeklySalesDiff'] = (df['weeklySales'] - df['weeklySalesLag']) / df['weeklySalesLag']
```

`weeklySales` データポイントを 45 データセット分前方にオフセットし、45 データセット分後方にオフセットして新しい列を作成するので、最初と最後の 45 データポイントには NaN 値が設定されます。これらのポイントは、NaN 値を持つすべての行を削除する `df.dropna()` 関数を使用してデータセットから削除できます。

```PYTHON
df.dropna(0, inplace=True)
```

スコアリングデータローダーの `load()` 関数は、スコアリングデータセットを出力として使用して完了する必要があります。

### パイプラインファイル {#pipeline-file}

`pipeline.py` ファイルには、トレーニングとスコアリングのロジックが含まれます。

### トレーニング {#training}

トレーニングの目的は、トレーニングデータセットの特徴とラベルを使用してモデルを作成することです。

>[!NOTE]
> 
>  特徴とは、機械学習モデルがラベルを予測するために使用する入力変数を指します。

`train()` 関数には、トレーニングモデルを含め、トレーニング済みモデルを返す必要があります。様々なモデルの例は、[scikit-learn ユーザーガイドドキュメント](https://scikit-learn.org/stable/user_guide.html)に記載されています。

トレーニングモデルを選択したら、x および y トレーニングデータセットをモデルに適合させ、トレーニング済みモデルが返されます。例を次に示します。

```PYTHON
def train(configProperties, data):

    print("Train Start")

    #########################################
    # Extract fields from configProperties
    #########################################
    learning_rate = float(configProperties['learning_rate'])
    n_estimators = int(configProperties['n_estimators'])
    max_depth = int(configProperties['max_depth'])


    #########################################
    # Fit model
    #########################################
    X_train = data.drop('weeklySalesAhead', axis=1).values
    y_train = data['weeklySalesAhead'].values

    seed = 1234
    model = GradientBoostingRegressor(learning_rate=learning_rate,
                                      n_estimators=n_estimators,
                                      max_depth=max_depth,
                                      random_state=seed)

    model.fit(X_train, y_train)

    print("Train Complete")

    return model
```

アプリケーションに応じて、`GradientBoostingRegressor()` 関数に引数が含まれることに注意してください。`xTrainingDataset` には、トレーニングに使用する特徴を含める必要がありますが、`yTrainingDataset` にはラベルを含める必要があります。

### スコアリング {#scoring}

`score()` 関数には、スコアリングアルゴリズムを含め、モデルの成功度を示す測定値を返す必要があります。`score()` 関数は、スコアリングデータセットラベルとトレーニング済みモデルを使用して、予測された特徴のセットを生成します。次に、これらの予測値が、スコアリングデータセットの実際の特徴と比較されます。この例では、`score()` 関数は、トレーニング済みモデルを使用して、スコアリングデータセットのラベルを使用して特徴を予測します。予測された特徴が返されます。

```PYTHON
def score(configProperties, data, model):

    print("Score Start")

    X_test = data.drop('weeklySalesAhead', axis=1).values
    y_test = data['weeklySalesAhead'].values
    y_pred = model.predict(X_test)

    data['prediction'] = y_pred
    data = data[['store', 'prediction']].reset_index()
    data['date'] = data['date'].astype(str)

    print("Score Complete")

    return data
```

### 評価ファイル {#evaluator-file}

`evaluator.py` ファイルには、トレーニングレシピの評価方法とトレーニングデータの分割方法に関するロジックが含まれています。小売販売の例では、トレーニングデータの読み込みと準備のロジックが含まれます。以下の 2 つの節で説明します。

### データセットの分割 {#split-the-dataset}

トレーニングのデータ準備段階では、トレーニングとテストに使用するデータセットを分割する必要があります。この `val` データは、トレーニング後にモデルを評価するために暗黙的に使用されます。このプロセスはスコアリングとは別のものです。

この節では、まずデータをノートブックに読み込み、次にデータセット内の関連のない列を削除してデータをクリーンアップする `split()` 関数を示します。ここから、データ内の既存の生の特徴から関連する特徴を追加で作成するプロセスである特徴量エンジニアリングを実行できます。このプロセスの例を以下に説明と共に示します。

`split()` 関数を次に示します。引数で指定されたデータフレームは、`train` 変数と `val` 変数に分割されて返されます。

```PYTHON
def split(self, configProperties={}, dataframe=None):
    train_start = '2010-02-12'
    train_end = '2012-01-27'
    val_start = '2012-02-03'
    train = dataframe[train_start:train_end]
    val = dataframe[val_start:]

    return train, val
```

### トレーニング済みモデルの評価 {#evaluate-the-trained-model}

`evaluate()` 関数は、モデルのトレーニングが終わると実行され、モデルの成功度を示す指標を返します。`evaluate()` 関数は、テストデータセットラベルとトレーニング済みモデルを使用して、一連の特徴を予測します。これらの予測値は、テストデータセットの実際の特徴と比較されます。一般的なスコアリングアルゴリズムには、次のものがあります。
- [平均絶対誤差率（MAPE）](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
- [平均絶対誤差（MAE）](https://en.wikipedia.org/wiki/Mean_absolute_error)
- [平均平方根誤差（RMSE）](https://en.wikipedia.org/wiki/Root-mean-square_deviation)


小売売上サンプルの `evaluate()` 関数を次に示します。

```PYTHON
def evaluate(self, data=[], model={}, configProperties={}):
    print ("Evaluation evaluate triggered")
    val = data.drop('weeklySalesAhead', axis=1)
    y_pred = model.predict(val)
    y_actual = data['weeklySalesAhead'].values
    mape = np.mean(np.abs((y_actual - y_pred) / y_actual))
    mae = np.mean(np.abs(y_actual - y_pred))
    rmse = np.sqrt(np.mean((y_actual - y_pred) ** 2))

    metric = [{"name": "MAPE", "value": mape, "valueType": "double"},
                {"name": "MAE", "value": mae, "valueType": "double"},
                {"name": "RMSE", "value": rmse, "valueType": "double"}]

    return metric
```

この関数は、評価指標の配列を含む `metric` オブジェクトを返します。これらの指標は、トレーニング済みモデルのパフォーマンスを評価するために使用されます。

### データセーバーファイル {#data-saver-file}

`datasaver.py` ファイルには、スコアリングのテスト中に予測を保存する `save()` 関数が含まれています。`save()` 関数は予測を受け取り、[!DNL Experience Platform Catalog] API を使用して、`scoring.conf` ファイルで指定した `scoringResultsDataSetId` にデータを書き込みます。

小売販売のサンプルレシピでの使用例を次に示します。Platform へのデータ書き込みにおける `DataSetWriter` ライブラリの使用に注意してください。

```PYTHON
from data_access_sdk_python.writer import DataSetWriter

def save(configProperties, prediction):
    print("Datasaver Start")
    print("Setting up Writer")

    catalog_url = "https://platform.adobe.io/data/foundation/catalog"
    ingestion_url = "https://platform.adobe.io/data/foundation/import"

    writer = DataSetWriter(catalog_url=catalog_url,
                           ingestion_url=ingestion_url,
                           client_id=configProperties['ML_FRAMEWORK_IMS_USER_CLIENT_ID'],
                           user_token=configProperties['ML_FRAMEWORK_IMS_TOKEN'],
                           service_token=configProperties['ML_FRAMEWORK_IMS_ML_TOKEN'])

    print("Writer Configured")

    writer.write(data_set_id=configProperties['scoringResultsDataSetId'],
                 dataframe=prediction,
                 ims_org=configProperties['ML_FRAMEWORK_IMS_TENANT_ID'])

    print("Write Done")
    print("Datasaver Finish")
    print(prediction)
```

## トレーニングとスコアリング {#training-and-scoring}

ノートブックの変更が完了し、レシピのトレーニングをおこなう場合は、バーの上部にある関連ボタンをクリックして、セル内にトレーニングランを作成できます。ボタンをクリックすると、トレーニングスクリプトのコマンドと出力のログがノートブック（`evaluator.py` セルの下）に表示されます。Conda は、最初にすべての依存関係をインストールし、その後トレーニングを開始します。

スコアリングを実行する前に、少なくとも 1 回はトレーニングを実行する必要があります。「**[!UICONTROL スコアリングを実行]**」ボタンをクリックすると、トレーニング中に生成されたトレーニング対象モデルに対してスコアが付けられます。スコアリングスクリプトが `datasaver.py` の下に表示されます。

デバッグの目的で、非表示の出力を表示する場合は、出力セルの末尾に `debug` を追加し、再実行します。

## レシピの作成 {#create-recipe}

レシピの編集が完了し、トレーニング/スコアリングの出力に満足したら、右上のナビゲーションの「 **[!UICONTROL レシピの作成]** 」を押して、ノートブックからレシピを作成できます。

![](../images/jupyterlab/create-recipe/create-recipe.png)

ボタンを押すと、レシピ名の入力を求めるプロンプトが表示されます。 この名前は、[!DNL Platform] で作成された実際のレシピを表します。

![](../images/jupyterlab/create-recipe/enter_recipe_name.png)

「**[!UICONTROL OK]**」を押すと、[Adobe Experience Platform ](https://platform.adobe.com/)で新しいレシピに移動できます。「**[!UICONTROL レシピを表示]**」ボタンをクリックすると、「**[!UICONTROL ML モデル]**」の下の「**[!UICONTROL レシピ]**」タブに移動できます。

![](../images/jupyterlab/create-recipe/recipe_creation_started.png)

処理が完了すると、レシピは次のようになります。

![](../images/jupyterlab/create-recipe/recipe_details.png)

>[!CAUTION]
>
> - ファイルのセルを削除しないでください
> - ファイルのセルの先頭の `%%writefile` 行を編集しないでください
> - 異なるノートブックに同時にレシピを作成しないでください


## 次の手順 {#next-steps}

このチュートリアルでは、Recipe Builder ノートブックで機械学習モデルを作成する方法を学習しました。また、ノートブック内のレシピワークフローに従ってノートブックを使用し、[!DNL Data Science Workspace] 内にレシピを作成する方法も学習しました。

[!DNL Data Science Workspace] 内でリソースを使用する方法を引き続き学習するには、[!DNL Data Science Workspace] のレシピとモデルのドロップダウンを参照してください。

## その他のリソース {#additional-resources}

次のビデオは、モデルの構築とデプロイに関する理解を深めるためのものです。

>[!VIDEO](https://video.tv.adobe.com/v/30575?quality=12&enable10seconds=on&speedcontrol=on)
