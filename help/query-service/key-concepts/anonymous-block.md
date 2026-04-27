---
title: クエリサービスの匿名ブロック
description: 匿名ブロックは、Adobe Experience Platform クエリサービスでサポートされている SQL 構文であり、クエリのシーケンスを効率的に実行できます
exl-id: ec497475-9d2b-43aa-bcf4-75a430590496
source-git-commit: f2d81f05c8c19c6f28849fc4dbe9bfa26be64645
workflow-type: tm+mt
source-wordcount: '619'
ht-degree: 68%

---

# クエリサービスの匿名ブロック

Adobe Experience Platform クエリサービスは、匿名ブロックをサポートします。 匿名ブロック機能を使用すると、順に実行される 1 つ以上の SQL 文を連結できます。 また、例外処理のオプションも使用できます。

匿名ブロック機能は、操作またはクエリのシーケンスを効率的に実行する方法です。 ブロック内のクエリの連結をテンプレートとして保存し、特定の時間または間隔で実行するようにスケジュールできます。 これらのクエリは、データの書き込みと追加を使用して新しいデータセットを作成するために使用でき、通常は依存関係がある場合に使用されます。

次の表に、実行と例外処理に関するブロックの主要セクションの分類を示します。 セクションは、キーワード `BEGIN`、`END` および `EXCEPTION` によって定義されます。

| セクション | description |
|---|---|
| 実行 | 実行可能セクションは、キーワード `BEGIN` で始まり、キーワード `END` で終わります。 `BEGIN` および `END` キーワード内に含まれる文のセットは順番に実行され、シーケンス内の前のクエリが完了するまで後続のクエリが実行されないようにします。 |
| 例外処理 | オプションの例外処理セクションは、キーワード `EXCEPTION` で始まります。 これには、実行セクションの SQL 文のいずれかが失敗した場合に、例外を取得して処理するコードが含まれています。 クエリのいずれかが失敗した場合、ブロック全体が停止します。 |

ブロックは実行可能な文であり、他のブロック内にネストできます。

>[!NOTE]
>
> 小さなデータセットに対してクエリをテストし、クエリが期待どおりに動作することを確認することを強くお勧めします。 クエリに構文エラーがある場合、例外がスローされ、ブロック全体が中止されます。 クエリの整合性を確認したら、クエリの連結を開始できます。 これにより、ブロックを操作する前に、ブロックが期待どおりに動作します。

## 匿名ブロッククエリのサンプル

次のクエリは、SQL 文を連結する例を示しています。 使用されている SQL 構文について詳しくは、[クエリサービスの SQL 構文](../sql/syntax.md)ドキュメントを参照してください。

```SQL
$$ BEGIN
    CREATE TABLE ADLS_TABLE_A AS SELECT * FROM ADLS_TABLE_1....;
    ....
    CREATE TABLE ADLS_TABLE_D AS SELECT * FROM ADLS_TABLE_C....; 
    EXCEPTION WHEN OTHERS THEN SET @ret = SELECT 'ERROR';
END
$$;
```

以下の例では、`SET` は指定されたローカル変数に `SELECT` クエリの結果を保持します。 変数は、匿名ブロックに対してスコープ設定されます。

スナップショット ID は、ローカル変数（`@current_sid`）として保存されます。 その後、次のクエリで使用して、同じデータセット/テーブルのスナップショットに基づいて結果を返します。 [スナップショット句に関する情報](../sql/syntax.md#SNAPSHOT-clause)については、SQL 構文のドキュメントを参照してください。

```SQL
$$ BEGIN                                             
  SET @current_sid = SELECT parent_id  FROM (SELECT history_meta('your_table_name')) WHERE  is_current = true;
  CREATE temp table abcd_temp_table AS SELECT count(1) FROM your_table_name  SNAPSHOT SINCE @current_sid;                                                                                           
END
$$;
```

## サードパーティクライアントとの匿名ブロック {#third-party-clients}

特定のサードパーティクライアントでは、スクリプトの一部を単一のステートメントとして処理する必要があることを示すために、SQL ブロックの前後に個別の識別子が必要になる場合があります。 サードパーティクライアントでクエリサービスを使用する際にエラーメッセージが表示される場合は、SQL ブロックの使用に関するサードパーティクライアントのドキュメントを参照する必要があります。

例えば、**DbVisualizer**&#x200B;では、区切り文字が行の唯一のテキストである必要があります。 DbVisualizerでは、開始識別子のデフォルト値は`--/`で、終了識別子のデフォルト値は`/`です。 DbVisualizerの匿名ブロックの例を次に示します。

```SQL
--/
$$ BEGIN
    CREATE TABLE ADLS_TABLE_A AS SELECT * FROM ADLS_TABLE_1....;
    ....
    CREATE TABLE ADLS_TABLE_D AS SELECT * FROM ADLS_TABLE_C....;
    EXCEPTION WHEN OTHERS THEN SET @ret = SELECT 'ERROR';
END
$$;
/
```

特にDbVisualizerの場合、UIに「[!DNL Execute the complete buffer as one SQL statement]」へのオプションもあります。 詳しくは、[DbVisualizerのドキュメント ](https://confluence.dbvis.com/display/UG120/Executing+Complex+Statements#ExecutingComplexStatements-UsingExecuteBuffer)を参照してください。

## 次の手順

このドキュメントを参照することで、匿名ブロックとその構造を明確に理解できます。 クエリの作成について詳しくは、[ クエリ実行ガイド ](../best-practices/writing-queries.md)を参照してください。

また、[増分読み込みデザインパターン ](./incremental-load.md)で匿名ブロックを使用してクエリの効率を高める方法についても説明します。
