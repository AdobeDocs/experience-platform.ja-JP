---
title: Working with nested data structures in Query Service
description: このドキュメントでは、CTAS および INSERT INTO 文を使用してネストされたデータフィールドを処理および変換する作業例を示します。
source-git-commit: 838ee939a8438c2f09ff64044c129e20c37ea01a
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 2%

---

# クエリサービスでのネストされたデータ構造の使用

Adobe Experience Platform Query Service supports the use of nested data fields. The complexity of enterprise data structures can make transforming or processing this data complicated. このドキュメントでは、ネストされたデータ構造を含む複雑なデータ型のデータセットを作成、処理、変換する方法の例を示します。

クエリサービスは、PostgreSQL インターフェイスを提供し、Experience Platformが管理するすべてのデータセットに対して SQL クエリを実行します。 Platform では、構造体、配列、マップ、深くネストされた構造体、配列、マップなどのテーブル列で、プリミティブデータ型または複雑なデータ型の使用がサポートされています。 Datasets can also contain nested structures where the column data type can be as complex as an array of nested structures, or a map of maps wherein the value of a key-value pair can be a structure with multiple levels of nesting.

## はじめに

このチュートリアルでは、サードパーティの PSQL クライアントまたはクエリエディターツールを使用して、Experience Platformユーザーインターフェイス (UI) 内でクエリの書き込み、検証、実行をおこなう必要があります。 Full details on how to run queries through the UI can be found in the [Query Editor UI guide](../ui/user-guide.md). サードパーティのデスクトップクライアントがクエリサービスに接続できる詳細なリストについては、 [クライアント接続の概要](../clients/overview.md).

You should also have a good understanding of the `INSERT INTO` and `CTAS` syntax. 使用に関する具体的な情報については、 [`INSERT INTO`](../sql/syntax.md#insert-into) および [`CTAS`](../sql/syntax.md#create-table-as-select) セクション [SQL 構文リファレンスドキュメント](../sql/syntax.md).

## データセットの作成

クエリサービスは、「テーブルを選択として作成」(`CTAS`) 機能を使用して、 `SELECT` 文、またはこの場合と同様に、Adobe Experience Platformの既存の XDM スキーマへの参照を使用します。 Displayed below is the XDM schema for `Final_subscription` created for this example.

![final_subscription スキーマの図。](../images/best-practices/final-subscription-schema.png)

The following example demonstrates the SQL used to create the `final_subscription_test2` dataset. `final_subscription_test2` は `Final_subscription` スキーマ。 Data is extracted from the source using a `SELECT` clause to populate some rows.

```sql
CREATE TABLE final_subscription_test2 with(schema='Final_subscription') AS (
        SELECT struct(userid, collect_set(subscription) AS subscription) AS _lumaservices3 FROM(
            SELECT user AS userid,
                   struct( last(eventtime) AS last_eventtime,
                           last(status) AS last_status,
                           offer_id, 
                           subsid AS subscription_id)
                   AS subscription
             FROM (
                   SELECT _lumaservices3.msftidentities.userid user
                        , _lumaservices3.subscription.subscription_id subsid
                        , _lumaservices3.subscription.subscription_status status
                        , _lumaservices3.subscription.offer_id offer_id
                        , TIMESTAMP eventtime
 
                   FROM
                        xbox_subscription_event
                   UNION   
                   SELECT _lumaservices3.msftidentities.userid user
                        , _lumaservices3.subscription.subscription_id subsid
                        , _lumaservices3.subscription.subscription_status status
                        , _lumaservices3.subscription.offer_id offer_id
                        , TIMESTAMP eventtime
                   FROM
                        office365_subscription_event
             ) 
             GROUP BY user,subsid,offer_id
             ORDER BY user ASC
       ) GROUP BY userid)
```

最初のデータセット内 `final_subscription_test2`の場合、構造体データ型は、 `subscription` フィールドと `userid` 各ユーザーに固有の この `subscription` 「 」フィールドは、ユーザーの製品購読を表します。 複数の購読を設定できますが、テーブルには 1 行につき 1 つの購読の情報のみを含めることができます。

## INSERT INTO を使用して、ネストされたデータフィールドを更新します

After the `final_subscription_test2` dataset has been created, the `INSERT INTO` statement is used to append additional data to the table. When copying data, the data types in source and target must match. または、ソースデータタイプが `CAST` をターゲットデータ型に追加します。 次に、次の SQL を使用して、増分データをターゲットデータセットに追加します。

```sql
INSERT INTO final_subscription_test
      SELECT struct(userid, collect_set(subscription) AS subscription) AS _lumaservices3 FROM(
            SELECT user AS userid,
                   struct( last(eventtime) AS last_eventtime,
                           last(status) AS last_status,
                           offer_id, 
                           subsid AS subscription_id)
                   AS subscription
             FROM  SELECT _lumaservices3.msftidentities.userid user
                        , _lumaservices3.subscription.subscription_id subsid
                        , _lumaservices3.subscription.subscription_status status
                        , _lumaservices3.subscription.offer_id offer_id
                        , TIMESTAMP eventtime
 
                   FROM
                        xbox_subscription_event
                   UNION   
                   SELECT _lumaservices3.msftidentities.userid user
                        , _lumaservices3.subscription.subscription_id subsid
                        , _lumaservices3.subscription.subscription_status status
                        , _lumaservices3.subscription.offer_id offer_id
                        , timestamp eventtime
                   FROM
                        office365_subscription_event
             ) 
             GROUP BY user,subsid,offer_id
             ORDER BY user ASC
       ) GROUP BY userid)
```

## ネストされたデータセットからデータを処理する

To find out the list of a user&#39;s active subscriptions from a dataset, you must write a query that separates the elements of an array into multiple rows and columns. これをおこなうには、まず、サブスクリプション情報がデータセット内にネストされた配列内に保持されるので、データモデルの形状を理解する必要があります。

PSQL `\d` コマンドを使用して、必要なサブスクリプションデータにレベルごとに移動します。 次の表に、 `final_subscription_test2` データセット。 Complex data types can be recognized at a glance because they are not typical type values such as text, boolean, timestamp, etc.

| 列 | タイプ |
|--------|-------|
| `_lumaservices3` | final_subscription_test2__lumaservices3 |

次の列のフィールドは、 `\d final_subscription_test2__lumaservices3` コマンドを使用します。

| 列 | タイプ |
|---------|-------|
| `userid` | テキスト |
| `subscription` | _lumaservices3_subscription_e[] |

`subscription` is an array of struct elements. フィールドは、 `\d _lumaservices3_subscription_e[]` コマンドを使用します。

| 列 | タイプ |
|---------|-------|
| `last_eventtime` | timestamp |
| `last_status` | テキスト |
| `offer_id` | テキスト |
| `subscription_id` | テキスト |

購読のネストされたフィールドに対してクエリを実行するには、まず、 `subscription` 配列を複数の行に分割し、explode 関数を使用して結果を返します。 次の SQL の例では、ユーザーのアクティブな購読を `userid`.

```sql
SELECT userid, subs AS active_subscription FROM (
    SELECT _lumaservices3.userid AS userid, explode(_lumaservices3.subscription) AS subs 
    FROM final_subscription_test2
)
WHERE subs.last_status='Active';
```

このシンプル化されたサンプルソリューションでは、1 人のアクティブなユーザーサブスクリプションのみを使用できます。 現実的には、1 人のユーザーに対して多数のアクティブな購読が存在する可能性があります。 The following example modifies the previous query to allow for multiple simultaneous active subscriptions.

```sql
SELECT userid, collect_list(subs) AS active_subscriptions FROM (
     SELECT
          _lumaservices3.userid AS userid,
          explode(_lumaservices3.subscription) AS subs
     FROM final_subscription_test2
     )
WHERE subs.last_status='Active' 
GROUP BY userid ;
```

この SQL の例はますます複雑になっていますが、 `collect_list` アクティブなサブスクリプションでは、出力がソースと同じ順序になるとは限りません。 ユーザーのアクティブな購読のリストを作成するには、GROUP BY を使用するか、シャッフリングを使用してリストの結果を集計する必要があります。

## 次の手順

このドキュメントでは、Adobe Experience Platformクエリサービスで複雑なデータ型を使用するデータセットを処理または変換する方法について説明します。 Please see the [queries execution guidance](./writing-queries.md) for more information about running SQL queries on datasets within the Data Lake.
