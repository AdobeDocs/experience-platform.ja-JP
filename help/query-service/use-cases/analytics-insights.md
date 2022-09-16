---
title: Web およびモバイルでのインタラクションに関する Analytics インサイト
description: このドキュメントでは、取り込んだAdobe Analyticsデータから実用的なインサイトを作成するクエリサービスの使用方法を説明します。
exl-id: f64e61ef-0157-4f0a-88f8-bbe4f9aa83f0
source-git-commit: d573c01a0aa9989f581796a0be4aec6904ffc569
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 1%

---

# Web およびモバイルインタラクションに関する Analytics インサイト

Adobe Experience Platformでは、エクスペリエンスデータモデル (XDM) フィールドを使用して、Adobe Analyticsレポートスイートからデータを取り込み、データセットを設定できます。 その後、クエリサービスは、SQL クエリを実行して、この分析データを利用し、デジタルプラットフォームを介したユーザーの行動から有益なインサイトを生成できます。

このドキュメントでは、Web およびモバイルの Analytics データからインサイトを作成する際の一般的な使用例を示す、様々な SQL クエリ例を提供します。

詳しくは、 [Analytics フィールドマッピングドキュメント](../../sources/connectors/adobe-applications/mapping/analytics.md) analytics データの取り込みとマッピングについて詳しくは、こちらを参照してください。

## はじめに

次の各使用例では、パラメーター化された SQL クエリの例が、カスタマイズ用のテンプレートとして提供されています。 が表示される場所にパラメーターを入力 `{ }` ( 評価するデータセット、eVar、イベントまたは時間枠の SQL 例 ) を参照してください。

## 目標

次の例は、Adobe Analyticsデータを分析する一般的な使用例に関する SQL クエリを示しています。

### 指定した日の 1 時間ごとの訪問者数を生成します

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

### 最もアクティブな 10 人のユーザーを特定します。

```sql
SELECT enduserids._experience.aaid.id AS aaid,
       Count(timestamp) AS Count
FROM   {TARGET_TABLE}
WHERE TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')
GROUP BY enduserids._experience.aaid.id
ORDER BY Count DESC
LIMIT  10;
```

### ユーザアクティビティに基づいて、最も望ましい 10 の都市を特定する

```sql
SELECT concat(placeContext.geo.stateProvince, ' - ', placeContext.geo.city) AS state_city,
       Count(timestamp) AS Count
FROM   {TARGET_TABLE}
WHERE TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')
GROUP BY state_city
ORDER BY Count DESC
LIMIT  10;
```

### 最も多く閲覧された 10 個の製品を特定する

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

### 最も売上高の高い 10 件を特定

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
