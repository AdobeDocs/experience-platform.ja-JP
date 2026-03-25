---
title: APIでソース接続の変更データキャプチャを有効にする
description: APIでソース接続の変更データキャプチャを有効にする方法を説明します
exl-id: 362f3811-7d1e-4f16-b45f-ce04f03798aa
source-git-commit: 74743d7dc93e2ba291481ad11e923d28088c4903
workflow-type: tm+mt
source-wordcount: '1294'
ht-degree: 0%

---

# APIでソース接続の変更データキャプチャを有効にする

>[!AVAILABILITY]
>
>* 変更データキャプチャは、次のソースでサポートされています：[!DNL Amazon S3]、[!DNL Data Landing Zone]、[!DNL Marketo Engage]、[!DNL Microsoft Dynamics]、および[!DNL Salesforce]。
>
>* また、VA6 データセンターでAmazon Web Services （AWS）でAdobe Experience Platformを使用する場合は、[!DNL Amazon S3]および[!DNL Data Landing Zone]のソースに対して変更データキャプチャを有効にすることもできます。 AWS版Experience Platformは、現在、一部のユーザーのみが利用できます。 インフラストラクチャのサポートについて詳しくは、[Experience Platform マルチクラウドの概要](../../../landing/multi-cloud.md)を参照してください。

Adobe Experience Platformのソースで変更データキャプチャを使用すると、ソースと宛先のシステムをほぼリアルタイムで同期できます。

Experience Platformは現在&#x200B;**増分データコピー**&#x200B;をサポートしており、新しく作成または更新されたレコードをソースシステムから取り込まれたデータセットに定期的に転送します。 この方法は、変更を追跡するために&#x200B;**タイムスタンプ列**&#x200B;に依存していますが、削除を検出せず、時間の経過に伴ってデータの不整合が生じる可能性があります。

これとは対照的に、変更データキャプチャは、ほぼリアルタイムで挿入、更新、削除をキャプチャして適用します。 この包括的な変更追跡により、データセットがソースシステムと完全に連携し、増分コピーがサポートするものを超えて、完全な変更履歴が提供されます。 ただし、削除操作は、ターゲットデータセットを使用するすべてのアプリケーションに影響を与えるため、特別な配慮が必要です。

Experience Platformでのデータキャプチャの変更には、**[リレーショナルスキーマ](../../../xdm/data-mirror/overview.md)**&#x200B;を持つ[Data Mirror](../../../xdm/schema/relational.md)が必要です。 Data Mirrorには、次の2つの方法で変更データを提供できます。

* **[手動変更追跡](#file-based-sources)**：変更データキャプチャレコードをネイティブに生成しないソースのデータセットに`_change_request_type`列を含めます
* **[ネイティブの変更データキャプチャの書き出し](#database-sources)**：ソースシステムから直接書き出された変更データキャプチャレコードを使用

どちらのアプローチも、関係を維持し、一意性を適用するために、リレーショナルスキーマを備えたData Mirrorが必要です。

## Data Mirrorとリレーショナルスキーマ

>[!AVAILABILITY]
>
>Data Mirrorとリレーショナルスキーマは、Adobe Journey Optimizer **オーケストレーションキャンペーン**&#x200B;のライセンス保有者が利用できます。 また、ライセンスと機能の有効化に応じて、Customer Journey Analytics ユーザー向けの&#x200B;**限定リリース**&#x200B;としても利用できます。 アクセスについては、Adobe担当者にお問い合わせください。

>[!NOTE]
>
>**キャンペーンのオーケストレーション ユーザー**：このドキュメントに記載されているData Mirror機能を使用して、参照整合性を維持する顧客データを操作します。 ソースでデータキャプチャの変更フォーマットを使用していない場合でも、Data Mirrorでは、プライマリキーの適用、レコードレベルのアップサート、スキーマの関連付けなどのリレーショナル機能をサポートしています。 これらの機能により、接続されたデータセットをまたいで、一貫性のある信頼性の高いデータモデリングを実現できます。

Data Mirrorでは、リレーショナルスキーマを使用してchange data captureを拡張し、高度なデータベース同期機能を有効にします。 Data Mirrorの概要については、[Data Mirrorの概要](../../../xdm/data-mirror/overview.md)を参照してください。

リレーショナルスキーマ Experience Platformを拡張して、プライマリキーの一意性を適用したり、行レベルの変更を追跡したり、スキーマレベルの関係を定義したりできます。 変更データキャプチャを使用すると、データレイクに直接挿入、更新、削除が適用され、ETL （抽出、変換、格納）や手作業による紐付けの必要性が低減されます。

詳しくは、[ リレーショナルスキーマの概要](../../../xdm/schema/relational.md)を参照してください。

### 変更データ取得の関係スキーマ要件

変更データキャプチャでリレーショナルスキーマを使用する前に、次の識別子を設定します。

* プライマリキーを使用して各レコードを一意に識別します。
* バージョン識別子を使用して、更新を順番に適用します。
* 時系列スキーマの場合は、タイムスタンプ IDを追加します。

### 列の処理を制御 {#control-column-handling}

`_change_request_type`列を使用して、各行の処理方法を指定します。

* `u` — upsert （列が存在しない場合のデフォルト）
* `d` – 削除

この列は取り込み中にのみ評価され、XDM フィールドに保存またはマッピングされません。

### ワークフロー {#workflow}

リレーショナルスキーマで変更データキャプチャを有効にするには：

1. 関係スキーマの作成：
2. 必要な記述子を追加します。
   * [プライマリキー記述子](../../../xdm/api/descriptors.md#primary-key-descriptor)
   * [バージョン記述子](../../../xdm/api/descriptors.md#version-descriptor)
   * [ タイムスタンプ記述子](../../../xdm/api/descriptors.md#timestamp-descriptor) （時系列のみ）
3. スキーマからデータセットを作成し、変更データキャプチャを有効にします。
4. ファイルベースの取り込みのみ：削除操作を明示的に指定する必要がある場合は、ソースファイルに`_change_request_type`列を追加します。 CDC書き出し設定では、データベースソースに対してこれを自動的に処理します。
5. 取り込みを有効にするには、ソース接続の設定を完了します。

>[!NOTE]
>
>行レベルの変更動作を明示的に制御する場合は、ファイルベースのソース（Amazon S3、Azure Blob、Google Cloud Storage、SFTP）に対してのみ`_change_request_type`列が必要です。 ネイティブのCDC機能を持つデータベースソースの場合、変更操作はCDC書き出し設定を通じて自動的に処理されます。 ファイルベースの取り込みは、デフォルトでアップサート操作を前提としています。この列を追加する必要があるのは、ファイルアップロードで削除操作を指定する場合のみです。

>[!IMPORTANT]
>
>**データ削除計画が必要です**。 関係スキーマを使用するすべてのアプリケーションは、変更データキャプチャを実装する前に、削除の影響を理解する必要があります。 関連するデータセット、コンプライアンス要件、下流のプロセスに削除がどのように影響するかを計画します。 ガイダンスについては、[ データハイジーンに関する考慮事項](../../../hygiene/ui/record-delete.md#relational-record-delete)を参照してください。

## ファイルベースのソースに変更データを提供する {#file-based-sources}

>[!IMPORTANT]
>
>ファイルベースの変更データキャプチャには、リレーショナルスキーマを備えたData Mirrorが必要です。 以下のファイル形式の手順に従う前に、このドキュメントで前述した[Data Mirror設定ワークフロー](#workflow)を完了していることを確認してください。 次の手順では、Data Mirrorで処理される変更履歴を含めるようにデータファイルをフォーマットする方法について説明します。

ファイルベースのソース （[!DNL Amazon S3]、[!DNL Azure Blob]、[!DNL Google Cloud Storage]、および[!DNL SFTP]）の場合は、ファイルに`_change_request_type`列を含めます。

上記の`_change_request_type` コントロール列の処理[ セクションで定義されている](#control-column-handling)値を使用します。

>[!IMPORTANT]
>
>**ファイルベースのソースのみ**&#x200B;の場合、特定のアプリケーションでは、変更追跡機能を検証するために、`_change_request_type` （アップサート）または`u` （削除）を含む`d`列が必要になる場合があります。 例えば、Adobe Journey Optimizerの&#x200B;**オーケストレーションキャンペーン**&#x200B;機能では、この列で「オーケストレーションキャンペーン」トグルを有効にし、ターゲティングにデータセットの選択を許可する必要があります。 アプリケーション固有の検証要件は異なる場合があります。

以下のソース固有の手順に従います。

### クラウドストレージソース {#cloud-storage-sources}

次の手順に従って、クラウドストレージソースの変更データキャプチャを有効にします。

1. ソースのベース接続を作成します。

   | ソース | ベース接続ガイド |
   |---|---|
   | [!DNL Amazon S3] | [ ベース接続 [!DNL Amazon S3] を作成](../api/create/cloud-storage/s3.md) |
   | [!DNL Azure Blob] | [ ベース接続 [!DNL Azure Blob] を作成](../api/create/cloud-storage/blob.md) |
   | [!DNL Google Cloud Storage] | [ ベース接続 [!DNL Google Cloud Storage] を作成](../api/create/cloud-storage/google.md) |
   | [!DNL SFTP] | [ ベース接続 [!DNL SFTP] を作成](../api/create/cloud-storage/sftp.md) |

2. [ クラウドストレージのソース接続を作成](../api/collect/cloud-storage.md#create-a-source-connection)。

すべてのクラウドストレージソースは、上記の「`_change_request_type` ファイルベースのソース [」セクションで説明した同じ](#file-based-sources)列形式を使用します。

## データベースソース {#database-sources}

### [!DNL Azure Databricks]

[!DNL Azure Databricks]でchange data captureを使用するには、ソーステーブルで&#x200B;**change data feed**&#x200B;を有効にし、Experience PlatformのリレーショナルスキーマでData Mirrorを設定する必要があります。

次のコマンドを使用して、テーブルのデータフィードの変更を有効にします。

**新しいテーブル**

変更データ フィードを新しいテーブルに適用するには、`delta.enableChangeDataFeed` コマンドでテーブル プロパティ `TRUE`を`CREATE TABLE`に設定する必要があります。

```sql
CREATE TABLE student (id INT, name STRING, age INT) TBLPROPERTIES (delta.enableChangeDataFeed = true)
```

**既存のテーブル**

既存のテーブルに変更データ フィードを適用するには、`delta.enableChangeDataFeed` コマンドでテーブル プロパティ `TRUE`を`ALTER TABLE`に設定する必要があります。

```sql
ALTER TABLE myDeltaTable SET TBLPROPERTIES (delta.enableChangeDataFeed = true)
```

**すべての新しいテーブル**

すべての新しいテーブルに変更データ フィードを適用するには、既定のプロパティを`TRUE`に設定する必要があります。

```sql
set spark.databricks.delta.properties.defaults.enableChangeDataFeed = true;
```

詳しくは、変更データフィードの有効化に関する[[!DNL Azure Databricks]  ガイド ](https://docs.databricks.com/aws/en/delta/delta-change-data-feed#enable-change-data-feed)を参照してください。

[!DNL Azure Databricks] ソース接続の変更データキャプチャを有効にする手順については、次のドキュメントを参照してください。

* [ ベース接続 [!DNL Azure Databricks] を作成](../api/create/databases/databricks.md)。
* [ データベース ](../api/collect/database-nosql.md#create-a-source-connection)のソース接続を作成します。

### [!DNL Data Landing Zone]

[!DNL Data Landing Zone]でchange data captureを使用するには、ソーステーブルで&#x200B;**change data feed**&#x200B;を有効にし、Experience PlatformのリレーショナルスキーマでData Mirrorを設定する必要があります。

[!DNL Data Landing Zone] ソース接続の変更データキャプチャを有効にする手順については、次のドキュメントを参照してください。

* [ ベース接続 [!DNL Data Landing Zone] を作成](../api/create/cloud-storage/data-landing-zone.md)。
* [ クラウドストレージのソース接続を作成](../api/collect/cloud-storage.md#create-a-source-connection)。

### [!DNL Google BigQuery]

[!DNL Google BigQuery]でChange Data Captureを使用するには、ソーステーブルで変更履歴を有効にし、Experience Platformでリレーショナルスキーマを使用してData Mirrorを設定する必要があります。

[!DNL Google BigQuery] ソース接続で変更履歴を有効にするには、[!DNL Google BigQuery] コンソールの[!DNL Google Cloud] ページに移動し、`enable_change_history`を`TRUE`に設定します。 このプロパティは、データテーブルの変更履歴を有効にします。

詳しくは、[ [!DNL GoogleSQL]の](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language#table_option_list) データ定義言語ステートメントに関するガイドを参照してください。

[!DNL Google BigQuery] ソース接続の変更データキャプチャを有効にする手順については、次のドキュメントを参照してください。

* [ ベース接続 [!DNL Google BigQuery] を作成](../api/create/databases/bigquery.md)。
* [ データベース ](../api/collect/database-nosql.md#create-a-source-connection)のソース接続を作成します。

### [!DNL Snowflake]

[!DNL Snowflake]でchange data captureを使用するには、ソーステーブルで&#x200B;**change tracking**&#x200B;を有効にし、Experience Platformでリレーショナルスキーマを使用してData Mirrorを設定する必要があります。

[!DNL Snowflake]で、`ALTER TABLE`を使用して`CHANGE_TRACKING`を`TRUE`に設定し、変更追跡を有効にします。

```sql
ALTER TABLE mytable SET CHANGE_TRACKING = TRUE
```

詳細については、[[!DNL Snowflake] 変更条項の使用に関するガイド ](https://docs.snowflake.com/en/sql-reference/constructs/changes#usage-notes)を参照してください。

[!DNL Snowflake] ソース接続の変更データキャプチャを有効にする手順については、次のドキュメントを参照してください。

* [ ベース接続 [!DNL Snowflake] を作成](../api/create/databases/snowflake.md)。
* [ データベース ](../api/collect/database-nosql.md#create-a-source-connection)のソース接続を作成します。
