---
title: クエリサービスの匿名ブロック
description: 匿名ブロックは、Adobe Experience Platform クエリサービスでサポートされている SQL 構文であり、クエリのシーケンスを効率的に実行できます
exl-id: ec497475-9d2b-43aa-bcf4-75a430590496
source-git-commit: d3ea7ee751962bb507c91e1afea0da35da60a66d
workflow-type: ht
source-wordcount: '515'
ht-degree: 100%

---

# クエリサービスの匿名ブロック

Adobe Experience Platform クエリサービスは、匿名ブロックをサポートします。匿名ブロック機能を使用すると、順に実行される 1 つ以上の SQL 文を連結できます。また、例外処理のオプションも使用できます。

匿名ブロック機能は、操作またはクエリのシーケンスを効率的に実行する方法です。ブロック内のクエリの連結をテンプレートとして保存し、特定の時間または間隔で実行するようにスケジュールできます。これらのクエリは、データの書き込みと追加を使用して新しいデータセットを作成するために使用でき、通常は依存関係がある場合に使用されます。

>[!IMPORTANT]
>
>匿名ブロックを使用したクエリのスケジュールは、現在、[!DNL Query Service] API を使用してのみ可能です。[API を使用してクエリをスケジュールするための完全な手順](../api/scheduled-queries.md)については、ドキュメントを参照してください。

次の表に、実行と例外処理に関するブロックの主要セクションの分類を示します。セクションは、キーワード `BEGIN`、`END` および `EXCEPTION` によって定義されます。

| セクション | 説明 |
|---|---|
| 実行 | 実行可能セクションは、キーワード `BEGIN` で始まり、キーワード `END` で終わります。`BEGIN` および `END` キーワード内に含まれる文のセットは順番に実行され、シーケンス内の前のクエリが完了するまで後続のクエリが実行されないようにします。 |
| 例外処理 | オプションの例外処理セクションは、キーワード `EXCEPTION` で始まります。これには、実行セクションの SQL 文のいずれかが失敗した場合に、例外を取得して処理するコードが含まれています。クエリのいずれかが失敗した場合、ブロック全体が停止します。 |

ブロックは実行可能な文であり、他のブロック内にネストできます。

>[!NOTE]
>
> 小さなデータセットに対してクエリをテストし、クエリが期待どおりに動作することを確認することを強くお勧めします。クエリに構文エラーがある場合、例外がスローされ、ブロック全体が中止されます。クエリの整合性を確認したら、クエリの連結を開始できます。これにより、ブロックを操作する前に、ブロックが期待どおりに動作します。

## 匿名ブロッククエリのサンプル

次のクエリは、SQL 文を連結する例を示しています。 使用されている SQL 構文について詳しくは、[クエリサービスの SQL 構文](../sql/syntax.md)ドキュメントを参照してください。

```SQL
$$ BEGIN
    CREATE TABLE ADLS_TABLE_A AS SELECT * FROM ADLS_TABLE_1....;
    ....
    CREATE TABLE ADLS_TABLE_D AS SELECT * FROM ADLS_TABLE_C....; 
    EXCEPTION WHEN OTHER THEN SET @ret = SELECT 'ERROR';
END
$$;
```

以下の例では、`SET` は指定されたローカル変数に `SELECT` クエリの結果を保持します。変数は、匿名ブロックに対してスコープ設定されます。

スナップショット ID は、ローカル変数（`@current_sid`）として保存されます。その後、次のクエリで使用され、同じデータセット／テーブルからの SNAPSHOT に基づいて結果が返されます。

データベーススナップショットは、SQL Server データベースの読み取り専用の静的ビューです。[スナップショット句に関する情報](../sql/syntax.md#SNAPSHOT-clause)については、SQL 構文のドキュメントを参照してください。

```SQL
$$ BEGIN                                             
  SET @current_sid = SELECT parent_id  FROM (SELECT history_meta('your_table_name')) WHERE  is_current = true;
  CREATE temp table abcd_temp_table AS SELECT count(1) FROM your_table_name  SNAPSHOT SINCE @current_sid;                                                                                           
END
$$;
```

## 次の手順

このドキュメントを参照することで、匿名ブロックとその構造を明確に理解できます。[クエリの実行について詳しくは](../best-practices/writing-queries.md)、クエリサービスでのクエリの実行に関するガイドを参照してください。

また、クエリの効率を高めるために、[増分読み込みデザインパターンで匿名ブロックを使用する方法](./incremental-load.md)についても参照してください。
