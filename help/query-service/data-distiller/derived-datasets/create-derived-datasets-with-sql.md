---
title: SQL での派生データセットの作成
description: SQL を使用して、プロファイルに対して有効な派生データセットを作成する方法、およびリアルタイム顧客プロファイルとセグメント化サービスに対してデータセットを使用する方法について説明します。
exl-id: bb1a1d8d-4662-40b0-857a-36efb8e78746
source-git-commit: 5bf54374773fd95ae1c40dd00b5dbe633031b70e
workflow-type: tm+mt
source-wordcount: '1235'
ht-degree: 2%

---

# SQL を使用した派生データセットの作成

SQL クエリを使用して、既存のデータセットのデータを操作および変換し、プロファイルに対して有効な派生データセットを作成する方法を説明します。 このワークフローは、リアルタイム顧客プロファイルのビジネス事例の派生データセットを作成する、効率的で代替の方法を提供します。

このドキュメントでは、リアルタイム顧客プロファイルで使用する派生データセットを生成する、様々な便利な SQL 拡張機能の概要を説明します。 このワークフローにより、様々な API 呼び出しや Platform UI の操作を通じて、完了する必要があるプロセスが簡略化されます。

通常、リアルタイム顧客プロファイル用の派生データセットを生成して公開するには、次の手順に従います。

* ID 名前空間が存在しない場合は作成します。
* 必要に応じて、派生データセットを保存するデータ型を作成します。
* そのデータ型を持つフィールドグループを作成して、派生データセット情報を保存します。
* 先ほど作成した名前空間で、プライマリ ID 列を作成または割り当てます。
* 先ほど作成したフィールドグループとデータ型を使用してスキーマを作成します。
* スキーマを使用して新しいデータセットを作成し、必要に応じてプロファイル用に有効にします。
* オプションで、データセットをプロファイルが有効になっているとマークします。

上記の手順を完了したら、データセットを設定する準備が整いました。 プロファイルのデータセットを有効にした場合は、新しいデータセットを参照するセグメントを作成し、インサイトの生成を開始することもできます。

クエリサービスを使用すると、SQL クエリを使用して上記のすべてのアクションを実行できます。 これには、必要に応じてデータセットやフィールドグループに変更を加えることも含まれます。

## プロファイルに対して有効にするオプションを含むテーブルを作成 {#enable-dataset-for-profile}

>[!NOTE]
>
>以下に示す SQL クエリは、既存の名前空間を使用することを前提としています。

Create Table as Select(CTAS) クエリを使用して、データセットの作成、データ型の割り当て、プライマリ ID の設定、スキーマの作成、プロファイル対応としてのマークをおこないます。 次の SQL 文の例では、データセットを作成し、Real-time Customer Data Platform(Real-Time CDP) で使用できるようにします。 SQL クエリは、次の例に示す形式に従います。

```sql
CREATE TABLE <your_table_name> [IF NOT EXISTS] (fieldname <your_data_type> primary identity namespace <your_namespace>, [field_name2 <your_data_type>]) [WITH(LABEL='PROFILE')];
```

サポートされるデータタイプは、boolean、date、datetime、text、float、bigint、integer、map、array、struct/row です。

以下の SQl コードブロックは、構造体/行、マップ、配列のデータ型を定義する例です。 行 1 は、行の構文を示しています。 2 行目は、マップの構文と 3 行目の配列の構文を示しています。

```sql {line-numbers="true"}
ROW (Column_name <data_type> [, column name <data_type> ]*)
MAP <data_type, data_type>
ARRAY <data_type>
```

または、Platform UI を使用して、データセットをプロファイルに対して有効にすることもできます。 データセットをプロファイルで有効としてマークする方法について詳しくは、 [リアルタイム顧客プロファイルドキュメントのデータセットの有効化](../../../catalog/datasets/user-guide.md#enable-profile).

次のクエリの例では、 `decile_table` データセットは `id` をプライマリ id 列として使用し、名前空間を持つ `IDFA`. また、 `decile1Month` マップデータ型の。 作成されたテーブル (`decile_table`) がプロファイルで有効になっていることを確認します。

```sql
CREATE TABLE decile_table (id text PRIMARY KEY NAMESPACE 'IDFA', 
            decile1Month map<text, integer>) WITH (label='PROFILE');
```

クエリが正常に実行されると、次の例に示すように、データセット ID がコンソールに返されます。

```console
Created Table DataSet Id
>
637fd84969ba291e62dba79f
(1 row)
```

用途 `label='PROFILE'` の `CREATE TABLE` コマンドを使用して、プロファイル対応のデータセットを作成します。 The `upsert` デフォルトでは、機能はオンになっています。 The `upsert` 機能は、 `ALTER` コマンドを使用します。以下の例で示すように。

```sql
ALTER TABLE <your_table_name> DROP label upsert;
```

SQl 構文のドキュメントで、 [ALTER TABLE](../../sql/syntax.md#alter-table) コマンドと [CTAS クエリの一部としてのラベル](../../sql/syntax.md#create-table-as-select).

## SQL を使用した派生データセットの管理に役立つ構成

以下に説明する機能は、SQL を使用して派生データセットを管理する場合に大きなメリットを提供します。

### 既存のデータセットをプロファイルに対して有効にする変更 {#enable-existing-dataset-for-profile}

ALTER TABLE SQL 構文は、既存のデータセットをプロファイルに対して有効にするために使用できます。 そのためには、プロファイル対応のタグをスキーマと対応するデータセットの両方に追加する必要があります。

```sql
ALTER TABLE your_decile_table ADD label 'PROFILE';
```

>[!NOTE]
>
>の正常な実行時 `ALTER TABLE` コマンドを実行すると、コンソールは `ALTER SUCCESS`.

### 既存のデータセットへのプライマリ ID の追加 {#add-primary-identity}

データセット内の既存の列をプライマリ ID セットとしてマークします。マークしないと、エラーが発生します。 SQL を使用してプライマリ ID を設定するには、次に示すクエリフォーマットを使用します。

```sql
ALTER TABLE <your_table_name> ADD CONSTRAINT primary identity NAMESPACE
```

以下に例を示します。

```sql
ALTER TABLE test1_dataset ADD CONSTRAINT PRIMARY KEY(id2) NAMESPACE 'IDFA';
```

提供された例では、 `id2` は、 `test1_dataset`.

### プロファイルのデータセットの無効化 {#disable-dataset-for-profile}

プロファイルの使用に対してテーブルを無効にする場合は、DROP[ ドロップ ] コマンドを使用する必要があります。 USES を示す SQL 文の例 `DROP` を以下に示します。

```sql
ALTER TABLE table_name DROP LABEL 'PROFILE';
```

以下に例を示します。

```sql
ALTER TABLE decile_table DROP label 'PROFILE';
```

この SQL 文は、API 呼び出しを使用する効率的な代替方法を提供します。 詳しくは、 [データセット API を使用してReal-Time CDPで使用するデータセットを無効にする](../../../catalog/datasets/enable-upsert.md#disable-the-dataset-for-profile).

### データセットの更新および挿入機能を許可する {#enable-upsert-functionality-for-dataset}

UPSERT[ 挿入 ] コマンドを使用すると、新しいレコードを挿入したり、テーブル内の既存のデータを更新したりできます。 特に、指定した値がテーブルに既に存在する場合は既存の行を更新でき、指定した値が存在しない場合は新しい行を挿入できます。

正しい形式を使用する文の例を以下に示します。

```sql
ALTER TABLE table_name ADD LABEL 'UPSERT';
```

以下に例を示します。

```sql
ALTER TABLE table_with_a_decile ADD label 'UPSERT';
```

この SQL 文は、API 呼び出しを使用する効率的な代替方法を提供します。 詳しくは、 [データセット API を使用して、Real-Time CDPで使用するデータセットを有効にし、UPSERT を実行します。](../../../catalog/datasets/enable-upsert.md#enable-the-dataset).

### データセットの更新および挿入機能を無効にする {#disable-upsert-functionality-for-dataset}

このコマンドを実行すると、データセットに行を更新して挿入する機能が無効になります。

正しい形式を使用する文の例を以下に示します。

```sql
ALTER TABLE table_name DROP LABEL 'UPSERT';
```

以下に例を示します。

```sql
ALTER TABLE table_with_a_decile DROP label 'UPSERT';
```

### 各テーブルに関連する追加のテーブル情報を表示 {#show-labels-for-tables}

プロファイルが有効なデータセットに対しては、追加のメタデータが保持されます。 以下を使用します。 `SHOW TABLES` 追加のを表示するコマンド `labels` テーブルに関連付けられたラベルに関する情報を提供する列。

このコマンドの出力の例を次に示します。

```sql
       name          |        dataSetId         |     dataSet    | description | labels 
---------------------+--------------------------+----------------+-------------+----------
 luma_midvalues      | 5bac030c29bb8d12fa992e58 | Luma midValues |             | false
 luma_postvalues     | 5c86b896b3c162151785b43c | Luma midValues |             | false
 table_with_a_decile | 5c86b896b3c162151785b43c | Luma midValues |             | 'UPSERT', 'PROFILE'
(3 rows)
```

この例から分かるように `table_with_a_decile` はプロファイルに対して有効になり、次のようなラベルで適用されます。 [&#39;アップサート&#39;](#enable-upsert-functionality-for-dataset), [&#39;プロファイル&#39;](#enable-existing-dataset-for-profile) 前述のように。

### SQL を使用したフィールドグループの作成

SQL を使用してフィールドグループを作成できるようになりました。 これにより、Platform UI 内でスキーマエディターを使用するか、スキーマレジストリに対して API 呼び出しを実行するかの代替手段を提供します。

フィールドグループを作成するステートメントの例を以下に示します。

```sql
CREATE FIELDGROUP <field_group_name> [IF NOT EXISTS]  (field_name <data_type> primary identity namespace <namespace>, [field_name_2 >data_type>]) [ WITH(LABEL='PROFILE') ];
```

>[!IMPORTANT]
>
>SQL を使用したフィールドグループの作成が失敗するのは、 `label` フラグが文で指定されていないか、フィールドグループが既に存在する場合は。
>クエリに `IF NOT EXISTS` 句を使用して、フィールドグループが既に存在しているのでクエリが失敗するのを回避します。

実際の例は、次に示すようになります。

```sql
CREATE FIELDGROUP field_group_for_test123 (decile1Month map<text, integer>, decile3Month map<text, integer>, decile6Month map<text, integer>, decile9Month map<text, integer>, decile12Month map<text, integer>, decilelietime map<text, integer>) WITH (LABEL-'PROFILE');
```

この文が正常に実行されると、作成されたフィールドグループ ID が返されます。 例：`c731a1eafdfdecae1683c6dca197c66ed2c2b49ecd3a9525`。

方法に関するドキュメントを参照してください。 [スキーマエディターで新しいフィールドグループを作成します。](../../../xdm/ui/resources/field-groups.md#create) または [スキーマレジストリ API](../../../xdm/api/field-groups.md#create) を参照してください。

### フィールドグループをドロップ

スキーマレジストリからフィールドグループを削除する必要が生じる場合があります。 これは、 `DROP FIELDGROUP` 」コマンドを使用して、フィールドグループ ID を指定します。

```sql
DROP FIELDGROUP [IF EXISTS] <your_field_group_id>;
```

以下に例を示します。

```sql
DROP FIELDGROUP field_group_for_test123;
```

>[!IMPORTANT]
>
>フィールドグループが存在しない場合、SQL を使用したフィールドグループの削除は失敗します。 文に `IF EXISTS` 句を使用してクエリが失敗するのを回避します。

### テーブルのすべてのフィールドグループ名と ID を表示

The `SHOW FIELDGROUPS` コマンドは、テーブルの名前、fieldgroupId、および owner を含むテーブルを返します。

このコマンドの出力の例を次に示します。

```sql
       name                      |        fieldgroupId                             |     owner      |
---------------------------------+-------------------------------------------------+-----------------
 AEP Mobile Lifecycle Details    | _experience.aep-mobile-lifecycle-details        | Luma midValues |
 AEP Web SDK ExperienceEvent     | _experience.aep-web-sdk-experienceevent         | Luma midValues |
 AJO Classification Fields       | _experience.journeyOrchestration.classification | Luma midValues |
 AJO Entity Fields               | _experience.customerJourneyManagement.entities  | Luma midValues |
(4 rows)
```

## 次の手順

このドキュメントを読むと、SQL を使用してプロファイルを作成し、派生データセットに基づいて有効なデータセットをアップサートする方法について、より深く理解できます。 これで、このデータセットをバッチ取得ワークフローで使用して、プロファイルデータを更新する準備が整いました。 データを Adobe Experience Platform に取り込む方法の詳細については、まず [データ取り込みの概要](../../../ingestion/home.md)を参照してください。
