---
keywords: Experience Platform；ホーム；人気のあるトピック；クエリサービス；クエリサービス；SQL構文；SQL;CTAS;CTAS；選択としてテーブルを作成
solution: Experience Platform
title: クエリサービスのSQL構文
topic-legacy: syntax
description: このドキュメントは、Adobe Experience PlatformクエリサービスでサポートされるSQL構文を示しています。
exl-id: 2bd4cc20-e663-4aaa-8862-a51fde1596cc
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1981'
ht-degree: 14%

---

# クエリサービスのSQL構文

Adobe Experience Platformクエリサービスは、`SELECT`文およびその他の制限付きコマンドに標準ANSI SQLを使用する機能を提供します。 このドキュメントは[!DNL Query Service]でサポートされるSQL構文をカバーしています。

## クエリを選択{#select-queries}

次の構文は、[!DNL Query Service]がサポートする`SELECT`クエリを定義します。

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

`from_item`は、次のオプションのいずれかです。

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

と`grouping_element`は、次のオプションのいずれかです。

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

以下のサブセクションでは、クエリで使用できる追加条項の詳細を示します。ただし、上述の形式に従う場合を除きます。

### SNAPSHOT句

この句は、スナップショットIDに基づいてテーブルのデータを増分的に読み取るために使用できます。 スナップショットIDは、Long型の数値で表されるチェックポイントマーカーで、データが書き込まれるたびにデータレークテーブルに適用されます。 `SNAPSHOT`句は、その次に使用するテーブルの関係に付加されます。

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

`SNAPSHOT`句はテーブルまたはテーブルの別名で使用できますが、サブクエリまたは表示の上にはありません。 `SNAPSHOT`句は、テーブルの`SELECT`クエリが適用できる任意の場所で機能します。

また、`HEAD`と`TAIL`を、スナップショット句の特別なオフセット値として使用できます。 `HEAD`を使用すると、最初のスナップショットの前のオフセットを参照し、`TAIL`を使用すると、最後のスナップショットの後のオフセットを参照します。

### WHERE句

デフォルトでは、`SELECT`クエリの`WHERE`句で生成される一致では大文字と小文字が区別されます。 一致の大文字と小文字を区別しない場合は、`LIKE`の代わりにキーワード`ILIKE`を使用できます。

```sql
    [ WHERE condition { LIKE | ILIKE | NOT LIKE | NOT ILIKE } pattern ]
```

LIKE句とILIKE句のロジックを次の表に示します。

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

このクエリは、名前が「A」または「a」で始まる顧客を返します。

### 参加

結合を使用する`SELECT`クエリは、次の構文を持ちます。

```sql
SELECT statement
FROM statement
[JOIN | INNER JOIN | LEFT JOIN | LEFT OUTER JOIN | RIGHT JOIN | RIGHT OUTER JOIN | FULL JOIN | FULL OUTER JOIN]
ON join condition
```

### UNION、INTERSECT、EXCEPT

`UNION`、`INTERSECT`および`EXCEPT`句は、2つ以上のテーブルの行と同様の行を結合または除外するために使用します。

```sql
SELECT statement 1
[UNION | UNION ALL | UNION DISTINCT | INTERSECT | EXCEPT | MINUS]
SELECT statement 2
```

### CREATE TABLE AS SELECT

次の構文は、`CREATE TABLE AS SELECT` (CTAS)クエリを定義します。

```sql
CREATE TABLE table_name [ WITH (schema='target_schema_title', rowvalidation='false') ] AS (select_query)
```

**パラメーター**

- `schema`:XDMスキーマのタイトル。この句は、CTASクエリで作成された新しいデータセットに対して既存のXDMスキーマを使用する場合にのみ使用します。
- `rowvalidation`:（オプション）新しく作成したデータセットに対して取り込まれた新しいバッチすべてについて、行レベルの検証を必要とするかどうかを指定します。デフォルト値は `true` です。
- `select_query`:ス `SELECT` テートメント。`SELECT`クエリの構文は、[SELECTクエリセクション](#select-queries)にあります。

**例**

```sql
CREATE TABLE Chairs AS (SELECT color, count(*) AS no_of_chairs FROM Inventory i WHERE i.type=="chair" GROUP BY i.color)

CREATE TABLE Chairs WITH (schema='target schema title') AS (SELECT color, count(*) AS no_of_chairs FROM Inventory i WHERE i.type=="chair" GROUP BY i.color)

CREATE TABLE Chairs AS (SELECT color FROM Inventory SNAPSHOT SINCE 123)
```

>[!NOTE]
>
>`SELECT` 文には、`COUNT`、`SUM`、`MIN` などの集計関数のエイリアスが含まれている必要があります。また、`SELECT`ステートメントには、括弧()を使用することも、使用しないこともできます。 ターゲットテーブルに増分差分を読み込むための`SNAPSHOT`句を指定できます。

## INSERT INTO

`INSERT INTO`コマンドは次のように定義されます。

```sql
INSERT INTO table_name select_query
```

**パラメーター**

- `table_name`:クエリを挿入するテーブルの名前。
- `select_query`:ス `SELECT` テートメント。`SELECT`クエリの構文は、[SELECTクエリセクション](#select-queries)にあります。

**例**

```sql
INSERT INTO Customers SELECT SupplierName, City, Country FROM OnlineCustomers;

INSERT INTO Customers AS (SELECT * from OnlineCustomers SNAPSHOT AS OF 345)
```

>[!NOTE]
> `SELECT`ステートメント&#x200B;**は、**&#x200B;を括弧()で囲まないでください。 また、`SELECT`ステートメントの結果のスキーマは、`INSERT INTO`ステートメントで定義されたテーブルの結果に従う必要があります。 ターゲットテーブルに増分差分を読み込むための`SNAPSHOT`句を指定できます。

## DROP TABLE

`DROP TABLE`コマンドは、既存のテーブルを削除し、外部テーブルでない場合は、そのテーブルに関連付けられたディレクトリをファイルシステムから削除します。 テーブルが存在しない場合は、例外が発生します。

```sql
DROP TABLE [IF EXISTS] [db_name.]table_name
```

**パラメーター**

- `IF EXISTS`:この値を指定した場合、テーブルにテキストが表示されない場合でも例外は発生し **** ません。

## CREATE VIEW

次の構文は`CREATE VIEW`クエリを定義します。

```sql
CREATE VIEW view_name AS select_query
```

**パラメーター**

- `view_name`:作成する表示の名前。
- `select_query`:ス `SELECT` テートメント。`SELECT`クエリの構文は、[SELECTクエリセクション](#select-queries)にあります。

**例**

```sql
CREATE VIEW V1 AS SELECT color, type FROM Inventory

CREATE OR REPLACE VIEW V1 AS SELECT model, version FROM Inventory
```

## DROP VIEW

次の構文は`DROP VIEW`クエリを定義します。

```sql
DROP VIEW [IF EXISTS] view_name
```

**パラメーター**

- `IF EXISTS`:これを指定した場合、表示が **** notexistを実行しない場合は例外がスローされません。
- `view_name`:削除する表示の名前。

**例**

```sql
DROP VIEW v1
DROP VIEW IF EXISTS v1
```

## [!DNL Spark] SQLコマンド

次のサブセクションは、クエリサービスでサポートされるSpark SQLコマンドをカバーしています。

### SET

`SET`コマンドはプロパティを設定し、既存のプロパティの値を返すか、既存のプロパティのリストをすべて返します。 既存プロパティのキーに値が指定された場合、古い値が上書きされます。

```sql
SET property_key = property_value
```

**パラメーター**

- `property_key`:リストまたは変更するプロパティの名前。
- `property_value`:プロパティを設定する値。

任意の設定の値を返すには、`property_value`を付けずに`SET [property key]`を使用します。

## PostgreSQL コマンド

以下のサブセクションは、クエリサービスがサポートするPostgreSQLコマンドについて説明します。

### BEGIN

`BEGIN`コマンド、または`BEGIN WORK`または`BEGIN TRANSACTION`コマンドが、トランザクションブロックを開始します。 beginコマンドの後に入力される文は、明示的なCOMMITまたはROLLBACKコマンドが指定されるまで、1つのトランザクションで実行されます。 このコマンドは`START TRANSACTION`と同じです。

```sql
BEGIN
BEGIN WORK
BEGIN TRANSACTION
```

### CLOSE

`CLOSE`コマンドは、オープンカーソルに関連付けられたリソースを解放します。 カーソルを閉じた後の操作は許可されません。不要になったカーソルは閉じる必要があります。

```sql
CLOSE name
CLOSE ALL
```

`CLOSE name`を使用する場合、`name`は閉じる必要がある開いたカーソルの名前を表します。 `CLOSE ALL`を使用すると、すべてのオープンカーソルが閉じられます。

### DEALLOCATE

`DEALLOCATE`コマンドを使用すると、以前準備したSQL文の割り当てを解除できます。 準備された文の割り当てを明示的に解除しない場合は、セッションが終了すると割り当てが解除されます。準備された文の詳細については、[PREPAREコマンド](#prepare)の節を参照してください。

```sql
DEALLOCATE name
DEALLOCATE ALL
```

`DEALLOCATE name`を使用する場合、`name`は、解放する必要がある準備された文の名前を表します。 `DEALLOCATE ALL`を使用すると、準備された文はすべて解放されます。

### DECLARE

`DECLARE`コマンドを使用すると、クエリを作成できます。カーソルは、大きなカーソルから少数の行を取得するのに使用できます。 カーソルが作成された後、`FETCH` を使用して行がカーソルから取得されます。

```sql
DECLARE name CURSOR FOR query
```

**パラメーター**

- `name`：作成するカーソルの名前。
- `query`：カーソルが返す行を指定する `SELECT` または `VALUES` コマンド。

### EXECUTE

`EXECUTE`コマンドは、事前に準備された文を実行するために使用します。 準備された文はセッション中の間だけ存在するので、準備された文は現在のセッションの前に実行された`PREPARE`文によって作成されなければなりません。 準備文の使用に関する詳細は、[`PREPARE`コマンド](#prepare)のセクションを参照してください。

ステートメントを作成した`PREPARE`ステートメントで一部のパラメーターが指定されている場合は、互換性のある一連のパラメーターを`EXECUTE`ステートメントに渡す必要があります。 これらのパラメーターが渡されない場合は、エラーが発生します。

```sql
EXECUTE name [ ( parameter ) ]
```

**パラメーター**

- `name`：実行する準備済み文の名前。
- `parameter`：準備済み文のパラメーターの実際の値。これは、準備済み文の作成時に決定された、このパラメーターのデータ型と互換性のある値を生成する式である必要があります。プリペアドステートメントに対して複数のパラメーターがある場合は、コンマで区切ります。

### EXPLAIN

`EXPLAIN`コマンドは、指定された文の実行計画を表示します。 実行計画には、文によって参照される表がスキャンされる方法が示されます。  複数のテーブルが参照されている場合、各入力テーブルの必要な行を統合するために使用される結合アルゴリズムが表示されます。

```sql
EXPLAIN option statement
```

`option`は次のいずれかになります。

```sql
ANALYZE
FORMAT { TEXT | JSON }
```

**パラメーター**

- `ANALYZE`:が `option` 含まれる場合 `ANALYZE`は、実行時間とその他の統計情報が表示されます。
- `FORMAT`:にが `option` 含まれる場合 `FORMAT`は、出力形式を指定します( `TEXT` または `JSON`)。テキスト以外の出力には、テキスト出力形式と同じ情報が含まれますが、プログラムの解析が容易です。このパラメーターのデフォルトは `TEXT` です。
- `statement`：実行計画を表示する `SELECT`、`INSERT`、`UPDATE`、`DELETE`、`VALUES`、`EXECUTE`、`DECLARE`、`CREATE TABLE AS`、`CREATE MATERIALIZED VIEW AS` 文のいずれか。

>[!IMPORTANT]
>
> `ANALYZE` オプションを使用すると、実際に文が実行されることに注意してください。`EXPLAIN` は、`SELECT` が返す出力を破棄しますが、文の他の副作用は通常どおり発生します。

**例**

次の例は、1つの`integer`列と10000行のテーブルに対する単純なクエリの計画を示しています。

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

`FETCH`コマンドは、以前に作成したカーソルを使用して行を取得します。

```sql
FETCH num_of_rows [ IN | FROM ] cursor_name
```

**パラメーター**

- `num_of_rows`:取得する行数。
- `cursor_name`:情報を取得するカーソルの名前。

### PREPARE {#prepare}

`PREPARE`コマンドを使用すると、準備された文を作成できます。 プリペアドステートメントは、サーバー側のオブジェクトで、同様のSQLステートメントをテンプレート化するのに使用できます。

準備された文は、パラメータを取ることができます。パラメータは、文の実行時にその文の中に置換される値です。 パラメーターは、準備されたステートメントを使用する場合、位置で参照され、$1、$2などが使用されます。

必要に応じて、パラメータデータタイプのリストを指定できます。 パラメーターのデータ型がリストにない場合、その型はコンテキストから推論できます。

```sql
PREPARE name [ ( data_type [, ...] ) ] AS SELECT
```

**パラメーター**

- `name`:プリペアドステートメントの名前。
- `data_type`:プリペアドステートメントのパラメーターのデータ型です。パラメーターのデータ型がリストにない場合、その型はコンテキストから推論できます。 複数のデータ型を追加する必要がある場合は、コンマ区切りリストで追加できます。

### ROLLBACK

`ROLLBACK`コマンドは現在のトランザクションを取り消し、トランザクションが行った更新をすべて破棄します。

```sql
ROLLBACK
ROLLBACK WORK
```

### SELECT INTO

`SELECT INTO`コマンドは、新しいテーブルを作成し、クエリが計算したデータを使用して入力します。 通常の`SELECT`コマンドを使用するので、データはクライアントに返されません。 新しいテーブルの列には、`SELECT`コマンドの出力列に関連付けられた名前とデータ型が表示されます。

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

標準のSELECTクエリパラメーターの詳細については、[SELECTクエリセクション](#select-queries)を参照してください。 このセクションでは、`SELECT INTO`コマンド専用のリストパラメーターのみを使用します。

- `TEMPORARY` または `TEMP`:オプションのパラメーター。指定した場合、作成されるテーブルは一時テーブルになります。
- `UNLOGGED`:オプションのパラメーター。指定した場合、として作成されるテーブルはログなしのテーブルになります。 ログに記録されていないテーブルに関する詳細は、[PostgreSQLドキュメント](https://www.postgresql.org/docs/current/sql-createtable.html)を参照してください。
- `new_table`:作成するテーブルの名前。

**例**

次のクエリは、テーブル`films`の最新のエントリのみから成る新しいテーブル`films_recent`を作成します。

```sql
SELECT * INTO films_recent FROM films WHERE date_prod >= '2002-01-01';
```

### SHOW

`SHOW`コマンドは、ランタイムパラメーターの現在の設定を表示します。 これらの変数は、`SET`文を使用して、`postgresql.conf`設定ファイルを編集するか、`PGOPTIONS`環境変数（libpqまたはlibpqベースのアプリケーションを使用する場合）を使用して、またはPostgresサーバーの起動時にコマンドラインフラグを使用して設定できます。

```sql
SHOW name
SHOW ALL
```

**パラメーター**

- `name`:情報を取得するランタイムパラメーターの名前。ランタイムパラメーターに指定できる値は次のとおりです。
   - `SERVER_VERSION`:このパラメーターは、サーバーのバージョン番号を表示します。
   - `SERVER_ENCODING`:このパラメーターは、サーバー側の文字セットエンコーディングを表示します。
   - `LC_COLLATE`:このパラメータは、照合（テキスト順序）に対するデータベースのロケール設定を示します。
   - `LC_CTYPE`:このパラメータは、文字分類に対するデータベースのロケール設定を表示します。
      `IS_SUPERUSER`:このパラメータは、現在の役割にスーパーユーザ権限があるかどうかを示します。
- `ALL`：すべての設定パラメーターの値と説明を表示します。

**例**

次のクエリは、パラメーター`DateStyle`の現在の設定を示しています。

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

`COPY`コマンドは、任意の`SELECT`クエリの出力を指定された場所にダンプします。 このコマンドを正常に実行するには、ユーザーがこの場所にアクセスできる必要があります。

```sql
COPY query
    TO '%scratch_space%/folder_location'
    [  WITH FORMAT 'format_name']
```

**パラメーター**

- `query`:コピーするクエリ。
- `format_name`:クエリをコピーする形式です。`format_name`は、`parquet`、`csv`、または`json`のいずれかです。 デフォルト値は`parquet`です。

>[!NOTE]
>
>完全な出力パスは`adl://<ADLS_URI>/users/<USER_ID>/acp_foundation_queryService/folder_location/<QUERY_ID>`です

### ALTER TABLE

`ALTER TABLE`コマンドを使用すると、主キーや外部キーの制約を追加または削除したり、テーブルに列を追加したりできます。

#### または追加DROP CONSTRAINT

次のSQLクエリは、表に制約を追加または削除する例を示しています。

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
- `referenced_table_name`:外部キーで参照されるテーブルの名前。
- `primary_column_name`:外部キーで参照される列の名前。

>[!NOTE]
>
>テーブルスキーマは一意であり、複数のテーブル間で共有されない必要があります。 また、主キーの制約にはこの名前空間が必須です。

#### 追加列

次のSQLクエリは、テーブルに列を追加する例を示しています。

```sql
ALTER TABLE table_name ADD COLUMN column_name data_type

ALTER TABLE table_name ADD COLUMN column_name_1 data_type1, column_name_2 data_type2 
```

**パラメーター**

- `table_name`:編集するテーブルの名前。
- `column_name`:追加する列の名前。
- `data_type`:追加する列のデータ型です。次のようなデータタイプがサポートされています。bigint, char，文字列，日付，日付，日時，重複,重複精度，整数， smallint, tinyint, varchar。

### プライマリキーを表示

`SHOW PRIMARY KEYS`コマンドは、指定したデータベースの主キー制約をすべてリストします。

```sql
SHOW PRIMARY KEYS
```

```console
    tableName | columnName    | datatype | namespace
------------------+----------------------+----------+-----------
 table_name_1 | column_name1  | text     | "ECID"
 table_name_2 | column_name2  | text     | "AAID"
```

### 外部キーを表示

`SHOW FOREIGN KEYS`コマンドは、指定したデータベースのすべての外部キー制約をリストします。

```sql
SHOW FOREIGN KEYS
```

```console
    tableName   |     columnName      | datatype | referencedTableName | referencedColumnName | namespace 
------------------+---------------------+----------+---------------------+----------------------+-----------
 table_name_1   | column_name1        | text     | table_name_3        | column_name3         |  "ECID"
 table_name_2   | column_name2        | text     | table_name_4        | column_name4         |  "AAID"
```
