---
title: 匿名ブロッククエリの例
description: 匿名ブロックは、Adobe Experience Platformクエリサービスでサポートされる SQL 構文で、一連のクエリを効率的に実行できます
exl-id: ec497475-9d2b-43aa-bcf4-75a430590496
source-git-commit: 9f4e34edc47a333aa88153529d0af6a10f189a15
workflow-type: tm+mt
source-wordcount: '499'
ht-degree: 1%

---

# 匿名ブロックのクエリ例

Adobe Experience Platformクエリサービスは、匿名ブロックをサポートします。 匿名ブロック機能を使用すると、順に実行される 1 つ以上の SQL 文を連結できます。 また、例外処理のオプションも使用できます。

匿名ブロック機能は、一連の操作やクエリを効率的に実行する方法です。 ブロック内のクエリの連鎖は、テンプレートとして保存し、特定の時間または間隔で実行するようにスケジュール設定できます。 これらのクエリは、データの書き込みと追加を使用して新しいデータセットを作成するために使用でき、通常は依存関係がある場合に使用されます。

次の表に、ブロックの主要セクションの分類を示します。実行および例外処理に関する情報です。 セクションは、キーワードによって定義されます `BEGIN`, `END`、および `EXCEPTION`.

| セクション | description |
|---|---|
| 実行 | 実行可能セクションは、キーワードで始まる `BEGIN` キーワードで終わる `END`. 内に含まれる文のセット `BEGIN` および `END` キーワードは順番に実行され、シーケンス内の前のクエリが完了するまで後続のクエリは実行されません。 |
| 例外処理 | オプションの例外処理セクションは、キーワードで始まります。 `EXCEPTION`. 実行セクション内の SQL 文が失敗した場合に例外を取得して処理するコードが含まれています。 いずれかのクエリが失敗した場合、ブロック全体が停止します。 |

ブロックは実行可能な文であり、他のブロック内にネストできることに注意する必要があります。

>[!NOTE]
>
> 小さなデータセットに対してクエリをテストし、クエリが期待どおりに動作することを確認することを強くお勧めします。 クエリに構文エラーがある場合、例外がスローされ、ブロック全体が中止されます。 クエリの整合性を確認したら、クエリの連結を開始できます。 これにより、ブロックを操作する前に、ブロックが期待どおりに動作します。

## 匿名ブロッククエリのサンプル

次のクエリは、SQL 文を連結する例を示しています。 詳しくは、 [クエリサービスの SQL 構文](../sql/syntax.md) ドキュメントを参照してください。

```SQL
BEGIN
     
    CREATE TABLE ADLS_TABLE_A AS SELECT * FROM ADLS_TABLE_1....;
    ....
    CREATE TABLE ADLS_TABLE_D AS SELECT * FROM ADLS_TABLE_C....;
     
    EXCEPTION WHEN OTHER THEN SET @ret = SELECT 'ERROR';
     
END;
```

<!-- The block below uses `SET` to persist the result of a select query with a variable. It is used in the anonymous block to store the response from a query as a local variable for use with the `SNAPSHOT` feature. -->

次の例では、 `SET` の結果を保持します `SELECT` クエリを指定します。 変数は、匿名ブロックに対してスコープ設定されます。

スナップショット ID はローカル変数 (`@current_sid`) をクリックします。 次のクエリで使用され、同じデータセット/テーブルの SNAPSHOT に基づいて結果が返されます。

データベーススナップショットは、SQL Server データベースの読み取り専用の静的ビューです。 詳細 [スナップショット句に関する情報](../sql/syntax.md#SNAPSHOT-clause) SQL 構文のドキュメントを参照してください。

```SQL
BEGIN                                             
  SET @current_sid = SELECT parent_id  FROM (SELECT history_meta('your_table_name')) WHERE  is_current = true;
  CREATE temp table abcd_temp_table AS SELECT count(1) FROM your_table_name  SNAPSHOT SINCE @current_sid;                                                                                                     
END;
```

## 次の手順

このドキュメントを読むと、匿名ブロックの構造と構造を明確に理解できます。 [クエリの実行の詳細](./writing-queries.md)を参照してください。クエリサービスでのクエリの実行に関するガイドをお読みください。

クエリサービス内で使用できるクエリのその他のサンプルについては、 [Adobe Analyticsクエリ例](./adobe-analytics.md), [Adobe Targetクエリ例](./adobe-target.md)または [ExperienceEvent クエリ例](./experience-event-queries.md).
