---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: アドビ定義関数
topic: queries
translation-type: tm+mt
source-git-commit: 41fdee979db32b97a5935a02e9ffcde3308b6d54

---


# アドビ定義関数の使用

アドビの大きな差別化要因の1つは、エクスペリエンスデータを理解し、顧客がそのデータを使用して何を行う必要があるかを把握することです。 この理解を利用して、ジョブをより簡単にするヘルパー関数を作成できます。

このドキュメントでは、Adobeで定義された関数(ADF)について説明し、次の3つの主要なAnalyticsアクティビティをサポートします。
- [セッション化](#sessionization)
- [帰属](#attribution)
- [パス](#pathing)

## セッション化

は、Adobe Analytics `SESS_TIMEOUT()` で見つかった訪問のグループを再現します。 時間ベースのグループ化と同様の動作を行いますが、パラメーターはカスタマイズ可能です。

**構文：**

`SESS_TIMEOUT(timestamp, timeout_in_seconds) OVER ([partition] [order] [frame])`

**戻り値：**

フィールドを含む構造 `(timestamp_diff, num, is_new, depth)`

### 行レベルと出力の `SESS_TIMEOUT()` 調査

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

### 訪問者、セッションおよびページ表示

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

## 帰属

アトリビューションとは、売上高、注文、サインアップなどの指標やコンバージョンをマーケティング活動に割り当てる方法です。

Adobe Analyticsでは、アトリビューション設定はeVarなどの変数を使用して設定され、データの取り込み時に生成されます。

アトリビューションサービスで見つかったAttribution ADFを使用すると、クエリ時にこれらの割り当てを定義し、生成することができます。クエリ時に

この例では、ラストタッチアトリビューションに焦点を当てますが、アドビではファーストタッチオファーのアトリビューションも説明しています。

>[!NOTE] タイムアウトとイベントベースの有効期限が設定されたその他のオプションは、今後のバージョンのクエリサービスで利用できます。

**構文：**

`ATTRIBUTION_LAST_TOUCH(timestamp, [channel_name], column) OVER ([partition] [order] [frame])`

**戻り値：**

フィールドを含む構造 `(value)`

### 行レベルのアトリビューションの調査

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

### 最後のメンバーレベル別の注文の分類の作成(eVar10)

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

パスは、顧客がサイトをどのようにナビゲートしているかを理解するのに役立ちます。 これは、 `NEXT()` ADFとADF `PREVIOUS()` が可能にする。

**構文：**

```
NEXT(key, [shift, [ignoreNulls]]) OVER ([partition] [order] [frame])
PREVIOUS(key, [shift, [ignoreNulls]]) OVER ([partition] [order] [frame])
```

**戻り値：**

フィールドを含む構造 `(value)`

### 現在のページと次のページを選択

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

### セッションの入口時に上位5つのページ名に対する内訳レポートを作成する

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

