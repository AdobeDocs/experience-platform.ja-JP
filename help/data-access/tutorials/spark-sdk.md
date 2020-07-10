---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Secure Spark Data Access SDK
topic: tutorial
translation-type: tm+mt
source-git-commit: f7714b8bebe37b29290794a48314962e42b24058
workflow-type: tm+mt
source-wordcount: '523'
ht-degree: 1%

---


# Secure Spark Data Access SDK

Secure Spark [!DNL Data Access] SDKは、Adobe Experience Platformからのデータセットの読み取りと書き込みを可能にするソフトウェア開発キットです。

## はじめに

Secure Spark [SDKを呼び出す値にアクセスするには、次の](../../tutorials/authentication.md) 認証 [!DNL Data Access] チュートリアルを完了する必要があります。

- `{ACCESS_TOKEN}`
- `{API_KEY}`
- `{IMS_ORG}`

内のすべてのリソース [!DNL Experience Platform] は、特定の仮想サンドボックスに分離されます。 Spark SDKを使用するには、操作を実行するサンドボックスの名前とIDが必要です。

- `{SANDBOX_NAME}`
- `{SANDBOX_ID}`

のサンドボックスについて詳し [!DNL Platform]くは、 [Sandboxの概要ドキュメントを参照してください](../../sandboxes/home.md)。

## 環境設定

SDKでは、環境変数またはデータソースオプションで資格情報が提供されることを期待しています。 [!DNL Spark]

| Variable | 値 |
| -------- | ----- | 
| `SERVICE_TOKEN` | サービス認証トークン。 |
| `SERVICE_API_KEY` | サービスAPIキー。 これは、通常、IMSクライアントIDと同じです。 |
| `ORG_ID` | ID値。 `{IMS_ORG}` |
| `USER_TOKEN` | あなたの `{ACCESS_TOKEN}` 価値。 |

その他の設定パラメーターは次のとおりです。

| Variable | 値 |
| -------- | ----- |
| `ENVIRONMENT_NAME` | 接続しようとしている環境。 「dev」、「stage」、「prod」のいずれかです。 |
| `SANDBOX_NAME` | 接続先のサンドボックスの名前です。 |

また、次の例に示すよう `ENVIRONMENT_NAME`に、上記の環境変数を以下の変数内に設定するこ `QSOption`ともできます。

```scala
val df = spark.read
    .format("com.adobe.platform.query")
    .option(QSOption.userToken, userToken) // same as env var USER_TOKEN
    .option(QSOption.imsOrg, "69A7534C5808063C0A494234@AdobeOrg") // same as env var ORG_ID
    .option(QSOption.apiKey, "acp_foundation_queryService") // same as env var SERVICE_API_KEY
    .option("mode", "interactive")
    .option("query", "SELECT * FROM csv10000row_xcm_001 LIMIT 10")
    .load()
```

## 設置

SDKを使用するには、パフォーマンスの最適化をに追加する必要があり [!DNL Spark]`SparkSession`ます。 次のいずれかの方法を使用して、適用できます。

それを現在のSparkSessionに直接適用します。

```scala
import com.adobe.platform.query.QSOptimizations
QSOptimizations.apply(spark)
```

SparkSessionの作成前または作成時に、次のconfを設定します。

```scala
spark.sql.extensions = com.adobe.platform.query.QSSparkSessionExtensions
```

## データセットの読み取り

SDKは、次の2つの読み取りモードをサポートしています。 [!DNL Spark] インタラクティブとバッチ。

インタラクティブモードは、に対するJava Database Connectivity(JDBC)接続を作成し [!DNL Query Service] 、通常のJDBCを介して結果を取得します。このJDBCは、に自動的 `ResultSet` に変換され `DataFrame`ます。 このモードは、組み込みのSparkメソッドと同様に動作 `spark.read.jdbc()`します。 このモードは小さなデータセットに対してのみ使用でき、認証にはユーザートークンのみが必要です。

バッチモードでは、のCOPY[コピー]コマンド [!DNL Query Service]を使用して、共有場所にParket結果セットを生成します。 これらのパーケファイルは、後でさらに処理できます。 このモードには、ユーザートークンと、 `acp.foundation.catalog.credentials` スコープを持つサービストークンの両方が必要です。

インタラクティブモードでのデータセットの読み取りの例を次に示します。

```scala
val df = spark.read
      .format("com.adobe.platform.query")
      .option("user-token", {USER_TOKEN})
      .option("ims-org", {IMS_ORG})
      .option("api-key", {SERVICE_API_KEY})
      .option("mode", "interactive")
      .option("dataset-id", {DATASET_ID})
      .option("sandbox-name", {SANDBOX_NAME})
      .load()
df.show()
```

同様に、バッチモードでデータセットを読み取る例を次に示します。

```scala
val df = spark.read
      .format("com.adobe.platform.query")
      .option("user-token", {USER_TOKEN})
      .option("service-token", {SERVICE_TOKEN})
      .option("ims-org", {IMS_ORG})
      .option("api-key", {SERVICE_API_KEY})
      .option("mode", "batch")
      .option("dataset-id", {DATASET_ID})
      .option("sandbox-name", {SANDBOX_NAME})
      .load()
df.show()
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

SDKは、データセットの書き込みをサポートしています。 [!DNL Spark] ユーザーは、まず、以前のデータセットを取得して新しいデータセットに書き込む必要があります。

```scala
val df = spark.read
      .format("com.adobe.platform.query")
      .option("user-token", userToken)
      .option("ims-org", "{IMS_ORG}")
      .option("api-key", "{API_KEY}")
      .option("mode", "interactive")
      .option("dataset-id", "{DATASET_ID}")
      .load()

    df.write
      .format("com.adobe.platform.query")
      .option("user-token", {USER_TOKEN})
      .option("service-token", {SERVICE_TOKEN})
      .option("ims-org", "{IMS_ORG_ID})
      .option("api-key", "{API_KEY}")
      .option("create-dataset", "{DATASET_ID}")
      .save()
```
