---
keywords: Experience Platform;Tutorial;Feature Pipeline;Data Science Workspace;popular topics
solution: Experience Platform
title: フィーチャパイプラインの作成
topic: Tutorial
translation-type: tm+mt
source-git-commit: b9b0578a43182650b3cfbd71f46bcb817b3b0cda

---


# フィーチャパイプラインの作成

Adobe Experience Platformを使用すると、カスタム機能パイプラインを作成して、Sensei Machine Learning Framework Runtime（以下「Runtime」と呼ばれる）を通じてスケールで機能エンジニアリングを実行できます。

このドキュメントでは、機能パイプラインに含まれる様々なクラスについて説明し、PySparkおよびSparkの [Model Authoring SDK](./sdk.md) （モデルオーサリングSDK）を使用してカスタム機能パイプラインを作成する手順を説明します。

このチュートリアルでは、次の手順について説明します。
- [フィーチャパイプラインクラスの実装](#implement-your-feature-pipeline-classes)
   - [設定ファイルでの変数の定義](#define-variables-in-the-configuration-json-file)
   - [DataLoaderを使用した入力データの準備](#prepare-the-input-data-with-dataloader)
   - [DatasetTransformerを使用したデータセットの変換](#transform-a-dataset-with-datasettransformer)
   - [FeaturePipelineFactoryを使用したデータ機能のエンジニアリング](#engineer-data-features-with-featurepipelinefactory)
   - [機能データセットをDataSaverに保存する](#store-your-feature-dataset-with-datasaver)
   - [実装したクラス名をアプリケーションファイルで指定します](#specify-your-implemented-class-names-in-the-application-file)
- [バイナリアーティファクトの作成](#build-the-binary-artifact)
- [APIを使用したフィーチャーパイプラインエンジンの作成](#create-a-feature-pipeline-engine-using-the-api)

## フィーチャパイプラインクラス

次の表に、フィーチャパイプラインを構築するために拡張する必要がある主要な抽象クラスを示します。

| 抽象クラス | 説明 |
| -------------- | ----------- |
| DataLoader | DataLoaderクラスは、入力データを取得するための実装を提供します。 |
| DatasetTransformer | DatasetTransformerクラスは、入力データセットを変換する実装を提供します。 代わりに、DatasetTransformerクラスを提供せず、FeaturePipelineFactoryクラス内にフィーチャエンジニアリングロジックを実装することもできます。 |
| FeaturePipelineFactory | FeaturePipelineFactoryクラスは、一連のSpark変圧器から成るSpark Pipelineを構築し、フィーチャエンジニアリングを実行します。 FeaturePipelineFactoryクラスを提供しないように選択し、代わりにDatasetTransformerクラス内にフィーチャエンジニアリングロジックを実装できます。 |
| DataSaver | DataSaverクラスは、機能データセットのストレージのロジックを提供します。 |

Feature Pipelineジョブが開始されると、RuntimeはまずDataLoaderを実行して入力データをDataFrameとして読み込み、次にDatasetTransformer、FeaturePipelineFactory、またはその両方を実行してDataFrameを変更します。 最後に、結果の機能データセットはDataSaverを通じて保存されます。

次のフローチャートに、ランタイムの実行順序を示します。

![](../images/authoring/feature-pipeline/FeaturePipeline_Runtime_flow.png)


## フィーチャパイプラインクラスの実装

以下のセクションでは、フィーチャパイプラインに必要なクラスの実装の詳細と例を示します。

### 設定JSONファイルでの変数の定義

設定JSONファイルは、キーと値のペアで構成され、実行時に後で定義する変数を指定することを意図しています。 これらのキーと値のペアは、入力データセットの場所、出力データセットID、テナントID、列ヘッダーなどのプロパティを定義できます。

次の例は、設定ファイル内にあるキーと値のペアを示しています。 例を展開して詳細を表示します。


**設定JSONの例**

```json
[
    {
        "name": "fp",
        "parameters": [
            {
                "key": "datasetId",
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



設定JSONには、パラメーターとして定義された任意のクラスメソッドを通じて `configProperties` アクセスできます。 以下に例を示します。

**PySpark**

```python
input_dataset_id = str(configProperties.get("datasetId"))
```

**Spark**

```scala
val input_dataset_id: String = configProperties.get("datasetId")
```


### DataLoaderを使用した入力データの準備

DataLoaderは、入力データの取得とフィルタリングを行います。 DataLoaderの実装は、抽象クラスを拡張し、抽象メソッドをオ `DataLoader` ーバーライドする必要がありま `load`す。

次の例では、プラットフォームデータセットをIDで取得し、DataFrameとして返します。データセットID(`datasetId`)は設定ファイル内の定義済みプロパティです。 各例を展開して詳細を表示します。


**PySparkの例**

```python
# PySpark

from sdk.data_loader import DataLoader

class MyDataLoader(DataLoader):
    def load(self, configProperties, spark):

        # preliminary checks
        if configProperties is None :
            raise ValueError("configProperties parameter is null")
        if spark is None:
            raise ValueError("spark parameter is null")

        # prepare variables
        dataset_id = str(
            configProperties.get("datasetId"))
        service_token = str(
            spark.sparkContext.getConf().get("ML_FRAMEWORK_IMS_ML_TOKEN"))
        user_token = str(
            spark.sparkContext.getConf().get("ML_FRAMEWORK_IMS_TOKEN"))
        org_id = str(
            spark.sparkContext.getConf().get("ML_FRAMEWORK_IMS_ORG_ID"))
        api_key = str(
            spark.sparkContext.getConf().get("ML_FRAMEWORK_IMS_CLIENT_ID"))

        # validate variables
        for arg in ['dataset_id', 'service_token', 'user_token', 'org_id', 'api_key']:
            if eval(arg) == 'None':
                raise ValueError("%s is empty" % arg)

        # load dataset through Spark session
        df = spark.read.format("com.adobe.platform.dataset") \
            .option('serviceToken', service_token) \
            .option('userToken', user_token) \
            .option('orgId', org_id) \
            .option('serviceApiKey', api_key) \
            .load(dataset_id)

        # return as DataFrame
        return df
```




**Sparkの例**

```scala
// Spark

import com.adobe.platform.ml.config.ConfigProperties
import com.adobe.platform.ml.sdk.DataLoader
import org.apache.spark.sql.{DataFrame, SparkSession}

class MyDataLoader extends DataLoader {
    override def load(configProperties: ConfigProperties, sparkSession: SparkSession): DataFrame = {

        // preliminary checks
        require(configProperties != null)
        require(sparkSession != null)

        // prepare variables
        val dataSetId: String = configProperties
            .get("datasetId").getOrElse("")
        val serviceToken: String = sparkSession.sparkContext.getConf
            .get("ML_FRAMEWORK_IMS_ML_TOKEN", "").toString
        val userToken: String = sparkSession.sparkContext.getConf
            .get("ML_FRAMEWORK_IMS_TOKEN", "").toString
        val orgId: String = sparkSession.sparkContext.getConf
            .get("ML_FRAMEWORK_IMS_ORG_ID", "").toString
        val apiKey: String = sparkSession.sparkContext.getConf
            .get("ML_FRAMEWORK_IMS_CLIENT_ID", "").toString

        // validate variables
        List(dataSetId, serviceToken, userToken, orgId, apiKey).foreach(
            value => required(value != "")
        )

        // load dataset through Spark session
        var df = sparkSession.read.format("com.adobe.platform.dataset")
            .option(DataSetOptions.orgId, orgId)
            .option(DataSetOptions.serviceToken, serviceToken)
            .option(DataSetOptions.userToken, userToken)
            .option(DataSetOptions.serviceApiKey, apiKey)
            .load(dataSetId)
        
        // return as DataFrame
        df
    }
}
```



### DatasetTransformerを使用したデータセットの変換

DatasetTransformerは、入力DataFrameを変換するロジックを提供し、新しい派生DataFrameを返します。 このクラスは、FeaturePipelineFactoryと共に動作し、唯一のフィーチャエンジニアリングコンポーネントとして動作するように実装するか、このクラスを実装しないように選択できます。

次の例は、DatasetTransformerクラスを拡張したものです。 各例を展開して詳細を表示します。


**PySparkの例**

```python
# PySpark

from sdk.dataset_transformer import DatasetTransformer

class MyDatasetTransformer(DatasetTransformer):

    def transform(self, configProperties, dataset):
        transformed = dataset

        '''
        Transformations
        '''

        # return new DataFrame
        return transformed
```




**Sparkの例**

```scala
// Spark

import com.adobe.platform.ml.config.ConfigProperties
import com.adobe.platform.ml.sdk.DatasetTransformer

class MyDatasetTransformer extends DatasetTransformer {

    override def transform(configProperties: ConfigProperties, dataset: Dataset[_]): Dataset[_] = {
        val transformed = dataset

        /*
        transformations
        */
        
        // return new DataFrame
        transformed
    }
}
```



### FeaturePipelineFactoryを使用したデータ機能のエンジニアリング

FeaturePipelineFactoryを使用すると、Spark Pipelineを介して一連のSpark Transformerを定義し、連結することで、機能エンジニアリングロジックを実装できます。 このクラスは、DatasetTransformerと連携して動作する、唯一のフィーチャエンジニアリングコンポーネントとして動作する、またはこのクラスを実装しないように選択できます。

次の例は、FeaturePipelieFactoryクラスを拡張し、一連のSpark変圧器をSparkパイプラインの複数のステージとして実装します。 各例を展開して詳細を表示します。


**PySparkの例**

```python
# PySpark

from pyspark.ml import Pipeline
from pyspark.ml.feature import HashingTF, Tokenizer
from sdk.feature_pipeline_factory import FeaturePipelineFactory

class MyFeaturePipelineFactory(FeaturePipelineFactory):

    def create_pipeline(self, configProperties):

        # Spark Transformers
        tokenizer = Tokenizer(inputCol="lower_text", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")

        # Chain together Spark Transformers as Spark Pipeline Stages
        pipeline = Pipeline(stages=[tokenizer, hashingTF])

        # return a Spark Pipeline
        return pipeline

    def get_param_map(self, configProperties, sparkSession):
        pass
```




**Sparkの例**

```scala
// Spark

import com.adobe.platform.ml.config.ConfigProperties
import com.adobe.platform.ml.sdk.FeaturePipelineFactory
import org.apache.spark.ml.feature.{HashingTF, Tokenizer}
import org.apache.spark.ml.Pipeline
import org.apache.spark.ml.param.ParamMap
import org.apache.spark.sql.SparkSession

class MyFeaturePipelineFactory(uid:String) extends FeaturePipelineFactory(uid) {
    def this() = this("MyFeaturePipeline")

    override def createPipeline(configProperties: ConfigProperties): Pipeline = {
        
        // Spark Transformers
        val tokenizer = new Tokenizer()
            .setInputCol("lower_text")
            .setOutputCol("words")
        val hashingTF = new HashingTF()
            .setInputCol(tokenizer.getOutputCol())
            .setOutputCol("features")

        // Chain together Spark Transformers as Spark Pipeline Stages
        val pipeline = new Pipeline()
            .setStages(Array(tokenizer, hashingTF))
        
        // return a Spark Pipeline
        pipeline
    }

    override def getParamMap(configProperties: ConfigProperties, sparkSession: SparkSession): ParamMap = {
        val map = new ParamMap()
        map
    }
}
```



### 機能データセットをDataSaverに保存する

DataSaverは、結果の機能データセットをストレージの場所に保存します。 DataSaverの実装は、抽象クラスを拡張し、抽象メソッドをオ `DataSaver` ーバーライドする必要がありま `save`す。

次の例は、DataSaverクラスを拡張して、データをプラットフォームデータセットにID別に格納します。データセットID(`featureDatasetId`)とテナントID(`tenantId`)は、設定ファイルで定義されたプロパティです。 各例を展開して詳細を表示します。


**PySparkの例**

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




**Sparkの例**

```scala
// Spark

import com.adobe.platform.dataset.DataSetOptions
import com.adobe.platform.ml.config.ConfigProperties
import com.adobe.platform.ml.impl.Constants
import com.adobe.platform.ml.sdk.DataSaver
import org.apache.spark.sql.DataFrame
import org.apache.spark.sql.functions._
import org.apache.spark.sql.types.TimestampType

class MyDataSaver extends DataSaver {
    override def save(configProperties: ConfigProperties, dataFrame: DataFrame): Unit =  {

        // Spark session
        val sparkSession = dataFrame.sparkSession

        // preliminary checks
        require(configProperties != null)
        require(dataFrame != null)

        // prepare variables
        val timestamp:String = "2019-01-01 00:00:00"
        val output_dataset_id: String = configProperties
            .get("featureDatasetId").getOrElse("")
        val tenant_id:String = configProperties
            .get("tenantId").getOrElse("")
        val serviceToken: String = sparkSession.sparkContext.getConf
            .get("ML_FRAMEWORK_IMS_ML_TOKEN", "").toString
        val userToken: String = sparkSession.sparkContext.getConf
            .get("ML_FRAMEWORK_IMS_TOKEN", "").toString
        val orgId: String = sparkSession.sparkContext.getConf
            .get("ML_FRAMEWORK_IMS_ORG_ID", "").toString
        val apiKey: String = sparkSession.sparkContext.getConf
            .get("ML_FRAMEWORK_IMS_CLIENT_ID", "").toString

        // validate variables
        List(output_dataset_id, tenant_id, serviceToken, userToken, orgId, apiKey).foreach(
            value => require(value != "")
        )

        // create and prepare DataFrame with valid columns
        import sparkSession.implicits._

        var output_df  = dataFrame.withColumn("date", $"date".cast("String"))
        output_df = output_df.withColumn("timestamp", lit(timestamp).cast(TimestampType))
        output_df = output_df.withColumn("_id", lit("empty"))
        output_df = output_df.withColumn("eventType", lit("empty"))

        // store data into dataset
        output_df.select(tenant_id, "_id", "eventType", "timestamp").write.format("com.adobe.platform.dataset")
            .option(DataSetOptions.orgId, orgId)
            .option(DataSetOptions.serviceToken, serviceToken)
            .option(DataSetOptions.userToken, userToken)
            .option(DataSetOptions.serviceApiKey, apiKey)
            .save(output_dataset_id)
    }
}
```

### 実装したクラス名をアプリケーションファイルで指定します

フィーチャパイプラインクラスが定義され、実装されたら、アプリケーションファイルでクラスの名前を指定する必要があります。

次の例では、実装されるクラス名を指定します。 例を展開して詳細を表示します。


**PySparkの例**

```yaml
# application.yaml

# Name of the implementation of DataLoader abstract class
feature.dataLoader: MyDataLoader

# Name of the implementation of DatasetTransformer abstract class
feature.dataset.transformer: MyDatasetTransformer

# Name of the implementation of FeaturePipelineFactory abstract class
feature.pipeline.class: MyFeaturePipelineFactory

# Name of the implementation of DataSaver abstract class
feature.dataSaver: MyDataSaver
```




**Sparkの例**

```properties
# application.properties

# Name of the implementation of DataLoader abstract class
feature.pipeline.class=MyDataLoader

# Name of the implementation of DatasetTransformer abstract class
feature.dataset.transformer=MyDatasetTransformer

# Name of the implementation of FeaturePipelineFactory abstract class
feature.dataLoader=MyFeaturePipelineFactory

# Name of the implementation of DataSaver abstract class
feature.dataSaver=MyDataSaver
```



## バイナリアーティファクトの作成

これで、フィーチャパイプラインクラスを実装し、バイナリアーティファクトにコンパイルし、API呼び出しを通じてフィーチャパイプラインを作成できます。

**PySpark**

PySpark機能パイプラインを構築するには、モデルオーサリングSDKのル `setup.py` ートディレクトリにあるPythonスクリプトを実行します。

>[!NOTE] PySpark機能パイプラインを構築するには、Python 3がマシンにインストールされている必要があります。

```shell
python3 setup.py bdist_egg
```

フィーチャパイプラインの構築に成功すると、ディレ `.egg` クトリにアーティファク `/dist` トが生成されます。このアーティファクトはフィーチャパイプラインの作成に使用されます。

**Spark**

Spark機能のパイプラインを構築するには、モデルオーサリングSDKのルートディレクトリで次のコンソールコマンドを実行します。

>[!NOTE] Spark機能パイプラインを作成するには、Scalaとsbtがコンピューターにインストールされている必要があります。

```shell
mvn clean install
```

フィーチャパイプラインの構築に成功すると、ディレ `.jar` クトリにアーティファク `/dist` トが生成されます。このアーティファクトはフィーチャパイプラインの作成に使用されます。

## APIを使用したフィーチャーパイプラインエンジンの作成

フィーチャパイプラインを作成し、バイナリアーティファクトを構築したので、Sensei Machine Learning APIを使 [用してフィーチャパイプラインエンジンを作成できます](../api/engines.md#create-a-feature-pipeline-engine-using-binary-artifacts)。 フィーチャーパイプラインエンジンを正常に作成すると、応答本体の一部としてエンジンIDが提供されます。次の手順に進む前に、この値を保存してください。

## 次の手順

[//]: # (Next steps section should refer to tutorials on how to score data using the Feature Pipeline Engine. Update this document once those tutorials are available)

このドキュメントを読むと、モデルオーサリングSDKを使用して機能パイプラインを作成し、バイナリアーティファクトを構築し、API呼び出しを使用して機能パイプラインエンジンを作成します。 これで、新しく作成したエンジン [と開始変換データセットを使用し](../api/mlinstances.md#create-an-mlinstance) 、フィーチャーパイプラインモデルを作成し、スケールでデータフィーチャーを抽出する準備が整いました。