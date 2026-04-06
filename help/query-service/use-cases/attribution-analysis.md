---
title: アトリビューション分析
description: このドキュメントでは、Query Serviceを使用して、ファーストタッチとラストタッチのマーケティングアトリビューションモデルに基づくマーケティング効果測定手法を作成する方法について説明します。
exl-id: d62cd349-06fc-4ce6-a5e8-978f11186927
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '1418'
ht-degree: 10%

---

# 属性分析

アトリビューションとは、ビジネスの売上やコンバージョンに貢献するチャネル、オファー、メッセージなどのマーケティング戦術を決定するのに役立つ分析コンセプトです。 この概念は、カスタマージャーニー（顧客が企業の目標を達成するために企業とインタラクションするプロセス）を評価し、その結果として顧客接点（消費者が企業とインタラクションするときはいつでも）にもとづいて購入または獲得をおこないます。 マーケターは、アトリビューション分析を通じて、潜在顧客につながるチャネルの投資収益率を評価できます。

## はじめに

このドキュメント全体のSQLの例は、Adobe Analytics データで一般的に使用されるクエリです。 このチュートリアルでは、次のコンポーネントについて理解する必要があります。

* [&#x200B; レポートスイートデータの概要](../../sources/connectors/adobe-applications/mapping/analytics.md)用のAdobe Analytics ソースコネクタ。
* [Analytics フィールドマッピングに関するドキュメント &#x200B;](../../sources/connectors/adobe-applications/mapping/analytics.md)では、クエリサービスで使用するAnalytics データの取り込みとマッピングに関する詳細情報を提供しています。
* [Attribution IQの概要](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/attribution/overview.html)
* [Adobe Analytics アトリビューションパネルガイド &#x200B;](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/panels/attribution.html?lang=ja)。

`OVER()`関数内のパラメーターの説明については、[&#x200B; ウィンドウ関数セクション &#x200B;](../sql/adobe-defined-functions.md#window-functions)を参照してください。 [Adobe マーケティングおよびCommerce用語の用語集](https://business.adobe.com/glossary/index.html)も使用できます。

次の各ユースケースでは、カスタマイズ用のテンプレートとして、パラメーター化されたSQL クエリの例が提供されます。 評価に関心のあるSQLの例の`{ }`が表示される場所にパラメーターを指定します。

## 目標

アトリビューションのユースケースでは、Adobe Analyticsのデータを利用して、顧客の行動を成功した結果に関連付けることができます。 顧客体験に影響を与える要因を把握するうえで、この関連性は不可欠な要素です。 アトリビューション分析データは、カスタマージャーニーにおける顧客接点の重要性を把握するために活用できます。

このドキュメントに含まれるクエリの例では、有効期限の設定が異なるファーストタッチアトリビューションとラストタッチアトリビューションの様々なユースケースをサポートしています。 このガイドでは、次の主要な概念について説明します。

* ファーストタッチとラストタッチのアトリビューション。
* 有効期限タイムアウト付きのファーストタッチとラストタッチアトリビューション。
* 有効期限が設定されたファーストタッチとラストタッチアトリビューション。

## アトリビューションクエリパラメーター {#attribution-query-parameters}

次の表に、ファーストタッチアトリビューションクエリとラストタッチアトリビューションクエリで使用されるパラメーターとその説明を示します。

| パラメーター | 説明 |
|---|---|
| `{TIMESTAMP}` | データセット内で見つかったタイムスタンプフィールド。 |
| `{CHANNEL_NAME}` | 返されるオブジェクトのラベル。 |
| `{CHANNEL_VALUE}` | クエリのターゲットチャネルである列またはフィールド。 |
| `{EXP_TIMEOUT}` | チャネルイベントの前の時間のウィンドウ。クエリがファーストタッチイベントを検索する秒単位。 |
| `{EXP_CONDITION}` | チャネルの有効期限を決定する条件。 |
| `{EXP_BEFORE}` | 指定された条件`{EXP_CONDITION}`が満たされる前または後にチャネルが期限切れになるかどうかを示すブール値。 これは主に、セッションの有効期限の条件に対して有効になり、前のセッションからファーストタッチが選択されないようにします。 デフォルトでは、この値は `false` に設定されています。 |

## クエリ結果列コンポーネント {#query-result-column-components}

アトリビューションクエリの結果は、`first_touch`列または`last_touch`列のどちらかに表示されます。 これらの列は、次のコンポーネントで構成されます。

```console
({NAME}, {VALUE}, {TIMESTAMP}, {FRACTION})
```

| パラメーター | 説明 |
| ---------- | ----------- |
| `{NAME}` | Azure Data Factory （ADF）にラベルとして入力された`{CHANNEL_NAME}`。 |
| `{VALUE}` | 指定した `{EXP_TIMEOUT}` 間隔内のラストタッチである `{CHANNEL_VALUE}` から得た値 |
| `{TIMESTAMP}` | 最後のタッチが発生した[!DNL Experience Event]のタイムスタンプ |
| `{FRACTION}` | ラストタッチのアトリビューション。小数点以下桁で表します。 |

### ファーストタッチ属性 {#first-touch}

ファーストタッチアトリビューションでは、最初のチャネルで消費者が遭遇した結果が成功した場合に、その責任の100%を認定します。 このSQLの例は、その後の一連の顧客アクションにつながったインタラクションを強調するために使用されます。

以下のクエリは、ターゲット [!DNL Experience Event] データセット内の最初のタッチアトリビューション値とチャネルの詳細を返します。 また、選択したチャネルの`struct` オブジェクトを、各行の最初のタッチ値、タイムスタンプ、アトリビューションとともに返します。

>[!NOTE]
>
>Experience Cloud ID （ECID）はMCIDとも呼ばれ、ネームスペースで引き続き使用されます。

**クエリ構文**

```sql
ATTRIBUTION_FIRST_TOUCH({TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}) OVER ({PARTITION} {ORDER} {FRAME})
```

必要になる可能性のあるパラメーターとその説明の完全なリストについては、[&#x200B; アトリビューションクエリパラメーターの節](#attribution-query-parameters)を参照してください。

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

以下の結果では、最初のトラッキングコード `em:946426`が[!DNL Experience Event] データセットから取得されます。 このトラッキングコードは、最初のインタラクションであったため、顧客のアクションに対する責任の100% （`1.0`）に起因しています。

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

`first_touch`列に表示される結果の内訳については、「[列コンポーネント」セクション &#x200B;](#query-result-column-components)を参照してください。

### ラストタッチ属性 {#second-touch}

ラストタッチアトリビューションでは、成功した結果に対する責任の100%を、消費者が最後に遭遇したチャネルに割り当てます。 このSQLの例は、一連の顧客アクションの最終的なインタラクションを強調するために使用されます。

クエリは、ターゲット [!DNL Experience Event] データセット内のラストタッチアトリビューション値とチャネルの詳細を返します。 また、選択したチャネルの`struct` オブジェクトを、各行のラストタッチ値、タイムスタンプ、アトリビューションと共に返します。

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

以下に表示される結果では、返されたオブジェクト内のトラッキングコードは、各[!DNL Experience Event] レコード内の最後のインタラクションです。 各コードは、最後のインタラクションであったため、顧客のアクションに対して100% （`1.0`）の責任があると考えられています。

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

`last_touch`列に表示される結果の内訳については、「[列コンポーネント」セクション &#x200B;](#query-result-column-components)を参照してください。

### 有効期限条件を持つファーストタッチ属性 {#first-touch-attribution-with-expiration-condition}

このクエリは、選択した条件によって決定される[!DNL Experience Event] データセットの一部で、どのインタラクションが一連の顧客アクションにつながったかを確認するために使用されます。

クエリは、ターゲット [!DNL Experience Event] データセット内の1つのチャネルのファーストタッチアトリビューション値と詳細を返します。条件の後または前に有効期限が切れます。 また、選択したチャネルに対して返された各行の最初のタッチ値、タイムスタンプ、アトリビューションを含む`struct` オブジェクトも返されます。

**クエリ構文**

```sql
ATTRIBUTION_FIRST_TOUCH_EXP_IF(
    {TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}, {EXP_CONDITION}, {EXP_BEFORE})
    OVER ({PARTITION} {ORDER} {FRAME})
```

必要になる可能性のあるパラメーターとその説明の完全なリストについては、[&#x200B; アトリビューションクエリパラメーターの節](#attribution-query-parameters)を参照してください。

**クエリの例**

次の例では、結果に表示されている4日間（7月15日、21日、23日、29日）ごとに購入が記録され（`commerce.purchases.value IS NOT NULL`）、各日の最初のトラッキングコードは、顧客のアクションに対して100% （`1.0`）の責任を負っています。

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

`first_touch`列に表示される結果の内訳については、「[列コンポーネント」セクション &#x200B;](#query-result-column-components)を参照してください。

### 有効期限タイムアウトを持つファーストタッチ属性 {#first-touch-attribution-with-expiration-timeout}

このクエリは、選択した期間内に、顧客のアクションが成功したインタラクションを検索するために使用されます。

以下のクエリは、指定された期間のターゲット [!DNL Experience Event] データセット内の単一チャネルのファーストタッチアトリビューション値と詳細を返します。 クエリは、選択したチャネルに対して返される各行のファーストタッチ値、タイムスタンプおよび属性を持つ `struct` オブジェクトを返します。

**クエリ構文**

```sql
ATTRIBUTION_FIRST_TOUCH_EXP_IF(
    {TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}, {EXP_CONDITION}, {EXP_BEFORE})
    OVER ({PARTITION} {ORDER} {FRAME})
```

必要になる可能性のあるパラメーターとその説明の完全なリストについては、[&#x200B; アトリビューションクエリパラメーターの節](#attribution-query-parameters)を参照してください。

**クエリの例**

次の例では、各顧客アクションに対して返されるファーストタッチは、過去7日以内の最も早いインタラクションです（expTimeout = 86400 * 7）。

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

`first_touch`列に表示される結果の内訳については、「[列コンポーネント」セクション &#x200B;](#query-result-column-components)を参照してください。

### 有効期限条件を持つラストタッチ属性 {#last-touch-attribution-with-expiration-condition}

このクエリは、選択した条件によって決定される[!DNL Experience Event] データセットの一部で、一連の顧客アクションの最後のインタラクションを見つけるために使用されます。

以下のクエリは、ターゲット [!DNL Experience Event] データセット内の単一チャネルのラストタッチアトリビューション値と詳細を返します。条件の後または前に有効期限が切れます。 クエリは、選択したチャネルに対して返される各行のラストタッチ値、タイムスタンプおよび属性を持つ `struct` オブジェクトを返します。

**クエリ構文**

```sql
ATTRIBUTION_LAST_TOUCH_EXP_IF(
    {TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}, {EXP_CONDITION}, {EXP_BEFORE}) 
    OVER ({PARTITION} {ORDER} {FRAME})
```

必要になる可能性のあるパラメーターとその説明の完全なリストについては、[&#x200B; アトリビューションクエリパラメーターの節](#attribution-query-parameters)を参照してください。

**クエリの例**

次の例では、結果に表示されている4日間（7月15日、21日、23日、29日）ごとに購入が記録され（`commerce.purchases.value IS NOT NULL`）、各日の最後のトラッキングコードは、顧客のアクションに対する100% （`1.0`）の責任を負っています。

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

`last_touch`列に表示される結果の内訳については、「[列コンポーネント」セクション &#x200B;](#query-result-column-components)を参照してください。

### 有効期限タイムアウトを持つラストタッチ属性 {#last-touch-attribution-with-expiration-timeout}

このクエリは、選択した時間間隔の最後のインタラクションを検索するために使用されます。 クエリは、指定された期間のターゲット [!DNL Experience Event] データセット内の1つのチャネルのラストタッチアトリビューション値と詳細を返します。 クエリは、選択したチャネルに対して返される各行のラストタッチ値、タイムスタンプおよび属性を持つ `struct` オブジェクトを返します。

**クエリ構文**

```sql
ATTRIBUTION_LAST_TOUCH_EXP_TIMEOUT(
    {TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}, {EXP_TIMEOUT}) 
    OVER ({PARTITION} {ORDER} {FRAME})
```

必要になる可能性のあるパラメーターとその説明の完全なリストについては、[&#x200B; アトリビューションクエリパラメーターの節](#attribution-query-parameters)を参照してください。

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

`last_touch`列に表示される結果の内訳については、「[列コンポーネント」セクション &#x200B;](#query-result-column-components)を参照してください。
