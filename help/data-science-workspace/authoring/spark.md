---
keywords: Experience Platform；ホーム；人気のトピック；データアクセス；spark sdk；データアクセス api;spark レシピ；read spark;write spark
solution: Experience Platform
title: Data Science Workspace の Spark を使用したデータへのアクセス
type: Tutorial
description: 次のドキュメントには、Spark を使用して Data Science Workspace でデータにアクセスし、使用する方法の例が含まれています。
exl-id: 9bffb52d-1c16-4899-b455-ce570d76d3b4
source-git-commit: 86e6924078c115fb032ce39cd678f1d9c622e297
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 0%

---

# Data Science Workspace の Spark を使用したデータへのアクセス

次のドキュメントには、Spark を使用して Data Science Workspace でデータにアクセスし、使用する方法の例が含まれています。 JupyterLab ノートブックを使用したデータへのアクセスについては、 [JupyterLab ノートブックのデータアクセス](../jupyterlab/access-notebook-data.md) ドキュメント。

## はじめに

使用 [!DNL Spark] には、 `SparkSession`. また、 `configProperties` 後でデータセットに読み書きする場合に使用します。

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

Spark を使用する場合は、次の 2 つの読み取りモードにアクセスできます。インタラクティブおよびバッチ。

インタラクティブモードは、Java Database Connectivity(JDBC) 接続を [!DNL Query Service] とは、通常の JDBC を通じて結果を取得します。 `ResultSet` が `DataFrame`. このモードは、組み込みの [!DNL Spark] メソッド `spark.read.jdbc()`. このモードは、小さなデータセットにのみ使用します。 データセットの行数が 500 万行を超える場合は、バッチモードに置き換えることをお勧めします。

バッチモードでは [!DNL Query Service]の COPY コマンドを使用して、共有場所に Parquet 結果セットを生成します。 これらの Parquet ファイルは、後で処理できます。

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

同様に、バッチモードでのデータセットの読み取りの例を次に示します。

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

### データセットから列を選択

```scala
df = df.select("column-a", "column-b").show()
```

### DISTINCT 句

DISTINCT 句を使用すると、行/列レベルですべてのユニーク値を取得し、応答からすべての重複値を削除できます。

の使用例 `distinct()` 関数は次のように表示されます。

```scala
df = df.select("column-a", "column-b").distinct().show()
```

### WHERE 句

この [!DNL Spark] SDK では、次の 2 つのフィルタリング方法を使用できます。SQL 式を使用するか、条件をフィルタリングして使用します。

これらのフィルタリング関数の使用例を以下に示します。

#### SQL 式

```scala
df.where("age > 15")
```

#### フィルター条件

```scala
df.where("age" > 15 || "name" = "Steve")
```

### ORDER BY 句

ORDER BY 句を使用すると、受け取った結果を特定の順序（昇順または降順）で指定した列で並べ替えることができます。 内 [!DNL Spark] SDK の場合、これは `sort()` 関数に置き換えます。

の使用例 `sort()` 関数は次のように表示されます。

```scala
df = df.sort($"column1", $"column2".desc)
```

### LIMIT 句

LIMIT 句を使用すると、データセットから受け取るレコードの数を制限できます。

の使用例 `limit()` 関数は次のように表示されます。

```scala
df = df.limit(100)
```

## データセットへの書き込み

を使用して、 `configProperties` マッピングを使用する場合は、次を使用してExperience Platform内のデータセットに書き込むことができます `QSOption`.

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

Adobe Experience Platform Data Science Workspace には、上記のコードサンプルを使用してデータの読み取りと書き込みをおこなう Scala(Spark) レシピのサンプルが用意されています。 Spark を使用してデータにアクセスする方法の詳細については、 [Data Science Workspace Scala GitHub リポジトリ](https://github.com/adobe/experience-platform-dsw-reference/tree/master/recipes/scala).
