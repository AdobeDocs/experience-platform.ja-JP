---
keywords: Experience Platform;Tutorial;feature pipeline;Data Science Workspace;popular topics
title: フィーチャパイプラインの作成
topic: tutorial
type: Tutorial
description: Adobe Experience Platformでは、カスタムフィーチャパイプラインを作成して作成し、Senei Machine Learning Framework Runtimeを通じて尺度を変更してフィーチャエンジニアリングを実行できます。 このドキュメントでは、フィーチャパイプラインにある各種クラスについて説明し、PySparkのModel Authoring SDKを使用してカスタムフィーチャパイプラインを作成するための手順を説明します。
translation-type: tm+mt
source-git-commit: 8c94d3631296c1c3cc97501ccf1a3ed995ec3cab
workflow-type: tm+mt
source-wordcount: '1421'
ht-degree: 28%

---


# フィーチャパイプラインの作成

>[!IMPORTANT]
>
> 機能のパイプラインは現在、APIからのみ使用できます。

Adobe Experience Platformでは、Sensei Machine Learning Framework Runtime（以下「Runtime」と呼ぶ）を使用して、カスタムフィーチャパイプラインを作成し、スケールでフィーチャエンジニアリングを実行できます。

This document describes the various classes found in a feature pipeline, and provides a step-by-step tutorial for creating a custom feature pipeline using the [Model Authoring SDK](./sdk.md) in PySpark.

フィーチャパイプラインを実行すると、次のワークフローが実行されます。

1. レシピはデータセットをパイプラインに読み込みます。
2. データセット上で機能の変換が行われ、Adobe Experience Platformに書き戻されます。
3. 変換されたデータは、トレーニング用に読み込まれます。
4. フィーチャーパイプラインは、選択したモデルとしてグラデーション倍力回帰を使用してステージを定義します。
5. パイプラインはトレーニングデータのフィットに使用され、トレーニングされたモデルが作成されます。
6. モデルがスコアリングデータセットと共に変換されます。
7. 出力の対象となる列が選択され、関連するデータ [!DNL Experience Platform] と共に保存されます。

## はじめに

任意の組織でレシピを実行するには、次の手順が必要です。
- 入力データセット。
- データセットのスキーマ。
- 変換後のスキーマと、そのスキーマに基づく空のデータセット。
- 出力スキーマと、そのスキーマに基づく空のデータセット。

上記のデータセットはすべて、 [!DNL Platform] UIにアップロードする必要があります。 この設定を行うには、Adobeが提供する [ブートストラップスクリプトを使用し](https://github.com/adobe/experience-platform-dsw-reference/tree/master/bootstrap)ます。

## フィーチャパイプラインクラス

次の表に、フィーチャパイプラインを構築するために拡張する必要がある主な抽象クラスを示します。

| 抽象クラス | 説明 |
| -------------- | ----------- |
| DataLoader | DataLoader クラスは、入力データを取得するための実装を提供します。 |
| DatasetTransformer | DatasetTransformer クラスは、入力データセットを変換するための実装を提供します。DatasetTransformer クラスを指定する代わりに、FeaturePipelineFactory クラス内に機能エンジニアリングロジックを実装することもできます。 |
| FeaturePipelineFactory | FeaturePipelineFactory クラスは、一連の Spark Transformer から成り、機能エンジニアリングを実施する Spark パイプラインを構築します。FeaturePipelineFactory クラスを指定する代わりに、DatasetTransformer クラス内に機能エンジニアリングロジックを実装することもできます。 |
| DataSaver | DataSaver クラスは、機能データセットのストレージのロジックを提供します。 |

Feature Pipelineジョブが開始されると、Runtimeは最初にDataLoaderを実行して入力データをDataFrameとして読み込み、次にDatasetTransformer、FeaturePipelineFactory、またはその両方を実行してDataFrameを変更します。 最後に、生成された機能データセットが DataSaver を通して保存されます。

次のフローチャートに、Runtime の実行順序を示します。

![](../images/authoring/feature-pipeline/FeaturePipeline_Runtime_flow.png)


## 機能パイプラインのクラスを実装する {#implement-your-feature-pipeline-classes}

以下の節では、機能パイプラインに必要なクラスの説明とその実装例を示します。

### 設定 JSON ファイルで変数を定義する {#define-variables-in-the-configuration-json-file}

設定 JSON ファイルはキーと値のペアで構成され、後から実行時に定義する変数を指定することを目的としています。これらのキーと値のペアでは、入力データセットの場所、出力データセット ID、テナント ID、列ヘッダーなどのプロパティを定義できます。

次の例は、設定ファイル内にあるキーと値のペアを示しています。

**設定JSONの例**

```json
[
    {
        "name": "fp",
        "parameters": [
            {
                "key": "dataset_id",
                "value": "000"
            },
            {
                "key": "featureDatasetId",
                "value": "111"
            },
            {
                "key": "tenantId",
                "value": "_tenantid"
            }
        ]
    }
]
```

設定 JSON にアクセスするには、任意のクラスメソッドで `config_properties` をパラメーターとして定義します。以下に例を示します。

**PySpark**

```python
dataset_id = str(config_properties.get(dataset_id))
```

詳細な設定例については、Data Science Workspaceが提供する [pipeline.json](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/feature_pipeline_recipes/pyspark/pipeline.json) ファイルを参照してください。

### DataLoader を使用して入力データを準備する {#prepare-the-input-data-with-dataloader}

DataLoader では、入力データの取得とフィルタリングをおこないます。DataLoader の実装では、抽象クラス `DataLoader` を拡張し、抽象メソッド `load` をオーバーライドする必要があります。

The following example retrieves a [!DNL Platform] dataset by ID and returns it as a DataFrame, where the dataset ID (`dataset_id`) is a defined property in the configuration file.

**PySpark の例**

```python
# PySpark

from pyspark.sql.types import StringType, TimestampType
from pyspark.sql.functions import col, lit, struct
import logging

class MyDataLoader(DataLoader):
    def load_dataset(config_properties, spark, tenant_id, dataset_id):
    PLATFORM_SDK_PQS_PACKAGE = "com.adobe.platform.query"
    PLATFORM_SDK_PQS_INTERACTIVE = "interactive"

    service_token = str(spark.sparkContext.getConf().get("ML_FRAMEWORK_IMS_ML_TOKEN"))
    user_token = str(spark.sparkContext.getConf().get("ML_FRAMEWORK_IMS_TOKEN"))
    org_id = str(spark.sparkContext.getConf().get("ML_FRAMEWORK_IMS_ORG_ID"))
    api_key = str(spark.sparkContext.getConf().get("ML_FRAMEWORK_IMS_CLIENT_ID"))

    dataset_id = str(config_properties.get(dataset_id))

    for arg in ['service_token', 'user_token', 'org_id', 'dataset_id', 'api_key']:
        if eval(arg) == 'None':
            raise ValueError("%s is empty" % arg)

    query_options = get_query_options(spark.sparkContext)

    pd = spark.read.format(PLATFORM_SDK_PQS_PACKAGE) \
        .option(query_options.userToken(), user_token) \
        .option(query_options.serviceToken(), service_token) \
        .option(query_options.imsOrg(), org_id) \
        .option(query_options.apiKey(), api_key) \
        .option(query_options.mode(), PLATFORM_SDK_PQS_INTERACTIVE) \
        .option(query_options.datasetId(), dataset_id) \
        .load()
    pd.show()

    # Get the distinct values of the dataframe
    pd = pd.distinct()

    # Flatten the data
    if tenant_id in pd.columns:
        pd = pd.select(col(tenant_id + ".*"))

    return pd
```

### DatasetTransformer を使用してデータセットを変換する {#transform-a-dataset-with-datasettransformer}

DatasetTransformer は、入力 DataFrame を変換するロジックを提供し、新しい派生 DataFrame を返します。このクラスは、FeaturePipelineFactory と協働するように、または唯一の機能エンジニアリングコンポーネントとして動作するように実装できます。また、このクラスを実装しないことも可能です。

次の例は、DatasetTransformerクラスを拡張したものです。


**PySpark の例**

```python
# PySpark

from sdk.dataset_transformer import DatasetTransformer
from pyspark.ml.feature import StringIndexer
from pyspark.sql.types import IntegerType
from pyspark.sql.functions import unix_timestamp, from_unixtime, to_date, lit, lag, udf, date_format, lower, col, split, explode
from pyspark.sql import Window
from .helper import setupLogger

class MyDatasetTransformer(DatasetTransformer):
    logger = setupLogger(__name__)

    def transform(self, config_properties, dataset):
        tenant_id = str(config_properties.get("tenantId"))

        # Flatten the data
        if tenant_id in dataset.columns:
            self.logger.info("Flatten the data before transformation")
            dataset = dataset.select(col(tenant_id + ".*"))
            dataset.show()

        # Convert isHoliday boolean value to Int
        # Rename the column to holiday and drop isHoliday
        pd = dataset.withColumn("holiday", col("isHoliday").cast(IntegerType())).drop("isHoliday")
        pd.show()

        # Get the week and year from date
        pd = pd.withColumn("week", date_format(to_date("date", "MM/dd/yy"), "w").cast(IntegerType()))
        pd = pd.withColumn("year", date_format(to_date("date", "MM/dd/yy"), "Y").cast(IntegerType()))

        # Convert the date to TimestampType
        pd = pd.withColumn("date", to_date(unix_timestamp(pd["date"], "MM/dd/yy").cast("timestamp")))

        # Convert categorical data
        indexer = StringIndexer(inputCol="storeType", outputCol="storeTypeIndex")
        pd = indexer.fit(pd).transform(pd)

        # Get the WeeklySalesAhead and WeeklySalesLag column values
        window = Window.orderBy("date").partitionBy("store")
        pd = pd.withColumn("weeklySalesLag", lag("weeklySales", 1).over(window)).na.drop(subset=["weeklySalesLag"])
        pd = pd.withColumn("weeklySalesAhead", lag("weeklySales", -1).over(window)).na.drop(subset=["weeklySalesAhead"])
        pd = pd.withColumn("weeklySalesScaled", lag("weeklySalesAhead", -1).over(window)).na.drop(subset=["weeklySalesScaled"])
        pd = pd.withColumn("weeklySalesDiff", (pd['weeklySales'] - pd['weeklySalesLag'])/pd['weeklySalesLag'])

        pd = pd.na.drop()
        self.logger.debug("Transformed dataset count is %s " % pd.count())

        # return transformed dataframe
        return pd
```

### FeaturePipelineFactory を使用してデータ機能を開発する {#engineer-data-features-with-featurepipelinefactory}

FeaturePipelineFactory を使用すると、機能エンジニアリングロジックを実装できます。そのためには、Spark パイプラインを通じて、一連の Spark Transformer を定義および連結します。このクラスは、DatasetTransformer と協働するように、または唯一の機能エンジニアリングコンポーネントとして動作するように実装できます。また、このクラスを実装しないことも可能です。

次の例は、FeaturePipelineFactoryクラスを拡張します。

**PySpark の例**

```python
# PySpark

from pyspark.ml import Pipeline
from pyspark.ml.regression import GBTRegressor
from pyspark.ml.feature import VectorAssembler

import numpy as np

from sdk.pipeline_factory import PipelineFactory

class MyFeaturePipelineFactory(FeaturePipelineFactory):

    def apply(self, config_properties):
        if config_properties is None:
            raise ValueError("config_properties parameter is null")

        tenant_id = str(config_properties.get("tenantId"))
        input_features = str(config_properties.get("ACP_DSW_INPUT_FEATURES"))

        if input_features is None:
            raise ValueError("input_features parameter is null")
        if input_features.startswith(tenant_id):
            input_features = input_features.replace(tenant_id + ".", "")

        learning_rate = float(config_properties.get("learning_rate"))
        n_estimators = int(config_properties.get("n_estimators"))
        max_depth = int(config_properties.get("max_depth"))

        feature_list = list(input_features.split(","))
        feature_list.remove("date")
        feature_list.remove("storeType")

        cols = np.array(feature_list)

        # Gradient-boosted tree estimator
        gbt = GBTRegressor(featuresCol='features', labelCol='weeklySalesAhead', predictionCol='prediction',
                       maxDepth=max_depth, maxBins=n_estimators, stepSize=learning_rate)

        # Assemble the fields to a vector
        assembler = VectorAssembler(inputCols=cols, outputCol="features")

        # Construct the pipeline
        pipeline = Pipeline(stages=[assembler, gbt])

        return pipeline

    def train(self, config_properties, dataframe):
        pass

    def score(self, config_properties, dataframe, model):
        pass

    def getParamMap(self, config_properties, sparkSession):
        return None
```

### DataSaver を使用して機能データセットを保存する {#store-your-feature-dataset-with-datasaver}

DataSaverは、結果の機能データセットをストレージの場所に保存する役割を持ちます。 DataSaver の実装では、抽象クラス `DataSaver` を拡張し、抽象メソッド `save` をオーバーライドする必要があります。

The following example extends the DataSaver class which stores data to a [!DNL Platform] dataset by ID, where the dataset ID (`featureDatasetId`) and tenant ID (`tenantId`) are defined properties in the configuration.

**PySpark の例**

```python
# PySpark

from sdk.data_saver import DataSaver
from pyspark.sql.types import StringType, TimestampType
from pyspark.sql.functions import col, lit, struct


class MyDataSaver(DataSaver):
    def save(self, configProperties, data_feature):

        # Spark context
        sparkContext = data_features._sc

        # preliminary checks
        if configProperties is None:
            raise ValueError("configProperties parameter is null")
        if data_features is None:
            raise ValueError("data_features parameter is null")
        if sparkContext is None:
            raise ValueError("sparkContext parameter is null")

        # prepare variables
        timestamp = "2019-01-01 00:00:00"
        output_dataset_id = str(
            configProperties.get("featureDatasetId"))
        tenant_id = str(
            configProperties.get("tenantId"))
        service_token = str(
            sparkContext.getConf().get("ML_FRAMEWORK_IMS_ML_TOKEN"))
        user_token = str(
            sparkContext.getConf().get("ML_FRAMEWORK_IMS_TOKEN"))
        org_id = str(
            sparkContext.getConf().get("ML_FRAMEWORK_IMS_ORG_ID"))
        api_key = str(
            sparkContext.getConf().get("ML_FRAMEWORK_IMS_CLIENT_ID"))

        # validate variables
        for arg in ['output_dataset_id', 'tenant_id', 'service_token', 'user_token', 'org_id', 'api_key']:
            if eval(arg) == 'None':
                raise ValueError("%s is empty" % arg)

        # create and prepare DataFrame with valid columns
        output_df = data_features.withColumn("date", col("date").cast(StringType()))
        output_df = output_df.withColumn(tenant_id, struct(col("date"), col("store"), col("features")))
        output_df = output_df.withColumn("timestamp", lit(timestamp).cast(TimestampType()))
        output_df = output_df.withColumn("_id", lit("empty"))
        output_df = output_df.withColumn("eventType", lit("empty"))

        # store data into dataset
        output_df.select(tenant_id, "_id", "eventType", "timestamp") \
            .write.format("com.adobe.platform.dataset") \
            .option('orgId', org_id) \
            .option('serviceToken', service_token) \
            .option('userToken', user_token) \
            .option('serviceApiKey', api_key) \
            .save(output_dataset_id)
```


### アプリケーションファイルで実装済みクラス名を指定する {#specify-your-implemented-class-names-in-the-application-file}

フィーチャパイプラインクラスが定義され、実装されたら、クラスの名前をアプリケーションのYAMLファイルに指定する必要があります。

次の例では、実装されるクラス名を示します。

**PySpark の例**

```yaml
#Name of the class which contains implementation to get the input data.
feature.dataLoader: InputDataLoaderForFeaturePipeline

#Name of the class which contains implementation to get the transformed data.
feature.dataset.transformer: MyDatasetTransformer

#Name of the class which contains implementation to save the transformed data.
feature.dataSaver: DatasetSaverForTransformedData

#Name of the class which contains implementation to get the training data
training.dataLoader: TrainingDataLoader

#Name of the class which contains pipeline. It should implement PipelineFactory.scala
pipeline.class: TrainPipeline

#Name of the class which contains implementation for evaluation metrics.
evaluator: Evaluator
evaluateModel: True

#Name of the class which contains implementation to get the scoring data.
scoring.dataLoader: ScoringDataLoader

#Name of the class which contains implementation to save the scoring data.
scoring.dataSaver: MyDatasetSaver
```

## Create your feature pipeline Engine using the API {#create-feature-pipeline-engine-api}

フィーチャーパイプラインを作成したら、Dockerイメージを作成して、 [!DNL Sensei Machine Learning] APIのフィーチャーパイプラインエンドポイントを呼び出す必要があります。 フィーチャパイプラインエンドポイントを呼び出すには、DockerイメージURLが必要です。

>[!TIP]
>
>Docker URLがない場合は、 [Package source files into a recipe](../models-recipes/package-source-files-recipe.md) tutorialを参照し、DockerホストURLの作成に関する手順を説明します。

オプションで、次のPostmanコレクションを使用して、機能のパイプラインAPIワークフローの完了を支援することもできます。

https://www.postman.com/collections/c5fc0d1d5805a5ddd41a

### フィーチャパイプラインエンジンを作成する {#create-engine-api}

Dockerイメージの場所を特定したら、に対するPOSTを実行して [、](../api/engines.md#feature-pipeline-docker) APIを使用してフィーチャパイプラインエンジンを [!DNL Sensei Machine Learning] 作成でき `/engines`ます。 フィーチャパイプラインエンジンの作成に成功すると、エンジン固有の識別子(`id`)が提供されます。 続行する前に、この値を保存してください。

### MLInstance の作成 {#create-mlinstance}

新しく作成したMLIstanceを使用し `engineID`て、エンドポイントにPOSTリクエストを作成し [、MLIstanceを](../api/mlinstances.md#create-an-mlinstance)`/mlInstance` 作成する必要があります。 A successful response returns a payload containing the details of the newly created MLInstance including its unique identifier (`id`) used in the next API call.

### Experiment の作成 {#create-experiment}

次に、テストを [作成する必要があります](../api/experiments.md#create-an-experiment)。 テストを作成するには、MLIstance unique identifier(`id`)を持ち、エンドポイントにPOSTリクエストを行う必要があり `/experiment` ます。 A successful response returns a payload containing the details of the newly created Experiment including its unique identifier (`id`) used in the next API call.

### テスト実行機能のパイプラインタスクを指定 {#specify-feature-pipeline-task}

テストを作成したら、テストのモードをに変更する必要があり `featurePipeline`ます。 モードを変更するには、をに追加POSTし [`experiments/{EXPERIMENT_ID}/runs`](../api/experiments.md#experiment-training-scoring) 、ボディ送信でフィーチャーパイプラインのテスト実行 `EXPERIMENT_ID``{ "mode":"featurePipeline"}` を指定します。

完了したら、テストのステータス `/experiments/{EXPERIMENT_ID}` を [取得するGETリクエストを作成し](../api/experiments.md#retrieve-specific) 、テストのステータスが更新されて完了するのを待ちます。

### テストの実行のトレーニングタスクを指定します {#training}

次に、トレーニングの実行タスクを [指定する必要があります](../api/experiments.md#experiment-training-scoring)。 本文にPOST `experiments/{EXPERIMENT_ID}/runs` を行い、本文にモードを設定して、トレーニングパラメータを含む一連のタスク `train` を送信します。 成功応答は、要求された実験の詳細を含むペイロードを返します。

完了したら、テストのステータス `/experiments/{EXPERIMENT_ID}` を [取得するGETリクエストを作成し](../api/experiments.md#retrieve-specific) 、テストのステータスが更新されて完了するのを待ちます。

### テストの実行スコアタスクの指定 {#scoring}

>[!NOTE]
>
> この手順を完了するには、テストに関連したトレーニングを1回以上実行する必要があります。

トレーニングの実行が成功したら、スコアリングの実行タスクを [指定する必要があります](../api/experiments.md#experiment-training-scoring)。 本文にPOST `experiments/{EXPERIMENT_ID}/runs` を行い、本文で `mode` 属性を「スコア」に設定します。 この開始は、スコアリングテストの実行です。

完了したら、テストのステータス `/experiments/{EXPERIMENT_ID}` を [取得するGETリクエストを作成し](../api/experiments.md#retrieve-specific) 、テストのステータスが更新されて完了するのを待ちます。

スコアリングが完了したら、機能のパイプラインが動作する必要があります。

## 次の手順 {#next-steps}

[//]: # (Next steps section should refer to tutorials on how to score data using the feature pipeline Engine. Update this document once those tutorials are available)

このドキュメントを読むと、Model Authoring SDKを使用してフィーチャパイプラインを作成し、Dockerイメージを作成し、DockerイメージURLを使用して、 [!DNL Sensei Machine Learning] APIを使用してフィーチャパイプラインモデルを作成します。 これで、を使用して、データセットの変換とデータ機能の抽出をスケール設定で続行する準備が整い [[!DNL Sensei Machine Learning API]](../api/getting-started.md)ました。
