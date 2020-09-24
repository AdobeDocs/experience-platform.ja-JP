---
keywords: Experience Platform;home;popular topics;query service;Query service;writing queries;writing query;
solution: Experience Platform
title: クエリの記述
topic: queries
type: Tutorial
translation-type: tm+mt
source-git-commit: 97dfd3a9a66fe2ae82cec8954066bdf3b6346830
workflow-type: tm+mt
source-wordcount: '643'
ht-degree: 73%

---


# General guidance for query execution in [!DNL Query Service]

This document details important details to know when writing queries in Adobe Experience Platform [!DNL Query Service].

For detailed information on the SQL syntax used in [!DNL Query Service], please read the [SQL syntax documentation](../sql/syntax.md).

## クエリ実行モデル

Adobe Experience Platform [!DNL Query Service] has two models of query execution: interactive and non-interactive. インタラクティブな実行は、ビジネスインテリジェンスツールでのクエリの開発とレポートの生成に使用され、非インタラクティブな実行は、データ処理ワークフローの一部として大規模なジョブや運用クエリに使用されます。

### インタラクティブクエリの実行

Queries can be executed interactively by submitting them through the [!DNL Query Service] UI or [through a connected client](../clients/overview.md). When running [!DNL Query Service] through a connected client, an active session runs between the client and [!DNL Query Service] until either the submitted query returns or times out.

インタラクティブクエリの実行には、次の制限があります。

| パラメーター | 制限事項 |
| --------- | ---------- |
| クエリのタイムアウト | 10 分 |
| 返される最大行数 | 50,000 |
| 最大同時クエリ | 5 |

>[!NOTE]
>
> 最大行数の制限を上書きするには、`LIMIT 0` をクエリに含めます。10 分のクエリタイムアウトは引き続き適用されます。

デフォルトでは、インタラクティブクエリの結果はクライアントに返され、永続 化&#x200B;**されません**。In order to persist the results as a dataset in [!DNL Experience Platform], the query must use the `CREATE TABLE AS SELECT` syntax.

### 非インタラクティブクエリの実行

Queries submitted through the [!DNL Query Service] API are run non-interactively. Non-interactive execution means that [!DNL Query Service] receives the API call and executes the query in the order it is received. Non-interactive queries always result in either the generation of a new dataset in [!DNL Experience Platform] to receive the results, or the insertion of new rows into an existing dataset.

## オブジェクト内の特定のフィールドへのアクセス

クエリ内のオブジェクト内のフィールドにアクセスするには、ドット表記（`.`）または角括弧表記（`[]`）を使用します。次の SQL 文は、ドット表記を使用し、`endUserIds` オブジェクトを下に移動して `mcid` オブジェクトを表示します。

```sql
SELECT endUserIds._experience.mcid
FROM {ANALYTICS_TABLE_NAME}
WHERE endUserIds._experience.mcid IS NOT NULL
LIMIT 1
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{ANALYTICS_TABLE_NAME}` | 解析テーブルの名前。 |

次の SQL 文は、括弧表記を使用し、`endUserIds` オブジェクトを下に移動して `mcid` オブジェクトを表示します。

```sql
SELECT endUserIds['_experience']['mcid']
FROM {ANALYTICS_TABLE_NAME}
WHERE endUserIds._experience.mcid IS NOT NULL
LIMIT 1
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{ANALYTICS_TABLE_NAME}` | 解析テーブルの名前。 |

>[!NOTE]
>
> 各表記タイプは同じ結果を返すので、好みに応じてどれを使用するかを選択します。

上記の例のクエリはどちらも、単一の値ではなく、フラット化されたオブジェクトを返します。

```console
              endUserIds._experience.mcid   
--------------------------------------------------------
 (48168239533518554367684086979667672499,"(ECID)",true)
(1 row)
```

返される `endUserIds._experience.mcid` オブジェクトには、次のパラメーターに対応する値が含まれます。

- `id`
- `namespace`
- `primary`

列がオブジェクトまでしか宣言されていない場合、オブジェクト全体を文字列として返します。ID のみを表示するには、次を使用します。

```sql
SELECT endUserIds._experience.mcid.id
FROM {ANALYTICS_TABLE_NAME}
WHERE endUserIds._experience.mcid IS NOT NULL
LIMIT 1
```

```console
     endUserIds._experience.mcid.id 
----------------------------------------
 48168239533518554367684086979667672499
(1 row)
```

## 一重引用符、重複引用符、逆引用符を使用する場合

この節では、クエリで一重引用符、重複引用符、逆引用符を使用するタイミングについて説明します。

### 一重引用符

一重引用符（`'`）は、テキスト文字列の作成に使用されます。例えば、この変数を `SELECT` ステートメント内で使用して、結果に静的なテキスト値を返し、列の内容を評価する `WHERE` 句内で使用することができます。

次のクエリは、列の静的テキスト値（`'datasetA'`）を宣言します。

```sql
SELECT 
  'datasetA',
  timestamp,
  web.webPageDetails.name
FROM {ANALYTICS_TABLE_NAME}
LIMIT 10
```

次のクエリでは、WHERE 句において一重引用符で囲まれた文字列（`'homepage'`）を使用して、特定のページのイベントを返します。

```sql
SELECT 
  timestamp,
  endUserIds._experience.mcid.id
FROM {ANALYTICS_TABLE_NAME}
WHERE web.webPageDetails.name = 'homepage'
LIMIT 10
```

### 二重引用符

二重引用符（`"`）は、識別子をスペースで宣言するために使用されます。

次のクエリでは、二重引用符を使用して、1 つの列で識別子にスペースが含まれている場合に、指定した列の値を返します。

```sql
SELECT
  no_space_column,
  "space column"
FROM
( SELECT 
    'column1' as no_space_column,
    'column2' as "space column"
)
```

>[!NOTE]
>
> 二重引用符は、ドット表記のフィールドアクセスでは&#x200B;**使用できません**。

### 逆引用符

逆引用符 `` ` `` は、ドット表記の構文を使用する場合に&#x200B;**のみ**&#x200B;予約列名をエスケープするために使用します。例えば、`order` は SQL では予約語なので、逆引用符をしようしてフィールド `commerce.order` にアクセスする必要があります。

```sql
SELECT 
  commerce.`order`
FROM {ANALYTICS_TABLE_NAME}
LIMIT 10
```

逆引用符は、数字で開始するフィールドにアクセスするためにも使用されます。例えば、フィールド `30_day_value` にアクセスするには、逆引用符表記を使用する必要があります。

```SQL
SELECT
    commerce.`30_day_value`
FROM {ANALYTICS_TABLE_NAME}
LIMIT 10
```

角括弧表記を使用する場合は、逆引用符は&#x200B;**不要**&#x200B;です。

```sql
 SELECT
  commerce['order']
 FROM {ANALYTICS_TABLE_NAME}
 LIMIT 10
```

## 次の手順

By reading this document, you have been introduced to some important considerations when writing queries using [!DNL Query Service]. SQL 構文を使用して独自のクエリを記述する方法の詳細については、[SQL構文のドキュメント](../sql/syntax.md)を参照してください。