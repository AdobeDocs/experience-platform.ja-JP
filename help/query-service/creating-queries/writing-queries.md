---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 手書きクエリ
topic: queries
translation-type: tm+mt
source-git-commit: 7d5d98d8e32607abf399fdc523d2b3bc99555507

---


# クエリ業務に関する一般クエリ

このドキュメントでは、Adobe Experience Platform Platform Serviceでクエリを記述する際に知っておく必要のある重要な詳細について説明します。

クエリサービスで使用されるSQL構文の詳細については、 [SQL構文のドキュメントを参照してください](../sql/syntax.md)。

## クエリ実行モデル

Adobe Experience Platformクエリサービスには、次の2つのモデルのクエリ実行があります。インタラクティブで非インタラクティブ。 インタラクティブな実行は、ビジネスインテリジェンスツールでのクエリの開発とレポートの生成に使用され、非インタラクティブな実行は、データ処理ワークフローの一部として大規模なジョブや運用クエリに使用されます。

### インタラクティブクエリの実行

クエリは、クエリサービスのUIを通じて送信するか、接続されたクライアントを通じて送信することで、イン [タラクティブに実行できま](../clients/overview.md)す。 接続されたクライアントを通じてクエリサービスを実行する場合、送信されたクエリが戻るかタイムアウトするまで、クライアントとクエリサービスの間でアクティブなセッションが実行されます。

インタラクティブクエリの実行には、次の制限があります。

| パラメーター | 制限事項 |
| --------- | ---------- |
| クエリタイムアウト | 10 分 |
| 返される最大行数 | 50,000 |
| 最大同時クエリ | 5 |

>[!NOTE] 最大行数の制限を上書きするには、を `LIMIT 0` クエリに含めます。 10分のクエリタイムアウトが適用されます。

デフォルトでは、インタラクティブクエリの結果はクライアントに返され、永続 **化されませ** ん。 結果をExperience Platformのデータセットとして永続化するには、クエリが構文を使用する必要があ `CREATE TABLE AS SELECT` ります。

### 非インタラクティブクエリの実行

クエリサービスAPIを通じて送信されたクエリは、非インタラクティブに実行されます。 非インタラクティブ実行とは、クエリサービスがAPI呼び出しを受け取り、受け取った順序でクエリを実行することです。 非インタラクティブクエリは、常にExperience Platformで新しいデータセットを生成して結果を受け取るか、既存のデータセットに新しい行を挿入します。

## オブジェクト内の特定のフィールドへのアクセス

クエリ内のオブジェクト内のフィールドにアクセスするには、ドット表記(`.`)または角括弧表記(`[]`)を使用します。 次のSQL文は、ドット表記を使用して、オブジェクトを下に `endUserIds` 移動してオブジェクトを表示 `mcid` します。

```sql
SELECT endUserIds._experience.mcid
FROM {ANALYTICS_TABLE_NAME}
WHERE endUserIds._experience.mcid IS NOT NULL
LIMIT 1
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{ANALYTICS_TABLE_NAME}` | 解析テーブルの名前。 |

次のSQL文は、括弧表記を使用して、オブジェクトを下に `endUserIds` 移動してオブジェクトを表示 `mcid` します。

```sql
SELECT endUserIds['_experience']['mcid']
FROM {ANALYTICS_TABLE_NAME}
WHERE endUserIds._experience.mcid IS NOT NULL
LIMIT 1
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{ANALYTICS_TABLE_NAME}` | 解析テーブルの名前。 |

>[!NOTE] 各表記タイプは同じ結果を返すので、使用する場合は好みに応じて選択します。

上記の例のクエリはどちらも、単一の値ではなく、分割・統合されたオブジェクトを返します。

```console
              endUserIds._experience.mcid   
--------------------------------------------------------
 (48168239533518554367684086979667672499,"(ECID)",true)
(1 row)
```

返されるオブジ `endUserIds._experience.mcid` ェクトには、次のパラメーターに対応する値が含まれます。

- `id`
- `namespace`
- `primary`

列がオブジェクトまでしか宣言されていない場合、オブジェクト全体を文字列として返します。 IDのみを表示するには、次を使用します。

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

この節では、一重引用符、重複引用符、逆引用符を使用するタイミングをクエリします。

### 一重引用符

一重引用符(`'`)は、テキスト文字列の作成に使用されます。 例えば、この変数をステートメント内で使 `SELECT` 用して、結果に静的なテキスト値を返し、列の内容を評価す `WHERE` る句内で使用することができます。

次のクエリは、列の静的テキスト値(`'datasetA'`)を宣言します。

```sql
SELECT 
  'datasetA',
  timestamp,
  web.webPageDetails.name
FROM {ANALYTICS_TABLE_NAME}
LIMIT 10
```

次のクエリでは、WHERE句で一重引用符で囲まれた文字列(`'homepage'`)を使用して、特定のページのイベントを返します。

```sql
SELECT 
  timestamp,
  endUserIds._experience.mcid.id
FROM {ANALYTICS_TABLE_NAME}
WHERE web.webPageDetails.name = 'homepage'
LIMIT 10
```

### 重複引用符

重複の引用符(`"`)は、識別子をスペースで宣言するために使用されます。

次のクエリでは、重複の引用符を使用して、1つの列の識別子にスペースが含まれている場合に、指定した列の値を返します。

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

>[!NOTE] 重複の引用 **符は** 、ドット表記のフィールドアクセスでは使用できません。

### 引用符

バッククオートは、ドッ `` ` `` ト表記の構文を使用する場合にのみ、 **予約列名** をエスケープするために使用します。 例えば、はSQLで予約 `order` 語なので、バッククォートを使用してフィールドにアクセスする必要がありま `commerce.order`す。

```sql
SELECT 
  commerce.`order`
FROM {ANALYTICS_TABLE_NAME}
LIMIT 10
```

逆引用符は、数字を持つフィールドにアクセスするためにも開始が使用されます。 例えば、フィールドにアクセスする `30_day_value`には、逆引用符表記を使用する必要があります。

```SQL
SELECT
    commerce.`30_day_value`
FROM {ANALYTICS_TABLE_NAME}
LIMIT 10
```

角括弧表記を使 **用する** 場合は、逆引用符は不要です。

```sql
 SELECT
  commerce['order']
 FROM {ANALYTICS_TABLE_NAME}
 LIMIT 10
```

## 次の手順

このドキュメントを読むことで、クエリサービスを使用してクエリを記述する際の重要な考慮事項について説明します。 SQL構文を使用して独自のクエリを記述する方法の詳細については、 [SQL構文のドキュメントを参照してください](../sql/syntax.md)。