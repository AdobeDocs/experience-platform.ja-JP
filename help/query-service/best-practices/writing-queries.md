---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；クエリの記述；クエリの記述；
solution: Experience Platform
title: クエリサービスでのクエリ実行に関する一般的なガイダンス
topic-legacy: queries
type: Tutorial
description: このドキュメントでは、Adobe Experience Platformクエリサービスでクエリを記述する際に知っておくべき重要な詳細について説明します。
exl-id: a7076c31-8f7c-455e-9083-cbbb029c93bb
source-git-commit: 9c450f340706040593dfea5292702c4b00dd9852
workflow-type: tm+mt
source-wordcount: '1047'
ht-degree: 45%

---

# でのクエリ実行に関する一般的なガイダンス [!DNL Query Service]

このドキュメントでは、Adobe Experience Platformでクエリを記述する際に知っておくべき重要な詳細について説明します。 [!DNL Query Service].

で使用される SQL 構文の詳細については、 [!DNL Query Service]を読んでください。 [SQL 構文ドキュメント](../sql/syntax.md).

## クエリ実行モデル

Adobe Experience Platform [!DNL Query Service] には、次の 2 つのモデルのクエリを実行します。インタラクティブで非インタラクティブ。 インタラクティブな実行は、ビジネスインテリジェンスツールでのクエリの開発とレポートの生成に使用され、非インタラクティブな実行は、データ処理ワークフローの一部として大規模なジョブや運用クエリに使用されます。

### インタラクティブクエリの実行

クエリは、 [!DNL Query Service] UI または [接続されたクライアントを通じて](../clients/overview.md). 実行時 [!DNL Query Service] 接続されたクライアントを通じて、クライアントと [!DNL Query Service] 送信されたクエリが返されるか、タイムアウトするまで。

インタラクティブクエリの実行には、次の制限があります。

| パラメーター | 制限事項 |
| --------- | ---------- |
| クエリのタイムアウト | 10 分 |
| 返される最大行数 | 50,000 |
| 最大同時クエリ | 5 |

>[!NOTE]
>
> 最大行数の制限を上書きするには、`LIMIT 0` をクエリに含めます。10 分のクエリタイムアウトは引き続き適用されます。

デフォルトでは、インタラクティブクエリの結果はクライアントに返され、永続 化&#x200B;**されません**。結果をデータセットとしてに保持するには [!DNL Experience Platform]を使用する場合、クエリでは `CREATE TABLE AS SELECT` 構文と同じです。

### 非インタラクティブクエリの実行

を通じて送信されたクエリ [!DNL Query Service] API は、非インタラクティブに実行されます。 非インタラクティブ実行とは、 [!DNL Query Service] は API 呼び出しを受け取り、クエリを受信した順序で実行します。 非インタラクティブクエリを使用すると、常に [!DNL Experience Platform] 結果を受け取るか、既存のデータセットに新しい行を挿入します。

## オブジェクト内の特定のフィールドへのアクセス

クエリ内のオブジェクト内のフィールドにアクセスするには、ドット表記（`.`）または角括弧表記（`[]`）を使用します。次の SQL 文は、ドット表記を使用し、`endUserIds` オブジェクトを下に移動して `mcid` オブジェクトを表示します。

>[!NOTE]
>
>Experience CloudID(ECID) は MCID とも呼ばれ、名前空間で引き続き使用されます。

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
AND TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')
LIMIT 1
```

```console
     endUserIds._experience.mcid.id 
----------------------------------------
 48168239533518554367684086979667672499
(1 row)
```

## 引用符

一重引用符、二重引用符、逆引用符は、クエリサービスクエリ内で異なる使用状況を持ちます。

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
> 二重引用符は、ドット表記のフィールドアクセスでは&#x200B;**使用できません**。

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

クエリサービスに接続すると、Platform で使用可能なすべてのテーブルを表示するには、 `\d` または `SHOW TABLES` コマンド

### 標準のテーブル表示

この `\d` コマンドは、標準を表示します。 [!DNL PostgreSQL] 表を一覧表示するためのビュー。 このコマンドの出力の例を次に示します。

```sql
             List of relations
 Schema |       Name      | Type  |  Owner   
--------+-----------------+-------+----------
 public | luma_midvalues  | table | postgres
 public | luma_postvalues | table | postgres
(2 rows)
```

### 詳細なテーブル表示

`SHOW TABLES` コマンドは、テーブルに関する詳細情報を提供するカスタムコマンドです。 このコマンドの出力の例を次に示します。

```sql
       name      |        dataSetId         |     dataSet    | description | resolved 
-----------------+--------------------------+----------------+-------------+----------
 luma_midvalues  | 5bac030c29bb8d12fa992e58 | Luma midValues |             | false
 luma_postvalues | 5c86b896b3c162151785b43c | Luma midValues |             | false
(2 rows)
```

### スキーマ情報

テーブル内のスキーマに関する詳細情報を表示するには、 `\d {TABLE_NAME}` コマンド、ここで `{TABLE_NAME}` は、スキーマ情報を表示するテーブルの名前です。

次の例は、 `luma_midvalues` 表を使用して表示する `\d luma_midvalues`:

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

さらに、特定の列に関する詳細な情報を取得するには、その列の名前をテーブル名に追加します。 これは次の形式で書き込まれます： `\d {TABLE_NAME}_{COLUMN}`.

次の例は、 `web` 列に格納され、次のコマンドを使用して呼び出されます。 `\d luma_midvalues_web`:

```sql
                 Composite type "public.luma_midvalues_web"
     Column     |               Type                | Collation | Nullable | Default 
----------------+-----------------------------------+-----------+----------+---------
 webpagedetails | luma_midvalues_web_webpagedetails |           |          | 
 webreferrer    | web_webreferrer                   |           |          | 
```

## データセットの結合

複数のデータセットを結合して、他のデータセットのデータをクエリに含めることができます。

次の例は、次の 2 つのデータセット (`your_analytics_table` および `custom_operating_system_lookup`) とをクリックし、 `SELECT` 上位 50 のオペレーティングシステムに関する文（ページ表示数別）。

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

| OperatingSystem | PageViews |
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
| Windows Server 2003 および XP x64 エディション | 28883.0 |
| Android 4.1.1 | 24336.0 |
| Android 2.3.6 | 15735.0 |
| OSX 10.6 | 13357.0 |
| Windows Phone 7.5 | 11054.0 |
| Android 4.3 | 9221.0 |

## 重複排除

クエリサービスは、データの重複排除、またはデータからの重複行の削除をサポートします。 重複排除の詳細については、 [クエリサービス重複排除ガイド](./deduplication.md).

## クエリサービスでのタイムゾーンの計算

クエリサービスは、UTC タイムスタンプ形式を使用して、Adobe Experience Platformで保持されるデータを標準化します。 タイムゾーン要件を UTC タイムスタンプに変換する方法と UTC タイムスタンプから変換する方法の詳細については、 [タイムゾーンを UTC タイムスタンプに変更する方法と UTC タイムスタンプから変更する方法に関する FAQ の節](../troubleshooting-guide.md#How-do-I-change-the-time-zone-to-and-from-a-UTC-Timestamp?).

## 次の手順

このドキュメントでは、 [!DNL Query Service]. SQL 構文を使用して独自のクエリを記述する方法の詳細については、[SQL構文のドキュメント](../sql/syntax.md)を参照してください。

クエリサービス内で使用できるクエリのその他のサンプルについては、次の使用例のドキュメントを参照してください。

- [Analytics インサイト](../use-cases/analytics-insights.md)
- [Adobe Targetでのアクティビティ分析](../use-cases/activity-analysis-with-adobe-target.md)
- [ExperienceEvent クエリ例](../sample-queries/experience-event.md).
