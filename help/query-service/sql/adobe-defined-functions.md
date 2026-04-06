---
keywords: Experience Platform；ホーム；人気のトピック；クエリサービス；クエリサービス；アドビ定義関数；sql;
solution: Experience Platform
title: クエリサービスでのAdobe定義のSQL関数
description: このドキュメントでは、Adobe Experience Platform クエリサービスで使用できるAdobe定義の関数について説明します。
exl-id: 275aa14e-f555-4365-bcd6-0dd6df2456b3
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '1468'
ht-degree: 13%

---

# クエリサービスでのAdobe定義のSQL関数

Adobeで定義された関数（ADFと呼ばれます）は、Adobe Experience Platform Query Serviceの事前定義済みの関数で、[!DNL Experience Event]個のデータに対して一般的なビジネス関連タスクを実行するのに役立ちます。 これらは、[&#x200B; セッション化](https://experienceleague.adobe.com/docs/analytics/components/virtual-report-suites/vrs-mobile-visit-processing.html?lang=ja)および[&#x200B; アトリビューション &#x200B;](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/attribution/overview.html?lang=ja)の関数を、Adobe Analyticsで見つかったものと同様に含みます。

このドキュメントでは、[!DNL Query Service]で利用できるAdobe定義の関数について説明します。

>[!NOTE]
>
>Experience Cloud ID （ECID）はMCIDとも呼ばれ、ネームスペースで引き続き使用されます。

## 窓関数 {#window-functions}

ビジネスロジックの大部分は、顧客のタッチポイントを収集し、時間順に並べる必要があります。このサポートは、ウィンドウ関数の形式で[!DNL Spark] SQLによって提供されます。 窓関数は標準 SQL の一部で、他の多くの SQL エンジンでサポートされています。

窓関数は、集計を更新し、順序付けられたサブセットの各行の 1 つの項目を返します。最も基本的な集計関数は `SUM()` です。`SUM()` は行を取得し、合計 1 つを提供します。代わりに `SUM()` をウィンドウに適用して、窓関数に変換すると、各行の累積合計が返されます。

[!DNL Spark] SQL ヘルパーの大部分は、ウィンドウ内の各行を更新し、その行の状態を追加するウィンドウ関数です。

**クエリ構文**

```sql
OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 | 例 |
| --------- | ----------- | ------- |
| `{PARTITION}` | 列または使用可能なフィールドに基づく行のサブグループ。 | `PARTITION BY endUserIds._experience.mcid.id` |
| `{ORDER}` | サブセットまたは行の順序に使用する列または使用可能なフィールド。 | `ORDER BY timestamp` |
| `{FRAME}` | パーティション内の行のサブグループ。 | `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` |

## セッション化

Web サイト、モバイルアプリケーション、インタラクティブ音声応答システム、またはその他の顧客インタラクションチャネルから送信される[!DNL Experience Event] データを操作する場合、関連するアクティビティの期間を中心にイベントをグループ化できるかどうかを確認できます。 通常、製品調査、請求書の支払い、アカウント残高の確認、申請の入力など、特定の意図にもとづいてアクティビティを推進します。

データをグループ化して分類することで、イベントを関連付けて、顧客体験に関する詳細なコンテキストを明らかにできます。

Adobe Analyticsでのセッション化について詳しくは、[&#x200B; コンテキスト対応セッション &#x200B;](https://experienceleague.adobe.com/docs/analytics/components/virtual-report-suites/vrs-mobile-visit-processing.html?lang=ja)のドキュメントを参照してください。

**クエリ構文**

```sql
SESS_TIMEOUT({TIMESTAMP}, {EXPIRATION_IN_SECONDS}) OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TIMESTAMP}` | データセット内で見つかったタイムスタンプフィールド。 |
| `{EXPIRATION_IN_SECONDS}` | 現在のセッションの終了と新しいセッションの開始を検証するためにイベント間に必要な秒数。 |

`OVER()`関数内のパラメーターの説明については、[&#x200B; ウィンドウ関数セクション &#x200B;](#window-functions)を参照してください。

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
|----------------------------------+-----------------------+--------------------
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

指定されたサンプルクエリの場合、結果は`session`列に表示されます。 `session`列は、次のコンポーネントで構成されています。

```sql
({TIMESTAMP_DIFF}, {NUM}, {IS_NEW}, {DEPTH})
```

| パラメーター | 説明 |
| ---------- | ------------- |
| `{TIMESTAMP_DIFF}` | 現在のレコードと前のレコードの間の時間の差（秒単位）。 |
| `{NUM}` | ウィンドウ関数の`PARTITION BY`で定義されたキーに対して、1から始まる一意のセッション番号。 |
| `{IS_NEW}` | レコードがセッションの最初のレコードかどうかを識別するために使用されるブール値。 |
| `{DEPTH}` | セッション内の現在のレコードの深さ。 |

### SESS_START_IF

このクエリは、現在のタイムスタンプと指定された式に基づいて、現在の行のセッションの状態を返し、現在の行で新しいセッションを開始します。

**クエリ構文**

```sql
SESS_START_IF({TIMESTAMP}, {TEST_EXPRESSION}) OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TIMESTAMP}` | データセット内で見つかったタイムスタンプフィールド。 |
| `{TEST_EXPRESSION}` | データのフィールドをチェックする式。 例：`application.launches > 0` |

`OVER()`関数内のパラメーターの説明については、[&#x200B; ウィンドウ関数セクション &#x200B;](#window-functions)を参照してください。

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
|----------------------------------+-----------------------+----------+--------------------
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

指定されたサンプルクエリの場合、結果は`session`列に表示されます。 `session`列は、次のコンポーネントで構成されています。

```sql
({TIMESTAMP_DIFF}, {NUM}, {IS_NEW}, {DEPTH})
```

| パラメーター | 説明 |
| ---------- | ------------- |
| `{TIMESTAMP_DIFF}` | 現在のレコードと前のレコードの間の時間の差（秒単位）。 |
| `{NUM}` | ウィンドウ関数の`PARTITION BY`で定義されたキーに対して、1から始まる一意のセッション番号。 |
| `{IS_NEW}` | レコードがセッションの最初のレコードかどうかを識別するために使用されるブール値。 |
| `{DEPTH}` | セッション内の現在のレコードの深さ。 |

### SESS_END_IF

このクエリは、現在のタイムスタンプと指定された式に基づいて、現在の行のセッションの状態を返し、現在のセッションを終了し、次の行で新しいセッションを開始します。

**クエリ構文**

```sql
SESS_END_IF({TIMESTAMP}, {TEST_EXPRESSION}) OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TIMESTAMP}` | データセット内で見つかったタイムスタンプフィールド。 |
| `{TEST_EXPRESSION}` | データのフィールドをチェックする式。 例：`application.launches > 0` |

`OVER()`関数内のパラメーターの説明については、[&#x200B; ウィンドウ関数セクション &#x200B;](#window-functions)を参照してください。

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
|----------------------------------+-----------------------+----------+--------------------
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

指定されたサンプルクエリの場合、結果は`session`列に表示されます。 `session`列は、次のコンポーネントで構成されています。

```sql
({TIMESTAMP_DIFF}, {NUM}, {IS_NEW}, {DEPTH})
```

| パラメーター | 説明 |
| ---------- | ------------- |
| `{TIMESTAMP_DIFF}` | 現在のレコードと前のレコードの間の時間の差（秒単位）。 |
| `{NUM}` | ウィンドウ関数の`PARTITION BY`で定義されたキーに対して、1から始まる一意のセッション番号。 |
| `{IS_NEW}` | レコードがセッションの最初のレコードかどうかを識別するために使用されるブール値。 |
| `{DEPTH}` | セッション内の現在のレコードの深さ。 |


## パス

経路は、顧客のエンゲージメントの深さを把握し、エクスペリエンスの意図されたステップが設計どおりに機能していることを確認し、顧客に影響を与える可能性のある課題を特定するために使用できます。

次のADFは、以前と次の関係からのパスビューの確立をサポートしています。 前のページや次のページを作成したり、複数のイベントを順を追ってパスを作成したりすることができます。

### 前のページ

特定のフィールドの、ウィンドウ内の定義されたステップ数だけ離れた前の値を決定します。この例では、`WINDOW`関数がADFを設定する`ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`のフレームで設定されており、現在の行とそれ以降のすべての行を確認できることに注意してください。

**クエリ構文**

```sql
PREVIOUS({KEY}, {SHIFT}, {IGNORE_NULLS}) OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{KEY}` | イベントの列またはフィールド。 |
| `{SHIFT}` | （オプション）現在のイベントから離れたイベントの数。 デフォルトでは、値は1です。 |
| `{IGNORE_NULLS}` | （オプション） null `{KEY}`値を無視する必要があるかどうかを示すブール値。 デフォルトでは、値は`false`です。 |

`OVER()`関数内のパラメーターの説明については、[&#x200B; ウィンドウ関数セクション &#x200B;](#window-functions)を参照してください。

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
|-----------------------------------+-----------------------+-------------------------------------+-----------------------------------------------------
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

指定されたサンプルクエリの場合、結果は`previous_page`列に表示されます。 `previous_page`列内の値は、ADFで使用されている`{KEY}`に基づいています。

### 次のページ

特定のフィールドの、ウィンドウ内の定義されたステップ数だけ離れた次の値を決定します。この例では、`WINDOW`関数がADFを設定する`ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING`のフレームで設定されており、現在の行とそれ以降のすべての行を確認できることに注意してください。

**クエリ構文**

```sql
NEXT({KEY}, {SHIFT}, {IGNORE_NULLS}) OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{KEY}` | イベントの列またはフィールド。 |
| `{SHIFT}` | （オプション）現在のイベントから離れたイベントの数。 デフォルトでは、値は1です。 |
| `{IGNORE_NULLS}` | （オプション） null `{KEY}`値を無視する必要があるかどうかを示すブール値。 デフォルトでは、値は`false`です。 |

`OVER()`関数内のパラメーターの説明については、[&#x200B; ウィンドウ関数セクション &#x200B;](#window-functions)を参照してください。

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
|-----------------------------------+-----------------------+-------------------------------------+---------------------------------------
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

指定されたサンプルクエリの場合、結果は`previous_page`列に表示されます。 `previous_page`列内の値は、ADFで使用されている`{KEY}`に基づいています。

## 間隔

時間間隔では、イベントが発生する前または後の特定の期間内に、潜在的な顧客行動を調査できます。

### 前の一致までの時間

このクエリは、前の一致イベントが表示されてからの時間の単位を表す数値を返します。 一致するイベントが見つからなかった場合は、nullを返します。

**クエリ構文**

```sql
TIME_BETWEEN_PREVIOUS_MATCH(
    {TIMESTAMP}, {EVENT_DEFINITION}, {TIME_UNIT})
    OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TIMESTAMP}` | すべてのイベントに入力されたデータセット内に見つかったタイムスタンプフィールド。 |
| `{EVENT_DEFINITION}` | 前のイベントを修飾する式。 |
| `{TIME_UNIT}` | 出力の単位。 可能な値には、日、時間、分、秒が含まれます。 デフォルトでは、値は秒です。 |

`OVER()`関数内のパラメーターの説明については、[&#x200B; ウィンドウ関数セクション &#x200B;](#window-functions)を参照してください。

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
|-----------------------------------+------------------------------------
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

指定されたサンプルクエリの場合、結果は`average_minutes_since_registration`列に表示されます。 `average_minutes_since_registration`列内の値は、現在のイベントと前のイベントの時間の差です。 時間の単位は、以前に`{TIME_UNIT}`で定義されていました。

### 次の一致までの時間

このクエリは、次に一致するイベントの背後にある時間の単位を表す負の数値を返します。 一致するイベントが見つからない場合は、nullが返されます。

**クエリ構文**

```sql
TIME_BETWEEN_NEXT_MATCH({TIMESTAMP}, {EVENT_DEFINITION}, {TIME_UNIT}) OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TIMESTAMP}` | すべてのイベントに入力されたデータセット内に見つかったタイムスタンプフィールド。 |
| `{EVENT_DEFINITION}` | 次のイベントを修飾する式。 |
| `{TIME_UNIT}` | （オプション）出力の単位。 可能な値には、日、時間、分、秒が含まれます。 デフォルトでは、値は秒です。 |

`OVER()`関数内のパラメーターの説明については、[&#x200B; ウィンドウ関数セクション &#x200B;](#window-functions)を参照してください。

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
|-----------------------------------+------------------------------------------
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

指定されたサンプルクエリの場合、結果は`average_minutes_until_order_confirmation`列に表示されます。 `average_minutes_until_order_confirmation`列内の値は、現在のイベントと次のイベントの時間の差です。 時間の単位は、以前に`{TIME_UNIT}`で定義されていました。

## 次の手順

ここで説明した関数を使用して、[!DNL Experience Event]を使用して独自の[!DNL Query Service] データセットにアクセスするためのクエリを作成できます。 [!DNL Query Service]でのクエリのオーサリングについて詳しくは、[&#x200B; クエリの作成](../best-practices/writing-queries.md)に関するドキュメントを参照してください。

## その他のリソース

次のビデオでは、Adobe Experience Platform インターフェイスおよび PSQL クライアントでクエリを実行する方法を説明します。 さらに、このビデオでは、XDM オブジェクト内の個々のプロパティ、Adobe定義の関数の使用、CREATE TABLE AS SELECT （CTAS）の使用に関する例も使用しています。

>[!VIDEO](https://video.tv.adobe.com/v/34782?captions=jpn&quality=12&learn=on)
