---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: アドビ定義関数
topic: queries
translation-type: tm+mt
source-git-commit: 3b710e7a20975880376f7e434ea4d79c01fa0ce5
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 71%

---


# アドビ定義関数の使用

アドビの大きな差別化要因の 1 つは、エクスペリエンスデータについて、およびそのデータを使用して顧客が何をする必要があるかを把握しているということです。お客様はこの理解に基づいて、業務を容易におこなえるようヘルパー関数を作成できます。

This document covers Adobe-defined functions (ADFs) to support three key [!DNL Analytics] activities:
- [セッション化](#sessionization)
- [アトリビューション](#attribution)
- [パス](#pathing)

## セッション化

`SESS_TIMEOUT()` は、Adobe Analytics で見つかった訪問のグループ化を再現します。時間ベースのグループ化と同様の動作を実行しますが、パラメーターはカスタマイズ可能です。

**構文：**

`SESS_TIMEOUT(timestamp, timeout_in_seconds) OVER ([partition] [order] [frame])`

**戻り値：**

フィールドを含む構造`(timestamp_diff, num, is_new, depth)`

### 行レベルの `SESS_TIMEOUT()` と出力の参照

```sql
SELECT analyticsVisitor,
      session.is_new,
      session.timestamp_diff,
      session.num,
      session.depth
FROM  (
        SELECT endUserIDs._experience.aaid.id as analyticsVisitor,
        SESS_TIMEOUT(timestamp, 60 * 30)
        OVER (PARTITION BY endUserIDs._experience.aaid.id
        ORDER BY timestamp
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
        AS session
        FROM your_analytics_table
        WHERE _ACP_YEAR = 2018
      )
LIMIT 100;
```

![画像](../images/queries/adobe-functions/sess-timeout.png)

### 訪問者、セッションおよびページビューを使用して新しいトレンドレポートを作成する

```sql
SELECT
      date_format( from_utc_timestamp(timestamp, 'EDT') , 'yyyy-MM-dd') as Day,
      COUNT(DISTINCT analyticsVisitor ) as Visitors,
      COUNT(DISTINCT analyticsVisitor || session.num ) as Sessions,
      SUM( PageViews ) as PageViews
FROM
    (
      SELECT
          timestamp,
          endUserIDs._experience.aaid.id as analyticsVisitor,
          SESS_TIMEOUT(timestamp, 60 * 30)
      OVER (PARTITION BY endUserIDs._experience.aaid.id
      ORDER BY timestamp
      ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
      AS session,
          web.webPageDetails.pageviews.value as PageViews
      FROM your_analytics_table
      WHERE _ACP_YEAR = 2018
    )
GROUP BY Day 
ORDER BY Day DESC 
LIMIT 31;
```

![画像](../images/queries/adobe-functions/trended-report.png)

## アトリビューション

アトリビューションとは、売上高、注文、新規登録などの指標やコンバージョンをマーケティング活動に割り当てる方法です。

Adobe Analytics では、アトリビューション設定は eVar などの変数を使用して設定され、データの取り込み時に生成されます。

The Attribution ADFs found in [!DNL Query Service] allow those allocations to be defined and generated at query time.

この例では、ラストタッチアトリビューションに焦点を当てますが、アドビではファーストタッチオファーのアトリビューションも提供しています。

>[!NOTE]
>
>Other options with timeouts and event-based expiration will be available in future versions of [!DNL Query Service].

**構文：**

`ATTRIBUTION_LAST_TOUCH(timestamp, [channel_name], column) OVER ([partition] [order] [frame])`

**戻り値：**

フィールドを含む構造`(value)`

### 行レベルのアトリビューションの参照

```sql
SELECT
  endUserIds._experience.aaid.id,
  _experience.analytics.customDimensions.evars.evar10 as MemberLevel,
  ATTRIBUTION_LAST_TOUCH(timestamp, 'eVar10', _experience.analytics.customDimensions.evars.evar10)
      OVER(PARTITION BY endUserIds._experience.aaid.id
           ORDER BY timestamp
           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW).value
      AS LastMemberLevel,
  commerce.purchases.value as Orders
FROM your_analytics_table 
WHERE _ACP_YEAR=2018 AND _ACP_MONTH=4
LIMIT 50;
```

![画像](../images/queries/adobe-functions/row-level-attribution.png)

### 注文の内訳（最後のメンバーレベル別）を作成する（eVar10）

```sql
SELECT
  LastMemberLevel,
  SUM(Orders) as MemberLevelOrders
FROM 
(SELECT
  ATTRIBUTION_LAST_TOUCH(timestamp, 'eVar10', _experience.analytics.customDimensions.evars.evar10)
      OVER(PARTITION BY endUserIds._experience.aaid.id
           ORDER BY timestamp
           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW).value
      AS LastMemberLevel,
  commerce.purchases.value as Orders
FROM your_analytics_table 
WHERE _ACP_YEAR=2018 AND _ACP_MONTH=4
)
GROUP BY LastMemberLevel 
ORDER BY MemberLevelOrders DESC
LIMIT 25;
```

![画像](../images/queries/adobe-functions/last-member-level.png)

## パス

パスは、顧客がサイトをどのように移動したかを理解するのに役立ちます。`NEXT()` および `PREVIOUS()` ADF を使用することでこれが可能となります。

**構文：**

```
NEXT(key, [shift, [ignoreNulls]]) OVER ([partition] [order] [frame])
PREVIOUS(key, [shift, [ignoreNulls]]) OVER ([partition] [order] [frame])
```

**戻り値：**

フィールドを含む構造`(value)`

### 現在のページと次のページを選択する

```sql
SELECT 
      endUserIds._experience.aaid.id,
      timestamp,
      web.webPageDetails.name,
      NEXT(web.webPageDetails.name, 1, true)
          OVER(PARTITION BY endUserIds._experience.aaid.id
              ORDER BY timestamp
              ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING).value
          AS next_pagename
FROM your_analytics_table
WHERE _ACP_YEAR=2018 
LIMIT 10;
```

![画像](../images/queries/adobe-functions/select-current-page.png)

### セッション開始時の上位 5 ページの名前に対する内訳レポートを作成する

```sql
  SELECT 
    PageName,
    PageName_2,
    PageName_3,
    PageName_4,
    PageName_5,
    SUM(PageViews) as PageViews
  FROM
    (SELECT
      PageName,
      NEXT(PageName, 1, true)
        OVER(PARTITION BY VisitorID, session.num
              ORDER BY timestamp
              ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING).value
        AS PageName_2,
      NEXT(PageName, 2, true)
        OVER(PARTITION BY VisitorID, session.num
              ORDER BY timestamp
              ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING).value
        AS PageName_3,
      NEXT(PageName, 3, true)
         OVER(PARTITION BY VisitorID, session.num
              ORDER BY timestamp
              ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING).value
        AS PageName_4,
      NEXT(PageName, 4, true)
         OVER(PARTITION BY VisitorID, session.num
              ORDER BY timestamp
              ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING).value
        AS PageName_5,
      PageViews,
      session.depth AS SessionPageDepth
    FROM
      (SELECT
        endUserIds._experience.aaid.id as VisitorID,
        timestamp,
        web.webPageDetails.pageviews.value AS PageViews,
        web.webPageDetails.name AS PageName,
        SESS_TIMEOUT(timestamp, 60 * 30) 
          OVER (PARTITION BY endUserIDs._experience.aaid.id 
                ORDER BY timestamp 
                ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
        AS session
      FROM your_analytics_table
      WHERE _ACP_YEAR=2018)
    )
  WHERE SessionPageDepth=1
  GROUP BY PageName, PageName_2, PageName_3, PageName_4, PageName_5
  ORDER BY PageViews DESC
  LIMIT 100;
```

![画像](../images/queries/adobe-functions/create-breakdown-report.png)

## その他のリソース

次のビデオでは、Adobe Experience PlatformインターフェイスとPSQLクライアントでクエリを実行する方法を示します。 また、このビデオでは、XDMオブジェクト内の個々のプロパティ、Adobe定義関数の使用、CREATE TABLE AS SELECT(CTAS)の使用に関する例も使用しています。

>[!VIDEO](https://video.tv.adobe.com/v/29796?quality=12&learn=on)

