---
title: 分析データからマーチャンダイジング変数を返して使用する
description: Analytics データセット内のマーチャンダイジング変数にアクセスするための XDM フィールドとサンプルクエリの提供方法について説明します。
source-git-commit: cde7c99291ec34be811ecf3c85d12fad09bcc373
workflow-type: tm+mt
source-wordcount: '1112'
ht-degree: 14%

---

# 分析データからマーチャンダイジング変数を返して使用する

クエリサービスを使用して、Adobe AnalyticsからAdobe Experience Platformにデータセットとして取り込まれたデータを管理します。 以下の節では、Analytics データセット内のマーチャンダイジング変数にアクセスするために使用できるクエリ例を示します。 詳しくは、ドキュメントを参照してください。 [Adobe Analyticsデータの取得とマッピング方法](https://experienceleague.adobe.com/docs/experience-platform/sources/connectors/adobe-applications/mapping/analytics.html?lang=ja) Analytics ソースを使用

## マーチャンダイジング変数 {#merchandising-variables}

マーチャンダイジング変数は次の 2 つの構文のいずれかに従います。

* **製品の構文**:eVar値を商品に関連付けます。 
* **コンバージョン変数の構文**:バインディングイベントが発生した場合にのみeVarを製品に関連付けます。 バインディングイベントとして機能するイベントを選択できます。

## 製品の構文 {#product-syntax}

Adobe Analyticsでは、マーチャンダイジング変数と呼ばれる特別に設定された変数を使用して、カスタムの製品レベルのデータを収集できます。 これらは、イベントまたはカスタムeVarに基づいています。 これらの変数とその一般的な使用法の違いは、ヒットの単一の値ではなく、ヒットの各製品の個別の値を表す点です。

これらの変数は、製品構文マーチャンダイジング変数と呼ばれます。 これにより、製品ごとの「割引額」や、顧客の検索結果での製品の「ページ上の場所」に関する情報などの情報を収集できます。

製品構文の使用方法の詳細については、 Adobe Analyticsのドキュメントを参照してください。 [製品構文を使用した eVar の実装](https://experienceleague.adobe.com/docs/analytics/implementation/vars/page-vars/evar-merchandising.html#implement-using-product-syntax).

以下の節では、 [!DNL Analytics] データセット：

### eVar

```console
productListItems[#]._experience.analytics.customDimensions.evars.evar#
```

* `#`:アクセスする配列のインデックス。
* `evar#`:アクセスする特定のeVar変数。

### カスタムイベント

```console
productListItems[#]._experience.analytics.event1to100.event#.value
```

* `#`:アクセスする配列のインデックス。
* `event#`:アクセスする特定のカスタムイベント変数。

## 製品構文の使用例 {#product-use-cases}

以下の使用例では、 `productListItems` SQL を使用する配列。

### マーチャンダイジングeVarとイベントを返す

以下のクエリは、 `productListItems` 配列。

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

### productListItems 配列を展開して、各製品のマーチャンダイジングeVarとイベントを返します。

次のクエリでは、 `productListItems` 配列を返します。また、製品ごとの各マーチャンダイジングeVarとイベントを返します。 元のヒットとの関係を示すために、`_id` フィールドが含まれます。この `_id` の値は、データセットの一意のプライマリキーです。

>[!NOTE]
>
>explode 関数は、配列の要素を複数の行に分割します。 null 値は除外されます。

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

### コンバージョン変数の構文 {#conversion-variable-syntax}

Adobe Analyticsにある別のタイプのマーチャンダイジング変数は、コンバージョン変数の構文です。 eVar値を products 変数に設定できない場合、コンバージョン変数の構文が使用されます。 このシナリオは一般的には、ページにマーチャンダイジングチャネルや検索方法のコンテキストがない場合です。このような場合は、ユーザーが製品ページに到達する前にマーチャンダイジング変数を設定し、その値がバインディングイベントの発生まで保持されるようにする必要があります。

例えば、以下の製品の検索シナリオでは、製品に関連するコンバージョンやイベントが発生する前に、必要なデータがどのようにページに存在するかを示しています。

1. ユーザーが「冬帽子」に対して内部検索を実行し、コンバージョン構文が有効なマーチャンダイジングeVar6 を「内部検索：冬帽子」に設定します。
2. ユーザーが「ワッフルビーニー」をクリックし、製品の詳細ページに移動します。\
   a. ここでランディングすると、12.99 ドルの「ワッフルビーニー」の `Product View` イベントが発生します。\
   b.次以降 `Product View` がバインディングイベントとして設定されると、製品の「ワッフルビーニー」が「内部検索：冬帽子」のeVar6 値にバインドされるようになりました。 「ワッフルビーニー」製品が収集されるたびに、「内部検索：冬帽子」に関連付けられます。 これは、eVarの有効期限の設定に達するか、新しいeVar6 値が設定され、その製品で再びバインディングイベントが発生するまで発生します。
3. ユーザーが製品を買い物かごに追加し、`Cart Add` イベントが発生します。
4. ユーザーが「夏シャツ」に対して別の内部検索を実行すると、コンバージョン構文によりマーチャンダイジングeVar6 が「内部検索：夏シャツ」に設定されます。
5. ユーザーが「スポーティーな T シャツ」を選択し、製品の詳細ページに移動します。\
   a. ここでランディングすると、19.99 ドルの「スポーティーな T シャツ」の `Product View` イベントが発生します。\
   b.を `Product View` イベントがバインディングイベントである場合、製品の「スポーティーな T シャツ」が「内部検索：夏シャツ」のeVar6 値にバインドされるようになりました。 以前の製品の「ワッフルビーニー」は、引き続き「内部検索：ワッフルビーニー」のeVar6 値にバインドされます。
6. ユーザーが製品を買い物かごに追加し、`Cart Add` イベントが発生します。
7. ユーザーは両方の製品をチェックアウトします。

レポートでは、注文件数、売上高、製品表示数、買い物かごへの追加数がeVar6 に対してレポートされ、連結された製品のアクティビティに合わせて調整されます。

| eVar6 （製品の検索方法） | 売上高 | 注文件数 | 製品表示 | 買い物かごへの追加 |
| ------------------------------ | ------- | ------ | ------------- | ----- |
| 内部検索：夏のシャツ | 19.99 | 1 | 1 | 1 |
| 内部検索：冬帽子 | 12.99 | 1 | 1 | 1 |

コンバージョン変数の構文の使用について詳しくは、 Adobe Analyticsのドキュメントを参照してください。 [コンバージョン変数の構文を使用した eVar の実装](https://experienceleague.adobe.com/docs/analytics/implementation/vars/page-vars/evar-merchandising.html#implement-using-conversion-variable-syntax).

次に、 [!DNL Analytics] データセット：

#### eVar

```console
_experience.analytics.customDimensions.evars.evar#
```

* `evar#`:アクセスする特定のeVar変数。

#### 製品

```console
productListItems[#].sku
```

* `#`:アクセスする配列のインデックス。

## コンバージョン変数の使用例 {#conversion-variable-use-cases}

以下の使用例は、コンバージョン変数の構文が必要なシナリオを反映しています。

### 値を特定の製品とイベントのペアにバインドする

以下のクエリは、値を特定の製品とイベントのペアにバインドします。 この例では、値は製品表示イベントにバインドされます。

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

### それぞれの製品の後続の発生に連結値を保持

以下のサンプルクエリは、それぞれの製品の後続の出現に連結された値を保持します。 最も低いサブクエリは、宣言されたバインディングイベント上の製品と値の関係を確立します。 次のサブクエリは、それぞれの製品との後続のインタラクションにわたって、その連結値のアトリビューションを実行します。トップレベルの SELECT は結果を集計してレポートを生成します。

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

## 次の手順

このドキュメントでは、製品構文を使用してマーチャンダイジングeVarを返し、値をコンバージョン変数構文で特定の製品に結び付ける方法について、より深く理解する必要があります。

まだおこなっていない場合は、 [Web およびモバイルインタラクションに関する Analytics インサイトドキュメント](./analytics-insights.md) 次へ 一般的な使用例を提供し、クエリサービスを使用して Web およびモバイルAdobe Analyticsデータから実用的なインサイトを作成する方法を示します。
