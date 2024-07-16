---
title: Web およびモバイルインタラクションに関する分析インサイト
description: このドキュメントでは、クエリサービスを使用して、取り込んだAdobe Analytics データから実用的なインサイトを作成する方法について説明します。
exl-id: f64e61ef-0157-4f0a-88f8-bbe4f9aa83f0
source-git-commit: cde7c99291ec34be811ecf3c85d12fad09bcc373
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 1%

---

# Web およびモバイルインタラクションに関する分析インサイト

Adobe Experience Platformでは、エクスペリエンスデータモデル（XDM）フィールドを使用してAdobe Analytics レポートスイートからデータを取り込み、データセットに入力できます。 この分析データは、[!DNL XDM ExperienceEvent] クラスに準拠するように変更されます。 その後、クエリサービスで SQL クエリを実行してこのデータを利用し、デジタルプラットフォームでのユーザーの行動から貴重なインサイトを生成できます。

このドキュメントでは、web およびモバイル分析データからインサイトを作成する際の一般的なユースケースを示す、様々な SQL クエリのサンプルを提供します。

分析データの取り込みとマッピングについて詳しくは、[Analytics フィールドのマッピングに関するドキュメント ](../../sources/connectors/adobe-applications/mapping/analytics.md) を参照してください。

## はじめに

次の各ユースケースについて、パラメーター化された SQL クエリの例をテンプレートとして提供し、カスタマイズします。 評価したいデータセット、eVar、イベント、時間枠の SQL 例で `{ }` が表示される場所にパラメーターを指定します。

## 目標

次の例は、Adobe Analytics データを分析するための一般的なユースケースに対する SQL クエリを示しています。

### 特定の日の 1 時間ごとの訪問者数の生成

```sql
SELECT Substring(from_utc_timestamp(timestamp, 'America/New_York'), 1, 10) AS Day,
       Substring(from_utc_timestamp(timestamp, 'America/New_York'), 12, 2) AS Hour,
       Count(DISTINCT enduserids._experience.aaid.id) AS Visitor_Count
FROM   {TARGET_TABLE}
WHERE TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')
GROUP BY Day, Hour
ORDER BY Hour;
```

### 特定の日に最も閲覧された 10 ページを特定する

```SQL
SELECT web.webpagedetails.name AS Page_Name,
       Sum(web.webpagedetails.pageviews.value) AS Page_Views
FROM   {TARGET_TABLE}
WHERE TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')
GROUP BY web.webpagedetails.name
ORDER BY page_views DESC
LIMIT  10;
```

### 最もアクティブな 10 人のユーザーを特定

```sql
SELECT enduserids._experience.aaid.id AS aaid,
       Count(timestamp) AS Count
FROM   {TARGET_TABLE}
WHERE TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')
GROUP BY enduserids._experience.aaid.id
ORDER BY Count DESC
LIMIT  10;
```

### ユーザーアクティビティに基づいて、最も必要な 10 の都市を特定します。

```sql
SELECT concat(placeContext.geo.stateProvince, ' - ', placeContext.geo.city) AS state_city,
       Count(timestamp) AS Count
FROM   {TARGET_TABLE}
WHERE TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')
GROUP BY state_city
ORDER BY Count DESC
LIMIT  10;
```

### 最も閲覧された 10 製品の特定

```sql
SELECT Product_SKU,
       Sum(Product_Views) AS Total_Product_Views
FROM  (SELECT Explode(productlistitems.sku) AS Product_SKU,
              commerce.productviews.value   AS Product_Views
       FROM   {TARGET_TABLE}
            WHERE TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')
              AND commerce.productviews.value IS NOT NULL)
GROUP BY Product_SKU
ORDER BY Total_Product_Views DESC
LIMIT  10;
```

### 上位 10 件の売上高の特定

```sql
SELECT Purchase_ID,
       Round(Sum(Product_Items.priceTotal * Product_Items.quantity), 2) AS Total_Order_Revenue
FROM   (SELECT commerce.`order`.purchaseid AS Purchase_ID,
               Explode(productlistitems)   AS Product_Items
        FROM   {TARGET_TABLE}
        WHERE  commerce.`order`.purchaseid IS NOT NULL
                AND TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')

GROUP BY Purchase_ID
ORDER BY total_order_revenue DESC
LIMIT  10;
```
