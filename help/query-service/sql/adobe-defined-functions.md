---
keywords: Experience Platform；ホーム；人気のトピック；クエリサービス；Query Service;Adobe 定義関数；SQL;
solution: Experience Platform
title: クエリサービスのAdobe定義の SQL 関数
description: ここでは、Adobe Experience Platform クエリサービスで使用できるAdobe定義関数について説明します。
exl-id: 275aa14e-f555-4365-bcd6-0dd6df2456b3
source-git-commit: 58eadaaf461ecd9598f3f508fab0c192cf058916
workflow-type: tm+mt
source-wordcount: '1468'
ht-degree: 13%

---

# クエリサービスのAdobe定義の SQL 関数

ここで ADF と呼ばれるAdobe定義関数は、データに対して一般的なビジネス関連タスクを実行するのに役立つ、Adobe Experience Platform クエリサービスの事前定義済み関数 [!DNL Experience Event] す。 これには、Adobe Analyticsの関数 [Sessionization](https://experienceleague.adobe.com/docs/analytics/components/virtual-report-suites/vrs-mobile-visit-processing.html) や [Attribution](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/attribution/overview.html) の関数が含まれます。

この文書では、[!DNL Query Service] で使用できるAdobe定義関数について説明します。

>[!NOTE]
>
>Experience CloudID （ECID）は、MCID とも呼ばれ、名前空間で引き続き使用されます。

## 窓関数 {#window-functions}

ビジネスロジックの大部分は、顧客のタッチポイントを収集し、時間順に並べる必要があります。このサポートは、SQL[!DNL Spark] ウィンドウ関数の形式で提供されます。 窓関数は標準 SQL の一部で、他の多くの SQL エンジンでサポートされています。

窓関数は、集計を更新し、順序付けられたサブセットの各行の 1 つの項目を返します。最も基本的な集計関数は `SUM()` です。`SUM()` は行を取得し、合計 1 つを提供します。代わりに `SUM()` をウィンドウに適用して、窓関数に変換すると、各行の累積合計が返されます。

[!DNL Spark] の SQL ヘルパーの大部分は、ウィンドウ内の各行を更新するウィンドウ関数で、その行の状態が追加されています。

**クエリ構文**

```sql
OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 | 例 |
| --------- | ----------- | ------- |
| `{PARTITION}` | 列または使用可能フィールドに基づく行のサブグループ。 | `PARTITION BY endUserIds._experience.mcid.id` |
| `{ORDER}` | サブセットまたは行の順序付けに使用する列または使用可能なフィールド。 | `ORDER BY timestamp` |
| `{FRAME}` | パーティション内の行のサブグループ。 | `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` |

## セッション化

Web サイト、モバイルアプリケーション、インタラクティブ音声応答システム、その他の顧客インタラクションチャネルから生じる [!DNL Experience Event] データを操作する場合は、イベントを関連するアクティビティ期間にグループ化できると便利です。 通常は、製品の調査、請求書の支払い、口座残高の確認、申込書の入力など、アクティビティを推進する特定の意図があります。

このグループ化、つまりデータのセッション化は、イベントを関連付けて、顧客体験に関するより多くのコンテキストを明らかにするのに役立ちます。

Adobe Analyticsのセッション化について詳しくは、[ コンテキスト対応セッション ](https://experienceleague.adobe.com/docs/analytics/components/virtual-report-suites/vrs-mobile-visit-processing.html) に関するドキュメントを参照してください。

**クエリ構文**

```sql
SESS_TIMEOUT({TIMESTAMP}, {EXPIRATION_IN_SECONDS}) OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TIMESTAMP}` | データセットで見つかったタイムスタンプ フィールド。 |
| `{EXPIRATION_IN_SECONDS}` | 現在のセッションの終了と新しいセッションの開始の条件を満たすためにイベント間に必要な秒数。 |

`OVER()` 関数内のパラメーターについて詳しくは、[window 関数 ](#window-functions) を参照してください。

**クエリの例**

```sql
SELECT 
  endUserIds._experience.mcid.id as id, 
  timestamp,
  SESS_TIMEOUT(timestamp, 60 * 30)
    OVER (PARTITION BY endUserIds._experience.mcid.id
        ORDER BY timestamp
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
    AS session
FROM experience_events
ORDER BY id, timestamp ASC
LIMIT 10
```

**結果**

```console
                id                |       timestamp       |      session       
----------------------------------+-----------------------+--------------------
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-01-18 06:55:53.0 | (0,1,true,1)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-01-18 06:56:51.0 | (58,1,false,2)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-01-18 06:57:47.0 | (56,1,false,3)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-01-18 06:58:27.0 | (40,1,false,4)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-01-18 06:59:22.0 | (55,1,false,5)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-02-03 01:16:23.0 | (1361821,2,true,1)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-02-03 01:17:17.0 | (54,2,false,2)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-02-03 01:18:06.0 | (49,2,false,3)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-02-03 01:18:39.0 | (33,2,false,4)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-02-03 01:19:10.0 | (31,2,false,5)
(10 rows)
```

指定したサンプルクエリの結果は、`session` 列に指定されています。 `session` の列は、次のコンポーネントで構成されます。

```sql
({TIMESTAMP_DIFF}, {NUM}, {IS_NEW}, {DEPTH})
```

| パラメーター | 説明 |
| ---------- | ------------- |
| `{TIMESTAMP_DIFF}` | 現在のレコードと前のレコードの時間差（秒）。 |
| `{NUM}` | Window 関数の `PARTITION BY` で定義されたキーの、1 から始まる一意のセッション番号。 |
| `{IS_NEW}` | レコードがセッションの最初かどうかを識別するために使用されるブール値。 |
| `{DEPTH}` | セッション内の現在のレコードの深さ。 |

### SESS_START_IF

このクエリは、指定された現在のタイムスタンプと式に基づいて、現在の行のセッションの状態を返し、現在の行で新しいセッションを開始します。

**クエリ構文**

```sql
SESS_START_IF({TIMESTAMP}, {TEST_EXPRESSION}) OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TIMESTAMP}` | データセットで見つかったタイムスタンプ フィールド。 |
| `{TEST_EXPRESSION}` | データのフィールドを確認する式。 例：`application.launches > 0`。 |

`OVER()` 関数内のパラメーターについて詳しくは、[window 関数 ](#window-functions) を参照してください。

**クエリの例**

```sql
SELECT
    endUserIds._experience.mcid.id AS id,
    timestamp,
    IF(application.launches.value > 0, true, false) AS isLaunch,
    SESS_START_IF(timestamp, application.launches.value > 0)
        OVER (PARTITION BY endUserIds._experience.mcid.id
            ORDER BY timestamp
            ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
        AS session
    FROM experience_events
    ORDER BY id, timestamp ASC
    LIMIT 10
```

**結果**

```console
                id                |       timestamp       | isLaunch |      session       
----------------------------------+-----------------------+----------+--------------------
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-01-18 06:55:53.0 | true     | (0,1,true,1)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-01-18 06:56:51.0 | false    | (58,1,false,2)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-01-18 06:57:47.0 | false    | (56,1,false,3)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-01-18 06:58:27.0 | true     | (40,2,true,1)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-01-18 06:59:22.0 | false    | (55,2,false,2)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-02-03 01:16:23.0 | false    | (1361821,2,false,3)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-02-03 01:17:17.0 | false    | (54,2,false,4)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-02-03 01:18:06.0 | false    | (49,2,false,5)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-02-03 01:18:39.0 | false    | (33,2,false,6)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-02-03 01:19:10.0 | false    | (31,2,false,7)
(10 rows)
```

指定したサンプルクエリの結果は、`session` 列に指定されています。 `session` の列は、次のコンポーネントで構成されます。

```sql
({TIMESTAMP_DIFF}, {NUM}, {IS_NEW}, {DEPTH})
```

| パラメーター | 説明 |
| ---------- | ------------- |
| `{TIMESTAMP_DIFF}` | 現在のレコードと前のレコードの時間差（秒）。 |
| `{NUM}` | Window 関数の `PARTITION BY` で定義されたキーの、1 から始まる一意のセッション番号。 |
| `{IS_NEW}` | レコードがセッションの最初かどうかを識別するために使用されるブール値。 |
| `{DEPTH}` | セッション内の現在のレコードの深さ。 |

### SESS_END_IF

このクエリは、現在のタイムスタンプと指定された式に基づいて、現在のローのセッションの状態を返し、現在のセッションを終了して、次のローで新しいセッションを開始します。

**クエリ構文**

```sql
SESS_END_IF({TIMESTAMP}, {TEST_EXPRESSION}) OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TIMESTAMP}` | データセットで見つかったタイムスタンプ フィールド。 |
| `{TEST_EXPRESSION}` | データのフィールドを確認する式。 例：`application.launches > 0`。 |

`OVER()` 関数内のパラメーターについて詳しくは、[window 関数 ](#window-functions) を参照してください。

**クエリの例**

```sql
SELECT
    endUserIds._experience.mcid.id AS id,
    timestamp,
    IF(application.applicationCloses.value > 0 OR application.crashes.value > 0, true, false) AS isExit,
    SESS_END_IF(timestamp, application.applicationCloses.value > 0 OR application.crashes.value > 0)
        OVER (PARTITION BY endUserIds._experience.mcid.id
            ORDER BY timestamp
            ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
        AS session
    FROM experience_events
    ORDER BY id, timestamp ASC
    LIMIT 10
```

**結果**

```console
                id                |       timestamp       | isExit   |      session       
----------------------------------+-----------------------+----------+--------------------
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-01-18 06:55:53.0 | false    | (0,1,true,1)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-01-18 06:56:51.0 | false    | (58,1,false,2)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-01-18 06:57:47.0 | true     | (56,1,false,3)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-01-18 06:58:27.0 | false    | (40,2,true,1)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-01-18 06:59:22.0 | false    | (55,2,false,2)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-02-03 01:16:23.0 | false    | (1361821,2,false,3)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-02-03 01:17:17.0 | false    | (54,2,false,4)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-02-03 01:18:06.0 | false    | (49,2,false,5)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-02-03 01:18:39.0 | false    | (33,2,false,6)
 100080F22A45CB40-3A2B7A8E11096B6 | 2018-02-03 01:19:10.0 | false    | (31,2,false,7)
(10 rows)
```

指定したサンプルクエリの結果は、`session` 列に指定されています。 `session` の列は、次のコンポーネントで構成されます。

```sql
({TIMESTAMP_DIFF}, {NUM}, {IS_NEW}, {DEPTH})
```

| パラメーター | 説明 |
| ---------- | ------------- |
| `{TIMESTAMP_DIFF}` | 現在のレコードと前のレコードの時間差（秒）。 |
| `{NUM}` | Window 関数の `PARTITION BY` で定義されたキーの、1 から始まる一意のセッション番号。 |
| `{IS_NEW}` | レコードがセッションの最初かどうかを識別するために使用されるブール値。 |
| `{DEPTH}` | セッション内の現在のレコードの深さ。 |


## パス

パスを使用すると、顧客のエンゲージメントの深さを把握し、エクスペリエンスが意図した手順が設計どおりに動作していることを確認し、顧客に影響を与える潜在的な問題点を特定できます。

以下のオーストラリア国防軍は、過去および次回の関係からパスビューを確立することを支持している。 前のページと次のページを作成するか、複数のイベントを順を追ってパスを作成することができます。

### 前のページ

特定のフィールドの、ウィンドウ内の定義されたステップ数だけ離れた前の値を決定します。この例では、`WINDOW` 関数がフレームで設定され、現在のロー `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` 後続のすべてのローを参照するように ADF が設定されています。

**クエリ構文**

```sql
PREVIOUS({KEY}, {SHIFT}, {IGNORE_NULLS}) OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{KEY}` | イベントの列またはフィールド。 |
| `{SHIFT}` | （任意）現在のイベントから離れたイベントの数。 デフォルト値は 1 です。 |
| `{IGNORE_NULLS}` | （任意） null`{KEY}` 値を無視する必要があるかどうかを示すブール値。 デフォルト値は `false` です。 |

`OVER()` 関数内のパラメーターについて詳しくは、[window 関数 ](#window-functions) を参照してください。

**クエリの例**

```sql
SELECT endUserIds._experience.mcid.id, timestamp, web.webPageDetails.name
    PREVIOUS(web.webPageDetails.name, 3)
      OVER(PARTITION BY endUserIds._experience.mcid.id
           ORDER BY timestamp
           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
      AS previous_page
FROM experience_events
ORDER BY endUserIds._experience.mcid.id, timestamp ASC
```

**結果**

```console
                id                 |       timestamp       |                 name                |                    previous_page                    
-----------------------------------+-----------------------+-------------------------------------+-----------------------------------------------------
 457C3510571E5930-69AA721C4CBF9339 | 2017-11-08 17:15:28.0 |                                     | 
 457C3510571E5930-69AA721C4CBF9339 | 2017-11-08 17:53:05.0 | Home                                | 
 457C3510571E5930-69AA721C4CBF9339 | 2017-11-08 17:53:45.0 | Kids                                | (Home)
 457C3510571E5930-69AA721C4CBF9339 | 2017-11-08 19:22:34.0 |                                     | (Kids)
 457C3510571E5930-69AA721C4CBF9339 | 2017-11-08 20:01:12.0 | Home                                | 
 457C3510571E5930-69AA721C4CBF9339 | 2017-11-08 20:01:57.0 | Kids                                | (Home)
 457C3510571E5930-69AA721C4CBF9339 | 2017-11-08 20:03:36.0 | Search Results                      | (Kids)
 457C3510571E5930-69AA721C4CBF9339 | 2017-11-08 20:04:30.0 | Product Details: Pemmican Power Bar | (Search Results)
 457C3510571E5930-69AA721C4CBF9339 | 2017-11-08 20:05:27.0 | Shopping Cart: Cart Details         | (Product Details: Pemmican Power Bar)
 457C3510571E5930-69AA721C4CBF9339 | 2017-11-08 20:06:07.0 | Shopping Cart: Shipping Information | (Shopping Cart: Cart Details)
(10 rows)
```

指定したサンプルクエリの結果は、`previous_page` 列に指定されています。 `previous_page` 列内の値は、ADF で使用される `{KEY}` に基づいています。

### 次のページ

特定のフィールドの、ウィンドウ内の定義されたステップ数だけ離れた次の値を決定します。この例では、`WINDOW` 関数がフレームで設定され、現在のロー `ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING` 後続のすべてのローを参照するように ADF が設定されています。

**クエリ構文**

```sql
NEXT({KEY}, {SHIFT}, {IGNORE_NULLS}) OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{KEY}` | イベントの列またはフィールド。 |
| `{SHIFT}` | （任意）現在のイベントから離れたイベントの数。 デフォルト値は 1 です。 |
| `{IGNORE_NULLS}` | （任意） null`{KEY}` 値を無視する必要があるかどうかを示すブール値。 デフォルト値は `false` です。 |

`OVER()` 関数内のパラメーターについて詳しくは、[window 関数 ](#window-functions) を参照してください。

**クエリの例**

```sql
SELECT endUserIds._experience.aaid.id, timestamp, web.webPageDetails.name,
    NEXT(web.webPageDetails.name, 1, true)
      OVER(PARTITION BY endUserIds._experience.aaid.id
           ORDER BY timestamp
           ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING)
      AS next_page
FROM experience_events
ORDER BY endUserIds._experience.aaid.id, timestamp ASC
LIMIT 10
```

**結果**

```console
                id                 |       timestamp       |                name                 |             previous_page             
-----------------------------------+-----------------------+-------------------------------------+---------------------------------------
 457C3510571E5930-69AA721C4CBF9339 | 2017-11-08 17:15:28.0 |                                     | (Home)
 457C3510571E5930-69AA721C4CBF9339 | 2017-11-08 17:53:05.0 | Home                                | (Kids)
 457C3510571E5930-69AA721C4CBF9339 | 2017-11-08 17:53:45.0 | Kids                                | (Home)
 457C3510571E5930-69AA721C4CBF9339 | 2017-11-08 19:22:34.0 |                                     | (Home)
 457C3510571E5930-69AA721C4CBF9339 | 2017-11-08 20:01:12.0 | Home                                | (Kids)
 457C3510571E5930-69AA721C4CBF9339 | 2017-11-08 20:01:57.0 | Kids                                | (Search Results)
 457C3510571E5930-69AA721C4CBF9339 | 2017-11-08 20:03:36.0 | Search Results                      | (Product Details: Pemmican Power Bar)
 457C3510571E5930-69AA721C4CBF9339 | 2017-11-08 20:04:30.0 | Product Details: Pemmican Power Bar | (Shopping Cart: Cart Details)
 457C3510571E5930-69AA721C4CBF9339 | 2017-11-08 20:05:27.0 | Shopping Cart: Cart Details         | (Shopping Cart: Shipping Information)
 457C3510571E5930-69AA721C4CBF9339 | 2017-11-08 20:06:07.0 | Shopping Cart: Shipping Information | (Shopping Cart: Billing Information)
(10 rows)
```

指定したサンプルクエリの結果は、`previous_page` 列に指定されています。 `previous_page` 列内の値は、ADF で使用される `{KEY}` に基づいています。

## 間隔

時間間隔を使用すると、イベントが発生する前後の特定の期間内に、潜在的な顧客の行動を調べることができます。

### 前の一致までの時間

このクエリは、前の一致するイベントが表示されてからの時間の単位を表す数値を返します。 一致するイベントが見つからなかった場合は、null を返します。

**クエリ構文**

```sql
TIME_BETWEEN_PREVIOUS_MATCH(
    {TIMESTAMP}, {EVENT_DEFINITION}, {TIME_UNIT})
    OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TIMESTAMP}` | すべてのイベントに設定されているデータセットで見つかったタイムスタンプフィールド。 |
| `{EVENT_DEFINITION}` | 前のイベントを選定するための式。 |
| `{TIME_UNIT}` | 出力の単位。 日数、時間数、分数、秒数などの値を指定できます。 デフォルト値は秒です。 |

`OVER()` 関数内のパラメーターについて詳しくは、[window 関数 ](#window-functions) を参照してください。

**クエリの例**

```sql
SELECT 
  page_name,
  SUM (time_between_previous_match) / COUNT(page_name) as average_minutes_since_registration
FROM
(
SELECT 
  endUserIds._experience.mcid.id as id, 
  timestamp, web.webPageDetails.name as page_name, 
  TIME_BETWEEN_PREVIOUS_MATCH(timestamp, web.webPageDetails.name='Account Registration|Confirmation', 'minutes')
    OVER(PARTITION BY endUserIds._experience.mcid.id
       ORDER BY timestamp
       ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
    AS time_between_previous_match
FROM experience_events
)
WHERE time_between_previous_match IS NOT NULL
GROUP BY page_name
ORDER BY average_minutes_since_registration
LIMIT 10
```

**結果**

```console
             page_name             | average_minutes_since_registration 
-----------------------------------+------------------------------------
                                   |                                   
 Account Registration|Confirmation |                                0.0
 Seasonal                          |                   5.47029702970297
 Equipment                         |                  6.532110091743119
 Women                             |                  7.287081339712919
 Men                               |                  7.640918580375783
 Product List                      |                  9.387459807073954
 Unlimited Blog|February           |                  9.954545454545455
 Product Details|Buffalo           |                 13.304347826086957
 Unlimited Blog|June               |                  770.4285714285714
(10 rows)
```

指定したサンプルクエリの結果は、`average_minutes_since_registration` 列に指定されています。 `average_minutes_since_registration` 列内の値は、現在のイベントと前のイベントの時間の差です。 時間の単位は、`{TIME_UNIT}` で事前に定義されています。

### 次の一致までの時間

このクエリは、次の一致するイベントの後の時間単位を表す負の数を返します。 一致するイベントが見つからない場合は、null が返されます。

**クエリ構文**

```sql
TIME_BETWEEN_NEXT_MATCH({TIMESTAMP}, {EVENT_DEFINITION}, {TIME_UNIT}) OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TIMESTAMP}` | すべてのイベントに設定されているデータセットで見つかったタイムスタンプフィールド。 |
| `{EVENT_DEFINITION}` | 次のイベントを選定するための式。 |
| `{TIME_UNIT}` | （任意）出力の単位。 日数、時間数、分数、秒数などの値を指定できます。 デフォルト値は秒です。 |

`OVER()` 関数内のパラメーターについて詳しくは、[window 関数 ](#window-functions) を参照してください。

**クエリの例**

```sql
SELECT 
  page_name,
  SUM (time_between_next_match) / COUNT(page_name) as average_minutes_until_order_confirmation
FROM
(
SELECT 
  endUserIds._experience.mcid.id as id, 
  timestamp, web.webPageDetails.name as page_name, 
  TIME_BETWEEN_NEXT_MATCH(timestamp, web.webPageDetails.name='Shopping Cart|Order Confirmation', 'minutes')
    OVER(PARTITION BY endUserIds._experience.mcid.id
       ORDER BY timestamp
       ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING)
    AS time_between_next_match
FROM experience_events
)
WHERE time_between_next_match IS NOT NULL
GROUP BY page_name
ORDER BY average_minutes_until_order_confirmation DESC
LIMIT 10
```

**結果**

```console
             page_name             | average_minutes_until_order_confirmation 
-----------------------------------+------------------------------------------
 Shopping Cart|Order Confirmation  |                                      0.0
 Men                               |                       -9.465295629820051
 Equipment                         |                       -9.682098765432098
 Product List                      |                       -9.690661478599221
 Women                             |                       -9.759459459459459
 Seasonal                          |                                  -10.295
 Shopping Cart|Order Review        |                      -366.33567364956144
 Unlimited Blog|February           |                       -615.0327868852459
 Shopping Cart|Billing Information |                       -775.6200495367711
 Product Details|Buffalo           |                      -1274.9571428571428
(10 rows)
```

指定したサンプルクエリの結果は、`average_minutes_until_order_confirmation` 列に指定されています。 `average_minutes_until_order_confirmation` 列内の値は、現在のイベントと次のイベントの時間の差です。 時間の単位は、`{TIME_UNIT}` で事前に定義されています。

## 次の手順

ここで説明した関数を使用すると、[!DNL Query Service] を使用して独自の [!DNL Experience Event] データセットにアクセスするクエリを記述できます。 [!DNL Query Service] でのクエリの作成について詳しくは、[ クエリの作成 ](../best-practices/writing-queries.md) に関するドキュメントを参照してください。

## その他のリソース

次のビデオでは、Adobe Experience Platform インターフェイスおよび PSQL クライアントでクエリを実行する方法を説明します。 さらに、このビデオでは、XDM オブジェクト内の個々のプロパティに関連する例、Adobe定義関数の使用例、CREATE TABLE AS SELECT （CTAS）の使用例も示しています。

>[!VIDEO](https://video.tv.adobe.com/v/29796?quality=12&learn=on)
