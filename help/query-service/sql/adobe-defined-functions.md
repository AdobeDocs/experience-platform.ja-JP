---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；アドビ定義関数；sql;
solution: Experience Platform
title: クエリ・サービスのAdobe定義SQL関数
topic-legacy: functions
description: このドキュメントでは、Adobe Experience Platformクエリサービスで使用できるAdobe定義関数について説明します。
exl-id: 275aa14e-f555-4365-bcd6-0dd6df2456b3
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '2913'
ht-degree: 26%

---

# クエリサービスのAdobe定義SQL関数

ADFと呼ばれるAdobe定義関数は、Adobe Experience Platformクエリサービスで事前に構築された関数で、[!DNL Experience Event]データに対する一般的なビジネス関連タスクの実行に役立ちます。 例えば、[セッション化](https://experienceleague.adobe.com/docs/analytics/components/virtual-report-suites/vrs-mobile-visit-processing.html)と[アトリビューション](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/attribution/overview.html)の関数は、Adobe Analyticsで見つかるのと同じように、セッション化との関数です。

このドキュメントは、[!DNL Query Service]で利用できるAdobe定義関数の情報を提供します。

## 窓関数 {#window-functions}

ビジネスロジックの大部分は、顧客のタッチポイントを収集し、時間順に並べる必要があります。[!DNL Spark] SQLでは、このサポートはウィンドウ関数の形式で提供されます。 窓関数は標準 SQL の一部で、他の多くの SQL エンジンでサポートされています。

窓関数は、集計を更新し、順序付けられたサブセットの各行の 1 つの項目を返します。最も基本的な集計関数は `SUM()` です。`SUM()` は行を取得し、合計 1 つを提供します。代わりに `SUM()` をウィンドウに適用して、窓関数に変換すると、各行の累積合計が返されます。

[!DNL Spark] SQLヘルパーの大部分は、ウィンドウ内の各行を更新し、その行の状態を追加するウィンドウ関数です。

**クエリ構文**

```sql
OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 | 例 |
| --------- | ----------- | ------- |
| `{PARTITION}` | 列または使用可能なフィールドに基づく行のサブグループ。 | `PARTITION BY endUserIds._experience.mcid.id` |
| `{ORDER}` | サブセットまたは行の順序を指定する列または使用可能なフィールド。 | `ORDER BY timestamp` |
| `{FRAME}` | パーティション内の行のサブグループ。 | `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` |

## セッション化

Webサイト、モバイルアプリ、対話型音声応答システム、またはその他の顧客対話型チャネルからの[!DNL Experience Event]データを扱う場合、関連するアクティビティの周りにイベントをグループ化できると便利です。 通常、製品の調査、請求書の支払い、口座残高の確認、アプリケーションへの入力などのアクティビティを推進する特定の目的があります。

このグループ化（データのセッション化）は、顧客体験に関するより詳細なコンテキストを明らかにするために、イベントを関連付けるのに役立ちます。

Adobe Analyticsでのセッション化について詳しくは、[コンテキスト対応セッション](https://experienceleague.adobe.com/docs/analytics/components/virtual-report-suites/vrs-mobile-visit-processing.html)のドキュメントを参照してください。

**クエリ構文**

```sql
SESS_TIMEOUT({TIMESTAMP}, {EXPIRATION_IN_SECONDS}) OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TIMESTAMP}` | データセット内のタイムスタンプフィールド。 |
| `{EXPIRATION_IN_SECONDS}` | 現在のセッションの終了と新しいイベントの開始を認定するために、セッション間の秒数。 |

`OVER()`関数内のパラメーターの説明は、[ウィンドウ関数セクション](#window-functions)にあります。

**クエリ例**

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

指定したサンプルクエリの場合、結果は`session`列に表示されます。 `session`列は、次のコンポーネントで構成されています。

```sql
({TIMESTAMP_DIFF}, {NUM}, {IS_NEW}, {DEPTH})
```

| パラメーター | 説明 |
| ---------- | ------------- |
| `{TIMESTAMP_DIFF}` | 現在のレコードと前のレコードの時間差（秒）。 |
| `{NUM}` | window関数の`PARTITION BY`で定義されているキーに対する、1から始まる一意のセッション番号。 |
| `{IS_NEW}` | レコードがセッションの最初であるかどうかを識別するために使用されるブール値. |
| `{DEPTH}` | セッション内の現在のレコードの深さ。 |

### SESS_開始_IF

このクエリは、現在のタイムスタンプと指定された式に基づいて、現在の行のセッションの状態を返し、現在の行の新しいセッションを開始します。

**クエリ構文**

```sql
SESS_START_IF({TIMESTAMP}, {TEST_EXPRESSION}) OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TIMESTAMP}` | データセット内のタイムスタンプフィールド。 |
| `{TEST_EXPRESSION}` | データのフィールドを確認する式。 例：`application.launches > 0` |

`OVER()`関数内のパラメーターの説明は、[ウィンドウ関数セクション](#window-functions)にあります。

**クエリ例**

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

指定したサンプルクエリの場合、結果は`session`列に表示されます。 `session`列は、次のコンポーネントで構成されています。

```sql
({TIMESTAMP_DIFF}, {NUM}, {IS_NEW}, {DEPTH})
```

| パラメーター | 説明 |
| ---------- | ------------- |
| `{TIMESTAMP_DIFF}` | 現在のレコードと前のレコードの時間差（秒）。 |
| `{NUM}` | window関数の`PARTITION BY`で定義されているキーに対する、1から始まる一意のセッション番号。 |
| `{IS_NEW}` | レコードがセッションの最初であるかどうかを識別するために使用されるブール値. |
| `{DEPTH}` | セッション内の現在のレコードの深さ。 |

### SESS_END_IF

このクエリは、現在のタイムスタンプと指定された式に基づいて、現在の行のセッションの状態を返し、現在のセッションを終了し、次の行の新しい開始をセッションします。

**クエリ構文**

```sql
SESS_END_IF({TIMESTAMP}, {TEST_EXPRESSION}) OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TIMESTAMP}` | データセット内のタイムスタンプフィールド。 |
| `{TEST_EXPRESSION}` | データのフィールドを確認する式。 例：`application.launches > 0` |

`OVER()`関数内のパラメーターの説明は、[ウィンドウ関数セクション](#window-functions)にあります。

**クエリ例**

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

指定したサンプルクエリの場合、結果は`session`列に表示されます。 `session`列は、次のコンポーネントで構成されています。

```sql
({TIMESTAMP_DIFF}, {NUM}, {IS_NEW}, {DEPTH})
```

| パラメーター | 説明 |
| ---------- | ------------- |
| `{TIMESTAMP_DIFF}` | 現在のレコードと前のレコードの時間差（秒）。 |
| `{NUM}` | window関数の`PARTITION BY`で定義されているキーに対する、1から始まる一意のセッション番号。 |
| `{IS_NEW}` | レコードがセッションの最初であるかどうかを識別するために使用されるブール値. |
| `{DEPTH}` | セッション内の現在のレコードの深さ。 |

## 帰属

顧客のアクションを成功に関連付けることは、顧客体験に影響を与える要因を理解する上で重要です。 以下のADFは、異なる有効期限設定のファーストタッチアトリビューションとラストタッチアトリビューションをサポートしています。

Adobe Analyticsのアトリビューションについて詳しくは、[!DNL Analytics]アトリビューションパネルガイドの[Attribution IQの概要](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/panels/attribution.html)を参照してください。

### ファーストタッチアトリビューション

このクエリは、ターゲット[!DNL Experience Event]データセット内の1つのチャネルに対するファーストタッチアトリビューション値と詳細を返します。 クエリは、選択したチャネルに対して返される各行のファーストタッチ値、タイムスタンプおよび属性を持つ `struct` オブジェクトを返します。

このクエリは、一連の顧客アクションに導いたインタラクションを確認する場合に役立ちます。以下の例では、[!DNL Experience Event]データの初期トラッキングコード(`em:946426`)は、最初のインタラクションであったので、顧客のアクションに対して100%(`1.0`)の責任を持ちます。

**クエリ構文**

```sql
ATTRIBUTION_FIRST_TOUCH({TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}) OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TIMESTAMP}` | データセット内のタイムスタンプフィールド。 |
| `{CHANNEL_NAME}` | 返されるオブジェクトのラベルです。 |
| `{CHANNEL_VALUE}` | クエリ－のターゲットチャネルである列またはフィールド. |

`OVER()`内のパラメーターの説明は、[ウィンドウ関数セクション](#window-functions)にあります。

**クエリ例**

```sql
SELECT endUserIds._experience.mcid.id, timestamp, marketing.trackingCode,
    ATTRIBUTION_FIRST_TOUCH(timestamp, 'Paid First', marketing.trackingCode)
      OVER(PARTITION BY endUserIds._experience.mcid.id
           ORDER BY timestamp
           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
      AS first_touch
FROM experience_events
ORDER BY endUserIds._experience.mcid.id, timestamp ASC
LIMIT 10
```

**結果**

```console
                id                 |       timestamp       | trackingCode |                   first_touch                    
-----------------------------------+-----------------------+--------------+--------------------------------------------------
 5D9D1DFBCEEBADF6-4097750903CE64DB | 2018-12-18 07:06:12.0 | em:946426    | (Paid First,em:946426,2018-12-18 07:06:12.0,1.0)
 5D9D1DFBCEEBADF6-4097750903CE64DB | 2018-12-18 07:07:02.0 | em:946426    | (Paid First,em:946426,2018-12-18 07:06:12.0,1.0)
 5D9D1DFBCEEBADF6-4097750903CE64DB | 2018-12-18 07:07:55.0 |              | (Paid First,em:946426,2018-12-18 07:06:12.0,1.0)
 5D9D1DFBCEEBADF6-4097750903CE64DB | 2018-12-18 07:08:44.0 |              | (Paid First,em:946426,2018-12-18 07:06:12.0,1.0)
 5D9D1DFBCEEBADF6-4097750903CE64DB | 2018-12-23 17:50:10.0 | em:513526    | (Paid First,em:946426,2018-12-18 07:06:12.0,1.0)
 5D9D1DFBCEEBADF6-4097750903CE64DB | 2018-12-23 17:50:43.0 | em:513526    | (Paid First,em:946426,2018-12-18 07:06:12.0,1.0)
 5D9D1DFBCEEBADF6-4097750903CE64DB | 2018-12-23 17:53:02.0 |              | (Paid First,em:946426,2018-12-18 07:06:12.0,1.0)
 5D9D1DFBCEEBADF6-4097750903CE64DB | 2018-12-26 20:37:12.0 | sms:70175    | (Paid First,em:946426,2018-12-18 07:06:12.0,1.0)
 5D9D1DFBCEEBADF6-4097750903CE64DB | 2018-12-26 20:37:57.0 |              | (Paid First,em:946426,2018-12-18 07:06:12.0,1.0)
 5D9D1DFBCEEBADF6-4097750903CE64DB | 2019-01-02 19:41:38.0 | em:526702    | (Paid First,em:946426,2018-12-18 07:06:12.0,1.0)
(10 rows)
```

指定したサンプルクエリの場合、結果は`first_touch`列に表示されます。 `first_touch`列は、次のコンポーネントで構成されています。

```sql
({NAME}, {VALUE}, {TIMESTAMP}, {FRACTION})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{NAME}` | `{CHANNEL_NAME}`は、ADFにラベルとして入力された。 |
| `{VALUE}` |  のファーストタッチである `{CHANNEL_VALUE}` から得た値[!DNL Experience Event] |
| `{TIMESTAMP}` | 最初のタッチが発生した[!DNL Experience Event]のタイムスタンプ。 |
| `{FRACTION}` | ファーストタッチの属性で、小数で表します。 |

### ラストタッチアトリビューション

このクエリは、ターゲット[!DNL Experience Event]データセット内の1つのチャネルに対するラストタッチアトリビューション値と詳細を返します。 クエリは、選択したチャネルに対して返される各行のラストタッチ値、タイムスタンプおよび属性を持つ `struct` オブジェクトを返します。

このクエリは、一連の顧客アクションの最終的なインタラクションを確認する場合に役立ちます。以下の例では、返されるオブジェクトのトラッキングコードが各[!DNL Experience Event]レコードでの最後のインタラクションです。 各コードは、最後のインタラクションであったので、顧客のアクションに対して100 %(`1.0`)の責任を負います。

**クエリ構文**

```sql
ATTRIBUTION_LAST_TOUCH({TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}) OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TIMESTAMP}` | データセット内のタイムスタンプフィールド。 |
| `{CHANNEL_NAME}` | 返されるオブジェクトのラベル。 |
| `{CHANNEL_VALUE}` | クエリ－のターゲットチャネルである列またはフィールド. |

`OVER()`内のパラメーターの説明は、[ウィンドウ関数セクション](#window-functions)にあります。

**クエリ例**

```sql
SELECT endUserIds._experience.mcid.id, timestamp, marketing.trackingCode,
    ATTRIBUTION_LAST_TOUCH(timestamp, 'trackingCode', marketing.trackingCode)
      OVER(PARTITION BY endUserIds._experience.mcid.id
           ORDER BY timestamp
           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
      AS last_touch
FROM experience_events
ORDER BY endUserIds._experience.mcid.id, timestamp ASC
```

**結果**

```console
                id                 |       timestamp       | trackingcode |                   last_touch                   
-----------------------------------+-----------------------+--------------+-------------------------------------------------
 5D9D1DFBCEEBADF6-4097750903CE64DB | 2017-12-18 07:06:12.0 | em:946426    | (Paid Last,em:946426,2017-12-18 07:06:12.0,1.0)
 5D9D1DFBCEEBADF6-4097750903CE64DB | 2017-12-18 07:07:02.0 | em:946426    | (Paid Last,em:946426,2017-12-18 07:07:02.0,1.0)
 5D9D1DFBCEEBADF6-4097750903CE64DB | 2017-12-18 07:07:55.0 |              | (Paid Last,em:946426,2017-12-18 07:07:02.0,1.0)
 5D9D1DFBCEEBADF6-4097750903CE64DB | 2017-12-18 07:08:44.0 |              | (Paid Last,em:946426,2017-12-18 07:07:02.0,1.0)
 5D9D1DFBCEEBADF6-4097750903CE64DB | 2017-12-23 17:50:10.0 | em:513526    | (Paid Last,em:513526,2017-12-23 17:50:10.0,1.0)
 5D9D1DFBCEEBADF6-4097750903CE64DB | 2017-12-23 17:50:43.0 | em:513526    | (Paid Last,em:513526,2017-12-23 17:50:43.0,1.0)
 5D9D1DFBCEEBADF6-4097750903CE64DB | 2017-12-23 17:53:02.0 |              | (Paid Last,em:513526,2017-12-23 17:50:43.0,1.0)
 5D9D1DFBCEEBADF6-4097750903CE64DB | 2017-12-26 20:37:12.0 | sms:70175    | (Paid Last,sms:70175,2017-12-26 20:37:12.0,1.0)
 5D9D1DFBCEEBADF6-4097750903CE64DB | 2017-12-26 20:37:57.0 |              | (Paid Last,sms:70175,2017-12-26 20:37:12.0,1.0)
 5D9D1DFBCEEBADF6-4097750903CE64DB | 2018-01-02 19:41:38.0 | em:526702    | (Paid Last,em:526702,2018-01-02 19:41:38.0,1.0)
(10 rows)
```

指定したサンプルクエリの場合、結果は`last_touch`列に表示されます。 `last_touch`列は、次のコンポーネントで構成されています。

```sql
({NAME}, {VALUE}, {TIMESTAMP}, {FRACTION})
```

| パラメーター | 説明 |
| ---------- | ----------- |
| `{NAME}` | `{CHANNEL_NAME}`は、ADFにラベルとして入力された。 |
| `{VALUE}` |  のラストタッチである `{CHANNEL_VALUE}` から得た値[!DNL Experience Event] |
| `{TIMESTAMP}` | `channelValue`が使用された[!DNL Experience Event]のタイムスタンプ。 |
| `{FRACTION}` | ラストタッチの属性で、小数で表します。 |

### 有効期限条件付きファーストタッチアトリビューション

このクエリは、ターゲット[!DNL Experience Event]データセット内の1つのチャネルに対するファーストタッチアトリビューション値と詳細を返します。このアトリビューション値は、条件の前後に期限が切れます。 クエリは、選択したチャネルに対して返される各行のファーストタッチ値、タイムスタンプおよび属性を持つ `struct` オブジェクトを返します。

このクエリは、選択した条件によって決定される[!DNL Experience Event]データセットの一部内で、何がインタラクションを引き起こして一連の顧客アクションを引き起こしたかを調べる場合に役立ちます。 次の例では、結果に表示される 4 日（7 月 15 日、21 日、23 日、29 日）ごとに購入が記録され（`commerce.purchases.value IS NOT NULL`）、各日の初期追跡コードは、顧客のアクションに対して 100%（`1.0`）責任があると見なされます。

**クエリ構文**

```sql
ATTRIBUTION_FIRST_TOUCH_EXP_IF(
    {TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}, {EXP_CONDITION}, {EXP_BEFORE}) 
    OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TIMESTAMP}` | データセット内のタイムスタンプフィールド。 |
| `{CHANNEL_NAME}` | 返されるオブジェクトのラベルです。 |
| `{CHANNEL_VALUE}` | クエリ－のターゲットチャネルである列またはフィールド. |
| `{EXP_CONDITION}` | チャネルの有効期限を決定する条件. |
| `{EXP_BEFORE}` | 指定した条件`{EXP_CONDITION}`の前後にチャネルの有効期限が切れるかどうかを示すboolean値です。 これは主に、セッションの有効期限切れ条件に対して有効になり、前のセッションからファーストタッチが選択されないようにします。 デフォルトでは、この値は `false` に設定されています。 |

`OVER()`関数内のパラメーターの説明は、[ウィンドウ関数セクション](#window-functions)にあります。

**クエリ例**

```sql
SELECT endUserIds._experience.mcid.id, timestamp, marketing.trackingCode,
    ATTRIBUTION_FIRST_TOUCH_EXP_IF(timestamp, 'Paid First', marketing.trackingCode, commerce.purchases.value IS NOT NULL, false)
      OVER(PARTITION BY endUserIds._experience.mcid.id
           ORDER BY timestamp
           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
      AS first_touch
FROM experience_events
ORDER BY endUserIds._experience.mcid.id, timestamp ASC
```

**結果**

```console
                id                 |       timestamp       | trackingCode |                   first_touch                    
-----------------------------------+-----------------------+--------------+--------------------------------------------------
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-15 06:04:10.0 | em:1024841   | (Paid First,em:1024841,2019-07-15 06:04:10.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-15 06:05:05.0 | em:1024841   | (Paid First,em:1024841,2019-07-15 06:04:10.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-15 06:05:35.0 |              | (Paid First,em:1024841,2019-07-15 06:04:10.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-15 06:08:30.0 |              | (Paid First,em:1024841,2019-07-15 06:04:10.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-21 18:45:10.0 | em:483339    | (Paid First,em:483339,2019-07-21 18:45:10.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-21 18:50:22.0 | em:483339    | (Paid First,em:483339,2019-07-21 18:45:10.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-21 18:56:56.0 |              | (Paid First,em:483339,2019-07-21 18:45:10.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-23 12:25:12.0 | sms:70558    | (Paid First,em:70558,2019-07-23 12:25:12.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-23 12:38:51.0 |              | (Paid First,em:70558,2019-07-23 12:25:12.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-29 21:33:30.0 | em:884210    | (Paid First,em:884210,2019-07-29 21:33:30.0,1.0)
(10 rows)
```

指定したサンプルクエリの場合、結果は`first_touch`列に表示されます。 `first_touch`列は、次のコンポーネントで構成されています。

```sql
({NAME}, {VALUE}, {TIMESTAMP}, {FRACTION})
```

| パラメーター | 説明 |
| ---------- | ----------- |
| `{NAME}` | `{CHANNEL_NAME}`は、ADFにラベルとして入力された。 |
| `{VALUE}` | `CHANNEL_VALUE}`からの値。`{EXP_CONDITION}`の前の[!DNL Experience Event]での最初のタッチです。 |
| `{TIMESTAMP}` | 最初のタッチが発生した[!DNL Experience Event]のタイムスタンプ。 |
| `{FRACTION}` | ファーストタッチの属性で、小数で表します。 |

### 有効期限が切れたファーストタッチアトリビューション

このクエリは、指定された期間のターゲット[!DNL Experience Event]データセット内の1つのチャネルのファーストタッチアトリビューション値と詳細を返します。 クエリは、選択したチャネルに対して返される各行のファーストタッチ値、タイムスタンプおよび属性を持つ `struct` オブジェクトを返します。

このクエリは、選択した時間間隔内で顧客の行動につながったインタラクションを確認する場合に役立ちます。次の例では、各顧客アクションに対して返されるファーストタッチが、過去 7 日間で最も早いインタラクションです（`expTimeout = 86400 * 7`）。

**仕様**

```sql
ATTRIBUTION_FIRST_TOUCH_EXP_TIMEOUT(
    {TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}, {EXP_TIMEOUT}) 
    OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TIMESTAMP}` | データセット内のタイムスタンプフィールド。 |
| `{CHANNEL_NAME}` | 返されるオブジェクトのラベルです。 |
| `{CHANNEL_VALUE}` | クエリ－のターゲットチャネルである列またはフィールド. |
| `{EXP_TIMEOUT}` | クエリがファーストタッチイベントを検索するチャネルイベント前の時間（秒）。 |

`OVER()`関数内のパラメーターの説明は、[ウィンドウ関数セクション](#window-functions)にあります。

**クエリ例**

```sql
SELECT endUserIds._experience.mcid.id, timestamp, marketing.trackingCode,
    ATTRIBUTION_FIRST_TOUCH_EXP_TIMEOUT(timestamp, 'Paid First', marketing.trackingCode, 86400 * 7)
      OVER(PARTITION BY endUserIds._experience.mcid.id
           ORDER BY timestamp
           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
      AS first_touch
FROM experience_events
ORDER BY endUserIds._experience.mcid.id, timestamp ASC
```

**結果**

```console
                id                 |       timestamp       | trackingCode |                   first_touch                    
-----------------------------------+-----------------------+--------------+--------------------------------------------------
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-15 06:04:10.0 | em:1024841   | (Paid First,em:1024841,2019-07-15 06:04:10.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-15 06:05:05.0 | em:1024841   | (Paid First,em:1024841,2019-07-15 06:04:10.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-15 06:05:35.0 |              | (Paid First,em:1024841,2019-07-15 06:04:10.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-15 06:08:30.0 |              | (Paid First,em:1024841,2019-07-15 06:04:10.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-21 18:45:10.0 | em:483339    | (Paid First,em:1024841,2019-07-15 06:04:10.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-21 18:50:22.0 | em:483339    | (Paid First,em:1024841,2019-07-15 06:04:10.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-21 18:56:56.0 |              | (Paid First,em:1024841,2019-07-15 06:04:10.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-23 12:25:12.0 | sms:70558    | (Paid First,em:483339,2019-07-23 12:25:12.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-23 12:38:51.0 |              | (Paid First,em:483339,2019-07-23 12:25:12.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-29 21:33:30.0 | em:884210    | (Paid First,em:884210,2019-07-29 21:33:30.0,1.0)
(10 rows)
```

指定したサンプルクエリの場合、結果は`first_touch`列に表示されます。 `first_touch`列は、次のコンポーネントで構成されています。

```sql
({NAME}, {VALUE}, {TIMESTAMP}, {FRACTION})
```

| パラメーター | 説明 |
| ---------- | ----------- |
| `{NAME}` | `{CHANNEL_NAME}`は、ADFにラベルとして入力された。 |
| `{VALUE}` | 指定した `{EXP_TIMEOUT}` 間隔内のファーストタッチである `CHANNEL_VALUE}` から得た値. |
| `{TIMESTAMP}` | 最初のタッチが発生した[!DNL Experience Event]のタイムスタンプ。 |
| `{FRACTION}` | ファーストタッチの属性で、小数で表します。 |

### 有効期限条件を含むラストタッチ属性

このクエリは、ターゲット[!DNL Experience Event]データセット内の1つのチャネルに対するラストタッチアトリビューション値と詳細を返します。この値は、条件の前後に期限が切れます。 クエリは、選択したチャネルに対して返される各行のラストタッチ値、タイムスタンプおよび属性を持つ `struct` オブジェクトを返します。

このクエリは、選択した条件によって決定される[!DNL Experience Event]データセットの一部内の一連の顧客アクションの最後のインタラクションを確認する場合に役立ちます。 次の例では、結果に表示される 4 日（7 月 15 日、21 日、23 日、29 日）ごとに購入が記録され（`commerce.purchases.value IS NOT NULL`）、各日の最後の追跡コードは、顧客のアクションに対して 100%（`1.0`）責任があると見なされます。

**クエリ構文**

```sql
ATTRIBUTION_LAST_TOUCH_EXP_IF(
    {TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}, {EXP_CONDITION}, {EXP_BEFORE}) 
    OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TIMESTAMP}` | データセット内のタイムスタンプフィールド。 |
| `{CHANNEL_NAME}` | 返されるオブジェクトのラベルです。 |
| `{CHANNEL_VALUE}` | クエリ－のターゲットチャネルである列またはフィールド. |
| `{EXP_CONDITION}` | チャネルの有効期限を決定する条件. |
| `{EXP_BEFORE}` | 指定した条件`{EXP_CONDITION}`の前後にチャネルの有効期限が切れるかどうかを示すboolean値です。 これは主に、セッションの有効期限切れ条件に対して有効になり、前のセッションからファーストタッチが選択されないようにします。 デフォルトでは、この値は `false` に設定されています。 |

**クエリ例**

```sql
SELECT endUserIds._experience.mcid.id, timestamp, marketing.trackingCode,
    ATTRIBUTION_LAST_TOUCH_EXP_IF(timestamp, 'trackingCode', marketing.trackingCode, commerce.purchases.value IS NOT NULL, false)
      OVER(PARTITION BY endUserIds._experience.mcid.id
           ORDER BY timestamp
           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
      AS last_touch
FROM experience_events
ORDER BY endUserIds._experience.mcid.id, timestamp ASC
```

**結果の例**

```console
                id                 |       timestamp       | trackingcode |                   last_touch                   
-----------------------------------+-----------------------+--------------+-------------------------------------------------
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-15 06:04:10.0 | em:1024841   | (Paid Last,em:550984,2019-07-15 06:08:30.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-15 06:05:35.0 | em:1024841   | (Paid Last,em:550984,2019-07-15 06:08:30.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-15 06:05:35.0 |              | (Paid Last,em:550984,2019-07-15 06:08:30.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-15 06:08:30.0 | em:550984    | (Paid Last,em:550984,2019-07-15 06:08:30.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-21 18:45:10.0 | em:483339    | (Paid Last,em:483339,2019-07-21 18:56:56.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-21 18:50:22.0 | em:483339    | (Paid Last,em:483339,2019-07-21 18:56:56.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-21 18:56:56.0 |              | (Paid Last,em:483339,2019-07-21 18:56:56.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-23 12:25:12.0 | sms:70558    | (Paid Last,em:380097,2019-07-23 12:38:51.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-23 12:38:51.0 | em:380097    | (Paid Last,em:380097,2019-07-23 12:38:51.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-29 21:33:30.0 | em:884210    | (Paid Last,em:884210,2019-07-29 21:33:30.0,1.0)
(10 rows)
```

指定したサンプルクエリの場合、結果は`last_touch`列に表示されます。 `last_touch`列は、次のコンポーネントで構成されています。

```sql
({NAME}, {VALUE}, {TIMESTAMP}, {FRACTION})
```

| パラメーター | 説明 |
| ---------- | ----------- |
| `{NAME}` | `{CHANNEL_NAME}`は、ADFにラベルとして入力された。 |
| `{VALUE}` | `{CHANNEL_VALUE}`からの値。`{EXP_CONDITION}`より前の[!DNL Experience Event]での最後のタッチです。 |
| `{TIMESTAMP}` | 最後のタッチが発生した[!DNL Experience Event]のタイムスタンプ。 |
| `{FRACTION}` | ラストタッチの属性で、小数で表します。 |

### 有効期限タイムアウトのあるラストタッチアトリビューション

このクエリは、指定された期間のターゲット[!DNL Experience Event]データセット内の1つのチャネルに対するラストタッチアトリビューション値と詳細を返します。 クエリは、選択したチャネルに対して返される各行のラストタッチ値、タイムスタンプおよび属性を持つ `struct` オブジェクトを返します。

このクエリは、選択した時間間隔内の最後のインタラクションを確認する場合に役立ちます。次の例では、各顧客アクションに対して返されるラストタッチが、次の 7 日間での最後のインタラクションです（`expTimeout = 86400 * 7`）。

**クエリ構文**

```sql
ATTRIBUTION_LAST_TOUCH_EXP_TIMEOUT(
    {TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}, {EXP_TIMEOUT}) 
    OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TIMESTAMP}` | データセット内のタイムスタンプフィールド。 |
| `{CHANNEL_NAME}` | 返されるオブジェクトのラベル |
| `{CHANNEL_VALUE}` | クエリ－のターゲットチャネルである列またはフィールド |
| `{EXP_TIMEOUT}` | クエリがラストタッチイベントを検索するチャネルイベント後の時間（秒）。 |

`OVER()`関数内のパラメーターの説明は、[ウィンドウ関数セクション](#window-functions)にあります。

**クエリ例**

```sql
SELECT endUserIds._experience.mcid.id, timestamp, marketing.trackingCode,
    ATTRIBUTION_LAST_TOUCH_EXP_TIMEOUT(timestamp, 'trackingCode', marketing.trackingCode, 86400 * 7)
      OVER(PARTITION BY endUserIds._experience.mcid.id
           ORDER BY timestamp
           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
      AS last_touch
FROM experience_events
ORDER BY endUserIds._experience.mcid.id, timestamp ASC
```

**結果**

```console
                id                 |       timestamp       | trackingcode |                   last_touch                   
-----------------------------------+-----------------------+--------------+-------------------------------------------------
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-15 06:04:10.0 | em:1024841   | (Paid Last,em:483339,2019-07-21 18:56:56.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-15 06:05:35.0 | em:1024841   | (Paid Last,em:483339,2019-07-21 18:56:56.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-15 06:05:35.0 |              | (Paid Last,em:483339,2019-07-21 18:56:56.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-15 06:08:30.0 |              | (Paid Last,em:483339,2019-07-21 18:56:56.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-21 18:45:10.0 | em:483339    | (Paid Last,sms:70558,2019-07-23 12:38:51.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-21 18:50:22.0 | em:483339    | (Paid Last,sms:70558,2019-07-23 12:38:51.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-21 18:56:56.0 |              | (Paid Last,sms:70558,2019-07-23 12:38:51.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-23 12:25:12.0 | sms:70558    | (Paid Last,em:884210,2019-07-29 21:33:30.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-23 12:38:51.0 |              | (Paid Last,em:884210,2019-07-29 21:33:30.0,1.0)
 7J82HGSSBNELKLD4-4107750913DE65DA | 2019-07-29 21:33:30.0 | em:884210    | (Paid Last,em:884210,2019-07-29 21:33:30.0,1.0)
(10 rows)
```

指定したサンプルクエリの場合、結果は`last_touch`列に表示されます。 `last_touch`列は、次のコンポーネントで構成されています。

```sql
({NAME}, {VALUE}, {TIMESTAMP}, {FRACTION})
```

| パラメーター | 説明 |
| ---------- | ----------- |
| `{NAME}` | `{CHANNEL_NAME}`は、ADFにラベルとして入力された。 |
| `{VALUE}` | 指定した `{EXP_TIMEOUT}` 間隔内のラストタッチである `{CHANNEL_VALUE}` から得た値 |
| `{TIMESTAMP}` | 最後のタッチが発生した[!DNL Experience Event]のタイムスタンプ。 |
| `{FRACTION}` | ラストタッチの属性で、小数で表します。 |

## パス

パスは、顧客の関与の深さを把握し、エクスペリエンスの意図したステップが設計どおりに機能していることを確認し、顧客に影響を与える潜在的な問題箇所を特定するために使用できます。

以下のADFは、前と次の関係からパス表示を確立することをサポートしています。 前のページと次のページを作成したり、複数のイベントをステップスルーしてパスを作成したりできます。

### 前のページ

特定のフィールドの、ウィンドウ内の定義されたステップ数だけ離れた前の値を決定します。例では、`WINDOW`関数は`ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`というフレームで設定され、ADFは現在の行とそれ以降の行をすべて参照するように設定されています。

**クエリ構文**

```sql
PREVIOUS({KEY}, {SHIFT}, {IGNORE_NULLS}) OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{KEY}` | イベントの列またはフィールド。 |
| `{SHIFT}` | （オプション）現在のイベントから離れたイベントの数です。 デフォルト値は1です。 |
| `{IGNORE_NULLS}` | （オプション）null `{KEY}`値を無視する必要があるかどうかを示すboolean。 デフォルト値は`false`です。 |

`OVER()`関数内のパラメーターの説明は、[ウィンドウ関数セクション](#window-functions)にあります。

**クエリ例**

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

指定したサンプルクエリの場合、結果は`previous_page`列に表示されます。 `previous_page`列内の値は、ADFで使用される`{KEY}`に基づきます。

### 次のページ

特定のフィールドの、ウィンドウ内の定義されたステップ数だけ離れた次の値を決定します。例では、`WINDOW`関数は`ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING`というフレームで設定され、ADFは現在の行とそれ以降の行をすべて参照するように設定されています。

**クエリ構文**

```sql
NEXT({KEY}, {SHIFT}, {IGNORE_NULLS}) OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{KEY}` | イベントの列またはフィールド。 |
| `{SHIFT}` | （オプション）現在のイベントから離れたイベントの数です。 デフォルト値は1です。 |
| `{IGNORE_NULLS}` | （オプション）null `{KEY}`値を無視する必要があるかどうかを示すboolean。 デフォルト値は`false`です。 |

`OVER()`関数内のパラメーターの説明は、[ウィンドウ関数セクション](#window-functions)にあります。

**クエリ例**

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

指定したサンプルクエリの場合、結果は`previous_page`列に表示されます。 `previous_page`列内の値は、ADFで使用される`{KEY}`に基づきます。

## 間隔

間隔：イベントが発生する前または発生した後の特定の期間内の潜在顧客の行動を調査できます。

### 前の一致までの時間

このクエリは、前回の一致イベントが見つかった時点からの単位を表す数値を返します。 一致するイベントが見つからなかった場合は、nullを返します。

**クエリ構文**

```sql
TIME_BETWEEN_PREVIOUS_MATCH(
    {TIMESTAMP}, {EVENT_DEFINITION}, {TIME_UNIT})
    OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TIMESTAMP}` | データセット内のすべてのイベントに入力されたタイムスタンプフィールド。 |
| `{EVENT_DEFINITION}` | 前のイベントを承認する式。 |
| `{TIME_UNIT}` | 出力の単位。 可能な値は、日、時間、分、秒です。 デフォルト値は秒です。 |

`OVER()`関数内のパラメーターの説明は、[ウィンドウ関数セクション](#window-functions)にあります。

**クエリ例**

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

指定したサンプルクエリの場合、結果は`average_minutes_since_registration`列に表示されます。 `average_minutes_since_registration`列内の値は、現在のイベントと前のバージョンの時間の差です。 時間の単位は、以前`{TIME_UNIT}`で定義されていました。

### 次の一致までの時間

このクエリは、次に一致するイベントの経過時間の単位を表す負の数を返します。 一致するイベントが見つからない場合、nullが返されます。

**クエリ構文**

```sql
TIME_BETWEEN_NEXT_MATCH({TIMESTAMP}, {EVENT_DEFINITION}, {TIME_UNIT}) OVER ({PARTITION} {ORDER} {FRAME})
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{TIMESTAMP}` | データセット内のすべてのイベントに入力されたタイムスタンプフィールド。 |
| `{EVENT_DEFINITION}` | 次のイベントを承認する式。 |
| `{TIME_UNIT}` | （オプション）出力の単位。 可能な値は、日、時間、分、秒です。 デフォルト値は秒です。 |

`OVER()`関数内のパラメーターの説明は、[ウィンドウ関数セクション](#window-functions)にあります。

**クエリ例**

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

指定したサンプルクエリの場合、結果は`average_minutes_until_order_confirmation`列に表示されます。 `average_minutes_until_order_confirmation`列の値は、現在のイベントと次のの時間の差です。 時間の単位は、以前`{TIME_UNIT}`で定義されていました。

## 次の手順

ここで説明する関数を使用して、[!DNL Query Service]を使用して独自の[!DNL Experience Event]データセットにアクセスするクエリを書き込むことができます。 [!DNL Query Service]でのオーサリングクエリについて詳しくは、[クエリの作成](../best-practices/writing-queries.md)に関するドキュメントを参照してください。

## その他のリソース

次のビデオでは、Adobe Experience PlatformインターフェイスとPSQLクライアントでクエリを実行する方法を示します。 また、このビデオでは、XDMオブジェクト内の個々のプロパティ、Adobe定義関数の使用、CREATE TABLE AS SELECT(CTAS)の使用に関する例も使用しています。

>[!VIDEO](https://video.tv.adobe.com/v/29796?quality=12&learn=on)
