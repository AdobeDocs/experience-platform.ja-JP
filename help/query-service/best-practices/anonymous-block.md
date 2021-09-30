---
title: 匿名ブロッククエリの例
description: '匿名ブロックは、Adobe Experience Platformクエリサービスでサポートされる SQL 構文で、一連のクエリを効率的に実行できます '
source-git-commit: dffa03d9ab8e26e2c99a359ae000022d6d469572
workflow-type: tm+mt
source-wordcount: '499'
ht-degree: 0%

---

# 匿名ブロックのクエリ例

Adobe Experience Platformクエリサービスは、匿名ブロックをサポートします。 匿名ブロック機能を使用すると、順に実行される 1 つ以上の SQL 文を連結できます。 また、例外処理のオプションも使用できます。

匿名ブロック機能は、一連の操作やクエリを効率的に実行する方法です。 ブロック内のクエリーの連鎖は、テンプレートとして保存し、特定の時間または間隔で実行するようにスケジュール設定できます。 これらのクエリは、新しいデータセットを作成するためのデータの書き込みと追加に使用でき、通常は依存関係がある場合に使用されます。

次の表に、ブロックのメインセクションの分類を示します。実行、例外処理を含む )。 各セクションは、キーワード `BEGIN`、`END`、`EXCEPTION` で定義します。

| セクション | description |
|---|---|
| 実行 | 実行可能セクションは、キーワード `BEGIN` で始まり、キーワード `END` で終わります。 `BEGIN` キーワードと `END` キーワードに含まれる文のセットは順番に実行され、シーケンス内の前のクエリが完了するまで後続のクエリは実行されません。 |
| 例外処理 | オプションの例外処理セクションは、キーワード `EXCEPTION` で始まります。 実行セクション内の SQL 文が失敗した場合に例外を取得して処理するコードが含まれています。 いずれかのクエリが失敗した場合、ブロック全体が停止します。 |

ブロックは実行可能な文であり、他のブロック内にネストできる点に注意する必要があります。

>[!NOTE]
>
> 小さなデータセットに対してクエリをテストし、クエリが期待どおりに動作することを確認することを強くお勧めします。 クエリに構文エラーがある場合、例外がスローされ、ブロック全体が中止されます。 クエリの整合性を確認したら、クエリのチェーン化を開始できます。 これにより、ブロックを操作する前に、ブロックが期待どおりに動作します。

## 匿名ブロッククエリの例

次のクエリは、SQL 文の連鎖の例を示しています。 使用する SQL 構文の詳細については、クエリサービス ](../sql/syntax.md) のドキュメントの [SQL 構文を参照してください。

```SQL
$$BEGIN
     
    CREATE TABLE ADLS_TABLE_A AS SELECT * FROM ADLS_TABLE_1....;
    ....
    CREATE TABLE ADLS_TABLE_D AS SELECT * FROM ADLS_TABLE_C....;
     
    EXCEPTION WHEN OTHER THEN SET @ret = SELECT 'ERROR';
     
END$$;
```

<!-- The block below uses `SET` to persist the result of a select query with a variable. It is used in the anonymous block to store the response from a query as a local variable for use with the `SNAPSHOT` feature. -->

次の例では、`SET` は、`SELECT` クエリの結果を指定したローカル変数に保持します。 変数は、匿名ブロックにスコープされます。

スナップショット ID はローカル変数 (`@current_sid`) として保存されます。 次のクエリで使用し、同じデータセット/テーブルの SNAPSHOT に基づいて結果を返します。

データベース・スナップショットは、SQL Server データベースの読み取り専用の静的ビューです。 [ スナップショット句 ](../sql/syntax.md#SNAPSHOT-clause) の詳細については、SQL 構文に関するドキュメントを参照してください。

```SQL
$$BEGIN                                             
  SET @current_sid = SELECT parent_id  FROM (SELECT history_meta('your_table_name')) WHERE  is_current = true;
  CREATE temp table abcd_temp_table AS SELECT count(1) FROM your_table_name  SNAPSHOT SINCE @current_sid;                                                                                                     
END$$;
```

## 次の手順

このドキュメントを読むと、匿名ブロックの構造と構造を明確に理解できます。 [クエリの実行の詳細については](./writing-queries.md)、クエリサービスでのクエリの実行に関するガイドを参照してください。

クエリサービス内で使用できるクエリの詳細なサンプルについては、[Adobe Analyticsサンプルクエリ ](./adobe-analytics.md)、[Adobe Targetサンプルクエリ ](./adobe-target.md)、[ExperienceEvent サンプルクエリ ](./experience-event-queries.md) のガイドを参照してください。
