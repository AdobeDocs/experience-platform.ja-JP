---
keywords: Experience Platform；ホーム；人気のあるトピック；クエリサービス；クエリサービス；クエリの書き込み；クエリの書き込み；
solution: Experience Platform
title: クエリ業務におけるクエリの実施に関する一般指導
topic-legacy: queries
type: Tutorial
description: このドキュメントでは、Adobe Experience Platform クエリサービスでクエリを記述する際に知っておく必要のある重要な詳細について説明します。
exl-id: a7076c31-8f7c-455e-9083-cbbb029c93bb
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '976'
ht-degree: 50%

---

# [!DNL Query Service]でのクエリの実行に関する一般的なガイダンス

このドキュメントは、Adobe Experience Platform[!DNL Query Service]でクエリを書く際に知っておくべき重要な詳細を詳細に説明しています。

[!DNL Query Service]で使用されるSQL構文の詳細については、[SQL構文ドキュメント](../sql/syntax.md)を参照してください。

## クエリ実行モデル

Adobe Experience Platform[!DNL Query Service]にはクエリ実行の2つのモデルがあります。インタラクティブと非インタラクティブ インタラクティブな実行は、ビジネスインテリジェンスツールでのクエリの開発とレポートの生成に使用され、非インタラクティブな実行は、データ処理ワークフローの一部として大規模なジョブや運用クエリに使用されます。

### インタラクティブクエリの実行

クエリは、[!DNL Query Service] UIを通して送信するか、[接続されたクライアント](../clients/overview.md)を通じて送信することで、対話形式で実行できます。 接続されたクライアントを介して[!DNL Query Service]を実行すると、送信されたクエリが戻るかタイムアウトするまで、クライアントと[!DNL Query Service]の間でアクティブなセッションが実行されます。

インタラクティブクエリの実行には、次の制限があります。

| パラメーター | 制限事項 |
| --------- | ---------- |
| クエリのタイムアウト | 10 分 |
| 返される最大行数 | 50,000 |
| 最大同時クエリ | 5 |

>[!NOTE]
>
> 最大行数の制限を上書きするには、`LIMIT 0` をクエリに含めます。10 分のクエリタイムアウトは引き続き適用されます。

デフォルトでは、インタラクティブクエリの結果はクライアントに返され、永続 化&#x200B;**されません**。[!DNL Experience Platform]で結果をデータセットとして保持するには、クエリーで`CREATE TABLE AS SELECT`構文を使用する必要があります。

### 非インタラクティブクエリの実行

[!DNL Query Service] API経由で送信されたクエリは、非対話的に実行されます。 非インタラクティブ実行とは、[!DNL Query Service]がAPI呼び出しを受け取り、受け取った順にクエリを実行することを意味します。 非インタラクティブクエリを使用すると、常に[!DNL Experience Platform]に新しいデータセットが生成されて結果が取得されるか、既存のデータセットに新しい行が挿入されます。

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

## 見積もり

一重引用符、重複引用符、逆引用符は、クエリサービスクエリ内では異なる使用法を持ちます。

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

## 表情報の表示

クエリサービスに接続すると、プラットフォーム上で`\d`または`SHOW TABLES`コマンドを使用して、使用可能なすべてのテーブルを表示できます。

### 標準テーブル表示

`\d`コマンドは、テーブルをリストする標準的なPostgreSQL表示を示します。 このコマンドの出力例を次に示します。

```sql
             List of relations
 Schema |       Name      | Type  |  Owner   
--------+-----------------+-------+----------
 public | luma_midvalues  | table | postgres
 public | luma_postvalues | table | postgres
(2 rows)
```

### 詳細な表の表示

`SHOW TABLES` コマンドは、テーブルに関する詳細な情報を提供するカスタムコマンドです。このコマンドの出力例を次に示します。

```sql
       name      |        dataSetId         |     dataSet    | description | resolved 
-----------------+--------------------------+----------------+-------------+----------
 luma_midvalues  | 5bac030c29bb8d12fa992e58 | Luma midValues |             | false
 luma_postvalues | 5c86b896b3c162151785b43c | Luma midValues |             | false
(2 rows)
```

### スキーマ情報

テーブル内のスキーマに関する詳細な情報を表示するには、`\d {TABLE_NAME}`コマンドを使用します。`{TABLE_NAME}`は、スキーマ情報を表示するテーブルの名前です。

次の例は、`luma_midvalues`テーブルのスキーマ情報を示しています。この情報は、`\d luma_midvalues`を使用すると確認できます。

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

また、特定の列の詳細については、列名をテーブル名に付加して確認できます。 これは`\d {TABLE_NAME}_{COLUMN}`の形式で書かれます。

次の例は、`web`列の追加情報を示しています。この情報は、次のコマンドを使用して呼び出します。`\d luma_midvalues_web`:

```sql
                 Composite type "public.luma_midvalues_web"
     Column     |               Type                | Collation | Nullable | Default 
----------------+-----------------------------------+-----------+----------+---------
 webpagedetails | luma_midvalues_web_webpagedetails |           |          | 
 webreferrer    | web_webreferrer                   |           |          | 
```

## データセットの結合

複数のデータセットを結合して、クエリ内の他のデータセットのデータを含めることができます。

次の例は、次の2つのデータセット（`your_analytics_table`と`custom_operating_system_lookup`）を結合し、ページ表示数別に上位50のオペレーティングシステム用の`SELECT`ステートメントを作成します。

**クエリ**

```sql
SELECT 
  b.operatingsystem AS OperatingSystem,
  SUM(a.web.webPageDetails.pageviews.value) AS PageViews
FROM your_analytics_table a 
     JOIN custom_operating_system_lookup b 
      ON a._experience.analytics.environment.operatingsystemID = b.operatingsystemid 
WHERE TIMESTAMP >= ('2018-01-01') AND TIMESTAMP <= ('2018-12-31')
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
| Windows Server 2003 および XP x64 エディション | 2883.0 |
| Android 4.1.1 | 2436.0 |
| Android 2.3.6 | 15735.0 |
| OSX 10.6 | 13357.0 |
| Windows Phone 7.5 | 11054.0 |
| Android 4.3 | 9221.0 |

## 重複排除

クエリサービスは、データの重複排除 - 重複、またはデータからの重複行の削除をサポートしています。 重複排除 - 重複の詳細については、『[クエリサービス重複排除 - 重複ガイド](./deduplication.md)』を参照してください。

## 次の手順

このドキュメントを読むことで、[!DNL Query Service]を使ってクエリを書く際の重要な考慮事項がいくつか紹介されました。 SQL 構文を使用して独自のクエリを記述する方法の詳細については、[SQL構文のドキュメント](../sql/syntax.md)を参照してください。

クエリサービス内で使用できるクエリのサンプルについては、[Adobe Analyticsサンプルクエリ](./adobe-analytics.md)、[Adobe Targetサンプルクエリ](./adobe-target.md)、または[ExperienceEventサンプルクエリ](./experience-event-queries.md)のガイドをお読みください。
