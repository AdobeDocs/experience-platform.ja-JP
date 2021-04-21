---
keywords: Experience Platform；ホーム；人気のあるトピック；データアクセス；spark sdk；データアクセスapi;sparkレシピ；読み取りspark；書き込みspark
solution: Experience Platform
title: Data Science WorkspaceのSparkを使用したデータへのアクセス
topic-legacy: tutorial
type: Tutorial
description: 次のドキュメントには、Data Science Workspaceで使用するSparkを使用してデータにアクセスする方法の例が含まれています。
exl-id: 9bffb52d-1c16-4899-b455-ce570d76d3b4
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 0%

---

# Data Science WorkspaceのSparkを使用したデータへのアクセス

次のドキュメントには、Data Science Workspaceで使用するSparkを使用してデータにアクセスする方法の例が含まれています。 JupterLabノートブックを使用したデータへのアクセスについては、[JupyterLabノートブックデータアクセス](../jupyterlab/access-notebook-data.md)のドキュメントを参照してください。

## はじめに

[!DNL Spark]を使用するには、`SparkSession`に追加する必要のあるパフォーマンスの最適化が必要です。 また、後でデータセットに対して読み書きを行う`configProperties`を設定することもできます。

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
            val userToken: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_TOKEN", "").toString
            val orgId: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_ORG_ID", "").toString
            val apiKey: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_CLIENT_ID", "").toString
            val sandboxName: String = sparkSession.sparkContext.getConf.get("sandboxName", "").toString

   }
}
```

## データセットの読み取り

Sparkを使用する場合は、次の2つの読み取りモードにアクセスできます。インタラクティブとバッチ。

インタラクティブモードは、[!DNL Query Service]へのJava Database Connectivity(JDBC)接続を作成し、通常のJDBC `ResultSet`を介して結果を取得します。この結果は、`DataFrame`に自動的に変換されます。 このモードは、組み込みの[!DNL Spark]メソッド`spark.read.jdbc()`と同様に動作します。 このモードは、小さなデータセットに対してのみ使用できます。 データセットの行数が500万行を超える場合は、バッチモードに切り替えることをお勧めします。

バッチモードは、[!DNL Query Service]のCOPYコマンドを使用して、共有場所にParket結果セットを生成します。 これらのパーケファイルは、後でさらに処理できます。

インタラクティブモードでのデータセットの読み取りの例を次に示します。

```scala
  // Read the configs
    val userToken: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_TOKEN", "").toString
    val orgId: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_ORG_ID", "").toString
    val apiKey: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_CLIENT_ID", "").toString
    val sandboxName: String = sparkSession.sparkContext.getConf.get("sandboxName", "").toString

 val dataSetId: String = configProperties.get(taskId).getOrElse("")

    // Load the dataset
    var df = sparkSession.read.format(PLATFORM_SDK_PQS_PACKAGE)
      .option(QSOption.userToken, userToken)
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

`distinct()`関数の使用例を次に示します。

```scala
df = df.select("column-a", "column-b").distinct().show()
```

### WHERE句

[!DNL Spark] SDKでは、次の2つのフィルタリング方法が可能です。SQL式を使用するか、条件をフィルタリングして使用します。

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

ORDER BY句を使用すると、受け取った結果を、指定した列で特定の順序（昇順または降順）で並べ替えることができます。 [!DNL Spark] SDKでは、`sort()`関数を使用して行います。

`sort()`関数の使用例を次に示します。

```scala
df = df.sort($"column1", $"column2".desc)
```

### LIMIT句

LIMIT句を使用すると、データセットから受け取るレコードの数を制限できます。

`limit()`関数の使用例を次に示します。

```scala
df = df.limit(100)
```

## データセットへの書き込み

`configProperties`マッピングを使用して、`QSOption`を使用してExperience Platform内のデータセットに書き込むことができます。

```scala
val userToken: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_TOKEN", "").toString
val orgId: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_ORG_ID", "").toString
val apiKey: String = sparkSession.sparkContext.getConf.get("ML_FRAMEWORK_IMS_CLIENT_ID", "").toString
val sandboxName: String = sparkSession.sparkContext.getConf.get("sandboxName", "").toString 

    df.write.format(PLATFORM_SDK_PQS_PACKAGE)
      .option(QSOption.userToken, userToken)
      .option(QSOption.imsOrg, orgId)
      .option(QSOption.apiKey, apiKey)
      .option(QSOption.datasetId, scoringResultsDataSetId)
      .option(QSOption.sandboxName, sandboxName)
      .save()
```


## 次の手順

Adobe Experience Platformデータサイエンスワークスペースには、上記のコードサンプルを使用してデータの読み取りと書き込みを行うScala(Spark)レシピサンプルが用意されています。 Sparkを使用してデータにアクセスする方法の詳細については、[Data Science Workspace Scala GitHub Repository](https://github.com/adobe/experience-platform-dsw-reference/tree/master/recipes/scala)を参照してください。
