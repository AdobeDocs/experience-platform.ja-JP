---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；SQL 構文；SQL;CTAS;CTAS；選択としてテーブルを作成
solution: Experience Platform
title: クエリサービスの SQL 構文
description: このドキュメントでは、Adobe Experience Platformクエリサービスでサポートされる SQL 構文を示します。
exl-id: 2bd4cc20-e663-4aaa-8862-a51fde1596cc
source-git-commit: b94536be6e92354e237b99d36af13adf5a49afa7
workflow-type: tm+mt
source-wordcount: '3863'
ht-degree: 8%

---

# クエリサービスの SQL 構文

Adobe Experience Platformクエリサービスは、 `SELECT` ステートメントおよびその他の制限付きコマンド。 このドキュメントでは、 [!DNL Query Service].

## SELECT クエリ {#select-queries}

次の構文は、 `SELECT` 次をサポートするクエリ： [!DNL Query Service]:

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

この句は、スナップショット ID に基づいてテーブルのデータを増分的に読み取るために使用できます。 スナップショット ID は、データが書き込まれるたびにデータレイクテーブルに適用される長整数型の数字で表されるチェックポイントマーカーです。 The `SNAPSHOT` 句は、その句の隣で使用されるテーブルの関係にそれ自体を付加します。

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

なお、 `SNAPSHOT` 句は、テーブルまたはテーブルのエイリアスに対して機能しますが、サブクエリまたはビューの上には機能しません。 A `SNAPSHOT` 条項はどこでも機能する `SELECT` テーブルのクエリを適用できます。

また、 `HEAD` および `TAIL` スナップショット句の特別なオフセット値として。 使用 `HEAD` は、最初のスナップショットの前のオフセットを指し、 `TAIL` は、最後のスナップショットの後のオフセットを指します。

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

### JOIN

A `SELECT` 結合を使用するクエリの構文は次のとおりです。

```sql
SELECT statement
FROM statement
[JOIN | INNER JOIN | LEFT JOIN | LEFT OUTER JOIN | RIGHT JOIN | RIGHT OUTER JOIN | FULL JOIN | FULL OUTER JOIN]
ON join condition
```

### UNION、INTERSECT、EXCEPT

The `UNION`, `INTERSECT`、および `EXCEPT` 句は、2 つ以上のテーブルから同様の行を結合または除外するために使用します。

```sql
SELECT statement 1
[UNION | UNION ALL | UNION DISTINCT | INTERSECT | EXCEPT | MINUS]
SELECT statement 2
```

### CREATE TABLE AS SELECT {#create-table-as-select}

次の構文は、 `CREATE TABLE AS SELECT` (CTAS) クエリ：

```sql
CREATE TABLE table_name [ WITH (schema='target_schema_title', rowvalidation='false', label='PROFILE') ] AS (select_query)
```

| パラメーター | 説明 |
| ----- | ----- |
| `schema` | XDM スキーマのタイトル。 この句は、CTAS クエリで作成された新しいデータセットに対して既存の XDM スキーマを使用する場合にのみ使用します。 |
| `rowvalidation` | （オプション）新しく作成したデータセットに対して取り込まれる新しいバッチの行レベルの検証を、すべてユーザーが必要とするかどうかを指定します。 デフォルト値は `true` です。 |
| `label` | CTAS クエリでデータセットを作成する場合、このラベルをの値と共に使用します。 `profile` をクリックし、データセットに対して「プロファイルを有効にする」というラベルを付けます。 つまり、データセットは作成時に自動的にプロファイル用にマークされます。 の使用について詳しくは、派生属性拡張機能のドキュメントを参照してください。 `label`. |
| `select_query` | A `SELECT` ステートメント。 の構文 `SELECT` クエリは、 [「SELECT queries」セクション](#select-queries). |

**例**

```sql
CREATE TABLE Chairs AS (SELECT color, count(*) AS no_of_chairs FROM Inventory i WHERE i.type=="chair" GROUP BY i.color)

CREATE TABLE Chairs WITH (schema='target schema title', label='PROFILE') AS (SELECT color, count(*) AS no_of_chairs FROM Inventory i WHERE i.type=="chair" GROUP BY i.color)

CREATE TABLE Chairs AS (SELECT color FROM Inventory SNAPSHOT SINCE 123)
```

>[!NOTE]
>
>`SELECT` 文には、`COUNT`、`SUM`、`MIN` などの集計関数のエイリアスが含まれている必要があります。また、 `SELECT` 文は () 括弧ありでもなしでも使用できます。 次の項目を指定できます。 `SNAPSHOT` 増分デルタをターゲットテーブルに読み込むための句。

## INSERT INTO

The `INSERT INTO` コマンドは次のように定義されます。

```sql
INSERT INTO table_name select_query
```

| パラメーター | 説明 |
| ----- | ----- |
| `table_name` | クエリを挿入するテーブルの名前。 |
| `select_query` | A `SELECT` ステートメント。 の構文 `SELECT` クエリは、 [「SELECT queries」セクション](#select-queries). |

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
> The `SELECT` 文 **を次の値に設定する** は () 括弧で囲まれます。 また、 `SELECT` 文は、 `INSERT INTO` ステートメント。 次の項目を指定できます。 `SNAPSHOT` 増分デルタをターゲットテーブルに読み込むための句。

実際の XDM スキーマ内のフィールドのほとんどは、ルートレベルで見つからず、SQL ではドット表記の使用は許可されていません。 ネストされたフィールドを使用して現実的な結果を得るには、各フィールドを `INSERT INTO` パス。

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

The `DROP TABLE` コマンドは、既存のテーブルを削除し、外部テーブルでない場合は、テーブルに関連付けられたディレクトリをファイルシステムから削除します。 テーブルが存在しない場合は、例外が発生します。

```sql
DROP TABLE [IF EXISTS] [db_name.]table_name
```

| パラメーター | 説明 |
| ------ | ------ |
| `IF EXISTS` | これを指定した場合、テーブルが **not** 存在する。 |

## データベースを作成

The `CREATE DATABASE` コマンドは ADLS データベースを作成します。

```sql
CREATE DATABASE [IF NOT EXISTS] db_name
```

## データベースを削除

The `DROP DATABASE` コマンドは、インスタンスからデータベースを削除します。

```sql
DROP DATABASE [IF EXISTS] db_name
```

| パラメーター | 説明 |
| ------ | ------ |
| `IF EXISTS` | これを指定した場合、データベースが **not** 存在する。 |

## スキーマをドロップ

The `DROP SCHEMA` コマンドは、既存のスキーマを削除します。

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
| `select_query` | A `SELECT` ステートメント。 の構文 `SELECT` クエリは、 [「SELECT queries」セクション](#select-queries). |

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

匿名ブロックは、実行可能セクションと例外処理セクションの 2 つのセクションで構成されます。 匿名ブロックでは、実行可能セクションは必須です。 ただし、例外処理セクションはオプションです。

次の例は、1 つ以上のステートメントを組み合わせて実行するブロックの作成方法を示しています。

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

匿名ブロックの使用例を以下に示します。

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

### JSON に自動 {#auto-to-json}

クエリサービスは、インタラクティブな SELECT クエリから最上位の複雑なフィールドを JSON 文字列として返す、オプションのセッションレベル設定をサポートしています。 The `auto_to_json` を設定すると、複雑なフィールドのデータを JSON として返し、標準ライブラリを使用して JSON オブジェクトに解析できます。

機能フラグを設定 `auto_to_json` を true に設定してから、複雑なフィールドを含む SELECT クエリを実行します。

```sql
set auto_to_json=true; 
```

#### を設定する前に `auto_to_json` フラグ

次の表は、 `auto_to_json` 設定が適用されます。 （以下に示すように）複雑なフィールドを持つテーブルをターゲットにするのと同じ SELECT クエリが、両方のシナリオで使用されました。

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

#### 設定後、 `auto_to_json` フラグ

次の表に、 `auto_to_json` の設定が結果のデータセットに適用されます。 両方のシナリオで同じ SELECT クエリが使用されました。

```console
                _id                |   receivedTimestamp   |       timestamp       |                                                                                                                   _experience                                                                                                                   |           application            |             commerce             |    dataSource    |                                                                  device                                                                   |                                                   endUserIDs                                                   |                                                                                                                                                                                           environment                                                                                                                                                                                            |                             identityMap                              |                                                                                            placeContext                                                                                            |      userActivityRegion      |                                                                                     web                                                                                      | _adcstageforpqs
-----------------------------------+-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------+----------------------------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------
 31892EE15DE00000-401D52664FF48A52 | 2022-09-02 19:47:14.0 | 2022-09-02 19:47:14.0 | {"analytics":{"customDimensions":{"eVars":{"eVar1":"1","eVar2":"1"},"props":{"prop1":"1","prop2":"1"}},"environment":{"browserID":-209479095,"browserIDStr":"4085488201","operatingSystemID":-2105158467,"operatingSystemIDStr":"2189808829"}}} | {"userPerspective":"background"} | {"order":{"currencyCode":"USD"}} | {"_id":"475341"} | {"colorDepth":32,"screenHeight":768,"screenWidth":1024,"typeID":"205202","typeIDService":"https://ns.adobe.com/xdm/external/deviceatlas"} | {"_experience":{"aaid":{"id":"31892EE080007B35-E6CE00000000000","namespace":{"code":"AAID"},"primary":true}}}  | {"browserDetails":{"acceptLanguage":"en-US","cookiesEnabled":false,"javaEnabled":false,"javaScriptEnabled":true,"javaScriptVersion":"1.6","userAgent":"Mozilla/5.0 (iPhone; U; CPU iPhone OS 4_1 like Mac OS X; ja-jp) AppleWebKit/532.9 (KHTML, like Gecko) Version/4.0.5 Mobile/8B117 Safari/6531.22.7","viewportHeight":490,"viewportWidth":1125},"domain":"xo.net","ipV4":"64.3.235.13"}     | {"AAID":[{"id":"31892EE080007B35-E6CE00000000000","primary":true}]}  | {"geo":{"_schema":{"latitude":34.01,"longitude":-84.0},"city":"lawrenceville","countryCode":"US","dmaID":524,"postalCode":"30043","stateProvince":"ga"},"localTimezoneOffset":600}                 | {"dataCenterLocation":"UT1"} | {"webPageDetails":{"isHomePage":false,"name":"Search Results","pageViews":{"value":1.0}},"webReferrer":{"URL":"http://www.google.com/search?ie=UTF-8&q=","type":"internal"}} |
 31892EE15DE00000-401B92664FF48AE8 | 2022-09-02 19:47:14.0 | 2022-09-02 19:47:14.0 | {"analytics":{"customDimensions":{"eVars":{"eVar1":"1","eVar2":"1"},"props":{"prop1":"1","prop2":"1"}},"environment":{"browserID":-209479095,"browserIDStr":"4085488201","operatingSystemID":-2105158467,"operatingSystemIDStr":"2189808829"}}} | {"userPerspective":"background"} | {"order":{"currencyCode":"USD"}} | {"_id":"475341"} | {"colorDepth":32,"screenHeight":768,"screenWidth":1024,"typeID":"205202","typeIDService":"https://ns.adobe.com/xdm/external/deviceatlas"} | {"_experience":{"aaid":{"id":"31892EE100007BF3-215FE00000000001","namespace":{"code":"AAID"},"primary":true}}} | {"browserDetails":{"acceptLanguage":"en-US","cookiesEnabled":false,"javaEnabled":false,"javaScriptEnabled":true,"javaScriptVersion":"1.5","userAgent":"Mozilla/5.0 (iPhone; U; CPU iPhone OS 4_1 like Mac OS X; ja-jp) AppleWebKit/532.9 (KHTML, like Gecko) Version/4.0.5 Mobile/8B117 Safari/6531.22.7","viewportHeight":768,"viewportWidth":556},"domain":"ntt.net","ipV4":"219.165.108.145"} | {"AAID":[{"id":"31892EE100007BF3-215FE00000000001","primary":true}]} | {"geo":{"_schema":{"latitude":34.989999999999995,"longitude":138.42},"city":"shizuoka","countryCode":"JP","dmaID":392005,"postalCode":"420-0812","stateProvince":"22"},"localTimezoneOffset":-240} | {"dataCenterLocation":"UT1"} | {"webPageDetails":{"isHomePage":false,"name":"Home - JJEsquire","pageViews":{"value":1.0}},"webReferrer":{"type":"typed_bookmarked"}}                                        |
(2 rows)
```

### 失敗時のフォールバックスナップショットの解決 {#resolve-fallback-snapshot-on-failure}

The `resolve_fallback_snapshot_on_failure` オプションは、期限切れのスナップショット ID の問題を解決するために使用されます。 スナップショットメタデータは 2 日後に期限切れになり、有効期限切れのスナップショットはスクリプトのロジックを無効にできます。 これは、匿名ブロックを使用する場合に問題になる可能性があります。

を設定します。 `resolve_fallback_snapshot_on_failure` オプションを true に設定すると、以前のスナップショット ID でスナップショットが上書きされます。

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

次のガイドを参照してください： [データアセットの論理的な編成](../best-practices/organize-data-assets.md) を参照してください。

## テーブルが存在します

The `table_exists` SQL コマンドは、現在システムにテーブルが存在するかどうかを確認するために使用します。 このコマンドは、ブール値を返します（テーブルが存在&#x200B;**する**&#x200B;場合は `true`、テーブルが存在&#x200B;**しない**&#x200B;場合は `false`）。

文を実行する前にテーブルが存在するかどうかを検証すると、 `table_exists` 機能を使用すると、匿名ブロックを書き込んで、 `CREATE` および `INSERT INTO` の使用例です。

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

The `inline` 関数は、構造体の配列の要素を分割し、値をテーブルに生成します。 これは、 `SELECT` リストまたは `LATERAL VIEW`.

The `inline` 関数 **できません** 他のジェネレータ関数がある選択リストに配置します。

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

![productListItems のスキーマ図。](../images/sql/productListItems.png)

**例**

```sql
select inline(productListItems) from source_dataset limit 10;
```

値は `source_dataset` を使用してターゲットテーブルにデータを入力します。

| SKU | _experience | quantity | priceTotal |
|---------------------|-----------------------------------|----------|--------------|
| product-id-1 | (&quot;(&quot;(&quot;(A,pass,B,NULL)&quot;)&quot;)&quot;)&quot; | 5 | 10.5 |
| product-id-5 | (&quot;(&quot;(&quot;(A, pass, B,NULL)&quot;)&quot;)&quot;)&quot; |          |              |
| product-id-2 | (&quot;(&quot;(&quot;(AF, C, D,NULL)&quot;)&quot;)&quot;)&quot; | 6 | 40 |
| product-id-4 | (&quot;(&quot;(&quot;(BM, pass, NA,NULL)&quot;)&quot;)&quot;)&quot; | 3 | 12 |

## [!DNL Spark] SQL コマンド

次のサブ節では、クエリサービスでサポートされる Spark SQL コマンドについて説明します。

### SET

The `SET` コマンドは、プロパティを設定し、既存のプロパティの値を返すか、既存のプロパティをすべてリストします。 既存プロパティのキーに値が指定された場合、古い値が上書きされます。

```sql
SET property_key = property_value
```

| パラメーター | 説明 |
| ------ | ------ |
| `property_key` | リストまたは変更するプロパティの名前。 |
| `property_value` | プロパティを設定する値です。 |

任意の設定の値を返すには、 `SET [property key]` 無しに `property_value`.

## [!DNL PostgreSQL] コマンド

以下のサブセクションでは、 [!DNL PostgreSQL] クエリサービスでサポートされるコマンド。

### テーブルを分析 {#analyze-table}

The `ANALYZE TABLE` コマンドは、名前付きのテーブル（複数可）の分布分析と統計計算を実行します。 の使用 `ANALYZE TABLE` データセットが [加速貯蔵](#compute-statistics-accelerated-store) または [データレイク](#compute-statistics-data-lake). 使用方法について詳しくは、それぞれの節を参照してください。

#### 高速ストアで統計を計算 {#compute-statistics-accelerated-store}

The `ANALYZE TABLE` コマンドは、高速ストア上のテーブルの統計を計算します。 統計は、高速ストアの特定のテーブルに対して実行された CTAS または ITAS クエリに対して計算されます。

**例**

```sql
ANALYZE TABLE <original_table_name>
```

以下は、 `ANALYZE TABLE` command:-

| 計算値 | 説明 |
|---|---|
| `field` | テーブル内の列の名前。 |
| `data-type` | 各列に指定できるデータのタイプ。 |
| `count` | このフィールドに null 以外の値を含む行の数。 |
| `distinct-count` | このフィールドのユニーク値またはユニーク値の数。 |
| `missing` | このフィールドに null 値を持つ行の数。 |
| `max` | 分析されたテーブルの最大値。 |
| `min` | 分析されたテーブルの最小値。 |
| `mean` | 分析されたテーブルの平均値。 |
| `stdev` | 分析されたテーブルの標準偏差です。 |

#### データレイクの統計を計算 {#compute-statistics-data-lake}

次の項目に関する列レベルの統計を計算できるようになりました。 [!DNL Azure Data Lake Storage] (ADLS) データセットと `COMPUTE STATISTICS` SQL コマンド。 データセット全体、データセットのサブセット、すべての列、または列のサブセットに関する列の統計を計算します。

`COMPUTE STATISTICS` は、 `ANALYZE TABLE` コマンドを使用します。 ただし、 `COMPUTE STATISTICS`, `FILTERCONTEXT`、および `FOR COLUMNS` 高速ストアテーブルでは、コマンドはサポートされません。 `ANALYZE TABLE` コマンドのこれらの拡張機能は、現在 ADLS テーブルでのみサポートされています。

**例**

```sql
ANALYZE TABLE tableName FILTERCONTEXT (timestamp >= to_timestamp('2023-04-01 00:00:00') and timestamp <= to_timestamp('2023-04-05 00:00:00')) COMPUTE STATISTICS  FOR COLUMNS (commerce, id, timestamp);
```

The `FILTER CONTEXT` コマンドは、指定されたフィルター条件に基づいて、データセットのサブセットに関する統計を計算します。 The `FOR COLUMNS` コマンドは、解析の対象となる特定の列を指定します。

>[!NOTE]
>
>The `Statistics ID` および生成された統計は、各セッションでのみ有効で、異なる PSQL セッションをまたいでアクセスすることはできません。<br><br>制限事項:<ul><li>配列またはマップのデータタイプでは、統計の生成はサポートされていません</li><li>計算済みの統計は次のとおりです **not** セッション間で保持されます。</li></ul><br><br>オプション:<br><ul><li>`skip_stats_for_complex_datatypes`</li></ul><br>デフォルトでは、フラグは true に設定されています。その結果、サポートされていないデータ型に対して統計がリクエストされた場合、エラーは発生しませんが、サポートされていないデータ型のフィールドは警告なくスキップされます。<br>サポートされていないデータ型に関する統計がリクエストされた場合にエラーに関する通知を有効にするには、次を使用します。 `SET skip_stats_for_complex_datatypes = false`.

コンソール出力は、次のように表示されます。

```console
|     Statistics ID      | 
| ---------------------- |
| adc_geometric_stats_1  |
(1 row)
```

その後、計算済みの統計を直接クエリするには、 `Statistics ID`. 以下の文の例を使用すると、 `Statistics ID` またはエイリアス名。 この機能の詳細については、 [エイリアス名ドキュメント](../essential-concepts/dataset-statistics.md#alias-name).

```sql
-- This statement gets the statistics generated for `alias adc_geometric_stats_1`.
SELECT * FROM adc_geometric_stats_1;
```

以下を使用します。 `SHOW STATISTICS` コマンドを使用して、セッションで生成されたすべての一時的な統計のメタデータを表示します。 このコマンドを使用すると、統計分析の範囲を絞り込むことができます。

```sql
SHOW STATISTICS;
```

SHOW STATISTICS の出力例を以下に示します。

```console
      statsId         |   tableName   | columnSet |         filterContext       |      timestamp
----------------------+---------------+-----------+-----------------------------+--------------------
adc_geometric_stats_1 | adc_geometric |   (age)   |                             | 25/06/2023 09:22:26
demo_table_stats_1    |  demo_table   |    (*)    |       ((age > 25))          | 25/06/2023 12:50:26
age_stats             | castedtitanic |   (age)   | ((age > 25) AND (age < 40)) | 25/06/2023 09:22:26
```

詳しくは、 [データセット統計ドキュメント](../essential-concepts/dataset-statistics.md) を参照してください。

#### 大さじ {#tablesample}

Adobe Experience Platform クエリサービスは、近似クエリ処理機能の一部としてサンプルデータセットを提供します。データセットのサンプルは、データセットに対する集計操作に正確な答えが必要ない場合に最も適しています。 この機能を使用すると、近似クエリを発行して近似回答を返すことで、大規模なデータセットに関するより効率的な探索的クエリを実行できます。

サンプルデータセットは、既存のサンプルから均一なランダムサンプルを使用して作成されます。 [!DNL Azure Data Lake Storage] (ADLS) データセット（元のレコードの割合のみを使用）。 データセットサンプル機能は、 `ANALYZE TABLE` コマンドを `TABLESAMPLE` および `SAMPLERATE` SQL コマンド。

次の例の 1 行目は、テーブルの 5%のサンプルを計算する方法を示しています。 2 行目は、テーブル内のデータのフィルター表示から 5%のサンプルを計算する方法を示しています。

**例**

```sql {line-numbers="true"}
ANALYZE TABLE tableName TABLESAMPLE SAMPLERATE 5;
ANALYZE TABLE tableName FILTERCONTEXT (timestamp >= to_timestamp('2023-01-01')) TABLESAMPLE SAMPLERATE 5:
```

詳しくは、 [データセットサンプルドキュメント](../essential-concepts/dataset-samples.md) を参照してください。

### BEGIN

The `BEGIN` コマンドを使用するか、または `BEGIN WORK` または `BEGIN TRANSACTION` コマンドを使用して、トランザクションブロックを開始します。 begin コマンドの後に入力された文は、明示的な COMMIT または ROLLBACK コマンドが指定されるまで、単一のトランザクションで実行されます。 このコマンドは、 `START TRANSACTION`.

```sql
BEGIN
BEGIN WORK
BEGIN TRANSACTION
```

### CLOSE

The `CLOSE` コマンドは、開いたカーソルに関連付けられたリソースを解放します。 カーソルを閉じた後の操作は許可されません。不要になったカーソルは閉じる必要があります。

```sql
CLOSE name
CLOSE ALL
```

次の場合 `CLOSE name` が使用されます。 `name` は、閉じる必要がある開いたカーソルの名前を表します。 次の場合 `CLOSE ALL` を使用すると、すべてのオープンカーソルが閉じられます。

### DEALLOCATE

The `DEALLOCATE` コマンドを使用すると、事前に準備された SQL 文の割り当てを解除できます。 準備された文の割り当てを明示的に解除しない場合は、セッションが終了すると割り当てが解除されます。準備済み文の詳細については、 [PREPARE コマンド](#prepare) 」セクションに入力します。

```sql
DEALLOCATE name
DEALLOCATE ALL
```

次の場合 `DEALLOCATE name` が使用されます。 `name` は、割り当てを解除する必要がある準備済み文の名前を表します。 次の場合 `DEALLOCATE ALL` を使用する場合、すべての準備済み文の割り当てが解除されます。

### DECLARE

The `DECLARE` コマンドを使用すると、カーソルを作成できます。これは、大きなクエリから少数の行を取得するために使用できます。 カーソルが作成された後、`FETCH` を使用して行がカーソルから取得されます。

```sql
DECLARE name CURSOR FOR query
```

| パラメーター | 説明 |
| ------ | ------ |
| `name` | 作成するカーソルの名前。 |
| `query` | A `SELECT` または `VALUES` コマンドを使用して、カーソルが返す行を指定します。 |

### EXECUTE

The `EXECUTE` コマンドは、事前に準備された文を実行するために使用されます。 準備済み文はセッション中にのみ存在するので、準備済み文は `PREPARE` 現在のセッションで前に実行された文。 準備済み文の使用に関する詳細については、 [`PREPARE` command](#prepare) 」セクションに入力します。

次の場合、 `PREPARE` 文を作成した文が一部のパラメーターを指定した場合は、互換性のある一連のパラメーターを `EXECUTE` ステートメント。 これらのパラメーターが渡されない場合、エラーが発生します。

```sql
EXECUTE name [ ( parameter ) ]
```

| パラメーター | 説明 |
| ------ | ------ |
| `name` | 実行する準備済み文の名前。 |
| `parameter` | 準備済み文のパラメーターの実際の値。 これは、準備済み文の作成時に決定された、このパラメーターのデータ型と互換性のある値を生成する式である必要があります。準備済み文に複数のパラメーターがある場合は、コンマで区切ります。 |

### EXPLAIN

The `EXPLAIN` コマンドは、指定された文の実行計画を表示します。 実行計画では、文で参照されるテーブルがスキャンされる方法が示されます。  複数のテーブルが参照されている場合は、各入力テーブルから必要な行を組み合わせるために使用される結合アルゴリズムが表示されます。

```sql
EXPLAIN statement
```

以下を使用します。 `FORMAT` キーワードを `EXPLAIN` コマンドを使用して、応答の形式を定義します。

```sql
EXPLAIN FORMAT { TEXT | JSON } statement
```

| パラメーター | 説明 |
| ------ | ------ |
| `FORMAT` | 以下を使用します。 `FORMAT` コマンドを使用して出力形式を指定します。 使用可能なオプションは次のとおりです。 `TEXT` または `JSON`. テキスト以外の出力には、テキスト出力形式と同じ情報が含まれますが、プログラムの解析が容易です。このパラメーターのデフォルトは `TEXT` です。 |
| `statement` | 任意 `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `VALUES`, `EXECUTE`, `DECLARE`, `CREATE TABLE AS`または `CREATE MATERIALIZED VIEW AS` ステートメントに含まれ、その実行プランを表示します。 |

>[!IMPORTANT]
>
>出力 `SELECT` で実行すると、ステートメントが破棄される可能性があります `EXPLAIN` キーワード。 この文の他の副作用は、通常どおり発生します。

**例**

次の例は、単一の `integer` 列と10000行：

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

The `FETCH` コマンドは、以前に作成したカーソルを使用して行を取得します。

```sql
FETCH num_of_rows [ IN | FROM ] cursor_name
```

| パラメーター | 説明 |
| ------ | ------ |
| `num_of_rows` | 取得する行の数。 |
| `cursor_name` | 情報を取得するカーソルの名前。 |

### PREPARE {#prepare}

The `PREPARE` コマンドを使用すると、準備済み文を作成できます。 準備済み文は、類似した SQL 文をテンプレート化するために使用できるサーバー側のオブジェクトです。

準備済み文はパラメーターを受け取ることができます。パラメーターは、文の実行時に文内で置き換えられる値です。 パラメーターは、準備済み文を使用する場合、$1、$2 などを使用して、位置で参照されます。

必要に応じて、パラメータのデータ型のリストを指定できます。 パラメーターのデータ型が一覧に表示されない場合は、その型をコンテキストから推論できます。

```sql
PREPARE name [ ( data_type [, ...] ) ] AS SELECT
```

| パラメーター | 説明 |
| ------ | ------ |
| `name` | 準備済み文の名前。 |
| `data_type` | 準備済み文のパラメーターのデータ型。 パラメーターのデータ型が一覧に表示されない場合は、その型をコンテキストから推論できます。 複数のデータ型を追加する必要がある場合は、コンマ区切りのリストで追加できます。 |

### ROLLBACK

The `ROLLBACK` コマンドは、現在のトランザクションを元に戻し、トランザクションが行ったすべての更新を破棄します。

```sql
ROLLBACK
ROLLBACK WORK
```

### SELECT INTO

The `SELECT INTO` コマンドは、新しいテーブルを作成し、クエリで計算されたデータで埋めます。 通常の `SELECT` コマンドを使用します。 新しいテーブルの列には、 `SELECT` コマンドを使用します。

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
| `UNLOGGED` | オプションのパラメーター。 指定した場合、として作成されたテーブルはログなしのテーブルになります。 ログが記録されていないテーブルの詳細については、 [[!DNL PostgreSQL] ドキュメント](https://www.postgresql.org/docs/current/sql-createtable.html). |
| `new_table` | 作成するテーブルの名前。 |

**例**

次のクエリは、新しいテーブルを作成します `films_recent` テーブルの最近のエントリのみから成る `films`:

```sql
SELECT * INTO films_recent FROM films WHERE date_prod >= '2002-01-01';
```

### SHOW

The `SHOW` コマンドは、ランタイムパラメータの現在の設定を表示します。 これらの変数は、 `SET` ステートメントを編集する `postgresql.conf` 設定ファイルを使用する場合は、 `PGOPTIONS` 環境変数（libpq または libpq ベースのアプリケーションを使用する場合）、または Postgres サーバーの起動時にコマンドラインフラグを使用します。

```sql
SHOW name
SHOW ALL
```

| パラメーター | 説明 |
| ------ | ------ |
| `name` | 情報が必要なランタイムパラメーターの名前。 ランタイムパラメーターに指定できる値は次のとおりです。<br>`SERVER_VERSION`：このパラメーターは、サーバーのバージョン番号を表示します。<br>`SERVER_ENCODING`：このパラメーターは、サーバー側の文字セットエンコーディングを表示します。<br>`LC_COLLATE`：このパラメーターは、照合（テキストの順序付け）のためのデータベースのロケール設定を表示します。<br>`LC_CTYPE`：このパラメーターは、文字分類に関するデータベースのロケール設定を表示します。<br>`IS_SUPERUSER`：このパラメーターは、現在の役割にスーパーユーザー権限があるかどうかを示します。 |
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

The `COPY` コマンドは、 `SELECT` クエリを指定した場所に送信します。 このコマンドを正常に実行するには、ユーザーがこの場所にアクセスできる必要があります。

```sql
COPY query
    TO '%scratch_space%/folder_location'
    [  WITH FORMAT 'format_name']
```

| パラメーター | 説明 |
| ------ | ------ |
| `query` | コピーするクエリ。 |
| `format_name` | クエリをコピーする形式。 The `format_name` 次のいずれかを指定できます。 `parquet`, `csv`または `json`. デフォルトでは、値は `parquet`. |

>[!NOTE]
>
>完全な出力パスは次のようになります。 `adl://<ADLS_URI>/users/<USER_ID>/acp_foundation_queryService/folder_location/<QUERY_ID>`

### ALTER TABLE {#alter-table}

The `ALTER TABLE` コマンドを使用すると、プライマリキーまたは外部キーの制約を追加または削除したり、テーブルに列を追加したりできます。


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

The `ALTER TABLE` コマンドを使用すると、プライマリ ID テーブル列とセカンダリ ID テーブル列の両方に対する制約を SQL を介して直接追加または削除できます。

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

次のドキュメントを参照してください： [アドホックデータセットで ID を設定する](../data-governance/ad-hoc-schema-identities.md) を参照してください。

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
| 7 | `double precision` | `float8` | `double precision` | `FLOAT8` は、 `double precision`.`double precision` は浮動小数点データ型です。 浮動小数点値は 8 バイトで格納されます。 |
| 8 | `date` | `date` | `date` | The `date` データタイプは、タイムスタンプ情報のない 4 バイトで保存されるカレンダー日付値です。 有効な日付の範囲は01-01-0001 ～ 12-31-9999です。 |
| 9 | `datetime` | `datetime` | `datetime` | インスタントを時刻で保存するために使用されるデータ型で、カレンダーの日時として表されます。 `datetime` には、年、月、日、時間、秒、分数の区切り記号が含まれます。 A `datetime` 宣言には、これらの時間単位のサブセットを含めることができます。この時間単位は、その順序で結合される場合も、単一の時間単位で構成される場合もあります。 |
| 10 | `char(len)` | `string` | `char(len)` | The `char(len)` キーワードは、項目が固定長の文字であることを示すために使用されます。 |

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
| `data_type` | 追加する列のデータ型です。 サポートされるデータタイプは、bigint、char、string、date、datetime、double、double precision、integer、smallint、tinyint、varchar です。 |

### プライマリキーを表示

The `SHOW PRIMARY KEYS` コマンドは、指定したデータベースのすべての主キー制約をリストします。

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

The `SHOW FOREIGN KEYS` コマンドは、指定されたデータベースのすべての外部キー制約をリストします。

```sql
SHOW FOREIGN KEYS
```

```console
    tableName   |     columnName      | datatype | referencedTableName | referencedColumnName | namespace 
------------------+---------------------+----------+---------------------+----------------------+-----------
 table_name_1   | column_name1        | text     | table_name_3        | column_name3         |  "ECID"
 table_name_2   | column_name2        | text     | table_name_4        | column_name4         |  "AAID"
```


### DATAGROUPS を表示

The `SHOW DATAGROUPS` コマンドは、関連するすべてのデータベースのテーブルを返します。 各データベースについて、テーブルにはスキーマ、グループタイプ、子タイプ、子名、子 ID が含まれます。

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

The `SHOW DATAGROUPS FOR` &#39;table_name&#39;コマンドは、パラメータを子として含むすべての関連データベースのテーブルを返します。 各データベースについて、テーブルにはスキーマ、グループタイプ、子タイプ、子名、子 ID が含まれます。

```sql
SHOW DATAGROUPS FOR 'table_name'
```

**パラメーター**

- `table_name`：関連付けられたデータベースを検索するテーブルの名前。

```console
   Database   |      Schema       | GroupType |      ChildType       |                     ChildName                      |               ChildId
  -------------+-------------------+-----------+----------------------+----------------------------------------------------+--------------------------------------
   dwh_db_demo | schema2           | QSACCEL   | Data Warehouse Table | _table_demo2                                       | d270f704-0a65-4f0f-b3e6-cb535eb0c8ce
   dwh_db_demo | schema1           | QSACCEL   | Data Warehouse Table | _table_demo2                                       | d270f704-0a65-4f0f-b3e6-cb535eb0c8ce
   qsaccel     | profile_aggs      | QSACCEL   | Data Warehouse Table | _table_demo2                                       | d270f704-0a65-4f0f-b3e6-cb535eb0c8ce
```
