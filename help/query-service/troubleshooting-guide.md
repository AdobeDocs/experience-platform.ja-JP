---
keywords: Experience Platform；ホーム；よく読まれるトピック；クエリサービス；クエリサービス；トラブルシューティングガイド；FAQ；トラブルシューティング；
solution: Experience Platform
title: クエリサービストラブルシューティングガイド
topic-legacy: troubleshooting
description: このドキュメントには、発生する一般的なエラーコードと考えられる原因に関する情報が含まれています。
exl-id: 14cdff7a-40dd-4103-9a92-3f29fa4c0809
source-git-commit: 42288ae7db6fb19bc0a0ee8e4ecfa50b7d63d017
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 22%

---

# [!DNL Query Service] トラブルシューティングガイド

このドキュメントでは、クエリサービスに関するよくある質問に対する回答を示し、クエリサービスを使用する際によく見られるエラーコードのリストを示します。 Adobe Experience Platform の他のサービスに関する質問やトラブルシューティングについては、[Experience Platform のトラブルシューティングガイド](../landing/troubleshooting.md)を参照してください。

## よくある質問

次に、クエリサービスに関するよくある質問に対する回答のリストを示します。

### クエリのメタデータのみを取得するにはどうすればよいですか？

クエリのメタデータのみを取得するには、次のように、0 行を返すクエリを実行します。

```sql
SELECT * FROM <table> WHERE 1=0
```

このクエリは、指定したテーブルのメタデータのみを返します。

### CTAS(Create Table as Select) クエリを実体化せずに、すばやく反復処理する方法を教えてください。

一時テーブルを作成して、クエリを素早く繰り返し実行し、使用するクエリを実現できます。 一時テーブルを使用して、クエリが機能しているかどうかを検証することもできます。

例えば、一時テーブルを作成できます。

```sql
CREATE temp TABLE temp_dataset AS
SELECT *
FROM actual_dataset
WHERE 1 = 0;
```

次に、一時テーブルを次のように使用できます。

```sql
INSERT INTO temp_dataset
SELECT a._company AS _company,
a._id AS _id,
a.timestamp AS timestamp
FROM actual_dataset a
WHERE timestamp >= TO_TIMESTAMP('2021-01-21 12:00:00')
AND timestamp < TO_TIMESTAMP('2021-01-21 13:00:00')
LIMIT 100;
```

### 時系列データのフィルタリング方法を教えてください。

時系列データを使用してクエリを実行する場合、分析をより正確におこなうには、可能な限りタイムスタンプフィルターを使用する必要があります。

>[!NOTE]
>
> 日付文字列 **は `yyyy-mm-ddTHH24:MM:SS` の形式にする必要があります。**

タイムスタンプフィルターの使用例を次に示します。

```sql
SELECT a._company  AS _company,
       a._id       AS _id,
       a.timestamp AS timestamp
FROM   dataset a
WHERE  timestamp >= To_timestamp('2021-01-21 12:00:00')
       AND timestamp < To_timestamp('2021-01-21 13:00:00')
```

### データセットからすべての行を取得する場合は、*などのワイルドカードを使用する必要がありますか？

クエリサービスは、従来の行ベースのストアシステムではなく **column-store** として扱う必要があるので、ワイルドカードを使用して行からすべてのデータを取得することはできません。

### SQL クエリで `NOT IN` を使用する必要はありますか。

`NOT IN` 演算子は、他のテーブルや SQL ステートメントに見つからない行を取得する場合によく使用されます。 この演算子は、パフォーマンスが低下する可能性があり、比較対象の列が `NOT NULL` を受け入れる場合、またはレコード数が多い場合は、予期しない結果が返される可能性があります。

`NOT IN` を使用する代わりに、`NOT EXISTS` または `LEFT OUTER JOIN` を使用できます。

例えば、次のテーブルが作成されている場合：

```sql
CREATE TABLE T1 (ID INT)
CREATE TABLE T2 (ID INT)
INSERT INTO T1 VALUES (1)
INSERT INTO T1 VALUES (2)
INSERT INTO T1 VALUES (3)
INSERT INTO T2 VALUES (1)
INSERT INTO T2 VALUES (2)
```

`NOT EXISTS` 演算子を使用している場合は、次のクエリを使用して `NOT IN` 演算子を使用してレプリケートできます。

```sql
SELECT ID FROM T1
WHERE NOT EXISTS
(SELECT ID FROM T2 WHERE T1.ID = T2.ID)
```

また、 `LEFT OUTER JOIN` 演算子を使用している場合は、次のクエリを使用して `NOT IN` 演算子を使用してレプリケートできます。

```sql
SELECT T1.ID FROM T1
LEFT OUTER JOIN T2 ON T1.ID = T2.ID
WHERE T2.ID IS NULL
```

### `OR` 演算子と `UNION` 演算子の正しい使用方法を教えてください。

### `CAST` 演算子を使用して SQL クエリのタイムスタンプを正しく変換するには、どうすればよいですか。

`CAST` 演算子を使用してタイムスタンプを変換する場合は、**と** の両方の日付を含める必要があります。

例えば、以下に示すように、時間コンポーネントが見つからないと、エラーが発生します。

```sql
SELECT * FROM ABC
WHERE timestamp = CAST('07-29-2021' AS timestamp)
```

`CAST` 演算子の正しい使用方法を次に示します。

```sql
SELECT * FROM ABC
WHERE timestamp = CAST('07-29-2021 00:00:00' AS timestamp)
```

## REST API エラー

| HTTP ステータスコード | 説明 | 考えられる原因 |
| ---------------- | ----------- | --------------- |
| 400 | 不正なリクエストです。 | クエリの形式が不正であるか、不正なクエリです。 |
| 401 | 認証に失敗しました。 | 認証トークンが無効です。 |
| 500 | 内部サーバーエラー。 | 内部システム障害が発生しています。 |

## PostgreSQL API エラー

| エラーコード | 接続状態 | 説明 | 考えられる原因 |
| ---------- | ---------------- | ----------- | -------------- |
| **08P01** | なし | メッセージの種類がサポートされていません。 | メッセージの種類がサポートされていません。 |
| **28P01** | 起動 — 認証 | パスワードが無効です。 | 認証トークンが無効です。 |
| **28000** | 起動 — 認証 | 認証タイプが無効です。 | 認証タイプが無効です。`AuthenticationCleartextPassword` である必要があります。 |
| **42P12** | 起動 — 認証 | テーブルが見つかりません。 | 使用するテーブルが見つかりません。 |
| **42601** | クエリ | 構文エラー。 | コマンドが無効であるか、構文にエラーがあります。 |
| **42P01** | クエリ | テーブルが見つかりません。 | クエリで指定されたテーブルが見つかりませんでした。 |
| **42P07** | クエリ | テーブルが存在します。 | 同じ名前のテーブルが既に存在します (CREATE TABLE) |
| **53400** | クエリ | LIMIT が最大値を超えています。 | ユーザーが LIMIT 句で 100,000 を超える行数を指定しました。 |
| **53400** | クエリ | ステートメントがタイムアウトになりました。 | 送信されたステートメントの処理時間が最大値の 10 分を超えました。 |
| **58000** | クエリ | システムエラー。 | 内部システム障害が発生しています。 |
| **0A000** | クエリ/コマンド | サポートなし | クエリ/コマンドの機能はサポートされていません |
| **42501** | DROP TABLE クエリ | クエリサービスで作成されていないテーブルを削除しています | 削除中のテーブルは、`CREATE TABLE` ステートメントを使用してクエリサービスによって作成されませんでした |
| **42501** | DROP TABLE クエリ | 認証済みユーザーが作成していないテーブル | 削除中のテーブルは、現在ログインしているユーザーによって作成されませんでした |
| **42P01** | DROP TABLE クエリ | テーブルが見つかりません。 | クエリで指定されたテーブルが見つかりませんでした |
| **42P12** | DROP TABLE クエリ | `dbName` のテーブルが見つかりません：`dbName` を確認してください | 現在のデータベースにテーブルが見つかりませんでした |
