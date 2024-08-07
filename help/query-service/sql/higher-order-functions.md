---
title: 高階関数を使用した配列およびマップ データ タイプの管理
description: クエリサービスの高階関数を使用して、配列とマップのデータタイプを管理する方法を説明します。 一般的なユースケースの例を示しています。
exl-id: dec4e4f6-ad6b-4482-ae8c-f10cc939a634
source-git-commit: d2bc580ba1cacdfab45bdc6356c630a63e7d0f6e
workflow-type: tm+mt
source-wordcount: '1470'
ht-degree: 1%

---

# 高階関数を使用した配列およびマップ データ タイプの管理

このガイドでは、高階関数を使用して、配列やマップなどの複雑なデータ型を処理する方法を説明します。 これらの関数は、配列を展開し、関数を実行してから、結果を組み合わせる必要がなくなります。 高階関数は、複雑なネストされた構造、配列、マップおよび様々なユースケースを特徴とすることが多い時系列データセットおよび分析を分析または処理する場合に特に役立ちます。

次のユースケースには、高階配列およびマップ操作関数の例が含まれています。

## トランスフォームを使用して価格合計を n で調整 {#adjust-price-total}

`transform(array<T>, function<T, U>): array<U>`

上記のスニペットは、配列の各要素に関数を適用し、変換された要素の新しい配列を返します。 具体的には、`transform` 関数は T 型の配列を取り、各要素を T 型から U 型に変換します。次に、U 型の配列を返します。実際の型 T と U は、変換関数の具体的な使用方法によって異なります。

`transform(array<T>, function<T, Int, U>): array<U>`

この配列変換関数は前の例と似ていますが、関数には 2 つの引数があります。 この関数の 2 番目の引数は、変換される以外に、配列内の要素のインデックスも受け取ります。

**例**

次の SQL の例は、このユースケースを示しています。 クエリは、指定されたテーブルから限られた行のセットを取得し、各項目の `priceTotal` 属性に 73 を掛けて`productListItems`配列を変換します。結果には、 `_id`列、 `productListItems`列および変換された `price_in_inr` 列が含まれます。 選択は、特定のタイムスタンプ範囲に基づいています。

```sql
SELECT _id,
       productListItems,
       Transform(productListItems, value -> value.priceTotal * 73) AS
       price_in_inr
FROM   geometrixxx_999_xdm_pqs_1batch_10k_rows
WHERE  timestamp > To_timestamp('2017-11-01 00:00:00')
       AND timestamp < To_timestamp('2017-11-02 00:00:00')
LIMIT  10;
```

**結果**

この SQL の結果は、以下のようになります。

```console
 productListItems | price_in_inr
-------------------+----------------
(8376, NULL, NULL) | 611448.0
{(Burbank Hills Jeans, NULL, NULL), (Thermomax Steel, NULL, NULL), (Bruin Point Shearling Boots, NULL, NULL), (Uintas Pro Ski Gloves, NULL, NULL), (Timberline Survival Knife, NULL, NULL), (Thermomax Steel, NULL, NULL), (Timpanogos Scarf, NULL, NULL), (Lost Prospector Beanie, NULL, NULL), (Timpanogos Scarf, NULL, NULL), (Uintas Pro Ski Gloves, NULL, NULL)} | {0.0,0.0.0.0,0,0,0,0,0,0,0,0,0,0,0,0,0.0}
(84763,NULL, NULL) | 6187699.0
(843765, NULL, NULL) | 6.1594845E7
(199684, NULL, NULL) | 1.4576932E7

(10 rows)
```

## 特定の SKU を持つ製品が存在するかどうかを検出するには、既存を使用します {#confirm-product-exists}

`exists(array<T>, function<T, boolean>): boolean`

上記のスニペットでは、`exists` 関数が配列の各要素に適用され、ブール値を返します。 ブール値は、指定した条件を満たす 1 つ以上の要素が配列にあるかどうかを示します。 この場合、特定の SKU を持つ製品が存在するかどうかを確認します。

**例**

以下の SQL の例では、クエリは `geometrixxx_999_xdm_pqs_1batch_10k_rows` テーブルから `productListItems` を取得し、`productListItems` 配列に `123679` と等しい SKU を持つ要素が存在するかどうかを評価します。 次に、特定のタイムスタンプレンジに基づいて結果をフィルタリングし、最終結果を 10 行に制限します。

```sql
SELECT productListItems
FROM geometrixxx_999_xdm_pqs_1batch_10k_rows
WHERE EXISTS( productListItems, value -> value.sku == 123679)
AND timestamp > to_timestamp('2017-11-01 00:00:00')
AND timestamp < to_timestamp('2017-11-02 00:00:00')limit 10;
```

**結果**

この SQL の結果は、次に示すような結果になります。

```console
productListItems
-----------------
{(123679, NULL,NULL)}
{(123679, NULL, NULL)}
{(123679, NULL, NULL), (150196, NULL, NULL)}
{(123679, NULL, NULL), (150196, NULL, NULL)}
{(123679, NULL, NULL), (150196, NULL, NULL)}
{(123679, NULL, NULL)}
{(123679, NULL, NULL)}
{(123679, NULL, NULL)}
{(123679, NULL,NULL)}
{(123679,NULL, NULL)}

(10 rows)
```

## フィルターを使用して SKU/100000 の商品を検索する {#find-specific-products}

`filter(array<T>, function<T, boolean>): array<T>`

この関数は、各要素をブール値として評価する指定の条件に基づいて、要素の配列をフィルタリングします。 次に、条件が true 値を返した要素のみを含む新しい配列を返します。

**例**

次のクエリでは、`productListItems` 列を選択し、SKU が 100000 より大きい要素のみを含めるフィルターを適用し、特定のタイムスタンプ範囲内の行に結果セットを制限します。 フィルタリングされた配列は、出力の `_filter` としてエイリアス化されます。

```sql
SELECT productListItems,
    Filter(productListItems, value -> value.sku > 100000) AS _filter
FROM geometrixxx_999_xdm_pqs_1batch_10k_rows
WHERE timestamp > To_timestamp('2017-11-01 00:00:00')
AND timestamp < To_timestamp('2017-11-02 00:00:00')
LIMIT 10; 
```

**結果**

この SQL の結果は、次に示すような結果になります。

```console
productListItems | _filter
-----------------+---------
(123679, NULL, NULL) (123679, NULL, NULL)
(1346, NULL, NULL) |
(98347, NULL, NULL) |
(176015, NULL, NULL) | (176015, NULL, NULL)

(10 rows)
```

## 集計を使用すると、特定の ID に関連付けられたすべての製品リスト項目の SKU を合計し、結果の合計を 2 倍にすることができます {#sum-specific-skus-and-double-the-resulting-total}

`aggregate(array<T>, A, function<A, T, A>[, function<A, R>]): R`

この集計操作では、初期状態と配列内のすべての要素にバイナリ演算子が適用されます。 また、複数の値を 1 つの状態に減らします。 この縮小後、最終状態は仕上げ関数を使用して最終結果に変換されます。 終了関数は、すべての配列要素にバイナリ演算子を適用した後に取得された最後の状態を取得し、それを使用して最終結果を生成します。

**例**

この例ではクエリ指定されたタイムスタンプ範囲内の `productListItems` 配列から最大SKU値を計算し、結果を 2 倍にします。 出力には元の `productListItems` 配列と計算された `max_value`が含まれます。

```sql
SELECT productListItems,
aggregate(productListItems, 0, (acc, value) ->
case
WHEN (
value.sku > acc) THEN cast(value.sku AS int)
ELSE cast(acc AS int)
END, acc -> acc * 2) AS max_value
FROM geometrixxx_999_xdm_pqs_1batch_10k_rows
WHERE timestamp > to_timestamp('2017-11-01 00:00:00')
AND timestamp < to_timestamp('2017-11-02 00:00:00')
LIMIT 50;
```

**結果**

この SQL の結果は、次に示すような結果になります。

```console
productListItems | max_value
-----------------+---------
(123679, NULL, NULL) | 247358
(1346,NULL, NULL) | 2692
(98347, NULL, NULL) | 196694
(176015, NULL, NULL) | 352030

(10 rows)
```

## zip_with を使用して、製品リスト内のすべての項目にシーケンス番号を割り当てます {#assign-a-sequence-number}

`zip_with(array<T>, array<U>, function<T, U, R>): array<R>`

このスニペットは、2 つの配列の要素を 1 つの新しい配列に組み合わせます。 操作は、配列の各要素で独立して実行され、値のペアを生成します。 1 つの配列が短い場合、長い配列の長さに一致するように null 値が追加されます。 これは、関数が適用される前に行われます。

**例**

次のクエリでは、`zip_with` 関数を使用して、2 つの配列から値のペアを作成します。 これを行うには、`Sequence` 関数を使用して生成された整数シーケンスに `productListItems` 配列の SKU 値を追加します。 結果は、元の `productListItems` 列と共に選択され、タイムスタンプの範囲に基づいて制限されます。

```sql
SELECT productListItems,
zip_with(Sequence(1,5), Transform(productListItems, p -> p.sku), (x,y) -> struct(x, y)) AS zip_with
FROM geometrixxx_999_xdm_pqs_1batch_10k_rows
WHERE timestamp > to_timestamp('2017-11-01 00:00:00')
AND timestamp < to_timestamp('2017-11-02 00:00:00')
limit 10;
```

**結果**

この SQL の結果は、次に示すような結果になります。

```console
productListItems     | zip_with
---------------------+---------
                     | {(1,NULL), (2,NULL), (3,NULL),(4,NULL), (5,NULL)}
(123679, NULL, NULL) | {(1,123679), (2,NULL), (3,NULL), (4,NULL), (5,NULL)}
                     | {(1,NULL), (2,NULL),(3,NULL),(4,NULL), (5,NULL)}
                     | {(1,NULL), (2,NULL), (3, NULL),(4,NULL), (5,NULL)}
(1346,NULL, NULL)    | {(1,1346), (2,NULL),(3,NULL),(4,NULL), (5,NULL)}
                     | {(1,NULL), (2,NULL), (3,NULL),(4,NULL), (5,NULL)}
(98347, NULL, NULL)  | {(1,98347), (2,NULL), (3,NULL), (4,NULL), (5,NULL)}
                     | {(1,NULL), (2,NULL), (3,NULL), (4,NULL), (5,NULL)}
(176015, NULL, NULL) | {(1,176015),(2,NULL), (3,NULL), (4,NULL), (5,NULL)}
                     | {(1,NULL), (2,NULL), (3,NULL), (4,NULL), (5,NULL)}

(10 rows)
```

## 製品リスト内の各項目にシーケンス番号を割り当て、最終的な結果をマップとして取得するには、map_from_entries を使用します {#assign-a-sequence-number-return-result-as-map}

`map_from_entries(array<struct<K, V>>): map<K, V>`

このスニペットは、キーと値のペアの配列をマップに変換します。 これは、より整理され効率的な構造のメリットが得られるキーと値のペアのデータを処理する際に役立ちます。

**例**

次のクエリでは、シーケンスと productListItems 配列から値のペアを作成し、map_from_entriesを使用してこれらのペアをマップに変換してから、新しく作成された map_from_entries 列と共に元の productListItems 列を選択します。 結果はフィルタリングされ、指定されたタイムスタンプ範囲に基づいて制限されます。

```sql
SELECT productListItems,      map_from_entries(zip_with(Sequence(1,Size(productListItems)), productListItems, (x,y) -> struct(x, y))) AS map_from_entries
FROM   geometrixxx_999_xdm_pqs_1batch_10k_rows
WHERE  timestamp > to_timestamp('2017-11-01 00:00:00')
AND    timestamp < to_timestamp('2017-11-02 00:00:00')
LIMIT 10;
```

**結果**

この SQL の結果は、以下のようになります。

```console
productListItems     | map_from_entries
---------------------+------------------
(123679, NULL, NULL) | [1 -> "(123679,NULL,NULL)"]
(1346, NULL, NULL)   | [1 -> "(1346, NULL, NULL)"]
(98347, NULL, NULL)  | [1 -> "(98347, NULL, NULL)"]
(176015, NULL, NULL) | [1 -> "(176015, NULL, NULL)"]
(92763, NULL, NULL)  | [1 -> "(92763, NULL, NULL)"] 
(48576, NULL, NULL)  | [1 -> "(48576, NULL, NULL)"] 
(135778, NULL, NULL) | [1 -> "(135778, NULL, NULL)"] 
(123679, NULL, NULL) | [1 -> "(123679, NULL, NULL)"] 
(98347, NULL, NULL)  | [1 -> "(98347, NULL, NULL)"]
(167753, NULL, NULL) | [1 -> "(167753, NULL, NULL)"] 

(10 rows)
```

## 製品リスト内の項目にシーケンス番号を割り当て、結果をマップとして返すには、map_form_arrays を使用します {#assign-sequence-numbers-to-items-return-the-result-as-a-map}

`map_form_arrays(array<K>, array<V>): map<K, V>`

`map_form_arrays` 関数は、2 つの配列からペアになった値を使用してマップを作成します。

>[!IMPORTANT]
>
>キーには null 要素を含めないでください。

**例**

次の SQL は、`Sequence` 関数を使用して生成されたキーがシーケンス番号であり、値が `productListItems` 配列の要素であるマップを作成します。 クエリは `productListItems` 列を選択し、`Map_from_arrays` 関数を使用して、生成された一連の数値と配列の要素に基づいてマップを作成します。 結果は 10 行に制限され、タイムスタンプの範囲に基づいてフィルタリングされます。

```sql
SELECT productListItems,
       Map_from_arrays(Sequence(1, Size(productListItems)), productListItems) AS
       map_from_arrays
FROM   geometrixxx_999_xdm_pqs_1batch_10k_rows
WHERE  Size(productListItems) > 0
       AND timestamp > To_timestamp('2017-11-01 00:00:00')
       AND timestamp < To_timestamp('2017-11-02 00:00:00')
LIMIT  10;
```

**結果**

この SQL の結果は、次に示すような結果になります。

```console
productListItems     | map_from_entries
---------------------+------------------
(123679, NULL, NULL) | [1 -> "(123679,NULL,NULL)"]
(1346, NULL, NULL)   | [1 -> "(1346, NULL, NULL)"]
(98347, NULL, NULL)  | [1 -> "(98347, NULL, NULL)"]
(176015, NULL, NULL) | [1 -> "(176015, NULL, NULL)"]
(92763, NULL, NULL)  | [1 -> "(92763, NULL, NULL)"] 
(48576, NULL, NULL)  | [1 -> "(48576, NULL, NULL)"] 
(135778, NULL, NULL) | [1 -> "(135778, NULL, NULL)"] 
(123679, NULL, NULL) | [1 -> "(123679, NULL, NULL)"] 
(98347, NULL, NULL)  | [1 -> "(98347, NULL, NULL)"]
(167753, NULL, NULL) | [1 -> "(167753, NULL, NULL)"] 

(10 rows)
```

## 2 つのマップを単一のマップとして連結するには、map_concat を使用します {#concatenate-two-maps-into-as-single-map}

`map_concat(map<K, V>, ...): map<K, V>`

上記のスニペットの `map_concat` 関数は、複数のマップを引数として受け取り、入力マップからすべてのキーと値のペアを組み合わせた新しいマップを返します。 この関数は、複数のマップを 1 つのマップに連結し、結果のマップには、入力マップからすべてのキーと値のペアが含まれます。

**例**

次の SQL は、`productListItems` 内の各項目がシーケンス番号に関連付けられたマップを作成し、次に、特定のシーケンス範囲でキーが生成される別のマップと連結されます。

```sql
SELECT productListItems,
      map_concat(           
         map_from_entries(zip_with(Sequence(1,Size(productListItems)), productListItems, (x,y) -> struct(x, y))),
         map_from_arrays(sequence(size(productListItems) + 1, size(productListItems) + size(productListItems)), productListItems) )
FROM geometrixxx_999_xdm_pqs_1batch_10k_rows
WHERE size(productListItems) > 0
AND timestamp > to_timestamp('2017-11-01 00:00:00')
AND timestamp < to_timestamp('2017-11-02 00:00:00')
limit 10;
```

**結果**

この SQL の結果は、次に示すような結果になります。

```console
productListItems     | map_from_entries
---------------------+------------------
(123679, NULL, NULL) | [1 -> "(123679,NULL,NULL)",2 -> "(123679, NULL, NULL)"]
(1346, NULL, NULL)   | [1 -> "(1346, NULL, NULL)",2 -> "(1346, NULL, NULL)"]
(98347, NULL, NULL)  | [1 -> "(98347, NULL, NULL)",2 -> "(98347, NULL, NULL)"]
(176015, NULL, NULL) | [1 -> "(176015, NULL, NULL)",2 -> "(176015, NULL, NULL)"]
(92763, NULL, NULL)  | [1 -> "(92763, NULL, NULL)",2 -> "(92763, NULL, NULL)"] 
(48576, NULL, NULL)  | [1 -> "(48576, NULL, NULL)",2 -> "(48576, NULL, NULL)"] 
(135778, NULL, NULL) | [1 -> "(135778, NULL, NULL)",2 -> "(135778, NULL, NULL)"] 
(123679, NULL, NULL) | [1 -> "(123679, NULL, NULL)",2 -> "(123679, NULL, NULL)"] 
(98347, NULL, NULL)  | [1 -> "(98347, NULL, NULL)",2 -> "(98347, NULL, NULL)"]
(167753, NULL, NULL) | [1 -> "(167753, NULL, NULL)",2 -> "(167753, NULL, NULL)"] 

(10 rows)
```

## element_at を使用して、さらに計算するために ID マップで「AAID」に対応する値を取得します {#retrieve-a-corresponding-value}

`element_at(array<T>, Int): T / element_at(map<K, V>, K): V`

配列の場合、スニペットは、指定された（1 ベースの）インデックスの要素、またはマップ内のキーに関連付けられた値を返します。 インデックスが &lt;0 の場合、最後から最初の要素にアクセスし、インデックスが配列の長さを超えると null を返します。

マップの場合、指定されたキーの値を返すか、キーがマップに含まれていない場合は null を返します。

**例**

クエリは、テーブル`geometrixxx_999_xdm_pqs_1batch_10k_rows`から`identitymap`列を選択し、各行のキー`AAID`に関連付けられた値を抽出します。結果は指定されたタイム・スタンプ範囲内の行に制限され、クエリは出力を 10 行に制限します。

```sql
SELECT identitymap,
              Element_at(identitymap, 'AAID')
FROM geometrixxx_999_xdm_pqs_1batch_10k_rows
WHERE timestamp > To_timestamp('2017-11-01 00:00:00')
AND timestamp < To_timestamp('2017-11-02 00:00:00')
LIMIT 10; 
```

**結果**

この SQL の結果は、以下のようになります。

```console
                                                                  identitymap                                            |  element_at(identitymap, AAID) 
-------------------------------------------------------------------------------------------------------------------------+-------------------------------------
[AAID -> "(3617FBB942466D79-5433F727AD6A0AD, false)",ECID -> "(67383754798169392543508586197135045866,true)"]            | (3617FBB942466D79-5433F727AD6A0AD, false) 
[AAID -> "[AAID -> "(533F56A682C059B1-396437F68879F61D, false)",ECID -> "(91989370462250197735311833131353001213,true)"] | (533F56A682C059B1-396437F68879F61D, false) 
[AAID -> "(22E195F8A8ECCC6A-A39615C93B72A9F, false)",ECID -> "(57699241367342030964647681192998909474,true)"]            | (22E195F8A8ECCC6A-A39615C93B72A9F, false) 
[AAID -> "(6A60527B9D66CCB9-29638A632B45E9, false)",ECID -> "(50117234882064422833184021414056250576,true)"]             | (6A60527B9D66CCB9-29638A632B45E9, false) 
[AAID -> "(64FB4DC317E21B59-2A23602D234647E7, false)",ECID -> "(79785479785408621882908938960039330887,true)"]           | (64FB4DC317E21B59-2A23602D234647E7, false) 
[AAID -> "(2E70E8CF6DB1DE86-270E55BBBA58B9C1, false)",ECID -> "(80073674009951685326146914344189474476,true)"]           | (2E70E8CF6DB1DE86-270E55BBBA58B9C1, false) 
[AAID -> "(22E195F8A8ECCC6A-A39615C93B72A9F, false)",ECID -> "(57699241367342030964647681192998909474,true)"]            | (22E195F8A8ECCC6A-A39615C93B72A9F, false) 
[AAID -> "(1CFB3297C3146F2F-28D6902A610BA3B1, false)",ECID -> "(88251082790399360979074868101758236669,true)"]           | (1CFB3297C3146F2F-28D6902A610BA3B1, false) 
[AAID -> "(533F56A682C059B1-396437F68879F61D, false)",ECID -> "(91989370462250197735311833131353001213,true)"]           | (533F56A682C059B1-396437F68879F61D, false) 
(10 rows)
```

## カーディナリティを使用して、ID マップ内の ID の数を調べます {#find-the-number-of-identities-in-the-identity-map}

`cardinality(array<T>): Int / cardinality(map<K, V>): Int`

このスニペットは、指定された配列またはマップのサイズを返し、エイリアスを提供します。 値が null の場合は–1 を返します。

**例**

次のクエリは、`identitymap` 列を取得し、`Cardinality` 関数は、`identitymap` 内の各マップ内の要素数を計算します。 結果は 10 行に制限され、指定したタイムスタンプ範囲に基づいてフィルタリングされます。

```sql
SELECT identitymap,
       Cardinality(identitymap)
FROM   geometrixxx_999_xdm_pqs_1batch_10k_rows
WHERE timestamp > To_timestamp('2017-11-01 00:00:00')
AND timestamp < To_timestamp('2017-11-02 00:00:00')
LIMIT  10;
```

**結果**

この SQL の結果は、次に示すような結果になります。

```console
                                                                  identitymap                                            |  size(identitymap) 
-------------------------------------------------------------------------------------------------------------------------+-------------------------------------
[AAID -> "(3617FBB942466D79-5433F727AD6A0AD, false)",ECID -> "(67383754798169392543508586197135045866,true)"]            |      2  
[AAID -> "[AAID -> "(533F56A682C059B1-396437F68879F61D, false)",ECID -> "(91989370462250197735311833131353001213,true)"] |      2  
[AAID -> "(22E195F8A8ECCC6A-A39615C93B72A9F, false)",ECID -> "(57699241367342030964647681192998909474,true)"]            |      2  
[AAID -> "(6A60527B9D66CCB9-29638A632B45E9, false)",ECID -> "(50117234882064422833184021414056250576,true)"]             |      2  
[AAID -> "(64FB4DC317E21B59-2A23602D234647E7, false)",ECID -> "(79785479785408621882908938960039330887,true)"]           |      2  
[AAID -> "(2E70E8CF6DB1DE86-270E55BBBA58B9C1, false)",ECID -> "(80073674009951685326146914344189474476,true)"]           |      2  
[AAID -> "(22E195F8A8ECCC6A-A39615C93B72A9F, false)",ECID -> "(57699241367342030964647681192998909474,true)"]            |      2  
[AAID -> "(1CFB3297C3146F2F-28D6902A610BA3B1, false)",ECID -> "(88251082790399360979074868101758236669,true)"]           |      2  
[AAID -> "(533F56A682C059B1-396437F68879F61D, false)",ECID -> "(91989370462250197735311833131353001213,true)"]           |      2  
(10 rows)
```

## productListItems 内のユニーク要素を検索するには、array_distinct を使用します {#find-distinct-elements}

`array_distinct(array<T>): array<T>`

上記のスニペットは、指定された配列から重複した値を削除します。

**例**

次のクエリでは、`productListItems` 列を選択し、配列から重複項目を削除し、指定したタイムスタンプ範囲に基づいて 10 行に出力を制限します。

```sql
SELECT productListItems,
              Array_distinct(productListItems)
FROM geometrixxx_999_xdm_pqs_1batch_10k_rows
WHERE timestamp > To_timestamp('2017-11-01 00:00:00')
AND timestamp < To_timestamp('2017-11-02 00:00:00')
LIMIT 10;
```

**結果**

この SQL の結果は、次に示すような結果になります。

```console
productListItems     | array_distinct(productListItems)
---------------------+---------------------------------
                     |
(123679, NULL, NULL) | (123679, NULL, NULL)
                     |
                     |
(1346,NULL, NULL)    | (1346,NULL, NULL)
                     |
(98347, NULL, NULL)  | (98347, NULL, NULL)
                     |
(176015, NULL, NULL) | (176015, NULL, NULL)
                     |

(10 rows)
```

### その他の高階関数 {#additional-higher-order-functions}

類似レコードの取得のユースケースの一部として、高階関数の次の例を説明します。 各関数の使用例と使用方法の説明は、このドキュメントの各セクションに記載されています。

[`transform` 関数の例では ](../use-cases/retrieve-similar-records.md#length-adjustment) 製品リストのトークン化について説明しています。

[`filter` 関数の例では ](../use-cases/retrieve-similar-records.md#filter-results) テキストデータから関連情報をより詳細かつ正確に抽出する方法を示しています。

[`reduce`機能](../use-cases/retrieve-similar-records.md#higher-order-function-solutions)は、さまざまな分析および計画プロセスで極めて重要な累積値または集計を導出する方法を提供します。
