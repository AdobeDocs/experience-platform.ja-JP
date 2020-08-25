---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: アドビ定義関数
topic: functions
translation-type: tm+mt
source-git-commit: 38cb8eeae3ac0a1852c59e433d1cacae82b1c6c0
workflow-type: tm+mt
source-wordcount: '2156'
ht-degree: 82%

---


# アドビ定義関数

Adobe-defined functions (ADFs) are prebuilt functions in [!DNL Query Service] that help perform common business related tasks on [!DNL ExperienceEvent] data. これには、Adobe Analytics のようなセッション化および属性の関数が含まれます。Adobe Analytics と、このページで定義されている ADF の概念について詳しくは、[Adobe Analytics のドキュメント](https://docs.adobe.com/content/help/ja-JP/analytics/landing/home.html)を参照してください。This document provides information for Adobe-defined functions available in [!DNL Query Service].

## 窓関数

ビジネスロジックの大部分は、顧客のタッチポイントを収集し、時間順に並べる必要があります。This support is provided by [!DNL Spark] SQL in the form of window functions. 窓関数は標準 SQL の一部で、他の多くの SQL エンジンでサポートされています。

窓関数は、集計を更新し、順序付けられたサブセットの各行の 1 つの項目を返します。最も基本的な集計関数は `SUM()` です。`SUM()` は行を取得し、合計 1 つを提供します。代わりに `SUM()` をウィンドウに適用して、窓関数に変換すると、各行の累積合計が返されます。

The majority of the [!DNL Spark] SQL helpers are window functions that update each row in your window, with the state of that row added.

### 仕様

構文：`OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| [分割] | 列または使用可能なフィールドに基づく行のサブグループ。例, `PARTITION BY endUserIds._experience.mcid.id` |
| [order] | サブセットまたは行の順序を指定する列または使用可能なフィールド。例, `ORDER BY timestamp` |
| [frame] | パーティション内の行のサブグループ。例, `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` |

## セッション化

When you are working with [!DNL ExperienceEvent] data originating from a website, mobile application, interactive voice response system, or any other customer interaction channel it helps if events can be grouped around a related period of activity. 通常、製品の調査、請求書の支払い、口座残高の確認、アプリケーションへの入力などのアクティビティを推進する特定の目的があります。このグループ化は、顧客体験に関するイベントを関連付け、顧客体験に関するより詳細なコンテキストを明らかにするのに役立ちます。

Adobe Analytics でのセッション化について詳しくは、[コンテキスト対応セッション](https://docs.adobe.com/content/help/ja-JP/analytics/components/virtual-report-suites/vrs-mobile-visit-processing.html)に関するドキュメントを参照してください。

### 仕様

構文：`SESS_TIMEOUT(timestamp, expirationInSeconds) OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| `timestamp` | データセット内のタイムスタンプフィールド |
| `expirationInSeconds` | 現在のセッションの終了と新しいイベントの開始を決定するために必要な秒数 |

| 返されるオブジェクトパラメーター | 説明 |
| ---------------------- | ------------- |
| `timestamp_diff` | 現在のレコードと前のレコードの間の時間（秒） |
| `num` | 窓関数の `PARTITION BY` で定義されたキーの 1 から始まる一意のセッション番号。 |
| `is_new` | レコードがセッションの最初であるかどうかを識別するために使用されるブール値 |
| `depth` | セッション内の現在のレコードの深さ |

#### クエリ例

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

#### 結果

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

## 帰属

顧客のアクションを成功に関連付けることは、顧客体験に影響を与える要因を理解する上で重要な役割を果たします。次の ADF は、異なる有効期限設定で First と Last の属性をサポートしています。

Adobe の属性について詳しくは、Analytics 分析ガイドの「[Attribution IQ の概要](https://docs.adobe.com/content/help/ja-JP/analytics/analyze/analysis-workspace/panels/attribution.html)」を参照してください。[!DNL Analytics]

### ファーストタッチ属性

Returns the first touch attribution value and details for a single channel in the target [!DNL ExperienceEvent] dataset. クエリは、選択したチャネルに対して返される各行のファーストタッチ値、タイムスタンプおよび属性を持つ `struct` オブジェクトを返します。

このクエリは、一連の顧客アクションに導いたインタラクションを確認する場合に役立ちます。以下の例では、 データの最初の追跡コード（`em:946426`[!DNL ExperienceEvent]）は、最初のインタラクションであったので、顧客のアクションに対して 100%（`1.0`）責任があると見なされます。

### 仕様

構文：`ATTRIBUTION_FIRST_TOUCH(timestamp, channelName, channelValue) OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| `timestamp` | データセット内のタイムスタンプフィールド |
| `channelName` | 返されるオブジェクトのラベルとして使用するわかりやすい名前 |
| `channelValue` | クエリ－のターゲットチャネルである列またはフィールド |


| 返されるオブジェクトパラメーター | 説明 |
| ---------------------- | ------------- |
| `name` | ADF にラベルとして入力された `channelName` |
| `value` |  のファーストタッチである `channelValue` から得た値[!DNL ExperienceEvent] |
| `timestamp` | The timestamp of the [!DNL ExperienceEvent] where the first touch occured |
| `fraction` | ファーストタッチ属性は、小数のクレジットとして表されます |

#### クエリ例

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

#### 結果

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

### ラストタッチ属性

Returns the last touch attribution value and details for a single channel in the target [!DNL ExperienceEvent] dataset. クエリは、選択したチャネルに対して返される各行のラストタッチ値、タイムスタンプおよび属性を持つ `struct` オブジェクトを返します。

このクエリは、一連の顧客アクションの最終的なインタラクションを確認する場合に役立ちます。In the example shown below, the tracking code in the returned object is the last interaction in each [!DNL ExperienceEvent] record. 各コードは、最後のインタラクションであったので、顧客のアクションに対して 100%（`1.0`）責任があると見なされます。

### 仕様

構文：`ATTRIBUTION_LAST_TOUCH(timestamp, channelName, channelValue) OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| `timestamp` | データセット内のタイムスタンプフィールド |
| `channelName` | 返されるオブジェクトのラベルとして使用するわかりやすい名前 |
| `channelValue` | クエリ－のターゲットチャネルである列またはフィールド |


| 返されるオブジェクトパラメーター | 説明 |
| ---------------------- | ------------- |
| `name` | ADF にラベルとして入力された `channelName` |
| `value` |  のラストタッチである `channelValue` から得た値[!DNL ExperienceEvent] |
| `timestamp` | The timestamp of the [!DNL ExperienceEvent] where the `channelValue` was used |
| `fraction` | ラストタッチ属性は、小数のクレジットとして表されます |

#### クエリ例

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

#### 結果

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

### 有効期限条件を持つファーストタッチ属性

Returns the first touch attribution value and details for a single channel in the target [!DNL ExperienceEvent] dataset, expiring after or before a condition. クエリは、選択したチャネルに対して返される各行のファーストタッチ値、タイムスタンプおよび属性を持つ `struct` オブジェクトを返します。

This query is useful if you want to see what interaction led to a series of customer actions within a portion of the [!DNL ExperienceEvent] dataset determined by a condition of your chosing. 次の例では、結果に表示される 4 日（7 月 15 日、21 日、23 日、29 日）ごとに購入が記録され（`commerce.purchases.value IS NOT NULL`）、各日の初期追跡コードは、顧客のアクションに対して 100%（`1.0`）責任があると見なされます。

#### 仕様

構文：`ATTRIBUTION_FIRST_TOUCH_EXP_IF(timestamp, channelName, channelValue, expCondition, expBefore) OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| `timestamp` | データセット内のタイムスタンプフィールド |
| `channelName` | 返されるオブジェクトのラベルとして使用するわかりやすい名前 |
| `channelValue` | クエリ－のターゲットチャネルである列またはフィールド |
| `expCondition` | チャネルの有効期限を決定する条件 |
| `expBefore` | デフォルト値は `false` です。指定した条件が満たされる前または後にチャネルの有効期限が切れるかどうかを示すブール値です。主に、セッションの有効期限条件（例えば `sess.depth = 1, true`）に対して有効になり、前のセッションからファーストタッチが選択されないようにします。 |

| 返されるオブジェクトパラメーター | 説明 |
| ---------------------- | ------------- |
| `name` | ADF にラベルとして入力された `channelName` |
| `value` | 以前の のファーストタッチである `channelValue`[!DNL ExperienceEvent] から得た値`expCondition` |
| `timestamp` | The timestamp of the [!DNL ExperienceEvent] where the first touch occured |
| `fraction` | ファーストタッチ属性は、小数のクレジットとして表されます |

#### クエリ例

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

#### 結果

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

### 有効期限タイムアウトを持つファーストタッチ属性

Returns the first touch attribution value and details for a single channel in the target [!DNL ExperienceEvent] dataset for a specified time period. クエリは、選択したチャネルに対して返される各行のファーストタッチ値、タイムスタンプおよび属性を持つ `struct` オブジェクトを返します。このクエリは、選択した時間間隔内で顧客の行動につながったインタラクションを確認する場合に役立ちます。次の例では、各顧客アクションに対して返されるファーストタッチが、過去 7 日間で最も早いインタラクションです（`expTimeout = 86400 * 7`）。

#### 仕様

構文：`ATTRIBUTION_FIRST_TOUCH_EXP_TIMEOUT(timestamp, channelName, channelValue, expTimeout) OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| `timestamp` | データセット内のタイムスタンプフィールド |
| `channelName` | 返されるオブジェクトのラベルとして使用するわかりやすい名前 |
| `channelValue` | クエリ－のターゲットチャネルである列またはフィールド |
| `expTimeout` | クエリがファーストタッチイベントを検索するチャネルイベントの前の時間（秒） |

| 返されるオブジェクトパラメーター | 説明 |
| ---------------------- | ------------- |
| `name` | ADF にラベルとして入力された `channelName` |
| `value` | 指定した `expTimeout` 間隔内のファーストタッチである `channelValue` から得た値 |
| `timestamp` | The timestamp of the [!DNL ExperienceEvent] where the first touch occured |
| `fraction` | ファーストタッチ属性は、小数のクレジットとして表されます |

#### クエリ例

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

#### 結果

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

### 有効期限条件を持つラストタッチ属性

Returns the last touch attribution value and details for a single channel in the target [!DNL ExperienceEvent] dataset, expiring after or before a condition. クエリは、選択したチャネルに対して返される各行のラストタッチ値、タイムスタンプおよび属性を持つ `struct` オブジェクトを返します。This query is useful if you want to see the last interaction in a series of customer actions within a portion of the [!DNL ExperienceEvent] dataset determined by a condition of your chosing. 次の例では、結果に表示される 4 日（7 月 15 日、21 日、23 日、29 日）ごとに購入が記録され（`commerce.purchases.value IS NOT NULL`）、各日の最後の追跡コードは、顧客のアクションに対して 100%（`1.0`）責任があると見なされます。

#### 仕様

構文：`ATTRIBUTION_LAST_TOUCH_EXP_IF(timestamp, channelName, channelValue, expCondition, expBefore) OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| `timestamp` | データセット内のタイムスタンプフィールド |
| `channelName` | 返されるオブジェクトのラベルとして使用するわかりやすい名前 |
| `channelValue` | クエリ－のターゲットチャネルである列またはフィールド |
| `expCondition` | チャネルの有効期限を決定する条件 |
| `expBefore` | デフォルト値は `false` です。指定した条件が満たされる前または後にチャネルの有効期限が切れるかどうかを示すブール値です。主に、セッションの有効期限条件（例えば`sess.depth = 1, true`）に対して有効になり、前のセッションからラストタッチが選択されないようにします。 |

| 返されるオブジェクトパラメーター | 説明 |
| ---------------------- | ------------- |
| `name` | ADF にラベルとして入力された `channelName` |
| `value` |  以前の のラストタッチである `channelValue`[!DNL ExperienceEvent] から得た値`expCondition` |
| `timestamp` | The timestamp of the [!DNL ExperienceEvent] where the last touch occured |
| `percentage` | ラストタッチ属性は、小数のクレジットとして表されます |

#### クエリ例

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

#### 結果

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

### 有効期限タイムアウトを持つラストタッチ属性

Returns the last touch attribution value and details for a single channel in the target [!DNL ExperienceEvent] dataset for a specified time period. クエリは、選択したチャネルに対して返される各行のラストタッチ値、タイムスタンプおよび属性を持つ `struct` オブジェクトを返します。このクエリは、選択した時間間隔内の最後のインタラクションを確認する場合に役立ちます。次の例では、各顧客アクションに対して返されるラストタッチが、次の 7 日間での最後のインタラクションです（`expTimeout = 86400 * 7`）。

#### 仕様

構文：`ATTRIBUTION_LAST_TOUCH_EXP_TIMEOUT(timestamp, channelName, channelValue, expTimeout) OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| `timestamp` | データセット内のタイムスタンプフィールド |
| `channelName` | 返されるオブジェクトのラベルとして使用するわかりやすい名前 |
| `channelValue` | クエリ－のターゲットチャネルである列またはフィールド |
| `expTimeout` | クエリがラストタッチイベントを検索するチャネルイベント後の時間（秒） |

| 返されるオブジェクトパラメーター | 説明 |
| ---------------------- | ------------- |
| `name` | ADF にラベルとして入力された `channelName` |
| `value` | 指定した `expTimeout` 間隔内のラストタッチである `channelValue` から得た値 |
| `timestamp` | The timestamp of the [!DNL ExperienceEvent] where the last touch occured |
| `percentage` | ラストタッチ属性は、小数のクレジットとして表されます |

#### クエリ例

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

#### 結果

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

## 前/次のタッチ

エクスペリエンス内で顧客がどのようにナビゲートしているかを理解することが重要です。これを使用して、顧客の関与の深さを理解し、エクスペリエンスの意図されたステップが設計どおりに機能していることを確認し、顧客に影響を与える潜在的な問題点を特定できます。以下の ADF は、前と次の関係からのパス表示の確立をサポートしています。前のページと次のページを作成したり、複数のイベントを順を追ってパスを作成したりできます。

### 前のタッチ

特定のフィールドの、ウィンドウ内の定義されたステップ数だけ離れた前の値を決定します。この例では、`WINDOW` 関数は`ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` フレームで構成され、ADF は現在の行とその前のすべての行を見るように設定されています。

#### 仕様

構文：`PREVIOUS(key, [shift, [ignoreNulls]]) OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| `key` | イベントの列またはフィールド。 |
| `shift` | （オプション）現在のイベントから離れたイベントの数。初期設定は 1 です。 |
| `ingnoreNulls` | Null `key` 値を無視する必要があるかどうかを示すブール値。デフォルトは `false` です。 |


| 返されるオブジェクトパラメーター | 説明 |
| ---------------------- | ------------- |
| `value` | ADF で使用される `key` に基づく値 |

#### クエリ例

```sql
SELECT endUserIds._experience.mcid.id, _experience.analytics.session.num, timestamp, web.webPageDetails.name
    PREVIOUS(web.webPageDetails.name, 3)
      OVER(PARTITION BY endUserIds._experience.mcid.id, _experience.analytics.session.num
           ORDER BY timestamp
           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
      AS previous_page
FROM experience_events
ORDER BY endUserIds._experience.mcid.id, _experience.analytics.session.num, timestamp ASC
```

#### 結果

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

### 次のタッチ

特定のフィールドの、ウィンドウ内の定義されたステップ数だけ離れた次の値を決定します。この例では、`WINDOW` 関数は`ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING` フレームで構成され、ADF は現在の行とその後すべての行を見るように設定されています。

#### 仕様

構文：`NEXT(key, [shift, [ignoreNulls]]) OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| `key` | イベントの列またはフィールド |
| `shift` | （オプション）現在のイベントから離れたイベントの数。初期設定は 1 です。 |
| `ingnoreNulls` | Null `key` 値を無視する必要があるかどうかを示すブール値。デフォルトは `false` です。 |


| 返されるオブジェクトパラメーター | 説明 |
| ---------------------- | ------------- |
| `value` | ADF で使用される `key` に基づく値 |

#### クエリ例

```sql
SELECT endUserIds._experience.aaid.id, timestamp, web.webPageDetails.name,
    NEXT(web.webPageDetails.name, 1, true)
      OVER(PARTITION BY endUserIds._experience.aaid.id
           ORDER BY timestamp
           ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING)
      AS previous_page
FROM experience_events
ORDER BY endUserIds._experience.aaid.id, timestamp ASC
LIMIT 10
```

#### 結果

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

## 間隔

間隔を使用すると、イベント発生前または発生後の期間内の潜在的な顧客行動を調査できます。すべての顧客のイベントや他のタイプのキャンペーンから 7 日以内にイベントを確認します。

### 前の一致までの時間

特定のインシデントから経過した時間を測定する新しいディメンションを提供します。

#### 仕様

構文：`TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventDefintion, [timeUnit]) OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| `timestamp` | すべてのイベントに入力されたデータセット内のタイムスタンプフィールド。 |
| `eventDefintion` | 前のイベントを決定する式。 |
| `timeUnit` | 出力単位：日、時間、分、秒。デフォルトは秒です。 |

出力：前の一致イベントが見つかってからの時間の単位を表す数値を返します。一致するイベントが見つからない場合は null のままになります。

#### クエリ例

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

#### 結果

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

### 次の一致までの時間

特定のディメンションが発生する前の時間を測定する新しいイベントを提供します。

#### 仕様

構文：`TIME_BETWEEN_NEXT_MATCH(timestamp, eventDefintion, [timeUnit]) OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| `timestamp` | すべてのイベントに入力されたデータセット内のタイムスタンプフィールド。 |
| `eventDefintion` | 次のイベントを決定する式。 |
| `timeUnit` | 出力単位：日、時間、分、秒。デフォルトは秒です。 |

出力：次の一致イベントの経過時間の単位を表す負の数値を返します。一致イベントが見つからない場合は null のままになります。

#### クエリ例

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

#### 結果

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

## 次の手順

Using the functions described here, you can write queries to access your own [!DNL ExperienceEvent] datasets using [!DNL Query Service]. For more information about authoring queries in [!DNL Query Service], see the documentation on [creating queries](../creating-queries/creating-queries.md).
