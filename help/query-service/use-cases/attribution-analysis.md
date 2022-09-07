---
title: 属性分析
description: このドキュメントでは、クエリサービスを使用して、ファーストタッチとラストタッチのマーケティング属性モデルに基づいて、マーケティングの効果を測定する手法を作成する方法を説明します。
exl-id: d62cd349-06fc-4ce6-a5e8-978f11186927
source-git-commit: e33d59c4ac28f55ba6ae2fc073d02f8738159263
workflow-type: tm+mt
source-wordcount: '1419'
ht-degree: 13%

---

# 属性分析

アトリビューションは、ビジネスの販売やコンバージョンに貢献するチャネル、オファー、メッセージなどのマーケティング戦術を決定するのに役立つ分析的概念です。 この概念は、消費者のジャーニー（顧客が会社とやり取りして目標を達成するプロセス）を評価し、（消費者がブランドとやり取りするたびに）顧客タッチポイントに基づいて購入または獲得を導き出します。 アトリビューション分析を通じて、マーケターは、潜在的な顧客につながるチャネルの投資回収率を評価できます。

## はじめに

このドキュメント全体の SQL の例は、Adobe Analyticsデータでよく使用されるクエリです。 このチュートリアルでは、次の コンポーネントに関する十分な知識が必要です。

* [レポートスイートデータ用のAdobe Analyticsソースコネクタの概要](../../sources/connectors/adobe-applications/mapping/analytics.md).
* [Analytics フィールドマッピングドキュメント](../../sources/connectors/adobe-applications/mapping/analytics.md) クエリサービスで使用する分析データの取得とマッピングに関する詳細情報を提供します。
* [Attribution IQの概要](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/attribution/overview.html?lang=ja)
* [Adobe Analyticsアトリビューションパネルガイド](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/panels/attribution.html?lang=ja).

内のパラメーターの説明 `OVER()` 関数は [窓関数セクション](../sql/adobe-defined-functions.md#window-functions). この [Adobeマーケティングおよびコマースの用語集](https://business.adobe.com/glossary/index.html) また、使用する場合もあります。

次の各使用例では、パラメーター化された SQL クエリの例が、カスタマイズ用のテンプレートとして提供されています。 が表示される場所にパラメーターを入力 `{ }` を参照してください。

## 目標

アトリビューションのユースケースでは、Adobe Analyticsデータを使用して、顧客のアクションを成功に導くのに役立ちます。 この関連付けは、顧客体験に影響を与える要因を理解する上で重要な役割を果たします。 アトリビューション分析データは、カスタマージャーニーにおける顧客のタッチポイントの重要性を理解するために使用できます。

このドキュメントに含まれるクエリ例は、ファーストタッチとラストタッチの様々な使用例をサポートし、有効期限の設定が異なります。 このガイドでは、次の主要概念について説明します。

* ファーストタッチおよびラストタッチの属性。
* 有効期限タイムアウトを持つファーストタッチおよびラストタッチの属性。
* 有効期限条件を持つファーストタッチおよびラストタッチ属性。

## 属性クエリパラメーター {#attribution-query-parameters}

次の表に、ファーストタッチおよびラストタッチの属性クエリで使用されるパラメーターとその説明の分類を示します。

| パラメーター | 説明 |
|---|---|
| `{TIMESTAMP}` | データセット内のタイムスタンプフィールド。 |
| `{CHANNEL_NAME}` | 返されるオブジェクトのラベル。 |
| `{CHANNEL_VALUE}` | クエリ－のターゲットチャネルである列またはフィールド. |
| `{EXP_TIMEOUT}` | クエリがファーストタッチイベントを検索するチャネルイベントの前の時間（秒）。 |
| `{EXP_CONDITION}` | チャネルの有効期限を決定する条件. |
| `{EXP_BEFORE}` | 指定した条件の前または後にチャネルの有効期限が切れるかどうかを示す boolean。 `{EXP_CONDITION}`、が満たされた場合にのみ表示されます。 これは主に、セッションの有効期限条件に対して有効になり、前のセッションからファーストタッチが選択されないようにします。 デフォルトでは、この値は `false` に設定されています。 |

## クエリ結果列のコンポーネント {#query-result-column-components}

属性クエリの結果は、 `first_touch` または `last_touch` 列。 これらの列は、次のコンポーネントで構成されています。

```console
({NAME}, {VALUE}, {TIMESTAMP}, {FRACTION})
```

| パラメーター | 説明 |
| ---------- | ----------- |
| `{NAME}` | この `{CHANNEL_NAME}`Azure Data Factory(ADF) にラベルとして入力された。 |
| `{VALUE}` | 指定した `{EXP_TIMEOUT}` 間隔内のラストタッチである `{CHANNEL_VALUE}` から得た値 |
| `{TIMESTAMP}` | のタイムスタンプ [!DNL Experience Event] ラストタッチが発生した場所 |
| `{FRACTION}` | ラストタッチの属性。小数で表されます。 |

### ファーストタッチ属性 {#first-touch}

ファーストタッチ属性は、消費者が遭遇した最初のチャネルへの成功の結果に対する責任の 100%を占めます。 この SQL の例を使用して、後続の一連の顧客アクションにつながったインタラクションを強調表示します。

以下のクエリは、ファーストタッチ属性値と、ターゲット内のチャネルの詳細を返します [!DNL Experience Event] データセット。 また、 `struct` 各行のファーストタッチ値、タイムスタンプおよび属性を持つ、選択したチャネルのオブジェクト。

>[!NOTE]
>
>Experience CloudID(ECID) は MCID とも呼ばれ、名前空間で引き続き使用されます。

**クエリ構文**

```sql
ATTRIBUTION_FIRST_TOUCH({TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}) OVER ({PARTITION} {ORDER} {FRAME})
```

潜在的に必要なパラメーターの完全なリストとその説明については、 [属性クエリー・パラメータの節](#attribution-query-parameters).

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

以下の結果では、最初のトラッキングコード `em:946426` が [!DNL Experience Event] データセット。 このトラッキングコードは 100% (`1.0`) に記載されています。これは、最初のインタラクションであったためです。

```console
                 id                 |       timestamp       | trackingCode |                   first_touch                   
-----------------------------------+-----------------------+--------------+-------------------------------------------------
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

結果の分類を表示する場合は、 `first_touch` 列、「 [列コンポーネントセクション](#query-result-column-components).

### ラストタッチ属性 {#second-touch}

ラストタッチ属性は、消費者が最後に発生したチャネルに対し、成功の結果に対する責任の 100%を占めます。 この SQL の例を使用して、一連の顧客アクションの最終的なインタラクションを強調表示します。

クエリは、ラストタッチ属性値と、ターゲット内のチャネルの詳細を返します [!DNL Experience Event] データセット。 また、 `struct` 各行のラストタッチ値、タイムスタンプおよび属性を持つ、選択したチャネルのオブジェクト。

**クエリ構文**

```sql
ATTRIBUTION_LAST_TOUCH({TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}) OVER ({PARTITION} {ORDER} {FRAME})
```

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

以下に表示される結果では、返されるオブジェクトのトラッキングコードが、各 [!DNL Experience Event] レコード。 各コードは 100%と見なされます (`1.0`) 顧客の行動に対する責任（最後のインタラクションとしての責任）。

```console
                 id                |       timestamp       | trackingCode |                   last_touch                   
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

結果の分類を表示する場合は、 `last_touch` 列、「 [列コンポーネントセクション](#query-result-column-components).

### 有効期限条件を持つファーストタッチ属性 {#first-touch-attribution-with-expiration-condition}

このクエリは、 [!DNL Experience Event] データセットは、選択した条件によって決定されます。

クエリは、ターゲット内の単一チャネルのファーストタッチ属性値と詳細を返します [!DNL Experience Event] データセットに含まれ、条件の前後で期限切れになるデータセット。 また、 `struct` オブジェクトに含まれます。

**クエリ構文**

```sql
ATTRIBUTION_FIRST_TOUCH_EXP_IF(
    {TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}, {EXP_CONDITION}, {EXP_BEFORE})
    OVER ({PARTITION} {ORDER} {FRAME})
```

潜在的に必要なパラメーターの完全なリストとその説明については、 [属性クエリー・パラメータの節](#attribution-query-parameters).

**クエリ例**

以下の例では、購入が記録されます (`commerce.purchases.value IS NOT NULL`) は、結果に表示される 4 日（7 月 15 日、21 日、23 日、29 日）ごとに、各日の初期トラッキングコードは 100%(`1.0`) お客様のアクションに対する責任。

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
----------------------------------+-----------------------+--------------+-------------------------------------------------
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

結果の分類を表示する場合は、 `first_touch` 列、「 [列コンポーネントセクション](#query-result-column-components).

### 有効期限タイムアウトを持つファーストタッチ属性 {#first-touch-attribution-with-expiration-timeout}

このクエリは、選択した期間内で顧客アクションの成功につながったインタラクションを見つけるために使用されます。

以下のクエリは、ターゲット内の単一チャネルのファーストタッチ属性値と詳細を返します [!DNL Experience Event] 指定した期間のデータセット。 クエリは、選択したチャネルに対して返される各行のファーストタッチ値、タイムスタンプおよび属性を持つ `struct` オブジェクトを返します。

**クエリ構文**

```sql
ATTRIBUTION_FIRST_TOUCH_EXP_IF(
    {TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}, {EXP_CONDITION}, {EXP_BEFORE})
    OVER ({PARTITION} {ORDER} {FRAME})
```

潜在的に必要なパラメーターの完全なリストとその説明については、 [属性クエリー・パラメータの節](#attribution-query-parameters).

**クエリ例**

次の例では、各顧客アクションに対して返されるファーストタッチが、過去 7 日間で最も早いインタラクションです (expTimeout = 86400 * 7)。

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
-----------------------------------+-----------------------+--------------+-------------------------------------------------
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

結果の分類を表示する場合は、 `first_touch` 列、「 [列コンポーネントセクション](#query-result-column-components).

### 有効期限条件を持つラストタッチ属性 {#last-touch-attribution-with-expiration-condition}

このクエリは、 [!DNL Experience Event] データセットは、選択した条件によって決定されます。

以下のクエリは、ターゲット内の単一チャネルのラストタッチ属性値と詳細を返します [!DNL Experience Event] データセットに含まれ、条件の前後で期限切れになるデータセット。 クエリは、選択したチャネルに対して返される各行のラストタッチ値、タイムスタンプおよび属性を持つ `struct` オブジェクトを返します。

**クエリ構文**

```sql
ATTRIBUTION_LAST_TOUCH_EXP_IF(
    {TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}, {EXP_CONDITION}, {EXP_BEFORE}) 
    OVER ({PARTITION} {ORDER} {FRAME})
```

潜在的に必要なパラメーターの完全なリストとその説明については、 [属性クエリー・パラメータの節](#attribution-query-parameters).

**クエリ例**

以下の例では、購入が記録されます (`commerce.purchases.value IS NOT NULL`) は、結果に表示される 4 日（7 月 15 日、21 日、23 日、29 日）ごとに、各日の最後のトラッキングコードは 100%(`1.0`) お客様のアクションに対する責任。

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
-----------------------------------+-----------------------+--------------+------------------------------------------------
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

結果の分類を表示する場合は、 `last_touch` 列、「 [列コンポーネントセクション](#query-result-column-components).

### 有効期限タイムアウトを持つラストタッチ属性 {#last-touch-attribution-with-expiration-timeout}

このクエリは、選択した時間間隔内の最後のインタラクションを検索するために使用されます。 クエリは、ターゲット内の単一チャネルのラストタッチ属性値と詳細を返します [!DNL Experience Event] 指定した期間のデータセット。 クエリは、選択したチャネルに対して返される各行のラストタッチ値、タイムスタンプおよび属性を持つ `struct` オブジェクトを返します。

**クエリ構文**

```sql
ATTRIBUTION_LAST_TOUCH_EXP_TIMEOUT(
    {TIMESTAMP}, {CHANNEL_NAME}, {CHANNEL_VALUE}, {EXP_TIMEOUT}) 
    OVER ({PARTITION} {ORDER} {FRAME})
```

潜在的に必要なパラメーターの完全なリストとその説明については、 [属性クエリー・パラメータの節](#attribution-query-parameters).

**クエリ例**

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

結果の分類を表示する場合は、 `last_touch` 列、「 [列コンポーネントセクション](#query-result-column-components).
