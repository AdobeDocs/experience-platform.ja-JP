---
keywords: Experience Platform；開発者ガイド；SDK；モデルオーサリング；Data Science Workspace；人気の高いトピック；テスト
solution: Experience Platform
title: モデルオーサリング SDK
description: モデルオーサリング SDK を使用すると、Adobe Experience Platform Data Science Workspace で使用できるカスタムの機械学習レシピと機能パイプラインを開発し、PySpark と Spark(Scala) で実装可能なテンプレートを提供できます。
exl-id: c7577f93-a64f-49b7-a76d-71f21d619052
source-git-commit: 86e6924078c115fb032ce39cd678f1d9c622e297
workflow-type: tm+mt
source-wordcount: '977'
ht-degree: 70%

---

# モデルオーサリング SDK

モデルオーサリング SDK を使用すると、で使用できるカスタムの機械学習レシピと機能パイプラインを開発できます。 [!DNL Adobe Experience Platform] Data Science Workspace（で実装可能なテンプレートを提供） [!DNL PySpark] および [!DNL Spark (Scala)].

このドキュメントでは、モデルオーサリング SDK 内の様々なクラスに関する情報を提供します。

## DataLoader {#dataloader}

DataLoader クラスは、生の入力データを取得、フィルタリング、返す操作に関連するすべてをカプセル化します。入力データの例としては、トレーニング、スコアリング、特徴エンジニアリングなどがあります。データローダーは `DataLoader` 抽象クラスを拡張し、`load` 抽象メソッドをオーバーライドする必要があります。

**PySpark**

次の表に、PySpark データローダークラスの抽象メソッドを示します。

<table>
    <thead>
        <tr>
            <th>メソッドと説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><code>load(self, configProperties, spark)</code></p>
                <p>Pandas DataFrame として Platform データを読み込み、返します。</p>
            </td>
            <td>
                <ul>
                    <li><code>self</code>：自己参照</li>
                    <li><code>configProperties</code>：設定プロパティのマップ</li>
                    <li><code>spark</code>：Spark セッション</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

**Spark**

次の表に、 [!DNL Spark] データローダークラス：

<table>
    <thead>
        <tr>
            <th>メソッドと説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><code>load(configProperties, sparkSession)</code></p>
                <p>Platform データを DataFrame として読み込み、返します。</p>
            </td>
            <td>
                <ul>
                    <li><code>configProperties</code>：設定プロパティのマップ</li>
                    <li><code>sparkSession</code>：Spark セッション</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

### からデータを読み込む [!DNL Platform] データセット {#load-data-from-a-platform-dataset}

次の例では、を取得します。 [!DNL Platform] データを ID 別に取得し、DataFrame を返します。このとき、データセット ID (`datasetId`) は、設定ファイル内で定義されたプロパティです。

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

DataSaver クラスは、スコアリングや特徴エンジニアリングの出力データなど、出力データの格納に関連するものをカプセル化します。データセーバーは `DataSaver` 抽象クラスを拡張し、`save` 抽象メソッドを上書きする必要があります。

**PySpark**

次の表に、 [!DNL PySpark] データセーバークラス：

<table>
    <thead>
        <tr>
            <th>メソッドと説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><code>save(self, configProperties, dataframe)</code></p>
                <p>出力データを DataFrame として受け取り、Platform データセットに保存します。</p>
            </td>
            <td>
                <ul>
                    <li><code>self</code>：自己参照</li>
                    <li><code>configProperties</code>：設定プロパティのマップ</li>
                    <li><code>dataframe</code>：DataFrame の形式で保存するデータ</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

**Spark (Scala)**

次の表に、 [!DNL Spark] データセーバークラス：

<table>
    <thead>
        <tr>
            <th>メソッドと説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><code>save(configProperties, dataFrame)</code></p>
                <p>出力データを DataFrame として受け取り、Platform データセットに保存します。</p>
            </td>
            <td>
                <ul>
                    <li><code>configProperties</code>：設定プロパティのマップ</li>
                    <li><code>dataFrame</code>：DataFrame の形式で保存するデータ</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

### にデータを保存する [!DNL Platform] データセット {#save-data-to-a-platform-dataset}

データをに保存するために [!DNL Platform] データセットの場合、プロパティは設定ファイルで指定するか、定義する必要があります。

- 有効な [!DNL Platform] データの保存先となるデータセット ID
- 組織に属するテナント ID

次の例では、データ（`prediction`[!DNL Platform]）を データセットに格納します。データセット ID（`datasetId`）とテナント ID（`tenantId`）は、設定ファイル内で定義されたプロパティです。


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

DatasetTransformer クラスは、データセットの構造を変更および変換します。この [!DNL Sensei Machine Learning Runtime] は、このコンポーネントを定義する必要はなく、要件に基づいて実装されます。

特徴パイプラインに関しては、データセットトランスフォーマーを特徴パイプラインファクトリと協力して使用し、特徴エンジニアリングのためのデータを準備できます。

**PySpark**

次の表に、PySpark データセットトランスフォーマークラスのクラスメソッドを示します。

<table>
    <thead>
        <tr>
            <th>メソッドと説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><i>abstract</i><br/><code>transform(self, configProperties, dataset)</code></p>
                <p>データセットを入力として受け取り、新しい派生データセットを出力します。</p>
            </td>
            <td>
                <ul>
                    <li><code>self</code>：自己参照</li>
                    <li><code>configProperties</code>：設定プロパティのマップ</li>
                    <li><code>dataset</code>：変換される入力データセット</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

**Spark (Scala)**

次の表に、 [!DNL Spark] データセット変換サービスのクラス：

<table>
    <thead>
        <tr>
            <th>メソッドと説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><code>transform(configProperties, dataset)</code></p>
                <p>データセットを入力として受け取り、新しい派生データセットを出力します。</p>
            </td>
            <td>
                <ul>
                    <li><code>configProperties</code>：設定プロパティのマップ</li>
                    <li><code>dataset</code>：変換される入力データセット</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

## FeaturePipelineFactory {#featurepipelinefactory}

FeaturePipelineFactory クラスには、特徴抽出アルゴリズムが含まれ、特徴パイプラインのステージを開始から終了まで定義します。

**PySpark**

次の表に、PySpark FeaturePipelineFactory のクラスメソッドを示します。

<table>
    <thead>
        <tr>
            <th>メソッドと説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><i>abstract</i><br/><code>create_pipeline(self, configProperties)</code></p>
                <p>一連の Spark トランスフォーマーを含む Spark パイプラインを作成して返します。</p>
            </td>
            <td>
                <ul>
                    <li><code>self</code>：自己参照</li>
                    <li><code>configProperties</code>：設定プロパティのマップ</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>
                <p><i>abstract</i><br/><code>get_param_map(self, configProperties, sparkSession)</code></p>
                <p>設定プロパティからパラメーターマップを取得して返します。</p>
            </td>
            <td>
                <ul>
                    <li><code>self</code>：自己参照</li>
                    <li><code>configProperties</code>：設定プロパティ</li>
                    <li><code>sparkSession</code>：Spark セッション</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

**Spark (Scala)**

次の表に、 [!DNL Spark] FeaturePipelineFactory:

<table>
    <thead>
        <tr>
            <th>メソッドと説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><i>abstract</i><br/><code>createPipeline(configProperties)</code></p>
                <p>一連のトランスフォーマーを含むパイプラインを作成して返します。</p>
            </td>
            <td>
                <ul>
                    <li><code>configProperties</code>：設定プロパティのマップ</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>
                <p><i>abstract</i><br/><code>getParamMap(configProperties, sparkSession)</code></p>
                <p>設定プロパティからパラメーターマップを取得して返します。</p>
            </td>
            <td>
                <ul>
                    <li><code>configProperties</code>：設定プロパティ</li>
                    <li><code>sparkSession</code>：Spark セッション</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

## PipelineFactory {#pipelinefactory}

PipelineFactory クラスは、モデルのトレーニングとスコアリングのメソッドと定義をカプセル化します。トレーニングロジックとアルゴリズムは、 [!DNL Spark] パイプライン。

**PySpark**

次の表に、PySpark PipelineFactory のクラスメソッドを示します。

<table>
    <thead>
        <tr>
            <th>メソッドと説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><i>abstract</i><br/><code>apply(self, configProperties)</code></p>
                <p>モデルのトレーニングとスコアリングのロジックとアルゴリズムを含む Spark パイプラインを作成して返します。</p>
            </td>
            <td>
                <ul>
                    <li><code>self</code>：自己参照</li>
                    <li><code>configProperties</code>：設定プロパティ</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>
                <p><i>abstract</i><br/><code>train(self, configProperties, dataframe)</code></p>
                <p>モデルをトレーニングするロジックとアルゴリズムを含むカスタムパイプラインを返します。Spark パイプラインを使用する場合、このメソッドは不要です。</p>
            </td>
            <td>
                <ul>
                    <li><code>self</code>：自己参照</li>
                    <li><code>configProperties</code>：設定プロパティ</li>
                    <li><code>dataframe</code>：トレーニング入力の特徴データセット</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>
                <p><i>abstract</i><br/><code>score(self, configProperties, dataframe, model)</code></p>
                <p>訓練済みモデルを使用してスコアを付け、結果を返します。</p>
            </td>
            <td>
                <ul>
                    <li><code>self</code>：自己参照</li>
                    <li><code>configProperties</code>：設定プロパティ</li>
                    <li><code>dataframe</code>：スコアリング用の入力データセット</li>
                    <li><code>model</code>：スコアリングに使用される訓練済みモデル</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>
                <p><i>abstract</i><br/><code>get_param_map(self, configProperties, sparkSession)</code></p>
                <p>設定プロパティからパラメーターマップを取得して返します。</p>
            </td>
            <td>
                <ul>
                    <li><code>self</code>：自己参照</li>
                    <li><code>configProperties</code>：設定プロパティ</li>
                    <li><code>sparkSession</code>：Spark セッション</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

**Spark (Scala)**

次の表に、 [!DNL Spark] PipelineFactory:

<table>
    <thead>
        <tr>
            <th>メソッドと説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><i>abstract</i><br/><code>apply(configProperties)</code></p>
                <p>モデルのトレーニングとスコアリングのロジックとアルゴリズムを含むパイプラインを作成して返します。</p>
            </td>
            <td>
                <ul>
                    <li><code>configProperties</code>：設定プロパティ</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>
                <p><i>abstract</i><br/><code>getParamMap(configProperties, sparkSession)</code></p>
                <p>設定プロパティからパラメーターマップを取得して返します。</p>
            </td>
            <td>
                <ul>
                    <li><code>configProperties</code>：設定プロパティ</li>
                    <li><code>sparkSession</code>：Spark セッション</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

## MLEvaluator {#mlevaluator}

MLEvaluator クラスは、評価指標を定義するメソッドと、トレーニングとテストデータセットを決定するためのメソッドを提供します。

**PySpark**

次の表に、PySpark MLEvaluator のクラスメソッドを示します。

<table>
    <thead>
        <tr>
            <th>メソッドと説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><i>abstract</i><br/><code>split(self, configProperties, dataframe)</code></p>
                <p>入力データセットをトレーニングサブセットとテストサブセットに分割します。</p>
            </td>
            <td>
                <ul>
                    <li><code>self</code>：自己参照</li>
                    <li><code>configProperties</code>：設定プロパティ</li>
                    <li><code>dataframe</code>：分割する入力データセット</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>
                <p><i>abstract</i><br/><code>evaluate(self, dataframe, model, configProperties)</code></p>
                <p>訓練済みモデルを評価し、評価結果を返します。</p>
            </td>
            <td>
                <ul>
                    <li><code>self</code>：自己参照</li>
                    <li><code>dataframe</code>:トレーニングデータとテストデータから成る DataFrame</li>
                    <li><code>model</code>：訓練済みモデル</li>
                    <li><code>configProperties</code>：設定プロパティ</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

**Spark (Scala)**

次の表に、 [!DNL Spark] MLEvaluator:

<table>
    <thead>
        <tr>
            <th>メソッドと説明</th>
            <th>パラメーター</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <p><i>abstract</i><br/><code>split(configProperties, data)</code></p>
                <p>入力データセットをトレーニングサブセットとテストサブセットに分割します。</p>
            </td>
            <td>
                <ul>
                    <li><code>configProperties</code>：設定プロパティ</li>
                    <li><code>data</code>：分割する入力データセット</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>
                <p><i>abstract</i><br/><code>evaluate(configProperties, model, data)</code></p>
                <p>訓練済みモデルを評価し、評価結果を返します。</p>
            </td>
            <td>
                <ul>
                    <li><code>configProperties</code>：設定プロパティ</li>
                    <li><code>model</code>：訓練済みモデル</li>
                    <li><code>data</code>:トレーニングデータとテストデータから成る DataFrame</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>
