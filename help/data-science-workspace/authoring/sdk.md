---
keywords: Experience Platform;developer guide;SDK;Model authoring;Data Science Workspace;popular topics
solution: Experience Platform
title: SDK開発者ガイド
topic: Overview
translation-type: tm+mt
source-git-commit: c48079ba997a7b4c082253a0b2867df76927aa6d
workflow-type: tm+mt
source-wordcount: '930'
ht-degree: 1%

---


# SDK開発者ガイド

モデルオーサリングSDKを使用すると、Data Science Workspaceで使用できるカスタムの機械学習レシピおよび機能パイプラインを開発し、およびで実装可能なテンプレートを提供でき [!DNL Adobe Experience Platform][!DNL PySpark][!DNL Spark (Scala)]ます。

このドキュメントは、モデルオーサリングSDK内の様々なクラスに関する情報を提供します。

## DataLoader {#dataloader}

DataLoaderクラスは、生の入力データの取得、フィルタリング、返信に関連するすべての要素をカプセル化します。 入力データの例としては、トレーニング、スコアリング、機能エンジニアリングなどがあります。 データローダは抽象クラスを拡張し、抽象メソッド `DataLoader` をオーバーライドする必要があり `load`ます。

**PySpark**

次の表は、PySpark Data Loaderクラスの抽象メソッドを説明しています。

<table>
    <thead>
        <tr>
            <th>方法と説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><code class=" language-undefined">load(self, configProperties, spark)</code></p>
                <p>PlatformデータをPandas DataFrameとして読み込んで返す</p>
            </td>
            <td>
                <ul>
                    <li><code class=" language-undefined">self</code>: 自己参照</li>
                    <li><code class=" language-undefined">configProperties</code>: 設定プロパティのマップ</li>
                    <li><code class=" language-undefined">spark</code>: Sparkセッション</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

**Spark**

次の表に、 [!DNL Spark] Data Loaderクラスの抽象メソッドを示します。

<table>
    <thead>
        <tr>
            <th>方法と説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><code class=" language-undefined">load(configProperties, sparkSession)</code></p>
                <p>PlatformデータをDataFrameとして読み込んで返す</p>
            </td>
            <td>
                <ul>
                    <li><code class=" language-undefined">configProperties</code>: 設定プロパティのマップ</li>
                    <li><code class=" language-undefined">sparkSession</code>: Sparkセッション</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

### データセットからのデータの読み込み [!DNL Platform] {#load-data-from-a-platform-dataset}

次の例では、IDで [!DNL Platform] データを取得し、DataFrameを返します。この場合、データセットID(`datasetId`)は設定ファイル内で定義されたプロパティです。

**PySpark**

```python
# PySpark

from sdk.data_loader import DataLoader

class MyDataLoader(DataLoader):
    """
    Implementation of DataLoader which loads a DataFrame and prepares data
    """

    def load_dataset(config_properties, spark, task_id):

        PLATFORM_SDK_PQS_PACKAGE = "com.adobe.platform.query"
        PLATFORM_SDK_PQS_INTERACTIVE = "interactive"

        # prepare variables
        service_token = str(spark.sparkContext.getConf().get("ML_FRAMEWORK_IMS_ML_TOKEN"))
        user_token = str(spark.sparkContext.getConf().get("ML_FRAMEWORK_IMS_TOKEN"))
        org_id = str(spark.sparkContext.getConf().get("ML_FRAMEWORK_IMS_ORG_ID"))
        api_key = str(spark.sparkContext.getConf().get("ML_FRAMEWORK_IMS_CLIENT_ID"))

        dataset_id = str(config_properties.get(task_id))

        # validate variables
        for arg in ['service_token', 'user_token', 'org_id', 'dataset_id', 'api_key']:
            if eval(arg) == 'None':
                raise ValueError("%s is empty" % arg)

        # load dataset through Spark session

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

        # return as DataFrame
        return pd
```

**Spark (Scala)**

```scala
// Spark

package com.adobe.platform.ml

import java.time.LocalDateTime

import com.adobe.platform.ml.config.ConfigProperties
import com.adobe.platform.query.QSOption
import org.apache.spark.ml.feature.StringIndexer
import org.apache.spark.sql.expressions.Window
import org.apache.spark.sql.functions._
import org.apache.spark.sql.types.{StructType, TimestampType}
import org.apache.spark.sql.{DataFrame, SparkSession}
import org.apache.spark.sql.Column

/**
 * Implementation of DataLoader which loads a DataFrame and prepares data
 */
class MyDataLoader extends DataLoader {

    final val PLATFORM_SDK_PQS_PACKAGE: String = "com.adobe.platform.query"
    final val PLATFORM_SDK_PQS_INTERACTIVE: String = "interactive"
    final val PLATFORM_SDK_PQS_BATCH: String = "batch"

    /**
    *
    * @param configProperties - Configuration Properties map
    * @param sparkSession     - SparkSession
    * @return                 - DataFrame which is loaded for training
    */


  def load_dataset(configProperties: ConfigProperties, sparkSession: SparkSession, taskId: String): DataFrame = {

    require(configProperties != null)
    require(sparkSession != null)

    // Read the configs
    val serviceToken: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_ML_TOKEN", "").toString
    val userToken: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_TOKEN", "").toString
    val orgId: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_ORG_ID", "").toString
    val apiKey: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_CLIENT_ID", "").toString

    val dataSetId: String = configProperties.get(taskId).getOrElse("")

    // Load the dataset
    var df = sparkSession.read.format(PLATFORM_SDK_PQS_PACKAGE)
      .option(QSOption.userToken, userToken)
      .option(QSOption.serviceToken, serviceToken)
      .option(QSOption.imsOrg, orgId)
      .option(QSOption.apiKey, apiKey)
      .option(QSOption.mode, PLATFORM_SDK_PQS_INTERACTIVE)
      .option(QSOption.datasetId, dataSetId)
      .load()
    df.show()
    df
    }
}
```

## DataSaver {#datasaver}

DataSaverクラスは、スコアリングや機能エンジニアリングの出力データなど、出力データの保存に関連するものをカプセル化します。 データ保存機能は抽象クラスを拡張し、抽象メソッド `DataSaver` をオーバーライドする必要があり `save`ます。

**PySpark**

次の表に、 [!DNL PySpark] Data Saverクラスの抽象メソッドを示します。

<table>
    <thead>
        <tr>
            <th>方法と説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><code class=" language-undefined">save(self, configProperties, dataframe)</code></p>
                <p>出力データをDataFrameとして受け取り、Platformデータセットに格納する</p>
            </td>
            <td>
                <ul>
                    <li><code class=" language-undefined">self</code>: 自己参照</li>
                    <li><code class=" language-undefined">configProperties</code>: 設定プロパティのマップ</li>
                    <li><code class=" language-undefined">dataframe</code>: DataFrameの形式で保存するデータ</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

**Spark (Scala)**

次の表に、 [!DNL Spark] Data Saverクラスの抽象メソッドを示します。

<table>
    <thead>
        <tr>
            <th>方法と説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><code class=" language-undefined">save(configProperties, dataFrame)</code></p>
                <p>出力データをDataFrameとして受け取り、Platformデータセットに格納する</p>
            </td>
            <td>
                <ul>
                    <li><code class=" language-undefined">configProperties</code>: 設定プロパティのマップ</li>
                    <li><code class=" language-undefined">dataFrame</code>: DataFrameの形式で保存するデータ</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

### データセットへのデータの [!DNL Platform] 保存 {#save-data-to-a-platform-dataset}

データセットにデータを格納するには、次のように設定ファイルでプロパティを指定するか、定義する必要があり [!DNL Platform] ます。

- データの格納先となる有効な [!DNL Platform] データセットID
- 組織に属するテナントID

次の例では、データ(`prediction`)をデータセットに格納します。データセットID( [!DNL Platform] )とテナントID(`datasetId``tenantId`)は、設定ファイル内で定義されたプロパティです。


**PySpark**

```python
# PySpark

from sdk.data_saver import DataSaver
from pyspark.sql.types import StringType, TimestampType
from pyspark.sql.functions import col, lit, struct
from .helper import *


class MyDataSaver(DataSaver):
    """
    Implementation of DataSaver which stores a DataFrame to a Platform dataset
    """

    def save(self, config_properties, prediction):

        # Spark context
        sparkContext = prediction._sc

        # preliminary checks
        if config_properties is None:
            raise ValueError("config_properties parameter is null")
        if prediction is None:
            raise ValueError("prediction parameter is null")
        if sparkContext is None:
            raise ValueError("sparkContext parameter is null")
        
        PLATFORM_SDK_PQS_PACKAGE = "com.adobe.platform.query"

        # prepare variables
        scored_dataset_id = str(config_properties.get("scoringResultsDataSetId"))
        tenant_id = str(config_properties.get("tenant_id"))
        timestamp = "2019-01-01 00:00:00"

        service_token = str(sparkContext.getConf().get("ML_FRAMEWORK_IMS_ML_TOKEN"))
        user_token = str(sparkContext.getConf().get("ML_FRAMEWORK_IMS_TOKEN"))
        org_id = str(sparkContext.getConf().get("ML_FRAMEWORK_IMS_ORG_ID"))
        api_key = str(sparkContext.getConf().get("ML_FRAMEWORK_IMS_CLIENT_ID"))

        # validate variables
       for arg in ['service_token', 'user_token', 'org_id', 'scored_dataset_id', 'api_key', 'tenant_id']:
            if eval(arg) == 'None':
                raise ValueError("%s is empty" % arg)
        
        scored_df = prediction.withColumn("date", col("date").cast(StringType()))
        scored_df = scored_df.withColumn(tenant_id, struct(col("date"), col("store"), col("prediction")))
        scored_df = scored_df.withColumn("timestamp", lit(timestamp).cast(TimestampType()))
        scored_df = scored_df.withColumn("_id", lit("empty"))
        scored_df = scored_df.withColumn("eventType", lit("empty")

        # store data into dataset

        query_options = get_query_options(sparkContext)

        scored_df.select(tenant_id, "_id", "eventType", "timestamp").write.format(PLATFORM_SDK_PQS_PACKAGE) \
            .option(query_options.userToken(), user_token) \
            .option(query_options.serviceToken(), service_token) \
            .option(query_options.imsOrg(), org_id) \
            .option(query_options.apiKey(), api_key) \
            .option(query_options.datasetId(), scored_dataset_id) \
            .save()
```

**Spark (Scala)**

```scala
// Spark

package com.adobe.platform.ml

import com.adobe.platform.ml.config.ConfigProperties
import com.adobe.platform.ml.impl.Constants
import com.adobe.platform.ml.sdk.DataSaver
import com.adobe.platform.query.QSOption
import org.apache.spark.sql.DataFrame
import org.apache.spark.sql.functions._
import org.apache.spark.sql.types.TimestampType

/**
 * Implementation of DataSaver which stores a DataFrame to a Platform dataset
 */

class ScoringDataSaver extends DataSaver {

  final val PLATFORM_SDK_PQS_PACKAGE: String = "com.adobe.platform.query"
  final val PLATFORM_SDK_PQS_BATCH: String = "batch"

  /**
    * Method that saves the scoring data into a dataframe
    * @param configProperties  - Configuration Properties map
    * @param dataFrame         - Dataframe with the scoring results
    */
    
  override def save(configProperties: ConfigProperties, dataFrame: DataFrame): Unit =  {

    require(configProperties != null)
    require(dataFrame != null)

    val predictionColumn = configProperties.get(Constants.PREDICTION_COL).getOrElse(Constants.DEFAULT_PREDICTION)
    val sparkSession = dataFrame.sparkSession

    val serviceToken: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_ML_TOKEN", "").toString
    val userToken: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_TOKEN", "").toString
    val orgId: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_ORG_ID", "").toString
    val apiKey: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_CLIENT_ID", "").toString
    val tenantId:String = configProperties.get("tenantId").getOrElse("")
    val timestamp:String = "2019-01-01 00:00:00"

    val scoringResultsDataSetId: String = configProperties.get("scoringResultsDataSetId").getOrElse("")
    import sparkSession.implicits._

    var df = dataFrame.withColumn("date", $"date".cast("String"))

    var scored_df  = df.withColumn(tenantId, struct(df("date"), df("store"), df(predictionColumn)))
    scored_df = scored_df.withColumn("timestamp", lit(timestamp).cast(TimestampType))
    scored_df = scored_df.withColumn("_id", lit("empty"))
    scored_df = scored_df.withColumn("eventType", lit("empty"))

    scored_df.select(tenantId, "_id", "eventType", "timestamp").write.format(PLATFORM_SDK_PQS_PACKAGE)
      .option(QSOption.userToken, userToken)
      .option(QSOption.serviceToken, serviceToken)
      .option(QSOption.imsOrg, orgId)
      .option(QSOption.apiKey, apiKey)
      .option(QSOption.datasetId, scoringResultsDataSetId)
      .save()
    }
}
```

## DatasetTransformer {#datasettransformer}

DatasetTransformerクラスは、データセットの構造を変更および変換します。 は、このコンポーネント [!DNL Sensei Machine Learning Runtime] を定義する必要はなく、要件に基づいて実装されます。

フィーチャパイプラインに関しては、データセット変圧器をフィーチャパイプラインファクトリと共に使用して、フィーチャエンジニアリング用のデータを準備できます。

**PySpark**

次の表に、PySparkデータセットトランスフォームクラスのクラスメソッドを示します。

<table>
    <thead>
        <tr>
            <th>方法と説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><i>abstract</i><br/><code class=" language-undefined">transform(self, configProperties, dataset)</code></p>
                <p>データセットを入力として受け取り、新しい派生データセットを出力します。</p>
            </td>
            <td>
                <ul>
                    <li><code class=" language-undefined">self</code>: 自己参照</li>
                    <li><code class=" language-undefined">configProperties</code>: 設定プロパティのマップ</li>
                    <li><code class=" language-undefined">dataset</code>: 変換用の入力データセット</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

**Spark (Scala)**

次の表に、 [!DNL Spark] dataset transformerクラスの抽象メソッドを示します。

<table>
    <thead>
        <tr>
            <th>方法と説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><code class=" language-undefined">transform(configProperties, dataset)</code></p>
                <p>データセットを入力として受け取り、新しい派生データセットを出力します。</p>
            </td>
            <td>
                <ul>
                    <li><code class=" language-undefined">configProperties</code>: 設定プロパティのマップ</li>
                    <li><code class=" language-undefined">dataset</code>: 変換用の入力データセット</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

## FeaturePipelineFactory {#featurepipelinefactory}

FeaturePipelineFactoryクラスには、フィーチャ抽出アルゴリズムが含まれており、フィーチャパイプラインのステージを開始から終了まで定義します。

**PySpark**

次の表に、PySpark FeaturePipelineFactoryのクラスメソッドを示します。

<table>
    <thead>
        <tr>
            <th>方法と説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><i>abstract</i><br/><code class=" language-undefined">create_pipeline(self, configProperties)</code></p>
                <p>一連のSpark変圧器を含むSparkパイプラインを作成して返す</p>
            </td>
            <td>
                <ul>
                    <li><code class=" language-undefined">self</code>: 自己参照</li>
                    <li><code class=" language-undefined">configProperties</code>: 設定プロパティのマップ</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>
                <p><i>abstract</i><br/><code class=" language-undefined">get_param_map(self, configProperties, sparkSession)</code></p>
                <p>設定プロパティからパラメーターマップを取得して返す</p>
            </td>
            <td>
                <ul>
                    <li><code class=" language-undefined">self</code>: 自己参照</li>
                    <li><code class=" language-undefined">configProperties</code>: 設定プロパティ</li>
                    <li><code class=" language-undefined">sparkSession</code>: Sparkセッション</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

**Spark (Scala)**

次の表に、FeaturePipelineFactoryのクラスメソッドを示し [!DNL Spark] ます。

<table>
    <thead>
        <tr>
            <th>方法と説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><i>abstract</i><br/><code class=" language-undefined">createPipeline(configProperties)</code></p>
                <p>一連の変圧器を含むパイプラインを作成して返す</p>
            </td>
            <td>
                <ul>
                    <li><code class=" language-undefined">configProperties</code>: 設定プロパティのマップ</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>
                <p><i>abstract</i><br/><code class=" language-undefined">getParamMap(configProperties, sparkSession)</code></p>
                <p>設定プロパティからパラメーターマップを取得して返す</p>
            </td>
            <td>
                <ul>
                    <li><code class=" language-undefined">configProperties</code>: 設定プロパティ</li>
                    <li><code class=" language-undefined">sparkSession</code>: Sparkセッション</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

## PipelineFactory {#pipelinefactory}

PipelineFactoryクラスには、モデルトレーニングとスコアリングのメソッドと定義がカプセル化されています。この場合、トレーニングロジックとアルゴリズムは [!DNL Spark] パイプラインの形式で定義されます。

**PySpark**

次の表に、PySpark PipelineFactoryのクラスメソッドを示します。

<table>
    <thead>
        <tr>
            <th>方法と説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><i>abstract</i><br/><code class=" language-undefined">apply(self, configProperties)</code></p>
                <p>モデルトレーニングとスコアリングのロジックとアルゴリズムを含むSparkパイプラインの作成と返却</p>
            </td>
            <td>
                <ul>
                    <li><code class=" language-undefined">self</code>: 自己参照</li>
                    <li><code class=" language-undefined">configProperties</code>: 設定プロパティ</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>
                <p><i>abstract</i><br/><code class=" language-undefined">train(self, configProperties, dataframe)</code></p>
                <p>モデルをトレーニングするロジックとアルゴリズムを含むカスタムパイプラインを返します。 Sparkパイプラインを使用する場合は、このメソッドは不要です</p>
            </td>
            <td>
                <ul>
                    <li><code class=" language-undefined">self</code>: 自己参照</li>
                    <li><code class=" language-undefined">configProperties</code>: 設定プロパティ</li>
                    <li><code class=" language-undefined">dataframe</code>: トレーニング入力用の機能データセット</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>
                <p><i>abstract</i><br/><code class=" language-undefined">score(self, configProperties, dataframe, model)</code></p>
                <p>トレーニングを受けたモデルを使用したスコア付けと結果の返却</p>
            </td>
            <td>
                <ul>
                    <li><code class=" language-undefined">self</code>: 自己参照</li>
                    <li><code class=" language-undefined">configProperties</code>: 設定プロパティ</li>
                    <li><code class=" language-undefined">dataframe</code>: スコアリング用の入力データセット</li>
                    <li><code class=" language-undefined">model</code>: スコアリングに使用されるトレーニングを受けたモデル</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>
                <p><i>abstract</i><br/><code class=" language-undefined">get_param_map(self, configProperties, sparkSession)</code></p>
                <p>設定プロパティからパラメーターマップを取得して返す</p>
            </td>
            <td>
                <ul>
                    <li><code class=" language-undefined">self</code>: 自己参照</li>
                    <li><code class=" language-undefined">configProperties</code>: 設定プロパティ</li>
                    <li><code class=" language-undefined">sparkSession</code>: Sparkセッション</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

**Spark (Scala)**

次の表に、PipelineFactoryのクラスメソッドを示し [!DNL Spark] ます。

<table>
    <thead>
        <tr>
            <th>方法と説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><i>abstract</i><br/><code class=" language-undefined">apply(configProperties)</code></p>
                <p>モデルトレーニングとスコアリングのロジックとアルゴリズムを含むパイプラインの作成と返却</p>
            </td>
            <td>
                <ul>
                    <li><code class=" language-undefined">configProperties</code>: 設定プロパティ</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>
                <p><i>abstract</i><br/><code class=" language-undefined">getParamMap(configProperties, sparkSession)</code></p>
                <p>設定プロパティからパラメーターマップを取得して返す</p>
            </td>
            <td>
                <ul>
                    <li><code class=" language-undefined">configProperties</code>: 設定プロパティ</li>
                    <li><code class=" language-undefined">sparkSession</code>: Sparkセッション</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

## MLEvaluator {#mlevaluator}

MLEvaluatorクラスは、評価指標を定義し、トレーニングおよびテストデータセットを決定するメソッドを提供します。

**PySpark**

次の表は、PySpark MLEvaluatorのクラスメソッドを説明しています。

<table>
    <thead>
        <tr>
            <th>方法と説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><i>abstract</i><br/><code class=" language-undefined">split(self, configProperties, dataframe)</code></p>
                <p>入力データセットをトレーニングサブセットとテストサブセットに分割します。</p>
            </td>
            <td>
                <ul>
                    <li><code class=" language-undefined">self</code>: 自己参照</li>
                    <li><code class=" language-undefined">configProperties</code>: 設定プロパティ</li>
                    <li><code class=" language-undefined">dataframe</code>: 分割する入力データセット</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>
                <p><i>abstract</i><br/><code class=" language-undefined">evaluate(self, dataframe, model, configProperties)</code></p>
                <p>トレーニングを受けたモデルを評価し、評価結果を返す</p>
            </td>
            <td>
                <ul>
                    <li><code class=" language-undefined">self</code>: 自己参照</li>
                    <li><code class=" language-undefined">dataframe</code>: トレーニングデータとテストデータから成るDataFrame</li>
                    <li><code class=" language-undefined">model</code>: 訓練を受けたモデル</li>
                    <li><code class=" language-undefined">configProperties</code>: 設定プロパティ</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

**Spark (Scala)**

次の表に、 [!DNL Spark] MLEvaluatorのクラスメソッドを示します。

<table>
    <thead>
        <tr>
            <th>方法と説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><i>abstract</i><br/><code class=" language-undefined">split(configProperties, data)</code></p>
                <p>入力データセットをトレーニングサブセットとテストサブセットに分割します。</p>
            </td>
            <td>
                <ul>
                    <li><code class=" language-undefined">configProperties</code>: 設定プロパティ</li>
                    <li><code class=" language-undefined">data</code>: 分割する入力データセット</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>
                <p><i>abstract</i><br/><code class=" language-undefined">evaluate(configProperties, model, data)</code></p>
                <p>トレーニングを受けたモデルを評価し、評価結果を返す</p>
            </td>
            <td>
                <ul>
                    <li><code class=" language-undefined">configProperties</code>: 設定プロパティ</li>
                    <li><code class=" language-undefined">model</code>: 訓練を受けたモデル</li>
                    <li><code class=" language-undefined">data</code>: トレーニングデータとテストデータから成るDataFrame</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>