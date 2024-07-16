---
keywords: Experience Platform；ホーム；人気のトピック；Query Service;Query Service；クエリの記述；クエリの記述；
solution: Experience Platform
title: クエリサービスでのクエリ実行の一般的なガイダンス
type: Tutorial
description: このドキュメントでは、Adobe Experience Platform クエリサービスでクエリを記述する際に知っておくべき重要な詳細の概要を説明します。
exl-id: a7076c31-8f7c-455e-9083-cbbb029c93bb
source-git-commit: 99cd69234006e6424be604556829b77236e92ad7
workflow-type: tm+mt
source-wordcount: '1088'
ht-degree: 40%

---

# [!DNL Query Service] でのクエリ実行の一般的なガイダンス

このドキュメントでは、Adobe Experience Platform [!DNL Query Service] でクエリを記述する際に知っておくべき重要な詳細を説明します。

[!DNL Query Service] で使用される SQL 構文について詳しくは、[SQL 構文ドキュメント ](../sql/syntax.md) を参照してください。

## クエリ実行モデル

Adobe Experience Platform [!DNL Query Service] には、インタラクティブと非インタラクティブの 2 つのクエリ実行モデルがあります。 インタラクティブな実行は、ビジネスインテリジェンスツールでのクエリの開発とレポートの生成に使用され、非インタラクティブな実行は、データ処理ワークフローの一部として大規模なジョブや運用クエリに使用されます。

### インタラクティブクエリの実行

クエリは、[!DNL Query Service] UI または [ 接続されたクライアントを使用 ](../clients/overview.md) して送信することで、インタラクティブに実行できます。 接続したクライアントを介して [!DNL Query Service] を実行すると、送信されたクエリが返されるかタイムアウトするまで、クライアントと [!DNL Query Service] の間でアクティブなセッションが実行されます。

インタラクティブクエリの実行には、次の制限があります。

| パラメーター | 制限事項 |
| --------- | ---------- |
| クエリのタイムアウト | 10 分 |
| 返される最大行数 | 50,000 |
| 最大同時クエリ | 5 |

>[!NOTE]
>
>最大行数の制限を上書きするには、クエリに `LIMIT 0` を含めます。 10 分のクエリタイムアウトは引き続き適用されます。

デフォルトでは、インタラクティブクエリの結果はクライアントに返され、永続 化&#x200B;**されません**。結果をデータセットとして [!DNL Experience Platform] に保持するには、クエリで `CREATE TABLE AS SELECT` 構文を使用する必要があります。

### 非インタラクティブクエリの実行

[!DNL Query Service] API を通じて送信されたクエリは、非インタラクティブに実行されます。 非インタラクティブ実行とは、[!DNL Query Service] が API 呼び出しを受信し、受信した順序でクエリを実行することを意味します。 非インタラクティブクエリの場合は、常に、結果を受け取る [!DNL Experience Platform] めに新しいデータセットを生成するか、既存のデータセットに新しい行を挿入します。

## オブジェクト内の特定のフィールドへのアクセス

クエリ内のオブジェクト内のフィールドにアクセスするには、ドット表記（`.`）または角括弧表記（`[]`）を使用します。次の SQL 文は、ドット表記を使用し、`endUserIds` オブジェクトを下に移動して `mcid` オブジェクトを表示します。

>[!NOTE]
>
>Experience CloudID （ECID）は、MCID とも呼ばれ、名前空間で引き続き使用されます。

```sql
SELECT endUserIds._experience.mcid
FROM {ANALYTICS_TABLE_NAME}
WHERE endUserIds._experience.mcid IS NOT NULL
AND TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')
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
AND TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')
LIMIT 1
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{ANALYTICS_TABLE_NAME}` | 解析テーブルの名前。 |

>[!NOTE]
>
>各表記タイプは同じ結果を返すので、使用する表記タイプは好みに応じて異なります。

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
AND TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')
LIMIT 1
```

```console
     endUserIds._experience.mcid.id 
----------------------------------------
 48168239533518554367684086979667672499
(1 row)
```

## 見積もり

クエリサービスのクエリ内では、一重引用符、二重引用符、逆引用符の使用方法が異なります。

### 一重引用符

一重引用符（`'`）は、テキスト文字列の作成に使用されます。例えば、この変数を `SELECT` ステートメント内で使用して、結果に静的なテキスト値を返し、列の内容を評価する `WHERE` 句内で使用することができます。

次のクエリは、列の静的テキスト値（`'datasetA'`）を宣言します。

```sql
SELECT 
  'datasetA',
  timestamp,
  web.webPageDetails.name
FROM {ANALYTICS_TABLE_NAME}
WHERE TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')
LIMIT 10
```

次のクエリでは、WHERE 句において一重引用符で囲まれた文字列（`'homepage'`）を使用して、特定のページのイベントを返します。

```sql
SELECT 
  timestamp,
  endUserIds._experience.mcid.id
FROM {ANALYTICS_TABLE_NAME}
WHERE web.webPageDetails.name = 'homepage'
AND TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')
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
>ドット表記フィールドへのアクセスでは、二重引用符 **使用できません** 使用できません。

### 逆引用符

逆引用符 `` ` `` は、ドット表記の構文を使用する場合に&#x200B;**のみ**&#x200B;予約列名をエスケープするために使用します。例えば、`order` は SQL では予約語なので、逆引用符をしようしてフィールド `commerce.order` にアクセスする必要があります。

```sql
SELECT 
  commerce.`order`
FROM {ANALYTICS_TABLE_NAME}
WHERE TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')
LIMIT 10
```

逆引用符は、数字で開始するフィールドにアクセスするためにも使用されます。例えば、フィールド `30_day_value` にアクセスするには、逆引用符表記を使用する必要があります。

```SQL
SELECT
    commerce.`30_day_value`
FROM {ANALYTICS_TABLE_NAME}
WHERE TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')
LIMIT 10
```

角括弧表記を使用する場合は、逆引用符は&#x200B;**不要**&#x200B;です。

```sql
 SELECT
  commerce['order']
 FROM {ANALYTICS_TABLE_NAME}
 WHERE TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')
 LIMIT 10
```

## テーブル情報の表示

クエリサービスに接続すると、`\d` または `SHOW TABLES` コマンドを使用して、Platform 上で使用可能なすべてのテーブルを表示できます。

### 標準テーブル表示

`\d` コマンドは、テーブルを一覧表示するための標準 [!DNL PostgreSQL] ビューを表示します。 このコマンドの出力例を次に示します。

```sql
             List of relations
 Schema |       Name      | Type  |  Owner   
--------+-----------------+-------+----------
 public | luma_midvalues  | table | postgres
 public | luma_postvalues | table | postgres
(2 rows)
```

### 詳細なテーブル表示

`SHOW TABLES` コマンドは、テーブルに関するより詳細な情報を提供するカスタムコマンドです。 このコマンドの出力例を次に示します。

```sql
       name      |        dataSetId         |     dataSet    | description | resolved 
-----------------+--------------------------+----------------+-------------+----------
 luma_midvalues  | 5bac030c29bb8d12fa992e58 | Luma midValues |             | false
 luma_postvalues | 5c86b896b3c162151785b43c | Luma midValues |             | false
(2 rows)
```

### スキーマ情報

テーブル内のスキーマに関する詳細を表示するには、`\d {TABLE_NAME}` コマンドを使用します。ここで、`{TABLE_NAME}` は、スキーマ情報を表示するテーブルの名前です。

次の例は、`\d luma_midvalues` を使用する場合に表示される、`luma_midvalues` テーブルのスキーマ情報を示しています。

```sql
                         Table "public.luma_midvalues"
      Column       |             Type            | Collation | Nullable | Default 
-------------------+-----------------------------+-----------+----------+---------
 timestamp         | timestamp                   |           |          | 
 _id               | text                        |           |          | 
 productlistitems  | anyarray                    |           |          | 
 commerce          | luma_midvalues_commerce     |           |          | 
 receivedtimestamp | timestamp                   |           |          | 
 enduserids        | luma_midvalues_enduserids   |           |          | 
 datasource        | datasource                  |           |          | 
 web               | luma_midvalues_web          |           |          | 
 placecontext      | luma_midvalues_placecontext |           |          | 
 identitymap       | anymap                      |           |          | 
 marketing         | marketing                   |           |          | 
 environment       | luma_midvalues_environment  |           |          | 
 _experience       | luma_midvalues__experience  |           |          | 
 device            | device                      |           |          | 
 search            | search                      |           |          | 
```

また、テーブル名に列の名前を追加することで、特定の列に関する詳細な情報を取得できます。 これは、`\d {TABLE_NAME}_{COLUMN}` の形式で記述されます。

次の例は、`web` 列の追加情報を示しており、次のコマンドを使用して呼び出されます。`\d luma_midvalues_web`

```sql
                 Composite type "public.luma_midvalues_web"
     Column     |               Type                | Collation | Nullable | Default 
----------------+-----------------------------------+-----------+----------+---------
 webpagedetails | luma_midvalues_web_webpagedetails |           |          | 
 webreferrer    | web_webreferrer                   |           |          | 
```

## データセットの結合

複数のデータセットを結合して、他のデータセットのデータをクエリに含めることができます。

次の例では、次の 2 つのデータセット（`your_analytics_table` と `custom_operating_system_lookup`）を結合し、ページビュー数によって上位 50 個のオペレーティングシステム用の `SELECT` ステートメントを作成します。

**クエリ**

```sql
SELECT 
  b.operatingsystem AS OperatingSystem,
  SUM(a.web.webPageDetails.pageviews.value) AS PageViews
FROM your_analytics_table a 
     JOIN custom_operating_system_lookup b 
      ON a._experience.analytics.environment.operatingsystemID = b.operatingsystemid 
WHERE TIMESTAMP >= TO_TIMESTAMP('2018-01-01') AND TIMESTAMP <= TO_TIMESTAMP('2018-12-31')
GROUP BY OperatingSystem 
ORDER BY PageViews DESC
LIMIT 50;
```

**結果**

| Os | PageView |
| --------------- | --------- |
| Windows 7 | 2781979.0 |
| Windows XP | 1669824.0 |
| Windows 8 | 420024.0 |
| Adobe AIR | 315032.0 |
| Windows Vista | 173566.0 |
| Mobile iOS 6.1.3 | 119069.0 |
| Linux | 56516.0 |
| OSX 10.6.8 | 53652.0 |
| Android 4.0.4 | 46167.0 |
| Android 4.0.3 | 31852.0 |
| Windows Server 2003 および XP x64 Edition | 28883.0 |
| Android 4.1.1 | 24336.0 |
| Android 2.3.6 | 15735.0 |
| OSX 10.6 | 13357.0 |
| Windows Phone 7.5 | 11054.0 |
| Android 4.3 | 9221.0 |

## 重複の除外

クエリサービスでは、データの重複排除、またはデータからの重複行の削除をサポートしています。 重複排除について詳しくは、[ クエリサービスの重複排除ガイド ](../key-concepts/deduplication.md) を参照してください。

## クエリサービスでのタイムゾーンの計算

クエリサービスは、UTC タイムスタンプ形式を使用して、Adobe Experience Platformで永続化されたデータを標準化します。 タイムゾーン要件を UTC タイムスタンプに変換する方法と UTC タイムスタンプから変換する方法について詳しくは、[FAQ タイムゾーンを UTC タイムスタンプに変更する方法と UTC タイムスタンプから変更する方法 ](../troubleshooting-guide.md#How-do-I-change-the-time-zone-to-and-from-a-UTC-Timestamp?) の節を参照してください。

## 次の手順

このドキュメントでは、[!DNL Query Service] を使用してクエリを記述する際の重要な考慮事項について説明しました。 SQL 構文を使用して独自のクエリを記述する方法の詳細については、[SQL構文のドキュメント](../sql/syntax.md)を参照してください。

クエリサービス内で使用できるクエリの例について詳しくは、次のユースケースドキュメントを参照してください。

- [Analytics insights](../use-cases/analytics-insights.md)
- [イベントのトレンドレポートの作成](../use-cases/trended-report-of-events.md)
- [訪問者のロールアップレポートの表示](../use-cases/roll-up-report-of-a-visitor.md)
- [ユーザーのページビューのリスト](../use-cases/list-visitor-sessions.md)
- [ページビュー数別の訪問者のリスト](../use-cases/visitors-by-number-of-page-views.md)
