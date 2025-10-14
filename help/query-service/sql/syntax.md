---
keywords: Experience Platform；ホーム；人気のトピック；クエリサービス；Query Service;SQL 構文；SQL;CTAS;CTAS;Create table as select
solution: Experience Platform
title: クエリサービスの SQL 構文
description: このドキュメントでは、Adobe Experience Platform クエリサービスでサポートされている SQL 構文の詳細と説明を説明します。
exl-id: 2bd4cc20-e663-4aaa-8862-a51fde1596cc
source-git-commit: cd4734b2d837bc04e1de015771a74a48ff37173f
workflow-type: tm+mt
source-wordcount: '4686'
ht-degree: 4%

---

# クエリサービスの SQL 構文

Adobe Experience Platform クエリサービスの `SELECT` ステートメントやその他の制限付きコマンドには、標準の ANSI SQL を使用できます。 このドキュメントでは、[!DNL Query Service] でサポートされる SQL 構文について説明します。

## クエリを選択 {#select-queries}

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

以下のタブ セクションには、FROM、GROUP、および WITH キーワードで使用可能なオプションが表示されます。

>[!BEGINTABS]

>[!TAB `from_item`]

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

>[!TAB `grouping_element`]

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

>[!TAB `with_query`]

```sql
 with_query_name [ ( column_name [, ...] ) ] AS ( select | values )
```

>[!ENDTABS]

次の項では、前述のフォーマットに従う場合に、問合せで使用できる追加条項の詳細を示します。

### SNAPSHOT 句

この句は、スナップショット ID に基づいて、テーブルのデータを増分的に読み取るために使用できます。 スナップショット ID は、データが書き込まれるたびにデータレイクテーブルに適用される、Long タイプの番号で表されるチェックポイントマーカーです。 `SNAPSHOT` 句は、次に使用するテーブル関係にアタッチします。

```sql
    [ SNAPSHOT { SINCE start_snapshot_id | AS OF end_snapshot_id | BETWEEN start_snapshot_id AND end_snapshot_id } ]
```

#### 例

```sql
SELECT * FROM table_to_be_queried SNAPSHOT SINCE start_snapshot_id;

SELECT * FROM table_to_be_queried SNAPSHOT AS OF end_snapshot_id;

SELECT * FROM table_to_be_queried SNAPSHOT BETWEEN start_snapshot_id AND end_snapshot_id;

SELECT * FROM table_to_be_queried SNAPSHOT BETWEEN 'HEAD' AND start_snapshot_id;

SELECT * FROM table_to_be_queried SNAPSHOT BETWEEN end_snapshot_id AND 'TAIL';

SELECT * FROM (SELECT id FROM table_to_be_queried SNAPSHOT BETWEEN start_snapshot_id AND end_snapshot_id) C;

(SELECT * FROM table_to_be_queried SNAPSHOT SINCE start_snapshot_id) a
  INNER JOIN 
(SELECT * from table_to_be_joined SNAPSHOT AS OF your_chosen_snapshot_id) b 
  ON a.id = b.id;
```

>[!NOTE]
>
>`SNAPSHOT` 句で `HEAD` または `TAIL` を使用する場合は、それらを一重引用符で囲む必要があります（「HEAD」、「TAIL」など）。 引用符なしで使用すると、構文エラーが発生します。

次の表に、SNAPSHOT 句内の各構文オプションの意味を示します。

| 構文 | 意味 |
|-------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| `SINCE start_snapshot_id` | 指定されたスナップショット ID からデータを読み取ります（排他的）。 |
| `AS OF end_snapshot_id` | 指定されたスナップショット ID （両端を含む）のデータをそのまま読み取ります。 |
| `BETWEEN start_snapshot_id AND end_snapshot_id` | 指定された開始 ID と終了 ID の間でデータを読み取ります。 `start_snapshot_id` を除き、`end_snapshot_id` を含みます。 |
| `BETWEEN HEAD AND start_snapshot_id` | 最初（最初のスナップショットの前）から、指定された開始スナップショット ID （この値を含む）までデータを読み取ります。 これは、`start_snapshot_id` の行のみを返すことに注意してください。 |
| `BETWEEN end_snapshot_id AND TAIL` | 指定された `end_snapshot_id` の直後からデータセットの最後（スナップショット ID を除く）までデータを読み取ります。 つまり、データセット内の最後のスナップショットが `end_snapshot_id` の場合、その最後のスナップショットを超えるスナップショットはないので、クエリはゼロ行を返します。 |
| `SINCE start_snapshot_id INNER JOIN table_to_be_joined AS OF your_chosen_snapshot_id ON table_to_be_queried.id = table_to_be_joined.id` | 指定されたスナップショット ID から始まるデータを `table_to_be_queried` から読み取り、`your_chosen_snapshot_id` の時点の `table_to_be_joined` からのデータと結合します。 結合は、結合する 2 つのテーブルの ID 列の一致する ID に基づきます。 |

`SNAPSHOT` 句は、テーブルまたはテーブルの別名と一緒に使用できますが、サブクエリまたはビューの上には使用できません。 `SNAPSHOT` 句は、テーブルの `SELECT` クエリを適用できる場所で機能します。

また、スナップショット句の特別なオフセット値として `HEAD` と `TAIL` を使用することもできます。 `HEAD` を使用する場合は、最初のスナップショットの前のオフセットを参照し、`TAIL` を使用する場合は、最後のスナップショットの後のオフセットを参照します。

>[!NOTE]
>
>2 つのスナップショット ID 間でクエリを実行する場合、「開始スナップショット」の有効期限が切れ、オプションのフォールバック動作フラグ （`resolve_fallback_snapshot_on_failure`）が設定されていると、次の 2 つのシナリオが発生する可能性があります。
>
>- オプションのフォールバック動作フラグが設定されている場合、クエリサービスは使用可能な最も古いスナップショットを選択して、それを開始スナップショットとして設定し、使用可能な最も古いスナップショットと指定された終了スナップショットの間のデータを返します。 このデータには、使用可能な最も古いスナップショットの **含む** が含まれます。

### WHERE 句

デフォルトでは、`SELECT` クエリの `WHERE` 句で生成される一致は、大文字と小文字が区別されます。 大文字と小文字を区別せずに一致させる場合は、`LIKE` の代わりにキーワード `ILIKE` を使用します。

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

このクエリは、「A」または「a」で始まる名前の顧客を返します。

### 結合

結合を使用する `SELECT` クエリの構文は次のとおりです。

```sql
SELECT statement
FROM statement
[JOIN | INNER JOIN | LEFT JOIN | LEFT OUTER JOIN | RIGHT JOIN | RIGHT OUTER JOIN | FULL JOIN | FULL OUTER JOIN]
ON join condition
```

### UNION、INTERSECT、EXCEPT

`UNION`、`INTERSECT`、および `EXCEPT` 句は、2 つ以上のテーブルの like 行を組み合わせたり、除外したりするために使用します。

```sql
SELECT statement 1
[UNION | UNION ALL | UNION DISTINCT | INTERSECT | EXCEPT | MINUS]
SELECT statement 2
```

### CREATE TABLE AS SELECT {#create-table-as-select}

`CREATE TABLE AS SELECT` （CTAS）コマンドを使用して、`SELECT` クエリの結果を新しいテーブルに生成します。 これは、変換後のデータセットを作成したり、集計を実行したり、機能がエンジニアリングされたデータをモデルで使用する前にプレビューしたりする場合に便利です。

変換後の機能を使用してモデルのトレーニングを行う準備が整ったら、[Models のドキュメント &#x200B;](../advanced-statistics/models.md) を参照して、`TRANSFORM` 句で `CREATE MODEL` を使用する方法を確認してください。

オプションで、`TRANSFORM` 句を含めて、CTAS 文内に 1 つ以上の機能エンジニアリング関数を直接適用することもできます。 `TRANSFORM` を使用して、モデルのトレーニング前に変換ロジックの結果を調べます。

この構文は、永続テーブルと一時テーブルの両方に適用されます。

```sql
CREATE TABLE table_name 
  [WITH (schema='target_schema_title', rowvalidation='false', label='PROFILE')] 
  [TRANSFORM (transformFunctionExpression1, transformFunctionExpression2, ...)]
AS (select_query)
```

```sql
CREATE TEMP TABLE table_name 
  [WITH (schema='target_schema_title', rowvalidation='false', label='PROFILE')] 
  [TRANSFORM (transformFunctionExpression1, transformFunctionExpression2, ...)]
AS (select_query)
```

| パラメーター | 説明 |
| ----- | ----- |
| `schema` | XDM スキーマのタイトル。 この句は、新しいテーブルを既存の XDM スキーマに関連付ける場合にのみ使用します。 |
| `rowvalidation` | （任意）データセットに取り込まれた各バッチに対して、行レベルの検証を有効にします。 デフォルトは true です。 |
| `label` | （オプション）値 `PROFILE` を使用して、プロファイル取り込みが有効としてデータセットにラベルを付けます。 |
| `transform` | （任意）データセットを実現する前に、機能エンジニアリング変換（文字列インデックス、ワンホットエンコーディング、TF-IDF など）を適用します。 この句は、変換されたフィーチャのプレビューに使用されます。 詳しくは [&#128279;](#transform)`TRANSFORM` 句のドキュメントを参照してください。 |
| `select_query` | データセットを定義する標準の `SELECT` ステートメント。 詳しくは、[`SELECT` クエリの節 &#x200B;](#select-queries) 参照してください。 |

>[!NOTE]
>
>`SELECT` ステートメントには、`COUNT`、`SUM`、`MIN` などの集計関数のエイリアスを含める必要があります。 `SELECT` クエリには括弧を使用しても使用しなくても指定できます。 `TRANSFORM` 句を使用するかどうかに関係なく、この設定が適用されます。

**例**

`TRANSFORM` 句を使用してエンジニアリングされた機能をいくつかプレビューする基本的な例を次に示します。

```sql
CREATE TABLE ctas_transform_table_vp14 
TRANSFORM(
  String_Indexer(additional_comments) si_add_comments,
  one_hot_encoder(si_add_comments) as ohe_add_comments,
  tokenizer(comments) as token_comments
)
AS SELECT * FROM movie_review_e2e_DND;
```

複数の変換手順を持つ、より高度な例です。

```sql
CREATE TABLE ctas_transform_table 
TRANSFORM(
  String_Indexer(additional_comments) si_add_comments,
  one_hot_encoder(si_add_comments) as ohe_add_comments,
  tokenizer(comments) as token_comments,
  stop_words_remover(token_comments, array('and','very','much')) stp_token,
  ngram(stp_token, 3) ngram_token,
  tf_idf(ngram_token, 20) ngram_idf,
  count_vectorizer(stp_token, 13) cnt_vec_comments,
  tf_idf(token_comments, 10, 1) as cmts_idf
)
AS SELECT * FROM movie_review;
```

一時テーブルの例を次に示します。

```sql
CREATE TEMP TABLE ctas_transform_table 
TRANSFORM(
  String_Indexer(additional_comments) si_add_comments,
  one_hot_encoder(si_add_comments) as ohe_add_comments,
  tokenizer(comments) as token_comments,
  stop_words_remover(token_comments, array('and','very','much')) stp_token,
  ngram(stp_token, 3) ngram_token,
  tf_idf(ngram_token, 20) ngram_idf,
  count_vectorizer(stp_token, 13) cnt_vec_comments,
  tf_idf(token_comments, 10, 1) as cmts_idf
)
AS SELECT * FROM movie_review;
```

#### 制限事項と動作 {#limitations-and-behavior}

`TRANSFORM` 句を `CREATE TABLE` または `CREATE TEMP TABLE` と共に使用する場合は、次の制限に注意してください。

- 変換関数がベクトル出力を生成する場合は、自動的に配列に変換されます。
- その結果、`TRANSFORM` を使用して作成されたテーブルは、`CREATE MODEL` ステートメントで直接使用できません。 適切なフィーチャーベクトルを生成するには、モデル作成時に変換ロジックを再定義する必要があります。
- 変換は、テーブルの作成時にのみ適用されます。 `INSERT INTO` を使用してテーブルに挿入された新しいデータは、**自動的には変換されません**。 新しいデータに変換を適用するには、`TRANSFORM` 句を使用してテーブルを再作成 `CREATE TABLE AS SELECT` る必要があります。
- この方法は、再利用可能な変換パイプラインを構築するためではなく、ある時点での変換のプレビューと検証を目的としています。

>[!NOTE]
>
>使用可能な変換関数とその出力タイプについて詳しくは、[&#x200B; フィーチャー変換出力データタイプ &#x200B;](../advanced-statistics/feature-transformation.md#available-transformations) を参照してください。


### TRANSFORM 句 {#transform}

モデルのトレーニングまたはテーブルの作成前に、`TRANSFORM` 句を使用して 1 つ以上のフィーチャーエンジニアリング関数をデータセットに適用します。 この句を使用すると、入力フィーチャの正確な形状をプレビュー、検証、または定義できます。

`TRANSFORM` 句は、次の文で使用できます。

- `CREATE MODEL`
- `CREATE TABLE`
- `CREATE TEMP TABLE`

変換の定義、モデルオプションの設定、トレーニングデータの設定の方法など、CREATE MODEL の使用方法について詳しくは、[&#x200B; モデルのドキュメント &#x200B;](../advanced-statistics/models.md) を参照してください。

`CREATE TABLE` の使用方法については、[CREATE TABLE AS SELECT セクション &#x200B;](#create-table-as-select) を参照してください。

#### モデルを作成の例

```sql
CREATE MODEL review_model
TRANSFORM(
  String_Indexer(additional_comments) si_add_comments,
  one_hot_encoder(si_add_comments) AS ohe_add_comments,
  tokenizer(comments) AS token_comments,
  stop_words_remover(token_comments, array('and','very','much')) AS stp_token,
  ngram(stp_token, 3) AS ngram_token,
  tf_idf(ngram_token, 20) AS ngram_idf,
  count_vectorizer(stp_token, 13) AS cnt_vec_comments,
  tf_idf(token_comments, 10, 1) AS cmts_idf,
  vector_assembler(array(cmts_idf, viewsgot, ohe_add_comments, ngram_idf, cnt_vec_comments)) AS features
)
OPTIONS(MODEL_TYPE='logistic_reg', LABEL='reviews')
AS SELECT * FROM movie_review_e2e_DND;
```

#### 制限事項 {#limitations}

`CREATE TABLE` で `TRANSFORM` を使用する場合は、次の制限が適用されます。 変換され `CREATE TABLE AS SELECT` データの保存方法、ベクトル出力の処理方法、結果をモデルトレーニングワークフローで直接再利用できない理由について詳しくは、制限事項と動作の節を参照してください。

- ベクトル出力は自動的に配列に変換されますが、`CREATE MODEL` で直接使用することはできません。
- 変換ロジックは、メタデータとして保持されず、バッチ間で再利用できません。

## INSERT INTO

`INSERT INTO` コマンドは、次のように定義されます。

>[!IMPORTANT]
>
>クエリサービスは、ITAS エンジンを使用した **追加のみの操作** をサポートしています。 `INSERT INTO` はサポートされている唯一のデータ操作コマンドであり、**update** および **delete** 操作は使用できません。 データの変更を反映するには、目的の状態を表す新しいレコードを挿入します。

```sql
INSERT INTO table_name select_query
```

| パラメーター | 説明 |
| ----- | ----- |
| `table_name` | クエリを挿入するテーブルの名前。 |
| `select_query` | `SELECT` 文。 `SELECT` クエリの構文は、[SELECT クエリ セクション &#x200B;](#select-queries) にあります。 |

**例**

>[!NOTE]
>
>以下は計画的な例であり、簡単に説明を目的としています。

```sql
INSERT INTO Customers SELECT SupplierName, City, Country FROM OnlineCustomers;

INSERT INTO Customers AS (SELECT * from OnlineCustomers SNAPSHOT AS OF 345)
```

>[!INFO]
> 
>`SELECT` 文 **丸カッコ（）** 囲まないでください。 また、`SELECT` ステートメントの結果のスキーマは、`INSERT INTO` ステートメントで定義されたテーブルのスキーマと一致する必要があります。 `SNAPSHOT` 句を指定すると、増分差分をターゲットテーブルに読み込むことができます。

実際の XDM スキーマのほとんどのフィールドはルートレベルで見つからず、SQL ではドット表記の使用が許可されていません。 ネストされたフィールドを使用して現実的な結果を得るには、`INSERT INTO` パスの各フィールドをマッピングする必要があります。

ネストされたパスを `INSERT INTO` すには、次の構文を使用します。

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

`DROP TABLE` コマンドは、既存のテーブルを削除し、そのテーブルに関連付けられているディレクトリが外部テーブルでない場合は、ファイルシステムから削除します。 テーブルが存在しない場合、例外が発生します。

```sql
DROP TABLE [IF EXISTS] [db_name.]table_name
```

| パラメーター | 説明 |
| ------ | ------ |
| `IF EXISTS` | これを指定した場合、テーブルが存在しない **場合でも例外はスローされ** せん。 |

## データベースを作成

`CREATE DATABASE` コマンドは、Azure Data Lake Storage （ADLS）データベースを作成します。

```sql
CREATE DATABASE [IF NOT EXISTS] db_name
```

## データベースの削除

`DROP DATABASE` コマンドは、インスタンスからデータベースを削除します。

```sql
DROP DATABASE [IF EXISTS] db_name
```

| パラメーター | 説明 |
| ------ | ------ |
| `IF EXISTS` | これを指定した場合、データベースが存在しない **場合は例外がスローされ** せん。 |

## スキーマをドロップ

`DROP SCHEMA` コマンドは、既存のスキーマを削除します。

```sql
DROP SCHEMA [IF EXISTS] db_name.schema_name [ RESTRICT | CASCADE]
```

| パラメーター | 説明 |
| ------ | ------ |
| `IF EXISTS` | このパラメーターを指定し、スキーマが存在し **い** 場合、例外はスローされません。 |
| `RESTRICT` | モードのデフォルト値。 指定した場合、スキーマにテーブルが含まれていない **場合** のみ削除されます。 |
| `CASCADE` | 指定した場合、スキーマは、スキーマに存在するすべてのテーブルと共にドロップされます。 |

## CREATE VIEW {#create-view}

SQL ビューは、SQL 文の結果セットに基づく仮想テーブルです。 `CREATE VIEW` ステートメントでビューを作成し、名前を付けます。 その後、その名前を使用して、クエリの結果を返すことができます。 これにより、複雑なクエリの再利用が容易になります。

次の構文は、データセットの `CREATE VIEW` クエリを定義します。 このデータセットは、ADLS または高速ストアデータセットにすることができます。

```sql
CREATE VIEW view_name AS select_query
```

| パラメーター | 説明 |
| ------ | ------ |
| `view_name` | 作成するビューの名前。 |
| `select_query` | `SELECT` 文。 `SELECT` クエリの構文は、[SELECT クエリ セクション &#x200B;](#select-queries) にあります。 |

**例**

```sql
CREATE VIEW V1 AS SELECT color, type FROM Inventory

CREATE OR REPLACE VIEW V1 AS SELECT model, version FROM Inventory
```

次の構文は、データベースとスキーマのコンテキストでビューを作成する `CREATE VIEW` クエリを定義します。

**例**

```sql
CREATE VIEW db_name.schema_name.view_name AS select_query
CREATE OR REPLACE VIEW db_name.schema_name.view_name AS select_query
```

| パラメーター | 説明 |
| ------ | ------ |
| `db_name` | データベースの名前。 |
| `schema_name` | スキーマの名前。 |
| `view_name` | 作成するビューの名前。 |
| `select_query` | `SELECT` 文。 `SELECT` クエリの構文は、[SELECT クエリ セクション &#x200B;](#select-queries) にあります。 |

**例**

```sql
CREATE VIEW <dbV1 AS SELECT color, type FROM Inventory;

CREATE OR REPLACE VIEW V1 AS SELECT model, version FROM Inventory;
```

## ビューの表示

次のクエリは、ビューのリストを示しています。

```sql
SHOW VIEWS;
```

```console
 Db Name  | Schema Name | Name  | Id       |  Dataset Dependencies | Views Dependencies | TYPE
----------------------------------------------------------------------------------------------
 qsaccel  | profile_agg | view1 | view_id1 | dwh_dataset1          |                    | DWH
          |             | view2 | view_id2 | adls_dataset          | adls_views         | ADLS
(2 rows)
```

## DROP VIEW

次の構文は、`DROP VIEW` クエリを定義します。

```sql
DROP VIEW [IF EXISTS] view_name
```

| パラメーター | 説明 |
| ------ | ------ |
| `IF EXISTS` | これを指定した場合、ビューが存在しない **場合でも例外はスローされ** せん。 |
| `view_name` | 削除するビューの名前。 |

**例**

```sql
DROP VIEW v1
DROP VIEW IF EXISTS v1
```

## 匿名ブロック {#anonymous-block}

匿名ブロックは、実行可能セクションと例外処理セクションの 2 つのセクションで構成されます。 匿名ブロックの場合、実行可能セクションは必須です。 ただし、例外処理セクションはオプションです。

次の例は、一緒に実行される 1 つ以上のステートメントを含むブロックを作成する方法を示しています。

```sql
$$BEGIN
  statementList
[EXCEPTION exceptionHandler]
$$END

exceptionHandler:
      WHEN OTHER
      THEN statementList

statementList:
    : (statement (';')) +
```

以下に、匿名ブロックを使用した例を示します。

```sql
$$BEGIN
   SET @v_snapshot_from = select parent_id  from (select history_meta('email_tracking_experience_event_dataset') ) tab where is_current;
   SET @v_snapshot_to = select snapshot_id from (select history_meta('email_tracking_experience_event_dataset') ) tab where is_current;
   SET @v_log_id = select now();
   CREATE TABLE tracking_email_id_incrementally
     AS SELECT _id AS id FROM email_tracking_experience_event_dataset SNAPSHOT BETWEEN @v_snapshot_from AND @v_snapshot_to;

EXCEPTION
  WHEN OTHER THEN
    DROP TABLE IF EXISTS tracking_email_id_incrementally;
    SELECT 'ERROR';
$$END;
```

### 匿名ブロック内の条件文 {#conditional-anonymous-block-statements}

IF-THEN-ELSE 制御構造を使用すると、条件が TRUE と評価された場合に、ステートメントのリストの条件付き実行が可能になります。 このコントロール構造は、匿名ブロック内でのみ適用できます。 この構造をスタンドアロンコマンドとして使用すると、構文エラー（「匿名ブロックの外部の無効なコマンド」）が発生します。

以下のコードスニペットは、匿名ブロック内の IF-THEN-ELSE 条件文の正しい形式を示しています。

```javascript
IF booleanExpression THEN
   List of statements;
ELSEIF booleanExpression THEN 
   List of statements;
ELSEIF booleanExpression THEN 
   List of statements;
ELSE
   List of statements;
END IF
```

**例**

次の例では、`SELECT 200;` を実行します。

```sql
$$BEGIN
    SET @V = SELECT 2;
    SELECT @V;
    IF @V = 1 THEN
       SELECT 100;
    ELSEIF @V = 2 THEN
       SELECT 200;
    ELSEIF @V = 3 THEN
       SELECT 300;
    ELSE    
       SELECT 'DEFAULT';
    END IF;   

 END$$;
```

この構造を `raise_error();` で使用すると、カスタムエラーメッセージを返すことができます。 以下に示すコードブロックは、「カスタムエラーメッセージ」で匿名ブロックを終了します。

**例**

```sql
$$BEGIN
    SET @V = SELECT 5;
    SELECT @V;
    IF @V = 1 THEN
       SELECT 100;
    ELSEIF @V = 2 THEN
       SELECT 200;
    ELSEIF @V = 3 THEN
       SELECT 300;
    ELSE    
       SELECT raise_error('custom error message');
    END IF;   

 END$$;
```

#### ネストされた IF ステートメント

ネストされた IF 文は、匿名ブロック内でサポートされます。

**例**

```sql
$$BEGIN
    SET @V = SELECT 1;
    IF @V = 1 THEN
       SELECT 100;
       IF @V > 0 THEN
         SELECT 1000;
       END IF;   
    END IF;   

 END$$; 
```

#### 例外ブロック

例外ブロックは、匿名ブロック内でサポートされます。

**例**

```sql
$$BEGIN
    SET @V = SELECT 2;
    IF @V = 1 THEN
       SELECT 100;
    ELSEIF @V = 2 THEN
       SELECT raise_error(concat('custom-error for v= ', '@V' ));

    ELSEIF @V = 3 THEN
       SELECT 300;
    ELSE    
       SELECT 'DEFAULT';
    END IF;  
EXCEPTION WHEN OTHER THEN 
  SELECT 'THERE WAS AN ERROR';    
 END$$;
```

### JSON に自動 {#auto-to-json}

クエリサービスは、オプションのセッションレベル設定をサポートし、インタラクティブ SELECT クエリから最上位の複雑なフィールドを JSON 文字列として返します。 `auto_to_json` 設定を使用すると、複雑なフィールドのデータを JSON として返し、標準のライブラリを使用して JSON オブジェクトに解析できます。

複雑なフィールドを含む SELECT クエリを実行する前に、機能フラグ `auto_to_json` を true に設定します。

```sql
set auto_to_json=true; 
```

#### `auto_to_json` フラグの設定前

次の表は、`auto_to_json` 設定が適用される前のクエリ結果の例を示しています。 両方のシナリオで、複雑なフィールドを持つテーブルをターゲットとする同じ SELECT クエリ（以下に示す）が使用されました。

```sql
SELECT * FROM TABLE_WITH_COMPLEX_FIELDS LIMIT 2;
```

結果は次のようになります。

```console
                _id                |                                _experience                                 | application  |                   commerce                   | dataSource |                               device                               |                       endUserIDs                       |                                                                                                environment                                                                                                |                     identityMap                     |                              placeContext                               |   receivedTimestamp   |       timestamp       | userActivityRegion |                                         web                                          | _adcstageforpqs
-----------------------------------+----------------------------------------------------------------------------+--------------+----------------------------------------------+------------+--------------------------------------------------------------------+--------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------+-------------------------------------------------------------------------+-----------------------+-----------------------+--------------------+--------------------------------------------------------------------------------------+-----------------
 31892EE15DE00000-401D52664FF48A52 | ("("("(1,1)","(1,1)")","(-209479095,4085488201,-2105158467,2189808829)")") | (background) | (NULL,"(USD,NULL)",NULL,NULL,NULL,NULL,NULL) | (475341)   | (32,768,1024,205202,https://ns.adobe.com/xdm/external/deviceatlas) | ("("(31892EE080007B35-E6CE00000000000,"(AAID)",t)")")  | ("(en-US,f,f,t,1.6,"Mozilla/5.0 (iPhone; U; CPU iPhone OS 4_1 like Mac OS X; ja-jp) AppleWebKit/532.9 (KHTML, like Gecko) Version/4.0.5 Mobile/8B117 Safari/6531.22.7",490,1125)",xo.net,64.3.235.13)     | [AAID -> "{(31892EE080007B35-E6CE00000000000,t)}"]  | ("("(34.01,-84.0)",lawrenceville,US,524,30043,ga)",600)                 | 2022-09-02 19:47:14.0 | 2022-09-02 19:47:14.0 | (UT1)              | ("(f,Search Results,"(1.0)")","(http://www.google.com/search?ie=UTF-8&q=,internal)") |
 31892EE15DE00000-401B92664FF48AE8 | ("("("(1,1)","(1,1)")","(-209479095,4085488201,-2105158467,2189808829)")") | (background) | (NULL,"(USD,NULL)",NULL,NULL,NULL,NULL,NULL) | (475341)   | (32,768,1024,205202,https://ns.adobe.com/xdm/external/deviceatlas) | ("("(31892EE100007BF3-215FE00000000001,"(AAID)",t)")") | ("(en-US,f,f,t,1.5,"Mozilla/5.0 (iPhone; U; CPU iPhone OS 4_1 like Mac OS X; ja-jp) AppleWebKit/532.9 (KHTML, like Gecko) Version/4.0.5 Mobile/8B117 Safari/6531.22.7",768,556)",ntt.net,219.165.108.145) | [AAID -> "{(31892EE100007BF3-215FE00000000001,t)}"] | ("("(34.989999999999995,138.42)",shizuoka,JP,392005,420-0812,22)",-240) | 2022-09-02 19:47:14.0 | 2022-09-02 19:47:14.0 | (UT1)              | ("(f,Home - JJEsquire,"(1.0)")","(NULL,typed_bookmarked)")                           |
(2 rows)  
```

#### `auto_to_json` フラグの設定後

次の表に、結果のデータセットに対する `auto_to_json` 設定の結果の違いを示します。 両方のシナリオで同じ SELECT クエリが使用されました。

```console
                _id                |   receivedTimestamp   |       timestamp       |                                                                                                                   _experience                                                                                                                   |           application            |             commerce             |    dataSource    |                                                                  device                                                                   |                                                   endUserIDs                                                   |                                                                                                                                                                                           environment                                                                                                                                                                                            |                             identityMap                              |                                                                                            placeContext                                                                                            |      userActivityRegion      |                                                                                     web                                                                                      | _adcstageforpqs
-----------------------------------+-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------+----------------------------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------
 31892EE15DE00000-401D52664FF48A52 | 2022-09-02 19:47:14.0 | 2022-09-02 19:47:14.0 | {"analytics":{"customDimensions":{"eVars":{"eVar1":"1","eVar2":"1"},"props":{"prop1":"1","prop2":"1"}},"environment":{"browserID":-209479095,"browserIDStr":"4085488201","operatingSystemID":-2105158467,"operatingSystemIDStr":"2189808829"}}} | {"userPerspective":"background"} | {"order":{"currencyCode":"USD"}} | {"_id":"475341"} | {"colorDepth":32,"screenHeight":768,"screenWidth":1024,"typeID":"205202","typeIDService":"https://ns.adobe.com/xdm/external/deviceatlas"} | {"_experience":{"aaid":{"id":"31892EE080007B35-E6CE00000000000","namespace":{"code":"AAID"},"primary":true}}}  | {"browserDetails":{"acceptLanguage":"en-US","cookiesEnabled":false,"javaEnabled":false,"javaScriptEnabled":true,"javaScriptVersion":"1.6","userAgent":"Mozilla/5.0 (iPhone; U; CPU iPhone OS 4_1 like Mac OS X; ja-jp) AppleWebKit/532.9 (KHTML, like Gecko) Version/4.0.5 Mobile/8B117 Safari/6531.22.7","viewportHeight":490,"viewportWidth":1125},"domain":"xo.net","ipV4":"64.3.235.13"}     | {"AAID":[{"id":"31892EE080007B35-E6CE00000000000","primary":true}]}  | {"geo":{"_schema":{"latitude":34.01,"longitude":-84.0},"city":"lawrenceville","countryCode":"US","dmaID":524,"postalCode":"30043","stateProvince":"ga"},"localTimezoneOffset":600}                 | {"dataCenterLocation":"UT1"} | {"webPageDetails":{"isHomePage":false,"name":"Search Results","pageViews":{"value":1.0}},"webReferrer":{"URL":"http://www.google.com/search?ie=UTF-8&q=","type":"internal"}} |
 31892EE15DE00000-401B92664FF48AE8 | 2022-09-02 19:47:14.0 | 2022-09-02 19:47:14.0 | {"analytics":{"customDimensions":{"eVars":{"eVar1":"1","eVar2":"1"},"props":{"prop1":"1","prop2":"1"}},"environment":{"browserID":-209479095,"browserIDStr":"4085488201","operatingSystemID":-2105158467,"operatingSystemIDStr":"2189808829"}}} | {"userPerspective":"background"} | {"order":{"currencyCode":"USD"}} | {"_id":"475341"} | {"colorDepth":32,"screenHeight":768,"screenWidth":1024,"typeID":"205202","typeIDService":"https://ns.adobe.com/xdm/external/deviceatlas"} | {"_experience":{"aaid":{"id":"31892EE100007BF3-215FE00000000001","namespace":{"code":"AAID"},"primary":true}}} | {"browserDetails":{"acceptLanguage":"en-US","cookiesEnabled":false,"javaEnabled":false,"javaScriptEnabled":true,"javaScriptVersion":"1.5","userAgent":"Mozilla/5.0 (iPhone; U; CPU iPhone OS 4_1 like Mac OS X; ja-jp) AppleWebKit/532.9 (KHTML, like Gecko) Version/4.0.5 Mobile/8B117 Safari/6531.22.7","viewportHeight":768,"viewportWidth":556},"domain":"ntt.net","ipV4":"219.165.108.145"} | {"AAID":[{"id":"31892EE100007BF3-215FE00000000001","primary":true}]} | {"geo":{"_schema":{"latitude":34.989999999999995,"longitude":138.42},"city":"shizuoka","countryCode":"JP","dmaID":392005,"postalCode":"420-0812","stateProvince":"22"},"localTimezoneOffset":-240} | {"dataCenterLocation":"UT1"} | {"webPageDetails":{"isHomePage":false,"name":"Home - JJEsquire","pageViews":{"value":1.0}},"webReferrer":{"type":"typed_bookmarked"}}                                        |
(2 rows)
```

### 失敗時のフォールバックスナップショットの解決 {#resolve-fallback-snapshot-on-failure}

`resolve_fallback_snapshot_on_failure` オプションは、期限切れのスナップショット ID の問題を解決するために使用します。

スナップショットを以前のスナップショット ID で上書きするには、`resolve_fallback_snapshot_on_failure` オプションを true に設定します。

```sql
SET resolve_fallback_snapshot_on_failure=true;
```

次のコード行では、メタデータから入手できる最も古い `snapshot_id` で `@from_snapshot_id` をオーバーライドしています。

```sql
$$ BEGIN
    SET resolve_fallback_snapshot_on_failure=true;
    SET @from_snapshot_id = SELECT coalesce(last_snapshot_id, 'HEAD') FROM checkpoint_log a JOIN
                            (SELECT MAX(process_timestamp)process_timestamp FROM checkpoint_log
                                WHERE process_name = 'DIM_TABLE_ABC' AND process_status = 'SUCCESSFUL' )b
                                on a.process_timestamp=b.process_timestamp;
    SET @to_snapshot_id = SELECT snapshot_id FROM (SELECT history_meta('DIM_TABLE_ABC')) WHERE  is_current = true;
    SET @last_updated_timestamp= SELECT CURRENT_TIMESTAMP;
    INSERT INTO DIM_TABLE_ABC_Incremental
     SELECT  *  FROM DIM_TABLE_ABC SNAPSHOT BETWEEN @from_snapshot_id AND @to_snapshot_id WHERE NOT EXISTS (SELECT _id FROM DIM_TABLE_ABC_Incremental a WHERE _id=a._id);

Insert Into
   checkpoint_log
   SELECT
       'DIM_TABLE_ABC' process_name,
       'SUCCESSFUL' process_status,
      cast( @to_snapshot_id AS string) last_snapshot_id,
      cast( @last_updated_timestamp AS TIMESTAMP) process_timestamp;
EXCEPTION
  WHEN OTHER THEN
    SELECT 'ERROR';
END
$$;
```


## データアセット組織

データアセットの増加に応じて、Adobe Experience Platform データレイク内のデータアセットを論理的に整理することが重要です。 クエリサービスは、サンドボックス内のデータアセットを論理的にグループ化できる SQL 構成を拡張します。 この編成方法を使用すると、スキーマを物理的に移動しなくても、スキーマ間でデータアセットを共有できます。

標準の SQL 構文を使用した次の SQL 構成がサポートされており、データを論理的に整理できます。

```SQL
CREATE DATABASE dg1;
CREATE SCHEMA dg1.schema1;
CREATE table t1 ...;
CREATE view v1 ...;
ALTER TABLE t1 ADD PRIMARY KEY (c1) NOT ENFORCED;
ALTER TABLE t2 ADD FOREIGN KEY (c1) REFERENCES t1(c1) NOT ENFORCED;
```

クエリサービスのベストプラクティスについて詳しくは、[&#x200B; データアセットの論理的構成 &#x200B;](../best-practices/organize-data-assets.md) ガイドを参照してください。

## テーブルが存在します

`table_exists` SQL コマンドは、テーブルが現在システムに存在するかどうかを確認するために使用します。 このコマンドは、ブール値を返します。テーブル **が存在** する場合は `true`、テーブル **が存在しない** 場合は `false` です。

文を実行する前にテーブルが存在するかどうかを検証することで、`table_exists` 機能により、`CREATE` と `INSERT INTO` のユースケースの両方に対応する匿名ブロックを記述するプロセスが簡略化されます。

次の構文は、`table_exists` コマンドを定義します。

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

`inline` 関数は、構造体の配列の要素を分割し、値をテーブルに生成します。 `SELECT` リストまたは `LATERAL VIEW` にのみ配置できます。

`inline` 関数 **使用できません** は、他のジェネレータ関数がある選択リストに配置できます。

デフォルトでは、生成される列の名前は「col1」、「col2」などです。 式が `NULL` の場合、行は生成されません。

>[!TIP]
>
>列名は、`RENAME` コマンドを使用して名前を変更できます。

**例**

```sql
> SELECT inline(array(struct(1, 'a'), struct(2, 'b'))), 'Spark SQL';
```

この例の戻り値は次のとおりです。

```text
1  a Spark SQL
2  b Spark SQL
```

この 2 番目の例では、`inline` 関数の概念と適用をさらに示しています。 この例のデータモデルを次の画像に示します。

![productListItems のスキーマ図。](../images/sql/productListItems.png)

**例**

```sql
select inline(productListItems) from source_dataset limit 10;
```

`source_dataset` から取得された値は、ターゲットテーブルの入力に使用されます。

| SKU | _experience | 数量 | priceTotal |
|---------------------|-----------------------------------|----------|--------------|
| product-id-1 | （&quot;（&quot;（&quot;（A,pass,B,NULL）&quot;）&quot;） | 5 | 10.5 |
| product-id-5 | （&quot;（&quot;（&quot;（A, pass, B,NULL）&quot;）&quot;） |          |              |
| product-id-2 | （「（（&quot;（AF, C, D,NULL）&quot;）&quot;） | 6 | 40 |
| product-id-4 | （&quot;（&quot;（&quot;（BM, pass, NA,NULL）&quot;）&quot;） | 3 | 12 |

## SET

`SET` コマンドは、プロパティを設定し、既存のプロパティの値を返すか、既存のすべてのプロパティを一覧表示します。 既存プロパティのキーに値が指定された場合、古い値が上書きされます。

```sql
SET property_key = property_value
```

| パラメーター | 説明 |
| ------ | ------ |
| `property_key` | リストまたは変更するプロパティの名前。 |
| `property_value` | プロパティに設定する値。 |

任意の設定の値を返すには、`property_value` を付けずに `SET [property key]` を使用します。

## [!DNL PostgreSQL] コマンド

以下のサブセクションでは、クエリサービスでサポートされている [!DNL PostgreSQL] コマンドについて説明します。

### 分析テーブル {#analyze-table}

`ANALYZE TABLE` コマンドは、指定されたテーブルに対して分布分析および統計計算を実行します。 `ANALYZE TABLE` の使用方法は、データセットが [&#x200B; 高速化ストア &#x200B;](#compute-statistics-data-lake) と [&#x200B; データレイク &#x200B;](#compute-statistics-accelerated-store) のどちらに保存されているかによって異なります。 使用方法について詳しくは、それぞれの節を参照してください。

#### 高速化ストアの統計を計算 {#compute-statistics-accelerated-store}

`ANALYZE TABLE` コマンドは、高速化ストア上のテーブルの統計を計算します。 統計は、高速化ストア上の特定のテーブルに対して実行された CTAS または ITAS クエリに基づいて計算されます。

**例**

```sql
ANALYZE TABLE <original_table_name>
```

`ANALYZE TABLE` コマンドを使用した後に使用できる統計計算のリストを以下に示します。-

| 計算された値 | 説明 |
|---|---|
| `field` | テーブルの列の名前。 |
| `data-type` | 各列に許容されるデータタイプ。 |
| `count` | このフィールドに null 以外の値を含む行の数。 |
| `distinct-count` | このフィールドの一意または個別の値の数。 |
| `missing` | このフィールドの null 値を持つ行の数。 |
| `max` | 分析されたテーブルの最大値。 |
| `min` | 分析されたテーブルの最小値。 |
| `mean` | 分析されたテーブルの平均値。 |
| `stdev` | 分析されたテーブルの標準偏差。 |

#### データレイクの統計を計算 {#compute-statistics-data-lake}

`COMPUTE STATISTICS` SQL コマンドを使用して、[!DNL Azure Data Lake Storage] （ADLS）データセットに関する列レベルの統計を計算できるようになりました。 データセット全体、データセットのサブセット、すべての列、列のサブセットのいずれかに関する列の統計を計算。

`COMPUTE STATISTICS` は、`ANALYZE TABLE` コマンドを拡張します。 ただし、`COMPUTE STATISTICS`、`FILTERCONTEXT`、`FOR COLUMNS` コマンドは、高速ストアテーブルではサポートされていません。 `ANALYZE TABLE` コマンドのこれらの拡張機能は、現在 ADLS テーブルでのみサポートされています。

**例**

```sql
ANALYZE TABLE tableName FILTERCONTEXT (timestamp >= to_timestamp('2023-04-01 00:00:00') and timestamp <= to_timestamp('2023-04-05 00:00:00')) COMPUTE STATISTICS  FOR COLUMNS (commerce, id, timestamp);
```

`FILTER CONTEXT` コマンドは、指定されたフィルター条件に基づいてデータセットのサブセットに関する統計を計算します。 `FOR COLUMNS` のコマンドは、分析用の特定の列をターゲットにします。

>[!NOTE]
>
>生成された `Statistics ID` と統計は、セッションごとに有効であり、異なる PSQL セッション間でアクセスすることはできません。<br><br> 制限事項：<ul><li>統計の生成は、配列またはマップのデータタイプではサポートされていません</li><li>計算された統計は、セッションをまたいで保持 **されません**。</li></ul><br><br> オプション：<br><ul><li>`skip_stats_for_complex_datatypes`</li></ul><br> デフォルトでは、フラグは true に設定されています。 その結果、サポートされていないデータタイプに関する統計がリクエストされた場合、エラーは発生しませんが、サポートされていないデータタイプのフィールドはサイレントにスキップされます。<br> サポートされていないデータタイプに関する統計がリクエストされた場合にエラーの通知を有効にするには、`SET skip_stats_for_complex_datatypes = false` を使用します。

コンソール出力は、次のように表示されます。

```console
|     Statistics ID      | 
| ---------------------- |
| adc_geometric_stats_1  |
(1 row)
```

その後、`Statistics ID` を参照して、計算された統計に対して直接クエリを実行できます。 以下のステートメントの例に示すように、`Statistics ID` またはエイリアス名を使用して、出力を完全に表示します。 この機能について詳しくは、[&#x200B; エイリアス名のドキュメント &#x200B;](../key-concepts/dataset-statistics.md#alias-name) を参照してください。

```sql
-- This statement gets the statistics generated for `alias adc_geometric_stats_1`.
SELECT * FROM adc_geometric_stats_1;
```

`SHOW STATISTICS` コマンドを使用して、セッションで生成されたすべての一時統計のメタデータを表示します。 このコマンドを使用すると、統計分析の範囲を絞り込むことができます。

```sql
SHOW STATISTICS;
```

SHOW STATISTICS の出力例を次に示します。

```console
      statsId         |   tableName   | columnSet |         filterContext       |      timestamp
----------------------+---------------+-----------+-----------------------------+--------------------
adc_geometric_stats_1 | adc_geometric |   (age)   |                             | 25/06/2023 09:22:26
demo_table_stats_1    |  demo_table   |    (*)    |       ((age > 25))          | 25/06/2023 12:50:26
age_stats             | castedtitanic |   (age)   | ((age > 25) AND (age < 40)) | 25/06/2023 09:22:26
```

詳しくは、[&#x200B; データセット統計のドキュメント &#x200B;](../key-concepts/dataset-statistics.md) を参照してください。

#### 大さじ {#tablesample}

Adobe Experience Platform クエリサービスは、近似クエリ処理機能の一部としてサンプルデータセットを提供します。

データセットのサンプルは、データセットに対する集計操作に対して正確な回答が必要ない場合に最適に使用されます。 近似クエリを発行して近似回答を返すことで、大規模なデータセットに対するより効率的な探索的クエリを実行するには、`TABLESAMPLE` 機能を使用します。

サンプルデータセットは、元のレコードのパーセンテージのみを使用して、既存の [!DNL Azure Data Lake Storage] （ADLS）データセットからの均一なランダムサンプルを使用して作成されます。 データセットサンプル機能は、`TABLESAMPLE` および `SAMPLERATE` SQL コマンドで `ANALYZE TABLE` コマンドを拡張します。

次の例では、1 行目にテーブルの 5% のサンプルを計算する方法が示されています。 2 行目では、テーブル内のデータのフィルター済みビューから 5% のサンプルを計算する方法を示しています。

**例**

```sql {line-numbers="true"}
ANALYZE TABLE tableName TABLESAMPLE SAMPLERATE 5;
ANALYZE TABLE tableName FILTERCONTEXT (timestamp >= to_timestamp('2023-01-01')) TABLESAMPLE SAMPLERATE 5:
```

詳しくは、[&#x200B; データセットサンプルのドキュメント &#x200B;](../key-concepts/dataset-samples.md) を参照してください。

### BEGIN

`BEGIN` コマンド、または `BEGIN WORK` または `BEGIN TRANSACTION` コマンドは、トランザクションブロックを開始します。 begin コマンドの後に入力された文は、明示的な COMMIT または ROLLBACK コマンドが指定されるまで、1 回のトランザクションで実行されます。 このコマンドは `START TRANSACTION` と同じです。

```sql
BEGIN
BEGIN WORK
BEGIN TRANSACTION
```

### CLOSE

`CLOSE` コマンドは、開いているカーソルに関連付けられているリソースを解放します。 カーソルを閉じた後の操作は許可されません。不要になったカーソルは閉じる必要があります。

```sql
CLOSE name
CLOSE ALL
```

`CLOSE name` を使用する場合、`name` は閉じる必要がある開いているカーソルの名前を表します。 `CLOSE ALL` を使用すると、開いているカーソルはすべて閉じられます。

### DEALLOCATE

以前に準備した SQL 文の割り当てを解除するには、`DEALLOCATE` コマンドを使用します。 プリペアドステートメントの割り当てを明示的に解除しなかった場合、セッションが終了すると割り当てが解除されます。 準備文の詳細については、[PREPARE コマンド &#x200B;](#prepare) の節を参照してください。

```sql
DEALLOCATE name
DEALLOCATE ALL
```

`DEALLOCATE name` を使用する場合、`name` は、割り当てを解除する必要がある準備済み文の名前を表します。 `DEALLOCATE ALL` を使用すると、準備済み文がすべて割り当て解除されます。

### DECLARE

`DECLARE` コマンドを使用すると、ユーザーはカーソルを作成できます。このカーソルを使用して、大きなクエリから少数の行を取得できます。 カーソルが作成された後、`FETCH` を使用して行がカーソルから取得されます。

```sql
DECLARE name CURSOR FOR query
```

| パラメーター | 説明 |
| ------ | ------ |
| `name` | 作成するカーソルの名前。 |
| `query` | カーソルによって返されるローを提供する `SELECT` または `VALUES` コマンド。 |

### EXECUTE

`EXECUTE` コマンドは、事前に準備された文を実行するために使用します。 準備済み文はセッション中にのみ存在するので、準備済み文は現在のセッションの前に実行された `PREPARE` 文によって作成されている必要があります。 準備文の使用について詳しくは、[`PREPARE` のコマンドの節を参照し &#x200B;](#prepare) ください。

ステートメントを作成した `PREPARE` ステートメントで一部のパラメーターが指定された場合は、互換性のあるパラメーターセットを `EXECUTE` ステートメントに渡す必要があります。 これらのパラメーターが渡されない場合、エラーが発生します。

```sql
EXECUTE name [ ( parameter ) ]
```

| パラメーター | 説明 |
| ------ | ------ |
| `name` | 実行する準備済みステートメントの名前。 |
| `parameter` | プリペアドステートメントに対するパラメーターの実際の値。 これは、準備文が作成されたときに決定されたように、このパラメータのデータ型と互換性のある値を生成する式でなければなりません。 プリペアドステートメントに複数のパラメーターがある場合は、コンマで区切ります。 |

### EXPLAIN

`EXPLAIN` コマンドは、指定された文の実行計画を表示します。 実行計画は、ステートメントによって参照されるテーブルのスキャン方法を示します。 複数のテーブルが参照されている場合、各入力テーブルから必要な行をまとめるために使用される結合アルゴリズムが表示されます。

```sql
EXPLAIN statement
```

応答の形式を定義するには、`FORMAT` キーワードを `EXPLAIN` コマンドと共に使用します。

```sql
EXPLAIN FORMAT { TEXT | JSON } statement
```

| パラメーター | 説明 |
| ------ | ------ |
| `FORMAT` | `FORMAT` コマンドを使用して、出力形式を指定します。 使用可能なオプションは、`TEXT` または `JSON` です。 テキスト以外の出力には、テキスト出力形式と同じ情報が含まれますが、プログラムの解析が容易です。このパラメーターのデフォルトは `TEXT` です。 |
| `statement` | 実行計画を確認する `SELECT`、`INSERT`、`UPDATE`、`DELETE`、`VALUES`、`EXECUTE`、`DECLARE`、`CREATE TABLE AS` または `CREATE MATERIALIZED VIEW AS` 文。 |

>[!IMPORTANT]
>
>`SELECT` ステートメントが返す出力は、`EXPLAIN` キーワードを指定して実行すると破棄されます。 その他の副作用は通常通り発生します。

**例**

次の例は、`integer` 列が 1 行と 10000 行を持つテーブルに対して単純なクエリを実行する計画を示しています。

```sql
EXPLAIN SELECT * FROM foo;
```

```console
                       QUERY PLAN
---------------------------------------------------------
 Seq Scan on foo (dataSetId = "6307eb92f90c501e072f8457", dataSetName = "foo") [0,1000000242,6973776840203d3d,6e616c58206c6153,6c6c6f430a3d4d20,74696d674c746365]
(1 row)
```

### FETCH

`FETCH` コマンドは、以前に作成したカーソルを使用して行を取得します。

```sql
FETCH num_of_rows [ IN | FROM ] cursor_name
```

| パラメーター | 説明 |
| ------ | ------ |
| `num_of_rows` | 取得する行数です。 |
| `cursor_name` | 情報を取得するカーソルの名前。 |

### PREPARE {#prepare}

`PREPARE` コマンドを使用すると、準備済み文を作成できます。 準備済み文は、類似の SQL 文をテンプレート化するために使用できるサーバー側オブジェクトです。

準備済みステートメントは、パラメーター（実行時にステートメントに置き換えられる値）を取ることができます。 プリペアドステートメントを使用する場合、パラメーターは$1、$2 などを使用して、位置によって参照されます。

オプションで、パラメーターデータタイプのリストを指定できます。 パラメーターのデータタイプがリストされていない場合、タイプはコンテキストから推論できます。

```sql
PREPARE name [ ( data_type [, ...] ) ] AS SELECT
```

| パラメーター | 説明 |
| ------ | ------ |
| `name` | 準備された文の名前。 |
| `data_type` | プリペアドステートメントのパラメーターのデータタイプ。 パラメーターのデータタイプがリストされていない場合、タイプはコンテキストから推論できます。 複数のデータタイプを追加する必要がある場合は、コンマ区切りのリストで追加できます。 |

### ROLLBACK

`ROLLBACK` コマンドは、現在のトランザクションを元に戻し、そのトランザクションによって行われたすべての更新を破棄します。

```sql
ROLLBACK
ROLLBACK WORK
```

### SELECT INTO

`SELECT INTO` コマンドは、新しいテーブルを作成し、クエリによって計算されたデータをテーブルに入力します。 通常の `SELECT` コマンドの場合のように、データはクライアントに返されません。 新しいテーブルの列には、`SELECT` コマンドの出力列に関連付けられた名前とデータ型があります。

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

標準の SELECT クエリパラメーターについて詳しくは、[SELECT クエリの節 &#x200B;](#select-queries) を参照してください。 このセクションには、`SELECT INTO` コマンド専用のパラメータのみがリストされます。

| パラメーター | 説明 |
| ------ | ------ |
| `TEMPORARY` または `TEMP` | オプションのパラメーター。 パラメーターを指定した場合、作成されたテーブルは一時テーブルになります。 |
| `UNLOGGED` | オプションのパラメーター。 パラメーターを指定した場合、作成されたテーブルはログのないテーブルになります。 ログに記録されていないテーブルについて詳しくは、[[!DNL PostgreSQL]  ドキュメント &#x200B;](https://www.postgresql.org/docs/current/sql-createtable.html) を参照してください。 |
| `new_table` | 作成するテーブルの名前。 |

**例**

次のクエリは、テーブル `films` の最近のエントリのみで構成される新しいテーブル `films_recent` を作成します。

```sql
SELECT * INTO films_recent FROM films WHERE date_prod >= '2002-01-01';
```

### SHOW

`SHOW` コマンドは、ランタイム パラメーターの現在の設定を表示します。 これらの変数を設定するには、`SET` 文を使用するか、`postgresql.conf` 設定ファイルを編集するか、`PGOPTIONS` 環境変数（libpq あるいは libpq ベースのアプリケーションを使用する場合）、または Postgres サーバ起動時のコマンドラインフラグを使用します。

```sql
SHOW name
SHOW ALL
```

| パラメーター | 説明 |
| ------ | ------ |
| `name` | 情報が必要なランタイムパラメーターの名前。 ランタイムパラメーターには、次の値を使用できます。<br>`SERVER_VERSION`：このパラメーターは、サーバーのバージョン番号を表示します。<br>`SERVER_ENCODING`：このパラメーターは、サーバーサイドの文字セットエンコーディングを示します。<br>`LC_COLLATE`：このパラメーターは、照合用のデータベースのロケール設定を示します（テキストの順序）。<br>`LC_CTYPE`：このパラメーターは、文字分類に対するデータベースのロケール設定を示します。<br>`IS_SUPERUSER`：このパラメーターは、現在の役割がスーパーユーザー権限を持っているかどうかを示します。 |
| `ALL` | すべての設定パラメーターの値と説明を表示します。 |

**例**

次のクエリは、パラメーター `DateStyle` の現在の設定を示しています。

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

`COPY` コマンドは、`SELECT` クエリの出力を指定した場所に複製します。 このコマンドを実行するには、ユーザーがこの場所にアクセスできる必要があります。

```sql
COPY query
    TO '%scratch_space%/folder_location'
    [  WITH FORMAT 'format_name']
```

| パラメーター | 説明 |
| ------ | ------ |
| `query` | コピーするクエリ。 |
| `format_name` | クエリのコピー先の形式。 `format_name` は、`parquet`、`csv`、`json` のいずれかです。 デフォルト値は `parquet` です。 |

>[!NOTE]
>
>完全な出力パスは `adl://<ADLS_URI>/users/<USER_ID>/acp_foundation_queryService/folder_location/<QUERY_ID>`

### テーブルの変更 {#alter-table}

`ALTER TABLE` コマンドを使用すると、プライマリ・キー制約または外部キー制約を追加または削除したり、テーブルにカラムを追加したりできます。

#### 制約を追加またはドロップ

次の SQL クエリは、テーブルに制約を追加または削除する例を示しています。 プライマリキーと外部キーの制約は、コンマ区切り値で複数の列に追加できます。 以下の例に示すように、2 つ以上の列名の値を渡すことによって、複合キーを作成できます。

**プライマリキーまたは複合キーの定義**

```sql
ALTER TABLE table_name ADD CONSTRAINT PRIMARY KEY ( column_name ) NAMESPACE namespace

ALTER TABLE table_name ADD CONSTRAINT PRIMARY KEY ( column_name1, column_name2 ) NAMESPACE namespace
```

**1 つ以上のキーに基づいてテーブル間の関係を定義する**

```sql
ALTER TABLE table_name ADD CONSTRAINT FOREIGN KEY ( column_name ) REFERENCES referenced_table_name ( primary_column_name )

ALTER TABLE table_name ADD CONSTRAINT FOREIGN KEY ( column_name1, column_name2 ) REFERENCES referenced_table_name ( primary_column_name1, primary_column_name2 )
```

**ID 列の定義**

```sql
ALTER TABLE table_name ADD CONSTRAINT PRIMARY IDENTITY ( column_name ) NAMESPACE namespace

ALTER TABLE table_name ADD CONSTRAINT IDENTITY ( column_name ) NAMESPACE namespace
```

**制約/関係/ID のドロップ**

```sql
ALTER TABLE table_name DROP CONSTRAINT PRIMARY KEY ( column_name )

ALTER TABLE table_name DROP CONSTRAINT PRIMARY KEY ( column_name1, column_name2 )

ALTER TABLE table_name DROP CONSTRAINT FOREIGN KEY ( column_name )

ALTER TABLE table_name DROP CONSTRAINT FOREIGN KEY ( column_name1, column_name2 )

ALTER TABLE table_name DROP CONSTRAINT PRIMARY IDENTITY ( column_name )

ALTER TABLE table_name DROP CONSTRAINT IDENTITY ( column_name )
```

| パラメーター | 説明 |
| ------ | ------ |
| `table_name` | 編集するテーブルの名前。 |
| `column_name` | 制約を追加する列の名前。 |
| `referenced_table_name` | 外部キーによって参照されるテーブルの名前。 |
| `primary_column_name` | 外部キーによって参照されるカラムの名前。 |

>[!NOTE]
>
>テーブルスキーマは一意で、複数のテーブル間で共有しないでください。 また、名前空間は、プライマリキー、プライマリ ID および ID 制約に必須です。

#### プライマリ ID とセカンダリ ID の追加またはドロップ

プライマリ ID テーブル列とセカンダリ ID テーブル列の両方の制約を追加または削除するには、`ALTER TABLE` コマンドを使用します。

次の例では、制約を追加してプライマリ ID とセカンダリ ID を追加します。

```sql
ALTER TABLE t1 ADD CONSTRAINT PRIMARY IDENTITY (id) NAMESPACE 'IDFA';
ALTER TABLE t1 ADD CONSTRAINT IDENTITY(id) NAMESPACE 'IDFA';
```

以下の例に示すように、制約を削除して ID を削除することもできます。

```sql
ALTER TABLE t1 DROP CONSTRAINT PRIMARY IDENTITY (c1) ;
ALTER TABLE t1 DROP CONSTRAINT IDENTITY (c1) ;
```

詳しくは、[&#x200B; アドホックデータセットでの ID の設定 &#x200B;](../data-governance/ad-hoc-schema-identities.md) のドキュメントを参照してください。

#### 列を追加

次の SQL クエリは、テーブルに列を追加する例を示しています。

```sql
ALTER TABLE table_name ADD COLUMN column_name data_type

ALTER TABLE table_name ADD COLUMN column_name_1 data_type1, column_name_2 data_type2 
```

##### サポートされるデータタイプ

次の表に、Azure SQL の [!DNL Postgres SQL]、XDM、[!DNL Accelerated Database Recovery] （ADR）を使用してテーブルに列を追加するために許可されるデータタイプを示します。

| --- | PSQL クライアント | XDM | AD | 説明 |
|---|---|---|---|---|
| 1 | `bigint` | `int8` | `bigint` | 大きな整数を格納するために使用される数値データ型で、-9,223,372,036,854,775,807 ～ 9,223,372,036,854,775,807 の範囲は 8 バイトです。 |
| 2 | `integer` | `int4` | `integer` | 4 バイト単位の–2,147,483,648 から 2,147,483,647 までの整数を格納するために使用される数値データ型。 |
| 3 | `smallint` | `int2` | `smallint` | 整数を格納するために使用される数値データ型で、-32,768 ～ 215-1 32,767 の範囲は 2 バイトです。 |
| 4 | `tinyint` | `int1` | `tinyint` | 1 バイトの 0 ～ 255 の整数を格納するために使用される数値データ型。 |
| 5 | `varchar(len)` | `string` | `varchar(len)` | 可変サイズの文字データタイプ。 `varchar` は、列データエントリのサイズが大幅に異なる場合に最適です。 |
| 6 | `double` | `float8` | `double precision` | `FLOAT8` および `FLOAT` は、`DOUBLE PRECISION` の有効な同義語です。 `double precision` は浮動小数点データ型です。 浮動小数点値は 8 バイトで格納されます。 |
| 7 | `double precision` | `float8` | `double precision` | `FLOAT8` は `double precision` の有効な同義語です。`double precision` は浮動小数点データ型です。 浮動小数点値は 8 バイトで格納されます。 |
| 8 | `date` | `date` | `date` | `date` のデータタイプは、タイムスタンプ情報を含まない 4 バイトの格納されたカレンダー日付値です。 有効な日付の範囲は、01-01-0001 ～ 12-31-9999 です。 |
| 9 | `datetime` | `datetime` | `datetime` | ある時点を保存するために使用されるデータタイプで、カレンダーの日付および時刻として表されます。 `datetime` には、年、月、日、時間、秒、分数の修飾子が含まれます。 `datetime` 宣言には、これらの時間単位のサブセットを含めることができ、この順序で結合することも、1 つの時間単位のみで構成することもできます。 |
| 10 | `char(len)` | `string` | `char(len)` | `char(len)` キーワードは、項目が固定長の文字であることを示すために使用されます。 |

#### スキーマを追加

次の SQL クエリは、データベース/スキーマにテーブルを追加する例を示しています。

```sql
ALTER TABLE table_name ADD SCHEMA database_name.schema_name
```

>[!NOTE]
>
> ADLS テーブルおよびビューは、DWH データベース / スキーマに追加できません。


#### スキーマを削除

次の SQL クエリは、データベース/スキーマからテーブルを削除する例を示しています。

```sql
ALTER TABLE table_name REMOVE SCHEMA database_name.schema_name
```

>[!NOTE]
>
> DWH テーブルとビューは、物理的にリンクされた DWH データベース/スキーマから削除することはできません。


**パラメーター**

| パラメーター | 説明 |
| ------ | ------ |
| `table_name` | 編集するテーブルの名前。 |
| `column_name` | 追加する列の名前。 |
| `data_type` | 追加する列のデータタイプ。 サポートされるデータタイプは、bigint、char、string、date、datetime、double、double precision、integer、smallint、tinyint、varchar です。 |

### プライマリキーを表示

`SHOW PRIMARY KEYS` コマンドは、指定されたデータベースのすべてのプライマリ・キー制約を一覧表示します。

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

`SHOW FOREIGN KEYS` コマンドは、指定されたデータベースのすべての外部キー制約を一覧表示します。

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

`SHOW DATAGROUPS` コマンドは、関連するすべてのデータベースのテーブルを返します。 データベースごとに、スキーマ、グループタイプ、子タイプ、子名、子 ID がテーブルに表示されます。

```sql
SHOW DATAGROUPS
```

```console
   Database   |      Schema       | GroupType |      ChildType       |                     ChildName                       |               ChildId
  -------------+-------------------+-----------+----------------------+----------------------------------------------------+--------------------------------------
   adls_db     | adls_scheema      | ADLS      | Data Lake Table      | adls_table1                                        | 6149ff6e45cfa318a76ba6d3
   adls_db     | adls_scheema      | ADLS      | Accelerated Store | _table_demo1                                       | 22df56cf-0790-4034-bd54-d26d55ca6b21
   adls_db     | adls_scheema      | ADLS      | View                 | adls_view1                                         | c2e7ddac-d41c-40c5-a7dd-acd41c80c5e9
   adls_db     | adls_scheema      | ADLS      | View                 | adls_view4                                         | b280c564-df7e-405f-80c5-64df7ea05fc3
```


### テーブルのデータグループを表示

`SHOW DATAGROUPS FOR 'table_name'` コマンドは、パラメーターを子として含む、関連付けられたすべてのデータベースのテーブルを返します。 データベースごとに、スキーマ、グループタイプ、子タイプ、子名、子 ID がテーブルに表示されます。

```sql
SHOW DATAGROUPS FOR 'table_name'
```

**パラメーター**

- `table_name`：関連付けられたデータベースを検索する表の名前。

```console
   Database   |      Schema       | GroupType |      ChildType       |                     ChildName                      |               ChildId
  -------------+-------------------+-----------+----------------------+----------------------------------------------------+--------------------------------------
   dwh_db_demo | schema2           | QSACCEL   | Accelerated Store | _table_demo2                                       | d270f704-0a65-4f0f-b3e6-cb535eb0c8ce
   dwh_db_demo | schema1           | QSACCEL   | Accelerated Store | _table_demo2                                       | d270f704-0a65-4f0f-b3e6-cb535eb0c8ce
   qsaccel     | profile_aggs      | QSACCEL   | Accelerated Store | _table_demo2                                       | d270f704-0a65-4f0f-b3e6-cb535eb0c8ce
```
