---
keywords: Experience Platform；ホーム；人気のトピック；データアクセス；spark sdk;data access api;spark レシピ；spark の読み取り；spark の書き込み
solution: Experience Platform
title: Data Science Workspaceでの Spark を使用したデータへのアクセス
type: Tutorial
description: 次のドキュメントでは、Data Science Workspaceで使用する Spark を使用してデータにアクセスする方法の例を示します。
exl-id: 9bffb52d-1c16-4899-b455-ce570d76d3b4
source-git-commit: 5d98dc0cbfaf3d17c909464311a33a03ea77f237
workflow-type: tm+mt
source-wordcount: '467'
ht-degree: 0%

---

# データサイエンスWorkspaceでの Spark を使用したデータへのアクセス

>[!NOTE]
>
>データサイエンスワークスペースは購入できなくなりました。
>
>このドキュメントは、以前に データ Science ワークスペース の利用資格を持つ既存のお客様を対象としています。

次のドキュメントには、データ Science ワークスペースで使用するために Spark を使用してデータにアクセスする方法の例が含まれています。 JupyterLab ノートブックを使用したデータへのアクセスについては、 [JupyterLab ノートブックのデータアクセス](../jupyterlab/access-notebook-data.md) ドキュメント訪問を参照してください。

## はじめに

[!DNL Spark]を使用するには、`SparkSession`に追加する必要があるパフォーマンスの最適化が必要です。さらに、後でデータセットを読み書きできるように `configProperties` を設定することもできます。

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

## データセット読み上げ

Spark を使用する際は、インタラクティブとバッチの 2 つの読み取りモードにアクセスできます。

インタラクティブモードは、[!DNL Query Service] への Java Database Connectivity （JDBC）接続を作成し、`DataFrame` に自動変換される通常の JDBC `ResultSet` を通じて結果を取得します。 このモードは、組み込みの [!DNL Spark] メソッド `spark.read.jdbc()` と同様に機能します。 このモードは、小規模なデータセットのみを対象としています。 データセットが 500 万行を超える場合は、バッチモードにスワップすることをお勧めします。

バッチモードでは、 [!DNL Query Service] の COPY コマンドを使用して、共有の場所に Parquet の結果セットを生成します。 これらの Parquet ファイルは、その後、さらに処理することができます。

インタラクティブモードでデータセットを読み取る例を以下に示します。

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

同様に、バッチモードでデータセットを読み取る例を以下に示します。

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

DISTINCT 句を使用すると、行/列レベルですべての個別の値をフェッチし、応答からすべての重複値を削除できます。

`distinct()`関数の使用例を以下に示します。

```scala
df = df.select("column-a", "column-b").distinct().show()
```

### WHERE 句

[!DNL Spark] SDK でフィルタリングできる方法は、SQL 式を使用する方法と、条件を使用したフィルタリング方法の 2 つです。

これらのフィルター関数の使用例を次に示します。

#### SQL 式

```scala
df.where("age > 15")
```

#### フィルター条件

```scala
df.where("age" > 15 || "name" = "Steve")
```

### ORDER BY 句

ORDER BY 句を使用すると、受信した結果を指定された列で特定の順序 (昇順または降順順) で並べ替えることができます。 [!DNL Spark] SDK では、`sort()` 関数を使用してこれを行います。

`sort()`関数の使用例を以下に示します。

```scala
df = df.sort($"column1", $"column2".desc)
```

### LIMIT 句

LIMIT 句を使用すると、データセットから受信するレコードの数を制限できます。

`limit()`関数の使用例を以下に示します。

```scala
df = df.limit(100)
```

## データセットへの書き込み

`configProperties` マッピングを使用すると、`QSOption` を使用してExperience Platformのデータセットに書き込むことができます。

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

Adobe Experience Platform データ Science ワークスペース には、上記のコード サンプルを使用してデータの読み取りと書き込みを行う Scala (Spark) レシピ サンプルが用意されています。 Spark を使用してデータにアクセスする方法の詳細については、 [データ Science ワークスペース Scala GitHub Repository](https://github.com/adobe/experience-platform-dsw-reference/tree/master/recipes/scala) を参照してください。
