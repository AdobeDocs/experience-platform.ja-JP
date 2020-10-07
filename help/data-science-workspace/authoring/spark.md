---
keywords: Experience Platform;home;popular topics;data access;spark sdk;data access api;spark recipe;read spark;write spark
solution: Experience Platform
title: Sparkを使用したデータへのアクセス
topic: tutorial
type: Tutorial
description: 次のドキュメントには、Data Science Workspaceで使用するSparkを使用してデータにアクセスする方法の例が含まれています。
translation-type: tm+mt
source-git-commit: fcb4088ecac76d10b0cb69b04ad55167f5cdac3e
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 0%

---


# Sparkを使用したデータへのアクセス

次のドキュメントには、Data Science Workspaceで使用するSparkを使用してデータにアクセスする方法の例が含まれています。 JupterLabノートブックを使用したデータへのアクセスについては、JupyterLabノートブックデータアクセス [](../jupyterlab/access-notebook-data.md) ・ドキュメントを参照してください。

## はじめに

を使用するには、にパフォーマンスの最適化を追加する必要が [!DNL Spark] あり `SparkSession`ます。 また、後でデータセットの読み取りと書き込み `configProperties` を行うように設定することもできます。

```scala
import com.adobe.platform.ml.config.ConfigProperties
import com.adobe.platform.query.QSOption
import org.apache.spark.sql.{DataFrame, SparkSession}

Class Helper {

 /**
   *
   * @param configProperties - Configuration Properties map
   * @param sparkSession     - SparkSession
   * @return                 - DataFrame which is loaded for training
   */

   def load_dataset(configProperties: ConfigProperties, sparkSession: SparkSession, taskId: String): DataFrame = {
            // Read the configs
            val serviceToken: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_ML_TOKEN", "").toString
            val userToken: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_TOKEN", "").toString
            val orgId: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_ORG_ID", "").toString
            val apiKey: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_CLIENT_ID", "").toString
            val sandboxName: String = sparkSession.sparkContext.getConf.get("sandboxName", "").toString

   }
}
```

## データセットの読み取り

Sparkを使用する場合は、次の2つの読み取りモードにアクセスできます。インタラクティブとバッチ。

インタラクティブモードは、に対するJava Database Connectivity(JDBC)接続を作成し [!DNL Query Service] 、通常のJDBCを介して結果を取得します。このJDBCは、に自動的 `ResultSet` に変換され `DataFrame`ます。 このモードは、組み込みの [!DNL Spark] 方法と同様に機能し `spark.read.jdbc()`ます。 このモードは、小さなデータセットに対してのみ使用できます。 データセットの行数が500万行を超える場合は、バッチモードに切り替えることをお勧めします。

バッチモードでは、のCOPY[コピー]コマンド [!DNL Query Service]を使用して、共有場所にParket結果セットを生成します。 これらのパーケファイルは、後でさらに処理できます。

インタラクティブモードでのデータセットの読み取りの例を次に示します。

```scala
  // Read the configs
    val serviceToken: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_ML_TOKEN", "").toString
    val userToken: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_TOKEN", "").toString
    val orgId: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_ORG_ID", "").toString
    val apiKey: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_CLIENT_ID", "").toString
    val sandboxName: String = sparkSession.sparkContext.getConf.get("sandboxName", "").toString

 val dataSetId: String = configProperties.get(taskId).getOrElse("")

    // Load the dataset
    var df = sparkSession.read.format(PLATFORM_SDK_PQS_PACKAGE)
      .option(QSOption.userToken, userToken)
      .option(QSOption.serviceToken, serviceToken)
      .option(QSOption.imsOrg, orgId)
      .option(QSOption.apiKey, apiKey)
      .option(QSOption.mode, "interactive")
      .option(QSOption.datasetId, dataSetId)
      .option(QSOption.sandboxName, sandboxName)
      .load()
    df.show()
    df
  }
```

同様に、バッチモードでデータセットを読み取る例を次に示します。

```scala
val df = sparkSession.read.format(PLATFORM_SDK_PQS_PACKAGE)
      .option(QSOption.userToken, userToken)
      .option(QSOption.serviceToken, serviceToken)
      .option(QSOption.imsOrg, orgId)
      .option(QSOption.apiKey, apiKey)
      .option(QSOption.mode, "batch")
      .option(QSOption.datasetId, dataSetId)
      .option(QSOption.sandboxName, sandboxName)
      .load()
    df.show()
    df
```

### データセットからSELECT列を

```scala
df = df.select("column-a", "column-b").show()
```

### DISTINCT句

DISTINCT句を使用すると、行/列レベルですべての個別の値を取得し、応答からすべての重複値を削除できます。

この `distinct()` 関数の使用例を次に示します。

```scala
df = df.select("column-a", "column-b").distinct().show()
```

### WHERE句

SDKでは、次の2つの方法でフィルタリングできます。 [!DNL Spark] SQL式を使用するか、条件をフィルタリングして使用します。

これらのフィルター機能の使用例を次に示します。

#### SQL式

```scala
df.where("age > 15")
```

#### フィルター条件

```scala
df.where("age" > 15 || "name" = "Steve")
```

### ORDER BY句

ORDER BY句を使用すると、受け取った結果を、指定した列で特定の順序（昇順または降順）で並べ替えることができます。 SDKでは、これは [!DNL Spark] 関数を使用して行われ `sort()` ます。

この `sort()` 関数の使用例を次に示します。

```scala
df = df.sort($"column1", $"column2".desc)
```

### LIMIT句

LIMIT句を使用すると、データセットから受け取るレコードの数を制限できます。

この `limit()` 関数の使用例を次に示します。

```scala
df = df.limit(100)
```

## データセットへの書き込み

マッピングを使用して、を使用してExperience Platform内のデータセットに書き込むことが `configProperties` でき `QSOption`ます。

```scala
val serviceToken: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_ML_TOKEN", "").toString
val userToken: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_TOKEN", "").toString
val orgId: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_ORG_ID", "").toString
val apiKey: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_CLIENT_ID", "").toString
val sandboxName: String = sparkSession.sparkContext.getConf.get("sandboxName", "").toString 

    df.write.format(PLATFORM_SDK_PQS_PACKAGE)
      .option(QSOption.userToken, userToken)
      .option(QSOption.serviceToken, serviceToken)
      .option(QSOption.imsOrg, orgId)
      .option(QSOption.apiKey, apiKey)
      .option(QSOption.datasetId, scoringResultsDataSetId)
      .option(QSOption.sandboxName, sandboxName)
      .save()
```


## 次の手順

Adobe Experience Platformデータサイエンスワークスペースには、上記のコードサンプルを使用してデータの読み取りと書き込みを行うScala(Spark)レシピサンプルが用意されています。 Sparkを使用してデータにアクセスする方法の詳細については、 [Data Science Workspace Scala GitHub Repositoryを参照してください](https://github.com/adobe/experience-platform-dsw-reference/tree/master/recipes/scala)。