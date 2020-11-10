---
keywords: Experience Platform;home;popular topics;query service;Query service;spark sql;Spark sql;spark;spark sql functions;functions;
solution: Experience Platform
title: Spark SQL 関数
topic: spark sql functions
description: このドキュメントには、SQL機能を拡張する組み込みのSpark SQL関数を提供するSpark SQLヘルパーに関する情報が含まれています。
translation-type: tm+mt
source-git-commit: d0fa57effb45fad6934345323366ef45383bed01
workflow-type: tm+mt
source-wordcount: '5009'
ht-degree: 97%

---


# [!DNL Spark] SQL関数

The [!DNL Spark] SQL helpers provide built-in [!DNL Spark] SQL functions to extend SQL functionality.

参照：[Spark SQL 関数のドキュメント](https://spark.apache.org/docs/2.4.0/api/sql/index.html)

>[!NOTE]
>
> 外部ドキュメント内のすべての関数がサポートされているわけではありません。

## カテゴリ

- [数学および統計の演算子と関数](#math)
- [論理演算子](#logical-operators)
- [日付／時間関数](#datetime-functions)
- [集計関数](#aggregate-functions)
- [配列](#arrays)
- [データタイプキャスト関数](#datatype-casting)
- [変換関数と書式設定関数](#conversion)
- [データ評価](#data-evaluation)
- [現在の情報](#current-information)
- [上位関数](#higher-order)

### 数学および統計の演算子と関数 {#math}

#### 剰余

`expr1 % expr2`：`expr1`/`expr2`の後の剰余を戻します。

例：

```sql
> SELECT 2 % 1.8;
 0.2
> SELECT MOD(2, 1.8);
 0.2
```

#### 乗算

`expr1 * expr2`: 戻り値 `expr1`*`expr2`.

例：

```sql
> SELECT 2 * 3;
 6
```

#### Add

`expr1 + expr2`: 戻り値 `expr1`+`expr2`.

例：

```sql
> SELECT 1 + 2;
 3
```

#### 減算

`expr1 - expr2`: 戻り値 `expr1`-`expr2`.

例：

```sql
> SELECT 2 - 1;
 1
```

#### 除算

`expr1 / expr2`: 戻り値 `expr1`/`expr2`.常に浮動小数点の除算を実行します。

例：

```sql
> SELECT 3 / 2;
 1.5
> SELECT 2L / 2L;
 1.0
```

#### abs

`abs(expr)`：数値の絶対値を戻します。

例：

```sql
> SELECT abs(-1);
  1
```

#### acos

`acos(expr)`：`expr` の逆余弦（アークコサイン）を戻します。この値は、`java.lang.Math.acos` で計算されたものと同じです。

例：

```sql
> SELECT acos(1);
 0.0
> SELECT acos(2);
 NaN
```

#### approx_percentile

`approx_percentile(col, percentage [, accuracy])`：指定された割合での数値列 `col` の近似百分位値を戻します。割合値は、0.0 と 1.0 の間である必要があります。`accuracy` パラメーター（デフォルト：10000）は正の数値リテラルで、メモリーのコストと引き換えに近似精度を制御します。`accuracy` の値を大きくすると、精度が向上し、`1.0/accuracy` は近似の相対誤差です。`percentage` が配列の場合、割合配列の各値は 0.0 と 1.0 の間である必要があります。この場合、指定された割合配列における列 `col` の近似百分位配列を戻します。

例：

```sql
> SELECT approx_percentile(10.0, array(0.5, 0.4, 0.1), 100);
 [10.0,10.0,10.0]
> SELECT approx_percentile(10.0, 0.5, 100);
 10.0
```

#### asin

`asin(expr)`：`expr` の逆正弦（アークサイン）を戻します。この値は、`java.lang.Math.asin` で計算されたものと同じです。

例：

```sql
> SELECT asin(0);
 0.0
> SELECT asin(2);
 NaN
```

#### atan

`atan(expr)`：`expr` の逆正接（アークタンジェント）を戻します。この値は、`java.lang.Math.atan` で計算されたものと同じです。

例：

```sql
> SELECT atan(0);
 0.0
```

#### atan2

`atan2(exprY, exprX)`：平面の正の x 軸と座標 (`exprX`,`exprY`) で指定された点との間の角度をラジアンで戻します。この値は、`java.lang.Math.atan2` で計算されたものと同じです。

引数：

`exprY`：Y 軸の座標`exprX`：X 軸の座標

例：

```sql
> SELECT atan2(0, 0);
 0.0
```

#### avg

`avg(expr)`：グループの値から計算された平均値を戻します。

#### cardinality

`cardinality(expr)`：配列またはマップのサイズを戻します。入力が null で、`spark.sql.legacy.sizeOfNull` が true（デフォルト）に設定されている場合、この関数は -1 を戻します。`spark.sql.legacy.sizeOfNull` を false に設定すると、null 入力の場合は null を戻します。

例：

```sql
> SELECT cardinality(array('b', 'd', 'c', 'a'));
 4
> SELECT cardinality(map('a', 1, 'b', 2));
 2
> SELECT cardinality(NULL);
 -1
```

#### cbrt

`cbrt(expr)`：`expr` の立方根を戻します。

例：

```sql
> Select cbrt(27.0);
 3.0
```

#### ceil

`ceil(expr)`：`expr` 以上の最小の整数を戻します。

例：

```sql
> SELECT ceil(-0.1);
 0
> SELECT ceil(5);
 5
```

#### ceiling

`ceiling(expr)`：`expr` 以上の最小の整数を戻します。

例：

```sql
> SELECT ceiling(-0.1);
 0
> SELECT ceiling(5);
 5
```

#### conv

`conv(num, from_base, to_base)`：`num` を `from_base` から `to_base` に変換します

例：

```sql
> SELECT conv('100', 2, 10);
 4
> SELECT conv(-10, 16, -10);
 -16
```

#### corr

`corr(expr1, expr2)`：一連の数の対の間のピアソン相関係数を戻します。

#### cos

`cos(expr)`：`expr` の正弦（コサイン）を戻します。この値は、`java.lang.Math.cos` で計算されたものと同じです。

例：

```sql
> SELECT cos(0);
 1.0
```

#### cosh

`cosh(expr)`：`expr` の双曲線余弦（ハイパボリックコサイン）を戻します。この値は、`java.lang.Math.cosh` で計算されたものと同じです。

引数：
- `expr`：双曲角

例：

```
> SELECT cosh(0);
 1.0
```

#### cot

`cot(expr)`：`expr`の余接（コタンジェント）を戻します。この値は、`1/java.lang.Math.cot` で計算されたものと同じです。

引数：
- `expr`：角度（ラジアン単位）

例：

```sql
> SELECT cot(1);
 0.6420926159343306
```

#### dense_rank

`dense_rank()`：値のグループ内の値のランクを計算します。結果は、以前に割り当てられたランク値に 1 を足した値になります。`rank` 関数とは異なり、`dense_rank` はランキング順序にギャップを生成しません。

#### e

`e()`：オイラー数 e を戻します。

例：

```sql
> SELECT e();
 2.718281828459045
```

#### exp

`exp(expr)`：e の `expr` 乗を戻します。

例：

```sql
> SELECT exp(0);
 1.0
```

#### expml

`expm1(expr)`：exp(`expr`) - 1 を戻します。

例：

```sql
> SELECT expm1(0);
 0.0
```

#### factorial

`factorial(expr)`：`expr` の階乗を戻します。`expr` は [0..20] です。それ以外の場合は null です。

例：

```
> SELECT factorial(5);
 120
```

#### floor

`floor(expr)`：`expr` 以下の最大の整数を戻します。

例：

```sql
> SELECT floor(-0.1);
 -1
> SELECT floor(5);
 5
```

#### greatest

`greatest(expr, ...)`：すべてのパラメーターの最大値を戻します（null 値をスキップします）。

例：

```sql
> SELECT greatest(10, 9, 2, 4, 3);
 10
```

#### hypot

`hypot(expr1, expr2)`：sqrt(`expr1`<sup>2</sup> + `expr2`<sup>2</sup>) を戻します。

例：

```sql
> SELECT hypot(3, 4);
 5.0
```

#### kurtosis

`kurtosis(expr)`：グループの値から計算された尖度の値を戻します。


#### least

`least(expr, ...)`：すべてのパラメーターの最小値を戻します（null 値をスキップします）。

例：

```sql
> SELECT least(10, 9, 2, 4, 3);
 2
```

#### levenshtein

`levenshtein(str1, str2)`：2 つの指定した文字列の間のレーベンシュタイン距離を戻します。

例：

```sql
> SELECT levenshtein('kitten', 'sitting');
 3
```

#### ln

`ln(expr)`：`expr` の自然対数（底 e）を戻します。

例：

```sql
> SELECT ln(1);
 0.0
```

#### log

`log(base, expr)`：`base` を底とする `expr` の対数を戻します。

例：

```sql
> SELECT log(10, 100);
 2.0
```

#### log10

`log10(expr)`：10 を底とする `expr` の対数を戻します。

例：

```sql
> SELECT log10(10);
 1.0
```

#### log1p

`log1p(expr)`: 戻り値 `log(1 + expr)`.

例：

```sql
> SELECT log1p(0);
 0.0
```

#### log2

`log2(expr)`：2 を底とする `expr` の対数を戻します。

例：

```sql
> SELECT log2(2);
 1.0
```

#### max

`max(expr)`：`expr` の最大値を戻します。

#### mean

`mean(expr)`：グループの値から計算された平均値を戻します。

#### min

`min(expr)`：`expr` の最小値を戻します。

#### monotonically_increasing_id

`monotonically_increasing_id()`：単調に増加する 64 ビット整数を戻します。生成される ID は、単調に増加し、一意であることが保証されますが、連続していることは保証されません。現在の実装では、パーティション ID が上位 31 ビットに配置され、下位 33 ビットは各パーティション内のレコード番号を表します。データフレームのパーティション数が 10 億未満で、各パーティションのレコード数が 80 億未満であると仮定します。結果がパーティション ID に依存するので、この関数は非決定的です。

#### negative

`negative(expr)`：`expr` の負の値を戻します。

例：

```sql
> SELECT negative(1);
 -1
```

#### percent_rank

`percent_rank()`：値のグループ内の値のパーセントのランクを計算します。

#### percentile

`percentile(col, percentage [, frequency])`：指定した割合での数値列 `col` の正確な百分位値を戻します。`percentage` の値は、0.0 と 1.0 の間である必要があります。`frequency` の値は正の整数である必要があります。

`percentile(col, array(percentage1 [, percentage2]...) [, frequency])`：指定した割合での数値列 `col` の正確な百分位値を戻します。割合配列の各値は、0.0 と 1.0 の間である必要があります。`frequency` の値は正の整数である必要があります。

#### percentile_approx

`percentile_approx(col, percentage [, accuracy])`：指定された割合での数値列 `col` の近似百分位値を戻します。`percentage` の値は、0.0 と 1.0 の間である必要があります。`accuracy` パラメーター（デフォルト：10000）は正の数値リテラルで、メモリーのコストと引き換えに近似精度を制御します。`accuracy` の値を大きくすると、精度が向上し、`1.0/accuracy` は近似の相対誤差です。`percentage` が配列の場合、割合配列の各値は 0.0 ～ 1.0 の範囲である必要があります。この場合、指定した割合配列における列 `col` の近似百分位配列を戻します。

例：

```sql
> SELECT percentile_approx(10.0, array(0.5, 0.4, 0.1), 100);
 [10.0,10.0,10.0]
> SELECT percentile_approx(10.0, 0.5, 100);
 10.0
```

#### pi

`pi()`：pi を戻します。

例：

```sql
> SELECT pi();
 3.141592653589793
```

#### pmod

`pmod(expr1, expr2)`：`expr1` mod `expr2` の正の値を戻します。

例：

```sql
> SELECT pmod(10, 3);
 1
> SELECT pmod(-10, 3);
 2
```

#### positive

`positive(expr)`：`expr` の正の値を戻します。 

#### pow

`pow(expr1, expr2)`：`expr2` を `expr1` 乗した値を戻します。

例：

```sql
> SELECT pow(2, 3);
 8.0
```

#### power

`power(expr1, expr2)`：`expr2` を `expr1` 乗した値を戻します。

例：

```sql
> SELECT power(2, 3);
 8.0
```

#### radians

`radians(expr)`：度をラジアンに変換します。

引数：

- `expr`：角度（度単位）

例：

```sql
> SELECT radians(180);
 3.141592653589793
```

#### rand

`rand([seed])`：(0, 1) の値を均等に分布した、独立同分布（i.i.d.）を持つランダムな値を戻します。

例：

```sql
> SELECT rand();
 0.9629742951434543
> SELECT rand(0);
 0.8446490682263027
> SELECT rand(null);
 0.8446490682263027
```

>[!NOTE]
>
> この関数は、一般的には非決定的です。

#### randn

`randn([seed])`：標準の正規分布から引き出された、独立同分布（i.i.d.）値を持つランダムな値を戻します。

例：

```sql
> SELECT randn();
 -0.3254147983080288
> SELECT randn(0);
 1.1164209726833079
> SELECT randn(null);
 1.1164209726833079
```

>[!NOTE]
>
> この関数は、一般的には非決定的です。

#### rint

`rint(expr)`：引数の値に最も近く、数学的整数に等しい double 値を戻します。

例：

```sql
> SELECT rint(12.3456);
 12.0
```

#### round

`round(expr, d)`：HALF_UP 丸めモードを使用して `expr` を小数点以下 `d` 桁に丸めた値を戻します。

例：

```sql
> SELECT round(2.5, 0);
 3.0
```

#### sign

`sign(expr)`：`expr` が負、0、正の場合にそれぞれ、-1.0、0.0、1.0 を戻します。

例：

```sql
> SELECT sign(40);
 1.0
```

#### signum

`signum(expr)`：`expr` が負、0、正の場合にそれぞれ、-1.0、0.0、1.0 を戻します。

例：

```sql
> SELECT signum(40);
 1.0
```

#### sin

`sin(expr)`：`expr` の正弦（サイン）を戻します。この値は、`java.lang.Math.sin` で計算されたものと同じです。

引数：

- `expr`：角度（ラジアン単位）

例：

```sql
> SELECT sin(0);
 0.0
```

#### sinh

`sinh(expr)`:`expr` の双曲線正弦（ハイパボリックサイン）を戻します。この値は、`java.lang.Math.sinh` で計算されたものと同じです。

引数：

- `expr`：双曲角

例：

```sql
> SELECT sinh(0);
 0.0
```

#### sqrt

`sqrt(expr)`：`expr` の平方根を戻します。

例：

```sql
> SELECT sqrt(4);
 2.0
```

#### stddev

`stddev(expr)`：グループの値から計算したサンプルの標準偏差を戻します。

#### stddev_pop

`sttdev_pop(expr)`：グループの値から計算された母集団の標準偏差を戻します。

#### stddev_samp

`stddev_samp(expr)`：グループの値から計算したサンプルの標準偏差を戻します。

#### sum

`sum(expr)`：グループの値から計算された合計を戻します。

#### tan

`tan(expr)`：`expr` の正接（タンジェント）を戻します。この値は、`java.lang.Math.tan` で計算されたものと同じです。

引数：

- `expr`：角度（ラジアン単位）

例：

```sql
> SELECT tan(0);
 0.0
```

#### tanh

`tanh(expr)`：`expr`の双曲線正接（ハイパボリックタンジェント）を戻します。この値は、`java.lang.Math.tanh`で計算されたものと同じです。

引数：

- `expr`：双曲角

例：

```sql
> SELECT tanh(0);
 0.0
```

#### Var_pop

`var_pop(expr)`：グループの値から計算された母分散を戻します。

#### Var_samp

`var_samp(expr)`：グループの値から計算された標本分散を戻します。

#### variance

`variance(expr)`：グループの値から計算された標本分散を戻します。

### 論理演算子 {#logical-operators}

#### 論理否定（not）

`! expr`: 論理否定（not）。

#### より小さい

`expr1 < expr2`：`expr1` が `expr2` より小さい場合は true を戻します。

引数：

- `expr1, expr2`：2 つの式は同じタイプであるか、共通のタイプにキャストでき、順序指定が可能なタイプである必要があります。例えば、マップタイプは順序指定不可なので、サポートされていません。配列や構造体などの複合型の場合、フィールドのデータタイプは順序指定可能である必要があります。

例：

```sql
> SELECT 1 < 2;
 true
> SELECT 1.1 < '1';
 false
> SELECT to_date('2009-07-30 04:17:52') < to_date('2009-07-30 04:17:52');
 false
> SELECT to_date('2009-07-30 04:17:52') < to_date('2009-08-01 04:17:52');
 true
> SELECT 1 < NULL;
 NULL
```

#### 次よりも小さいか等しい

`expr1 <= expr2`：`expr1` が `expr2` 以下である場合は true を戻します。

引数：

- `expr1, expr2`：2 つの式は同じタイプであるか、共通のタイプにキャストでき、順序付け可能なタイプである必要があります。例えば、マップタイプは順序指定不可なので、サポートされていません。配列や構造体などの複合型の場合、フィールドのデータタイプは並べ替え可能である必要があります。

例：

```sql
> SELECT 2 <= 2;
 true
> SELECT 1.0 <= '1';
 true
> SELECT to_date('2009-07-30 04:17:52') <= to_date('2009-07-30 04:17:52');
 true
> SELECT to_date('2009-07-30 04:17:52') <= to_date('2009-08-01 04:17:52');
 true
> SELECT 1 <= NULL;
 NULL
```

#### 次と等しい

`expr1 = expr2`：`expr1` と `expr2` が等しい場合は true を戻し、それ以外の場合は false を戻します。

引数：

- `expr1, expr2`：2 つの式は同じタイプであるか、共通のタイプにキャストでき、等価比較が可能なタイプである必要があります。マップタイプはサポートされていません。配列や構造体などの複合型の場合、フィールドのデータタイプは順序指定可能である必要があります。

例：

```sql
> SELECT 2 = 2;
 true
> SELECT 1 = '1';
 true
> SELECT true = NULL;
 NULL
> SELECT NULL = NULL;
 NULL
```

#### より大きい

`expr1 > expr2`：`expr1` が `expr2` より大きい場合は true を戻します。

引数：

- `expr1, expr2`：2 つの式は同じタイプであるか、共通のタイプにキャストでき、順序指定が可能なタイプである必要があります。例えば、マップタイプは順序指定不可なので、サポートされていません。配列や構造体などの複合型の場合、フィールドのデータタイプは順序指定可能である必要があります。

例：

```sql
> SELECT 2 > 1;
 true
> SELECT 2 > '1.1';
 true
> SELECT to_date('2009-07-30 04:17:52') > to_date('2009-07-30 04:17:52');
 false
> SELECT to_date('2009-07-30 04:17:52') > to_date('2009-08-01 04:17:52');
 false
> SELECT 1 > NULL;
 NULL
```

#### 以上

`expr1 >= expr2`：`expr1` が `expr2` 以上である場合は true を戻します。

引数：

- `expr1, expr2`：2 つの式は同じタイプであるか、共通のタイプにキャストでき、順序指定が可能なタイプである必要があります。例えば、マップタイプは順序指定不可なので、サポートされていません。配列や構造体などの複合型の場合、フィールドのデータタイプは順序指定可能である必要があります。

例：

```sql
> SELECT 2 >= 1;
 true
> SELECT 2.0 >= '2.1';
 false
> SELECT to_date('2009-07-30 04:17:52') >= to_date('2009-07-30 04:17:52');
 true
> SELECT to_date('2009-07-30 04:17:52') >= to_date('2009-08-01 04:17:52');
 false
> SELECT 1 >= NULL;
 NULL
```

#### ビット単位 XOR

`expr1 ^ expr2`：`expr1` と `expr2` のビット単位の排他的論理和（XOR）の結果を戻します。

例：

```sql
> SELECT 3 ^ 5;
 2
```

#### および

`expr1 and expr2`: 論理積（AND）。

#### arrays_overlap

`arrays_overlap(a1, a2)`：a1 に少なくても 1 つの、a2 にもある null 以外の要素が含まれている場合、true を戻します。配列に共通の要素がなく、両方とも空でなく、どちらか一方が null 要素を含む場合、null が戻されます。それ以外の場合は、false が戻されます。

例：

```sql
> SELECT arrays_overlap(array(1, 2, 3), array(3, 4, 5));
 true
```

バージョン 2.4.0 以降

#### assert_true

`assert_true(expr)`：`expr` が true でない場合は例外をスローします。

例：

```sql
> SELECT assert_true(0 < 1);
 NULL
```

#### if

`if(expr1, expr2, expr3)`：`expr1` が true に評価される場合は、`expr2` を戻します。それ以外の場合は、`expr3` を戻します。

例：

```sql
> SELECT if(1 < 2, 'a', 'b');
 a
```

#### ifnull

`ifnull(expr1, expr2)`：`expr1` が null の場合は `expr2` を戻し、それ以外の場合は `expr1` を戻します。

例：

```sql
> SELECT ifnull(NULL, array('2'));
 ["2"]
```

#### in

`expr1 in(expr2, expr3, ...)`：`expr` がいずれかの valN に等しい場合は true を戻します。

引数：
- `expr1, expr2, expr3, ...`：引数は同じ型である必要があります。

例：

```sql
> SELECT 1 in(1, 2, 3);
 true
> SELECT 1 in(2, 3, 4);
 false
> SELECT named_struct('a', 1, 'b', 2) in(named_struct('a', 1, 'b', 1), named_struct('a', 1, 'b', 3));
 false
> SELECT named_struct('a', 1, 'b', 2) in(named_struct('a', 1, 'b', 2), named_struct('a', 1, 'b', 3));
 true
```

#### isnan

`isnan(expr)`：`expr` が NaN の場合は true を、それ以外の場合は false を戻します。

例：

```sql
> SELECT isnan(cast('NaN' as double));
 true
```

#### isnotnull

`isnotnull(expr)`：`expr` が null でない場合は true を、それ以外の場合は false を戻します。

例：

```sql
> SELECT isnotnull(1);
 true
```

#### isnull

`isnull(expr)`：`expr` が null の場合は true を、それ以外の場合は false を戻します。

例：

```sql
> SELECT isnull(1);
 false
```

#### nanvl

`nanvl(expr1, expr2)`：NaN でない場合は `expr1`、それ以外の場合は `expr2` を戻します。

例：

```sql
> SELECT nanvl(cast('NaN' as double), 123);
 123.0
```

#### not

`not expr`: 論理否定（not）。

#### or

`expr1 or expr2`：論理和（OR）。

#### xpath_boolean

`xpath_boolean(xml, xpath)`：XPath 式が true と評価される場合、または一致するノードが見つかった場合は、true を戻します。

例：

```sql
> SELECT xpath_boolean('<a><b>1</b></a>','a/b');
 true
```

### 日付/時間関数 {#datetime-functions}

#### add_months

`add_months(start_date, num_months)`：`start_date` の `num_months` 後の日付を戻します。

例：

```sql
> SELECT add_months('2016-08-31', 1);
 2016-09-30
```

バージョン 1.5.0 以降

#### date_add

`date_add(start_date, num_days)`：`start_date` の `num_days` 後の日付を戻します。

例：

```sql
> SELECT date_add('2016-07-30', 1);
 2016-07-31
```

バージョン 1.5.0 以降

#### date_format

`date_format(timestamp, fmt)`：`timestamp` を日付形式 `fmt` で指定された形式の文字列値に変換します。

例：

```sql
> SELECT date_format('2016-04-08', 'y');
 2016
```

バージョン 1.5.0 以降

#### date_sub

`date_sub(start_date, num_days)`：`start_date` の `num_days` 前の日付を戻します。

例：

```sql
> SELECT date_sub('2016-07-30', 1);
 2016-07-29
```

バージョン 1.5.0 以降

#### date_trunc

`date_trunc(fmt, ts)`:形式モデル `fmt` で指定された単位に切り捨てられた日時 ts を戻します。`fmt` は、[「YEAR」、「YYYY」、「YY」、「MON」、「MONTH」、「MM」、「DAY」、「DD」、「HOUR」、「MINUTE」、「SECOND」、「WEEK」、「QUARTER」]のいずれかになります。

例：

```sql
> SELECT date_trunc('YEAR', '2015-03-05T09:32:05.359');
 2015-01-01 00:00:00
> SELECT date_trunc('MM', '2015-03-05T09:32:05.359');
 2015-03-01 00:00:00
> SELECT date_trunc('DD', '2015-03-05T09:32:05.359');
 2015-03-05 00:00:00
> SELECT date_trunc('HOUR', '2015-03-05T09:32:05.359');
 2015-03-05 09:00:00
```

バージョン 2.3.0 以降

#### datediff

`datediff(endDate, startDate)`：`startDate` から `endDate` までの日数を戻します。

例：

```sql
> SELECT datediff('2009-07-31', '2009-07-30');
 1

> SELECT datediff('2009-07-30', '2009-07-31');
 -1
```

バージョン 1.5.0 以降

#### 日

`day(date)`：日付/タイムスタンプの月の日付けを戻します。

例：

```sql
> SELECT day('2009-07-30');
 30
```

バージョン 1.5.0 以降

#### dayofmonth

`dayofmonth(date)`：日付/タイムスタンプの月の日付けを戻します。

例：

```sql
> SELECT dayofmonth('2009-07-30');
 30
```

バージョン 1.5.0 以降

#### dayofweek

`dayofweek(date)`:日付/タイムスタンプの曜日を戻します（1=日曜日、2=月曜日…、7=土曜日）。

例：

```sql
> SELECT dayofweek('2009-07-30');
 5
```

バージョン 2.3.0 以降

#### dayofyear

`dayofyear(date)`:日付/タイムスタンプの年の日付を戻します。

例：

```sql
> SELECT dayofyear('2016-04-09');
 100
```

バージョン 1.5.0 以降

#### from_unixtime

`from_unixtime(unix_time, format)`：指定された `format` での `unix_time` を戻します。

例：

```sql
> SELECT from_unixtime(0, 'yyyy-MM-dd HH:mm:ss');
 1970-01-01 00:00:00
```

バージョン 1.5.0 以降

#### from_utc_timestamp

`from_utc_timestamp(timestamp, timezone)`：「2017-07-14 02:40:00.0」のようなタイムスタンプを UTC での時刻として解釈し、その時刻を指定されたタイムゾーンのタイムスタンプとしてレンダリングします。例えば、「GMT+1」では「2017-07-14 03:40:00.0」を戻します。

例：

```sql
> SELECT from_utc_timestamp('2016-08-31', 'Asia/Seoul');
 2016-08-31 09:00:00
```

バージョン 1.5.0 以降

#### hour

`hour(timestamp)`:文字列／タイムスタンプの時間コンポーネントを戻します。

例：

```sql
> SELECT hour('2009-07-30 12:58:59');
 12
```

バージョン 1.5.0 以降

#### last_day

`last_day(date):`：日付が属する月の最終日を戻します。

例：

```sql
> SELECT last_day('2009-01-12');
 2009-01-31
```

バージョン 1.5.0 以降

#### minute

`minute(timestamp)`:文字列／タイムスタンプの分コンポーネントを戻します。

例：

```sql
> SELECT minute('2009-07-30 12:58:59');
 58
```

バージョン 1.5.0 以降

#### month

`month(date)`：日付／タイムスタンプの月コンポーネントを戻します。

例：

```sql
> SELECT month('2016-07-30');
 7
```

バージョン 1.5.0 以降

#### months_between

`months_between(timestamp1, timestamp2[, roundOff])`：`timestamp1` が `timestamp2` より後の場合、結果は正の数になります。`timestamp1` と `timestamp2` が同じ日付の場合、または両方が月の最後の日付の場合、時刻は無視されます。それ以外の場合、差は 1 か月を 31 日として計算され、 `roundOff=false` 出ない限り 8 桁に丸められます。

例：

```sql
> SELECT months_between('1997-02-28 10:30:00', '1996-10-30');
 3.94959677
> SELECT months_between('1997-02-28 10:30:00', '1996-10-30', false);
 3.9495967741935485
```

バージョン 1.5.0 以降

#### next_day

`next_day(start_date, day_of_week)`：`start_date` より後の最初で、指定された名前を持つ日付を戻します。

例：

```sql
> SELECT next_day('2015-01-14', 'TU');
 2015-01-20
```

バージョン 1.5.0 以降

#### quarter

`quarter(date)`：日付の四半期を 1 ～ 4 の範囲で戻します。

例：

```sql
> SELECT quarter('2016-08-31');
 3
```

バージョン 1.5.0 以降

#### second

`second(timestamp)`:文字列／タイムスタンプの秒コンポーネントを戻します。

例：

```sql
> SELECT second('2009-07-30 12:58:59');
 59
```

バージョン 1.5.0 以降

#### to_date

`to_date(date_str[, fmt])`：`date_str` 式を `fmt` 式を日付に解析します。入力が無効な場合は null を戻します。デフォルトでは、`fmt` を省略した場合、日付へのキャストルールに従っています。

例：

```sql
> SELECT to_date('2009-07-30 04:17:52');
 2009-07-30
> SELECT to_date('2016-12-31', 'yyyy-MM-dd');
 2016-12-31
```

バージョン 1.5.0 以降

#### to_timestamp

`to_timestamp(timestamp[, fmt])`：タイムスタンプを `timestamp` 式を `fmt` 式でタイムスタンプに解析します。入力が無効な場合は null を戻します。デフォルトでは、`fmt` を省略した場合、タイムスタンプへのキャストルールに従っています。

例：

```sql
> SELECT to_timestamp('2016-12-31 00:12:00');
 2016-12-31 00:12:00
> SELECT to_timestamp('2016-12-31', 'yyyy-MM-dd');
 2016-12-31 00:00:00
```

バージョン 2.2.0 以降

#### to_unix_timestamp

`to_unix_timestamp(expr[, pattern])`：指定された時刻の UNIX タイムスタンプを戻します。

例：

```sql
> SELECT to_unix_timestamp('2016-04-08', 'yyyy-MM-dd');
 1460041200
```

バージョン 1.6.0 以降

#### to_utc_timestamp

`to_utc_timestamp(timestamp, timezone)`:「2017-07-14 02:40:00.0」のようなタイムスタンプを指定されたタイムゾーンの時刻として解釈し、その時刻を UTC でのタイムスタンプとしてレンダリングします。例えば、「GMT+1」では「2017-07-14 1:40:00.0」を戻します。

例：

```sql
> SELECT to_utc_timestamp('2016-08-31', 'Asia/Seoul');
 2016-08-30 15:00:00
```

バージョン 1.5.0 以降

#### trunc

`trunc(date, fmt)`：日付の時間部分が、形式モデル `fmt` で指定された単位に切り捨てられた日付を戻します。`fmt` は、[「year」、「yyyy」、「yy」、「mon」、「month」、「mm」]のいずれかです。

例：

```sql
> SELECT trunc('2009-02-12', 'MM');
 2009-02-01
> SELECT trunc('2015-10-27', 'YEAR');
 2015-01-01
```

バージョン 1.5.0 以降

#### unix_timestamp

`unix_timestamp([expr[, pattern]])`：現在または指定された時間の UNIX タイムスタンプを戻します。

例：

```sql
> SELECT unix_timestamp();
 1476884637
> SELECT unix_timestamp('2016-04-08', 'yyyy-MM-dd');
 1460041200
```

バージョン 1.5.0 以降

#### weekday

`weekday(date)`：日付/タイムスタンプの曜日を戻します（0=月曜日、1=火曜日、...、6=日曜日）。

例：

```sql
> SELECT weekday('2009-07-30');
 3
```

バージョン 2.4.0 以降

#### week_of_year

`weekofyear(date)`：指定された日付の年の週を戻します。週は月曜日で始まると見なされ、1 週目は最初の 3 日以上の週です。

例：

```sql
> SELECT weekofyear('2008-02-20');
 8
```

バージョン 1.5.0 以降

#### when

`CASE WHEN expr1 THEN expr2 [WHEN expr3 THEN expr4]* [ELSE expr5] END`：`expr1` が true の場合は、`expr2` を戻します。それ以外の場合、`expr3` が true の場合、`expr4` を戻し、それ以外の場合は `expr5` を戻します。

引数：

- `expr1`、`expr3`：ブランチ条件の式は、すべてブール型にする必要があります。
- `expr2`、`expr4`、`expr5`：ブランチ値の式と「それ以外の場合」の値の式は、すべて同じ型であるか、共通型に強制可能である必要があります。

例：

```sql
> SELECT CASE WHEN 1 > 0 THEN 1 WHEN 2 > 0 THEN 2.0 ELSE 1.2 END;
 1
> SELECT CASE WHEN 1 < 0 THEN 1 WHEN 2 > 0 THEN 2.0 ELSE 1.2 END;
 2
> SELECT CASE WHEN 1 < 0 THEN 1 WHEN 2 < 0 THEN 2.0 END;
 NULL
```

#### year

`year(date)`：日付／タイムスタンプの年コンポーネントを戻します。

例：

```sql
> SELECT year('2016-07-30');
 2016
```

バージョン 1.5.0 以降

### 集計関数 {#aggregate-functions}

#### approx_count_distinct

`approx_count_distinct(expr[, relativeSD])`：HyperLogLog++ による推定基数を戻します。`relativeSD`：許可される最大推定誤差を定義します。

### 配列 {#arrays}

#### array

`array(expr, ...)`：指定された要素を持つ配列を戻します。

例：

```sql
> SELECT array(1, 2, 3);
 [1,2,3]
```

#### array_contains

`array_contains(array, value)`：配列に値が含まれる場合は true を戻します。

例：

```sql
> SELECT array_contains(array(1, 2, 3), 2);
 true
```

#### array_distinct

`array_distinct(array)`：配列から重複値を削除します。

例：

```sql
> SELECT array_distinct(array(1, 2, 3, null, 3));
 [1,2,3,null]
```

バージョン 2.4.0 以降

#### array_except

`array_except(array1, array2)`：`array1` の要素のうち `array2` にない要素の配列を戻します（重複なし）。

例：

```sql
> SELECT array_except(array(1, 2, 3), array(1, 3, 5));
 [2]
```

バージョン 2.4.0 以降

#### array_intersect

`array_intersect(array1, array2)`：`array1` と `array2` の積集合にある要素の配列を戻します（重複なし）。

例：

```sql
> SELECT array_intersect(array(1, 2, 3), array(1, 3, 5));
 [1,3]
```

バージョン 2.4.0 以降

#### array_join

`array_join(array, delimiter[, nullReplacement])`：指定された配列の要素を区切り文字とオプションの文字列を使用して連結し、NULL を置き換えます。`nullReplacement` に値が設定されていない場合、 null 値はすべてフィルターされます。

例：

```sql
> SELECT array_join(array('hello', 'world'), ' ');
 hello world
> SELECT array_join(array('hello', null ,'world'), ' ');
 hello world
> SELECT array_join(array('hello', null ,'world'), ' ', ',');
 hello , world
```

バージョン 2.4.0 以降

#### array_max

`array_max(array)`：配列の最大値を戻します。Null 要素はスキップされます。

例：

```sql
> SELECT array_max(array(1, 20, null, 3));
 20
```

バージョン 2.4.0 以降

#### array_min

`array_min(array)`:配列の最小値を戻します。Null 要素はスキップされます。

例：

```sql
> SELECT array_min(array(1, 20, null, 3));
 1
```

バージョン 2.4.0 以降

#### array_position

`array_position(array, element)`：配列の最初の要素の（1 から始まる）インデックスを長整数型（long）で戻します。

例：

```sql
> SELECT array_position(array(3, 2, 1), 1);
 3
```

バージョン 2.4.0 以降

#### array_remove

`array_remove(array, element)`：要素と等しい要素をすべて配列から削除します。

例：

```sql
> SELECT array_remove(array(1, 2, 3, null, 3), 3);
 [1,2,null]
```

バージョン 2.4.0 以降

#### array_repeat

`array_repeat(element, count)`：要素をカウント回含む配列を戻します。

例：

```sql
> SELECT array_repeat('123', 2);
 ["123","123"]
```

バージョン 2.4.0 以降

#### array_sort

`array_sort(array)`：入力配列を昇順に並べ替えます。入力配列の要素は、順序指定可能である必要があります。Null 要素は、戻された配列の末尾に配置されます。

例：

```sql
> SELECT array_sort(array('b', 'd', null, 'c', 'a'));
 ["a","b","c","d",null]
```

バージョン 2.4.0 以降

#### array_union

`array_union(array1, array2)`：`array1` と `array2` の和集合にある要素の配列を戻します（重複なし）。

例：

```sql
> SELECT array_union(array(1, 2, 3), array(1, 3, 5));
 [1,2,3,5]
```

バージョン 2.4.0 以降

#### array_zip

`arrays_zip(a1, a2, ...)`：N 番目の構造体に入力配列のすべての N 番目の値が含まれている、構造体の結合配列を戻します。

例：

```sql
> SELECT arrays_zip(array(1, 2, 3), array(2, 3, 4));
 [{"0":1,"1":2},{"0":2,"1":3},{"0":3,"1":4}]
> SELECT arrays_zip(array(1, 2), array(2, 3), array(3, 4));
 [{"0":1,"1":2,"2":3},{"0":2,"1":3,"2":4}]
```

バージョン 2.4.0 以降

#### element_at

`element_at(array, index)`：指定された（1 から始まる）インデックスの配列の要素を戻します。`index < 0` の場合、要素を最後から最初にアクセスします。インデックスが配列の長さを超える場合は NULL を戻します。

`element_at(map, key)`：指定されたキーの値を戻します。キーがマップに含まれていない場合は NULL を戻します。

例：

```sql
> SELECT element_at(array(1, 2, 3), 2);
 2
> SELECT element_at(map(1, 'a', 2, 'b'), 2);
 b
```

バージョン 2.4.0 以降

#### explode

`explode(expr)`：配列 `expr` の要素を複数の行に分割するか、マップ `expr` の要素を複数の行と列に分割します。

例：

```sql
> SELECT explode(array(10, 20));
 10
 20
```

#### explode_outer

`explode_outer(expr)`：配列 `expr` の要素を複数の行に分割するか、マップ `expr` の要素を複数の行と列に分割します。

例：

```sql
> SELECT explode_outer(array(10, 20));
 10
 20
```

#### find_in_set

`find_in_set(str, str_array)`：指定された文字列（`str`）のインデックス（1 から始まる）を、カンマで区切ったリスト（`str_array`）で戻します。文字列が見つからなかった場合、または指定した文字列（`str`）にコンマが含まれている場合は 0 を戻します。

例：

```sql
> SELECT find_in_set('ab','abc,b,ab,c,def');
 3
```

#### flatten

`flatten(arrayOfArrays)`：配列の配列を単一の配列に変換します。

例：

```sql
> SELECT flatten(array(array(1, 2), array(3, 4)));
 [1,2,3,4]
```

バージョン 2.4.0 以降

#### inline

`inline(expr)`：構造体の配列をテーブルに分解します。

例：

```sql
> SELECT inline(array(struct(1, 'a'), struct(2, 'b')));
 1  a
 2  b
```

#### inline_outer

`inline_outer(expr)`：構造体の配列をテーブルに分解します。

例：

```sql
> SELECT inline_outer(array(struct(1, 'a'), struct(2, 'b')));
 1  a
 2  b
```

#### posexplode

`posexplode(expr)`：配列 `expr` の要素を、位置を持つ複数の行に分割する、またはマップ `expr` の要素を、位置を持つ複数の行と列に分割します。

例：

```sql
> SELECT posexplode(array(10,20));
 0  10
 1  20
```

#### posexplode_outer

`posexplode_outer(expr)`：配列 `expr` の要素を、位置を持つ複数の行に分割する、またはマップ `expr` の要素を、位置を持つ複数の行と列に分割します。

例：

```sql
> SELECT posexplode_outer(array(10,20));
 0  10
 1  20
```

#### reverse

`reverse(array)`：逆順の文字列、または要素の順序が逆の配列を戻します。

例：

```sql
> SELECT reverse('Spark SQL');
 LQS krapS
> SELECT reverse(array(2, 1, 4, 3));
 [3,4,1,2]
```

バージョン 1.5.0 以降

>[!NOTE]
>
> 配列の reverse 論理は 2.4.0 以降で使用可能です。

#### shuffle

`shuffle(array)`：指定された配列のランダムな順列を戻します。

例：

```sql
> SELECT shuffle(array(1, 20, 3, 5));
 [3,1,5,20]
> SELECT shuffle(array(1, 20, null, 3));
 [20,null,3,1]
```

バージョン 2.4.0 以降

>[!NOTE]
>
> 関数は非決定的です。

#### slice

`slice(x, start, length)`：start インデックスから始まる（または、start が負の場合は end）指定された長さのサブセット配列 x。

例：

```sql
> SELECT slice(array(1, 2, 3, 4), 2, 2);
 [2,3]
> SELECT slice(array(1, 2, 3, 4), -2, 2);
 [3,4]
```

バージョン 2.4.0 以降

#### sort_array

`sort_array(array[, ascendingOrder])`：入力配列を、配列要素の自然な順序に従って昇順または降順に並べ替えます。Null 要素は、昇順または降順で戻された配列の最後に配置されます。

例：

```sql
> SELECT sort_array(array('b', 'd', null, 'c', 'a'), true);
 [null,"a","b","c","d"]
```

#### zip_with

`zip_with(left, right, func)`：関数を使用して、指定された 2 つの配列を要素ごとに 1 つの配列に結合します。いずれかの配列が短い場合は、関数を適用する前に、長い方の配列の長さに合わせて末尾に null が追加されます。

例：

```sql
> SELECT zip_with(array(1, 2, 3), array('a', 'b', 'c'), (x, y) -> (y, x));
 [{"y":"a","x":1},{"y":"b","x":2},{"y":"c","x":3}]
> SELECT zip_with(array(1, 2), array(3, 4), (x, y) -> x + y);
 [4,6]
> SELECT zip_with(array('a', 'b', 'c'), array('d', 'e', 'f'), (x, y) -> concat(x, y));
 ["ad","be","cf"]
```

バージョン 2.4.0 以降

### データタイプキャスト関数 {#datatype-casting}

#### bigint

`bigint(expr)`：`expr` 値をデターゲットデータタイプ `bigint` にキャストします。

#### binary

`binary(expr)`：`expr` 値をデターゲットデータタイプ `binary` にキャストします。

#### ブール型

`boolean(expr)`：`expr` 値をデターゲットデータタイプ `boolean` にキャストします。

#### cast

`cast(expr AS type)`：`expr` 値をデターゲットデータタイプ `type` にキャストします。

例：

```sql
> SELECT cast('10' as int);
 10
```

#### date

`date(expr)`：`expr` 値をデターゲットデータタイプ `date` にキャストします。

#### decimal

`decimal(expr)`：`expr` 値をデターゲットデータタイプ `decimal` にキャストします。

#### double

`double(expr)`：`expr` 値をデターゲットデータタイプ `double` にキャストします。

#### float

`float(expr)`：`expr` 値をデターゲットデータタイプ `float` にキャストします。

#### int

`int(expr)`：`expr` 値をデターゲットデータタイプ `int` にキャストします。

#### map

`map(key0, value0, key1, value1, ...)`：指定されたキーと値のペアを持つマップを作成します。

例：

```sql
> SELECT map(1.0, '2', 3.0, '4');
 {1.0:"2",3.0:"4"}
```

#### smallint

`smallint(expr)`：`expr` 値をデターゲットデータタイプ `smallint` にキャストします。

#### str_to_map

`str_to_map(text[, pairDelim[, keyValueDelim]])`：区切り文字を使用してテキストをキーと値の対に分割した後に、マップを作成します。デフォルトの区切り文字は、「,」（`pairDelim`の場合）および「:」（`keyValueDelim`の場合）です。

例：

```sql
> SELECT str_to_map('a:1,b:2,c:3', ',', ':');
 map("a":"1","b":"2","c":"3")
> SELECT str_to_map('a');
 map("a":null)
```

#### 文字列

`string(expr)`：`expr` 値をデターゲットデータタイプ `string` にキャストします。

#### struct

`struct(col1, col2, col3, ...)`：指定されたフィールド値を持つ構造体を作成します。

#### tinyint

`tinyint(expr)`：`expr` 値をデターゲットデータタイプ `tinyint` にキャストします。

### 変換関数と書式設定関数 {#conversion}

#### ascii

`ascii(str)`：`str` の最初の文字の数値を戻します。

例：

```sql
> SELECT ascii('222');
 50
> SELECT ascii(2);
 50
```

#### base64

`base64(bin)`：引数をバイナリ `bin` から基数 64 の文字列に変換します。

例：

```sql
> SELECT base64('Spark SQL');
 U3BhcmsgU1FM
```

#### bin

`bin(expr)`：バイナリ形式で表された long 値 `expr` の文字列表現を戻します。

例：

```sql
> SELECT bin(13);
 1101
> SELECT bin(-13);
 1111111111111111111111111111111111111111111111111111111111110011
> SELECT bin(13.3);
 1101
```

#### bit_length

`bit_length(expr)`：文字列データのビット長またはバイナリデータのビット数を戻します。

例：

```sql
> SELECT bit_length('Spark SQL');
 72
```

#### char

`char(expr)`：`expr` と同等のバイナリを持つ ASCII 文字を戻します。N が 256 より大きい場合、結果は `chr(n % 256)` と同じになります 。

例：

```sql
> SELECT char(65);
 A
```

#### char_length

`char_length(expr)`：文字列データの文字長またはバイナリデータのバイト数を戻します。文字列データの長さには末尾の空白文字が含まれます。バイナリデータの長さには、バイナリゼロが含まれます。

例：

```sql
> SELECT char_length('Spark SQL ');
 10
> SELECT CHAR_LENGTH('Spark SQL ');
 10
> SELECT CHARACTER_LENGTH('Spark SQL ');
 10
```

#### character_length

`character_length(expr)`：文字列データの文字長またはバイナリデータのバイト数を戻します。文字列データの長さには末尾の空白文字が含まれます。バイナリデータの長さには、バイナリゼロが含まれます。

例：

```sql
> SELECT character_length('Spark SQL ');
 10
> SELECT CHAR_LENGTH('Spark SQL ');
 10
> SELECT CHARACTER_LENGTH('Spark SQL ');
 10
```

#### chr

`chr(expr)`：Expr に相当するバイナリを持つ ASCII 文字を戻します。N が 256 より大きい場合、結果は `chr(n % 256)` と同じになります。

例：

```sql
> SELECT chr(65);
 A
```

#### degrees

`degrees(expr)`：ラジアンを度に変換します。

引数：
- `expr`：角度（ラジアン単位）

例：

```sql
> SELECT degrees(3.141592653589793);
 180.0
```

#### format_number

`format_number(expr1, expr2)`：「#,###,####.##」のような数値 `expr1` の小数点以下 `expr2` 桁数に丸められた形式を設定します。`expr2` が 0 の場合、結果には小数点や小数部分は含まれません。`expr2` は、ユーザー指定の形式も受け入れます。これは、MySQL `FORMAT` と同様に機能することを目的としています。

例：

```sql
> SELECT format_number(12332.123456, 4);
 12,332.1235
> SELECT format_number(12332.123456, '##################.###');
 12332.123
```

#### from_json

`from_json(jsonStr, schema[, options])`：指定された `jsonStr` と `schema` と共に struct 値を戻します。

例：

```sql
> SELECT from_json('{"a":1, "b":0.8}', 'a INT, b DOUBLE');
 {"a":1, "b":0.8}
> SELECT from_json('{"time":"26/08/2015"}', 'time Timestamp', map('timestampFormat', 'dd/MM/yyyy'));
 {"time":"2015-08-26 00:00:00.0"}
```

バージョン 2.2.0 以降

#### hash

`hash(expr1, expr2, ...)`：引数のハッシュ値を戻します。

例：

```sql
> SELECT hash('Spark', array(123), 2);
 -1321691492
```

#### hex

`hex(expr)`：`expr` を 16 進数に変換します。

例：

```sql
> SELECT hex(17);
 11
> SELECT hex('Spark SQL');
 537061726B2053514C
```

#### initcap

`initcap(str)`：`str` の各単語の最初の文字を大文字にして戻します。その他の文字はすべて小文字で表されます。単語は空白で区切られます。

例：

```sql
> SELECT initcap('sPark sql');
 Spark Sql
```

#### lcase

`lcase(str)`：`str` のすべての文字をを小文字にして戻します。 

例：

```sql
> SELECT lcase('SparkSql');
 sparksql
```

#### lower

`lower(str)`：`str` のすべての文字をを小文字にして戻します。

例：

```sql
> SELECT lower('SparkSql');
 sparksql
```

#### lpad

`lpad(str, len, pad)`：`str` の左に `pad` を必要に応じて連続的に埋め込み、長さ `len` にして戻します。`str` が `len` より長い場合、戻り値は `len` 文字に短縮されます。

例：

```sql
> SELECT lpad('hi', 5, '??');
 ???hi
> SELECT lpad('hi', 1, '??');
 h
```

#### map

`map(key0, value0, key1, value1, ...)`：指定されたキーと値のペアを持つマップを作成します。

例：

```
> SELECT map(1.0, '2', 3.0, '4');
 {1.0:"2",3.0:"4"}
```

#### map_from_arrays

`map_from_arrays(keys, values)`：指定されたキーと値の配列の対を持つマップを作成します。キー内の要素は NULL にできません。

例：

```sql
> SELECT map_from_arrays(array(1.0, 3.0), array('2', '4'));
 {1.0:"2",3.0:"4"}
```

バージョン 2.4.0 以降

#### map_from_entries

`map_from_entries(arrayOfEntries)`：渡されたエントリの配列から作成されたマップを戻します。

例：

```sql
> SELECT map_from_entries(array(struct(1, 'a'), struct(2, 'b')));
 {1:"a",2:"b"}
```

バージョン 2.4.0 以降

#### md5

`md5(expr)`：MD5 128 ビットのチェックサムを `expr` の 16 進文字列で戻します。

例：

```sql
> SELECT md5('Spark');
 8cde774d6f7333752ed72cacddb05126
```

#### rpad

`rpad(str, len, pad)`：`str` の右に `pad` を必要に応じて連続的に埋め込み、長さ `len` にして戻します。`str` が `len` より長い場合、戻り値は `len` 文字に短縮されます。

例：

```sql
> SELECT rpad('hi', 5, '??');
 hi???
> SELECT rpad('hi', 1, '??');
 h
```

#### rtrim

`rtrim(str)`：`str` から末尾の空白文字を削除します。

`rtrim(trimStr, str)`：末尾の、`str` からのトリム文字列を削除します。

引数：
- `str`：文字列式。
- `trimStr`：トリミングするトリミング文字列文字。デフォルト値は 1 つのスペースです。

例：

```sql
> SELECT rtrim('    SparkSQL   ');
 SparkSQL
> SELECT rtrim('LQSa', 'SSparkSQLS');
 SSpark
```

#### sha

`sha(expr)`：`sha1` ハッシュ値を `expr` の 16 進数文字列として戻します。

例：

```sql
> SELECT sha('Spark');
 85f5955f4b27a9a4c2aab6ffe5d7189fc298b92c
```

#### sha1

`sha1(expr)`：`sha1` ハッシュ値を `expr` の 16 進数文字列として戻します。

例：

```sql
> SELECT sha1('Spark');
 85f5955f4b27a9a4c2aab6ffe5d7189fc298b92c
```

#### sha2

`sha2(expr, bitLength)`：SHA-2 ファミリーのチェックサムを `expr` の 16 進文字列として戻します。SHA-224、SHA-256、SHA-384、SHA-512 がサポートされています。ビット長が 0 の場合は 256 と等しくなります。

例：

```sql
> SELECT sha2('Spark', 256);
 529bc3b07127ecb7e53a4dcf1991d9152c24537d919178022b2c42657f79a26b
```

#### soundex

`soundex(str)`：文字列の Soundex コードを戻します。

例：

```sql
> SELECT soundex('Miller');
 M460
```

#### stack

`stack(n, expr1, ..., exprk)`：`expr1`、...、`exprk` を `n` 行に分割します。

例：

```sql
> SELECT stack(2, 1, 2, 3);
 1  2
 3  NULL
```

#### substr

`substr(str, pos[, len])`：`pos` で始まり、長さが `len` である `str` の部分文字列、または `pos` で始まり、長さが `len` あるバイト配列のスライスを戻します。

例：

```sql
> SELECT substr('Spark SQL', 5);
 k SQL
> SELECT substr('Spark SQL', -3);
 SQL
> SELECT substr('Spark SQL', 5, 1);
 k
```

#### substring

`substring(str, pos[, len])`：`pos` で始まり、長さが `len` である `str` の部分文字列、または `pos` で始まり、長さが `len` あるバイト配列のスライスを戻します。

例：

```sql
> SELECT substring('Spark SQL', 5);
 k SQL
> SELECT substring('Spark SQL', -3);
 SQL
> SELECT substring('Spark SQL', 5, 1);
 k
```

#### to_json

`to_json(expr[, options])`：指定された構造体値で JSON 文字列を戻します。

例：

```sql
> SELECT to_json(named_struct('a', 1, 'b', 2));
 {"a":1,"b":2}
> SELECT to_json(named_struct('time', to_timestamp('2015-08-26', 'yyyy-MM-dd')), map('timestampFormat', 'dd/MM/yyyy'));
 {"time":"26/08/2015"}
> SELECT to_json(array(named_struct('a', 1, 'b', 2)));
 [{"a":1,"b":2}]
> SELECT to_json(map('a', named_struct('b', 1)));
 {"a":{"b":1}}
> SELECT to_json(map(named_struct('a', 1),named_struct('b', 2)));
 {"[1]":{"b":2}}
> SELECT to_json(map('a', 1));
 {"a":1}
> SELECT to_json(array((map('a', 1))));
 [{"a":1}]
```

バージョン 2.2.0 以降

#### translate

`translate(input, from, to)`：`from` 文字列内に存在する文字を `to` 文字列内に対応して存在する文字で置き換えることによって、`input` 文字列を変換します。

例：

```sql
> SELECT translate('AaBbCc', 'abc', '123');
 A1B2C3
```

#### trim

`trim(str)`：先頭と末尾にある `str` からの空白文字を削除します。

`trim(BOTH trimStr FROM str)`：`str` から、先頭と末尾にある `trimStr` 文字を削除します。

`trim(LEADING trimStr FROM str)`：`str` から、先頭にある `trimStr` からの文字を削除します。

`trim(TRAILING trimStr FROM str)`：`str` から末尾の `trimStr` 文字を削除します。

引数：
- `str`：文字列式。
- `trimStr`：トリミングするトリミング文字列文字。デフォルト値は 1 つのスペースです。
- `BOTH`、`FROM`：文字列の両端から文字列を切り抜くことを指定するキーワードです。
- `LEADING`、`FROM`：文字列の左端から文字列の文字を切り抜くことを指定するキーワードです
- `TRAILING`、`FROM`：文字列の右端から文字列の文字を切り抜くことを指定するキーワードです。

例：

```sql
> SELECT trim('    SparkSQL   ');
 SparkSQL
> SELECT trim('SL', 'SSparkSQLS');
 parkSQ
> SELECT trim(BOTH 'SL' FROM 'SSparkSQLS');
 parkSQ
> SELECT trim(LEADING 'SL' FROM 'SSparkSQLS');
 parkSQLS
> SELECT trim(TRAILING 'SL' FROM 'SSparkSQLS');
 SSparkSQ
```

#### ucase

`ucase(str)`：すべての `str` 文字を大文字に変更して戻します。

例：

```sql
> SELECT ucase('SparkSql');
 SPARKSQL
```

#### unbase64

`unbase64(str)`：引数を基数 64 の文字列 `str` からバイナリに変換します。

例：

```sql
> SELECT unbase64('U3BhcmsgU1FM');
 Spark SQL
```

#### unhex

`unhex(expr)`：16 進数 `expr` を 2 進数に変換します。

例：

```sql
> SELECT decode(unhex('537061726B2053514C'), 'UTF-8');
 Spark SQL
```

#### upper

`upper(str)`：すべての `str` 文字を大文字に変更して戻します。

例：

```sql
> SELECT upper('SparkSql');
 SPARKSQL
```

#### uuid

`uuid()`：UUID（Universally Unique Identifier）文字列を戻します。値は、標準の UUID 36 文字の文字列として戻されます。

例：

```sql
> SELECT uuid();
 46707d92-02f4-4817-8116-a4c3b23e6266
```

>[!NOTE]
>
> 関数は非決定的です。

### データ評価 {#data-evaluation}

#### coalesce

`coalesce(expr1, expr2, ...)`：存在する場合は、最初の null 以外の引数を戻します。それ以外の場合は null です。

例：

```sql
> SELECT coalesce(NULL, 1, NULL);
 1
```

#### collect_list

`collect_list(expr)`：一意でない要素のリストを収集し、戻します。

#### collect_set

`collect_set(expr)`：一意の要素のセットを収集して戻します。

#### concat

`concat(col1, col2, ..., colN)`：col1、col2、...、colN を連結して戻します。

例：

```sql
> SELECT concat('Spark', 'SQL');
 SparkSQL
> SELECT concat(array(1, 2, 3), array(4, 5), array(6));
 [1,2,3,4,5,6]
```

>[!NOTE]
>
>`concat` 配列の  論理は 2.4.0 以降で使用可能です。

#### concat_ws

`concat_ws(sep, [str | array(str)]+)`：`sep` で区切られた文字列を連結して戻します。

例：

```sql
> SELECT concat_ws(' ', 'Spark', 'SQL');
  Spark SQL
```

#### count

`count(*)`:取得した行の総数（null を含む行も含む）を戻します。

`count(expr[, expr...])`：行の式がすべて null 以外の、指定された行の数を戻します。

`count(DISTINCT expr[, expr...])`：行の式が一意で null 以外の、指定された行の数を戻します。

#### crc32

`crc32(expr)`：`expr` の巡回冗長チェック値を bigint として戻します。

例：

```sql
> SELECT crc32('Spark');
 1557323817
```

#### decode

`decode(bin, charset)`：2 番目の引数文字セットを使用して、最初の引数を復号します。

例：

```sql
> SELECT decode(encode('abc', 'utf-8'), 'utf-8');
 abc
```

#### elt

`elt(n, input1, input2, ...)`：`n` 番目の入力を戻します。例えば、`n` が 2 の場合は `input2` を戻します。

例：

```sql
> SELECT elt(1, 'scala', 'java');
 scala
```

#### encode

`encode(str, charset)`：2 番目の引数文字セットを使用して、最初の引数を符号化します。

例：

```sql
> SELECT encode('abc', 'utf-8');
 abc
```

#### first

`first(expr[, isIgnoreNull])`：行のグループの最初の `expr` 値を戻します。`isIgnoreNull` が true の場合、null 以外の値のみを戻します。

#### first_value

`first_value(expr[, isIgnoreNull])`：行のグループの最初の `expr` 値を戻します。`isIgnoreNull` が true の場合、null 以外の値のみを戻します。

#### get_json_object

`get_json_object(json_txt, path)`：`path` から json オブジェクトを抽出します。

例：

```sql
> SELECT get_json_object('{"a":"b"}', '$.a');
 b
```

#### grouping

<!-- was blank -->

#### grouping_id

<!-- was blank -->

#### instr

`instr(str, substr)`：`str` で 1 番目に発生する `substr` 値の（1 から始まる）インデックスを戻します。

例：

```sql
> SELECT instr('SparkSQL', 'SQL');
 6
```

#### json_tuple

`json_tuple(jsonStr, p1, p2, ..., pn)`：`get_json_object` 関数と同じタプルを戻しますが、複数の名前を取ります。すべての入力パラメーターと出力列の型は文字列です。

例：

```sql
> SELECT json_tuple('{"a":1, "b":2}', 'a', 'b');
 1  2
```

#### lag

`lag(input[, offset[, default]])`：ウィンドウ内の現在の行の `offset` 行前の行の `input` 値を戻します。`offset` のデフォルト値は 1 で、`default` のデフォルト値は null です。`input` の `offset` 行目の値が null の場合、null が戻されます。このようなオフセット行がない場合（例えば、オフセットが 1 の場合、ウィンドウの最初の行には前の行がありません）、`default` を戻します。

#### last

`last(expr[, isIgnoreNull])`：行のグループの最後の `expr` 値を戻します。`isIgnoreNull` が true の場合、null 以外の値のみを戻します。

#### last_value

`last_value(expr[, isIgnoreNull])`：行のグループの最後の `expr` 値を戻します。`isIgnoreNull` が true の場合、null 以外の値のみを戻します。

#### lead

`lead(input[, offset[, default]])`：ウィンドウ内の現在の行の `input` 行後の行の `offset` 値を戻します。`offset` のデフォルト値は 1 で、`default` のデフォルト値は null です。`input` の `offset` 行目の値が null の場合、null が戻されます。このようなオフセット行がない場合（例えば、オフセットが 1 の場合、ウィンドウの最後の行には後の行がありません）、`default` が戻されます。


#### left

`left(str, len)`：`str` 文字列の左端の `len`（`len` は文字列型の場合もあります）を戻します。`len` が 0 以下の場合、結果は空の文字列になります。

例：

```sql
> SELECT left('Spark SQL', 3);
 Spa
```

#### length

`length(expr)`：文字列データの文字長またはバイナリデータのバイト数を戻します。文字列データの長さには末尾の空白文字が含まれます。バイナリデータの長さには、バイナリゼロが含まれます。

例：

```sql
> SELECT length('Spark SQL ');
 10
> SELECT CHAR_LENGTH('Spark SQL ');
 10
> SELECT CHARACTER_LENGTH('Spark SQL ');
 10
```

#### locate

`locate(substr, str[, pos])`：`str` で位置 `pos` の後での最初の `substr` 値の位置を戻します。指定された `pos` と戻り値は 1 から始まります。

例：

```sql
> SELECT locate('bar', 'foobarbar');
 4
> SELECT locate('bar', 'foobarbar', 5);
 7
> SELECT POSITION('bar' IN 'foobarbar');
 4
```

#### map_concat

`map_concat(map, ...)`：すべての指定されたマップの和集合を戻します。

例：

```sql
> SELECT map_concat(map(1, 'a', 2, 'b'), map(2, 'c', 3, 'd'));
 {1:"a",2:"c",3:"d"}
```

バージョン 2.4.0 以降

#### map_keys

`map_keys(map)`：マップのキーを含む、順不同配列を戻します。

例：

```sql
> SELECT map_keys(map(1, 'a', 2, 'b'));
 [1,2]
```

#### map_values

`map_values(map)`：マップの値を含む、順不同の配列を戻します。

例：

```sql
> SELECT map_values(map(1, 'a', 2, 'b'));
 ["a","b"]
```

#### ntile

`ntile(n)`：各ウィンドウパーティションの行を、1 と `n` の間の `n` のグループに分割します。

#### nullif

`nullif(expr1, expr2)`：`expr1` が `expr2` に等しい場合は null を戻し、それ以外の場合は `expr1` を戻します。

例：

```sql
> SELECT nullif(2, 2);
 NULL
```

#### nvl

`nvl(expr1, expr2)`：`expr1` が null の場合は `expr2` を戻し、それ以外の場合は `expr1` を戻します。

例：

```sql
> SELECT nvl(NULL, array('2'));
 ["2"]
```

#### nvl2

`nvl2(expr1, expr2, expr3)`：`expr1` が null でない場合は `expr2` を戻し、それ以外の場合は `expr3` を戻します。

例：

```sql
> SELECT nvl2(NULL, 2, 1);
 1
```

#### parse_url

`parse_url(url, partToExtract[, key])`：URL から部分を抽出します。

例：

```sql
> SELECT parse_url('http://spark.apache.org/path?query=1', 'HOST')
 spark.apache.org
> SELECT parse_url('http://spark.apache.org/path?query=1', 'QUERY')
 query=1
> SELECT parse_url('http://spark.apache.org/path?query=1', 'QUERY', 'query')
 1
```

#### position

`position(substr, str[, pos])`：`str` で位置 `pos` の後での最初の `substr` 値の位置を戻します。指定された `pos` と戻り値は 1 から始まります。

例：

```sql
> SELECT position('bar', 'foobarbar');
 4
> SELECT position('bar', 'foobarbar', 5);
 7
> SELECT POSITION('bar' IN 'foobarbar');
 4
```

#### rank

`rank()`：値のグループ内の値のランクを計算します。結果は、1 に、パーティションの順序付けで現在の行の前または現在の行数を足したものになります。この値は、シーケンス内でギャップを生成します。

#### regexp_extract

`regexp_extract(str, regexp[, idx])`：`regexp` に一致するグループを抽出します。

例：

```sql
> SELECT regexp_extract('100-200', '(\\d+)-(\\d+)', 1);
 100
```

#### regex_replace

`regexp_replace(str, regexp, rep)`：`regexp` を `rep` に一致するすべての `str` の部分文字列を置き換えます。

例：

```sql
> SELECT regexp_replace('100-200', '(\\d+)', 'num');
 num-num
```

#### repeat

`repeat(str, n)`：指定された文字列値を n 回繰り返す文字列を戻します。

例：

```sql
> SELECT repeat('123', 2);
 123123
```

#### replace

`replace(str, search[, replace])`：`search` のすべての出現箇所を `replace` で置き換えます。

引数：
- `str`：文字列式。
- `search`：文字列式。`search` が `str` に見つからない場合、`str` が変更なしで戻されます。
- `replace`：文字列式。`replace` が指定されていない場合、または空の文字列の場合、`str` から削除された文字列は何も置き換えられません。

例：

```sql
> SELECT replace('ABCabc', 'abc', 'DEF');
 ABCDEF
```

#### rollup

<!-- was blank -->

#### row_number

`row_number()`：ウィンドウパーティション内の行の順序に従って、各行に 1 から始まる一意の順番を割り当てます。

#### schema_of_json

`schema_of_json(json[, options])`：スキーマを JSON 文字列の DDL 形式で戻します。

例：

```sql
> SELECT schema_of_json('[{"col":0}]');
 array<struct<col:int>>
```

バージョン 2.4.0 以降

#### sentences

`sentences(str[, lang, country])`：`str` を単語の配列の配列に分割します。

例：

```sql
> SELECT sentences('Hi there! Good morning.');
 [["Hi","there"],["Good","morning"]]
```

#### sequence

`sequence(start, stop, step)`：Start から stop までの要素の配列を生成し、step ごとに増分します。戻される要素の型は、引数式の型と同じです。

byte、short、integer、long、date、timestamp がサポートされています。

`start` 式と `stop` 式は同じ型に解決する必要があります。`start` 式と `stop` 式が「date」または「timestamp」型に解決される場合、`step` 式は「interval」タイプに解決される必要があります。それ以外の場合は、`start` 式と `stop` 式と同じ型に解決されます。

引数：
- `start`：式。範囲の開始。
- `stop`：式。範囲の終了（範囲を含む）。
- `step`：オプションの式。範囲のステップ。By default `step` is &#39;1&#39; if `start` is less than or equal to `stop`, otherwise &#39;-1&#39;. 時間シーケンスの場合は、それぞれ「1」日と「 —1」日になります。 `start` が `stop` より大きい場合は、`step` は負の値に設定し、逆の場合も同様に設定します。

例：

```sql
> SELECT sequence(1, 5);
 [1,2,3,4,5]
> SELECT sequence(5, 1);
 [5,4,3,2,1]
> SELECT sequence(to_date('2018-01-01'), to_date('2018-03-01'), interval '1' month);
 [2018-01-01,2018-02-01,2018-03-01]
```

バージョン 2.4.0 以降

#### shiftleft

`shiftleft(base, expr)`：ビット単位の左シフト。

例：

```sql
> SELECT shiftleft(2, 1);
 4
```

#### shiftright

`shiftright(base, expr)`：ビット単位の符号付き右シフト。

例：

```sql
> SELECT shiftright(4, 1);
 2
```

#### shiftrightunsigned

`shiftrightunsigned(base, expr)`：ビット単位の符号なし右シフト。

例：

```sql
> SELECT shiftrightunsigned(4, 1);
 2
```

#### size

`size(expr)`：配列またはマップのサイズを戻します。入力が null で `spark.sql.legacy.sizeOfNull` が true に設定されている場合は、-1 を戻します。`spark.sql.legacy.sizeOfNull` を false に設定すると、null 入力の場合は null を戻します。デフォルトでは、`spark.sql.legacy.sizeOfNull` パラメーターは true に設定されています。

例：

```sql
> SELECT size(array('b', 'd', 'c', 'a'));
 4
> SELECT size(map('a', 1, 'b', 2));
 2
> SELECT size(NULL);
 -1
```

#### space

`space(n)`：`n` 個のスペースで構成される文字列を戻します。

例：

```sql
> SELECT concat(space(2), '1');
   1
```

#### split

`split(str, regex)`：`str` を `regex` に一致する出現箇所を囲んで分割します。

例：

```sql
> SELECT split('oneAtwoBthreeC', '[ABC]');
 ["one","two","three",""]
```

#### substring_index

`substring_index(str, delim, count)`：`delim` 区切り文字の `count` 個の出現箇所の前の、`str`の部分文字列を戻します。`count` が正の場合、（左から数えて）最後の区切り文字の左側がすべて戻されます。`count` が負の場合、（右から数えて）最後の区切り文字の右側がすべて戻されます。`substring_index` 関数は、大文字と小文字を区別して `delim` への一致を検索します。

例：

```sql
> SELECT substring_index('www.apache.org', '.', 2);
 www.apache
```

#### window

<!-- was blank -->

#### xpath

`xpath(xml, xpath)`：XPath 式と一致する xml のノード内の値の文字列配列を戻します。

例：

```sql
> SELECT xpath('<a><b>b1</b><b>b2</b><b>b3</b><c>c1</c><c>c2</c></a>','a/b/text()');
 ['b1','b2','b3']
```

#### xpath_double

`xpath_double(xml, xpath)`：double 値を戻します。一致が見つからない場合は値 0 を戻し、一致が見つかったが値が数値以外の場合は NaN を戻します。

例：

```sql
> SELECT xpath_double('<a><b>1</b><b>2</b></a>', 'sum(a/b)');
 3.0
```

#### xpath_float

`xpath_float(xml, xpath)`：Float 値を戻します。一致が見つからない場合は値 0 を戻し、一致が見つかったが値が数値以外の場合は NaN を戻します。

例：

```sql
> SELECT xpath_float('<a><b>1</b><b>2</b></a>', 'sum(a/b)');
 3.0
```

#### xpath_int

`xpath_int(xml, xpath)`：integer 値を戻します。一致が見つからない場合や、一致が見つかったが値が数値以外の場合は値 0 を戻します。

例：

```sql
> SELECT xpath_int('<a><b>1</b><b>2</b></a>', 'sum(a/b)');
 3
```

#### xpath_long

`xpath_long(xml, xpath)`：long integer 値を戻します。一致が見つからない場合や、一致が見つかったが値が数値以外の場合は値 0 を戻します。

例：

```sql
> SELECT xpath_long('<a><b>1</b><b>2</b></a>', 'sum(a/b)');
 3
```

#### xpath_number

`xpath_number(xml, xpath)`：double 値を戻します。一致が見つからない場合は値 0 を戻し、一致が見つかったが値が数値以外の場合は NaN を戻します。

例：

```sql
> SELECT xpath_number('<a><b>1</b><b>2</b></a>', 'sum(a/b)');
 3.0
```

#### xpath_short

`xpath_short(xml, xpath)`：short integer 値を戻します。一致が見つからない場合や、一致が見つかったが値が数値以外の場合は値 0 を戻します。

例：

```sql
> SELECT xpath_short('<a><b>1</b><b>2</b></a>', 'sum(a/b)');
 3
```

#### xpath_string

`xpath_string(xml, xpath)`：XPath 式に一致する最初の xml ノードのテキスト内容を戻します。

例：

```sql
> SELECT xpath_string('<a><b>b</b><c>cc</c></a>','a/c');
 cc
```

### Current information {#current-information}

#### current_database

`current_database()`：現在のデータベースを戻します。

例：

```sql
> SELECT current_database();
 default
```

#### current_date

`current_date()`：クエリ評価の開始時の現在の日付を戻します。

バージョン 1.5.0 以降

#### current_timestamp

`current_timestamp()`：クエリ評価の開始時の現在のタイムスタンプを戻します。

バージョン 1.5.0 以降

#### now

`now()`：クエリ評価の開始時の現在のタイムスタンプを戻します。

バージョン 1.5.0 以降

### 上位関数 {#higher-order}

#### 変換

`transform(array, lambdaExpression): array`

関数を使用して配列内の要素を変換します。

ラムダ関数に2つの引数がある場合、2番目の引数は要素のインデックスを意味します。

例：

```sql
> SELECT transform(array(1, 2, 3), x -> x + 1);
  [2,3,4]
> SELECT transform(array(1, 2, 3), (x, i) -> x + i);
  [1,3,5]
```


#### 存在する

`exists(array, lambdaExpression returning Boolean): Boolean`

アレイ内の1つ以上の要素に対して述語が保持されているかどうかをテストします。

例：

```sql
> SELECT exists(array(1, 2, 3), x -> x % 2 == 0);
  true
```

#### filter

`filter(array, lambdaExpression returning Boolean): array`

指定した述語を使用して入力配列をフィルタします。

例：

```sql
> SELECT filter(array(1, 2, 3), x -> x % 2 == 1);
 [1,3]
```


#### 集計

`aggregate(array, <initial accumulator value>, lambdaExpression to accumulate the value): array`

初期状態と配列内のすべての要素にバイナリ演算子を適用し、これを単一の状態に減らします。 仕上げ関数を適用して、最終状態を最終結果に変換する。

例：

```sql
> SELECT aggregate(array(1, 2, 3), 0, (acc, x) -> acc + x);
  6
> SELECT aggregate(array(1, 2, 3), 0, (acc, x) -> acc + x, acc -> acc * 10);
  60
```
