---
keywords: Experience Platform；ホーム；人気のあるトピック；データアクセス；spark sdk；データアクセス api;spark レシピ；読み取り spark；書き込み spark
solution: Experience Platform
title: Data Science Workspace の Spark を使用したデータへのアクセス
topic-legacy: tutorial
type: Tutorial
description: 次のドキュメントには、Data Science Workspace で使用する Spark を使用してデータにアクセスする方法の例が含まれています。
exl-id: 9bffb52d-1c16-4899-b455-ce570d76d3b4
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 0%

---

# Data Science Workspace の Spark を使用したデータへのアクセス

次のドキュメントには、Data Science Workspace で使用する Spark を使用してデータにアクセスする方法の例が含まれています。 JupyterLab ノートブックを使用したデータへのアクセスについては、 [JupyterLab ノートブックデータアクセス ](../jupyterlab/access-notebook-data.md) のドキュメントを参照してください。

## はじめに

[!DNL Spark] を使用する場合は、`SparkSession` に追加する必要のあるパフォーマンスの最適化が必要です。 さらに、後でデータセットに読み書きする `configProperties` を設定することもできます。

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

Spark の使用時は、次の 2 つの読み取りモードにアクセスできます。インタラクティブおよびバッチ。

インタラクティブモードは、[!DNL Query Service] への Java Database Connectivity(JDBC) 接続を作成し、`DataFrame` に自動的に変換される通常の JDBC `ResultSet` を通じて結果を取得します。 このモードは、組み込みの [!DNL Spark] メソッド `spark.read.jdbc()` と同様に機能します。 このモードは、小さなデータセットにのみ使用します。 データセットの行数が 500 万行を超える場合は、バッチモードに置き換えることをお勧めします。

バッチモードでは、[!DNL Query Service] の COPY コマンドを使用して、共有場所に Parquet 結果セットを生成します。 これらの Parquet ファイルは、さらに処理できます。

インタラクティブモードでのデータセットの読み取りの例を以下に示します。

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

### データセットからの SELECT 列

```scala
df = df.select("column-a", "column-b").show()
```

### DISTINCT 句

DISTINCT 句を使用すると、行/列レベルですべてのユニーク値をフェッチし、重複する値をすべてレスポンスから削除できます。

`distinct()` 関数の使用例を次に示します。

```scala
df = df.select("column-a", "column-b").distinct().show()
```

### WHERE 句

[!DNL Spark] SDK では、次の 2 つの方法でフィルタリングできます。SQL 式を使用するか、条件をフィルターして使用する。

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

ORDER BY 句を使用すると、受け取った結果を特定の列で特定の順序（昇順または降順）で並べ替えることができます。 [!DNL Spark] SDK では、`sort()` 関数を使用してこれをおこないます。

`sort()` 関数の使用例を次に示します。

```scala
df = df.sort($"column1", $"column2".desc)
```

### LIMIT 句

LIMIT 句を使用すると、データセットから受信するレコードの数を制限できます。

`limit()` 関数の使用例を次に示します。

```scala
df = df.limit(100)
```

## データセットへの書き込み

`configProperties` マッピングを使用すると、`QSOption` を使用してExperience Platform内のデータセットに書き込むことができます。

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

Adobe Experience Platform Data Science Workspace には、上記のコードサンプルを使用してデータの読み取りと書き込みをおこなう Scala(Spark) レシピサンプルが用意されています。 Spark を使用してデータにアクセスする方法の詳細については、[Data Science Workspace Scala GitHub リポジトリ ](https://github.com/adobe/experience-platform-dsw-reference/tree/master/recipes/scala) を参照してください。
