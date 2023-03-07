---
title: クエリサービスでの増分読み込み
description: 増分読み込み機能は、匿名ブロックとスナップショットの両方の機能を使用して、一致するデータを無視しながらデータレイクからデータウェアハウスにデータを移動するための、ほぼリアルタイムのソリューションを提供します。
exl-id: 1418d041-29ce-4153-90bf-06bd8da8fb78
source-git-commit: 11a947addce65887385c983ac81d884fb4244291
workflow-type: tm+mt
source-wordcount: '688'
ht-degree: 100%

---

# クエリサービスでの増分読み込み

増分読み込みデザインパターンは、データを管理するためのソリューションです。 このパターンでは、データセット内で前回の読み込みの実行以降に作成または変更された情報のみを処理します。

増分読み込みでは、匿名ブロックやスナップショットなど、Adobe Experience Platform クエリサービスで提供される様々な機能を使用します。 このデザインパターンにより、ソースから既に処理されたデータがスキップされるので、処理効率が向上します。 ストリーミングデータ処理とバッチデータ処理の両方で使用できます。

このドキュメントでは、増分処理のためのデザインパターンを構築するための一連の手順を示します。 これらの手順は、独自の増分データ読み込みクエリを作成するテンプレートとして使用できます。

## はじめに

このドキュメントの随所で使用されている SQL の例を参考にするには、匿名ブロックとスナップショットの機能について理解している必要があります。 [匿名ブロッククエリの例](./anonymous-block.md)と [SNAPSHOT 句](../sql/syntax.md#snapshot-clause)のドキュメントを参照することをお勧めします。

このガイドで使用されている用語のガイダンスについては、[SQL 構文ガイド](../sql/syntax.md)を参照してください。

## データの増分読み込み

次の手順では、スナップショットおよび匿名ブロック機能を使用して、データを作成し増分的に読み込む方法を示します。 このデザインパターンは、独自のクエリシーケンスのテンプレートとして使用できます。

1. . データの正常な処理に使用された最新のスナップショットを追跡するための `checkpoint_log` テーブルを作成します。データセットを増分的に処理するには、トラッキングテーブル（この例では `checkpoint_log`）をまず `null` に初期化する必要があります。

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

1. . 増分処理が必要なデータセットの空のレコード 1 つを `checkpoint_log` テーブルに入力します。 `DIM_TABLE_ABC` は、以下の例で処理されるデータセットです。 `DIM_TABLE_ABC` の処理を初めて行う際、`last_snapshot_id` は `null` に初期化されます。これにより、初回はデータセット全体を処理し、それ以降は増分的に処理することができます。

   ```SQL
   INSERT INTO
      checkpoint_log
      SELECT
         'DIM_TABLE_ABC' process_name,
         'SUCCESSFUL' process_status,
         cast(NULL AS string) last_snapshot_id,
         CURRENT_TIMESTAMP process_timestamp;
   ```

1. . `DIM_TABLE_ABC_Incremental` からの処理済みの出力を含むように `DIM_TABLE_ABC` を初期化します。  以下の SQL サンプルの&#x200B;**必須**&#x200B;実行セクションに含まれている匿名ブロック（手順 1～4 で説明）が順番に実行されて、データが増分的に処理されます。

   1. 処理の開始位置を示す `from_snapshot_id` を設定します。この例の `from_snapshot_id` は、`DIM_TABLE_ABC` で使用するために `checkpoint_log` テーブルからクエリされます。初回の実行時に、スナップショット ID は `null` になります。つまり、データセット全体が処理されます。
   1. `to_snapshot_id` をソーステーブル（`DIM_TABLE_ABC`）の現在のスナップショット ID として設定します。 例では、これはソーステーブルのメタデータテーブルからクエリされます。
   1. `CREATE` キーワードを使用して、`DIM_TABLE_ABC_Incremenal` を宛先テーブルとして作成します。宛先テーブルは、ソースデータセット（`DIM_TABLE_ABC`）からの処理済みデータを保持します。これにより、ソーステーブルからの処理済みデータ（`from_snapshot_id` から `to_snapshot_id` まで）を宛先テーブルに増分的に追加できます。
   1. `DIM_TABLE_ABC` で正常に処理されたソースデータの `to_snapshot_id` を `checkpoint_log` テーブルに反映させます。
   1. 匿名ブロックの順次実行されたクエリのいずれかが失敗した場合は、**オプション**&#x200B;の例外セクションが実行されます。 この結果、エラーが返され、プロセスが終了します。

   >[!NOTE]
   >
   >`history_meta('source table name')` は、データセット内の使用可能なスナップショットにアクセスできるようにするための便利な方法です。

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

1. . 以下の匿名ブロックの例で増分データ読み込みロジックを使用して、ソースデータセット内の新しいデータ（最新のタイムスタンプ以降のデータ）を通常のサイクルで処理して宛先テーブルに追加できるようにします。 この例では、`DIM_TABLE_ABC` の変更されたデータが処理され、`DIM_TABLE_ABC_incremental` に追加されます。

   >[!NOTE]
   >
   > `_ID` は `DIM_TABLE_ABC_Incremental` と `SELECT history_meta('DIM_TABLE_ABC')` の両方のプライマリキーです。

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

このロジックを任意のテーブルに適用して、増分読み込みを実行できます。

## 期限切れのスナップショット

>[!IMPORTANT]
>
>スナップショットメタデータは **2** 日後に有効期限が切れます。スナップショットの有効期限が切れると、上記で示したスクリプトのロジックは無効になります。

期限切れのスナップショット ID の問題を解決するには、匿名ブロックの先頭に次のコマンドを挿入します。 次のコード行では、メタデータから入手できる最も古い `snapshot_id` で `@from_snapshot_id` をオーバーライドしています。

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

このドキュメントを通して、匿名ブロックおよびスナップショット機能を使用して増分読み込みを実行する方法をより深く理解し、このロジックを独自の具体的なクエリに適用できるようになりました。クエリの実行に関する一般的なガイダンスについては、[クエリサービスでのクエリのー実行に関するガイド](../best-practices/writing-queries.md)を参照してください。
