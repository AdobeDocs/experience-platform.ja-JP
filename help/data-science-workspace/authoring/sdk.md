---
keywords: Experience Platform;developer guide;SDK;Model authoring;Data Science Workspace;popular topics
solution: Experience Platform
title: SDK開発者ガイド
topic: Overview
translation-type: tm+mt
source-git-commit: 83e74ad93bdef056c8aef07c9d56313af6f4ddfd
workflow-type: tm+mt
source-wordcount: '943'
ht-degree: 1%

---


# SDK開発者ガイド

モデルオーサリングSDKを使用すると、Data Science Workspaceで使用できるカスタムの機械学習レシピと機能パイプラインを開発でき、PySparkおよびSparkで実装可能なテンプレートを提供します。 [!DNL Adobe Experience Platform]

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
                <p>Pandas DataFrameとしてプラットフォームデータを読み込んで返す</p>
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

次の表に、Spark Data Loaderクラスの抽象メソッドを示します。

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
                <p>プラットフォームデータをDataFrameとして読み込んで返す</p>
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

### プラットフォームデータセットからのデータの読み込み {#load-data-from-a-platform-dataset}

次の例では、プラットフォームデータをIDで取得し、DataFrameを返します。データセットID(`datasetId`)は設定ファイル内で定義されたプロパティです。

**PySpark**

```python
# PySpark

from sdk.data_loader import DataLoader

class MyDataLoader(DataLoader):
    """
    Implementation of DataLoader which loads a DataFrame and prepares data
    """

    def load(self, configProperties, spark):
        """
        Load and return dataset

        :param configProperties:    Configuration properties
        :param spark:               Spark session
        :return:                    DataFrame
        """

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
        pd = spark.read.format("com.adobe.platform.dataset") \
            .option('serviceToken', service_token) \
            .option('userToken', user_token) \
            .option('orgId', org_id) \
            .option('serviceApiKey', api_key) \
            .load(dataset_id)

        # return as DataFrame
        return pd
```

**Spark**

```scala
// Spark

import com.adobe.platform.ml.config.ConfigProperties
import com.adobe.platform.ml.sdk.DataLoader
import org.apache.spark.sql.{DataFrame, SparkSession}

/**
 * Implementation of DataLoader which loads a DataFrame and prepares data
 */
class MyDataLoader extends DataLoader {

    /**
     * @param configProperties  - Configuration properties
     * @param sparkSession      - Spark session
     * @return                  - DataFrame
     */
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

## DataSaver {#datasaver}

DataSaverクラスは、スコアリングや機能エンジニアリングの出力データなど、出力データの保存に関連するものをカプセル化します。 データ保存機能は抽象クラスを拡張し、抽象メソッド `DataSaver` をオーバーライドする必要があり `save`ます。

**PySpark**

次の表は、PySpark Data Saverクラスの抽象メソッドを説明しています。

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
                <p>出力データをDataFrameとして受け取り、プラットフォームデータセットに格納する</p>
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

**Spark**

次の表に、Spark Data Saverクラスの抽象メソッドを示します。

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
                <p>出力データをDataFrameとして受け取り、プラットフォームデータセットに格納する</p>
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

### プラットフォームデータセットへのデータの保存 {#save-data-to-a-platform-dataset}

データをプラットフォームデータセットに格納するには、次のいずれかのプロパティを指定するか、設定ファイルで定義する必要があります。

- データの格納先となる有効なプラットフォームデータセットID
- 組織に属するテナントID

次の例では、データ(`prediction`)をプラットフォームデータセットに格納します。データセットID(`datasetId`)とテナントID(`tenantId`)は、設定ファイル内で定義されたプロパティです。


**PySpark**

```python
# PySpark

from sdk.data_saver import DataSaver
from pyspark.sql.types import StringType, TimestampType


class MyDataSaver(DataSaver):
    """
    Implementation of DataSaver which stores a DataFrame to a Platform dataset
    """

    def save(self, configProperties, prediction):
        """
        Store DataFrame to a Platform dataset

        :param configProperties:    Configuration properties
        :param prediction:          DataFrame to be stored to a Platform dataset
        """

        # Spark context
        sparkContext = prediction._sc

        # preliminary checks
        if configProperties is None:
            raise ValueError("configProperties parameter is null")
        if prediction is None:
            raise ValueError("prediction parameter is null")
        if sparkContext is None:
            raise ValueError("sparkContext parameter is null")

        # prepare variables
        timestamp = "2019-01-01 00:00:00"
        output_dataset_id = str(
            configProperties.get("datasetId"))
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

        # store data into dataset
        prediction.write.format("com.adobe.platform.dataset") \
            .option('orgId', org_id) \
            .option('serviceToken', service_token) \
            .option('userToken', user_token) \
            .option('serviceApiKey', api_key) \
            .save(output_dataset_id)
```




**Spark**

```scala
// Spark

import com.adobe.platform.dataset.DataSetOptions
import com.adobe.platform.ml.config.ConfigProperties
import com.adobe.platform.ml.impl.Constants
import com.adobe.platform.ml.sdk.DataSaver
import org.apache.spark.sql.DataFrame
import org.apache.spark.sql.functions._
import org.apache.spark.sql.types.TimestampType

/**
 * Implementation of DataSaver which stores a DataFrame to a Platform dataset
 */
class ScoringDataSaver extends DataSaver {

    /**
     * @param configProperties  - Configuration properties
     * @param dataFrame         - DataFrame to be stored to a Platform dataset
     */
    override def save(configProperties: ConfigProperties, dataFrame: DataFrame): Unit =  {

        // Spark session
        val sparkSession = dataFrame.sparkSession
        import sparkSession.implicits._

        // preliminary checks
        require(configProperties != null)
        require(dataFrame != null)

        // prepare variables
        val predictionColumn = configProperties.get(Constants.PREDICTION_COL)
            .getOrElse(Constants.DEFAULT_PREDICTION)
        val timestamp:String = "2019-01-01 00:00:00"
        val output_dataset_id: String = configProperties
            .get("datasetId").getOrElse("")
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

        // store data into dataset
        dataFrame.write.format("com.adobe.platform.dataset")
            .option(DataSetOptions.orgId, orgId)
            .option(DataSetOptions.serviceToken, serviceToken)
            .option(DataSetOptions.userToken, userToken)
            .option(DataSetOptions.serviceApiKey, apiKey)
            .save(output_dataset_id)
    }
}
```

## DatasetTransformer {#datasettransformer}

DatasetTransformerクラスは、データセットの構造を変更および変換します。 Senesie Machine Learning Runtimeは、このコンポーネントを定義する必要はなく、必要に応じて実装します。

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

**Spark**

次の表に、Sparkデータセットトランスフォームクラスの抽象メソッドを示します。

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

**Spark**

次の表に、Spark FeaturePipelineFactoryのクラスメソッドを示します。

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

PipelineFactoryクラスには、モデルトレーニングとスコアリングのメソッドと定義がカプセル化されています。トレーニングロジックとアルゴリズムは、Sparkパイプラインの形式で定義されます。

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

**Spark**

次の表に、Spark PipelineFactoryのクラスメソッドを示します。

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

**Spark**

次の表に、Spark MLEvaluatorのクラスメソッドを示します。

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