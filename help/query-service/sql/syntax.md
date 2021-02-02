---
keywords: Experience Platform；ホーム；人気のあるトピック；クエリサービス；クエリサービス；SQL構文；SQL;CTAS;CTAS；選択としてテーブルを作成
solution: Experience Platform
title: SQL 構文
topic: syntax
description: このドキュメントは、クエリサービスでサポートされる SQL 構文を示します。
translation-type: tm+mt
source-git-commit: 14cb1d304fd8aad2ca287f8d66ac6865425db4c5
workflow-type: tm+mt
source-wordcount: '2212'
ht-degree: 83%

---


# SQL 構文

[!DNL Query Service] は、標準のANSI SQLを文や他の制限付きコマンドに使用する機能を `SELECT` 提供します。このドキュメントは、[!DNL Query Service]がサポートするSQL構文を示しています。

## SELECT クエリーの定義

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

ここで、`from_item` は以下のいずれかを指定できます。

```sql
table_name [ * ] [ [ AS ] alias [ ( column_alias [, ...] ) ] ]
    [ LATERAL ] ( select ) [ AS ] alias [ ( column_alias [, ...] ) ]
    with_query_name [ [ AS ] alias [ ( column_alias [, ...] ) ] ]
    from_item [ NATURAL ] join_type from_item [ ON join_condition | USING ( join_column [, ...] ) ]
```

`grouping_element` は以下のいずれかを指定できます。

```sql
( )
    expression
    ( expression [, ...] )
    ROLLUP ( { expression | ( expression [, ...] ) } [, ...] )
    CUBE ( { expression | ( expression [, ...] ) } [, ...] )
    GROUPING SETS ( grouping_element [, ...] )
```

`with_query` は以下になります。

```sql
 with_query_name [ ( column_name [, ...] ) ] AS ( select | values )
 
TABLE [ ONLY ] table_name [ * ]
```

### SNAPSHOT句

この句は、スナップショットIDに基づいてテーブルのデータを増分的に読み取るために使用できます。 スナップショットIDは、データが書き込まれるたびに、データレークテーブル上の数値（タイプLong）で識別されるチェックポイントマーカーです。 SNAPSHOT句は、その次に使用するテーブルの関係に付加されます。

```sql
    [ SNAPSHOT { SINCE start_snapshot_id | AS OF end_snapshot_id | BETWEEN start_snapshot_id AND end_snapshot_id } ]
```

#### 例

```sql
SELECT * FROM Customers SNAPSHOT SINCE 123;

SELECT * FROM Customers SNAPSHOT AS OF 345;

SELECT * FROM Customers SNAPSHOT BETWEEN 123 AND 345;

SELECT * FROM (SELECT id FROM CUSTOMERS BETWEEN 123 AND 345) C 

SELECT * FROM Customers SNAPSHOT SINCE 123 INNER JOIN Inventory AS OF 789 ON Customers.id = Inventory.id;
```

SNAPSHOT句はテーブルまたはテーブルの別名で使用できますが、サブクエリまたは表示の上には使用できません。 SNAPHOST句は、テーブルにSELECTクエリを適用できるあらゆる場所で機能します。

### WHERE ILIKE 句

SELECT クエリの WHERE 句で LIKE キーワードの代わりに ILIKE を使用して、大文字と小文字を区別せずに一致させることができます。

```sql
    [ WHERE condition { LIKE | ILIKE | NOT LIKE | NOT ILIKE } pattern ]
```

LIKE 句と ILIKE 句のロジックは次のとおりです。
- ```WHERE condition LIKE pattern```（```~~``` と同じ）
- ```WHERE condition NOT LIKE pattern```（```!~~``` と同じ）
- ```WHERE condition ILIKE pattern```（```~~*``` と同じ）
- ```WHERE condition NOT ILIKE pattern```（```!~~*``` と同じ）


#### 例

```sql
SELECT * FROM Customers
WHERE CustomerName ILIKE 'a%';
```

名前が「A」または「a」で始まる顧客を返します。

## JOIN

JOIN を使用する `SELECT` クエリの構文は次のとおりです。

```sql
SELECT statement
FROM statement
[JOIN | INNER JOIN | LEFT JOIN | LEFT OUTER JOIN | RIGHT JOIN | RIGHT OUTER JOIN | FULL JOIN | FULL OUTER JOIN]
ON join condition
```


## UNION、INTERSECT、EXCEPT

`UNION`、`INTERSECT`、`EXCEPT`句は、複数のテーブルから同様の行を結合または除外するためのものです。

```sql
SELECT statement 1
[UNION | UNION ALL | UNION DISTINCT | INTERSECT | EXCEPT | MINUS]
SELECT statement 2
```

## CREATE TABLE AS SELECT

次の構文は、[!DNL Query Service]がサポートする`CREATE TABLE AS SELECT` (CTAS)クエリを定義します。

```sql
CREATE TABLE table_name [ WITH (schema='target_schema_title', rowvalidation='false') ] AS (select_query)
```

ここで
`target_schema_title`はXDMスキーマのタイトルです。 この句は、CTASクエリが作成した新しいデータセットに対して既存のXDMスキーマを使用する場合にのみ使用してください
`rowvalidation`は、新しく作成されたデータセットに対して取り込まれたすべての新しいバッチについて、行レベルの検証を必要とするかどうかを指定します。 デフォルト値は「true」です。

`select_query` は `SELECT` 文で、その構文は上述されています。


### 例

```sql
CREATE TABLE Chairs AS (SELECT color, count(*) AS no_of_chairs FROM Inventory i WHERE i.type=="chair" GROUP BY i.color)
CREATE TABLE Chairs WITH (schema='target schema title') AS (SELECT color, count(*) AS no_of_chairs FROM Inventory i WHERE i.type=="chair" GROUP BY i.color)
```

特定の CTAS クエリについて：

1. `SELECT` 文には、`COUNT`、`SUM`、`MIN` などの集計関数のエイリアスが含まれている必要があります。
2. `SELECT` 文は () 括弧ありでもなしでも使用できます。
3. `SELECT`ステートメントには、増分差分をターゲットテーブルに読み込むためのSNAPSHOT句を指定できます。

```sql
CREATE TABLE Chairs AS (SELECT color FROM Inventory SNAPSHOT SINCE 123)
```

## INSERT INTO

次の構文は、[!DNL Query Service]がサポートする`INSERT INTO`クエリを定義します。

```sql
INSERT INTO table_name select_query
```

`select_query` は `SELECT` 文で、その構文は上述されています。

### 例

```sql
INSERT INTO Customers SELECT SupplierName, City, Country FROM OnlineCustomers;
```

指定した INSERT INTO クエリに対して：

1. `SELECT` 文は () 括弧で囲んではいけません。
2. `SELECT` 文の結果のスキーマは、`INSERT INTO` 文で定義されたテーブルの結果に準拠している必要があります。
3. `SELECT`ステートメントには、増分差分をターゲットテーブルに読み込むためのSNAPSHOT句を指定できます。

```sql
INSERT INTO Customers AS (SELECT * from OnlineCustomers SNAPSHOT AS OF 345)
```

### DROP TABLE

テーブルが EXTERNAL テーブルでない場合は、テーブルを削除し、テーブルに関連付けられたディレクトリーをファイルシステムから削除します。削除するテーブルが存在しない場合は、例外が発生します。

```sql
DROP [TEMP] TABLE [IF EXISTS] [db_name.]table_name
```

### パラメーター

- `IF EXISTS`：テーブルが存在しない場合は、何も起こりません
- `TEMP`：一時テーブル

## CREATE VIEW

次の構文は、[!DNL Query Service]がサポートする`CREATE VIEW`クエリを定義します。

```sql
CREATE [ OR REPLACE ] VIEW view_name AS select_query
```

ここで、`view_name` は、作成するビューの名前です。`select_query` は `SELECT` 文で、その構文は前述したとおりです。

例：

```sql
CREATE VIEW V1 AS SELECT color, type FROM Inventory
CREATE OR REPLACE VIEW V1 AS SELECT model, version FROM Inventory
```

### DROP VIEW

次の構文は、[!DNL Query Service]がサポートする`DROP VIEW`クエリを定義します。

```sql
DROP VIEW [IF EXISTS] view_name
```

ここで、`view_name` は削除するビューの名前です。

例：

```sql
DROP VIEW v1
DROP VIEW IF EXISTS v1
```

## [!DNL Spark] SQLコマンド

### SET

プロパティを設定するか、既存プロパティの値を返すか、すべての既存プロパティをリストします。既存プロパティのキーに値が指定された場合、古い値が上書きされます。

```sql
SET property_key [ To | =] property_value
```

任意の設定の値を返すには、`SHOW [setting name]` を使用します。

## PostgreSQL コマンド

### BEGIN

このコマンドは解析され、完了したコマンドがクライアントに送り返されます。これは `START TRANSACTION` コマンドと同じです。

```sql
BEGIN [ TRANSACTION ]
```

#### パラメーター

- `TRANSACTION`：キーワード（オプション）。リッスンします。操作は行われません。

### CLOSE

`CLOSE` は、開いたカーソルに関連付けられたリソースを解放します。カーソルを閉じた後の操作は許可されません。不要になったカーソルは閉じる必要があります。

```sql
CLOSE { name }
```

#### パラメーター

- `name`：閉じられる、開いたカーソルの名前。

### COMMIT

[!DNL Query Service]では、コミットトランザクション文への応答としてのアクションは行われません。

```sql
COMMIT [ WORK | TRANSACTION ]
```

#### パラメーター

- `WORK`
- `TRANSACTION`：キーワード（オプション）。何の効果もありません。

### DEALLOCATE

`DEALLOCATE` は、事前に準備された SQL 文の割り当てを解除する場合に使用します。準備された文の割り当てを明示的に解除しない場合は、セッションが終了すると割り当てが解除されます。

```sql
DEALLOCATE [ PREPARE ] { name | ALL }
```

#### パラメーター

- `Prepare`：このキーワードは無視されます。
- `name`：割り当てを解除する準備済み文の名前。
- `ALL`：すべての準備文の割り当てを解除します。

### DECLARE

`DECLARE` を使用するとカーソルを作成できます。これは、大きなクエリ一から一度に少数の行を取得するために使用できます。カーソルが作成された後、`FETCH` を使用して行がカーソルから取得されます。

```sql
DECLARE name CURSOR [ WITH  HOLD ] FOR query
```

#### パラメーター

- `name`：作成するカーソルの名前。
- `WITH HOLD`：カーソルを作成したトランザクションが正常にコミットされた後も、カーソルを引き続き使用できるように指定します。
- `query`：カーソルが返す行を指定する `SELECT` または `VALUES` コマンド。

### EXECUTE

`EXECUTE` は、事前に準備済みの文を実行するために使用されます。準備済み文はセッション中にのみ存在するので、現在のセッション中に前もって実行された `PREPARE` 文によって作成されたものでなければなりません。

`PREPARE` 文を作成した文が一部のパラメーターを指定した場合は、互換性のある一連のパラメーターを `EXECUTE` 文に渡す必要があります。そうしないと、エラーが発生します。関数とは異なり、準備済み文は、パラメーターの型や数に基づいてオーバーロードされません。準備済み文の名前は、データベースセッション内で一意である必要があります。

```sql
EXECUTE name [ ( parameter [, ...] ) ]
```

#### パラメーター

- `name`：実行する準備済み文の名前。
- `parameter`：準備済み文のパラメーターの実際の値。これは、準備済み文の作成時に決定された、このパラメーターのデータ型と互換性のある値を生成する式である必要があります。

### EXPLAIN

このコマンドは、指定された文に対して PostgreSQL プランナーが生成する実行計画を表示します。実行計画は、文によって参照されるテーブルがどのようにスキャンされるか（単純な順次スキャン、インデックススキャンなど）を示します。複数のテーブルが参照されている場合、各入力テーブルから必要な行をまとめるのに使用される結合アルゴリズムも示されます。

表示の最も重要な部分は、文の実行コストの見積もりです。これは、プランナーが文の実行にかかる時間を推測したものです（任意のコスト単位で測定されますが、従来はディスクページの取得を意味します）。実際には、最初の行を返す前の開始上のコストと、すべての行を返す総コストの 2 つの数値が表示されます。ほとんどのクエリでは、総コストが重要ですが、EXISTS　のサブクエリーなどのコンテキストでは、プランナーは最小の総コストではなく最小の起動コストを選択します（実行プログラムはどちらにしても 1 行を取得した後に停止するため）。また、`LIMIT` 句で返す行数を制限すると、プランナーはエンドポイントのコストを適切に補間して、実際に最も安いプランを見積もります。 

`ANALYZE` オプションを使用すると、文は計画されるだけでなく、実行されます。次に、各計画ノード内で費やされた合計経過時間（ミリ秒）と、返された合計行数を含む、実際の実行時間統計が表示に追加されます。これは、プランナーの予測が現実に近いかどうかを調べるのに役立ちます。

```sql
EXPLAIN [ ( option [, ...] ) ] statement
EXPLAIN [ ANALYZE ] statement

where option can be one of:
    ANALYZE [ boolean ]
    TYPE VALIDATE
    FORMAT { TEXT | JSON }
```

#### パラメーター

- `ANALYZE`：コマンドを実行し、実行時間やその他の統計を表示します。このパラメーターのデフォルトは `FALSE` です。
- `FORMAT`：出力形式を指定します（TEXT、XML、JSON、YAML）。テキスト以外の出力には、テキスト出力形式と同じ情報が含まれますが、プログラムの解析が容易です。このパラメーターのデフォルトは `TEXT` です。
- `statement`：実行計画を表示する `SELECT`、`INSERT`、`UPDATE`、`DELETE`、`VALUES`、`EXECUTE`、`DECLARE`、`CREATE TABLE AS`、`CREATE MATERIALIZED VIEW AS` 文のいずれか。

>[!IMPORTANT]
>
> `ANALYZE` オプションを使用すると、実際に文が実行されることに注意してください。`EXPLAIN` は、`SELECT` が返す出力を破棄しますが、文の他の副作用は通常どおり発生します。

#### 例

以下は、単一の `integer` 列と 10000 行のテーブルに、単純なクエリの計画を表示します。

```sql
EXPLAIN SELECT * FROM foo;

                       QUERY PLAN
---------------------------------------------------------
 Seq Scan on foo  (cost=0.00..155.00 rows=10000 width=4)
(1 row)
```

### FETCH

`FETCH` は、以前に作成したカーソルを使用して行を取得します。

カーソルには関連付けられている位置があり、これは `FETCH` によって使用されます。カーソル位置は、クエリ一結果の最初の行の前、特定の行の前、または最後の行の後に設定できます。作成時のカーソル位置は、最初の行の前です。行を取得すると、カーソル位置は最も最近取得した行になります。`FETCH` が使用可能な行の末尾を越えた場合、カーソル位置は最後の行の後のままです。該当する行がない場合は、空の結果が返され、必要に応じて、カーソル位置は最初の行の前または最後の行の後になります。

```sql
FETCH num_of_rows [ IN | FROM ] cursor_name
```

#### パラメーター

- `num_of_rows`：取得する行の位置または数を指定する整数定数（符号付きの場合あり）。
- `cursor_name`：開いたカーソルの名前。

### PREPARE

`PREPARE` は準備済み文を作成します。準備済み文は、パフォーマンスの最適化に使用できるサーバーサイドのオブジェクトです。`PREPARE` 文が実行されると、指定した文が解析、分析され、書き換えられます。その後、`EXECUTE` コマンドが発行されると、準備済み文が計画され、実行されます。この分業によって、繰り返しの解析分析作業を避けると共に、実行計画を指定された特定のパラメーター値に依存させることができます。

準備済み文は、パラメーターを受け取ることができます。パラメーターは、文の実行時に文内で置き換えられる値です。準備済み文を作成する場合は、$1、$2 などを使用して、位置別にパラメーターを参照します。対応するリストのパラメーターデータ型を任意で指定できます。パラメーターのデータ型が指定されていない場合、または不明として宣言されている場合、可能であれば、パラメーターが最初に参照されるコンテキストから型が推論されます。文を実行する場合は、`EXECUTE` 文内のこれらのパラメーターの実際の値を指定します。

準備済み文は、現在のデータベースセッションの間のみ有効です。セッションが終わると、準備済み文は忘れられるので、再び使用する前に再作成する必要があります。つまり、複数の同時データベースクライアントが 1 つの準備済み文を使用することはできません。ただし、各クライアントは独自の準備済み文を作成して使用できます。準備済み文は、`DEALLOCATE` コマンドを使用して手動でクリーンアップできます。

準備済み文は、1 つのセッションを使用して多数の類似の文を実行する場合、パフォーマンスが最も高くなる可能性があります。パフォーマンスの違いは、クエリが多数のテーブルの結合を含む場合や、複数のルールの適用を必要とする場合など、文が計画や書き換えを複雑におこなう場合に特に重要です。計画と書き換えが比較的簡単で、実行に費やすコストが比較的高い文の場合、準備済み文のパフォーマンス上の利点は目立ちません。

```sql
PREPARE name [ ( data_type [, ...] ) ] AS SELECT
```

#### パラメーター

- `name`：この準備済み文に与えられる任意の名前。この変数は、単一のセッション内で一意である必要があり、その後、事前に準備された文を実行または割り当て解除するために使用されます。
- `data-type`：準備済み文のパラメーターのデータ型。特定のパラメーターのデータ型が指定されていない場合、または不明なデータ型が指定されている場合は、そのパラメーターが最初に参照されるコンテキストから推定されます。準備済み文自体のパラメーターを参照するには、$1、$2 などを使用します。


### ROLLBACK

`ROLLBACK` は、現在のトランザクションをロールバックし、トランザクションによって行われたすべての更新を破棄します。

```sql
ROLLBACK [ WORK ]
```

#### パラメーター

- `WORK`

### SELECT INTO

`SELECT INTO` は新しいテーブルを作成し、クエリで計算されたデータで埋めます。通常の `SELECT` の場合とは異なり、データはクライアントに返されません 。新しいテーブルの列には、`SELECT` の出力列に関連付けられた名前とデータ型が含まれます。

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

#### パラメーター

- `TEMPORARAY` または `TEMP`：指定した場合、テーブルは一時テーブルとして作成されます。
- `UNLOGGED:`：指定した場合、テーブルはログなしのテーブルとして作成されます。
- `new_table` 作成するテーブルの名前（スキーマ修飾名も可）。

#### 例

`films` テーブルの最近のエントリのみから構成される新しい `films_recent` テーブルを作成します。

```sql
SELECT * INTO films_recent FROM films WHERE date_prod >= '2002-01-01';
```

### SHOW

`SHOW` は実行時パラメーターの現在の設定を表示します。これらの変数は、`SET` 文を使用して、postgresql.conf 設定ファイルを編集するか、`PGOPTIONS` 環境変数を介して（libpq または libpq ベースのアプリケーションを使用している場合）、または postgres サーバーを起動する際にコマンドラインフラグを使用して設定できます。

```sql
SHOW name
```

#### パラメーター

-  `name`。
   - `SERVER_VERSION`：サーバーのバージョン番号を表示します。
   - `SERVER_ENCODING`：サーバーサイドの文字セットエンコーディングを表示します。エンコーディングはデータベース作成時に決定されるので、現時点では、このパラメーターは表示できるだけで設定できません。
   - `LC_COLLATE`：照合（テキストの順序付け）のためのデータベースのロケール設定を表示します。設定はデータベース作成時に決定されるので、現時点では、このパラメーターは表示できるだけで設定できません。
   - `LC_CTYPE`：文字分類に関するデータベースのロケール設定を表示します。設定はデータベース作成時に決定されるので、現時点では、このパラメーターは表示できるだけで設定できません。
      `IS_SUPERUSER`：現在の役割にスーパーユーザー権限がある場合は true です。
- `ALL`：すべての設定パラメーターの値と説明を表示します。

#### 例

`DateStyle` パラメーターの現在の設定を表示します

```sql
SHOW DateStyle;
 DateStyle
-----------
 ISO, MDY
(1 row)
```

### START TRANSACTION

このコマンドは解析され、完了したコマンドがクライアントに返されます。これは `BEGIN` コマンドと同じです。

```sql
START TRANSACTION [ transaction_mode [, ...] ]

where transaction_mode is one of:

    ISOLATION LEVEL { SERIALIZABLE | REPEATABLE READ | READ COMMITTED | READ UNCOMMITTED }
    READ WRITE | READ ONLY
```

### コピー

このコマンドは、任意のSELECTクエリの出力を指定された場所にダンプします。 このコマンドを正常に実行するには、ユーザーがこの場所にアクセスできる必要があります。

```sql
COPY  query
    TO '%scratch_space%/folder_location'
    [  WITH FORMAT 'format_name']

where 'format_name' is be one of:
    'parquet', 'csv', 'json'

'parquet' is the default format.
```

>[!NOTE]
>
>完全な出力パスは`adl://<ADLS_URI>/users/<USER_ID>/acp_foundation_queryService/folder_location/<QUERY_ID>`です


### ALTER

このコマンドは、主キー制約または外部キー制約をテーブルに追加または削除するのに役立ちます。

```sql
Alter TABLE table_name ADD CONSTRAINT Primary key ( column_name )

Alter TABLE table_name ADD CONSTRAINT Foreign key ( column_name ) references referenced_table_name ( primary_column_name )

Alter TABLE table_name ADD CONSTRAINT Foreign key ( column_name ) references referenced_table_name Namespace 'namespace'

Alter TABLE table_name DROP CONSTRAINT Primary key ( column_name )

Alter TABLE table_name DROP CONSTRAINT  Foreign key ( column_name )
```

>[!NOTE]
>テーブルスキーマは一意であり、複数のテーブル間で共有されない必要があります。 また、この名前空間は必須です。


### プライマリキーを表示

このコマンドは、指定したデータベースのすべての主キー制約をリストします。

```sql
SHOW PRIMARY KEYS
    tableName | columnName    | datatype | namespace
------------------+----------------------+----------+-----------
 table_name_1 | column_name1  | text     | "ECID"
 table_name_2 | column_name2  | text     | "AAID"
```


### 外部キーを表示

このコマンドは、指定したデータベースのすべての外部キー制約をリストします。

```sql
SHOW FOREIGN KEYS
    tableName   |     columnName      | datatype | referencedTableName | referencedColumnName | namespace 
------------------+---------------------+----------+---------------------+----------------------+-----------
 table_name_1   | column_name1        | text     | table_name_3        | column_name3         |  "ECID"
 table_name_2   | column_name2        | text     | table_name_4        | column_name4         |  "AAID"
```
