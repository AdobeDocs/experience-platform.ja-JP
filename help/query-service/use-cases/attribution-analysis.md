---
title: アトリビューション分析
description: このドキュメントでは、クエリサービスを使用して、ファーストタッチとラストタッチのマーケティングアトリビューションモデルに基づくマーケティング効果測定手法を作成する方法について説明します。
exl-id: d62cd349-06fc-4ce6-a5e8-978f11186927
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '1418'
ht-degree: 10%

---

# 属性分析

アトリビューションは、ビジネスの販売やコンバージョンに貢献するチャネル、オファー、メッセージなどのマーケティング戦術を決定するのに役立つ分析概念です。 この概念では、（消費者がブランドとやり取りする際に）顧客のタッチポイントに基づいて購入または獲得につながる消費者ジャーニー（顧客が目標を達成するために会社とやり取りするプロセス）を評価します。 アトリビューション分析を通じて、マーケターは、顧客を見込み客と結び付けるチャネルの投資回収率を評価できます。

## はじめに

このドキュメント全体での SQL の例は、Adobe Analytics データで一般的に使用されるクエリです。 このチュートリアルでは、次のコンポーネントに関する十分な知識が必要です。

* [ レポートスイートデータ概要用のAdobe Analytics ソースコネクタ ](../../sources/connectors/adobe-applications/mapping/analytics.md)。
* [Analytics フィールドマッピングのドキュメント ](../../sources/connectors/adobe-applications/mapping/analytics.md) では、クエリサービスで使用する分析データの取り込みとマッピングについて詳しく説明しています。
* [Attribution IQの概要 ](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/attribution/overview.html)
* [Adobe Analytics アトリビューションパネルガイド ](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/panels/attribution.html)。

`OVER()` 関数内のパラメーターについて詳しくは、[window 関数 ](../sql/adobe-defined-functions.md#window-functions) を参照してください。 [Adobe マーケティングおよびCommerce用語の用語集 ](https://business.adobe.com/glossary/index.html) も使用される場合があります。

次の各ユースケースについて、パラメーター化された SQL クエリの例をテンプレートとして提供し、カスタマイズします。 評価する SQL 例の `{ }` に表示されるパラメータを指定します。

## 目標

アトリビューションのユースケースでは、Adobe Analytics データを使用して、顧客のアクションを成功に関連付けます。 この関連付けは、顧客体験に影響を与える要因を理解するうえで重要な部分です。 アトリビューション分析データを使用すると、カスタマージャーニー中の顧客のタッチポイントの重要性を把握できます。

このドキュメントに含まれるクエリの例では、異なる有効期限設定を使用したファーストタッチとラストタッチのアトリビューションの様々なユースケースをサポートしています。 このガイドでは、次の主要な概念について説明します。

* ファーストタッチとラストタッチのアトリビューション。
* 有効期限タイムアウトを使用したファーストタッチとラストタッチのアトリビューション。
* ファーストタッチとラストタッチ （有効期限条件）のアトリビューション。

## 属性クエリパラメーター {#attribution-query-parameters}

次の表に、ファーストタッチアトリビューションクエリとラストタッチアトリビューションクエリで使用されるパラメーターの分類とその説明を示します。

| パラメーター | 説明 |
|---|---|
| `{TIMESTAMP}` | データセットで見つかったタイムスタンプ フィールド。 |
| `{CHANNEL_NAME}` | 返されたオブジェクトのラベル。 |
| `{CHANNEL_VALUE}` | クエリのターゲットチャネルである列またはフィールド。 |
| `{EXP_TIMEOUT}` | クエリがファーストタッチイベントを検索する、チャネルイベントより前の時間（秒単位）。 |
| `{EXP_CONDITION}` | チャネルの有効期限を決定する条件。 |
| `{EXP_BEFORE}` | 指定された条件 `{EXP_CONDITION}` が満たされた前後にチャネルの有効期限が切れるかどうかを示すブール値。 これは主に、セッションの有効期限の条件で有効になり、ファーストタッチが以前のセッションから選択されないようにするためのものです。 デフォルトでは、この値は `false` に設定されています。 |

## クエリ結果列コンポーネント {#query-result-column-components}

アトリビューションクエリの結果は、`first_touch` 列または `last_touch` 列のいずれかで示されます。 これらの列は、次のコンポーネントで構成されています。

```console
({NAME}, {VALUE}, {TIMESTAMP}, {FRACTION})
```

| パラメーター | 説明 |
| ---------- | ----------- |
| `{NAME}` | `{CHANNEL_NAME}`。Azure Data Factory （ADF）でラベルとして入力されます。 |
| `{VALUE}` | 指定した `{EXP_TIMEOUT}` 間隔内のラストタッチである `{CHANNEL_VALUE}` から得た値 |
| `{TIMESTAMP}` | ラストタッチが発生した [!DNL Experience Event] のタイムスタンプ |
| `{FRACTION}` | ラストタッチのアトリビューション （小数点以下の桁数で表します）。 |

### ファーストタッチ属性 {#first-touch}

ファーストタッチアトリビューションでは、消費者が遭遇した最初のチャネルに成功した結果に対する責任の 100% を認めます。 この SQL の例は、後続の一連の顧客アクションを引き起こしたインタラクションをハイライト表示するために使用されます。

以下のクエリは、ターゲット [!DNL Experience Event] データセットのチャネルのファーストタッチアトリビューション値と詳細を返します。 また、選択したチャネルの `struct` オブジェクトと、各行のファーストタッチ値、タイムスタンプ、アトリビューションも返します。

>[!NOTE]
>
>Experience Cloud ID （ECID）は、MCID とも呼ばれ、名前空間で引き続き使用されます。

**クエリ構文**

```sql
ATTRIBUTION_FIRST_TOUCH({TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}) OVER ({PARTITION} {ORDER} {FRAME})
```

必要になる可能性のあるパラメーターの完全なリストとその説明については、[ アトリビューションクエリパラメーター ](#attribution-query-parameters) の節を参照してください。

**クエリの例**

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

以下の結果では、初期トラッキングコード `em:946426` が [!DNL Experience Event] データセットから取得されています。 このトラッキングコードは、最初のインタラクションであったので、顧客アクションの責任の 100% （`1.0`）に関連付けられます。

```console
                 id                 |       timestamp       | trackingCode |                   first_touch                   
|-----------------------------------+-----------------------+--------------+-------------------------------------------------
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

`first_touch` 列に表示される結果の分類については、[ 列コンポーネント ](#query-result-column-components) を参照してください。

### ラストタッチ属性 {#second-touch}

ラストタッチアトリビューションでは、成功した結果に対する責任の 100% を、消費者が最後に遭遇したチャネルにクレジットします。 次の SQL の例は、一連のお客様のアクションにおける最終的なインタラクションをハイライト表示するために使用されます。

クエリは、ターゲット [!DNL Experience Event] データセットのラストタッチアトリビューション値とチャネルの詳細を返します。 また、選択したチャネルの `struct` オブジェクトと、各行のラストタッチ値、タイムスタンプ、アトリビューションも返します。

**クエリ構文**

```sql
ATTRIBUTION_LAST_TOUCH({TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}) OVER ({PARTITION} {ORDER} {FRAME})
```

**クエリの例**

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

以下に示す結果では、返されたオブジェクトのトラッキングコードが、各 [!DNL Experience Event] レコードの最後のインタラクションになります。 各コードは、最後のインタラクションであったので、顧客のアクションに対して 100% （`1.0`）の責任を負います。

```console
                 id                |       timestamp       | trackingCode |                   last_touch                   
|-----------------------------------+-----------------------+--------------+-------------------------------------------------
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

`last_touch` 列に表示される結果の分類については、[ 列コンポーネント ](#query-result-column-components) を参照してください。

### 有効期限条件を持つファーストタッチ属性 {#first-touch-attribution-with-expiration-condition}

このクエリを使用すると、選択した条件によって決定される、[!DNL Experience Event] データセットの一部において、一連の顧客アクションを引き起こしたインタラクションを確認できます。

クエリは、条件の後または前に有効期限が切れる、ターゲット [!DNL Experience Event] データセット内の単一チャネルのファーストタッチアトリビューション値と詳細を返します。 また、選択したチャネルについて返された各行のファーストタッチ値、タイムスタンプ、アトリビューションを含む `struct` オブジェクトも返されます。

**クエリ構文**

```sql
ATTRIBUTION_FIRST_TOUCH_EXP_IF(
    {TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}, {EXP_CONDITION}, {EXP_BEFORE})
    OVER ({PARTITION} {ORDER} {FRAME})
```

必要になる可能性のあるパラメーターの完全なリストとその説明については、[ アトリビューションクエリパラメーター ](#attribution-query-parameters) の節を参照してください。

**クエリの例**

次の例では、結果に示された 4 日間（7 月 15 日、21 日、23 日および 29 日）の購入が記録され（`commerce.purchases.value IS NOT NULL`）、各日の初期トラッキングコードは、顧客アクションの属性 100% （`1.0`）の責任になります。

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
                 id               |       timestamp       | trackingCode |                   first_touch                   
|----------------------------------+-----------------------+--------------+-------------------------------------------------
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

`first_touch` 列に表示される結果の分類については、[ 列コンポーネント ](#query-result-column-components) を参照してください。

### 有効期限タイムアウトを持つファーストタッチ属性 {#first-touch-attribution-with-expiration-timeout}

このクエリは、選択した期間内に、顧客アクションを成功に導いたインタラクションを見つけるために使用されます。

以下のクエリは、指定された期間のターゲット [!DNL Experience Event] データセット内の単一チャネルのファーストタッチアトリビューション値と詳細を返します。 クエリは、選択したチャネルに対して返される各行のファーストタッチ値、タイムスタンプおよび属性を持つ `struct` オブジェクトを返します。

**クエリ構文**

```sql
ATTRIBUTION_FIRST_TOUCH_EXP_IF(
    {TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}, {EXP_CONDITION}, {EXP_BEFORE})
    OVER ({PARTITION} {ORDER} {FRAME})
```

必要になる可能性のあるパラメーターの完全なリストとその説明については、[ アトリビューションクエリパラメーター ](#attribution-query-parameters) の節を参照してください。

**クエリの例**

次に示す例では、各顧客アクションに対して返されるファーストタッチは、過去 7 日以内の最も古いインタラクションです（expTimeout = 86400 * 7）。

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
|-----------------------------------+-----------------------+--------------+-------------------------------------------------
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

`first_touch` 列に表示される結果の分類については、[ 列コンポーネント ](#query-result-column-components) を参照してください。

### 有効期限条件を持つラストタッチ属性 {#last-touch-attribution-with-expiration-condition}

このクエリは、選択した条件によって決定される [!DNL Experience Event] データセットの部分内で、一連の顧客アクションの最後のインタラクションを見つけるために使用されます。

以下のクエリは、条件の後または前に有効期限が切れる、ターゲット [!DNL Experience Event] データセット内の単一チャネルのラストタッチアトリビューション値と詳細を返します。 クエリは、選択したチャネルに対して返される各行のラストタッチ値、タイムスタンプおよび属性を持つ `struct` オブジェクトを返します。

**クエリ構文**

```sql
ATTRIBUTION_LAST_TOUCH_EXP_IF(
    {TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}, {EXP_CONDITION}, {EXP_BEFORE}) 
    OVER ({PARTITION} {ORDER} {FRAME})
```

必要になる可能性のあるパラメーターの完全なリストとその説明については、[ アトリビューションクエリパラメーター ](#attribution-query-parameters) の節を参照してください。

**クエリの例**

次の例では、結果に示された 4 日間（7 月 15 日、21 日、23 日および 29 日）の購入が記録され（`commerce.purchases.value IS NOT NULL`）、各日の最後のトラッキングコードは、顧客アクションの属性 100% （`1.0`）の責任になります。

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
                id                 |       timestamp       | trackingCode |                   last_touch                   
|-----------------------------------+-----------------------+--------------+------------------------------------------------
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

`last_touch` 列に表示される結果の分類については、[ 列コンポーネント ](#query-result-column-components) を参照してください。

### 有効期限タイムアウトを持つラストタッチ属性 {#last-touch-attribution-with-expiration-timeout}

このクエリは、選択した期間内の最後のインタラクションを検索するために使用されます。 クエリは、指定された期間のターゲット [!DNL Experience Event] ータデータセット内の単一チャネルのラストタッチアトリビューション値と詳細を返します。 クエリは、選択したチャネルに対して返される各行のラストタッチ値、タイムスタンプおよび属性を持つ `struct` オブジェクトを返します。

**クエリ構文**

```sql
ATTRIBUTION_LAST_TOUCH_EXP_TIMEOUT(
    {TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}, {EXP_TIMEOUT}) 
    OVER ({PARTITION} {ORDER} {FRAME})
```

必要になる可能性のあるパラメーターの完全なリストとその説明については、[ アトリビューションクエリパラメーター ](#attribution-query-parameters) の節を参照してください。

**クエリの例**

次の例では、各顧客アクションに対して返されるラストタッチが、次の 7 日間での最後のインタラクションです（`expTimeout = 86400 * 7`）。

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
|-----------------------------------+-----------------------+--------------+-------------------------------------------------
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

`last_touch` 列に表示される結果の分類については、[ 列コンポーネント ](#query-result-column-components) を参照してください。
