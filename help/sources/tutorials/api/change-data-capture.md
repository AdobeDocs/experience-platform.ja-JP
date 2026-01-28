---
title: API でのソース接続の change data capture の有効化
description: API でソース接続の change data capture を有効にする方法を説明します
exl-id: 362f3811-7d1e-4f16-b45f-ce04f03798aa
source-git-commit: bd28d5be932823b8bf9c98280f97694ff221d76d
workflow-type: tm+mt
source-wordcount: '1291'
ht-degree: 0%

---

# API でのソース接続の change data capture の有効化

>[!AVAILABILITY]
>
>VA6 データセンターに接続しながらAmazon Web Services（AWS）でAdobe Experience Platformを実行している場合、[!DNL Amazon S3] および [!DNL Data Landing Zone] ソースに対して CHANGE DATA CAPTURE を使用できるようになりました。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../landing/multi-cloud.md) を参照してください。

Adobe Experience Platform ソースでチェンジ・データ・キャプチャを使用して、ほぼリアルタイムでソース・システムとデスティネーション・システムの同期を維持します。

Experience Platformは現在、**増分データコピー** をサポートしています。このコピーは、新しく作成または更新されたレコードを、ソースシステムから取り込んだデータセットに定期的に転送します。 この方法は、変更を追跡するために **timestamp 列** に依存しますが、時間の経過に伴うデータの不整合を引き起こす可能性がある削除は検出しません。

これに対し、チェンジ・データ・キャプチャは、ほぼリアルタイムで挿入、更新、削除をキャプチャして適用します。 この包括的な変更の履歴管理により、データセットがソース・システムと完全に整合した状態で維持され、差分コピーのサポート以外の完全な変更履歴が提供されます。 ただし、削除操作はターゲットデータセットを使用するすべてのアプリケーションに影響を与えるので、特別な考慮が必要です。

Experience Platformでのチェンジ データ キャプチャには、**[Data Mirror](../../../xdm/data-mirror/overview.md)** （[ リレーショナル スキーマ ](../../../xdm/schema/relational.md)）が必要です。 Data Mirrorに変更データを提供するには、次の 2 つの方法があります。

* **[手動の変更トラッキング](#file-based-sources)**：変更データキャプチャレコードをネイティブに生成しないソースのデータセットに `_change_request_type` の列を含めます
* **[ネイティブ変更データ・キャプチャのエクスポート](#database-sources)**：ソース・システムから直接書き出された変更データ・キャプチャ・レコードを使用します

どちらのアプローチでも、関係を保持し一意性を確保するには、リレーショナルスキーマを使用したData Mirrorが必要です。

## リレーショナルスキーマを使用したData Mirror

>[!AVAILABILITY]
>
>Data Mirrorおよびリレーショナルスキーマは、Adobe Journey Optimizer **オーケストレートキャンペーン** のライセンスホルダーが利用できます。 ライセンスとイネーブルメント機能に応じて、Customer Journey Analytics ユーザー向けの **限定リリース** としても使用できます。 アクセスについては、Adobe担当者にお問い合わせください。

>[!NOTE]
>
>**調整されたキャンペーンユーザー**：このドキュメントで説明されているData Mirror機能を使用して、参照整合性を維持するカスタマーデータを操作します。 ソースが CHANGE DATA CAPTURE フォーマットを使用していない場合でも、Data Mirrorは、プライマリキーの適用、レコードレベルのアップサート、スキーマの関係などのリレーショナル機能をサポートします。 これらの機能により、接続されたデータセット間で一貫した信頼性の高いデータモデリングが可能になります。

Data Mirrorでは、リレーショナルスキーマを使用してチェンジ・データ・キャプチャを拡張し、高度なデータベース同期機能を有効にします。 Data Mirrorの概要については、[Data Mirrorの概要 ](../../../xdm/data-mirror/overview.md) を参照してください。

リレーショナルスキーマは、Experience Platformを拡張して、プライマリキーの一意性の確保、行レベルの変更の追跡、スキーマレベルの関係の定義を行います。 チェンジ・データ・キャプチャでは、挿入、更新、削除をデータ・レイク内で直接適用し、抽出、変換、ロード（ETL）または手動の紐付けの必要性を減らします。

詳しくは、[ リレーショナルスキーマの概要 ](../../../xdm/schema/relational.md) を参照してください。

### 変更データ取得のリレーショナル・スキーマ要件

変更データ・キャプチャでリレーショナル・スキーマを使用する前に、次の識別子を設定します。

* 各レコードをプライマリキーで一意に識別します。
* バージョン識別子を使用して、更新を順番に適用します。
* 時系列スキーマの場合は、タイムスタンプ識別子を追加します。

### 列の処理を制御 {#control-column-handling}

`_change_request_type` の列を使用して、各行の処理方法を指定します。

* `u` — アップサート（列がない場合はデフォルト）
* `d` – 削除

この列は取り込み時にのみ評価され、XDM フィールドには保存もマッピングもされません。

### ワークフロー {#workflow}

リレーショナルスキーマを使用してチェンジ・データ・キャプチャを有効にする手順は、次のとおりです。

1. リレーショナルスキーマを作成します。
2. 必要な記述子を追加します。
   * [プライマリキー記述子](../../../xdm/api/descriptors.md#primary-key-descriptor)
   * [バージョン記述子](../../../xdm/api/descriptors.md#version-descriptor)
   * [ タイムスタンプ記述子 ](../../../xdm/api/descriptors.md#timestamp-descriptor) （時系列のみ）
3. スキーマからデータセットを作成し、変更データキャプチャを有効にします。
4. ファイルベースの取り込みのみ：削除操作を明示的に指定する必要がある場合は、ソースファイルに `_change_request_type` 列を追加します。 CDC の書き出し設定では、データベースソースに対してこれを自動的に処理します。
5. ソース接続の設定を完了して取り込みを有効にします。

>[!NOTE]
>
>`_change_request_type` 列は、行レベルの変更動作を明示的に制御する場合に、ファイルベースのソース（Amazon S3、Azure Blob、Google Cloud Storage、SFTP）にのみ必要です。 ネイティブの CDC 機能を持つデータベース・ソースの場合、変更操作は CDC エクスポート構成を通じて自動的に処理されます。 ファイルベースの取り込みは、デフォルトでアップサート操作を想定します。ファイルアップロードで削除操作を指定する場合にのみ、この列を追加する必要があります。

>[!IMPORTANT]
>
>**データ削除の計画が必要です**。 リレーショナルスキーマを使用するすべてのアプリケーションは、変更データキャプチャを実装する前に、削除の影響を理解する必要があります。 削除が関連するデータセット、コンプライアンス要件およびダウンストリームプロセスに与える影響を計画します。 詳しくは、[ データハイジーンに関する考慮事項 ](../../../hygiene/ui/record-delete.md#relational-record-delete) を参照してください。

## ファイルベースのソースの変更データの提供 {#file-based-sources}

>[!IMPORTANT]
>
>ファイルベースのチェンジ・データ・キャプチャには、リレーショナル・スキーマを使用したData Mirrorが必要です。 以下のファイル形式設定手順に従う前に、このドキュメントで前述した [Data Mirror セットアップワークフローを完了してい ](#workflow) ことを確認してください。 次の手順では、Data Mirrorで処理される変更のトラッキング情報を含めるようにデータファイルをフォーマットする方法について説明します。

ファイルベースのソース（[!DNL Amazon S3]、[!DNL Azure Blob]、[!DNL Google Cloud Storage] および [!DNL SFTP]）の場合は、ファイルに `_change_request_type` 列を含めます。

上記の `_change_request_type` コントロール列の処理 [ の節で定義した ](#control-column-handling) の値を使用します。

>[!IMPORTANT]
>
>**ファイルベースのソースのみ** の場合、変更追跡機能を検証するために、特定のアプリケーションで、`_change_request_type` （アップサート）または `u` （削除）を含む `d` 列が必要になる場合があります。 例えば、Adobe Journey Optimizerの **オーケストレートキャンペーン** 機能では、「オーケストレートキャンペーン」切替スイッチを有効にし、ターゲティング用のデータセット選択を許可するために、この列が必要です。 アプリケーション固有の検証要件は異なる場合があります。

次のソース固有の手順に従います。

### クラウドストレージソース {#cloud-storage-sources}

次の手順に従って、クラウドストレージソースの CHANGE DATA CAPTURE を有効にします。

1. ソースのベース接続を作成します。

   | ソース | ベース接続ガイド |
   |---|---|
   | [!DNL Amazon S3] | [ ベ  [!DNL Amazon S3]  ス接続の作成 ](../api/create/cloud-storage/s3.md) |
   | [!DNL Azure Blob] | [ ベ  [!DNL Azure Blob]  ス接続の作成 ](../api/create/cloud-storage/blob.md) |
   | [!DNL Google Cloud Storage] | [ ベ  [!DNL Google Cloud Storage]  ス接続の作成 ](../api/create/cloud-storage/google.md) |
   | [!DNL SFTP] | [ ベ  [!DNL SFTP]  ス接続の作成 ](../api/create/cloud-storage/sftp.md) |

2. [ クラウドストレージのソース接続の作成 ](../api/collect/cloud-storage.md#create-a-source-connection)。

すべてのクラウドストレージソースは、上記の `_change_request_type` ファイルベースのソース [ の節で説明したのと同じ ](#file-based-sources) 列形式を使用します。

## データベースソース {#database-sources}

### [!DNL Azure Databricks]

[!DNL Azure Databricks] でチェンジ・データ・キャプチャを使用するには、ソース・テーブルで **チェンジ・データ・フィード** を使用可能にし、Experience PlatformでData Mirrorをリレーショナル・スキーマとともに構成する必要があります。

以下のコマンドを使用して、テーブルで変更データフィードを有効にします。

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

### [!DNL Data Landing Zone]

[!DNL Data Landing Zone] でチェンジ・データ・キャプチャを使用するには、ソース・テーブルで **チェンジ・データ・フィード** を使用可能にし、Experience PlatformでData Mirrorをリレーショナル・スキーマとともに構成する必要があります。

[!DNL Data Landing Zone] ソース接続の change data capture を有効にする手順については、次のドキュメントを参照してください。

* [ ベース接続  [!DNL Data Landing Zone]  作成 ](../api/create/cloud-storage/data-landing-zone.md)。
* [ クラウドストレージのソース接続の作成 ](../api/collect/cloud-storage.md#create-a-source-connection)。

### [!DNL Google BigQuery]

[!DNL Google BigQuery] でチェンジ・データ・キャプチャを使用するには、ソース・テーブルで変更履歴を使用可能にし、Experience PlatformでData Mirrorをリレーショナル・スキーマとともに構成する必要があります。

[!DNL Google BigQuery] ソース接続の変更履歴を有効にするには、[!DNL Google BigQuery] コンソールで [!DNL Google Cloud] ページに移動し、`enable_change_history` を `TRUE` に設定します。 このプロパティは、データ テーブルの変更履歴を有効にします。

詳しくは、[ のデータ定義言語ステートメント  [!DNL GoogleSQL]](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language#table_option_list) に関するガイドを参照してください。

[!DNL Google BigQuery] ソース接続の change data capture を有効にする手順については、次のドキュメントを参照してください。

* [ ベース接続  [!DNL Google BigQuery]  作成 ](../api/create/databases/bigquery.md)。
* [ データベースのソース接続の作成 ](../api/collect/database-nosql.md#create-a-source-connection)。

### [!DNL Snowflake]

[!DNL Snowflake] でチェンジ・データ・キャプチャを使用するには、ソース・テーブルで **チェンジ・トラッキング** を使用可能にし、Experience PlatformでData Mirrorをリレーショナル・スキーマとともに構成する必要があります。

[!DNL Snowflake] では、`ALTER TABLE` を使用し、`CHANGE_TRACKING` を `TRUE` に設定して、変更の追跡を有効にします。

```sql
ALTER TABLE mytable SET CHANGE_TRACKING = TRUE
```

詳しくは、[[!DNL Snowflake] changes 句の使用に関するガイド ](https://docs.snowflake.com/en/sql-reference/constructs/changes#usage-notes) を参照してください。

[!DNL Snowflake] ソース接続の change data capture を有効にする手順については、次のドキュメントを参照してください。

* [ ベース接続  [!DNL Snowflake]  作成 ](../api/create/databases/snowflake.md)。
* [ データベースのソース接続の作成 ](../api/collect/database-nosql.md#create-a-source-connection)。
