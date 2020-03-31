---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 準備済みの明細書
topic: prepared statements
translation-type: tm+mt
source-git-commit: 5699022d1f18773c81a0a36d4593393764cb771a

---


# 準備済みの明細書

SQLでは、準備された文を使用して、類似したクエリや更新をテンプレート化します。 Adobe Experience Platformクエリサービスは、パラメータ化されたパラメーターを使用して準備されたステートメントをサポートしてクエリします。 これは、クエリの再解析を何度も繰り返す必要がなくなるので、パフォーマンスの最適化に使用できます。

## 準備文の使用

プリペアドステートメントを使用する場合、次の構文がサポートされます。

- [PREPARE](#prepare)
- [EXECUTE](#execute)
- [割り当て解除](#deallocate)

### 準備済みの文の準備 {#prepare}

このSQLクエリは、書き込まれたSELECTクエリを、として指定された名前で保存しま `PLAN_NAME`す。 実際の値の代わりに、などの変 `$1` 数を使用できます。 この準備された文は、現在のセッション中に保存されます。 プラン名では大文字と小文字が区別されないこ **とに注意** してください。

#### SQL形式

```sql
PREPARE {PLAN_NAME} AS {SELECT_QUERY}
```

#### サンプルSQL

```sql
PREPARE test AS SELECT * FROM table WHERE country = $1 AND city = $2;
```

### 準備文の実行 {#execute}

このSQLクエリは、前に作成した準備文を使用します。

#### SQL形式

```sql
EXECUTE {PLAN_NAME}('{PARAMETERS}')
```

#### サンプルSQL

```sql
EXECUTE test('canada', 'vancouver');
```

### 準備文の割り当て解除 {#deallocate}

このSQLクエリは、名前の付いたプリペアドステートメントを削除するために使用されます。

#### SQL形式

```sql
DEALLOCATE {PLAN_NAME}
```

#### サンプルSQL

```sql
DEALLOCATE test;
```

## 準備文を使用したフローの例

最初は、次のようなSQLクエリを使用できます。

```sql
SELECT * FROM table WHERE id >= 10000 AND id <= 10005;
```

上記のSQLクエリは、次の応答を返します。

| id | 名 | lastname | 誕生日 | 電子メール | city | country |
|--- | --------- | -------- | --------- | ----- | ------- | ---- |
| 10000 | アレクサン | デイビス | 1993-09-15 | example@example.com | バンクーバー | カナダ |
| 10001 | アントイン | デュボ | 1967-03-14 | example2@example.com | パリ | フランス |
| 10002 | 京子 | 桜 | 1999-11-26 | example3@example.com | 東京 | 日本 |
| 10003 | 亜麻 | ペテルソン | 1982-06-03 | example4@example.com | ストックホルム | スウェーデン |
| 10004 | aasir | ワイタカ | 1976-12-17 | example5@example.com | ナイロビ | ケニア |
| 10005 | フェルナンド | リオ | 2002-07-30 | example6@example.com | サンティアゴ | チリ |

このSQLクエリは、次のプリペアドステートメントを使用してパラメータ化できます。

```sql
PREPARE getIdRange AS SELECT * FROM table WHERE id >= $1 AND id <= $2; 
```

これで、次の呼び出しを使用して、プリペアドステートメントを実行できます。

```sql
EXECUTE getIdRange(10000, 10005);
```

これを呼び出すと、以前と同じ結果が表示されます。

| id | 名 | lastname | 誕生日 | 電子メール | city | country |
|--- | --------- | -------- | --------- | ----- | ------- | ---- |
| 10000 | アレクサン | デイビス | 1993-09-15 | example@example.com | バンクーバー | カナダ |
| 10001 | アントイン | デュボ | 1967-03-14 | example2@example.com | パリ | フランス |
| 10002 | 京子 | 桜 | 1999-11-26 | example3@example.com | 東京 | 日本 |
| 10003 | 亜麻 | ペテルソン | 1982-06-03 | example4@example.com | ストックホルム | スウェーデン |
| 10004 | aasir | ワイタカ | 1976-12-17 | example5@example.com | ナイロビ | ケニア |
| 10005 | フェルナンド | リオ | 2002-07-30 | example6@example.com | サンティアゴ | チリ |

準備された文の使用が終了したら、次の呼び出しを使用して、その文の割り当てを解除できます。

```sql
DEALLOCATE getIdRange;
```
