---
title: API でのソース接続の change data capture の有効化
description: API でソース接続の change data capture を有効にする方法を説明します
source-git-commit: d8b4557424e1f29dfdd8893932aef914226dd60d
workflow-type: tm+mt
source-wordcount: '815'
ht-degree: 0%

---

# API でのソース接続の change data capture の有効化

Adobe Experience Platform ソースでのチェンジ・データ・キャプチャは、ソース・システムとデスティネーション・システム間のリアルタイムのデータ同期を維持するために使用できる機能です。

現在、Experience Platformでは、ソースシステムで新しく作成または更新されたレコードが、取り込まれたデータセットに定期的にコピーされるようにする **増分データコピー** をサポートしています。 このプロセスは、変更を追跡し、**新しく挿入または更新されたデータのみ** を取得するために `LastModified` など **タイムスタンプ列** を使用することに依存します。 ただし、この方法ではレコードの削除が考慮されないため、時間の経過と共にデータに不整合が生じる可能性があります。

変更データのキャプチャでは、特定のフローがキャプチャされ、挿入、更新、削除を含むすべての変更が適用されます。 同様に、Experience Platform データセットは、ソースシステムと完全に同期されたままになります。

CHANGE DATA CAPTURE は、次のソースに対して使用できます。

## [!DNL Amazon S3]

Experience Platformに取り込む `_change_request_type` ファイルに [!DNL Amazon S3] が存在することを確認します。 さらに、次の有効な値がファイルに含まれていることを確認する必要があります。

* `u`：挿入および更新用
* `d`：削除用。

ファイルに `_change_request_type` が存在しない場合は、デフォルト値の `u` が使用されます。

[!DNL Amazon S3] ソース接続の change data capture を有効にする手順については、次のドキュメントを参照してください。

* [ ベース接続  [!DNL Amazon S3]  作成 ](../api/create/cloud-storage/s3.md)。
* [ クラウドストレージのソース接続の作成 ](../api/collect/cloud-storage.md#create-a-source-connection)。

## [!DNL Azure Blob]

Experience Platformに取り込む `_change_request_type` ファイルに [!DNL Azure Blob] が存在することを確認します。 さらに、次の有効な値がファイルに含まれていることを確認する必要があります。

* `u`：挿入および更新用
* `d`：削除用。

ファイルに `_change_request_type` が存在しない場合は、デフォルト値の `u` が使用されます。

[!DNL Azure Blob] ソース接続の change data capture を有効にする手順については、次のドキュメントを参照してください。

* [ ベース接続  [!DNL Azure Blob]  作成 ](../api/create/cloud-storage/blob.md)。
* [ クラウドストレージのソース接続の作成 ](../api/collect/cloud-storage.md#create-a-source-connection)。

## [!DNL Azure Databricks]

ソース接続で change data capture を使用するには、**テーブルで** change data feed[!DNL Azure Databricks] を有効にする必要があります。

次のコマンドを使用して、[!DNL Azure Databricks] で「データフィードを変更」オプションを明示的に有効にします

**新規テーブル**

変更データフィードを新しいテーブルに適用するには、`delta.enableChangeDataFeed` コマンドでテーブルプロパティ `TRUE` を `CREATE TABLE` に設定する必要があります。

```sql
CREATE TABLE student (id INT, name STRING, age INT) TBLPROPERTIES (delta.enableChangeDataFeed = true)
```

**既存のテーブル**

変更データフィードを既存のテーブルに適用するには、`delta.enableChangeDataFeed` コマンドでテーブルプロパティ `TRUE` を `ALTER TABLE` に設定する必要があります。

```sql
ALTER TABLE myDeltaTable SET TBLPROPERTIES (delta.enableChangeDataFeed = true)
```

**すべての新規テーブル**

すべての新規テーブルに変更データフィードを適用するには、デフォルトのプロパティを `TRUE` に設定する必要があります。

```sql
set spark.databricks.delta.properties.defaults.enableChangeDataFeed = true;
```

詳しくは、[[!DNL Azure Databricks]  変更データフィードの有効化に関するガイド ](https://docs.databricks.com/aws/en/delta/delta-change-data-feed#enable-change-data-feed) を参照してください。

[!DNL Azure Databricks] ソース接続の change data capture を有効にする手順については、次のドキュメントを参照してください。

* [ ベース接続  [!DNL Azure Databricks]  作成 ](../api/create/databases/databricks.md)。
* [ データベースのソース接続の作成 ](../api/collect/database-nosql.md#create-a-source-connection)。

## [!DNL Data Landing Zone]

ソース接続で change data capture を使用するには、**テーブルで** change data feed[!DNL Data Landing Zone] を有効にする必要があります。

次のコマンドを使用して、[!DNL Data Landing Zone] で「データフィードを変更」オプションを明示的に有効にします。

[!DNL Data Landing Zone] ソース接続の change data capture を有効にする手順については、次のドキュメントを参照してください。

* [ ベース接続  [!DNL Data Landing Zone]  作成 ](../api/create/cloud-storage/data-landing-zone.md)。
* [ クラウドストレージのソース接続の作成 ](../api/collect/cloud-storage.md#create-a-source-connection)。

## [!DNL Google BigQuery]

[!DNL Google BigQuery] ソース接続でチェンジ データ キャプチャを使用するには [!DNL Google BigQuery] コンソールで [!DNL Google Cloud] ページに移動し、`enable_change_history` を `TRUE` に設定します。 このプロパティは、データ テーブルの変更履歴を有効にします。

詳しくは、[ のデータ定義言語ステートメント  [!DNL GoogleSQL]](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language#table_option_list) に関するガイドを参照してください。

[!DNL Google BigQuery] ソース接続の change data capture を有効にする手順については、次のドキュメントを参照してください。

* [ ベース接続  [!DNL Google BigQuery]  作成 ](../api/create/databases/bigquery.md)。
* [ データベースのソース接続の作成 ](../api/collect/database-nosql.md#create-a-source-connection)。

## [!DNL Google Cloud Storage]

Experience Platformに取り込む `_change_request_type` ファイルに [!DNL Google Cloud Storage] が存在することを確認します。 さらに、次の有効な値がファイルに含まれていることを確認する必要があります。

* `u`：挿入および更新用
* `d`：削除用。

ファイルに `_change_request_type` が存在しない場合は、デフォルト値の `u` が使用されます。

[!DNL Google Cloud Storage] ソース接続の change data capture を有効にする手順については、次のドキュメントを参照してください。

* [ ベース接続  [!DNL Google Cloud Storage]  作成 ](../api/create/cloud-storage/google.md)。
* [ クラウドストレージのソース接続の作成 ](../api/collect/cloud-storage.md#create-a-source-connection)。


## [!DNL SFTP]

Experience Platformに取り込む `_change_request_type` ファイルに [!DNL SFTP] が存在することを確認します。 さらに、次の有効な値がファイルに含まれていることを確認する必要があります。

* `u`：挿入および更新用
* `d`：削除用。

ファイルに `_change_request_type` が存在しない場合は、デフォルト値の `u` が使用されます。

[!DNL SFTP] ソース接続の change data capture を有効にする手順については、次のドキュメントを参照してください。

* [ ベース接続  [!DNL SFTP]  作成 ](../api/create/cloud-storage/sftp.md)。
* [ クラウドストレージのソース接続の作成 ](../api/collect/cloud-storage.md#create-a-source-connection)。


## [!DNL Snowflake]

ソース接続で Change Data Capture を使用するには、**テーブルで** 変更の追跡 [!DNL Snowflake] を有効にする必要があります。

[!DNL Snowflake] では、`ALTER TABLE` を使用し、`CHANGE_TRACKING` を `TRUE` に設定して、変更の追跡を有効にします。

```sql
ALTER TABLE mytable SET CHANGE_TRACKING = TRUE
```

詳しくは、[[!DNL Snowflake] changes 句の使用に関するガイド ](https://docs.snowflake.com/en/sql-reference/constructs/changes#usage-notes) を参照してください。

[!DNL Snowflake] ソース接続の change data capture を有効にする手順については、次のドキュメントを参照してください。

* [ ベース接続  [!DNL Snowflake]  作成 ](../api/create/databases/snowflake.md)。
* [ データベースのソース接続の作成 ](../api/collect/database-nosql.md#create-a-source-connection)。

