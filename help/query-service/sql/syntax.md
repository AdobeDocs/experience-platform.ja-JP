---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: SQL構文
topic: syntax
translation-type: tm+mt
source-git-commit: 45da024d45b5eebdfc393ee14890e24aed6021ce

---


# SQL構文

クエリサービスは、標準のANSI SQLを文やその他の制限付きコマンドに `SELECT` 使用する機能を提供します。 このドキュメントは、クエリサービスでサポートされるSQL構文を示します。

## SELECTオプションのクエリ

次の構文は、クエリサービスでサポートさ `SELECT` れるクエリを定義します。

```
[ WITH with_query [, ...] ]
SELECT [ ALL | DISTINCT [( expression [, ...] ) ] ]
    [ * | expression [ [ AS ] output_name ] [, ...] ]
    [ FROM from_item [, ...] ]
    [ WHERE condition ]
    [ GROUP BY grouping_element [, ...] ]
    [ HAVING condition [, ...] ]
    [ WINDOW window_name AS ( window_definition ) [, ...] ]
    [ { UNION | INTERSECT | EXCEPT | MINUS } [ ALL | DISTINCT ] select ]
    [ ORDER BY expression [ ASC | DESC | USING operator ] [ NULLS { FIRST | LAST } ] [, ...] ]
    [ LIMIT { count | ALL } ]
    [ OFFSET start ]
```

次のい `from_item` ずれかを指定できます。

```
table_name [ * ] [ [ AS ] alias [ ( column_alias [, ...] ) ] ]
    [ LATERAL ] ( select ) [ AS ] alias [ ( column_alias [, ...] ) ]
    with_query_name [ [ AS ] alias [ ( column_alias [, ...] ) ] ]
    from_item [ NATURAL ] join_type from_item [ ON join_condition | USING ( join_column [, ...] ) ]
```

には、次の `grouping_element` いずれかを指定できます。

```
( )
    expression
    ( expression [, ...] )
    ROLLUP ( { expression | ( expression [, ...] ) } [, ...] )
    CUBE ( { expression | ( expression [, ...] ) } [, ...] )
    GROUPING SETS ( grouping_element [, ...] )
```

とは、次のよ `with_query` うになります。

```
 with_query_name [ ( column_name [, ...] ) ] AS ( select | values )
 
TABLE [ ONLY ] table_name [ * ]
```

### WHERE ILIKE句

SELECT句のWHERE句では、大文字と小文字を区別せずに、LIKEの代わりにILIKEというキーワードを使用して一致クエリを作成できます。

```
    [ WHERE condition { LIKE | ILIKE | NOT LIKE | NOT ILIKE } pattern ]
```

LIKE句とILIKE句のロジックは次のとおりです。
- ```WHERE condition LIKE pattern```の値は、 ```~~``` パターンと同じです。
- ```WHERE condition NOT LIKE pattern```の値は、 ```!~~``` パターンと同じです。
- ```WHERE condition ILIKE pattern```、パタ ```~~*``` ーンと同等
- ```WHERE condition NOT ILIKE pattern```、パタ ```!~~*``` ーンと同等


#### 例

```
SELECT * FROM Customers
WHERE CustomerName ILIKE 'a%';
```

名前が「A」または「a」で始まる顧客を返します。

## 結合

結合を使 `SELECT` 用するクエリの構文は次のとおりです。

```
SELECT statement
FROM statement
[JOIN | INNER JOIN | LEFT JOIN | LEFT OUTER JOIN | RIGHT JOIN | RIGHT OUTER JOIN | FULL JOIN | FULL OUTER JOIN]
ON join condition
```


## 和集合、交差、および例外

複数のテ `UNION`ーブル `INTERSECT`の「いいね！」行を `EXCEPT` 結合または除外する、句、句、およびがサポートされています。

```
SELECT statement 1
[UNION | UNION ALL | UNION DISTINCT | INTERSECT | EXCEPT | MINUS]
SELECT statement 2
```

## 選択としてテーブルを作成

次の構文は、クエリサービスでサ `CREATE TABLE AS SELECT` ポートされる(CTAS)クエリを定義します。

```
CREATE TABLE table_name [ WITH (schema='target_schema_title') ] AS (select_query)
```

はXDM `target_schema_title` スキーマのタイトル この句は、CTASクエリで作成された新しいデータセットに対して既存のXDMスキーマを使用する場合にのみ使用します。

とはス `select_query` テートメ `SELECT` ントで、この構文はこのドキュメントで上述します。


### 例

```
CREATE TABLE Chairs AS (SELECT color, count(*) AS no_of_chairs FROM Inventory i WHERE i.type=="chair" GROUP BY i.color)
CREATE TABLE Chairs WITH (schema='target schema title') AS (SELECT color, count(*) AS no_of_chairs FROM Inventory i WHERE i.type=="chair" GROUP BY i.color)
```

特定のCTASクエリ:

1. ステー `SELECT` トメントには、、、などの集計関数のエイリ `COUNT`アスが `SUM`含まれ `MIN`ている必要があります。
2. 文は、 `SELECT` 括弧()を含めるか、含めないで指定できます。

## 挿入する

次の構文は、クエリサービスでサポートさ `INSERT INTO` れるクエリを定義します。

```
INSERT INTO table_name select_query
```

はス `select_query` テートメ `SELECT` ントで、この構文はこのドキュメントで上述します。

### 例

```
INSERT INTO Customers SELECT SupplierName, City, Country FROM OnlineCustomers;
```

指定したINSERT INTOクエリ:

1. 文は括 `SELECT` 弧()で囲まないでください。
2. 文の結果のスキーマは、文で定 `SELECT` 義された表の結果に準拠している必要があ `INSERT INTO` ります。

### テーブルをドロップ

テーブルがEXTERNALテーブルでない場合は、テーブルを削除し、テーブルに関連付けられたディレクトリをファイルシステムから削除します。 削除するテーブルが存在しない場合は、例外が発生します。

```
DROP [TEMP] TABLE [IF EXISTS] [db_name.]table_name
```

### パラメーター

- `IF EXISTS`:テーブルが存在しない場合は、何も起こりません
- `TEMP`:一時テーブル

## CREATE表示

次の構文は、クエリサービスでサポートさ `CREATE VIEW` れるクエリを定義します。

```
CREATE [ OR REPLACE ] VIEW view_name AS select_query
```

ここ `view_name` では、作成する表示の名前、 `select_query` はステートメ `SELECT` ントです。この構文は、このドキュメントで前述したとおりです。

例：

```
CREATE VIEW V1 AS SELECT color, type FROM Inventory
CREATE OR REPLACE VIEW V1 AS SELECT model, version FROM Inventory
```

### ドロップ表示

次の構文は、クエリサービスでサポートさ `DROP VIEW` れるクエリを定義します。

```
DROP VIEW [IF EXISTS] view_name
```

ここで、 `view_name` は削除する表示の名前です。

例：

```
DROP VIEW v1
DROP VIEW IF EXISTS v1
```

## Spark SQLコマンド

### SET

プロパティを設定するか、既存のプロパティの値を返すか、既存のプロパティをリストします。 既存のプロパティキーに値が指定された場合、古い値が上書きされます。

```
SET property_key [ To | =] property_value
```

任意の設定の値を返すには、を使用しま `SHOW [setting name]`す。

## PostgreSQLコマンド

### BEGIN

このコマンドは解析され、完了したコマンドがクライアントに送り返されます。 This is the same as the `START TRANSACTION` command.

```
BEGIN [ TRANSACTION ]
```

#### パラメーター

- `TRANSACTION`:キーワード（オプション）。 リッスンします。この操作は行われません。

### CLOSE

`CLOSE` オープンカーソルに関連付けられたリソースを解放します。 カーソルを閉じた後は、その後の操作は許可されません。 不要になったカーソルは閉じる必要があります。

```
CLOSE { name }
```

#### パラメーター

- `name`:閉じる開いたカーソルの名前。

### コミット

コミットトランザクションのクエリに対する応答として、トランザクションサービスでは何も行われません。

```
COMMIT [ WORK | TRANSACTION ]
```

#### パラメーター

- `WORK`
- `TRANSACTION`:キーワード（オプション）。 何の効果もない。

### 割り当て解除

事前に準 `DEALLOCATE` 備されたSQL文の割り当てを解除する場合に使用します。 準備された文の割り当てを明示的に解除しない場合は、セッションが終了すると割り当てが解除されます。

```
DEALLOCATE [ PREPARE ] { name | ALL }
```

#### パラメーター

- `Prepare`:このキーワードは無視されます。
- `name`:割り当てを解除する準備済みステートメントの名前。
- `ALL`:すべての準備文の割り当てを解除します。

### DECLARE

`DECLARE` ユーザーがカーソルを作成できます。カーソルは、大きなカーソルの中から少ない数の行を一度に取得するのに使用できます。クエリ カーソルが作成された後、を使用して行がカーソルからフェッチされま `FETCH`す。

```
DECLARE name CURSOR [ WITH  HOLD ] FOR query
```

#### パラメーター

- `name`:作成するカーソルの名前。
- `WITH HOLD`:カーソルを作成したトランザクションが正常にコミットされた後も、カーソルを引き続き使用できるように指定します。
- `query`:カーソ `SELECT` ルが返 `VALUES` す行を指定するまたはコマンド。

### EXECUTE

`EXECUTE` は、事前に準備された文を実行するために使用されます。 準備された文はセッション中にのみ存在するので、準備された文は、現在のセッションの前に実行された文によ `PREPARE` って作成されたものでなければなりません。

文を作成し `PREPARE` た文が一部のパラメータを指定した場合は、互換性のある一連のパラメータを文に渡す必要がありま `EXECUTE` す。そうしないと、エラーが発生します。 プリペアドステートメント（関数とは異なり）は、パラメータのタイプや数に基づいてオーバーロードされません。 プリペアドステートメントの名前は、データベースセッション内で一意である必要があります。

```
EXECUTE name [ ( parameter [, ...] ) ]
```

#### パラメーター

- `name`:実行する準備済み文の名前。
- `parameter`:準備されたステートメントのパラメーターの実際の値。 これは、準備された式の作成時に決定される、このパラメータのデータ型と互換性のある値を生成するパラメータである必要があります。

### 説明

このコマンドは、指定された文に対してPostgreSQLプランナが生成する実行計画を表示します。 実行計画には、文によって参照される表がどのようにスキャンされるかが示されます。 通常のシーケンシャル・スキャン、インデックス・スキャンなどによる また、複数のテーブルが参照されている場合、どの結合アルゴリズムを使用して各入力テーブルの必要な行が統合されます。

表示の最も重要な部分は、ステートメントの実行推定コストです。これは、（任意で、通常はディスクページのフェッチを意味するコスト単位で測定される）ステートメントの実行にかかる時間をプランナが推測するものです。 実際には、2つの数値が表示されます。最初の行を返す前の開始上のコストと、すべての行を返す総コスト。 ほとんどのクエリでは、総コストは重要ですが、EXISTSのサブクエリなどのコンテキストでは、プランナは最も小さい総コストではなく最も小さい開始コストを選択します（実行者が1行を取得した後に停止するので）。 また、句で返す行数を制限すると、プランナはエンドポイントのコストを適切に補間して、実際に最も安い計画を見積もります。 `LIMIT`

このオ `ANALYZE` プションを使用すると、計画済みだけでなく、文が実行されます。 次に、各計画ノード内で費やされた合計経過時間（ミリ秒）と、返された合計行数を含む、実際の実行時間統計が表示に追加されます。 これは、プランナーの予測が現実に近いかどうかを調べるのに役立ちます。

```
EXPLAIN [ ( option [, ...] ) ] statement
EXPLAIN [ ANALYZE ] statement

where option can be one of:
    ANALYZE [ boolean ]
    TYPE VALIDATE
    FORMAT { TEXT | JSON }
```

#### パラメーター

- `ANALYZE`:コマンドを実行し、実行時間やその他の統計を表示します。 このパラメーターのデフォルトは `FALSE`です。
- `FORMAT`:出力形式を指定します。TEXT、XML、JSONまたはYAMLを指定できます。 テキスト以外の出力には、テキスト出力形式と同じ情報が含まれますが、プログラムの解析が容易です。 このパラメーターのデフォルトは `TEXT`です。
- `statement`:どれ `SELECT`も，,, `INSERT``UPDATE``DELETE``VALUES``EXECUTE``DECLARE``CREATE TABLE AS``CREATE MATERIALIZED VIEW AS` ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,

> [!IMPORTANT] このオプションを使用すると、実際にステートメントが実行さ `ANALYZE` れることに注意してください。 が返す `EXPLAIN` 出力は破棄されますが、 `SELECT` 文の他の副作用は通常どおり発生します。

#### 例

単一の列と1,000行の表に、単純なクエリの計画を表 `integer` 示する手順は、次のとおりです。

```
EXPLAIN SELECT * FROM foo;

                       QUERY PLAN
---------------------------------------------------------
 Seq Scan on foo  (cost=0.00..155.00 rows=10000 width=4)
(1 row)
```

### FETCH

`FETCH` 以前に作成したカーソルを使用して行を取得します。

カーソルには、が使用する位置が関連付けられていま `FETCH`す。 カーソル位置は、クエリ結果の最初の行の前、結果の特定の行の前、または結果の最後の行の後に設定できます。 カーソルを作成すると、最初の行の前に配置されます。 一部のローをフェッチした後、カーソルは最も最近取得したローに配置されます。 を使 `FETCH` 用可能な行の末尾から離れた場合、カーソルは最後の行の後に残ります。 該当する行がない場合は、空の結果が返され、必要に応じて、カーソルは最初の行の前または最後の行の後に配置されます。

```
FETCH num_of_rows [ IN | FROM ] cursor_name
```

#### パラメーター

- `num_of_rows`:取得する行の位置または数を指定する、符号付きの整数定数。
- `cursor_name`:開いたカーソルの名前。

### PREPARE

`PREPARE` プリペアドステートメントを作成します。 プリペアドステートメントは、パフォーマンスの最適化に使用できるサーバー側のオブジェクトです。 文が実行さ `PREPARE` れると、指定した文が解析、分析、書き換えられます。 その後、コ `EXECUTE` マンドが発行されると、準備された文が計画され、実行されます。 この分業によって、繰り返しの解析分析作業を避けると共に、実行計画を指定された特定のパラメータ値に依存させることができます。

準備されたステートメントは、パラメーターを受け取ることができます。パラメーターは、ステートメントの実行時にステートメント内で置き換えられる値です。 プリペアドステートメントを作成する場合は、$1、$2などを使用して、位置別にパラメーターを参照します。 対応するリストのパラメータデータ型を任意で指定できます。 パラメーターのデータ型が指定されていない場合、または不明として宣言されている場合、可能であれば、パラメーターが最初に参照されるコンテキストから型が推論されます。 ステートメントを実行する場合は、ステートメント内のこれらのパラメーターの実際の値を指定 `EXECUTE` します。

準備された文は、現在のデータベース・セッションの間のみ有効です。 セッションが終わると、準備された文は忘れられるので、再び使用する前に再作成する必要があります。 つまり、複数の同時データベース・クライアントが1つのプリペアド・ステートメントを使用することはできません。 ただし、各クライアントは独自のプリペアドステートメントを作成して使用できます。 準備された文は、コマンドを使用して手動でクリーンアップで `DEALLOCATE` きます。

プリペアドステートメントは、1つのセッションを使用して多数の類似のステートメントを実行する場合、パフォーマンスが最も高くなる可能性があります。 パフォーマンスの違いは、クエリが多数のテーブルの結合を含む場合や、複数のルールの適用を必要とする場合など、文が計画や書き換えを複雑に行う場合に特に重要です。 計画と書き換えが比較的簡単で、実行に費やすコストが比較的高い文の場合、準備された文のパフォーマンス上の利点はあまり目立ちません。

```
PREPARE name [ ( data_type [, ...] ) ] AS SELECT
```

#### パラメーター

- `name`:このプリペアドステートメントに与えられる任意の名前。 この変数は、単一のセッション内で一意である必要があり、その後、事前に準備された文を実行または割り当て解除するために使用されます。
- `data-type`:準備されたステートメントのパラメーターのデータ型です。 特定のパラメーターのデータ型が指定されていない場合、または不明なデータ型が指定されている場合は、そのパラメーターが最初に参照されるコンテキストから推定されます。 プリペアドステートメント自体のパラメーターを参照するには、$1、$2などを使用します。


### ROLLBACK

`ROLLBACK` 現在のトランザクションをロールバックし、トランザクションによって行われたすべての更新を破棄します。

```
ROLLBACK [ WORK ]
```

#### パラメーター

- `WORK`

### 選択して

`SELECT INTO` 新しいテーブルを作成し、クエリで計算されたデータで埋めます。 通常の場合とは異なり、データはクライアントに返されません `SELECT`。 新しいテーブルの列には、の出力列に関連付けられた名前とデータタイプが含まれま `SELECT`す。

```
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

- `TEMPORARAY` または `TEMP`:指定した場合、テーブルは一時テーブルとして作成されます。
- `UNLOGGED:` 指定した場合、テーブルはログなしのテーブルとして作成されます。
- `new_table` 作成するテーブルの名前(スキーマ修飾名も可)。

#### 例

テーブルの最近のエント `films_recent` リのみから構成される新しいテーブルを作成しま `films`す。

```
SELECT * INTO films_recent FROM films WHERE date_prod >= '2002-01-01';
```

### 表示

`SHOW` 実行時パラメータの現在の設定を表示します。 これらの変数は、文を使用して、 `SET` postgresql.conf設定ファイルを編集するか、環境変数（libpqまたはlibpqベースのアプリケーションを使用する場合）を使用して、または `PGOPTIONS` postgresサーバを起動する際にコマンドラインフラグを使用して設定できます。

```
SHOW name
```

#### パラメーター

-  `name`。
   - `SERVER_VERSION`:サーバーのバージョン番号を表示します。
   - `SERVER_ENCODING`:サーバ側の文字セットエンコーディングを表示します。 エンコーディングはデータベース作成時に決定されるので、現時点では、このパラメーターを表示することはできますが、設定することはできません。
   - `LC_COLLATE`:照合（テキストの順序付け）のためのデータベースのロケール設定を表示します。 現在、このパラメータは表示できますが、設定はできません。これは、設定がデータベースの作成時に決定されるからです。
   - `LC_CTYPE`:文字分類に関するデータベースのロケール設定を表示します。 現在、このパラメータは表示できますが、設定はできません。これは、設定がデータベースの作成時に決定されるからです。
      `IS_SUPERUSER`:現在の役割にスーパーユーザ権限がある場合はtrue。
- `ALL`:すべての設定パラメーターの値と説明を表示します。

#### 例

パラメータの現在の設定を表示 `DateStyle`

```
SHOW DateStyle;
 DateStyle
-----------
 ISO, MDY
(1 row)
```

### 開始取引

このコマンドは解析され、完了したコマンドがクライアントに返されます。 This is the same as the `BEGIN` command.

```
START TRANSACTION [ transaction_mode [, ...] ]

where transaction_mode is one of:

    ISOLATION LEVEL { SERIALIZABLE | REPEATABLE READ | READ COMMITTED | READ UNCOMMITTED }
    READ WRITE | READ ONLY
```
