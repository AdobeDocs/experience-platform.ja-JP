---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: アドビ定義関数
topic: functions
translation-type: tm+mt
source-git-commit: 7d5d98d8e32607abf399fdc523d2b3bc99555507

---


# アドビ定義関数

アドビ定義関数(ADF)は、ExperienceEventデータに関する一般的なビジネス関連タスクを実行するのに役立つ、クエリサービスの事前にビルドされた関数です。 これには、Adobe Analyticsのようなセッションおよびアトリビューションの関数が含まれます。 Adobe Analyticsと、このペ [ージで定義されているADFの概念について詳しくは](https://docs.adobe.com/content/help/en/analytics/landing/home.html) 、Adobe Analyticsのドキュメントを参照してください。 このドキュメントでは、アドビが定義した関数に関する情報をクエリサービスで提供します。

## ウィンドウ関数

ビジネスロジックの大部分は、顧客のタッチポイントを収集し、時間別に注文する必要があります。 このサポートは、Spark SQLでウィンドウ関数の形式で提供されます。 ウィンドウ関数は標準SQLの一部で、他の多くのSQLエンジンでサポートされています。

ウィンドウ関数は、集計を更新し、順序付けられたサブセットの各行の1つの項目を返します。 最も基本的な集計関数はです `SUM()`。 `SUM()` 行を取得し、合計1つを提供します。 ウィンドウに適用し `SUM()` 、ウィンドウ関数に変換すると、各行の累積合計が返されます。

Spark SQLヘルパーの大部分は、ウィンドウ内の各行を更新し、その行の状態を追加するウィンドウ関数です。

### 仕様

構文：`OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| [分割] | 列または使用可能なフィールドに基づく行のサブグループ。 例, `PARTITION BY endUserIds._experience.mcid.id` |
| [order] | サブセットまたは行の順序を指定する列または使用可能なフィールド。 例, `ORDER BY timestamp` |
| [frame] | パーティション内の行のサブグループ。 例, `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` |

## セッション化

Webサイト、モバイルアプリケーション、インタラクティブな音声応答チャネル、またはその他の顧客のインタラクションシステムから生成されるExperienceEventデータを扱う場合、関連するアクティビティ期間中にイベントをグループ化できる場合に役立ちます。 通常、アクティビティの調査、請求書の支払い、口座残高の確認、申込書の記入など、特定の意図がを駆り立てることができます。 このグループ化は、顧客エクスペリエンスに関するイベントを関連付け、顧客エクスペリエンスに関するより詳細なコンテキストを明らかにするのに役立ちます。

Adobe Analyticsでのセッション化について詳しくは、コンテキスト対応セッションに関するドキュメ [ントを参照してください](https://docs.adobe.com/content/help/en/analytics/components/virtual-report-suites/vrs-mobile-visit-processing.html)。

### 仕様

構文：`SESS_TIMEOUT(timestamp, expirationInSeconds) OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| `timestamp` | データセット内のタイムスタンプフィールド |
| `expirationInSeconds` | 現在のセッションの終了と新しいイベントの開始を決定するために必要な秒数 |

| 返されるオブジェクトパラメータ | 説明 |
| ---------------------- | ------------- |
| `timestamp_diff` | 現在のレコードと前のレコードの間の時間（秒） |
| `num` | window関数ので定義されたキーの1から始まる一意のセッ `PARTITION BY` ション番号です。 |
| `is_new` | レコードがセッションの最初であるかどうかを識別するために使用されるブール値です。 |
| `depth` | セッション内の現在のレコードの深さ |

#### サンプルクエリ

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

```
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

顧客の行動を成功に関連付けることは、顧客体験に影響を与える要因を理解する上で重要な役割を果たします。 次のADFは、異なる有効期限設定でFirstとLastのアトリビューションをサポートしています。

Adobe Analyticsのアトリビューションについて詳しくは、Analytics分析ガイドの [アトリビューションIQの概要](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/panels/attribution.html) （英語のみ）を参照してください。

### ファーストタッチアトリビューション

ターゲットExperienceEventデータセット内の1つのイベントに対するファーストタッチアトリビューション値と詳細チャネルを返します。 クエリは、選択し `struct` たチャネルに対して返される各行のファーストタッチ値、タイムスタンプおよび属性を持つオブジェクトを返します。

このクエリは、一連の顧客アクションに導いたインタラクションを確認する場合に役立ちます。 以下の例では、ExperienceEventデータの最初の追跡コード(`em:946426`)は、最初のインタラクションであったので、顧客のアクションに対する100%(`1.0`)の責任と見なされます。

### 仕様

構文：`ATTRIBUTION_FIRST_TOUCH(timestamp, channelName, channelValue) OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| `timestamp` | データセット内のタイムスタンプフィールド |
| `channelName` | 返されるオブジェクトのラベルとして使用するわかりやすい名前 |
| `channelValue` | クエリのターゲットチャネルである列またはフィールド |


| 返されるオブジェクトパラメータ | 説明 |
| ---------------------- | ------------- |
| `name` | ADFにラ `channelName` ベルとして入力されたもの |
| `value` | ExperienceEventの最初 `channelValue` のタッチから得た値 |
| `timestamp` | ファーストタッチが発生したExperienceEventのタイムスタンプ |
| `fraction` | ファーストタッチのアトリビューションは、小数部のクレジットとして表されます |

#### サンプルクエリ

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

```
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

### ラストタッチアトリビューション

ターゲットExperienceEventデータセット内の1つのイベントに対するラストタッチアトリビューション値と詳細チャネルを返します。 クエリは、選択し `struct` たチャネルに対して返される各行のラストタッチ値、タイムスタンプおよび属性を持つオブジェクトを返します。

このクエリは、一連の顧客アクションの最終的なインタラクションを確認する場合に役立ちます。 以下の例では、返されるオブジェクトのトラッキングコードが、各ExperienceEventレコードの最後のインタラクションです。 各コードは、最後のインタラクションであったので、顧客`1.0`のアクションに対する100 %()の責任を負います。

### 仕様

構文：`ATTRIBUTION_LAST_TOUCH(timestamp, channelName, channelValue) OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| `timestamp` | データセット内のタイムスタンプフィールド |
| `channelName` | 返されるオブジェクトのラベルとして使用するわかりやすい名前 |
| `channelValue` | クエリのターゲットチャネルである列またはフィールド |


| 返されるオブジェクトパラメータ | 説明 |
| ---------------------- | ------------- |
| `name` | ADFにラ `channelName` ベルとして入力されたもの |
| `value` | ExperienceEventの最後 `channelValue` のタッチから得た値 |
| `timestamp` | 使用されたExperienceEventのタイムスタ `channelValue` ンプ。 |
| `fraction` | ラストタッチのアトリビューションは、小数部のクレジットとして表されます |

#### サンプルクエリ

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

```
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

### 有効期限条件を持つファーストタッチアトリビューション

ターゲットのExperienceEventデータセット内の1つのチャネルに対するファーストタッチアトリビューション値と詳細を返します。この値は、条件の前後に期限切れになります。 クエリは、選択し `struct` たチャネルに対して返される各行のファーストタッチ値、タイムスタンプおよび属性を持つオブジェクトを返します。

このクエリは、選択の条件によって決定されるExperienceEventデータセットの一部内で、どのようなインタラクションが一連の顧客アクションを引き起こしたかを確認する場合に役立ちます。 次の例では、結果に表示される4日（7月15日、21日、23日および29日）ごとに購入が記録され(`commerce.purchases.value IS NOT NULL`)、各日の初期追跡コードは、顧客のアクションに対する100%(`1.0`)の責任と見なされます。

#### 仕様

構文：`ATTRIBUTION_FIRST_TOUCH_EXP_IF(timestamp, channelName, channelValue, expCondition, expBefore) OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| `timestamp` | データセット内のタイムスタンプフィールド |
| `channelName` | 返されるオブジェクトのラベルとして使用するわかりやすい名前 |
| `channelValue` | クエリのターゲットチャネルである列またはフィールド |
| `expCondition` | チャネル |
| `expBefore` | デフォルト値は `false` です。指定した条件が満たされる前または後にチャネルの有効期限が切れるかどうかを示すブール値です。 主に、セッションの有効期限条件(例えば `sess.depth = 1, true`)に対して有効になり、前のセッションからファーストタッチが選択されないようにします。 |

| 返されるオブジェクトパラメータ | 説明 |
| ---------------------- | ------------- |
| `name` | ADFにラ `channelName` ベルとして入力されたもの |
| `value` | その値 `channelValue` から、 `expCondition` |
| `timestamp` | ファーストタッチが発生したExperienceEventのタイムスタンプ |
| `fraction` | ファーストタッチのアトリビューションは、小数部のクレジットとして表されます |

#### サンプルクエリ

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

```
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

### 有効期限が切れたファーストタッチアトリビューション

指定された期間のターゲットExperienceEventデータセット内の単一のチャネルに対するファーストタッチアトリビューション値と詳細を返します。 クエリは、選択し `struct` たチャネルに対して返される各行のファーストタッチ値、タイムスタンプおよび属性を持つオブジェクトを返します。 このクエリは、選択した時間間隔内で顧客の行動につながったインタラクションを確認する場合に役立ちます。 次の例では、各顧客アクションに対して返されるファーストタッチが、過去7日間で最も早いインタラクションで`expTimeout = 86400 * 7`す()。

#### 仕様

構文：`ATTRIBUTION_FIRST_TOUCH_EXP_TIMEOUT(timestamp, channelName, channelValue, expTimeout) OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| `timestamp` | データセット内のタイムスタンプフィールド |
| `channelName` | 返されるオブジェクトのラベルとして使用するわかりやすい名前 |
| `channelValue` | クエリのターゲットチャネルである列またはフィールド |
| `expTimeout` | クエリがファーストタッチイベントを検索するチャネルイベントの前の時間（秒） |

| 返されるオブジェクトパラメータ | 説明 |
| ---------------------- | ------------- |
| `name` | ADFにラ `channelName` ベルとして入力されたもの |
| `value` | 指定した `channelValue` 間隔内の最初のタッチの値 `expTimeout` 。 |
| `timestamp` | ファーストタッチが発生したExperienceEventのタイムスタンプ |
| `fraction` | ファーストタッチのアトリビューションは、小数部のクレジットとして表されます |

#### サンプルクエリ

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

```
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

### 有効期限条件付きのラストタッチアトリビューション

ターゲットのExperienceEventデータセット内の1つのチャネルの、条件の前後で期限切れになるラストタッチアトリビューション値と詳細を返します。 クエリは、選択し `struct` たチャネルに対して返される各行のラストタッチ値、タイムスタンプおよび属性を持つオブジェクトを返します。 このクエリは、選択の条件によって決定されるExperienceEventデータセットの一部内の一連の顧客アクションの中で、最後のインタラクションを確認する場合に役立ちます。 次の例では、結果に表示される4日（7月15日、21日、23日および29日）ごとに購入が記録され(`commerce.purchases.value IS NOT NULL`)、各日の最後の追跡コードは、顧客のアクションに対する100%(`1.0`)の責任と見なされます。

#### 仕様

構文：`ATTRIBUTION_LAST_TOUCH_EXP_IF(timestamp, channelName, channelValue, expCondition, expBefore) OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| `timestamp` | データセット内のタイムスタンプフィールド |
| `channelName` | 返されるオブジェクトのラベルとして使用するわかりやすい名前 |
| `channelValue` | クエリのターゲットチャネルである列またはフィールド |
| `expCondition` | チャネル |
| `expBefore` | デフォルト値は `false` です。指定した条件が満たされる前または後にチャネルの有効期限が切れるかどうかを示すブール値です。 主にセッションの有効期限切れ条件(例えば、 `sess.depth = 1, true`)で有効になり、前のセッションからラストタッチが選択されないようにします。 |

| 返されるオブジェクトパラメータ | 説明 |
| ---------------------- | ------------- |
| `name` | ADFにラ `channelName` ベルとして入力されたもの |
| `value` | その値 `channelValue` から、 `expCondition` |
| `timestamp` | ラストタッチが発生したExperienceEventのタイムスタンプ |
| `percentage` | ラストタッチのアトリビューションは、小数部のクレジットとして表されます |

#### サンプルクエリ

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

```
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

### 有効期限が切れたラストタッチアトリビューション

指定された期間の、ターゲットExperienceEventデータセット内の1つのチャネルに対するラストタッチアトリビューション値と詳細を返します。 クエリは、選択し `struct` たチャネルに対して返される各行のラストタッチ値、タイムスタンプおよび属性を持つオブジェクトを返します。 このクエリは、選択した時間間隔内の最後のインタラクションを表示する場合に便利です。 次の例では、各顧客アクションに対して返されるラストタッチは、7日以内の最終的なインタラクションで`expTimeout = 86400 * 7`す。

#### 仕様

構文：`ATTRIBUTION_LAST_TOUCH_EXP_TIMEOUT(timestamp, channelName, channelValue, expTimeout) OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| `timestamp` | データセット内のタイムスタンプフィールド |
| `channelName` | 返されるオブジェクトのラベルとして使用するわかりやすい名前 |
| `channelValue` | クエリのターゲットチャネルである列またはフィールド |
| `expTimeout` | クエリがラストタッチイベントを検索するチャネルイベント後の時間（秒） |

| 返されるオブジェクトパラメータ | 説明 |
| ---------------------- | ------------- |
| `name` | ADFにラ `channelName` ベルとして入力されたもの |
| `value` | 指定した間 `channelValue` 隔内の最後のタッチからの値 `expTimeout` 。 |
| `timestamp` | ラストタッチが発生したExperienceEventのタイムスタンプ |
| `percentage` | ラストタッチのアトリビューションは、小数部のクレジットとして表されます |

#### サンプルクエリ

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

```
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

## 前へ/次へのタッチ

エクスペリエンス内で顧客がどのようにナビゲートしているかを理解することが重要です。 顧客の関与の深さを把握し、エクスペリエンスの意図した手順が設計どおりに機能していることを確認し、顧客に影響を与える潜在的な問題点を特定するために使用できます。 以下のADFは、PreviousとNextの関係からのパス表示の確立をサポートしています。 前のページと次のページを作成したり、複数のイベントを順を追ってパスを作成できます。

### 前回のタッチ

特定のフィールドの前の値を、ウィンドウ内で一定のステップ数だけ離れた位置から判断します。 この例では、現在の行 `WINDOW` とその前のすべてを見るよ `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` うにADFを設定するフレームでFunctionが設定されています。

#### 仕様

構文：`PREVIOUS(key, [shift, [ignoreNulls]]) OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| `key` | イベントの列またはフィールド。 |
| `shift` | （オプション）現在のイベントから離れたイベントの数。 初期設定は 1 です。 |
| `ingnoreNulls` | null値を無視する必要がある `key` 場合に示すブール値。 デフォルトは `false` です。 |


| 返されるオブジェクトパラメータ | 説明 |
| ---------------------- | ------------- |
| `value` | ADFで使用される値 `key` に基づく値 |

#### サンプルクエリ

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

```
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

特定のフィールドの次の値を、ウィンドウ内で一定のステップ数だけ離れた位置から判断します。 この例では、現在の行 `WINDOW` とその後すべてを見るよ `ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING` うにADFを設定するフレームでFunctionが設定されています。

#### 仕様

構文：`NEXT(key, [shift, [ignoreNulls]]) OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| `key` | イベント |
| `shift` | （オプション）現在のイベントから離れたイベントの数。 初期設定は 1 です。 |
| `ingnoreNulls` | null値を無視する必要がある `key` 場合に示すブール値。 デフォルトは `false` です。 |


| 返されるオブジェクトパラメータ | 説明 |
| ---------------------- | ------------- |
| `value` | ADFで使用される値 `key` に基づく値 |

#### サンプルクエリ

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

```
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

間隔：イベントの発生前または発生後に潜在的な顧客行動を調査できます。 すべての顧客のイベントや他のタイプのキャンペーンから7日以内にイベントを確認します。

### 前回の一致までの時間

特定のインシデントからの経過時間を測定する新しいディメンションを提供します。

#### 仕様

構文：`TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventDefintion, [timeUnit]) OVER ([partition] [order] [frame])`

| パラメーター | 説明 |
| --- | --- |
| `timestamp` | すべてのタイムスタンプに設定されたデータセット内のイベントフィールド。 |
| `eventDefintion` | 式を選択します。 |
| `timeUnit` | 出力単位：日、時間、分、秒。 初期設定は秒です。 |

出力：前回の一致イベントが見つかってからの時間の単位を表す数値を返します。一致するイベントが見つからない場合はnullのままになります。

#### サンプルクエリ

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

```
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
| `timestamp` | すべてのタイムスタンプに設定されたデータセット内のイベントフィールド。 |
| `eventDefintion` | 式。 |
| `timeUnit` | 出力単位：日、時間、分、秒。 初期設定は秒です。 |

出力：次の一致イベントの経過時間の単位を表す負の数値を返します。一致イベントが見つからない場合はnullのままになります。

#### サンプルクエリ

```
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

```
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

ここで説明する関数を使用して、クエリサービスを使用して独自のExperienceEventデータセットにアクセスするクエリを作成できます。 クエリサービスでのオーサリングクエリについて詳しくは、クエリの作成に関するドキュメントを参 [照してくださ](../creating-queries/creating-queries.md)い。
