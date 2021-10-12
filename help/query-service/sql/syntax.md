---
keywords: Experience Platform；ホーム；人気のあるトピック；クエリサービス；クエリサービス；SQL 構文；SQL;CTAS;CTAS；選択としてテーブルを作成
solution: Experience Platform
title: クエリサービスの SQL 構文
topic-legacy: syntax
description: このドキュメントでは、Adobe Experience Platformクエリサービスでサポートされる SQL 構文を示します。
exl-id: 2bd4cc20-e663-4aaa-8862-a51fde1596cc
source-git-commit: 6f697bb249c50e58f9e8a5821fa71f2d4c9a7aac
workflow-type: tm+mt
source-wordcount: '2154'
ht-degree: 13%

---

# クエリサービスの SQL 構文

Adobe Experience Platformクエリサービスは、`SELECT` 文やその他の制限付きコマンドに標準の ANSI SQL を使用する機能を提供します。 このドキュメントでは、[!DNL Query Service] でサポートされる SQL 構文について説明します。

## SELECT クエリ {#select-queries}

次の構文は、[!DNL Query Service] でサポートされる `SELECT` クエリを定義します。

```sql
[ WITH with_query [, ...] ]
SELECT [ ALL | DISTINCT [( expression [, ...] ) ] ]
    [ * | expression [ [ AS ] output_name ] [, ...] ]
    [ FROM from_item [, ...] ]
    [ SNAPSHOT { SINCE start_snapshot_id | AS OF end_snapshot_id | BETWEEN start_snapshot_id AND end_snapshot_id } ]
    [ WHERE condition ]
    [ GROUP BY grouping_element [, ...] ]
    [ HAVING condition [, ...] ]
    [ WINDOW window_name AS ( window_definition ) [, ...] ]
    [ { UNION | INTERSECT | EXCEPT | MINUS } [ ALL | DISTINCT ] select ]
    [ ORDER BY expression [ ASC | DESC | USING operator ] [ NULLS { FIRST | LAST } ] [, ...] ]
    [ LIMIT { count | ALL } ]
    [ OFFSET start ]
```

ここで、`from_item` は、次のいずれかのオプションです。

```sql
table_name [ * ] [ [ AS ] alias [ ( column_alias [, ...] ) ] ]
```

```sql
[ LATERAL ] ( select ) [ AS ] alias [ ( column_alias [, ...] ) ]
```

```sql
with_query_name [ [ AS ] alias [ ( column_alias [, ...] ) ] ]
```

```sql
from_item [ NATURAL ] join_type from_item [ ON join_condition | USING ( join_column [, ...] ) ]
```

と `grouping_element` は、次のいずれかのオプションです。

```sql
( )
```

```sql
expression
```

```sql
( expression [, ...] )
```

```sql
ROLLUP ( { expression | ( expression [, ...] ) } [, ...] )
```

```sql
CUBE ( { expression | ( expression [, ...] ) } [, ...] )
```

```sql
GROUPING SETS ( grouping_element [, ...] )
```

`with_query` は以下になります。

```sql
 with_query_name [ ( column_name [, ...] ) ] AS ( select | values )
```

次のサブセクションでは、上記の形式に従っている場合に、クエリで使用できる追加の句の詳細を示します。

### SNAPSHOT 句

この句は、スナップショット ID に基づいてテーブルのデータを増分的に読み取る場合に使用できます。 スナップショット ID は、データが書き込まれるたびにデータレイクテーブルに適用される Long 型の番号で表されるチェックポイントマーカーです。 `SNAPSHOT` 句は、その隣で使用されるテーブルの関係にそれ自体を付加します。

```sql
    [ SNAPSHOT { SINCE start_snapshot_id | AS OF end_snapshot_id | BETWEEN start_snapshot_id AND end_snapshot_id } ]
```

#### 例

```sql
SELECT * FROM Customers SNAPSHOT SINCE 123;

SELECT * FROM Customers SNAPSHOT AS OF 345;

SELECT * FROM Customers SNAPSHOT BETWEEN 123 AND 345;

SELECT * FROM Customers SNAPSHOT BETWEEN HEAD AND 123;

SELECT * FROM Customers SNAPSHOT BETWEEN 345 AND TAIL;

SELECT * FROM (SELECT id FROM CUSTOMERS BETWEEN 123 AND 345) C 

SELECT * FROM Customers SNAPSHOT SINCE 123 INNER JOIN Inventory AS OF 789 ON Customers.id = Inventory.id;
```

`SNAPSHOT` 句はテーブルまたはテーブルのエイリアスで使用できますが、サブクエリやビューの上にはありません。 `SNAPSHOT` 句は、テーブルに対して `SELECT` クエリを適用できる場所であればどこでも機能します。

また、`HEAD` と `TAIL` は、スナップショット句の特別なオフセット値として使用できます。 `HEAD` を使用すると最初のスナップショットの前のオフセットを参照し、 `TAIL` は最後のスナップショットの後のオフセットを参照します。

>[!NOTE]
>
>2 つのスナップショット ID 間でクエリを実行し、開始スナップショットの有効期限が切れた場合、オプションのフォールバック動作フラグ (`resolve_fallback_snapshot_on_failure`) が設定されているかどうかに応じて、次の 2 つのシナリオが発生する可能性があります。
>
>- オプションのフォールバック動作フラグが設定されている場合、クエリサービスは最も古い使用可能なスナップショットを選択し、開始スナップショットとして設定し、最も古い使用可能なスナップショットと指定した終了スナップショットの間のデータを返します。 このデータは、利用可能な最も古いスナップショットの **** を含みます。
>
>- オプションのフォールバック動作フラグが設定されていない場合は、エラーが返されます。


### WHERE 句

デフォルトでは、`SELECT` クエリの `WHERE` 句で生成された一致では、大文字と小文字が区別されます。 一致で大文字と小文字を区別しない場合は、`LIKE` の代わりにキーワード `ILIKE` を使用できます。

```sql
    [ WHERE condition { LIKE | ILIKE | NOT LIKE | NOT ILIKE } pattern ]
```

LIKE 句と ILIKE 句のロジックを次の表に示します。

| 句 | 演算子 |
| ------ | -------- |
| `WHERE condition LIKE pattern` | `~~` |
| `WHERE condition NOT LIKE pattern` | `!~~` |
| `WHERE condition ILIKE pattern` | `~~*` |
| `WHERE condition NOT ILIKE pattern` | `!~~*` |

**例**

```sql
SELECT * FROM Customers
WHERE CustomerName ILIKE 'a%';
```

このクエリでは、名前が「A」または「a」で始まる顧客を返します。

### 結合

JOIN を使用する `SELECT` クエリの構文は次のとおりです。

```sql
SELECT statement
FROM statement
[JOIN | INNER JOIN | LEFT JOIN | LEFT OUTER JOIN | RIGHT JOIN | RIGHT OUTER JOIN | FULL JOIN | FULL OUTER JOIN]
ON join condition
```

### UNION、INTERSECT、EXCEPT

`UNION`、`INTERSECT` および `EXCEPT` 句は、2 つ以上のテーブルから同様の行を結合または除外するために使用します。

```sql
SELECT statement 1
[UNION | UNION ALL | UNION DISTINCT | INTERSECT | EXCEPT | MINUS]
SELECT statement 2
```

### CREATE TABLE AS SELECT

次の構文は、`CREATE TABLE AS SELECT` (CTAS) クエリを定義します。

```sql
CREATE TABLE table_name [ WITH (schema='target_schema_title', rowvalidation='false') ] AS (select_query)
```

**パラメーター**

- `schema`:XDM スキーマのタイトル。この句は、CTAS クエリで作成された新しいデータセットに対して既存の XDM スキーマを使用する場合にのみ使用します。
- `rowvalidation`:（オプション）新しく作成したデータセットに対して取り込まれるすべての新しいバッチの行レベルの検証を、ユーザーが希望するかどうかを指定します。デフォルト値は `true` です。
- `select_query`:文 `SELECT` 。`SELECT` クエリの構文は、[SELECT クエリの節 ](#select-queries) に記載されています。

**例**

```sql
CREATE TABLE Chairs AS (SELECT color, count(*) AS no_of_chairs FROM Inventory i WHERE i.type=="chair" GROUP BY i.color)

CREATE TABLE Chairs WITH (schema='target schema title') AS (SELECT color, count(*) AS no_of_chairs FROM Inventory i WHERE i.type=="chair" GROUP BY i.color)

CREATE TABLE Chairs AS (SELECT color FROM Inventory SNAPSHOT SINCE 123)
```

>[!NOTE]
>
>`SELECT` 文には、`COUNT`、`SUM`、`MIN` などの集計関数のエイリアスが含まれている必要があります。また、`SELECT` 文は () 括弧付きでも付きも付きも付きもしないこともできます。 `SNAPSHOT` 句を指定して、増分差分をターゲットテーブルに読み込むことができます。

## INSERT INTO

`INSERT INTO` コマンドは次のように定義されます。

```sql
INSERT INTO table_name select_query
```

**パラメーター**

- `table_name`:クエリを挿入するテーブルの名前。
- `select_query`:文 `SELECT` 。`SELECT` クエリの構文は、[SELECT クエリの節 ](#select-queries) に記載されています。

**例**

```sql
INSERT INTO Customers SELECT SupplierName, City, Country FROM OnlineCustomers;

INSERT INTO Customers AS (SELECT * from OnlineCustomers SNAPSHOT AS OF 345)
```

>[!NOTE]
> `SELECT` 文 **は () 括弧で囲んではいけません**。 また、`SELECT` 文の結果のスキーマは、`INSERT INTO` 文で定義されたテーブルの結果に準拠している必要があります。 `SNAPSHOT` 句を指定して、増分差分をターゲットテーブルに読み込むことができます。

## DROP TABLE

`DROP TABLE` コマンドは、既存のテーブルを削除し、外部テーブルでない場合は、テーブルに関連付けられたディレクトリをファイルシステムから削除します。 テーブルが存在しない場合は、例外が発生します。

```sql
DROP TABLE [IF EXISTS] [db_name.]table_name
```

**パラメーター**

- `IF EXISTS`:これを指定した場合、テーブルに notexist が含まれている場合は例外はスローさ **** れません。

## データベースの削除

`DROP DATABASE` コマンドは、既存のデータベースを削除します。

```sql
DROP DATABASE [IF EXISTS] db_name
```

**パラメーター**

- `IF EXISTS`:これを指定した場合、データベースが notexist の場合は例外は発生し **** ません。

## スキーマを削除

`DROP SCHEMA` コマンドは既存のスキーマを削除します。

```sql
DROP SCHEMA [IF EXISTS] db_name.schema_name [ RESTRICT | CASCADE]
```

**パラメーター**

- `IF EXISTS`:これを指定した場合、スキーマが notexist の場合は例外は発生し **** ません。

- `RESTRICT`:モードのデフォルト値。これを指定した場合、スキーマは **テーブルを含まない** 場合にのみ削除されます。

- `CASCADE`:これを指定すると、スキーマは、スキーマ内に存在するすべてのテーブルと共に削除されます。

## CREATE VIEW

次の構文は `CREATE VIEW` クエリを定義します。

```sql
CREATE VIEW view_name AS select_query
```

**パラメーター**

- `view_name`:作成するビューの名前。
- `select_query`:文 `SELECT` 。`SELECT` クエリの構文は、[SELECT クエリの節 ](#select-queries) に記載されています。

**例**

```sql
CREATE VIEW V1 AS SELECT color, type FROM Inventory

CREATE OR REPLACE VIEW V1 AS SELECT model, version FROM Inventory
```

## DROP VIEW

次の構文は `DROP VIEW` クエリを定義します。

```sql
DROP VIEW [IF EXISTS] view_name
```

**パラメーター**

- `IF EXISTS`:これを指定した場合、ビューが notexist でない場合は例外は発生し **** ません。
- `view_name`:削除するビューの名前。

**例**

```sql
DROP VIEW v1
DROP VIEW IF EXISTS v1
```

## [!DNL Spark] SQL コマンド

次のサブ節では、クエリサービスでサポートされる Spark SQL コマンドを示します。

### SET

`SET` コマンドは、プロパティを設定し、既存のプロパティの値を返すか、既存のすべてのプロパティをリストします。 既存プロパティのキーに値が指定された場合、古い値が上書きされます。

```sql
SET property_key = property_value
```

**パラメーター**

- `property_key`:リストまたは変更するプロパティの名前。
- `property_value`:プロパティを設定する値です。

設定の値を返すには、`property_value` を付けずに `SET [property key]` を使用します。

## PostgreSQL コマンド

以下のサブ節では、クエリサービスでサポートされる PostgreSQL コマンドについて説明します。

### BEGIN

`BEGIN` コマンド、または `BEGIN WORK` または `BEGIN TRANSACTION` コマンドを使用して、トランザクションブロックを開始します。 begin コマンドの後に入力された文は、明示的な COMMIT または ROLLBACK コマンドが指定されるまで、単一のトランザクションで実行されます。 このコマンドは `START TRANSACTION` と同じです。

```sql
BEGIN
BEGIN WORK
BEGIN TRANSACTION
```

### CLOSE

`CLOSE` コマンドは、開いたカーソルに関連付けられたリソースを解放します。 カーソルを閉じた後の操作は許可されません。不要になったカーソルは閉じる必要があります。

```sql
CLOSE name
CLOSE ALL
```

`CLOSE name` を使用した場合、`name` は閉じる必要がある開いたカーソルの名前を表します。 `CLOSE ALL` を使用すると、すべてのオープンカーソルが閉じられます。

### DEALLOCATE

`DEALLOCATE` コマンドを使用すると、事前に準備された SQL 文の割り当てを解除できます。 準備された文の割り当てを明示的に解除しない場合は、セッションが終了すると割り当てが解除されます。準備済み文の詳細については、[PREPARE コマンド ](#prepare) の節を参照してください。

```sql
DEALLOCATE name
DEALLOCATE ALL
```

`DEALLOCATE name` を使用した場合、`name` は割り当てを解除する必要がある準備済み文の名前を表します。 `DEALLOCATE ALL` を使用すると、すべての準備済み文の割り当てが解除されます。

### DECLARE

`DECLARE` コマンドを使用すると、カーソルを作成できます。これは、大きなクエリから少数の行を取得するのに使用できます。 カーソルが作成された後、`FETCH` を使用して行がカーソルから取得されます。

```sql
DECLARE name CURSOR FOR query
```

**パラメーター**

- `name`：作成するカーソルの名前。
- `query`：カーソルが返す行を指定する `SELECT` または `VALUES` コマンド。

### EXECUTE

`EXECUTE` コマンドは、事前に準備された文を実行するために使用します。 準備済み文はセッション中にのみ存在するので、現在のセッションで前に実行された `PREPARE` 文によって作成される必要があります。 準備済み文の使用に関する詳細は、[`PREPARE` コマンド ](#prepare) の節を参照してください。

文を作成した `PREPARE` 文が一部のパラメータを指定した場合は、互換性のある一連のパラメータを `EXECUTE` 文に渡す必要があります。 これらのパラメーターが渡されない場合、エラーが発生します。

```sql
EXECUTE name [ ( parameter ) ]
```

**パラメーター**

- `name`：実行する準備済み文の名前。
- `parameter`：準備済み文のパラメーターの実際の値。これは、準備済み文の作成時に決定された、このパラメーターのデータ型と互換性のある値を生成する式である必要があります。準備済み文に複数のパラメーターがある場合は、コンマで区切ります。

### EXPLAIN

`EXPLAIN` コマンドは、指定された文の実行計画を表示します。 実行計画では、文で参照されるテーブルがスキャンされる方法が示されます。  複数のテーブルが参照されている場合は、各入力テーブルから必要な行を組み合わせるために使用される結合アルゴリズムが表示されます。

```sql
EXPLAIN option statement
```

ここで、`option` は次のいずれかになります。

```sql
ANALYZE
FORMAT { TEXT | JSON }
```

**パラメーター**

- `ANALYZE`:にが含ま `option` れる場 `ANALYZE`合は、実行時間と他の統計が表示されます。
- `FORMAT`:にが含ま `option` れ `FORMAT`る場合は、出力形式を指定します（または）。 `TEXT`   `JSON`テキスト以外の出力には、テキスト出力形式と同じ情報が含まれますが、プログラムの解析が容易です。このパラメーターのデフォルトは `TEXT` です。
- `statement`：実行計画を表示する `SELECT`、`INSERT`、`UPDATE`、`DELETE`、`VALUES`、`EXECUTE`、`DECLARE`、`CREATE TABLE AS`、`CREATE MATERIALIZED VIEW AS` 文のいずれか。

>[!IMPORTANT]
>
> `ANALYZE` オプションを使用すると、実際に文が実行されることに注意してください。`EXPLAIN` は、`SELECT` が返す出力を破棄しますが、文の他の副作用は通常どおり発生します。

**例**

次の例は、単一の `integer` 列と10000行のテーブルに対する単純なクエリの計画を示しています。

```sql
EXPLAIN SELECT * FROM foo;
```

```console
                       QUERY PLAN
---------------------------------------------------------
 Seq Scan on foo  (cost=0.00..155.00 rows=10000 width=4)
(1 row)
```

### FETCH

`FETCH` コマンドは、前に作成したカーソルを使用して行を取得します。

```sql
FETCH num_of_rows [ IN | FROM ] cursor_name
```

**パラメーター**

- `num_of_rows`:取得する行の数。
- `cursor_name`:情報を取得するカーソルの名前。

### PREPARE {#prepare}

`PREPARE` コマンドを使用すると、準備済み文を作成できます。 準備済み文は、類似の SQL 文をテンプレート化するために使用できるサーバー側のオブジェクトです。

準備済み文は、パラメーターを受け取ることができます。パラメーターとは、文の実行時に文内で置き換えられる値です。 パラメーターは、準備済み文を使用する場合、$1、$2 などを使用して、位置で参照されます。

必要に応じて、パラメータデータ型のリストを指定できます。 パラメーターのデータ型が一覧にない場合は、その型をコンテキストから推論できます。

```sql
PREPARE name [ ( data_type [, ...] ) ] AS SELECT
```

**パラメーター**

- `name`:準備済み文の名前。
- `data_type`:準備済み文のパラメーターのデータ型。パラメーターのデータ型が一覧にない場合は、その型をコンテキストから推論できます。 複数のデータ型を追加する必要がある場合は、コンマ区切りのリストで追加できます。

### ROLLBACK

`ROLLBACK` コマンドは、現在のトランザクションを取り消し、トランザクションによって行われた更新をすべて破棄します。

```sql
ROLLBACK
ROLLBACK WORK
```

### SELECT INTO

`SELECT INTO` コマンドは、新しいテーブルを作成し、クエリで計算されたデータで埋めます。 通常の `SELECT` コマンドと同様に、データはクライアントに返されません。 新しいテーブルの列には、`SELECT` コマンドの出力列に関連付けられた名前とデータ型が含まれます。

```sql
[ WITH [ RECURSIVE ] with_query [, ...] ]
SELECT [ ALL | DISTINCT [ ON ( expression [, ...] ) ] ]
    * | expression [ [ AS ] output_name ] [, ...]
    INTO [ TEMPORARY | TEMP | UNLOGGED ] [ TABLE ] new_table
    [ FROM from_item [, ...] ]
    [ WHERE condition ]
    [ GROUP BY expression [, ...] ]
    [ HAVING condition [, ...] ]
    [ WINDOW window_name AS ( window_definition ) [, ...] ]
    [ { UNION | INTERSECT | EXCEPT } [ ALL | DISTINCT ] select ]
    [ ORDER BY expression [ ASC | DESC | USING operator ] [ NULLS { FIRST | LAST } ] [, ...] ]
    [ LIMIT { count | ALL } ]
    [ OFFSET start [ ROW | ROWS ] ]
    [ FETCH { FIRST | NEXT } [ count ] { ROW | ROWS } ONLY ]
    [ FOR { UPDATE | SHARE } [ OF table_name [, ...] ] [ NOWAIT ] [...] ]
```

**パラメーター**

標準の SELECT クエリパラメータの詳細については、[SELECT クエリの節 ](#select-queries) を参照してください。 このセクションでは、`SELECT INTO` コマンドに対して排他的なパラメータのみをリストします。

- `TEMPORARY` または `TEMP`:オプションのパラメーター。指定した場合、作成されるテーブルは一時テーブルになります。
- `UNLOGGED`:オプションのパラメーター。指定した場合、として作成されたテーブルはログなしのテーブルになります。 ログなしのテーブルについて詳しくは、[PostgreSQL のドキュメント ](https://www.postgresql.org/docs/current/sql-createtable.html) を参照してください。
- `new_table`:作成するテーブルの名前。

**例**

次のクエリは、新しいテーブル `films_recent` を作成します。このテーブルの `films` の最近のエントリのみから構成されます。

```sql
SELECT * INTO films_recent FROM films WHERE date_prod >= '2002-01-01';
```

### SHOW

`SHOW` コマンドは、実行時パラメータの現在の設定を表示します。 これらの変数は、`SET` 文を使用して、`postgresql.conf` 設定ファイルを編集し、`PGOPTIONS` 環境変数（libpq または libpq ベースのアプリケーションを使用する場合）を使用して設定できます。また、Postgres サーバーを起動する際にはコマンドラインフラグを使用します。

```sql
SHOW name
SHOW ALL
```

**パラメーター**

- `name`:情報が必要なランタイムパラメーターの名前。ランタイムパラメーターに指定できる値は次のとおりです。
   - `SERVER_VERSION`:このパラメーターは、サーバーのバージョン番号を示します。
   - `SERVER_ENCODING`:このパラメーターは、サーバー側の文字セットエンコーディングを示します。
   - `LC_COLLATE`:このパラメータは、照合（テキストの順序付け）のためのデータベースのロケール設定を示します。
   - `LC_CTYPE`:このパラメータは、文字分類に関するデータベースのロケール設定を示します。
      `IS_SUPERUSER`:このパラメータは、現在の役割にスーパーユーザー権限があるかどうかを示します。
- `ALL`：すべての設定パラメーターの値と説明を表示します。

**例**

次のクエリは、パラメータ `DateStyle` の現在の設定を示しています。

```sql
SHOW DateStyle;
```

```console
 DateStyle
-----------
 ISO, MDY
(1 row)
```

### コピー

`COPY` コマンドは、任意の `SELECT` クエリの出力を指定した場所にダンプします。 このコマンドを正常に実行するには、ユーザーがこの場所にアクセスできる必要があります。

```sql
COPY query
    TO '%scratch_space%/folder_location'
    [  WITH FORMAT 'format_name']
```

**パラメーター**

- `query`:コピーするクエリ。
- `format_name`:クエリをコピーする形式。`format_name` は、`parquet`、`csv`、`json` のいずれかです。 デフォルト値は `parquet` です。

>[!NOTE]
>
>完全な出力パスは `adl://<ADLS_URI>/users/<USER_ID>/acp_foundation_queryService/folder_location/<QUERY_ID>` です。

### ALTER TABLE

`ALTER TABLE` コマンドを使用すると、プライマリキーや外部キーの制約を追加または削除したり、テーブルに列を追加したりできます。

#### 制約を追加または削除

次の SQL クエリは、表に制約を追加または削除する例を示しています。

```sql
ALTER TABLE table_name ADD CONSTRAINT constraint_name PRIMARY KEY ( column_name )

ALTER TABLE table_name ADD CONSTRAINT constraint_name FOREIGN KEY ( column_name ) REFERENCES referenced_table_name ( primary_column_name )

ALTER TABLE table_name ADD CONSTRAINT constraint_name PRIMARY KEY column_name NAMESPACE namespace

ALTER TABLE table_name DROP CONSTRAINT constraint_name PRIMARY KEY ( column_name )

ALTER TABLE table_name DROP CONSTRAINT constraint_name FOREIGN KEY ( column_name )
```

**パラメーター**

- `table_name`:編集するテーブルの名前。
- `constraint_name`:追加または削除する制約の名前。
- `column_name`:制約を追加する列の名前。
- `referenced_table_name`:外部キーが参照するテーブルの名前。
- `primary_column_name`:外部キーが参照する列の名前。

>[!NOTE]
>
>テーブルスキーマは一意であり、複数のテーブル間で共有されない必要があります。 また、プライマリキーの制約には名前空間が必須です。

#### 列を追加

次の SQL クエリは、テーブルに列を追加する例を示しています。

```sql
ALTER TABLE table_name ADD COLUMN column_name data_type

ALTER TABLE table_name ADD COLUMN column_name_1 data_type1, column_name_2 data_type2 
```

**パラメーター**

- `table_name`:編集するテーブルの名前。
- `column_name`:追加する列の名前。
- `data_type`:追加する列のデータ型。次のようなデータタイプがサポートされています。bigint, char，文字列，日付，日時，倍精度，整数， smallint, tinyint, varchar

### プライマリキーを表示

`SHOW PRIMARY KEYS` コマンドは、指定したデータベースの主キー制約をすべてリストします。

```sql
SHOW PRIMARY KEYS
```

```console
    tableName | columnName    | datatype | namespace
------------------+----------------------+----------+-----------
 table_name_1 | column_name1  | text     | "ECID"
 table_name_2 | column_name2  | text     | "AAID"
```

### 外部キーの表示

`SHOW FOREIGN KEYS` コマンドは、指定したデータベースの外部キー制約をすべてリストします。

```sql
SHOW FOREIGN KEYS
```

```console
    tableName   |     columnName      | datatype | referencedTableName | referencedColumnName | namespace 
------------------+---------------------+----------+---------------------+----------------------+-----------
 table_name_1   | column_name1        | text     | table_name_3        | column_name3         |  "ECID"
 table_name_2   | column_name2        | text     | table_name_4        | column_name4         |  "AAID"
```
