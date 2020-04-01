---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: サンプルクエリ
topic: queries
translation-type: tm+mt
source-git-commit: 45da024d45b5eebdfc393ee14890e24aed6021ce

---


# Adobe Analyticsクエリのサンプルデータ

選択したAdobe AnalyticsレポートスイートのデータはXDM ExperienceEventsに変換され、データセットとしてAdobe Experience Platformに取り込まれます。 このドキュメントでは、Adobe Experience Platformクエリサービスがこのデータを利用する多くの使用例を説明し、付属のサンプルクエリがAdobe Analyticsデータセットと連携するようにします。 XDM ExperienceEventsへのマッピングにつ [いて詳しくは、](../../source-connectors/ui/adobe-applications/analytics-mapping.md) Analyticsのフィールドマッピングドキュメントを参照してください。

## はじめに

このドキュメント全体のSQLの例では、SQLを編集し、評価するデータセット、eVar、イベントまたは期間に基づいて、クエリに対して期待されるパラメーターを入力する必要があります。 後述のSQLの例に示すよ `{ }` うに、任意のパラメータを指定します。

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

### 最もアクティブな上位10人のユーザ

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

### 上位10都市（ユーザ別）のアクティビティ

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

### 閲覧された商品の上位10件

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

### 上位10件の注文売上高

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

### イベント数（日別）

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

## マーチャンダイジング変数（製品構文）

Adobe Analyticsでは、「マーチャンダイジング変数」と呼ばれる特別に設定された変数を使用して、カスタムの製品レベルのデータを収集できます。 eVarまたはカスタムイベント。 これらの変数とその標準的な使用法の違いは、ヒットの単一の値ではなく、ヒットの各商品の個別の値を表すという点です。 これらの変数は、製品構文マーチャンダイジング変数と呼ばれます。 これにより、製品ごとの「割引金額」や、顧客の検索結果での製品の「ページ上の場所」に関する情報などの情報を収集できます。

Analyticsデータセット内のマーチャンダイジング変数にアクセスするXDMフィールドを次に示します。

### eVar

```
productListItems[#]._experience.analytics.customDimensions.evars.evar#
```

は配 `[#]` 列インデックス、は特 `evar#` 定のeVar変数です。

### カスタムイベント

```
productListItems[#]._experience.analytics.event1to100.event#.value
```

ここで、 `[#]` は配列インデックスで、は特 `event#` 定のカスタムイベント変数です。

### サンプルクエリ

以下に、にある最初のクエリのマーチャンダイジングeVarとイベントを返すサンプル製品を示しま `productListItems`す。

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

この次のクエリは、を「展開」し、製 `productListItems` 品ごとの各マーチャンダイジングeVarとイベントを返します。 元のヒ `_id` ットとの関係を示すために、このフィールドが含まれます。 この値 `_id` は、ExperienceEventデータセットの一意の主キーです。

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

### サンプルエラーの実装時の一般的なクエリ

現在のデータセットに存在しないフィールドを取得しようとすると、「No such field」エラーが発生します。 エラーメッセージに返された理由を評価して使用可能なフィールドを特定し、クエリを更新して再実行します。

```
ERROR: ErrorCode: 08P01 sessionId: XXXX queryId: XXXX Unknown error encountered. Reason: [No such struct field evar1 in eVar10, eVar13, eVar62, eVar88, eVar2;]
```

## マーチャンダイジング変数（コンバージョン構文）

Adobe Analyticsにある別のタイプのマーチャンダイジング変数は、コンバージョン構文です。 製品の構文を使用すると、値は製品と同時に収集されますが、同じページにデータが存在する必要があります。 製品に関連するコンバージョンや関心のあるイベントの前に、ページでデータが発生するシナリオがあります。 例えば、商品を探す方法の使用例を考えてみましょう。レポートの使用例を考えてみましょう。

1. ユーザが「winter hat」を実行し、内部検索を行い、コンバージョン構文が有効なマーチャンダイジングeVar6を「internal search:winter hat」に設定
2. ユーザが「ワッフルビーニー」をクリックし、製品の詳細ページに移動します。\
   a.ここでの着陸は、12.99ドルの「ワッフル `Product View` イベント」のためのランディングが発生します。\
   b.はバイ `Product View` ンディングイベントとして設定されているので、製品の「ワッフルビーニー」は「内部検索：冬帽」のeVar6値にバインドされるようになりました。 「ワッフル豆の品」が収集されるたびに、(1)有効期限の設定に達するか、(2)新しいeVar6値が設定され、その製品で再びバインディングイベントが発生するまで、「内部検索：冬帽」に関連付けられます。
3. ユーザーが製品を買い物かごに追加し、製品が起動さ `Cart Add` れます。
4. ユーザが「summer shirt」に対して別の内部検索を実行すると、コンバージョン構文によりマーチャンダイジングeVar6が「internal search:summer shirt」に設定されます。
5. ユーザーが「sporty t-shirt」をクリックし、製品の詳細ページに移動します。\
   a.着陸は「スポーティ `Product View` Tシャツ19.99ドル」のイベントを発射する。\
   b.イベント `Product View` は引き続きバインディングイベントなので、商品「sporty t-shirt」が「internal search:summer shirt」のeVar6値に、以前の商品「waffle beanie」が「internal search:waffle beanie」のeVar6値にバインドされています。
6. ユーザーが製品を買い物かごに追加し、製品が起動さ `Cart Add` れます。
7. ユーザーは両方の製品をチェックアウトします。

レポートでは、注文件数、売上高、商品表示数、買い物かごへの追加数がeVar6に対してレポートされ、連結された商品のアクティビティに合わせて調整されます。

| eVar6（製品の検索方法） | 売上高 | 注文 | 製品表示 | 買い物かごの追加 |
|---|---|---|---|---|
| 内部検索：サマーシャツ | 19.99 | 1 | 1 | 1 |
| 内部検索：冬帽 | 12.99 | 1 | 1 | 1 |

Analyticsデータセットにコンバージョン構文を生成するためのXDMフィールドを次に示します。

### eVar

```
_experience.analytics.customDimensions.evars.evar#
```

は特 `evar#` 定のeVar変数です。

### 製品

```
productListItems[#].sku
```

は配 `[#]` 列インデックスです。

### サンプルクエリ

次に、値を特定の製品とイベントのペア(この場合は製品の表示イベント)に連結するサンプルクエリを示します。

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

それぞれの製品の後続のクエリに連結値を永続化するサンプルのオカレンスを示します。 最も低いサブクエリは、宣言されたバインディングイベントの製品と値の関係を確立します。 次のサブクエリは、それぞれの製品との後続のインタラクションにわたって、その連結値のアトリビューションを実行します。 最上位レベルでは、集計を選択して結果を生成します。

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
