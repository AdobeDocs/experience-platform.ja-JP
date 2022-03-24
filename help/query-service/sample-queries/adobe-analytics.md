---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；クエリ例；クエリ例；クエリ例； adobe analytics;
solution: Experience Platform
title: Adobe Analytics Data のクエリ例
topic-legacy: queries
description: 選択したAdobe Analyticsレポートスイートのデータは XDM ExperienceEvents に変換され、データセットとしてAdobe Experience Platformに取り込まれます。 このドキュメントでは、クエリサービスがこのデータを利用する多くの使用例の概要を説明し、Adobe Analyticsデータセットと連携するように設計されたサンプルクエリを示します。
exl-id: 96da3713-c7ab-41b3-9a9d-397756d9dd07
source-git-commit: fec6f614946860e6ad377beaca05972a63052dd8
workflow-type: tm+mt
source-wordcount: '1066'
ht-degree: 46%

---

# Adobe Analytics データのクエリ例

選択したAdobe Analyticsレポートスイートのデータは、 [!DNL XDM ExperienceEvent] クラスを使用し、Adobe Experience Platformにデータセットとして取り込みました。

このドキュメントでは、Adobe Experience Platformの様々な使用例の概要を説明します [!DNL Query Service] は、Adobe Analyticsデータセットと連携するように設計されたサンプルクエリを含め、このデータを使用します。 詳しくは、 [Analytics フィールドマッピング](../../sources/connectors/adobe-applications/mapping/analytics.md) マッピングの詳細 [!DNL Experience Events].

## はじめに

このドキュメント全体の SQL の例では、SQL を編集し、評価するデータセット、eVar、イベントまたは期間に基づいて、クエリに対して期待されるパラメーターを入力する必要があります。後述の SQL の例では、`{ }` では常に任意のパラメータを指定します。

## よく使用される SQL の例

次の例は、Adobe Analyticsデータを分析する一般的な使用例に関する SQL クエリを示しています。

### 特定の日の時間別訪問者数

```sql
SELECT Substring(from_utc_timestamp(timestamp, 'America/New_York'), 1, 10) AS Day,
       Substring(from_utc_timestamp(timestamp, 'America/New_York'), 12, 2) AS Hour, 
       Count(DISTINCT enduserids._experience.aaid.id) AS Visitor_Count 
FROM   {TARGET_TABLE}
WHERE TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')
GROUP BY Day, Hour
ORDER BY Hour;
```

### 特定の日に閲覧された上位 10 ページ

```sql
SELECT web.webpagedetails.name AS Page_Name, 
       Sum(web.webpagedetails.pageviews.value) AS Page_Views 
FROM   {TARGET_TABLE}
WHERE TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')
GROUP BY web.webpagedetails.name 
ORDER BY page_views DESC 
LIMIT  10;
```

### 最もアクティブな上位 10 人のユーザ

```sql
SELECT enduserids._experience.aaid.id AS aaid, 
       Count(timestamp) AS Count
FROM   {TARGET_TABLE}
WHERE TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')
GROUP BY enduserids._experience.aaid.id
ORDER BY Count DESC
LIMIT  10;
```

### 上位 10 都市（ユーザ－アクティビティ別）

```sql
SELECT concat(placeContext.geo.stateProvince, ' - ', placeContext.geo.city) AS state_city, 
       Count(timestamp) AS Count
FROM   {TARGET_TABLE}
WHERE TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')
GROUP BY state_city
ORDER BY Count DESC
LIMIT  10;
```

### 閲覧された製品の上位 10 件

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

### 上位 10 件の注文売上高

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

### イベント数（日別）

```sql
SELECT Substring(from_utc_timestamp(timestamp, 'America/New_York'), 1, 10) AS Day, 
       Substring(from_utc_timestamp(timestamp, 'America/New_York'), 12, 2) AS Hour, 
       Sum(_experience.analytics.event1to100.{TARGET_EVENT}.value) AS Event_Count
FROM   {TARGET_TABLE}
WHERE  _experience.analytics.event1to100.{TARGET_EVENT}.value IS NOT NULL 
        AND TIMESTAMP = to_timestamp('{TARGET_YEAR}-{TARGET_MONTH}-{TARGET_DAY}')
GROUP BY Day, Hour
ORDER BY Hour;
```

## 重複の除外

[!DNL Query Service] は、データの重複排除をサポートします。 詳しくは、 [でのデータ重複排除 [!DNL Query Service] ドキュメント](../best-practices/deduplication.md) を参照してください。 [!DNL Experience Event] データセット。

## マーチャンダイジング変数（製品構文）

以下の節では、 [!DNL Analytics] データセット。

### 製品の構文

Adobe Analyticsでは、マーチャンダイジング変数と呼ばれる特別に設定された変数を使用して、カスタムの製品レベルのデータを収集できます。 これらは、イベントまたはカスタムeVarに基づいています。 これらの変数とその一般的な使用法の違いは、ヒットの単一の値ではなく、ヒットの各製品の個別の値を表す点です。

これらの変数は、製品構文マーチャンダイジング変数と呼ばれます。 これにより、製品ごとの「割引額」や、顧客の検索結果での製品の「ページ上の場所」に関する情報などの情報を収集できます。

製品構文の使用方法の詳細については、 Adobe Analyticsのドキュメントを参照してください。 [製品構文を使用した eVar の実装](https://experienceleague.adobe.com/docs/analytics/implementation/vars/page-vars/evar-merchandising.html?lang=en#implement-using-product-syntax).

以下の節では、 [!DNL Analytics] データセット：

#### eVar

```console
productListItems[#]._experience.analytics.customDimensions.evars.evar#
```

- `#`:アクセスする配列のインデックス。
- `evar#`:アクセスする特定のeVar変数。

#### カスタムイベント

```console
productListItems[#]._experience.analytics.event1to100.event#.value
```

- `#`:アクセスする配列のインデックス。
- `event#`:アクセスする特定のカスタムイベント変数。

#### クエリ例

以下に、`productListItems` にある最初のクエリのマーチャンダイジング eVar とイベントを返すクエリ例を示します。

```sql
SELECT
  productListItems[0]._experience.analytics.customDimensions.evars.eVar1,
  productListItems[0]._experience.analytics.event1to100.event1.value
FROM adobe_analytics_midvalues
WHERE timestamp = to_timestamp('2019-07-23')
  AND productListItems[0].SKU IS NOT NULL
  AND productListItems[0]._experience.analytics.customDimensions.evars.eVar1 IS NOT NULL
  AND productListItems[0]._experience.analytics.event1to100.event1.value IS NOT NULL
LIMIT 10
```

次のクエリでは、 `productListItems` 配列を返します。また、製品ごとの各マーチャンダイジングeVarとイベントを返します。 元のヒットとの関係を示すために、`_id` フィールドが含まれます。この `_id` の値は、データセットの一意のプライマリキーです。

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
  WHERE TIMESTAMP = to_timestamp('2019-07-23')
  AND productListItems[0].SKU IS NOT NULL
  AND productListItems[0]._experience.analytics.customDimensions.evars.eVar1 IS NOT NULL
  AND productListItems[0]._experience.analytics.event1to100.event1.value IS NOT NULL
)
LIMIT 20
```

>[!NOTE]
>
> 現在のデータセットに存在しないフィールドを取得しようとすると、「No such struct field」エラーが発生します。 エラーメッセージに返された理由を評価して使用可能なフィールドを特定し、クエリを更新して再実行します。
>
>
```console
>ERROR: ErrorCode: 08P01 sessionId: XXXX queryId: XXXX Unknown error encountered. Reason: [No such struct field evar1 in eVar10, eVar13, eVar62, eVar88, eVar2;]
>```

### コンバージョン構文

Adobe Analyticsにある別のタイプのマーチャンダイジング変数は、コンバージョン構文です。 製品の構文を使用すると、値は製品と同時に収集されますが、そのためにはデータが同じページに存在する必要があります。 製品に関連するコンバージョンや関心のあるイベントの前に、ページでデータが発生するシナリオがあります。例えば、製品検索方法の使用例を考えてみましょう。

1. ユーザーが「冬帽子」の内部検索を実行し、コンバージョン構文が有効なマーチャンダイジング eVar6 を「内部検索:冬帽子」に設定します。
2. ユーザーが「ワッフルビーニー」をクリックし、製品の詳細ページに移動します。\
   a. ここでランディングすると、12.99 ドルの「ワッフルビーニー」の `Product View` イベントが発生します。\
   b.次以降 `Product View` がバインディングイベントとして設定されると、製品の「ワッフルビーニー」が「内部検索：冬帽子」のeVar6 値にバインドされるようになりました。 「ワッフルビーニー」製品が収集されるたびに、(1) 有効期限の設定に達するか、(2) 新しい eVar6 値が設定され、その製品で再びバインディングイベントが発生するまで、「内部検索：冬帽」に関連付けられます。
3. ユーザーが製品を買い物かごに追加し、`Cart Add` イベントが発生します。
4. ユーザーが「夏シャツ」に対して別の内部検索を実行すると、コンバージョン構文によりマーチャンダイジング eVar6 が「内部検索: 夏シャツ」に設定されます。
5. ユーザーが「スポーティーな T シャツ」をクリックし、製品の詳細ページに移動します。\
   a. ここでランディングすると、19.99 ドルの「スポーティーな T シャツ」の `Product View` イベントが発生します。\
   b. `Product View` イベントは引き続きバインディングイベントなので、製品「スポーティーな T シャツ」が「内部検索:夏シャツ」の eVar6 値に、以前の製品「ワッフルビーニー」が「内部検索:ワッフルビーニー」の eVar6 値にバインドされています。
6. ユーザーが製品を買い物かごに追加し、`Cart Add` イベントが発生します。
7. ユーザーは両方の製品をチェックアウトします。

レポートでは、注文件数、売上高、製品表示数、買い物かごへの追加数が eVar6 に対してレポートされ、連結された製品のアクティビティに合わせて調整されます。

| eVar6 （製品の検索方法） | 売上高 | 注文件数 | 製品表示 | 買い物かごへの追加 |
| ------------------------------ | ------- | ------ | ------------- | ----- |
| 内部検索：夏のシャツ | 19.99 | 1 | 1 | 1 |
| 内部検索：冬帽子 | 12.99 | 1 | 1 | 1 |

変換構文の使用方法の詳細については、 Adobe Analyticsのドキュメント ( [コンバージョン構文を使用した eVar の実装](https://experienceleague.adobe.com/docs/analytics/implementation/vars/page-vars/evar-merchandising.html?lang=en#implement-using-conversion-variable-syntax).

次に、 [!DNL Analytics] データセット：

#### eVar

```console
_experience.analytics.customDimensions.evars.evar#
```

- `evar#`:アクセスする特定のeVar変数。

#### 製品

```console
productListItems[#].sku
```

- `#`:アクセスする配列のインデックス。

#### クエリ例

次に、値を特定の製品とイベントのペア（この場合は製品表示イベント）に連結するクエリ例を示します。

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

それぞれの製品の後続のクエリに連結値を永続化するサンプルのオカレンスを示します。最も低いサブクエリは、宣言されたバインディングイベントの製品と値の関係を確立します。次のサブクエリは、それぞれの製品との後続のインタラクションにわたって、その連結値のアトリビューションを実行します。最上位レベルでは、集計を選択して結果を生成します。

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
