---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: SQL構文
topic: syntax
translation-type: tm+mt
source-git-commit: 3b710e7a20975880376f7e434ea4d79c01fa0ce5
workflow-type: tm+mt
source-wordcount: '1940'
ht-degree: 1%

---


# SQL構文

[!DNL Query Service] は、標準のANSI SQLを文や他の制限付きコマンドに使用する機能を提供し `SELECT` ます。 このドキュメントは、でサポートされているSQL構文を示し [!DNL Query Service]ます。

## SELECTクエリの定義

次の構文は、でサポートされる `SELECT` クエリを定義し [!DNL Query Service]ます。

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

ここ `from_item` では、次のいずれかを指定できます。

```
table_name [ * ] [ [ AS ] alias [ ( column_alias [, ...] ) ] ]
    [ LATERAL ] ( select ) [ AS ] alias [ ( column_alias [, ...] ) ]
    with_query_name [ [ AS ] alias [ ( column_alias [, ...] ) ] ]
    from_item [ NATURAL ] join_type from_item [ ON join_condition | USING ( join_column [, ...] ) ]
```

は、次のい `grouping_element` ずれかになります。

```
( )
    expression
    ( expression [, ...] )
    ROLLUP ( { expression | ( expression [, ...] ) } [, ...] )
    CUBE ( { expression | ( expression [, ...] ) } [, ...] )
    GROUPING SETS ( grouping_element [, ...] )
```

と `with_query` は次の通りです。

```
 with_query_name [ ( column_name [, ...] ) ] AS ( select | values )
 
TABLE [ ONLY ] table_name [ * ]
```

### WHERE ILIKE句

SELECTクエリのWHERE句では、大文字と小文字が区別されないので、LIKEの代わりにILIKEというキーワードを使用して照合を行うことができます。

```
    [ WHERE condition { LIKE | ILIKE | NOT LIKE | NOT ILIKE } pattern ]
```

LIKE句とILIKE句のロジックは次のとおりです。
- ```WHERE condition LIKE pattern```は、パタ ```~~``` ーンと同じです。
- ```WHERE condition NOT LIKE pattern```は、パタ ```!~~``` ーンと同じです。
- ```WHERE condition ILIKE pattern```に ```~~*``` 相当します。
- ```WHERE condition NOT ILIKE pattern```に ```!~~*``` 相当します。


#### 例

```
SELECT * FROM Customers
WHERE CustomerName ILIKE 'a%';
```

名前が「A」または「a」で始まる顧客を返します。

## 結合

結合を使用する `SELECT` クエリは、次の構文を持ちます。

```
SELECT statement
FROM statement
[JOIN | INNER JOIN | LEFT JOIN | LEFT OUTER JOIN | RIGHT JOIN | RIGHT OUTER JOIN | FULL JOIN | FULL OUTER JOIN]
ON join condition
```


## 和集合、交差、および除外

、 `UNION``INTERSECT`および `EXCEPT` 句は、2つ以上のテーブルの行を組み合わせたり除外したりできます。

```
SELECT statement 1
[UNION | UNION ALL | UNION DISTINCT | INTERSECT | EXCEPT | MINUS]
SELECT statement 2
```

## 選択としてテーブルを作成

次の構文は、でサポートされる `CREATE TABLE AS SELECT` (CTAS)クエリを定義し [!DNL Query Service]ます。

```
CREATE TABLE table_name [ WITH (schema='target_schema_title') ] AS (select_query)
```

ここ `target_schema_title` で、はXDMスキーマのタイトルです。 この句は、CTASクエリが作成した新しいデータセットに対して既存のXDMスキーマを使用する場合にのみ使用します。

と `select_query` は `SELECT` 文で、構文はこのドキュメントで上述します。


### 例

```
CREATE TABLE Chairs AS (SELECT color, count(*) AS no_of_chairs FROM Inventory i WHERE i.type=="chair" GROUP BY i.color)
CREATE TABLE Chairs WITH (schema='target schema title') AS (SELECT color, count(*) AS no_of_chairs FROM Inventory i WHERE i.type=="chair" GROUP BY i.color)
```

特定のCTASクエリの場合は、次の点に注意してください。

1. この `SELECT` ステートメントには、 `COUNT`、 `SUM`、 `MIN`などの集計関数のエイリアスが必要です。
2. この `SELECT` ステートメントには、括弧()を含めることも含めることも含めることもできます。

## 挿入する

次の構文は、でサポートされる `INSERT INTO` クエリを定義し [!DNL Query Service]ます。

```
INSERT INTO table_name select_query
```

は `select_query` ステートメント `SELECT` で、構文はこのドキュメントで上述します。

### 例

```
INSERT INTO Customers SELECT SupplierName, City, Country FROM OnlineCustomers;
```

特定のINSERT INTOクエリの場合は、次の点に注意してください。

1. 文は括弧()で囲まないでください。 `SELECT`
2. 文の結果のスキーマは、文で定義された表の結果に従う `SELECT` 必要があり `INSERT INTO` ます。

### ドロップテーブル

テーブルがEXTERNALテーブルでない場合は、テーブルを削除し、テーブルに関連付けられたディレクトリをファイルシステムから削除します。 ドロップするテーブルが存在しない場合は、例外が発生します。

```
DROP [TEMP] TABLE [IF EXISTS] [db_name.]table_name
```

### パラメーター

- `IF EXISTS`: テーブルが存在しない場合は、何も起こりません
- `TEMP`: 一時テーブル

## 表示の作成

次の構文は、でサポートされる `CREATE VIEW` クエリを定義し [!DNL Query Service]ます。

```
CREATE [ OR REPLACE ] VIEW view_name AS select_query
```

ここ `view_name` で、は作成する表示の名前で `select_query` 、は `SELECT` ステートメントです。構文はこのドキュメントで上述します。

例：

```
CREATE VIEW V1 AS SELECT color, type FROM Inventory
CREATE OR REPLACE VIEW V1 AS SELECT model, version FROM Inventory
```

### ドロップ表示

次の構文は、でサポートされる `DROP VIEW` クエリを定義し [!DNL Query Service]ます。

```
DROP VIEW [IF EXISTS] view_name
```

ここ `view_name` で、は削除する表示の名前です。

例：

```
DROP VIEW v1
DROP VIEW IF EXISTS v1
```

## [!DNL Spark] SQLコマンド

### SET

プロパティを設定するか、既存のプロパティの値を返すか、既存のプロパティをリストします。 既存のプロパティキーに値が指定された場合、古い値は上書きされます。

```
SET property_key [ To | =] property_value
```

任意の設定の値を返すには、を使用し `SHOW [setting name]`ます。

## PostgreSQLコマンド

### BEGIN

このコマンドは解析され、完了したコマンドがクライアントに返されます。 This is the same as the `START TRANSACTION` command.

```
BEGIN [ TRANSACTION ]
```

#### パラメーター

- `TRANSACTION`: オプションのキーワード。 リッスンします。この操作は実行されません。

### CLOSE

`CLOSE` オープンカーソルに関連付けられたリソースを解放します。 カーソルを閉じた後は、そのカーソルに対して以降の操作は許可されません。 不要になった時点でカーソルを閉じる必要があります。

```
CLOSE { name }
```

#### パラメーター

- `name`: 閉じる開いたカーソルの名前。

### COMMIT

コミットトランザクション明細書への応答 [!DNL Query Service] としてのアクションは実行されません。

```
COMMIT [ WORK | TRANSACTION ]
```

#### パラメーター

- `WORK`
- `TRANSACTION`: オプションのキーワード。 効果はありません。

### 割り当て解除

事前に準備さ `DEALLOCATE` れたSQL文の割り当てを解除するために使用します。 プリペアド文の割り当てを明示的に解除しない場合は、セッションが終了すると割り当てが解除されます。

```
DEALLOCATE [ PREPARE ] { name | ALL }
```

#### パラメーター

- `Prepare`: このキーワードは無視されます。
- `name`: 割り当てを解除する準備済みステートメントの名前。
- `ALL`: すべての準備済み文の割り当てを解除します。

### DECLARE

`DECLARE` カーソルを作成できます。カーソルは、大きいクエリから少数の行を一度に取得するのに使用できます。 カーソルが作成された後、を使用して行がカーソルからフェッチされ `FETCH`ます。

```
DECLARE name CURSOR [ WITH  HOLD ] FOR query
```

#### パラメーター

- `name`: 作成するカーソルの名前。
- `WITH HOLD`: カーソルを作成したトランザクションが正常にコミットされた後も、カーソルを引き続き使用できるように指定します。
- `query`: カーソルが返す行 `SELECT` を提供する `VALUES` またはコマンド。

### EXECUTE

`EXECUTE` は、事前に準備された文を実行するために使用されます。 準備された文はセッション中にのみ存在するので、準備された文は現在のセッションの前に実行された `PREPARE` 文によって作成される必要があります。

ステートメントを作成した `PREPARE` ステートメントでパラメーターが指定されている場合は、互換性のある一連のパラメーターをステートメントに渡す必要があります。そうでない場合は、 `EXECUTE` エラーが発生します。 プリペアドステートメント（関数とは異なり）は、パラメーターの型や数に基づいてオーバーロードされません。 プリペアド・ステートメントの名前は、データベース・セッション内で一意である必要があります。

```
EXECUTE name [ ( parameter [, ...] ) ]
```

#### パラメーター

- `name`: 実行する準備済み文の名前。
- `parameter`: 準備されたステートメントに対するパラメーターの実際の値。 この式ーは、準備文の作成時に決定される、このパラメーターのデータ型と互換性のある値を生成するパラメーターである必要があります。

### 説明

このコマンドは、指定された文に対してPostgreSQLプランナが生成する実行計画を表示します。 実行計画では、ステートメントで参照されるテーブルが平次スキャン、インデックス・スキャンなどでどのようにスキャンされるかが示され、複数のテーブルが参照される場合、各入力テーブルから必要な行を集めるために使用される結合アルゴリズムが示されます。

表示の最も重要な部分は、ステートメントの実行コストの見積もりです。これは、プランナがステートメントの実行に要する時間を推測するものです（任意であるが、通常はディスクページの取り込みを意味するコスト単位で測定されます）。 実際には、2つの数値が表示されます。 最初の行が返される前の開始上限コストと、すべての行が返される合計コストです。 ほとんどのクエリでは、総コストは重要ですが、EXISTSのサブクエリなどのコンテキストでは、プランナは最も小さい総コストではなく最も小さい開始アップコストを選択します（実行者が1行を取得した後で停止するので）。 また、 `LIMIT` 句を使用して返す行数を制限する場合、プランナは、最も費用の安い計画を見積もるために、エンドポイントのコストの間に適切な補間を行います。

この `ANALYZE` オプションを使用すると、計画されただけでなく、文が実行されます。 次に、各計画ノード内で費やされた合計経過時間（ミリ秒）と、その計画ノードが返した合計行数を含む、実際の実行時の統計が表示に追加されます。 これは、プランナーの予測が現実に近いかどうかを調べるのに役立ちます。

```
EXPLAIN [ ( option [, ...] ) ] statement
EXPLAIN [ ANALYZE ] statement

where option can be one of:
    ANALYZE [ boolean ]
    TYPE VALIDATE
    FORMAT { TEXT | JSON }
```

#### パラメーター

- `ANALYZE`: コマンドを実行し、実際の実行時間やその他の統計を表示します。 このパラメーターのデフォルト値は `FALSE`です。
- `FORMAT`: 出力形式を指定します。TEXT、XML、JSON、YAMLのいずれかを指定できます。 テキスト以外の出力には、テキスト出力形式と同じ情報が含まれますが、プログラムは簡単に解析できます。 このパラメーターのデフォルト値は `TEXT`です。
- `statement`: `SELECT`,,,,,,,,, `INSERT`, `UPDATE`, `DELETE`, `VALUES`, `EXECUTE`, `DECLARE`, `CREATE TABLE AS``CREATE MATERIALIZED VIEW AS` , , , ，実行計画を表示したい。

>[!IMPORTANT]
>
>このオプションを使用すると、実際に文が実行されることに注意して `ANALYZE` ください。 返される出力は `EXPLAIN``SELECT` 破棄されますが、文の他の副作用は通常どおり発生します。

#### 例

単一の列と10000行の表に、単純なクエリの計画を表示する手順は、次のとおりで `integer` す。

```
EXPLAIN SELECT * FROM foo;

                       QUERY PLAN
---------------------------------------------------------
 Seq Scan on foo  (cost=0.00..155.00 rows=10000 width=4)
(1 row)
```

### FETCH

`FETCH` 以前に作成したカーソルを使用して行を取得します。

カーソルには、が使用する位置が関連付けられてい `FETCH`ます。 カーソルの位置は、クエリ結果の最初の行の前、結果の特定の行の前、または結果の最後の行の後に設定できます。 カーソルを作成すると、最初の行の前にカーソルが配置されます。 一部のローをフェッチした後、カーソルは最後に取得したローに配置されます。 使用可能な行の末尾 `FETCH` から外れた場合は、カーソルは最後の行の後ろに配置されます。 そのような行がない場合は、空の結果が返され、必要に応じてカーソルは最初の行の前または最後の行の後に配置されます。

```
FETCH num_of_rows [ IN | FROM ] cursor_name
```

#### パラメーター

- `num_of_rows`: 取得する行の位置または数を指定する、符号付きの整数定数。
- `cursor_name`: 開いたカーソルの名前。

### PREPARE

`PREPARE` プリペアド・ステートメントを作成します。 プリペアドステートメントは、パフォーマンスの最適化に使用できるサーバー側のオブジェクトです。 文を実行すると、指定した文が解析、分析、書き換えられます。 `PREPARE` その後 `EXECUTE` コマンドが発行されると、準備された文が計画され、実行される。 この分業によって、繰り返しの解析分析作業が回避され、実行計画は指定された特定のパラメータ値に依存することができます。

準備された文は、パラメータを受け取ることができます。パラメータは、文の実行時に代入される値です。 準備されたステートメントを作成する場合は、$1、$2などを使用して、位置別にパラメーターを参照します。 パラメーターデータ型の対応するリストを任意で指定できます。 パラメーターのデータ型が指定されていない場合、または不明として宣言されている場合、可能であれば、パラメーターが最初に参照されるコンテキストから型が推定されます。 文を実行する場合は、文でこれらのパラメーターの実際の値を指定し `EXECUTE` ます。

準備された文は、現在のデータベース・セッションの間のみ有効です。 セッションが終了すると、準備された文は忘れられるので、再び使用する前に再作成する必要があります。 つまり、1つのプリペアドステートメントを複数の同時データベースクライアントで使用することはできません。 ただし、各クライアントは、使用する独自のプリペアドステートメントを作成できます。 準備された文は、 `DEALLOCATE` コマンドを使用して手動でクリーンアップできます。

準備文は、1つのセッションを使用して多数の類似文を実行する場合、パフォーマンスが最も高くなる可能性があります。 パフォーマンスの違いは、クエリが多数のテーブルの結合を含む場合や、複数のルールの適用を必要とする場合など、文が計画や書き換えを複雑に行う場合に特に重要です。 計画と書き換えが比較的簡単で、実行に費やすコストが比較的高い文の場合、準備された文のパフォーマンス上の利点はあまり目立ちません。

```
PREPARE name [ ( data_type [, ...] ) ] AS SELECT
```

#### パラメーター

- `name`: このプリペアドステートメントに与えられた任意の名前。 この変数は、単一のセッション内で一意である必要があり、その後、事前に準備された文を実行または割り当て解除するために使用されます。
- `data-type`: プリペアドステートメントのパラメーターのデータ型です。 特定のパラメーターのデータ型が未指定の場合、または不明として指定された場合、そのパラメーターが最初に参照されるコンテキストから推測されます。 プリペアドステートメント自体のパラメーターを参照するには、$1、$2などを使用します。


### ROLLBACK

`ROLLBACK` 現在のトランザクションをロールバックし、トランザクションによって行われたすべての更新を破棄します。

```
ROLLBACK [ WORK ]
```

#### パラメーター

- `WORK`

### 選択して

`SELECT INTO` 新しいテーブルを作成し、クエリによって計算されたデータでそれを入力します。 通常の場合と同様に、データはクライアントに返されません `SELECT`。 新しいテーブルの列には、の出力列に関連付けられた名前とデータ型が表示され `SELECT`ます。

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

- `TEMPORARAY` または `TEMP`: 指定した場合、テーブルは一時テーブルとして作成されます。
- `UNLOGGED:` 指定した場合、ログなしのテーブルとしてテーブルが作成されます。
- `new_table` 作成するテーブルの名前(オプションでスキーマ修飾)。

#### 例

テーブルの最新のエントリ `films_recent` のみから構成される新しいテーブルを作成し `films`ます。

```
SELECT * INTO films_recent FROM films WHERE date_prod >= '2002-01-01';
```

### SHOW

`SHOW` 実行時パラメーターの現在の設定を表示します。 これらの変数は、 `SET` 文を使用して設定するか、postgresql.conf設定ファイルを編集するか、 `PGOPTIONS` 環境変数（libpqまたはlibpqベースのアプリケーションを使用する場合）を使用するか、postgresサーバを起動する際にコマンドラインフラグを使用して設定できます。

```
SHOW name
```

#### パラメーター

-  `name`。
   - `SERVER_VERSION`: サーバーのバージョン番号を表示します。
   - `SERVER_ENCODING`: サーバ側の文字セットエンコーディングを表示します。 エンコーディングはデータベース作成時に決定されるので、現在、このパラメータは表示できますが設定することはできません。
   - `LC_COLLATE`: 照合（テキスト順序）に対するデータベースのロケール設定を表示します。 現在、このパラメータは、データベース作成時に設定が決定されるので、表示することはできますが、設定することはできません。
   - `LC_CTYPE`: 文字分類に対するデータベースのロケール設定を表示します。 現在、このパラメータは、データベース作成時に設定が決定されるので、表示することはできますが、設定することはできません。
      `IS_SUPERUSER`: 現在の役割にスーパーユーザ権限がある場合はtrue。
- `ALL`: すべての設定パラメーターの値と説明を表示します。

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
