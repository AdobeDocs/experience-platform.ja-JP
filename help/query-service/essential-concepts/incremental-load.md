---
title: クエリサービスでの増分読み込み
description: 増分読み込み機能は、匿名ブロックとスナップショットの両方の機能を使用して、データレイクからデータウェアハウスにデータを移動する際に、一致するデータを無視する、ほぼリアルタイムのソリューションを提供します。
exl-id: 1418d041-29ce-4153-90bf-06bd8da8fb78
source-git-commit: cde7c99291ec34be811ecf3c85d12fad09bcc373
workflow-type: tm+mt
source-wordcount: '688'
ht-degree: 2%

---

# クエリサービスでの増分読み込み

増分読み込み設計パターンは、データを管理するためのソリューションです。 パターンは、前回の読み込みの実行以降に作成または変更されたデータセット内の情報のみを処理します。

増分読み込みでは、匿名ブロックやスナップショットなど、Adobe Experience Platformクエリサービスが提供する様々な機能を使用します。 この設計パターンにより、ソースから既に処理されたデータがスキップされるので、処理効率が向上します。 ストリーミングデータ処理とバッチデータ処理の両方で使用できます。

このドキュメントでは、増分処理のためのデザインパターンを構築するための一連の手順を示します。 これらの手順は、独自の増分データ読み込みクエリを作成するテンプレートとして使用できます。

## はじめに

このドキュメント全体の SQL の例では、匿名ブロックとスナップショットの機能について理解している必要があります。 次を読むことをお勧めします。 [匿名ブロッククエリの例](./anonymous-block.md) ドキュメントおよび [スナップショット句](../sql/syntax.md#snapshot-clause) ドキュメント。

このガイドで使用される用語のガイダンスについては、 [SQL 構文ガイド](../sql/syntax.md).

## データを増分的に読み込む

次の手順では、スナップショットと匿名ブロック機能を使用して、データを作成し増分的に読み込む方法を示します。 デザインパターンは、独自のクエリシーケンスのテンプレートとして使用できます。

1. a `checkpoint_log` データを正常に処理するために使用された最新のスナップショットを追跡するテーブル。 トラッキングテーブル (`checkpoint_log` （この例では）を最初に初期化して、 `null` データセットを増分的に処理するために使用します。

```SQL
DROP TABLE IF EXISTS checkpoint_log;
CREATE TABLE  checkpoint_log AS
SELECT
   cast(NULL AS string) process_name,
   cast(NULL AS string) process_status,
   cast(NULL AS string) last_snapshot_id,
   cast(NULL AS TIMESTAMP) process_timestamp
   WHERE false;
```

2. `checkpoint_log` 増分処理が必要なデータセットの空のレコードが 1 つあるテーブル。 `DIM_TABLE_ABC` は、次の例で処理されるデータセットです。 処理を初めておこなう場合 `DIM_TABLE_ABC`、 `last_snapshot_id` は次のように初期化されます： `null`. これにより、最初にデータセット全体を処理し、その後段階的に処理できます。

```SQL
INSERT INTO
   checkpoint_log
   SELECT
       'DIM_TABLE_ABC' process_name,
       'SUCCESSFUL' process_status,
       cast(NULL AS string) last_snapshot_id,
       CURRENT_TIMESTAMP process_timestamp;
```

3 次、初期化 `DIM_TABLE_ABC_Incremental` 処理済み出力を含む `DIM_TABLE_ABC`. 匿名ブロック ( **必須** 以下の SQL の例の実行の節では、手順 1 ～ 4 で説明していますが、データを増分的に処理するために、順番に実行されます。

1. を `from_snapshot_id` 処理の開始位置を示します。 この `from_snapshot_id` 例では、 `checkpoint_log` ～で使用するテーブル `DIM_TABLE_ABC`. 最初の実行時に、スナップショット ID は次のようになります。 `null` つまり、データセット全体が処理されます。
2. を `to_snapshot_id` ソーステーブルの現在のスナップショット ID(`DIM_TABLE_ABC`) をクリックします。 この例では、ソーステーブルの metadata テーブルからクエリが実行されます。
3. 以下を使用： `CREATE` 作成するキーワード `DIM_TABLE_ABC_Incremenal` を宛先テーブルとして使用します。 宛先テーブルには、ソースデータセット (`DIM_TABLE_ABC`) をクリックします。 これにより、ソーステーブルから、次の間の処理済みデータを取得できます。 `from_snapshot_id` および `to_snapshot_id`を追加します。
4. を更新します。 `checkpoint_log` テーブル `to_snapshot_id` ( ソースデータに対して `DIM_TABLE_ABC` 正常に処理されました。
5. 匿名ブロックの連続して実行されたクエリが失敗した場合、 **オプション** 例外セクションが実行されます。 これによりエラーが返され、プロセスが終了します。

>[!NOTE]
>
>この `history_meta('source table name')` は、データセット内の使用可能なスナップショットへのアクセス権を取得するために使用される便利な方法です。

```SQL
$$ BEGIN
    SET @from_snapshot_id = SELECT coalesce(last_snapshot_id, 'HEAD') FROM checkpoint_log a JOIN
                            (SELECT MAX(process_timestamp)process_timestamp FROM checkpoint_log
                                WHERE process_name = 'DIM_TABLE_ABC' AND process_status = 'SUCCESSFUL' )b
                                ON a.process_timestamp=b.process_timestamp;
    SET @to_snapshot_id = SELECT snapshot_id FROM (SELECT history_meta('DIM_TABLE_ABC')) WHERE  is_current = true;
    SET @last_updated_timestamp= SELECT CURRENT_TIMESTAMP;
    CREATE TABLE DIM_TABLE_ABC_Incremental AS
     SELECT  *  FROM DIM_TABLE_ABC SNAPSHOT BETWEEN @from_snapshot_id AND @to_snapshot_id ;
 
INSERT INTO
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

4 以下の匿名ブロックの例で増分データ読み込みロジックを使用して、（最新のタイムスタンプ以降の）ソースデータセットからの新しいデータを処理し、通常のサイクルで宛先テーブルに追加できるようにします。 この例では、データは `DIM_TABLE_ABC` が処理され、に追加されます `DIM_TABLE_ABC_incremental`.

>[!NOTE]
>
> `_ID` は両方のプライマリキー `DIM_TABLE_ABC_Incremental` および `SELECT history_meta('DIM_TABLE_ABC')`.

```SQL
$$ BEGIN
    SET @from_snapshot_id = SELECT coalesce(last_snapshot_id, 'HEAD') FROM checkpoint_log a join
                            (SELECT MAX(process_timestamp)process_timestamp FROM checkpoint_log
                                WHERE process_name = 'DIM_TABLE_ABC' AND process_status = 'SUCCESSFUL' )b
                                ON a.process_timestamp=b.process_timestamp;
    SET @to_snapshot_id = SELECT snapshot_id FROM (SELECT history_meta('DIM_TABLE_ABC')) WHERE  is_current = true;
    SET @last_updated_timestamp= SELECT CURRENT_TIMESTAMP;
    INSERT INTO DIM_TABLE_ABC_Incremental
     SELECT  *  FROM DIM_TABLE_ABC SNAPSHOT BETWEEN @from_snapshot_id AND @to_snapshot_id WHERE NOT EXISTS (SELECT _id FROM DIM_TABLE_ABC_Incremental a WHERE _id=a._id);
 
INSERT INTO
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

このロジックは任意のテーブルに適用して、増分荷重を実行できます。

## 期限切れのスナップショット

>[!IMPORTANT]
>
>スナップショットメタデータの有効期限： **2** 日。 期限切れのスナップショットは、上記で提供されたスクリプトのロジックを無効化します。

期限切れのスナップショット ID の問題を解決するには、匿名ブロックの先頭に次のコマンドを挿入します。 次のコード行は、 `@from_snapshot_id` 最も早く手に入る `snapshot_id` メタデータから。

```SQL
SET resolve_fallback_snapshot_on_failure=true;
```

コードブロック全体は次のようになります。

```SQL
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

## 次の手順

このドキュメントでは、匿名ブロックおよびスナップショット機能を使用して増分読み込みを実行する方法をより深く理解し、このロジックを独自のクエリに適用する方法について説明します。 クエリの実行に関する一般的なガイダンスについては、[クエリサービスでのクエリのー実行に関するガイド](../best-practices/writing-queries.md)を参照してください。
