---
title: SQL を使用した派生データセットの作成
description: SQL を使用して、プロファイルが有効な派生データセットを作成する方法と、データセットをリアルタイム顧客プロファイルおよびセグメント化サービスに使用する方法について説明します。
exl-id: bb1a1d8d-4662-40b0-857a-36efb8e78746
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '1238'
ht-degree: 2%

---

# SQL を使用した派生データセットの作成

SQL クエリを使用して既存のデータセットのデータを操作および変換し、プロファイルが有効な派生データセットを作成する方法について説明します。 このワークフローは、リアルタイム顧客プロファイルのビジネスユースケース向けに派生データセットを作成する、効率的な代替方法を提供します。

このドキュメントでは、リアルタイム顧客プロファイルで使用する派生データセットを生成する、様々な便利な SQL 拡張機能の概要を説明します。 ワークフローを使用すると、様々な API 呼び出しやExperience Platform UI のインタラクションを通じて実行する必要があるプロセスを簡単に実行できます。

通常、リアルタイム顧客プロファイル用の派生データセットの生成と公開には、次の手順が必要です。

* ID 名前空間が存在しない場合は、作成します。
* 必要に応じて、派生データセットを格納するデータタイプを作成します。
* そのデータタイプを持つフィールドグループを作成して、派生データセット情報を保存します。
* 以前に作成した名前空間を使用して、プライマリ ID 列を作成または割り当てます。
* 前に作成したフィールドグループとデータタイプを使用してスキーマを作成します。
* スキーマを使用して新しいデータセットを作成し、必要に応じてプロファイルに対して有効にします。
* オプションで、データセットをプロファイル対応としてマークします。

前述の手順を完了すると、データセットにデータを入力する準備が整います。 プロファイルのデータセットを有効にした場合は、新しいデータセットを参照するセグメントを作成し、インサイトの生成を開始することもできます。

クエリサービスを使用すると、上記のすべてのアクションを SQL クエリを使用して実行できます。 これには、必要に応じて、データセットやフィールドグループに変更を加えることが含まれます。

## プロファイルに対して有効にするオプションを含むテーブルを作成します {#enable-dataset-for-profile}

>[!NOTE]
>
>以下に示す SQL クエリは、既存の名前空間を使用することを前提としています。

テーブルを選択として作成（CTAS）クエリを使用して、データセットの作成、データタイプの割り当て、プライマリ ID の設定、スキーマの作成、プロファイル対応のマークを行います。 以下の SQL 文の例では、データセットを作成し、Real-Time Customer Data Platform（Real-Time CDP）で使用できるようにします。 SQL クエリは、次の例に示す形式に従います。

```sql
CREATE TABLE <your_table_name> [IF NOT EXISTS] (fieldname <your_data_type> primary identity namespace <your_namespace>, [field_name2 <your_data_type>]) [WITH(LABEL='PROFILE')];
```

サポートされているデータタイプは、ブール値、日付、日時、テキスト、float、bigint、整数、マップ、配列、構造体/行です。

以下の SQL コードブロックは、構造体/行、マップ、および配列データタイプを定義する例を示しています。 1 行目は行の構文を示しています。 2 行目はマップ構文、3 行目は配列構文を示しています。

```sql {line-numbers="true"}
ROW (Column_name <data_type> [, column name <data_type> ]*)
MAP <data_type, data_type>
ARRAY <data_type>
```

または、Experience Platform UI を使用して、データセットをプロファイルに対して有効にすることもできます。 プロファイルに対してデータセットを有効としてマークする方法について詳しくは、[ リアルタイム顧客プロファイルのデータセットを有効にする ](../../../catalog/datasets/user-guide.md#enable-profile) ドキュメントを参照してください。

以下のクエリの例では、`id` をプライマリ ID 列として持つ `decile_table` データセットが作成され、名前空間 `IDFA` が設定されています。 また、マップデータタイプの `decile1Month` という名前のフィールドもあります。 作成されたテーブル （`decile_table`）はプロファイルに対して有効になっています。

```sql
CREATE TABLE decile_table (id text PRIMARY KEY NAMESPACE 'IDFA', 
            decile1Month map<text, integer>) WITH (label='PROFILE');
```

クエリが正常に実行されると、データセット ID がコンソールに返されます（以下の例を参照）。

```console
Created Table DataSet Id
>
637fd84969ba291e62dba79f
(1 row)
```

`CREATE TABLE` コマンドで `label='PROFILE'` を使用して、プロファイルが有効なデータセットを作成します。 `upsert` 機能は、デフォルトでオンになっています。 以下の例に示すように、`ALTER` コマンドを使用して `upsert` 機能を上書きできます。

```sql
ALTER TABLE <your_table_name> DROP label upsert;
```

[ALTER TABLE](../../sql/syntax.md#alter-table) コマンドと [CTAS 問合せの一部としてのラベル ](../../sql/syntax.md#create-table-as-select) の使用について詳しくは、SQl 構文のドキュメントを参照してください。

## SQL を使用した派生データセットの管理に役立つ構成

以下に説明する機能は、SQL を使用して派生データセットを管理する際に大きなメリットがあります。

### プロファイルで有効にする既存のデータセットの変更 {#enable-existing-dataset-for-profile}

ALTER TABLE SQL コンストラクトを使用すると、既存のデータセットをプロファイルに対して有効にすることができます。 これには、プロファイルが有効になっているタグが、スキーマと、対応するデータセットの両方に追加されている必要があります。

```sql
ALTER TABLE your_decile_table ADD label 'PROFILE';
```

>[!NOTE]
>
>`ALTER TABLE` コマンドが正常に実行されると、コンソールは `ALTER SUCCESS` を返します。

### 既存のデータセットへのプライマリ ID の追加 {#add-primary-identity}

データセット内の既存の列をプライマリ ID セットとしてマークします。そうしないと、エラーが発生します。 SQL を使用してプライマリ ID を設定するには、次に表示されるクエリ形式を使用します。

```sql
ALTER TABLE <your_table_name> ADD CONSTRAINT primary identity NAMESPACE
```

例：

```sql
ALTER TABLE test1_dataset ADD CONSTRAINT PRIMARY KEY(id2) NAMESPACE 'IDFA';
```

この例では、`test1_dataset` に既存の列が `id2` ります。

### プロファイルのデータセットを無効にする {#disable-dataset-for-profile}

プロファイルが使用するテーブルを無効にする場合は、DROP コマンドを使用する必要があります。 USES `DROP` を使用する SQL 文の例を以下に示します。

```sql
ALTER TABLE table_name DROP LABEL 'PROFILE';
```

例：

```sql
ALTER TABLE decile_table DROP label 'PROFILE';
```

この SQL ステートメントは、API 呼び出しを使用する代わりの効率的な方法を提供します。 詳しくは、方法 [datasets API を使用してReal-Time CDPで使用するデータセットを無効にする ](../../../catalog/datasets/enable-upsert.md#disable-the-dataset-for-profile) に関するドキュメントを参照してください。

### データセットの更新を許可して機能を挿入する {#enable-upsert-functionality-for-dataset}

UPSERT コマンドを使用すると、新しいレコードを挿入したり、テーブル内の既存のデータを更新したりできます。 特に、指定した値がテーブル内に既に存在する場合は既存の行を更新し、指定した値が存在しない場合は新しい行を挿入できます。

正しい形式を使用するステートメントの例を以下に示します。

```sql
ALTER TABLE table_name ADD LABEL 'UPSERT';
```

例：

```sql
ALTER TABLE table_with_a_decile ADD label 'UPSERT';
```

この SQL ステートメントは、API 呼び出しを使用する代わりの効率的な方法を提供します。 詳しくは、方法 [Real-Time CDPでデータセットを有効にし、データセット API を使用して UPSERT する ](../../../catalog/datasets/enable-upsert.md#enable-the-dataset) に関するドキュメントを参照してください。

### データセットの更新および挿入機能を無効にする {#disable-upsert-functionality-for-dataset}

このコマンドは、データセットに行を更新して挿入する機能を無効にします。

正しい形式を使用するステートメントの例を以下に示します。

```sql
ALTER TABLE table_name DROP LABEL 'UPSERT';
```

例：

```sql
ALTER TABLE table_with_a_decile DROP label 'UPSERT';
```

### 各テーブルに関連付けられている追加テーブル情報を表示します {#show-labels-for-tables}

プロファイルが有効なデータセットには、追加のメタデータが保持されます。 `SHOW TABLES` コマンドを使用して、テーブルに関連付けられたラベルに関する情報を提供する追加の `labels` 列を表示します。

このコマンドの出力例を次に示します。

```sql
       name          |        dataSetId         |     dataSet    | description | labels 
---------------------+--------------------------+----------------+-------------+----------
 luma_midvalues      | 5bac030c29bb8d12fa992e58 | Luma midValues |             | false
 luma_postvalues     | 5c86b896b3c162151785b43c | Luma midValues |             | false
 table_with_a_decile | 5c86b896b3c162151785b43c | Luma midValues |             | 'UPSERT', 'PROFILE'
(3 rows)
```

例から、`table_with_a_decile` がプロファイルに対して有効になっており、前述のように [&#39;UPSERT&#39;](#enable-upsert-functionality-for-dataset)、[&#39;PROFILE&#39;などのラベルが適用されてい ](#enable-existing-dataset-for-profile) ことがわかります。

### SQL でのフィールドグループの作成

フィールドグループは、SQL を使用して作成できるようになりました。 これにより、Experience Platform UI 内でスキーマエディターを使用する代わりに、またはスキーマレジストリに対して API 呼び出しを行うことができます。

フィールドグループを作成するステートメントの例を以下に示します。

```sql
CREATE FIELDGROUP <field_group_name> [IF NOT EXISTS]  (field_name <data_type> primary identity namespace <namespace>, [field_name_2 >data_type>]) [ WITH(LABEL='PROFILE') ];
```

>[!IMPORTANT]
>
>SQL を使用したフィールドグループの作成は、ステートメントで `label` フラグが指定されていない場合、またはフィールドグループが既に存在する場合は失敗します。
>フィールドグループが既に存在するので、クエリが失敗しないように、クエリに `IF NOT EXISTS` 句が含まれていることを確認してください。

実際の例は、以下に示すものに似ている場合があります。

```sql
CREATE FIELDGROUP field_group_for_test123 (decile1Month map<text, integer>, decile3Month map<text, integer>, decile6Month map<text, integer>, decile9Month map<text, integer>, decile12Month map<text, integer>, decilelietime map<text, integer>) WITH (LABEL-'PROFILE');
```

このステートメントが正常に実行されると、作成されたフィールドグループ ID が返されます。 例：`c731a1eafdfdecae1683c6dca197c66ed2c2b49ecd3a9525`。

代替方法について詳しくは、[ スキーマエディターで新しいフィールドグループを作成する ](../../../xdm/ui/resources/field-groups.md#create) または [ スキーマレジストリ API](../../../xdm/api/field-groups.md#create) を使用する）方法に関するドキュメントを参照してください。

### フィールドグループをドロップ

場合によっては、スキーマレジストリからフィールドグループを削除する必要があります。 それには、フィールドグループ ID を指定して `DROP FIELDGROUP` コマンドを実行します。

```sql
DROP FIELDGROUP [IF EXISTS] <your_field_group_id>;
```

例：

```sql
DROP FIELDGROUP field_group_for_test123;
```

>[!IMPORTANT]
>
>SQL によるフィールドグループの削除は、フィールドグループが存在しない場合は失敗します。 クエリが失敗するのを避けるために、ステートメントに `IF EXISTS` 句が含まれていることを確認してください。

### テーブルのすべてのフィールドグループ名と ID を表示する

`SHOW FIELDGROUPS` コマンドは、テーブルの名前、fieldgroupId、所有者を含むテーブルを返します。

このコマンドの出力例を次に示します。

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

このドキュメントでは、SQL を使用してプロファイルを作成し、派生データセットに基づいてアップサート対応データセットを使用する方法について、より深く理解しました。 これで、このデータセットをバッチ取得ワークフローで使用して、プロファイルデータを更新する準備が整いました。 データを Adobe Experience Platform に取り込む方法の詳細については、まず [データ取り込みの概要](../../../ingestion/home.md)を参照してください。
