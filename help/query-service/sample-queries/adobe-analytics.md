---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: サンプルクエリ
topic: queries
translation-type: tm+mt
source-git-commit: bfbf2074a9dcadd809de043d62f7d2ddaa7c7b31
workflow-type: tm+mt
source-wordcount: '862'
ht-degree: 1%

---


# AdobeAnalyticsデータのサンプルクエリ

選択したAdobeAnalyticsレポートスイートのデータはXDMに変換され、Adobe Experience Platformにデータセットとして取り込ま [!DNL ExperienceEvents] れます。 このドキュメントでは、Adobe Experience Platformがこのデータを使用する様々な使用例を概説し、付属のサンプルクエリはAdobeAnalyticsのデータセットと連携する必要があります。 [!DNL Query Service] XDMへのマッピングについて詳しくは、 [Analyticsのフィールドマッピングドキュメント](../../sources/connectors/adobe-applications/mapping/analytics.md) を参照してくだ [!DNL ExperienceEvents]さい。

## はじめに

このドキュメント全体で示すSQLの例では、SQLを編集し、評価したいデータセット、eVar、イベント、または期間に基づいて、クエリに期待されるパラメーターを設定する必要があります。 次に示すSQLの例で示すよう `{ }` に、任意の場所にパラメータを指定します。

## よく使用されるSQLの例

### 特定の日の時間別訪問者数

```sql
SELECT Substring(from_utc_timestamp(timestamp, 'America/New_York'), 1, 10) AS Day,
       Substring(from_utc_timestamp(timestamp, 'America/New_York'), 12, 2) AS Hour, 
       Count(DISTINCT enduserids._experience.aaid.id) AS Visitor_Count 
FROM   {target_table}
WHERE _acp_year = {target_year} 
      AND _acp_month = {target_month}  
      AND _acp_day = {target_day}
GROUP BY Day, Hour
ORDER BY Hour;
```

### 特定の日に閲覧された上位10ページ

```sql
SELECT web.webpagedetails.name AS Page_Name, 
       Sum(web.webpagedetails.pageviews.value) AS Page_Views 
FROM   {target_table}
WHERE  _acp_year = {target_year}
       AND _acp_month = {target_month}
       AND _acp_day = {target_day}
GROUP BY web.webpagedetails.name 
ORDER BY page_views DESC 
LIMIT  10;
```

### 最もアクティブなユーザーの上位10人

```sql
SELECT enduserids._experience.aaid.id AS aaid, 
       Count(timestamp) AS Count
FROM   {target_table}
WHERE  _acp_year = {target_year}
       AND _acp_month = {target_month}
       AND _acp_day = {target_day}
GROUP BY enduserids._experience.aaid.id
ORDER BY Count DESC
LIMIT  10;
```

### ユーザーアクティビティ別上位10都市

```sql
SELECT concat(placeContext.geo.stateProvince, ' - ', placeContext.geo.city) AS state_city, 
       Count(timestamp) AS Count
FROM   {target_table}
WHERE  _acp_year = {target_year}
       AND _acp_month = {target_month}
       AND _acp_day = {target_day}
GROUP BY state_city
ORDER BY Count DESC
LIMIT  10;
```

### 閲覧された商品のトップ10

```sql
SELECT Product_SKU,
       Sum(Product_Views) AS Total_Product_Views
FROM  (SELECT Explode(productlistitems.sku) AS Product_SKU, 
              commerce.productviews.value   AS Product_Views 
       FROM   {target_table}
       WHERE  _acp_year = {target_year}
              AND _acp_month = {target_month}
              AND _acp_day = {target_day}
              AND commerce.productviews.value IS NOT NULL) 
GROUP BY Product_SKU 
ORDER BY Total_Product_Views DESC
LIMIT  10;
```

### 上位10件の注文総売上高

```sql
SELECT Purchase_ID, 
       Round(Sum(Product_Items.priceTotal * Product_Items.quantity), 2) AS Total_Order_Revenue 
FROM   (SELECT commerce.`order`.purchaseid AS Purchase_ID, 
               Explode(productlistitems)   AS Product_Items 
        FROM   {target_table} 
        WHERE  commerce.`order`.purchaseid IS NOT NULL 
               AND _acp_year = {target_year} 
               AND _acp_month = {target_month}  
               AND _acp_day = {target_day}) 
GROUP BY Purchase_ID 
ORDER BY total_order_revenue DESC 
LIMIT  10;
```

### 日別イベント数

```sql
SELECT Substring(from_utc_timestamp(timestamp, 'America/New_York'), 1, 10) AS Day, 
       Substring(from_utc_timestamp(timestamp, 'America/New_York'), 12, 2) AS Hour, 
       Sum(_experience.analytics.event1to100.{target_event}.value) AS Event_Count
FROM   {target_table}
WHERE  _experience.analytics.event1to100.{target_event}.value IS NOT NULL 
        AND _acp_year = {target_year} 
        AND _acp_month = {target_month}  
        AND _acp_day = {target_day}
GROUP BY Day, Hour
ORDER BY Hour;
```

## マーチャンダイジング変数（製品の構文）

アドビのAnalyticsでは、カスタムの製品レベルのデータは、「マーチャンダイジング変数」と呼ばれる特別に設定された変数を通して収集できます。 eVarまたはカスタムイベントに基づいています。 これらの変数とその標準的な使用方法の違いは、ヒットの単一の値ではなく、ヒットで見つかった各製品の個別の値を表すことです。 これらの変数は、製品構文マーチャンダイジング変数と呼ばれます。 これにより、製品ごとの「割引金額」や、顧客の検索結果での製品の「ページ上の場所」に関する情報などの情報を収集できます。

デー [!DNL Analytics] タセット内のマーチャンダイジング変数にアクセスするためのXDMフィールドを次に示します。

### eVar

```
productListItems[#]._experience.analytics.customDimensions.evars.evar#
```

ここ `[#]` で、は配列インデックス、 `evar#` は特定のeVar変数です。

### カスタムイベント

```
productListItems[#]._experience.analytics.event1to100.event#.value
```

ここ `[#]` で、は配列インデックス、 `event#` は特定のカスタムイベント変数です。

### サンプルクエリ

にある最初の商品のマーチャンダイジングeVarとイベントを返すクエリ例を示し `productListItems`ます。

```sql
SELECT
  productListItems[0]._experience.analytics.customDimensions.evars.eVar1,
  productListItems[0]._experience.analytics.event1to100.event1.value
FROM adobe_analytics_midvalues
WHERE _ACP_YEAR=2019 AND _ACP_MONTH=7 AND _ACP_DAY=23
  AND productListItems[0].SKU IS NOT NULL
  AND productListItems[0]._experience.analytics.customDimensions.evars.eVar1 IS NOT NULL
  AND productListItems[0]._experience.analytics.event1to100.event1.value IS NOT NULL
LIMIT 10
```

この次のクエリは、各マーチャンダイジングeVar `productListItems` とイベントを製品ごとに「展開」して返します。 元のヒットとの関係を示す `_id` フィールドが含まれます。 この `_id` 値は、データセット内の一意の主キー [!DNL ExperienceEvent] です。

```sql
SELECT
  _id,
  productItem._experience.analytics.customDimensions.evars.eVar1,
  productItem._experience.analytics.event1to100.event1.value
FROM (
  SELECT
    _id,
    explode(productListItems) as productItem
  FROM adobe_analytics_midvalues
  WHERE _ACP_YEAR=2019 AND _ACP_MONTH=7 AND _ACP_DAY=23
  AND productListItems[0].SKU IS NOT NULL
  AND productListItems[0]._experience.analytics.customDimensions.evars.eVar1 IS NOT NULL
  AND productListItems[0]._experience.analytics.event1to100.event1.value IS NOT NULL
)
LIMIT 20
```

### サンプルクエリの実装時の一般的なエラー

現在のデータセットに存在しないフィールドを取得しようとすると、「No such field」エラーが発生します。 エラーメッセージに返された理由を評価して使用可能なフィールドを特定し、クエリを更新して再実行します。

```
ERROR: ErrorCode: 08P01 sessionId: XXXX queryId: XXXX Unknown error encountered. Reason: [No such struct field evar1 in eVar10, eVar13, eVar62, eVar88, eVar2;]
```

## マーチャンダイジング変数（コンバージョン構文）

アドビのAnalyticsにある別のタイプのマーチャンダイジング変数は、コンバージョンの構文です。 製品の構文を使用すると、製品と同時に値が収集されますが、そのためにはデータが同じページに存在する必要があります。 コンバージョンや商品に関連する関心のあるイベントの前にページでデータが発生するシナリオがあります。 例えば、商品を探す方法のレポートの使用例を考えてみましょう。

1. ユーザが「winter hat」を実行し、内部検索を行います。これにより、コンバージョン構文でマーチャンダイジングeVar6が有効になり、「internal search:winter hat」に設定されます。
2. ユーザーが「ワッフルビーニー」をクリックし、商品の詳細ページに移動します。\
   a. ここに着陸すると、12.99ドルの「ワッフルビーニー」の `Product View` イベントが発生します。\
   b. がバインディングイベント `Product View` として設定されているので、製品「ワッフルビーニー」は、eVar6の値である「internal search:winter hat」にバインドされるようになりました。 「ワッフルビーニー」商品が収集されるたびに、(1)有効期限の設定に達するか(2)新しいeVar6値が設定され、その商品で再びバインディングイベントが発生するまで、この商品は「内部検索：冬帽子」に関連付けられます。
3. ユーザーが製品を買い物かごに追加し、 `Cart Add` イベントを発生させます。
4. ユーザが「summer shirt」に対して別の内部検索を実行し、「コンバージョンの構文」で「マーチャンダイジングeVar6」が「internal search:summer shirt」に設定されています。
5. ユーザーが「sporty t-shirt」をクリックし、製品の詳細ページに移動した。\
   a. 着陸は「スポーツTシャツ19ドル99セント」の `Product View` イベントから発射する。\
   b. この `Product View` イベントは引き続きバインディングイベントなので、製品「sporty t-shirt」はeVar6の「internal search:summer shirt」に、以前の製品「waffle beanie」はeVar6の「internal search:waffle beanie」にバインドされています。
6. ユーザーが製品を買い物かごに追加し、 `Cart Add` イベントを発生させます。
7. ユーザーは両方の製品をチェックアウトします。

レポートでは、注文数、売上高、商品表示数、買い物かごへの追加数はeVar6に対してレポートでき、連結された商品のアクティビティに合わせて調整されます。

| eVar6（製品の検索方法） | 売上高 | 注文数 | 製品表示 | 買い物かごの追加 |
|---|---|---|---|---|
| 内部検索：サマーシャツ | 19.99 | 1 | 1 | 1 |
| 内部検索：冬帽 | 12.99 | 1 | 1 | 1 |

デー [!DNL Analytics] タセットに変換構文を生成するためのXDMフィールドを次に示します。

### eVar

```
_experience.analytics.customDimensions.evars.evar#
```

ここ `evar#` で、は特定のeVar変数です。

### 製品

```
productListItems[#].sku
```

ここ `[#]` で、は配列インデックスです。

### サンプルクエリ

特定の製品とイベントのペア(この場合は製品表示イベント)に値を連結するサンプルクエリを示します。

```sql
SELECT
  endUserIds._experience.aaid.id AS AAID,
  timestamp,
  CASE WHEN commerce.productViews.value = 1 THEN ATTRIBUTION_LAST_TOUCH(timestamp, 'bindConversionSyntaxMerchVariable_eVar1', _experience.analytics.customDimensions.eVars.eVar1)
  OVER(PARTITION BY endUserIds._experience.aaid.id
       ORDER BY timestamp
       ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW).value
  END AS eVar1Bind,
  EXPLODE(productListItems) AS Product_List,
  commerce.productViews.value AS prodView,
  commerce.purchases.value AS purchase
FROM adobe_analytics_midvalues
WHERE commerce.productViews.value = 1 OR commerce.purchases.value = 1 OR _experience.analytics.customDimensions.eVars.eVar1 IS NOT NULL
LIMIT 100
```

それぞれの製品の後続のオカレンスに連結値を永続化するサンプルクエリを以下に示します。 最も低いサブクエリは、宣言された連結イベント上の商品との値の関係を確立します。 次のサブクエリは、それぞれの商品との後続のインタラクションにわたって、その連結値のアトリビューションを実行します。 トップレベルでは、集計を選択してレポートを生成します。

```sql
SELECT
  Product_List.SKU,
  eVar1101ConversionSyntax,
  SUM(prodView) AS Product_Views,
  SUM(purchase) AS Purchases
FROM
(
  SELECT
    Product_List,
    ATTRIBUTION_LAST_TOUCH(timestamp, 'ConversionSyntax_eVar1', eVar1Bind)
      OVER(PARTITION BY AAID, Product_List.SKU
           ORDER BY timestamp
           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW).value
    AS eVar1ConversionSyntax,
    prodView,
    purchase
  FROM
  (
    SELECT
      endUserIds._experience.aaid.id AS AAID,
      timestamp,
      CASE WHEN commerce.productViews.value = 1 THEN ATTRIBUTION_LAST_TOUCH(timestamp, 'bindConversionSyntaxMerchVariable_eVar1', _experience.analytics.customDimensions.eVars.eVar1)
      OVER(PARTITION BY endUserIds._experience.aaid.id
           ORDER BY timestamp
           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW).value
      END AS eVar1Bind,
      EXPLODE(productListItems) AS Product_List,
      commerce.productViews.value AS prodView,
      commerce.purchases.value AS purchase
    FROM adobe_analytics_midvalues
    WHERE commerce.productViews.value = 1 OR commerce.purchases.value = 1 OR _experience.analytics.customDimensions.eVars.eVar1 IS NOT NULL
  )
)
WHERE eVar1ConversionSyntax IS NOT NULL
GROUP BY 1, 2
ORDER BY 3 DESC
LIMIT 100
```
