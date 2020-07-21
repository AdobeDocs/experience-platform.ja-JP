---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: クエリの作成
topic: queries
translation-type: tm+mt
source-git-commit: 3b710e7a20975880376f7e434ea4d79c01fa0ce5
workflow-type: tm+mt
source-wordcount: '643'
ht-degree: 1%

---


# ～におけるクエリ実施に関する一般的なガイダンス [!DNL Query Service]

このドキュメントでは、Adobe Experience Platformでクエリを記述する際に知っておくべき重要な詳細について説明 [!DNL Query Service]します。

で使用されるSQL構文の詳細については、SQL構文のドキュメント [!DNL Query Service]を参照してください [](../sql/syntax.md)。

## クエリ実行モデル

Adobe Experience Platform [!DNL Query Service] には、クエリ実行の2つのモデルがあります。 インタラクティブと非インタラクティブ インタラクティブ実行は、ビジネスインテリジェンスツールでのクエリの開発とレポートの生成に使用されます。一方、非インタラクティブ実行は、データ処理ワークフローの一部として、より大規模なジョブや運用クエリに使用されます。

### 対話型クエリの実行

クエリは、 [!DNL Query Service] UI経由で送信するか、接続されたクライアント経由で送信するこ [とで、対話的に実行できます](../clients/overview.md)。 接続されたクライアント [!DNL Query Service] を通じて実行する場合、アクティブなセッションは、送信されたクエリが戻るかタイムアウトになる [!DNL Query Service] まで、クライアントとの間で実行されます。

インタラクティブクエリの実行には、次の制限があります。

| パラメーター | 制限事項 |
| --------- | ---------- |
| クエリタイムアウト | 10 分 |
| 返される最大行数 | 50,000 |
| 最大同時クエリ数 | 5 |

>[!NOTE]
>
>最大行数の制限を無効にするには、クエリ `LIMIT 0` にを含めます。 10分のクエリタイムアウトは、引き続き適用されます。

デフォルトでは、インタラクティブクエリの結果はクライアントに返され、保持され **ません** 。 で結果をデータセットとして保持するに [!DNL Experience Platform]は、クエリで構文を使用する必要があり `CREATE TABLE AS SELECT` ます。

### 非インタラクティブクエリの実行

APIを通じて送信されるクエリは、非インタラクティブに実行され [!DNL Query Service] ます。 非インタラクティブ実行とは、API呼び出しを [!DNL Query Service] 受け取り、受け取った順にクエリを実行することです。 非インタラクティブクエリでは、常に、で新しいデータセットが生成されて結果が取得されるか、既存のデータセット [!DNL Experience Platform] に新しい行が挿入されます。

## オブジェクト内の特定のフィールドへのアクセス

クエリ内のオブジェクト内のフィールドにアクセスするには、ドット表記(`.`)または角括弧表記(`[]`)を使用します。 次のSQL文では、ドット表記を使用して、 `endUserIds` オブジェクトを下に移動して `mcid` オブジェクトを探します。

```sql
SELECT endUserIds._experience.mcid
FROM {ANALYTICS_TABLE_NAME}
WHERE endUserIds._experience.mcid IS NOT NULL
LIMIT 1
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{ANALYTICS_TABLE_NAME}` | 解析テーブルの名前。 |

次のSQL文では、括弧表記を使用してオブジェクトを下に移動し、 `endUserIds` オブジェクトを下に移動し `mcid` ます。

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
>各表記型は同じ結果を返すので、選択した表記型は好みに応じて使用します。

上記の例のクエリはどちらも、単一の値ではなく、分割・統合したオブジェクトを返します。

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

列がオブジェクトまで宣言されている場合は、オブジェクト全体を文字列として返します。 IDのみを表示するには、次の値を使用します。

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

## 一重引用符、重複引用符、バック引用符を使用する場合

このセクションでは、クエリで一重引用符、重複引用符、逆引用符を使用するタイミングについて説明します。

### 一重引用符

一重引用符(`'`)は、テキスト文字列の作成に使用します。 例えば、この変数を文の中で使用して、結果に静的テキスト値を返し、 `SELECT``WHERE` 句の中で列の内容を評価することができます。

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

重複の引用符(`"`)は、識別子をスペースで宣言するために使用します。

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

>[!NOTE]
>
>重複引用符 **は** 、ドット表記のフィールドアクセスでは使用できません。

### 引用符

バッククオート `` ` `` は、ドット表記の構文を使用している **** 場合にのみ予約列名をエスケープするために使用します。 例えば、SQLでは予約語 `order` なので、バッククォートを使用してフィールドにアクセスする必要があり `commerce.order`ます。

```sql
SELECT 
  commerce.`order`
FROM {ANALYTICS_TABLE_NAME}
LIMIT 10
```

逆引用符は、数値を持つ開始が入力したフィールドにアクセスする場合にも使用されます。 例えば、フィールドにアクセスするに `30_day_value`は、バッククォート表記を使用する必要があります。

```SQL
SELECT
    commerce.`30_day_value`
FROM {ANALYTICS_TABLE_NAME}
LIMIT 10
```

括弧表記を使用する場合は、 **逆引用符は必要ありません** 。

```sql
 SELECT
  commerce['order']
 FROM {ANALYTICS_TABLE_NAME}
 LIMIT 10
```

## 次の手順

このドキュメントを読むことで、を使用してクエリを記述する際の重要な考慮事項をいくつか紹介し [!DNL Query Service]ます。 SQL構文を使用して独自のクエリを作成する方法の詳細については、 [SQL構文のドキュメントを参照してください](../sql/syntax.md)。