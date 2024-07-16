---
title: Analytics データからのマーチャンダイジング変数の返しと使用
description: Analytics データセットのマーチャンダイジング変数にアクセスするための XDM フィールドとサンプルクエリを提供する方法を説明します。
exl-id: 1e2ae095-4152-446f-8b66-dae5512d690e
source-git-commit: 7cde32f841497edca7de0c995cc4c14501206b1a
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 12%

---

# 分析データからのマーチャンダイジング変数の返しと使用

クエリサービスを使用して、Adobe AnalyticsからAdobe Experience Platformにデータセットとして取り込まれたデータを管理します。 次の節では、Analytics データセットのマーチャンダイジング変数にアクセスするために使用できるクエリの例について説明します。 Analytics ソースを使用した [Adobe Analytics データの取り込みとマッピングの方法 ](../../sources/connectors/adobe-applications/mapping/analytics.md) について詳しくは、ドキュメントを参照してください。

## マーチャンダイジング変数 {#merchandising-variables}

マーチャンダイジング変数は次の 2 つの構文のいずれかに従います。

* **商品構文**:eVar値を商品に関連付けます。 
* **コンバージョン変数構文**：バインディングイベントの発生によってのみeVar値が商品に結び付けられます。 バインディングイベントとして機能するイベントを選択できます。

## 製品の構文 {#product-syntax}

Adobe Analyticsでは、マーチャンダイジング変数と呼ばれる特別に設定された変数を使用して、カスタムの製品レベルのデータを収集できます。 これらは、eVarまたはカスタムイベントのいずれかに基づいています。 これらの変数と一般的な使用方法の違いは、ヒットの単一の値ではなく、ヒットで見つかった各製品の個別の値を表すことです。

これらの変数は、製品構文マーチャンダイジング変数と呼ばれます。 これにより、顧客の検索結果で、製品ごとの「割引額」や製品の「ページ上の場所」に関する情報などの情報を収集できます。

製品構文の使用について詳しくは、Adobe Analyticsのドキュメント [ 製品構文を使用した eVar の実装 ](https://experienceleague.adobe.com/docs/analytics/implementation/vars/page-vars/evar-merchandising.html#implement-using-product-syntax) を参照してください。

以下の節では、[!DNL Analytics] データセットのマーチャンダイジング変数にアクセスするために必要な XDM フィールドの概要を説明します。

### eVar

```console
productListItems[#]._experience.analytics.customDimensions.evars.evar#
```

* `#`: アクセスする配列のインデックス。
* `evar#`：アクセスする特定のeVar変数。

### カスタムイベント

```console
productListItems[#]._experience.analytics.event1to100.event#.value
```

* `#`: アクセスする配列のインデックス。
* `event#`：アクセスする特定のカスタムイベント変数。

## 製品構文のユースケース {#product-use-cases}

次のユースケースでは、SQL を使用して `productListItems` 配列からマーチャンダイジングeVarを返すことに重点を置いています。

### マーチャンダイジングeVarおよびイベントを返します

次のクエリは、`productListItems` 配列で見つかった最初の商品のマーチャンダイジングeVarとイベントを返します。

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

### productListItems 配列を展開し、各商品のマーチャンダイジングeVarとイベントを返します。

次のクエリでは、`productListItems` 配列を探索し、製品ごとに各マーチャンダイジングeVarとイベントを返します。 元のヒットとの関係を示すために、`_id` フィールドが含まれます。`_id` 値は、データセットの一意のプライマリキーです。

>[!NOTE]
>
>explode 関数は、配列の要素を複数の行に分割します。 null 値を除外します。

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
> 現在のデータセットに存在しないフィールドを取得しようとすると、「そのような構造体フィールドがありません」というエラーが発生します。 エラーメッセージで返された理由を評価して使用可能なフィールドを特定し、クエリを更新して再実行します。
>
>```console
>ERROR: ErrorCode: 08P01 sessionId: XXXX queryId: XXXX Unknown error encountered. Reason: [No such struct field evar1 in eVar10, eVar13, eVar62, eVar88, eVar2;]
>```

### コンバージョン変数の構文 {#conversion-variable-syntax}

Adobe Analyticsにある別のタイプのマーチャンダイジング変数は、コンバージョン変数構文です。 コンバージョン変数の構文は、eVar値を products 変数に設定できない場合に使用されます。 このシナリオは通常、ページにマーチャンダイジングチャネルや検索方法のコンテキストがないことを意味します。 このような場合、ユーザーが製品ページに到達する前にマーチャンダイジング変数を設定する必要があり、この値はバインディングイベントが発生するまで保持されます。

例えば、以下の製品検索シナリオでは、製品に関連するコンバージョンまたはイベントが発生する前に、必要なデータをページに表示する方法を示しています。

1. 利用者は、コンバージョン構文対応マーチャンダイジングeVar6 を「internal search:winter hat」とする「winter hat」の内部検索を行う。
2. ユーザーが「ワッフルビーニー」をクリックし、製品の詳細ページに移動します。\
   a. ここでランディングすると、12.99 ドルの「ワッフルビーニー」の `Product View` イベントが発生します。\
   b. `Product View` はバインディングイベントとして設定されているので、商品「wafle beanie」は「internal search:winter hat」のeVar6 値にバインドされるようになりました。 「ワッフルビーニー」製品が収集されるたびに、「内部検索：ウィンターハット」と関連付けられます。 これは、eVarの有効期限設定に達するか、新しいeVar6 値が設定されて、その商品でバインディングイベントが再び発生するまで発生します。
3. ユーザーが製品を買い物かごに追加し、`Cart Add` イベントが発生します。
4. コンバージョン構文対応マーチャンダイジングeVar6 を「internal search:summer shirt」に設定する「summer shirt」に対して別の内部検索を行う。
5. ユーザーが「スポーティな T シャツ」を選択し、製品詳細ページに移動します。\
   a. ここでランディングすると、19.99 ドルの「スポーティーな T シャツ」の `Product View` イベントが発生します。\
   b. `Product View` イベントがバインディングイベントなので、製品「sporty t-shirt」が「internal search:summer shirt」のeVar6 値にバインドされるようになりました。 旧商品「ワッフルビーニー」は、引き続き「内部検索：ワッフルビーニー」のeVar6 値にバインドされます。
6. ユーザーが製品を買い物かごに追加し、`Cart Add` イベントが発生します。
7. ユーザーは両方の製品をチェックアウトします。

レポートでは、注文、売上高、商品表示、買い物かごへの追加がeVar6 に照らしてレポート可能であり、バインドされた商品のアクティビティに合わせられます。

| eVar6 （商品検索方法） | 売上高 | 注文件数 | 製品表示 | 買い物かごへの追加 |
| ------------------------------ | ------- | ------ | ------------- | ----- |
| 内部検索：夏のシャツ | 19.99 | 1 | 1 | 1 |
| 内部検索：冬帽子 | 12.99 | 1 | 1 | 1 |

コンバージョン変数構文の使用について詳しくは、Adobe Analyticsのドキュメント [ コンバージョン変数構文を使用した eVar の実装 ](https://experienceleague.adobe.com/docs/analytics/implementation/vars/page-vars/evar-merchandising.html#implement-using-conversion-variable-syntax) を参照してください。

[!DNL Analytics] データセットでコンバージョン変数構文を生成するための XDM フィールドを以下に示します。

#### eVar

```console
_experience.analytics.customDimensions.evars.evar#
```

* `evar#`：アクセスする特定のeVar変数。

#### 製品

```console
productListItems[#].sku
```

* `#`: アクセスする配列のインデックス。

## コンバージョン変数のユースケース {#conversion-variable-use-cases}

以下のユースケースは、コンバージョン変数の構文が必要なシナリオを反映しています。

### 特定の製品とイベントのペアに値をバインドする

以下のクエリは、値を特定の製品とイベントのペアにバインドします。 この例では、値が製品表示イベントにバインドされています。

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

### 連結値を各製品の後続の発生に保持します

以下のサンプルクエリでは、バインドされた値をそれぞれの製品の後続の発生に保持します。 最小サブクエリは、宣言されたバインディングイベントで値と製品の関係を確立します。 次のサブクエリは、それぞれの製品との後続のインタラクションにわたって、その連結値のアトリビューションを実行します。最上位の SELECT では、結果を集計してレポートを生成します。

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

このドキュメントでは、product 構文を使用してマーチャンダイジングeVarを返し、コンバージョン変数の構文を使用して値を特定の商品にバインドする方法について、より深く理解する必要があります。

まだ行っていない場合は、次の [Web およびモバイルインタラクションに関する分析インサイトのドキュメント ](./analytics-insights.md) を参照してください。 これは一般的なユースケースを示し、クエリサービスを使用して web およびモバイルのAdobe Analytics データから実用的なインサイトを作成する方法を示しています。
