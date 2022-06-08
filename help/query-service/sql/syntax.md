---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；SQL 構文；SQL;CTAS;CTAS；選択としてテーブルを作成
solution: Experience Platform
title: クエリサービスの SQL 構文
topic-legacy: syntax
description: このドキュメントでは、Adobe Experience Platformクエリサービスでサポートされる SQL 構文を示します。
exl-id: 2bd4cc20-e663-4aaa-8862-a51fde1596cc
source-git-commit: 17a90bb716bd64c9fb9b383a5ffa49598e978288
workflow-type: tm+mt
source-wordcount: '3042'
ht-degree: 9%

---

# クエリサービスの SQL 構文

Adobe Experience Platformクエリサービスは、 `SELECT` ステートメントおよびその他の制限付きコマンド このドキュメントでは、 [!DNL Query Service].

## クエリを選択 {#select-queries}

次の構文は、 `SELECT` 次をサポートするクエリ [!DNL Query Service]:

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

場所 `from_item` は、次のいずれかのオプションです。

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

および `grouping_element` は、次のいずれかのオプションです。

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

以下のサブセクションでは、上記の形式に従う場合に、クエリで使用できる追加の句の詳細を示します。

### SNAPSHOT 句

この句は、スナップショット ID に基づいてテーブルのデータを増分的に読み取るために使用できます。 スナップショット ID は、データが書き込まれるたびにデータレイクテーブルに適用される長整数型の数字で表されるチェックポイントマーカーです。 この `SNAPSHOT` 句は、その句の隣で使用されるテーブルの関係にそれ自体を付加します。

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

なお、 `SNAPSHOT` 句は、テーブルまたはテーブルのエイリアスに対して機能しますが、サブクエリまたはビューの上には機能しません。 A `SNAPSHOT` 節はどこでも機能する `SELECT` テーブルのクエリを適用できます。

また、 `HEAD` および `TAIL` スナップショット句の特別なオフセット値として。 使用 `HEAD` は最初のスナップショットの前のオフセットを指し、 `TAIL` は、最後のスナップショットの後のオフセットを指します。

>[!NOTE]
>
>2 つのスナップショット ID 間でクエリを実行し、開始スナップショットが期限切れになった場合、オプションのフォールバック動作フラグ (`resolve_fallback_snapshot_on_failure`) が設定されている場合は次のようになります。
>
>- オプションのフォールバック動作フラグが設定されている場合、クエリサービスは、利用可能な最も古いスナップショットを選択し、開始スナップショットとして設定し、利用可能な最も古いスナップショットと指定された終了スナップショットの間のデータを返します。 このデータは **包括的** 最も古いスナップショットの
>
>- オプションのフォールバック動作フラグが設定されていない場合は、エラーが返されます。


### WHERE 句

デフォルトでは、 `WHERE` 条項 `SELECT` クエリでは大文字と小文字が区別されます。 一致で大文字と小文字を区別しない場合は、キーワード `ILIKE` の代わりに `LIKE`.

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

A `SELECT` 結合を使用するクエリの構文は次のとおりです。

```sql
SELECT statement
FROM statement
[JOIN | INNER JOIN | LEFT JOIN | LEFT OUTER JOIN | RIGHT JOIN | RIGHT OUTER JOIN | FULL JOIN | FULL OUTER JOIN]
ON join condition
```

### UNION、INTERSECT、EXCEPT

この `UNION`, `INTERSECT`、および `EXCEPT` 句は、2 つ以上のテーブルから同様の行を結合または除外するために使用します。

```sql
SELECT statement 1
[UNION | UNION ALL | UNION DISTINCT | INTERSECT | EXCEPT | MINUS]
SELECT statement 2
```

### CREATE TABLE AS SELECT

次の構文は、 `CREATE TABLE AS SELECT` (CTAS) クエリ：

```sql
CREATE TABLE table_name [ WITH (schema='target_schema_title', rowvalidation='false') ] AS (select_query)
```

| パラメーター | 説明 |
| ----- | ----- |
| `schema` | XDM スキーマのタイトル。 この句は、CTAS クエリで作成された新しいデータセットに対して既存の XDM スキーマを使用する場合にのみ使用します。 |
| `rowvalidation` | （オプション）新しく作成したデータセットに対して取り込まれる新しいバッチの行レベルの検証を、すべてユーザーが必要とするかどうかを指定します。 デフォルト値は `true` です。 |
| `select_query` | A `SELECT` 文。 の構文 `SELECT` クエリは、 [「SELECT queries」セクション](#select-queries). |

**例**

```sql
CREATE TABLE Chairs AS (SELECT color, count(*) AS no_of_chairs FROM Inventory i WHERE i.type=="chair" GROUP BY i.color)

CREATE TABLE Chairs WITH (schema='target schema title') AS (SELECT color, count(*) AS no_of_chairs FROM Inventory i WHERE i.type=="chair" GROUP BY i.color)

CREATE TABLE Chairs AS (SELECT color FROM Inventory SNAPSHOT SINCE 123)
```

>[!NOTE]
>
>`SELECT` 文には、`COUNT`、`SUM`、`MIN` などの集計関数のエイリアスが含まれている必要があります。また、 `SELECT` 文は () 括弧ありでもなしでも使用できます。 次の項目を指定できます。 `SNAPSHOT` 増分デルタをターゲットテーブルに読み込むための句。

## INSERT INTO

この `INSERT INTO` コマンドは次のように定義されます。

```sql
INSERT INTO table_name select_query
```

| パラメーター | 説明 |
| ----- | ----- |
| `table_name` | クエリを挿入するテーブルの名前。 |
| `select_query` | A `SELECT` 文。 の構文 `SELECT` クエリは、 [「SELECT queries」セクション](#select-queries). |

**例**

>[!NOTE]
>
>以下は、単に説明目的で考案された例です。

```sql
INSERT INTO Customers SELECT SupplierName, City, Country FROM OnlineCustomers;

INSERT INTO Customers AS (SELECT * from OnlineCustomers SNAPSHOT AS OF 345)
```

>[!INFO]
> 
> この `SELECT` 文 **次の値を指定する** は () 括弧で囲んでください。 また、 `SELECT` 文は、 `INSERT INTO` 文。 次の項目を指定できます。 `SNAPSHOT` 増分デルタをターゲットテーブルに読み込むための句。

実際の XDM スキーマ内のフィールドのほとんどは、ルートレベルで見つからず、SQL ではドット表記の使用は許可されていません。 ネストされたフィールドを使用して現実的な結果を得るには、 `INSERT INTO` パス。

宛先 `INSERT INTO` ネストされたパスには、次の構文を使用します。

```sql
INSERT INTO [dataset]
SELECT struct([source field1] as [target field in schema],
[source field2] as [target field in schema],
[source field3] as [target field in schema]) [tenant name]
FROM [dataset]
```

**例**

```sql
INSERT INTO Customers SELECT struct(SupplierName as Supplier, City as SupplierCity, Country as SupplierCountry) _Adobe FROM OnlineCustomers;
```

## DROP TABLE

この `DROP TABLE` コマンドは、既存のテーブルを削除し、外部テーブルでない場合は、テーブルに関連付けられたディレクトリをファイルシステムから削除します。 テーブルが存在しない場合は、例外が発生します。

```sql
DROP TABLE [IF EXISTS] [db_name.]table_name
```

| パラメーター | 説明 |
| ------ | ------ |
| `IF EXISTS` | これを指定した場合、テーブルが **not** 存在する。 |

## データベースを作成

この `CREATE DATABASE` コマンドは ADLS データベースを作成します。

```sql
CREATE DATABASE [IF NOT EXISTS] db_name
```

## データベースを削除

この `DROP DATABASE` コマンドは、インスタンスからデータベースを削除します。

```sql
DROP DATABASE [IF EXISTS] db_name
```

| パラメーター | 説明 |
| ------ | ------ |
| `IF EXISTS` | これを指定した場合、データベースが **not** 存在する。 |

## スキーマをドロップ

この `DROP SCHEMA` コマンドは既存のスキーマを削除します。

```sql
DROP SCHEMA [IF EXISTS] db_name.schema_name [ RESTRICT | CASCADE]
```

| パラメーター | 説明 |
| ------ | ------ |
| `IF EXISTS` | これを指定した場合、スキーマが **not** 存在する。 |
| `RESTRICT` | モードのデフォルト値。 これを指定した場合、スキーマは指定した場合にのみ削除されます **doesn&#39;t** には、任意のテーブルが含まれます。 |
| `CASCADE` | これを指定した場合、スキーマは、スキーマ内に存在するすべてのテーブルと共にドロップされます。 |

## CREATE VIEW

次の構文は、 `CREATE VIEW` クエリ：

```sql
CREATE VIEW view_name AS select_query
```

| パラメーター | 説明 |
| ------ | ------ |
| `view_name` | 作成するビューの名前。 |
| `select_query` | A `SELECT` 文。 の構文 `SELECT` クエリは、 [「SELECT queries」セクション](#select-queries). |

**例**

```sql
CREATE VIEW V1 AS SELECT color, type FROM Inventory

CREATE OR REPLACE VIEW V1 AS SELECT model, version FROM Inventory
```

## DROP VIEW

次の構文は、 `DROP VIEW` クエリ：

```sql
DROP VIEW [IF EXISTS] view_name
```

| パラメーター | 説明 |
| ------ | ------ |
| `IF EXISTS` | これを指定した場合、ビューが **not** 存在する。 |
| `view_name` | 削除するビューの名前。 |

**例**

```sql
DROP VIEW v1
DROP VIEW IF EXISTS v1
```

## 匿名ブロック

匿名ブロックは、次の 2 つのセクションで構成されます。実行可能セクションと例外処理セクション 匿名ブロックでは、実行可能セクションは必須です。 ただし、例外処理セクションはオプションです。

次の例は、1 つ以上のステートメントを組み合わせて実行するブロックの作成方法を示しています。

```sql
BEGIN
  statementList
[EXCEPTION exceptionHandler]
END

exceptionHandler:
      WHEN OTHER
      THEN statementList

statementList:
    : (statement (';')) +
```

匿名ブロックの使用例を以下に示します。

```sql
BEGIN
   SET @v_snapshot_from = select parent_id  from (select history_meta('email_tracking_experience_event_dataset') ) tab where is_current;
   SET @v_snapshot_to = select snapshot_id from (select history_meta('email_tracking_experience_event_dataset') ) tab where is_current;
   SET @v_log_id = select now();
   CREATE TABLE tracking_email_id_incrementally
     AS SELECT _id AS id FROM email_tracking_experience_event_dataset SNAPSHOT BETWEEN @v_snapshot_from AND @v_snapshot_to;

EXCEPTION
  WHEN OTHER THEN
    DROP TABLE IF EXISTS tracking_email_id_incrementally;
    SELECT 'ERROR';
END;
```

## データアセットの組織

Adobe Experience Platformデータレイク内のデータアセットを、成長に合わせて論理的に整理することが重要です。 クエリサービスは、サンドボックス内のデータアセットを論理的にグループ化できる SQL 構成を拡張します。 この編成方法を使用すると、データアセットを物理的に移動する必要なく、スキーマ間でデータアセットを共有できます。

次の SQL 構文では、標準の SQL 構文を使用して、データを論理的に整理できます。

```SQL
CREATE DATABASE dg1;
CREATE SCHEMA dg1.schema1;
CREATE table t1 ...;
CREATE view v1 ...;
ALTER TABLE t1 ADD PRIMARY KEY (c1) NOT ENFORCED;
ALTER TABLE t2 ADD FOREIGN KEY (c1) REFERENCES t1(c1) NOT ENFORCED;
```

詳しくは、 [データアセットの論理的な編成](../best-practices/organize-data-assets.md) を参照してください。

## テーブルが存在します。

この `table_exists` SQL コマンドは、現在システムにテーブルが存在するかどうかを確認するために使用します。 このコマンドは、ブール値を返します（テーブルが存在&#x200B;**する**&#x200B;場合は `true`、テーブルが存在&#x200B;**しない**&#x200B;場合は `false`）。

文を実行する前にテーブルが存在するかどうかを検証すると、 `table_exists` 機能を使用すると、匿名ブロックを書き込んで、 `CREATE` および `INSERT INTO` ユースケース。

次の構文は、 `table_exists` コマンド：

```SQL
$$
BEGIN

#Set mytableexist to true if the table already exists.
SET @mytableexist = SELECT table_exists('target_table_name');

#Create the table if it does not already exist (this is a one time operation).
CREATE TABLE IF NOT EXISTS target_table_name AS
  SELECT *
  FROM   profile_dim_date limit 10;

#Insert data only if the table already exists. Check if @mytableexist = 'true'
 INSERT INTO target_table_name           (
                     select *
                     from   profile_dim_date
                     WHERE  @mytableexist = 'true' limit 20
              ) ;
EXCEPTION
WHEN other THEN SELECT 'ERROR';

END $$; 
```

## インライン {#inline}

この `inline` 関数は、構造体の配列の要素を分割し、値をテーブルに生成します。 これは、 `SELECT` リストまたは `LATERAL VIEW`.

この `inline` 関数 **できません** 他のジェネレータ関数がある選択リストに配置します。

デフォルトでは、生成される列の名前は「col1」、「col2」などです。 式が `NULL` その場合、行は生成されません。

>[!TIP]
>
>列名は `RENAME` コマンドを使用します。

**例**

```sql
> SELECT inline(array(struct(1, 'a'), struct(2, 'b'))), 'Spark SQL';
```

この例では、次の値が返されます。

```text
1  a Spark SQL
2  b Spark SQL
```

この 2 つ目の例では、 `inline` 関数に置き換えます。 この例のデータモデルを次の画像に示します。

![productListItems のスキーマ図](../images/sql/productListItems.png)

**例**

```sql
select inline(productListItems) from source_dataset limit 10;
```

値は `source_dataset` を使用してターゲットテーブルに入力します。

| SKU | _experience | quantity | priceTotal |
|---------------------|-----------------------------------|----------|--------------|
| product-id-1 | (&quot;(&quot;(&quot;(A,pass,B,NULL)&quot;)&quot;)&quot;)&quot; | 5 | 10.5 |
| product-id-5 | (&quot;(&quot;(&quot;(A, pass, B,NULL)&quot;)&quot;)&quot;)&quot; |  |  |
| product-id-2 | (&quot;(&quot;(&quot;(AF, C, D,NULL)&quot;)&quot;)&quot;)&quot; | 6 | 40 |
| product-id-4 | (&quot;(&quot;(&quot;(BM, pass, NA,NULL)&quot;)&quot;)&quot;)&quot; | 3 | 12 |

## [!DNL Spark] SQL コマンド

次のサブ節では、クエリサービスでサポートされる Spark SQL コマンドについて説明します。

### SET

この `SET` コマンドは、プロパティを設定し、既存のプロパティの値を返すか、既存のプロパティをすべてリストします。 既存プロパティのキーに値が指定された場合、古い値が上書きされます。

```sql
SET property_key = property_value
```

| パラメーター | 説明 |
| ------ | ------ |
| `property_key` | リストまたは変更するプロパティの名前。 |
| `property_value` | プロパティを設定する値です。 |

任意の設定の値を返すには、 `SET [property key]` 無しに `property_value`.

## PostgreSQL コマンド

以下のサブ節では、クエリサービスでサポートされる PostgreSQL コマンドについて説明します。

### BEGIN

この `BEGIN` コマンドを使用するか、または `BEGIN WORK` または `BEGIN TRANSACTION` コマンドを使用して、トランザクションブロックを開始します。 begin コマンドの後に入力された文は、明示的な COMMIT または ROLLBACK コマンドが指定されるまで、単一のトランザクションで実行されます。 このコマンドは、 `START TRANSACTION`.

```sql
BEGIN
BEGIN WORK
BEGIN TRANSACTION
```

### CLOSE

この `CLOSE` コマンドは、開いたカーソルに関連付けられたリソースを解放します。 カーソルを閉じた後の操作は許可されません。不要になったカーソルは閉じる必要があります。

```sql
CLOSE name
CLOSE ALL
```

If `CLOSE name` が使用されます。 `name` は、閉じる必要がある開いたカーソルの名前を表します。 If `CLOSE ALL` を使用すると、すべてのオープンカーソルが閉じられます。

### DEALLOCATE

この `DEALLOCATE` コマンドを使用すると、事前に準備された SQL 文の割り当てを解除できます。 準備された文の割り当てを明示的に解除しない場合は、セッションが終了すると割り当てが解除されます。準備済み文の詳細については、 [PREPARE コマンド](#prepare) 」セクションに入力します。

```sql
DEALLOCATE name
DEALLOCATE ALL
```

If `DEALLOCATE name` が使用されます。 `name` は、割り当てを解除する必要がある準備済み文の名前を表します。 If `DEALLOCATE ALL` を使用する場合、すべての準備済み文の割り当てが解除されます。

### DECLARE

この `DECLARE` コマンドを使用すると、カーソルを作成できます。これは、大きなクエリから少数の行を取得するために使用できます。 カーソルが作成された後、`FETCH` を使用して行がカーソルから取得されます。

```sql
DECLARE name CURSOR FOR query
```

| パラメーター | 説明 |
| ------ | ------ |
| `name` | 作成するカーソルの名前。 |
| `query` | A `SELECT` または `VALUES` コマンドを使用して、カーソルが返す行を指定します。 |

### EXECUTE

この `EXECUTE` コマンドは、事前に準備された文を実行するために使用されます。 準備済み文はセッション中にのみ存在するので、準備済み文は `PREPARE` 現在のセッションで前に実行された文。 準備済み文の使用に関する詳細については、 [`PREPARE` command](#prepare) 」セクションに入力します。

この `PREPARE` 文を作成した文が一部のパラメーターを指定した場合は、互換性のある一連のパラメーターを `EXECUTE` 文。 これらのパラメーターが渡されない場合、エラーが発生します。

```sql
EXECUTE name [ ( parameter ) ]
```

| パラメーター | 説明 |
| ------ | ------ |
| `name` | 実行する準備済み文の名前。 |
| `parameter` | 準備済み文のパラメーターの実際の値。 これは、準備済み文の作成時に決定された、このパラメーターのデータ型と互換性のある値を生成する式である必要があります。準備済み文に複数のパラメーターがある場合は、コンマで区切ります。 |

### EXPLAIN

この `EXPLAIN` コマンドは、指定された文の実行計画を表示します。 実行計画では、文で参照されるテーブルがスキャンされる方法が示されます。  複数のテーブルが参照されている場合は、各入力テーブルから必要な行を組み合わせるために使用される結合アルゴリズムが表示されます。

```sql
EXPLAIN option statement
```

ここで、 `option` は、次のいずれかに該当します。

```sql
ANALYZE
FORMAT { TEXT | JSON }
```

| パラメーター | 説明 |
| ------ | ------ |
| `ANALYZE` | この `option` 次を含む `ANALYZE`に値を指定しない場合は、実行時間やその他の統計情報が表示されます。 |
| `FORMAT` | この `option` 次を含む `FORMAT`を指定する場合は、出力形式を指定します。 `TEXT` または `JSON`. テキスト以外の出力には、テキスト出力形式と同じ情報が含まれますが、プログラムの解析が容易です。このパラメーターのデフォルトは `TEXT` です。 |
| `statement` | 任意 `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `VALUES`, `EXECUTE`, `DECLARE`, `CREATE TABLE AS`または `CREATE MATERIALIZED VIEW AS` ステートメントに含まれ、その実行プランを表示します。 |

>[!IMPORTANT]
>
> `ANALYZE` オプションを使用すると、実際に文が実行されることに注意してください。`EXPLAIN` は、`SELECT` が返す出力を破棄しますが、文の他の副作用は通常どおり発生します。

**例**

次の例は、単一の `integer` 列と10000行：

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

この `FETCH` コマンドは、以前に作成したカーソルを使用して行を取得します。

```sql
FETCH num_of_rows [ IN | FROM ] cursor_name
```

| パラメーター | 説明 |
| ------ | ------ |
| `num_of_rows` | 取得する行の数。 |
| `cursor_name` | 情報を取得するカーソルの名前。 |

### PREPARE {#prepare}

この `PREPARE` コマンドを使用すると、準備済み文を作成できます。 準備済み文は、類似した SQL 文をテンプレート化するために使用できるサーバー側のオブジェクトです。

準備済み文は、パラメーターを受け取ることができます。パラメーターは、文の実行時に文内で置き換えられる値です。 パラメーターは、準備済み文を使用する場合、$1、$2 などを使用して、位置で参照されます。

必要に応じて、パラメータのデータ型のリストを指定できます。 パラメーターのデータ型が一覧に表示されない場合は、その型をコンテキストから推論できます。

```sql
PREPARE name [ ( data_type [, ...] ) ] AS SELECT
```

| パラメーター | 説明 |
| ------ | ------ |
| `name` | 準備済み文の名前。 |
| `data_type` | 準備済み文のパラメーターのデータ型。 パラメーターのデータ型が一覧に表示されない場合は、その型をコンテキストから推論できます。 複数のデータ型を追加する必要がある場合は、コンマ区切りのリストで追加できます。 |

### ROLLBACK

この `ROLLBACK` コマンドは、現在のトランザクションを元に戻し、トランザクションが行ったすべての更新を破棄します。

```sql
ROLLBACK
ROLLBACK WORK
```

### SELECT INTO

この `SELECT INTO` コマンドは、新しいテーブルを作成し、クエリで計算されたデータで埋めます。 通常の `SELECT` コマンドを使用します。 新しいテーブルの列には、 `SELECT` コマンドを使用します。

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

標準の SELECT クエリーパラメーターの詳細については、 [SELECT クエリセクション](#select-queries). このセクションでは、 `SELECT INTO` コマンドを使用します。

| パラメーター | 説明 |
| ------ | ------ |
| `TEMPORARY` または `TEMP` | オプションのパラメーター。 指定した場合、作成されるテーブルは一時テーブルになります。 |
| `UNLOGGED` | オプションのパラメーター。 指定した場合、として作成されたテーブルはログなしのテーブルになります。 ログが記録されていないテーブルの詳細については、 [PostgreSQL のドキュメント](https://www.postgresql.org/docs/current/sql-createtable.html). |
| `new_table` | 作成するテーブルの名前。 |

**例**

次のクエリは、新しいテーブルを作成します `films_recent` テーブルの最近のエントリのみから成る `films`:

```sql
SELECT * INTO films_recent FROM films WHERE date_prod >= '2002-01-01';
```

### SHOW

この `SHOW` コマンドは、ランタイムパラメータの現在の設定を表示します。 これらの変数は、 `SET` ステートメントを編集する `postgresql.conf` 設定ファイル ( `PGOPTIONS` 環境変数（libpq または libpq ベースのアプリケーションを使用する場合）、または Postgres サーバーの起動時にコマンドラインフラグを使用します。

```sql
SHOW name
SHOW ALL
```

| パラメーター | 説明 |
| ------ | ------ |
| `name` | 情報が必要なランタイムパラメーターの名前。 ランタイムパラメーターに指定できる値は次のとおりです。<br>`SERVER_VERSION`:このパラメータは、サーバーのバージョン番号を示します。<br>`SERVER_ENCODING`:このパラメーターは、サーバー側の文字セットエンコーディングを示します。<br>`LC_COLLATE`:このパラメータは、照合（テキストの順序付け）のためのデータベースのロケール設定を示します。<br>`LC_CTYPE`:このパラメータは、文字分類に関するデータベースのロケール設定を示します。<br>`IS_SUPERUSER`:このパラメータは、現在の役割にスーパーユーザー権限があるかどうかを示します。 |
| `ALL` | すべての設定パラメーターの値と説明を表示します。 |

**例**

次のクエリは、パラメーターの現在の設定を示します `DateStyle`.

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

この `COPY` コマンドは、 `SELECT` クエリを指定した場所に送信します。 このコマンドを正常に実行するには、ユーザーがこの場所にアクセスできる必要があります。

```sql
COPY query
    TO '%scratch_space%/folder_location'
    [  WITH FORMAT 'format_name']
```

| パラメーター | 説明 |
| ------ | ------ |
| `query` | コピーするクエリ。 |
| `format_name` | クエリをコピーする形式。 この `format_name` 次のいずれかを指定できます。 `parquet`, `csv`または `json`. デフォルトでは、値は `parquet`. |

>[!NOTE]
>
>完全な出力パスは次のようになります。 `adl://<ADLS_URI>/users/<USER_ID>/acp_foundation_queryService/folder_location/<QUERY_ID>`

### ALTER TABLE {#alter-table}

この `ALTER TABLE` コマンドを使用すると、プライマリキーまたは外部キーの制約を追加または削除したり、テーブルに列を追加したりできます。


#### 制約を追加または削除

次の SQL クエリは、テーブルに制約を追加または削除する例を示しています。

```sql
ALTER TABLE table_name ADD CONSTRAINT PRIMARY KEY ( column_name ) NAMESPACE namespace

ALTER TABLE table_name ADD CONSTRAINT FOREIGN KEY ( column_name ) REFERENCES referenced_table_name ( primary_column_name )

ALTER TABLE table_name ADD CONSTRAINT PRIMARY IDENTITY ( column_name ) NAMESPACE namespace

ALTER TABLE table_name ADD CONSTRAINT IDENTITY ( column_name ) NAMESPACE namespace

ALTER TABLE table_name DROP CONSTRAINT PRIMARY KEY ( column_name )

ALTER TABLE table_name DROP CONSTRAINT FOREIGN KEY ( column_name )

ALTER TABLE table_name DROP CONSTRAINT PRIMARY IDENTITY ( column_name )

ALTER TABLE table_name DROP CONSTRAINT IDENTITY ( column_name )
```

| パラメーター | 説明 |
| ------ | ------ |
| `table_name` | 編集するテーブルの名前。 |
| `column_name` | 制約を追加する列の名前。 |
| `referenced_table_name` | 外部キーによって参照されるテーブルの名前。 |
| `primary_column_name` | 外部キーによって参照される列の名前。 |


>[!NOTE]
>
>テーブルスキーマは一意で、複数のテーブル間で共有されないようにする必要があります。 また、名前空間は、プライマリキー、プライマリ ID、ID の制約に必須です。

#### プライマリ ID とセカンダリ ID の追加またはドロップ

この `ALTER TABLE` コマンドを使用すると、プライマリ ID テーブル列とセカンダリ ID テーブル列の両方に対する制約を SQL を介して直接追加または削除できます。

次の例では、制約を追加して、プライマリ ID とセカンダリ ID を追加します。

```sql
ALTER TABLE t1 ADD CONSTRAINT PRIMARY IDENTITY (id) NAMESPACE 'IDFA';
ALTER TABLE t1 ADD CONSTRAINT IDENTITY(id) NAMESPACE 'IDFA';
```

次の例に示すように、制約を削除して ID を削除することもできます。

```sql
ALTER TABLE t1 DROP CONSTRAINT PRIMARY IDENTITY (c1) ;
ALTER TABLE t1 DROP CONSTRAINT IDENTITY (c1) ;
```

次のドキュメントを参照してください： [アドホックデータセットで id を設定する](../data-governance/ad-hoc-schema-identities.md) を参照してください。

#### 列を追加

次の SQL クエリは、テーブルに列を追加する例を示しています。

```sql
ALTER TABLE table_name ADD COLUMN column_name data_type

ALTER TABLE table_name ADD COLUMN column_name_1 data_type1, column_name_2 data_type2 
```

##### サポートされるデータタイプ

次の表に、を使用して列を表に追加する際に使用できるデータ型を示します。 [!DNL Postgres SQL]、XDM、および [!DNL Accelerated Database Recovery] (ADR) を Azure SQL で使用します。

| --- | PSQL クライアント | XDM | ADR | 説明 |
|---|---|---|---|---|
| 1 | `bigint` | `int8` | `bigint` | -9,223,372,036,854,775,807 ～ 9,223,372,036,854,775,807 の大きな整数を 8 バイトで格納するのに使用される数値データ型です。 |
| 2 | `integer` | `int4` | `integer` | -2,147,483,648 ～ 2,147,483,647 の整数を 4 バイトで格納するのに使用される数値データ型です。 |
| 3 | `smallint` | `int2` | `smallint` | -32,768 ～ 215-1 32,767 の整数を 2 バイトで格納するために使用される数値データ型です。 |
| 4 | `tinyint` | `int1` | `tinyint` | 0 ～ 255 の整数を 1 バイトに格納するために使用される数値データ型です。 |
| 5 | `varchar(len)` | `string` | `varchar(len)` | 可変サイズの文字データ型です。 `varchar` は、列のデータエントリのサイズが大きく異なる場合に最も適しています。 |
| 6 | `double` | `float8` | `double precision` | `FLOAT8` および `FLOAT` は、次の同義語として有効です： `DOUBLE PRECISION`. `double precision` は浮動小数点データ型です。 浮動小数点値は 8 バイトで格納されます。 |
| 7 | `double precision` | `float8` | `double precision` | `FLOAT8` は `double precision`.`double precision` は浮動小数点データ型です。 浮動小数点値は 8 バイトで格納されます。 |
| 8 | `date` | `date` | `date` | この `date` データタイプは、タイムスタンプ情報のない 4 バイトで保存されるカレンダー日付値です。 有効な日付の範囲は01-01-0001 ～ 12-31-9999です。 |
| 9 | `datetime` | `datetime` | `datetime` | インスタントを時刻で保存するために使用されるデータ型で、カレンダーの日時として表されます。 `datetime` 次の修飾子が含まれます。年、月、日、時間、秒、分数 A `datetime` 宣言には、これらの時間単位のサブセットを含めることができます。この時間単位は、その順序で結合される場合も、単一の時間単位で構成される場合もあります。 |
| 10 | `char(len)` | `string` | `char(len)` | この `char(len)` キーワードは、項目が固定長の文字であることを示すために使用されます。 |

#### スキーマを追加

次の SQL クエリは、データベース/スキーマにテーブルを追加する例を示しています。

```sql
ALTER TABLE table_name ADD SCHEMA database_name.schema_name
```

>[!NOTE]
>
> ADLS テーブルとビューは、DWH データベース/スキーマに追加できません。


#### スキーマを削除

次の SQL クエリは、データベースまたはスキーマからテーブルを削除する例を示しています。

```sql
ALTER TABLE table_name REMOVE SCHEMA database_name.schema_name
```

>[!NOTE]
>
> DWH テーブルとビューは、物理的にリンクされた DWH データベース/スキーマからは削除できません。


**パラメーター**

| パラメーター | 説明 |
| ------ | ------ |
| `table_name` | 編集するテーブルの名前。 |
| `column_name` | 追加する列の名前。 |
| `data_type` | 追加する列のデータ型です。 次のようなデータタイプがサポートされています。bigint, char，文字列，日付，日時，倍精度，整数， smallint, tinyint, varchar. |

### プライマリキーを表示

この `SHOW PRIMARY KEYS` コマンドは、指定したデータベースのすべての主キー制約をリストします。

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

この `SHOW FOREIGN KEYS` コマンドは、指定されたデータベースのすべての外部キー制約をリストします。

```sql
SHOW FOREIGN KEYS
```

```console
    tableName   |     columnName      | datatype | referencedTableName | referencedColumnName | namespace 
------------------+---------------------+----------+---------------------+----------------------+-----------
 table_name_1   | column_name1        | text     | table_name_3        | column_name3         |  "ECID"
 table_name_2   | column_name2        | text     | table_name_4        | column_name4         |  "AAID"
```


### データグループを表示

この `SHOW DATAGROUPS` コマンドは、関連するすべてのデータベースのテーブルを返します。 各データベースについて、テーブルにはスキーマ、グループタイプ、子タイプ、子名、子 ID が含まれます。

```sql
SHOW DATAGROUPS
```

```console
   Database   |      Schema       | GroupType |      ChildType       |                     ChildName                       |               ChildId
  -------------+-------------------+-----------+----------------------+----------------------------------------------------+--------------------------------------
   adls_db     | adls_scheema      | ADLS      | Data Lake Table      | adls_table1                                        | 6149ff6e45cfa318a76ba6d3
   adls_db     | adls_scheema      | ADLS      | Data Warehouse Table | _table_demo1                                       | 22df56cf-0790-4034-bd54-d26d55ca6b21
   adls_db     | adls_scheema      | ADLS      | View                 | adls_view1                                         | c2e7ddac-d41c-40c5-a7dd-acd41c80c5e9
   adls_db     | adls_scheema      | ADLS      | View                 | adls_view4                                         | b280c564-df7e-405f-80c5-64df7ea05fc3
```


### テーブルのデータグループを表示

この `SHOW DATAGROUPS FOR` &#39;table_name&#39;コマンドは、パラメータを子として含むすべての関連データベースのテーブルを返します。 各データベースについて、テーブルにはスキーマ、グループタイプ、子タイプ、子名、子 ID が含まれます。

```sql
SHOW DATAGROUPS FOR 'table_name'
```

**パラメーター**

- `table_name`:関連するデータベースを検索するテーブルの名前。

```console
   Database   |      Schema       | GroupType |      ChildType       |                     ChildName                      |               ChildId
  -------------+-------------------+-----------+----------------------+----------------------------------------------------+--------------------------------------
   dwh_db_demo | schema2           | QSACCEL   | Data Warehouse Table | _table_demo2                                       | d270f704-0a65-4f0f-b3e6-cb535eb0c8ce
   dwh_db_demo | schema1           | QSACCEL   | Data Warehouse Table | _table_demo2                                       | d270f704-0a65-4f0f-b3e6-cb535eb0c8ce
   qsaccel     | profile_aggs      | QSACCEL   | Data Warehouse Table | _table_demo2                                       | d270f704-0a65-4f0f-b3e6-cb535eb0c8ce
```
