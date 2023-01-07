---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；準備済み文；準備済み；SQL;
solution: Experience Platform
title: クエリサービスの準備済み文
description: SQL では、準備済み文を使用して、類似したクエリや更新をテンプレート化します。 Adobe Experience Platform クエリサービスは、パラメーター化されたクエリを使用して準備済み文をサポートします。
exl-id: 7ee4a10e-2bfe-487f-a8c5-f03b5b1d77e3
source-git-commit: 58eadaaf461ecd9598f3f508fab0c192cf058916
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 85%

---

# 準備済み文

SQL では、準備済み文を使用して、類似したクエリや更新をテンプレート化します。Adobe Experience Platform [!DNL Query Service] は、パラメーター化されたクエリを使用して準備済み文をサポートします。 クエリを繰り返し再解析する必要がなくなったので、パフォーマンスを最適化できます。

## 準備済み文の使用

準備済み文を使用する場合、次の構文がサポートされます。

- [PREPARE](#prepare)
- [EXECUTE](#execute)
- [DEALLOCATE](#deallocate)

### 準備済み文の準備 {#prepare}

この SQL クエリは、書き込まれた SELECT クエリを、`PLAN_NAME` として指定された名前で保存します。実際の値の代わりに、`$1` などの変数を使用できます。この準備済み文は、現在のセッション中に保存されます。プラン名では大文字と小文字が区別&#x200B;**されない**&#x200B;ことに注意してください。

#### SQL 形式

```sql
PREPARE {PLAN_NAME} AS {SELECT_QUERY}
```

#### サンプル SQL

```sql
PREPARE test AS SELECT * FROM table WHERE country = $1 AND city = $2;
```

### 準備済み文の実行 {#execute}

この SQL クエリは、前に作成した準備済み文を使用します。

#### SQL 形式

```sql
EXECUTE {PLAN_NAME}('{PARAMETERS}')
```

#### サンプル SQL

```sql
EXECUTE test('canada', 'vancouver');
```

### 準備済み文の割り当て解除 {#deallocate}

この SQL クエリは、名前の付いた準備済み文を削除するために使用されます。

#### SQL 形式

```sql
DEALLOCATE {PLAN_NAME}
```

#### サンプル SQL

```sql
DEALLOCATE test;
```

## 準備済み文を使用したフローの例

最初は、次のような SQL クエリを使用できます。

```sql
SELECT * FROM table WHERE id >= 10000 AND id <= 10005;
```

上記の SQL クエリは、次の応答を返します。

| ID | 名 | 姓 | 生年月日 | 電子メール | 都市 | 国 |
|--- | --------- | -------- | --------- | ----- | ------- | ---- |
| 10000 | alexander | davis | 1993-09-15 | example@example.com | バンクーバー | カナダ |
| 10001 | antoine | dubois | 1967-03-14 | example2@example.com | パリ | フランス |
| 10002 | 京子 | 桜 | 1999-11-26 | example3@example.com | 東京 | 日本 |
| 10003 | linus | pettersson | 1982-06-03 | example4@example.com | ストックホルム | スウェーデン |
| 10004 | aasir | waithaka | 1976-12-17 | example5@example.com | ナイロビ | ケニア |
| 10005 | fernando | rios | 2002-07-30 | example6@example.com | サンティアゴ | チリ |

この SQL クエリは、次の準備済み文を使用してパラメータ-化できます。

```sql
PREPARE getIdRange AS SELECT * FROM table WHERE id >= $1 AND id <= $2; 
```

これで、次の呼び出しを使用して、準備済み文を実行できます。

```sql
EXECUTE getIdRange(10000, 10005);
```

呼び出すと、以前と同じ結果が表示されます。

| ID | 名 | 姓 | 生年月日 | 電子メール | 都市 | 国 |
|--- | --------- | -------- | --------- | ----- | ------- | ---- |
| 10000 | alexander | davis | 1993-09-15 | example@example.com | バンクーバー | カナダ |
| 10001 | antoine | dubois | 1967-03-14 | example2@example.com | パリ | フランス |
| 10002 | 京子 | 桜 | 1999-11-26 | example3@example.com | 東京 | 日本 |
| 10003 | linus | pettersson | 1982-06-03 | example4@example.com | ストックホルム | スウェーデン |
| 10004 | aasir | waithaka | 1976-12-17 | example5@example.com | ナイロビ | ケニア |
| 10005 | fernando | rios | 2002-07-30 | example6@example.com | サンティアゴ | チリ |

準備済み文の使用が終了したら、次の呼び出しを使用して、その文の割り当てを解除できます。

```sql
DEALLOCATE getIdRange;
```
