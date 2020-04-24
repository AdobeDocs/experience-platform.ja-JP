---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Spark SQL関数
topic: spark sql functions
translation-type: tm+mt
source-git-commit: a23ee02a9e801531a38b5ff70ef07497aa21b174

---


# Spark SQL関数

Spark SQLヘルパーは、SQL機能を拡張するSpark SQLの組み込み関数を提供します。

参照： [Spark SQL関数のドキュメント](https://spark.apache.org/docs/2.4.0/api/sql/index.html)

>[!NOTE] 外部ドキュメント内のすべての機能がサポートされているわけではありません。

## カテゴリ

- [数学および統計の演算子と関数](#math-and-statistical-operators-and-functions)
- [論理演算子](#logical-operators)
- [日付/時間関数](#date/time-functions)
- [集計関数](#aggregate-functions)
- [配列](#arrays)
- [データ型キャスト関数](#datatype-casting-functions)
- [変換関数と書式設定関数](#conversion-and-formatting-functions)
- [データ評価](#data-evaluation)
- [現在の情報](#current-information)

### 数学および統計の演算子と関数

#### 剰余

`expr1 % expr2`:/の後の剰余を返し `expr1`ます`expr2`。

例：

```
> SELECT 2 % 1.8;
 0.2
> SELECT MOD(2, 1.8);
 0.2
```

#### 乗算

`expr1 * expr2`: 戻り値 `expr1`*`expr2`.

例：

```
> SELECT 2 * 3;
 6
```

#### Add

`expr1 + expr2`: 戻り値 `expr1`+`expr2`.

例：

```
> SELECT 1 + 2;
 3
```

#### 減算

`expr1 - expr2`: 戻り値 `expr1`-`expr2`.

例：

```
> SELECT 2 - 1;
 1
```

#### 除算

`expr1 / expr2`: 戻り値 `expr1`/`expr2`. 常に浮動小数点の分割を実行します。

例：

```
> SELECT 3 / 2;
 1.5
> SELECT 2L / 2L;
 1.0
```

#### abs

`abs(expr)`:数値の絶対値を返します。

例：

```
> SELECT abs(-1);
  1
```

#### アコス

`acos(expr)`:の逆コサイン（アークコサインとも呼ばれます）を返します。こ `expr`の逆コサインは、で計算されたものと同じで `java.lang.Math.acos`す。

例：

```
> SELECT acos(1);
 0.0
> SELECT acos(2);
 NaN
```

#### approx_percentile

`approx_percentile(col, percentage [, accuracy])`:指定した割合での数値列の近似百分位 `col` 値を返します。 パーセント値は、0.0 ～ 1.0の範囲で指定する必要があります。パラメ `accuracy` ータ(デフォルト：10000)は正の数値リテラルで、メモリコストで近似精度を制御します。 の値を大きくす `accuracy` ると、精度が向上し `1.0/accuracy` 、近似の相対誤差が発生します。 が配 `percentage` 列の場合、割合配列の各値は0.0 ～ 1.0の範囲である必要があります。この場合、指定した割合の配列における列の概 `col` 算パーセンタイル配列が返されます。

例：

```
> SELECT approx_percentile(10.0, array(0.5, 0.4, 0.1), 100);
 [10.0,10.0,10.0]
> SELECT approx_percentile(10.0, 0.5, 100);
 10.0
```

#### asin

`asin(expr)`:逆サイン（アークサインとも呼ばれます）を返します。この逆サインは、で計算され `expr`たかのように、のアークサインで `java.lang.Math.asin`す。

例：

```
> SELECT asin(0);
 0.0
> SELECT asin(2);
 NaN
```

#### atan

`atan(expr)`:の逆正接（逆正接）を返します。こ `expr`の値は、 `java.lang.Math.atan`

例：

```
> SELECT atan(0);
 0.0
```

#### atan2

`atan2(exprY, exprX)`:平面の正のx軸と座標(`exprX`, `exprY``java.lang.Math.atan2`)で指定された点との間の角度をラジアンで返します。

引数：

`exprY`:Y軸の座標`exprX`:X軸の座標

例：

```
> SELECT atan2(0, 0);
 0.0
```

#### avg

`avg(expr)`:グループの値から計算された平均値を返します。

#### 基数

`cardinality(expr)`:配列またはマップのサイズを返します。 入力がnullで、true（デフォルト）に設定されている場合、 `spark.sql.legacy.sizeOfNull` この関数は —1を返します。 をfalse `spark.sql.legacy.sizeOfNull` に設定すると、null入力の場合はnullを返します。

例：

```
> SELECT cardinality(array('b', 'd', 'c', 'a'));
 4
> SELECT cardinality(map('a', 1, 'b', 2));
 2
> SELECT cardinality(NULL);
 -1
```

#### cbrt

`cbrt(expr)`:のキューブルートを返しま `expr`す。

例：

```
> Select cbrt(27.0);
 3.0
```

#### ceil

`ceil(expr)`:次の値以上の最小の整数を返しま `expr`す。

例：

```
> SELECT ceil(-0.1);
 0
> SELECT ceil(5);
 5
```

#### 天井

`ceiling(expr)`:次の値以上の最小の整数を返しま `expr`す。

例：

```
> SELECT ceiling(-0.1);
 0
> SELECT ceiling(5);
 5
```

#### conv

`conv(num, from_base, to_base)`:次の値 `num` に変換 `from_base` する `to_base`

例：

```
> SELECT conv('100', 2, 10);
 4
> SELECT conv(-10, 16, -10);
 -16
```

#### cor

`corr(expr1, expr2)`:一連の数の対の相関係数を返します。

#### cos

`cos(expr)`:のコサインを返しま `expr``java.lang.Math.cos`す。これは、

例：

```
> SELECT cos(0);
 1.0
```

#### コッシュ

`cosh(expr)`:のハイパボリックコサインを返します。このコサ `expr`インは、で計算された場合と同様に返され `java.lang.Math.cosh`ます。

引数：
- `expr`:ハイパボリック角

例：

```
> SELECT cosh(0);
 1.0
```

#### コット

`cot(expr)`:のコタンジェントを返しま `expr`す。この値は、で計算された場合と同様で `1/java.lang.Math.cot`す。

引数：
- `expr`:角度（ラジアン）

例：

```
> SELECT cot(1);
 0.6420926159343306
```

#### dense_rank

`dense_rank()`:値のグループ内の値のランクを計算します。 結果は、以前に割り当てられたランク値に1を足した値になります。 関数とは異なり、 `rank`はラ `dense_rank` ンキング順序にギャップを生成しません。

#### e

`e()`:オイラー数eを返します。

例：

```
> SELECT e();
 2.718281828459045
```

#### exp

`exp(expr)`:の累乗にeを返します `expr`。

例：

```
> SELECT exp(0);
 1.0
```

#### expml

`expm1(expr)`:exp(`expr`) - 1を返します。

例：

```
> SELECT expm1(0);
 0.0
```

#### 要因

`factorial(expr)`:の階乗を返します `expr`。 `expr` は [0 ～ 20です]。 それ以外の場合はnull。

例：

```
> SELECT factorial(5);
 120
```

#### 床

`floor(expr)`:次の値以下の最大の整数を返しま `expr`す。

例：

```
> SELECT floor(-0.1);
 -1
> SELECT floor(5);
 5
```

#### 最大

`greatest(expr, ...)`:すべてのパラメーターの最大値を返し、null値をスキップします。

例：

```
> SELECT greatest(10, 9, 2, 4, 3);
 10
```

#### 偽善

`hypot(expr1, expr2)`:sqrt(`expr1`<sup>2</sup> + `expr2`<sup>2</sup>)を返します。

例：

```
> SELECT hypot(3, 4);
 5.0
```

#### 尖度

`kurtosis(expr)`:グループの値から計算された尖度の値を返します。


#### least

`least(expr, ...)`:すべてのパラメーターの最小値を返し、null値をスキップします。

例：

```
> SELECT least(10, 9, 2, 4, 3);
 2
```

#### レベンシュテイン

`levenshtein(str1, str2)`:2つの指定した文字列の間のLevenshtein距離を返します。

例：

```
> SELECT levenshtein('kitten', 'sitting');
 3
```

#### ln

`ln(expr)`:の自然対数（底e）を返します `expr`。

例：

```
> SELECT ln(1);
 0.0
```

#### log

`log(base, expr)`:の対数を返し `expr` ます `base`。

例：

```
> SELECT log(10, 100);
 2.0
```

#### log10

`log10(expr)`:10を底とするの `expr` 対数を返します。

例：

```
> SELECT log10(10);
 1.0
```

#### log1p

`log1p(expr)`: 戻り値 `log(1 + expr)`.

例：

```
> SELECT log1p(0);
 0.0
```

#### log2

`log2(expr)`:2を底とするの対数 `expr` を返します。

例：

```
> SELECT log2(2);
 1.0
```

#### max

`max(expr)`:の最大値を返します `expr`。

#### 平均

`mean(expr)`:グループの値から計算された平均値を返します。

#### min

`min(expr)`:の最小値を返します `expr`。

#### 単調増加_id

`monotonically_increasing_id()`:単調に増加する64ビット整数を返します。 生成されるIDは、単調に増加し、一意であることが保証されますが、連続していることはありません。 現在の実装では、パーティションIDが上位31ビットに配置され、下位33ビットは各パーティション内のレコード番号を表します。 データフレームのパーティション数が10億未満で、各パーティションのレコード数が80億未満であると仮定します。 この関数の結果はパーティションIDに依存するので、決定的ではありません。

#### 陰性

`negative(expr)`:の負の値を返します `expr`。

例：

```
> SELECT negative(1);
 -1
```

#### percent_rank

`percent_rank()`:値のグループ内の値のパーセントのランクを計算します。

#### 百分位

`percentile(col, percentage [, frequency])`:指定した割合での数値列の正確なパーセ `col` ンタイル値を返します。 の値は、0.0 `percentage` から1.0の間である必要があります。の値は正の整 `frequency` 数である必要があります。

`percentile(col, array(percentage1 [, percentage2]...) [, frequency])`:渡された割合での数値列の正確なパーセンタ `col` イル値の配列を返します。 パーセント配列の各値は、0.0 ～ 1.0の範囲で指定する必要があります。の値は正の整 `frequency` 数である必要があります。

#### percentile_approx

`percentile_approx(col, percentage [, accuracy])`:指定した割合での数値列の近似百分位 `col` 値を返します。 の値は、0.0 `percentage` から1.0の間である必要があります。パラメ `accuracy` ータ(デフォルト：10000)は正の数値リテラルで、メモリコストで近似精度を制御します。 の値を大きくす `accuracy` ると、精度が向上し `1.0/accuracy` 、近似の相対誤差が発生します。 が配 `percentage` 列の場合、割合配列の各値は0.0 ～ 1.0の範囲である必要があります。この場合、指定した割合の配列での列の近似百分位 `col` の配列を返します。

例：

```
> SELECT percentile_approx(10.0, array(0.5, 0.4, 0.1), 100);
 [10.0,10.0,10.0]
> SELECT percentile_approx(10.0, 0.5, 100);
 10.0
```

#### pi

`pi()`:piを返します。

例：

```
> SELECT pi();
 3.141592653589793
```

#### pmod

`pmod(expr1, expr2)`:modの正の値を返し `expr1` ます `expr2`。

例：

```
> SELECT pmod(10, 3);
 1
> SELECT pmod(-10, 3);
 2
```

#### 陽性

`positive(expr)`:次の正の値を返します。 `expr`

#### ポウ

`pow(expr1, expr2)`:の `expr1` 力を引き上げ `expr2`る

例：

```
> SELECT pow(2, 3);
 8.0
```

#### 電力

`power(expr1, expr2)`:の `expr1` 力を引き上げ `expr2`る

例：

```
> SELECT power(2, 3);
 8.0
```

#### ラジアン

`radians(expr)`:度をラジアンに変換します。

引数：

- `expr`:角度（度単位）

例：

```
> SELECT radians(180);
 3.141592653589793
```

#### ランド

`rand([seed])`:(0, 1)の値を均等に分布した、独立した値（つまり、ID）を持つランダムな値を返します。

例：

```
> SELECT rand();
 0.9629742951434543
> SELECT rand(0);
 0.8446490682263027
> SELECT rand(null);
 0.8446490682263027
```

>[!NOTE] この関数は、一般的には非決定的です。

#### ランドン

`randn([seed])`:標準の正規分布から引き出された、独立した同一の分布（つまり、d）値を持つランダムな値を返します。

例：

```
> SELECT randn();
 -0.3254147983080288
> SELECT randn(0);
 1.1164209726833079
> SELECT randn(null);
 1.1164209726833079
```

>[!NOTE] この関数は、一般的には非決定的です。

#### rint

`rint(expr)`:引数に最も近い重複値を返し、数値の整数と等しい値を返します。

例：

```
> SELECT rint(12.3456);
 12.0
```

#### round

`round(expr, d)`:HALF_UP丸 `expr` めモードを `d` 使用して小数点以下の桁数に丸められた値を返します。

例：

```
> SELECT round(2.5, 0);
 3.0
```

#### sign

`sign(expr)`:-1.0、0.0または1.0を負、0または正 `expr` の数で返します。

例：

```
> SELECT sign(40);
 1.0
```

#### 信号

`signum(expr)`:-1.0、0.0または1.0を負、0または正 `expr` の値として返します。

例：

```
> SELECT signum(40);
 1.0
```

#### 罪

`sin(expr)`:のサインを返しま `expr``java.lang.Math.sin`す。このサインは、

引数：

- `expr`:角度（ラジアン）

例：

```
> SELECT sin(0);
 0.0
```

#### sinh

`sinh(expr)`:のハイパボリックサインを返し `expr``java.lang.Math.sinh`ます。このサインは、

引数：

- `expr`:ハイパボリック角

例：

```
> SELECT sinh(0);
 0.0
```

#### sqrt

`sqrt(expr)`:の平方根を返します `expr`。

例：

```
> SELECT sqrt(4);
 2.0
```

#### stddev

`stddev(expr)`:グループの値から計算したサンプルの標準偏差を返します。

#### stddev_pop

`sttdev_pop(expr)`:グループの値から計算された母集団の標準偏差を返します。

#### stddev_samp

`stddev_samp(expr)`:グループの値から計算したサンプルの標準偏差を返します。

#### sum

`sum(expr)`:グループの値から計算された合計を返します。

#### タン

`tan(expr)`:のタンジェントを返しま `expr``java.lang.Math.tan`す。このタンジェントは、

引数：

- `expr`:角度（ラジアン）

例：

```
> SELECT tan(0);
 0.0
```

#### タン

`tanh(expr)`:のハイパボリックタンジェントを返し `expr`ます。このタンジェントは、で計算された場合と同様で `java.lang.Math.tanh`す。

引数：

- `expr`:ハイパボリック角

例：

```
> SELECT tanh(0);
 0.0
```

#### Var_pop

`var_pop(expr)`:グループの値から計算された母集団の平方偏差を返します。

#### Var_samp

`var_samp(expr)`:グループの値から計算されたサンプルの平方偏差を返します。

#### 分散

`variance(expr)`:グループの値から計算されたサンプルの平方偏差を返します。

### 論理演算子

#### 論理否定

`! expr`:論理否定。

#### より小さい

`expr1 < expr2`:が次の値より小さ `expr1` い場合はtrueを返しま `expr2`す。

引数：

- `expr1, expr2`:2つの式は同じタイプであるか、共通のタイプにキャストでき、注文可能なタイプである必要があります。 例えば、マップタイプは順序指定不可なので、サポートされていません。 配列や構造体などの複合型の場合、フィールドのデータ型は並べ替え可能である必要があります。

例：

```
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

`expr1 <= expr2`:が次よりも小さ `expr1` いか等しい場合はtrueを返しま `expr2`す。

引数：

- `expr1, expr2`:2つの式は、同じタイプであるか、共通のタイプにキャストでき、また、注文可能なタイプである必要があります。 例えば、マップタイプは順序指定不可なので、サポートされていません。 配列や構造体などの複雑な型の場合は、フィールドのデータ型を並べ替え可能にする必要があります。

例：

```
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

`expr1 = expr2`:等しい場合はtrueを返し、そ `expr1` れ以外の `expr2`場合はfalseを返します。

引数：

- `expr1, expr2`:2つの式は同じ型であるか、共通の型にキャストでき、等価比較で使用できる型である必要があります。 マップの種類がサポートされていません。 配列や構造体などの複合型の場合、フィールドのデータ型は並べ替え可能である必要があります。

例：

```
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

`expr1 > expr2`:がより大きい場合 `expr1` はtrueを返しま `expr2`す。

引数：

- `expr1, expr2`:2つの式は同じタイプであるか、共通のタイプにキャストでき、注文可能なタイプである必要があります。 例えば、マップタイプは順序指定不可なので、サポートされていません。 配列や構造体などの複合型の場合、フィールドのデータ型は並べ替え可能である必要があります。

例：

```
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

#### 次よりも大きいか等しい

`expr1 >= expr2`:が次よりも大き `expr1` いか等しい場合はtrueを返しま `expr2`す。

引数：

- `expr1, expr2`:2つの式は同じタイプであるか、共通のタイプにキャストでき、注文可能なタイプである必要があります。 例えば、マップタイプは順序指定不可なので、サポートされていません。 配列や構造体などの複合型の場合、フィールドのデータ型は並べ替え可能である必要があります。

例：

```
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

#### ビット単位で排他的または

`expr1 ^ expr2`:ビット単位の排他的論理和(OR)の結果（およびの結果） `expr1` を返しま `expr2`す。

例：

```
> SELECT 3 ^ 5;
 2
```

#### および

`expr1 and expr2`: 論理積（AND）.

#### arrays_overlap

`arrays_overlap(a1, a2)`:a1にもa2にnull以外の要素が含まれている場合、trueを返します。 配列に共通の要素がなく、両方が空でなく、どちらか一方がnull要素を含む場合、nullが返されます。 それ以外の場合は、falseが返されます。

例：

```
> SELECT arrays_overlap(array(1, 2, 3), array(3, 4, 5));
 true
```

次の期間：2.4.0

#### assert_true

`assert_true(expr)`:がtrueでない場合は例外 `expr` をスローします。

例：

```
> SELECT assert_true(0 < 1);
 NULL
```

#### if

`if(expr1, expr2, expr3)`:評価が `expr1` trueの場合は、次の値を返しま `expr2`す。それ以外の場合は、を返し `expr3`ます。

例：

```
> SELECT if(1 < 2, 'a', 'b');
 a
```

#### ifnull

`ifnull(expr1, expr2)`:nullの場合 `expr2` は `expr1` 返し、それ以外の場合は `expr1` 返します。

例：

```
> SELECT ifnull(NULL, array('2'));
 ["2"]
```

#### in

`expr1 in(expr2, expr3, ...)`:いずれかのvalNに等し `expr` い場合はtrueを返します。

引数：
- `expr1, expr2, expr3, ...`:引数は同じ型である必要があります。

例：

```
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

`isnan(expr)`:がNaNの場合はtrueを返し、それ `expr` 以外の場合はfalseを返します。

例：

```
> SELECT isnan(cast('NaN' as double));
 true
```

#### NULL

`isnotnull(expr)`:nullでない場合はtrue `expr` を、それ以外の場合はfalseを返します。

例：

```
> SELECT isnotnull(1);
 true
```

#### isnull

`isnull(expr)`:nullの場合はtrueを返し、それ `expr` 以外の場合はfalseを返します。

例：

```
> SELECT isnull(1);
 false
```

#### nanvl

`nanvl(expr1, expr2)`:NaNでな `expr1` い場合は返し、それ以外の場合は返 `expr2` します。

例：

```
> SELECT nanvl(cast('NaN' as double), 123);
 123.0
```

#### not

`not expr`:論理否定。

#### または

`expr1 or expr2`:論理和。

#### xpath_boolean

`xpath_boolean(xml, xpath)`:XPathノードがtrueと評価される場合、または一致する式が見つかった場合は、trueを返します。

例：

```
> SELECT xpath_boolean('<a><b>1</b></a>','a/b');
 true
```

### 日付/時間関数

#### add_months

`add_months(start_date, num_months)`:後の日付を返し `num_months` ます `start_date`。

例：

```
> SELECT add_months('2016-08-31', 1);
 2016-09-30
```

次の期間：1.5.0

#### date_add

`date_add(start_date, num_days)`:後の日付を返し `num_days` ます `start_date`。

例：

```
> SELECT date_add('2016-07-30', 1);
 2016-07-31
```

次の期間：1.5.0

#### date_format

`date_format(timestamp, fmt)`:日付 `timestamp` 形式で指定された形式の文字列値に変換します `fmt`。

例：

```
> SELECT date_format('2016-04-08', 'y');
 2016
```

次の期間：1.5.0

#### date_sub

`date_sub(start_date, num_days)`:以前の日付を返し `num_days` ます `start_date`。

例：

```
> SELECT date_sub('2016-07-30', 1);
 2016-07-29
```

次の期間：1.5.0

#### date_trunc

`date_trunc(fmt, ts)`:形式モデルで指定された単位に切り捨てられたタイムスタンプを返しま `fmt`す。 `fmt` は、 [&quot;YEAR&quot;、&quot;YYYY&quot;、&quot;YY&quot;、&quot;MON&quot;、&quot;MM&quot;、&quot;DAY&quot;、&quot;DD&quot;、&quot;HOUR&quot;、&quot;MINUTE&quot;、&quot;SECOND&quot;、&quot;WEEK&quot;、&quot;QUARTER&quot;のいずれかになります。]

例：

```
> SELECT date_trunc('YEAR', '2015-03-05T09:32:05.359');
 2015-01-01 00:00:00
> SELECT date_trunc('MM', '2015-03-05T09:32:05.359');
 2015-03-01 00:00:00
> SELECT date_trunc('DD', '2015-03-05T09:32:05.359');
 2015-03-05 00:00:00
> SELECT date_trunc('HOUR', '2015-03-05T09:32:05.359');
 2015-03-05 09:00:00
```

次の期間：2.3.0

#### datediff

`datediff(endDate, startDate)`:からまでの日数を返し `startDate` ます `endDate`。

例：

```
> SELECT datediff('2009-07-31', '2009-07-30');
 1

> SELECT datediff('2009-07-30', '2009-07-31');
 -1
```

次の期間：1.5.0

#### 日

`day(date)`:日付/タイムスタンプの月の日を返します。

例：

```
> SELECT day('2009-07-30');
 30
```

次の期間：1.5.0

#### dayofmonth

`dayofmonth(date)`:日付/タイムスタンプの月の日を返します。

例：

```
> SELECT dayofmonth('2009-07-30');
 30
```

次の期間：1.5.0

#### dayofweek

`dayofweek(date)`:日付/タイムスタンプの曜日を返します（1 =日曜日、2 =月曜日…、7 =土曜日）。

例：

```
> SELECT dayofweek('2009-07-30');
 5
```

次の期間：2.3.0

#### dayofyear

`dayofyear(date)`:日付/タイムスタンプの年の日付を返します。

例：

```
> SELECT dayofyear('2016-04-09');
 100
```

次の期間：1.5.0

#### from_unixtime

`from_unixtime(unix_time, format)`:指定さ `unix_time` れた値を返しま `format`す。

例：

```
> SELECT from_unixtime(0, 'yyyy-MM-dd HH:mm:ss');
 1970-01-01 00:00:00
```

次の期間：1.5.0

#### from_utc_timestamp

`from_utc_timestamp(timestamp, timezone)`:「2017-07-14 02:40:00.0」のようなタイムスタンプをUTCでの時刻として解釈し、その時刻を指定されたタイムゾーンのタイムスタンプとしてレンダリングします。 例えば、「GMT+1」は「2017-07-14 03:40:00.0」を返します。

例：

```
> SELECT from_utc_timestamp('2016-08-31', 'Asia/Seoul');
 2016-08-31 09:00:00
```

次の期間：1.5.0

#### 時間

`hour(timestamp)`:文字列/タイムスタンプの時間要素を返します。

例：

```
> SELECT hour('2009-07-30 12:58:59');
 12
```

次の期間：1.5.0

#### last_day

`last_day(date):` 日付が属する月の最終日を返します。

例：

```
> SELECT last_day('2009-01-12');
 2009-01-31
```

次の期間：1.5.0

#### 分

`minute(timestamp)`:文字列/タイムスタンプの分の構成要素を返します。

例：

```
> SELECT minute('2009-07-30 12:58:59');
 58
```

次の期間：1.5.0

#### month

`month(date)` 日付/タイムスタンプの月の構成要素を返します。

例：

```
> SELECT month('2016-07-30');
 7
```

次の期間：1.5.0

#### months_between

`months_between(timestamp1, timestamp2[, roundOff])`:がよ `timestamp1` り後の場合、 `timestamp2`結果は正の数になります。 とが `timestamp1` 同 `timestamp2` じ日付の場合、または両方が月の最後の日付の場合、時刻は無視されます。 それ以外の場合、差は1か月に31日分に基づいて計算され、 `roundOff=false`

例：

```
> SELECT months_between('1997-02-28 10:30:00', '1996-10-30');
 3.94959677
> SELECT months_between('1997-02-28 10:30:00', '1996-10-30', false);
 3.9495967741935485
```

次の期間：1.5.0

#### next_day

`next_day(start_date, day_of_week)`:指定された日付より後の最初の日付を `start_date` 返します。

例：

```
> SELECT next_day('2015-01-14', 'TU');
 2015-01-20
```

次の期間：1.5.0

#### 四半期

`quarter(date)`:日付の四半期を1 ～ 4の範囲で返します。

例：

```
> SELECT quarter('2016-08-31');
 3
```

次の期間：1.5.0

#### second

`second(timestamp)`:文字列/タイムスタンプの2番目の要素を返します。

例：

```
> SELECT second('2009-07-30 12:58:59');
 59
```

次の期間：1.5.0

#### to_date

`to_date(date_str[, fmt])`:日付まで `date_str` 式を `fmt` 解析します。 無効な入力を含むnullを返します。 デフォルトでは、を省略した場合、日付へのキャストルールに従 `fmt` っています。

例：

```
> SELECT to_date('2009-07-30 04:17:52');
 2009-07-30
> SELECT to_date('2016-12-31', 'yyyy-MM-dd');
 2016-12-31
```

次の期間：1.5.0

#### to_timestamp

`to_timestamp(timestamp[, fmt])`:タイムスタンプを `timestamp` 持つ式を `fmt` 式で解析します。 無効な入力を含むnullを返します。 デフォルトでは、タイムスタンプを省略した場合、キャストルールに従 `fmt` っています。

例：

```
> SELECT to_timestamp('2016-12-31 00:12:00');
 2016-12-31 00:12:00
> SELECT to_timestamp('2016-12-31', 'yyyy-MM-dd');
 2016-12-31 00:00:00
```

次の期間：2.2.0

#### to_unix_timestamp

`to_unix_timestamp(expr[, pattern])`:指定した時刻のUNIXタイムスタンプを返します。

例：

```
> SELECT to_unix_timestamp('2016-04-08', 'yyyy-MM-dd');
 1460041200
```

次の期間：1.6.0

#### to_utc_timestamp

`to_utc_timestamp(timestamp, timezone)`:「2017-07-14 02:40:00.0」のようなタイムスタンプを指定されたタイムゾーンの時刻として解釈し、その時刻をUTCでのタイムスタンプとしてレンダリングします。 例えば、「GMT+1」は「2017-07-14 01:40:00.0」を返します。

例：

```
> SELECT to_utc_timestamp('2016-08-31', 'Asia/Seoul');
 2016-08-30 15:00:00
```

次の期間：1.5.0

#### 切り詰める

`trunc(date, fmt)`:日付の時間部分が、形式モデルで指定された単位に切り捨てられた日付を返します `fmt`。 `fmt` は、「 [year」、「yyyy」、「yy」、「mon」、「month」、「mm」のいずれかです。]

例：

```
> SELECT trunc('2009-02-12', 'MM');
 2009-02-01
> SELECT trunc('2015-10-27', 'YEAR');
 2015-01-01
```

次の期間：1.5.0

#### unix_timestamp

`unix_timestamp([expr[, pattern]])`:現在または指定された時間のUNIXタイムスタンプを返します。

例：

```
> SELECT unix_timestamp();
 1476884637
> SELECT unix_timestamp('2016-04-08', 'yyyy-MM-dd');
 1460041200
```

次の期間：1.5.0

#### 平日

`weekday(date)`:日付/タイムスタンプの曜日を返します（0 =月曜日、1 =火曜日、...、6 =日曜日）。

例：

```
> SELECT weekday('2009-07-30');
 3
```

次の期間：2.4.0

#### week_of_year

`weekofyear(date)`:指定した日付の年の週を返します。 週は月曜日の開始と見なされ、1週目は3日を超える最初の週です。

例：

```
> SELECT weekofyear('2008-02-20');
 8
```

次の期間：1.5.0

#### when

`CASE WHEN expr1 THEN expr2 [WHEN expr3 THEN expr4]* [ELSE expr5] END`:When `expr1` = true, returns `expr2`;else when `expr3` = trueの場合、返 `expr4`す；elseは、を返しま `expr5`す。

引数：

- `expr1`, `expr3`:ブランチ条件の式は、すべてBoolean型にする必要があります。
- `expr2`, `expr4`, `expr5`:ブランチ値の式とelse値の式は、すべて同じ型であるか、共通型に強制可能である必要があります。

例：

```
> SELECT CASE WHEN 1 > 0 THEN 1 WHEN 2 > 0 THEN 2.0 ELSE 1.2 END;
 1
> SELECT CASE WHEN 1 < 0 THEN 1 WHEN 2 > 0 THEN 2.0 ELSE 1.2 END;
 2
> SELECT CASE WHEN 1 < 0 THEN 1 WHEN 2 < 0 THEN 2.0 END;
 NULL
```

#### year

`year(date)`:日付/タイムスタンプの年の構成要素を返します。

例：

```
> SELECT year('2016-07-30');
 2016
```

次の期間：1.5.0

### 集計関数

#### approx_count_distinct

`approx_count_distinct(expr[, relativeSD])`:HyperLogLog++による推定基数を返します。 `relativeSD` 許可される最大推定エラーを定義します。

### 配列

#### 配列

`array(expr, ...)`:渡された要素を持つ配列を返します。

例：

```
> SELECT array(1, 2, 3);
 [1,2,3]
```

#### array_contains

`array_contains(array, value)`:配列に値が含まれる場合はtrueを返します。

例：

```
> SELECT array_contains(array(1, 2, 3), 2);
 true
```

#### array_distinct

`array_distinct(array)`:配列から重複値を削除します。

例：

```
> SELECT array_distinct(array(1, 2, 3, null, 3));
 [1,2,3,null]
```

次の期間：2.4.0

#### array_except

`array_except(array1, array2)`:の要素の配列を返しますが、の要素は含ま `array1` れません( `array2`重複なし)。

例：

```
> SELECT array_except(array(1, 2, 3), array(1, 3, 5));
 [2]
```

次の期間：2.4.0

#### array_intersect

`array_intersect(array1, array2)`:との交点にある要素の配列を返します。 `array1` 要素 `array2`は含まれません。

例：

```
> SELECT array_intersect(array(1, 2, 3), array(1, 3, 5));
 [1,3]
```

次の期間：2.4.0

#### array_join

`array_join(array, delimiter[, nullReplacement])`:指定した配列の要素を区切り文字とオプションの文字列を使用して連結し、NULLを置き換えます。 に値が設定されていない場合、 `nullReplacement`null値はすべてフィルタされます。

例：

```
> SELECT array_join(array('hello', 'world'), ' ');
 hello world
> SELECT array_join(array('hello', null ,'world'), ' ');
 hello world
> SELECT array_join(array('hello', null ,'world'), ' ', ',');
 hello , world
```

次の期間：2.4.0

#### array_max

`array_max(array)`:配列の最大値を返します。 Null要素はスキップされます。

例：

```
> SELECT array_max(array(1, 20, null, 3));
 20
```

次の期間：2.4.0

#### array_min

`array_min(array)`:配列の最小値を返します。 Null要素はスキップされます。

例：

```
> SELECT array_min(array(1, 20, null, 3));
 1
```

次の期間：2.4.0

#### array_position

`array_position(array, element)`:配列の最初の要素の（1から始まる）インデックスを長整数型で返します。

例：

```
> SELECT array_position(array(3, 2, 1), 1);
 3
```

次の期間：2.4.0

#### array_remove

`array_remove(array, element)`:要素と等しい要素をすべて配列から削除します。

例：

```
> SELECT array_remove(array(1, 2, 3, null, 3), 3);
 [1,2,null]
```

次の期間：2.4.0

#### array_repeat

`array_repeat(element, count)`:要素のカウント時間を含む配列を返します。

例：

```
> SELECT array_repeat('123', 2);
 ["123","123"]
```

次の期間：2.4.0

#### array_sort

`array_sort(array)`:入力配列を昇順に並べ替えます。 入力配列の要素は、順序が有効である必要があります。 null要素は、返された配列の末尾に配置されます。

例：

```
> SELECT array_sort(array('b', 'd', null, 'c', 'a'));
 ["a","b","c","d",null]
```

次の期間：2.4.0

#### array_和集合

`array_union(array1, array2)`:との和集合内の要素の配列を返します( `array1` 重複な `array2`し)。

例：

```
> SELECT array_union(array(1, 2, 3), array(1, 3, 5));
 [1,2,3,5]
```

次の期間：2.4.0

#### array_zip

`arrays_zip(a1, a2, ...)`:N番目の構造体に入力配列のN番目の値がすべて含まれている、構造体の結合配列を返します。

例：

```
> SELECT arrays_zip(array(1, 2, 3), array(2, 3, 4));
 [{"0":1,"1":2},{"0":2,"1":3},{"0":3,"1":4}]
> SELECT arrays_zip(array(1, 2), array(2, 3), array(3, 4));
 [{"0":1,"1":2,"2":3},{"0":2,"1":3,"2":4}]
```

次の期間：2.4.0

#### element_at

`element_at(array, index)`:渡された（1から始まる）インデックスの配列の要素を返します。 の場合 `index < 0`は、最後から最初の要素にアクセスします。 インデックスが配列の長さを超える場合はNULLを返します。

`element_at(map, key)`:渡されたキーの値を返します。キーがマップに含まれていない場合はNULLを返します。

例：

```
> SELECT element_at(array(1, 2, 3), 2);
 2
> SELECT element_at(map(1, 'a', 2, 'b'), 2);
 b
```

次の期間：2.4.0

#### 爆発する

`explode(expr)`:配列の要素を複数の行 `expr` に分割するか、マップの要素を複数の行 `expr` と列に分割します。

例：

```
> SELECT explode(array(10, 20));
 10
 20
```

#### explode_outer

`explode_outer(expr)`:配列の要素を複数の行 `expr` に分割するか、マップの要素を複数の行 `expr` と列に分割します。

例：

```
> SELECT explode_outer(array(10, 20));
 10
 20
```

#### find_in_set

`find_in_set(str, str_array)`:渡された文字列()のインデックス（1から始まる）を`str`、カンマで区切ったリスト(`str_array`)で返します。 文字列が見つからなかった場合、または指定した文字列(`str`)にコンマが含まれている場合は0を返します。

例：

```
> SELECT find_in_set('ab','abc,b,ab,c,def');
 3
```

#### 平らにする

`flatten(arrayOfArrays)`:配列の配列を単一の配列に変換します。

例：

```
> SELECT flatten(array(array(1, 2), array(3, 4)));
 [1,2,3,4]
```

次の期間：2.4.0

#### inline

`inline(expr)`:構造体の配列をテーブルに分解します。

例：

```
> SELECT inline(array(struct(1, 'a'), struct(2, 'b')));
 1  a
 2  b
```

#### inline_outer

`inline_outer(expr)`:構造体の配列をテーブルに分解します。

例：

```
> SELECT inline_outer(array(struct(1, 'a'), struct(2, 'b')));
 1  a
 2  b
```

#### 足踏み

`posexplode(expr)`:配列の要素を、位置を持 `expr` つ複数の行に分割するか、または位置を持つ複数の行と `expr` 列にマップの要素を分割します。

例：

```
> SELECT posexplode(array(10,20));
 0  10
 1  20
```

#### posexplode_outer

`posexplode_outer(expr)`:配列の要素を、位置を持 `expr` つ複数の行に分割するか、または位置を持つ複数の行と `expr` 列にマップの要素を分割します。

例：

```
> SELECT posexplode_outer(array(10,20));
 0  10
 1  20
```

#### reverse

`reverse(array)`:逆順の文字列、または要素の順序が逆の配列を返します。

例：

```
> SELECT reverse('Spark SQL');
 LQS krapS
> SELECT reverse(array(2, 1, 4, 3));
 [3,4,1,2]
```

次の期間：1.5.0
>[!NOTE] アレイの論理は2.4.0以降で使用可能です。

#### シャッフル

`shuffle(array)`:渡された配列のランダムな順列を返します。

例：

```
> SELECT shuffle(array(1, 20, 3, 5));
 [3,1,5,20]
> SELECT shuffle(array(1, 20, null, 3));
 [20,null,3,1]
```

次の期間：2.4.0
>[!NOTE] 関数が決定的でない場合。

#### スライス

`slice(x, start, length)`:指定した長さのインデックス開始(開始が負の場合は最後から開始)から始まるサブセット配列x。

例：

```
> SELECT slice(array(1, 2, 3, 4), 2, 2);
 [2,3]
> SELECT slice(array(1, 2, 3, 4), -2, 2);
 [3,4]
```

次の期間：2.4.0

#### sort_array

`sort_array(array[, ascendingOrder])`:入力配列を、配列要素の自然な順序に従って昇順または降順に並べ替えます。 null要素は、返された配列の昇順または降順で返された配列の最後に配置されます。

例：

```
> SELECT sort_array(array('b', 'd', null, 'c', 'a'), true);
 [null,"a","b","c","d"]
```

#### zip_with

`zip_with(left, right, func)`:関数を使用して、2つの要素ごとの配列を1つの配列に結合します。 1つの配列が短い場合は、関数を適用する前に、長い配列の長さに合わせて末尾にnullが追加されます。

例：

```
> SELECT zip_with(array(1, 2, 3), array('a', 'b', 'c'), (x, y) -> (y, x));
 [{"y":"a","x":1},{"y":"b","x":2},{"y":"c","x":3}]
> SELECT zip_with(array(1, 2), array(3, 4), (x, y) -> x + y);
 [4,6]
> SELECT zip_with(array('a', 'b', 'c'), array('d', 'e', 'f'), (x, y) -> concat(x, y));
 ["ad","be","cf"]
```

次の期間：2.4.0

### データ型キャスト関数

#### bigint

`bigint(expr)`:値をデータ型 `expr` ターゲットにキャストしま `bigint`す。

#### バイナリ

`binary(expr)`:値をデータ型 `expr` ターゲットにキャストしま `binary`す。

#### ブール型

`boolean(expr)`:値をデータ型 `expr` ターゲットにキャストしま `boolean`す。

#### 鋳造

`cast(expr AS type)`:値をデータ型 `expr` ターゲットにキャストしま `type`す。

例：

```
> SELECT cast('10' as int);
 10
```

#### date

`date(expr)`:値をデータ型 `expr` ターゲットにキャストしま `date`す。

#### decimal

`decimal(expr)`:値をデータ型 `expr` ターゲットにキャストしま `decimal`す。

#### 重複

`double(expr)`:値をデータ型 `expr` ターゲットにキャストしま `double`す。

#### float

`float(expr)`:値をデータ型 `expr` ターゲットにキャストしま `float`す。

#### int

`int(expr)`:値をデータ型 `expr` ターゲットにキャストしま `int`す。

#### map

`map(key0, value0, key1, value1, ...)`:指定したキーと値のペアを持つマップを作成します。

例：

```
> SELECT map(1.0, '2', 3.0, '4');
 {1.0:"2",3.0:"4"}
```

#### smallint

`smallint(expr)`:値をデータ型 `expr` ターゲットにキャストしま `smallint`す。

#### str_to_map

`str_to_map(text[, pairDelim[, keyValueDelim]])`:区切り文字を使用してテキストをキー/値のペアに分割した後に、マップを作成します。 デフォルトの区切り文字は、「,」（の場合） `pairDelim` および「:」（の場合）で `keyValueDelim`す。

例：

```
> SELECT str_to_map('a:1,b:2,c:3', ',', ':');
 map("a":"1","b":"2","c":"3")
> SELECT str_to_map('a');
 map("a":null)
```

#### 文字列

`string(expr)`:値をデータ型 `expr` ターゲットにキャストしま `string`す。

#### struct

`struct(col1, col2, col3, ...)`:渡されたフィールド値を持つ構造体を作成します。

#### tinyint

`tinyint(expr)`:値をデータ型 `expr` ターゲットにキャストしま `tinyint`す。

### 変換関数と書式設定関数

#### ascii

`ascii(str)`:の最初の文字の数値を返します `str`。

例：

```
> SELECT ascii('222');
 50
> SELECT ascii(2);
 50
```

#### base64

`base64(bin)`:引数をバイナリからベース64文 `bin` 字列に変換します。

例：

```
> SELECT base64('Spark SQL');
 U3BhcmsgU1FM
```

#### bin

`bin(expr)`:バイナリ形式で表されたlong値の文字列 `expr` 表現を返します。

例：

```
> SELECT bin(13);
 1101
> SELECT bin(-13);
 1111111111111111111111111111111111111111111111111111111111110011
> SELECT bin(13.3);
 1101
```

#### bit_length

`bit_length(expr)`:文字列データのビット長またはバイナリデータのビット数を返します。

例：

```
> SELECT bit_length('Spark SQL');
 72
```

#### char

`char(expr)`:と同等のバイナリを持つASCII文字を返しま `expr`す。 nが256より大きい場合、結果はと同じになります `chr(n % 256)`。

例：

```
> SELECT char(65);
 A
```

#### char_length

`char_length(expr)`:文字列データの文字長またはバイナリデータのバイト数を返します。 文字列データの長さの末尾に空白文字が含まれます。 バイナリデータの長さには、バイナリゼロが含まれます。

例：

```
> SELECT char_length('Spark SQL ');
 10
> SELECT CHAR_LENGTH('Spark SQL ');
 10
> SELECT CHARACTER_LENGTH('Spark SQL ');
 10
```

#### character_length

`character_length(expr)`:文字列データの文字長またはバイナリデータのバイト数を返します。 文字列データの長さの末尾に空白文字が含まれます。 バイナリデータの長さには、バイナリゼロが含まれます。

例：

```
> SELECT character_length('Spark SQL ');
 10
> SELECT CHAR_LENGTH('Spark SQL ');
 10
> SELECT CHARACTER_LENGTH('Spark SQL ');
 10
```

#### chr

`chr(expr)`:exprに相当するバイナリを持つASCII文字を返します。 nが256より大きい場合、結果は `chr(n % 256)`

例：

```
> SELECT chr(65);
 A
```

#### 度

`degrees(expr)`:ラジアンを度に変換します。

引数：
- `expr`:角度（ラジアン）

例：

```
> SELECT degrees(3.141592653589793);
 180.0
```

#### format_number

`format_number(expr1, expr2)`:「#,###,#### `expr1` 」のような数値の形式を設定します。##&#39;を小数点以下の桁数に丸 `expr2` めます。 が0の `expr2` 場合、結果には小数点や小数部分は含まれません。 `expr2` は、ユーザー指定の形式も受け入れます。 これは、MySQLと同様に機能することを目的としていま `FORMAT`す。

例：

```
> SELECT format_number(12332.123456, 4);
 12,332.1235
> SELECT format_number(12332.123456, '##################.###');
 12332.123
```

#### from_json

`from_json(jsonStr, schema[, options])`:渡されたと共にstruct値を返し `jsonStr` ます `schema`。

例：

```
> SELECT from_json('{"a":1, "b":0.8}', 'a INT, b DOUBLE');
 {"a":1, "b":0.8}
> SELECT from_json('{"time":"26/08/2015"}', 'time Timestamp', map('timestampFormat', 'dd/MM/yyyy'));
 {"time":"2015-08-26 00:00:00.0"}
```

次の期間：2.2.0

#### ハッシュ

`hash(expr1, expr2, ...)`:引数のハッシュ値を返します。

例：

```
> SELECT hash('Spark', array(123), 2);
 -1321691492
```

#### 16進

`hex(expr)`:16進数に `expr` 変換します。

例：

```
> SELECT hex(17);
 11
> SELECT hex('Spark SQL');
 537061726B2053514C
```

#### initcap

`initcap(str)`:各単語 `str` の最初の文字を大文字で返します。 その他の文字はすべて小文字で表されます。 単語は空白で区切られます。

例：

```
> SELECT initcap('sPark sql');
 Spark Sql
```

#### lcase

`lcase(str)`:すべての `str` 文字を小文字に変更して返します。

例：

```
> SELECT lcase('SparkSql');
 sparksql
```

#### lower

`lower(str)`:すべての `str` 文字を小文字に変更して返します。

例：

```
> SELECT lower('SparkSql');
 sparksql
```

#### lpad

`lpad(str, len, pad)`:の長 `str`さの左パディ `pad` ングを返します `len`。 がより `str` 長い場合、 `len`戻り値は文字に短縮され `len` ます。

例：

```
> SELECT lpad('hi', 5, '??');
 ???hi
> SELECT lpad('hi', 1, '??');
 h
```

#### map

`map(key0, value0, key1, value1, ...)`:指定したキーと値のペアを持つマップを作成します。

例：

```
> SELECT map(1.0, '2', 3.0, '4');
 {1.0:"2",3.0:"4"}
```

#### map_from_arrays

`map_from_arrays(keys, values)`:指定されたキー/値の配列のペアを持つマップを作成します。 キー内の要素はNULLにできません。

例：

```
> SELECT map_from_arrays(array(1.0, 3.0), array('2', '4'));
 {1.0:"2",3.0:"4"}
```

次の期間：2.4.0

#### map_from_entries

`map_from_entries(arrayOfEntries)`:渡されたエントリの配列から作成されたマップを返します。

例：

```
> SELECT map_from_entries(array(struct(1, 'a'), struct(2, 'b')));
 {1:"a",2:"b"}
```

次の期間：2.4.0

#### md5

`md5(expr)`:MD5 128ビットのチェックサムを16進文字列で返します `expr`。

例：

```
> SELECT md5('Spark');
 8cde774d6f7333752ed72cacddb05126
```

#### rpad

`rpad(str, len, pad)`:の長 `str`さの右パッドを `pad` 返します `len`。 がより `str` 長い場合、 `len`戻り値は文字に短縮され `len` ます。

例：

```
> SELECT rpad('hi', 5, '??');
 hi???
> SELECT rpad('hi', 1, '??');
 h
```

#### rtrim

`rtrim(str)`:末尾の空白文字をから削除しま `str`す。

`rtrim(trimStr, str)`:末尾の文字列を削除します。この文字列には、 `str`

引数：
- `str`:文字列式
- `trimStr`:トリミングするトリミング文字列文字。 デフォルト値は1つのスペースです

例：

```
> SELECT rtrim('    SparkSQL   ');
 SparkSQL
> SELECT rtrim('LQSa', 'SSparkSQLS');
 SSpark
```

#### sha

`sha(expr)`:の16進 `sha1` 数文字列としてハッシュ値を返します `expr`。

例：

```
> SELECT sha('Spark');
 85f5955f4b27a9a4c2aab6ffe5d7189fc298b92c
```

#### sha1

`sha1(expr)`:の16進 `sha1` 数文字列としてハッシュ値を返します `expr`。

例：

```
> SELECT sha1('Spark');
 85f5955f4b27a9a4c2aab6ffe5d7189fc298b92c
```

#### sha2

`sha2(expr, bitLength)`:SHA-2ファミリのチェックサムを16進文字列で返します `expr`。 SHA-224、SHA-256、SHA-384およびSHA-512がサポートされています。 ビット長が0の場合は256と等しくなります。

例：

```
> SELECT sha2('Spark', 256);
 529bc3b07127ecb7e53a4dcf1991d9152c24537d919178022b2c42657f79a26b
```

#### soundex

`soundex(str)`:文字列のSoundexコードを返します。

例：

```
> SELECT soundex('Miller');
 M460
```

#### 積み重ね

`stack(n, expr1, ..., exprk)`:... `expr1`を行に分割 `exprk` しま `n` す。

例：

```
> SELECT stack(2, 1, 2, 3);
 1  2
 3  NULL
```

#### substr

`substr(str, pos[, len])`:開始の長さと長さのサ `str` ブ文字列、ま `pos` たは開始が長で `len`あるバイト配列のスライスを返 `pos` します `len`。

例：

```
> SELECT substr('Spark SQL', 5);
 k SQL
> SELECT substr('Spark SQL', -3);
 SQL
> SELECT substr('Spark SQL', 5, 1);
 k
```

#### substring

`substring(str, pos[, len])`:開始の長さと長さのサ `str` ブ文字列、ま `pos` たは開始が長で `len`あるバイト配列のスライスを返 `pos` します `len`。

例：

```
> SELECT substring('Spark SQL', 5);
 k SQL
> SELECT substring('Spark SQL', -3);
 SQL
> SELECT substring('Spark SQL', 5, 1);
 k
```

#### to_json

`to_json(expr[, options])`:渡された構造体値を持つJSON文字列を返します。

例：

```
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

次の期間：2.2.0

#### translate

`translate(input, from, to)`:文字列内 `input` の文字を文字列内の対応す `from` る文字で置き換えて、文字列を変換し `to` ます。

例：

```
> SELECT translate('AaBbCc', 'abc', '123');
 A1B2C3
```

#### trim

`trim(str)`:先頭と末尾の空白文字を削除しま `str`す。

`trim(BOTH trimStr FROM str)`:の先頭と末尾の文字を `trimStr` 削除しま `str`す。

`trim(LEADING trimStr FROM str)`:の先頭文字を削 `trimStr` 除します `str`。

`trim(TRAILING trimStr FROM str)`:から末尾の文字を `trimStr` 削除しま `str`す。

引数：
- `str`:文字列式
- `trimStr`:トリミングするトリミング文字列文字。デフォルト値は1スペースです
- `BOTH`, `FROM`:文字列の両端から文字列を切り抜くことを指定するキーワードです。
- `LEADING`, `FROM`:文字列の左端から文字列の文字を切り抜くことを指定するキーワードです
- `TRAILING`, `FROM`:文字列の右端から文字列の文字を切り抜くことを指定するキーワードです。

例：

```
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

`ucase(str)`:すべての `str` 文字を大文字に変更して返します。

例：

```
> SELECT ucase('SparkSql');
 SPARKSQL
```

#### unbase64

`unbase64(str)`:引数をベース64文字列からバイナリに `str` 変換します。

例：

```
> SELECT unbase64('U3BhcmsgU1FM');
 Spark SQL
```

#### 16進法の

`unhex(expr)`:16進数を2進数 `expr` に変換します。

例：

```
> SELECT decode(unhex('537061726B2053514C'), 'UTF-8');
 Spark SQL
```

#### upper

`upper(str)`:すべての `str` 文字を大文字に変更して返します。

例：

```
> SELECT upper('SparkSql');
 SPARKSQL
```

#### uuid

`uuid()`:UUID(Universally Unique Identifier)文字列を返します。 値は、標準のUUID 36文字の文字列として返されます。

例：

```
> SELECT uuid();
 46707d92-02f4-4817-8116-a4c3b23e6266
```

>[!NOTE] 関数が決定的ではありません。

### データ評価

#### 合体

`coalesce(expr1, expr2, ...)`:最初のnull以外の引数が存在する場合は、それを返します。 それ以外の場合はnull。

例：

```
> SELECT coalesce(NULL, 1, NULL);
 1
```

#### collect_リスト

`collect_list(expr)`:一意でない要素のリストを収集し、返します。

#### collect_set

`collect_set(expr)`:一意の要素のセットを収集して返します。

#### concat

`concat(col1, col2, ..., colN)`:col1、col2、...、colNを連結して返します。

例：

```
> SELECT concat('Spark', 'SQL');
 SparkSQL
> SELECT concat(array(1, 2, 3), array(4, 5), array(6));
 [1,2,3,4,5,6]
```

>[!NOTE] 配 `concat` 列のロジックは2.4.0以降で使用可能です。

#### concat_ws

`concat_ws(sep, [str | array(str)]+)`:で区切られた文字列を連結して返しま `sep`す。

例：

```
> SELECT concat_ws(' ', 'Spark', 'SQL');
  Spark SQL
```

#### count

`count(*)`:取得した行の総数（nullを含む行も含む）を返します。

`count(expr[, expr...])`:指定された行の数を返します。この行の式はすべてnull以外です。

`count(DISTINCT expr[, expr...])`:指定された行のうち、指定された式が一意で、null以外の行数を返します。

#### crc32

`crc32(expr)`:の巡回冗長チェック値をbigintとして `expr` 返します。

例：

```
> SELECT crc32('Spark');
 1557323817
```

#### デコード

`decode(bin, charset)`:2番目の引数文字セットを使用して、最初の引数をデコードします。

例：

```
> SELECT decode(encode('abc', 'utf-8'), 'utf-8');
 abc
```

#### elt

`elt(n, input1, input2, ...)`:次の入 `n`力を返します。例えば、が2の場合 `input2` に返 `n` されます。

例：

```
> SELECT elt(1, 'scala', 'java');
 scala
```

#### エンコード

`encode(str, charset)`:2番目の引数文字セットを使用して、最初の引数をエンコードします。

例：

```
> SELECT encode('abc', 'utf-8');
 abc
```

#### first

`first(expr[, isIgnoreNull])`:行のグループの最初 `expr` の値を返します。 がtrueの `isIgnoreNull` 場合、null以外の値のみを返します。

#### first_value

`first_value(expr[, isIgnoreNull])`:行のグループの最初 `expr` の値を返します。 がtrueの `isIgnoreNull` 場合、null以外の値のみを返します。

#### get_json_object

`get_json_object(json_txt, path)`:からjsonオブジェクトを抽出しま `path`す。

例：

```
> SELECT get_json_object('{"a":"b"}', '$.a');
 b
```

#### グループ化

<!-- was blank --->

#### grouping_id

<!-- was blank --->

#### instr

`instr(str, substr)`:inの1番目の値の（1から始まる）インデックスを返し `substr` ます `str`。

例：

```
> SELECT instr('SparkSQL', 'SQL');
 6
```

#### json_tuple

`json_tuple(jsonStr, p1, p2, ..., pn)`:関数と同じタプルを返しますが、 `get_json_object`複数の名前を取ります。 すべての入力パラメーターと出力列のタイプは文字列です。

例：

```
> SELECT json_tuple('{"a":1, "b":2}', 'a', 'b');
 1  2
```

#### 遅れ

`lag(input[, offset[, default]])`:ウィンドウ内の現 `input` 在の行 `offset`の前の行の値を返します。 のデフォルト値 `offset` は1で、のデフォルト値は `default` nullです。 1行目の値がnull `input` の場 `offset`合、nullが返されます。 このようなオフセット行がない場合（例えば、オフセットが1の場合、ウィンドウの最初の行には前の行が含まれません）、が返 `default` されます。

#### last

`last(expr[, isIgnoreNull])`:行のグループの最後 `expr` の値を返します。 がtrueの `isIgnoreNull` 場合、null以外の値のみを返します。

#### last_value

`last_value(expr[, isIgnoreNull])`:行のグループの最後 `expr` の値を返します。 がtrueの `isIgnoreNull` 場合、null以外の値のみを返します。

#### 鉛

`lead(input[, offset[, default]])`:ウィンドウ内の現 `input` 在の `offset`行の後の行の値を返します。 のデフォルト値 `offset` は1で、のデフォルト値は `default` nullです。 1行目の値がnull `input` の場 `offset`合、nullが返されます。 オフセット行がない場合（例えば、オフセットが1の場合、ウィンドウの最後の行に後続の行がない場合）、が返さ `default` れます。


#### left

`left(str, len)`:文字列の左端( `len` 文字`len` 列タイプの場合もあります)を返します `str`。 が0以 `len` 下の場合、結果は空の文字列になります。

例：

> SELECT left(&#39;Spark SQL&#39;, 3);スパ


#### length

`length(expr)`:文字列データの文字長またはバイナリデータのバイト数を返します。 文字列データの長さの末尾に空白文字が含まれます。 バイナリデータの長さには、バイナリゼロが含まれます。

例：

```
> SELECT length('Spark SQL ');
 10
> SELECT CHAR_LENGTH('Spark SQL ');
 10
> SELECT CHARACTER_LENGTH('Spark SQL ');
 10
```

#### 検索

`locate(substr, str[, pos])`:inの最初の値の後の位置を `substr` 返 `str` します `pos`。 渡された値 `pos` と戻り値は1から始まります。

例：

```
> SELECT locate('bar', 'foobarbar');
 4
> SELECT locate('bar', 'foobarbar', 5);
 7
> SELECT POSITION('bar' IN 'foobarbar');
 4
```

#### map_concat

`map_concat(map, ...)`:指定されたすべての和集合のマップを返します。

例：

```
> SELECT map_concat(map(1, 'a', 2, 'b'), map(2, 'c', 3, 'd'));
 {1:"a",2:"c",3:"d"}
```

次の期間：2.4.0

#### map_keys

`map_keys(map)`:マップのキーを含む、順序のない配列を返します。

例：

```
> SELECT map_keys(map(1, 'a', 2, 'b'));
 [1,2]
```

#### map_values

`map_values(map)`:マップの値を含む、順不同の配列を返します。

例：

```
> SELECT map_values(map(1, 'a', 2, 'b'));
 ["a","b"]
```

#### 全く

`ntile(n)`:各ウィンドウパーティションの行を、1 ～ `n` 1以上の範囲のグループに分割しま `n`す。

#### 無効

`nullif(expr1, expr2)`:次に等しい場合はnullを `expr1` 返し、それ以外の `expr2`場合はnullを返 `expr1` します。

例：

```
> SELECT nullif(2, 2);
 NULL
```

#### nvl

`nvl(expr1, expr2)`:nullの場合 `expr2` は `expr1` 返し、それ以外の場合は `expr1` 返します。

例：

```
> SELECT nvl(NULL, array('2'));
 ["2"]
```

#### nvl2

`nvl2(expr1, expr2, expr3)`:nullでな `expr2` い場 `expr1` 合は返し、それ以外の場合は `expr3` 返します。

例：

```
> SELECT nvl2(NULL, 2, 1);
 1
```

#### parse_url

`parse_url(url, partToExtract[, key])`:URLから部分を抽出します。

例：

```
> SELECT parse_url('http://spark.apache.org/path?query=1', 'HOST')
 spark.apache.org
> SELECT parse_url('http://spark.apache.org/path?query=1', 'QUERY')
 query=1
> SELECT parse_url('http://spark.apache.org/path?query=1', 'QUERY', 'query')
 1
```

#### position

`position(substr, str[, pos])`:inの最初の値の後の位置を `substr` 返 `str` します `pos`。 渡された値 `pos` と戻り値は1から始まります。

例：

```
> SELECT position('bar', 'foobarbar');
 4
> SELECT position('bar', 'foobarbar', 5);
 7
> SELECT POSITION('bar' IN 'foobarbar');
 4
```

#### ランク

`rank()`:値のグループ内の値のランクを計算します。 結果は、パーティションの順序で現在の行の前または同じ行の数に1を加えた値になります。 この値は、シーケンス内でギャップを生成します。

#### regexp_extract

`regexp_extract(str, regexp[, idx])`:一致するグループを抽出しま `regexp`す。

例：

```
> SELECT regexp_extract('100-200', '(\\d+)-(\\d+)', 1);
 100
```

#### regex_replace

`regexp_replace(str, regexp, rep)`:に一致するすべてのサブ文字列 `str` をに置き換 `regexp` えます `rep`。

例：

```
> SELECT regexp_replace('100-200', '(\\d+)', 'num');
 num-num
```

#### repeat

`repeat(str, n)`:渡された文字列値をn回繰り返す文字列を返します。

例：

```
> SELECT repeat('123', 2);
 123123
```

#### replace

`replace(str, search[, replace])`:すべてののをに置き換 `search` えます `replace`。

引数：
- `str`:文字列式
- `search`:文字列式。 がに見 `search` つからない場合、変 `str`更なし `str` で返されます。
- `replace`:文字列式。 が指 `replace` 定されていない場合、または空の文字列の場合、削除された文字列は何も置き換えられませ `str`ん。

例：

```
> SELECT replace('ABCabc', 'abc', 'DEF');
 ABCDEF
```

#### ロールアップ

<!-- was blank -->

#### row_number

`row_number()`:ウィンドウ・パーティション内の行の順序に従って、各行に1から始まる一意の順番を割り当てます。

#### スキーマ_of_json

`schema_of_json(json[, options])`:スキーマをJSON文字列のDDL形式で返します。

例：

```
> SELECT schema_of_json('[{"col":0}]');
 array<struct<col:int>>
```

次の期間：2.4.0

#### 文

`sentences(str[, lang, country])`:単語 `str` の配列に分割します。

例：

```
> SELECT sentences('Hi there! Good morning.');
 [["Hi","there"],["Good","morning"]]
```

#### シーケンス

`sequence(start, stop, step)`:停止する要素の配列(開始を含む)を生成し、ステップごとに増分します。 返される要素の型は、引数の要素の型と同じです。式

次のタイプがサポートされています。byte、short、integer、long、date、timestamp。

との解 `start` 決は、 `stop` 式と同じタイプに解決する必要があります。 および `start``stop` 式が「日付」または「タイムスタンプ」タイプに解決される場合、 `step` 式は「間隔」タイプに解決される必要があります。それ以外の場合は、とと同じ型に解決さ `start` れ `stop` ます。

引数：
- `start`:式。 範囲の開始。
- `stop`:式。 範囲の終了（範囲を含む）。
- `step`:オプションの式。 範囲のステップ。 の値が次の値 `step` 以下の場合は `start` デフォルトで1、それ以外の場合は `stop`-1です。 時間シーケンスの場合、それぞれ1日と —1日です。 が次の値 `start` より大きい場 `stop`合は負の値に `step` 設定し、逆の場合も同様に設定します。

例：

```
> SELECT sequence(1, 5);
 [1,2,3,4,5]
> SELECT sequence(5, 1);
 [5,4,3,2,1]
> SELECT sequence(to_date('2018-01-01'), to_date('2018-03-01'), interval 1 month);
 [2018-01-01,2018-02-01,2018-03-01]
```

次の期間：2.4.0

#### 左へ

`shiftleft(base, expr)`:ビット単位の左シフト。

例：

```
> SELECT shiftleft(2, 1);
 4
```

#### 震え

`shiftright(base, expr)`:ビット単位（符号付き）の右シフト。

例：

```
> SELECT shiftright(4, 1);
 2
```

#### shiftrightunsigned

`shiftrightunsigned(base, expr)`:ビット単位の符号なし右シフト。

例：

```
> SELECT shiftrightunsigned(4, 1);
 2
```

#### size

`size(expr)`:配列またはマップのサイズを返します。 入力がnullで、trueに設定されている場合は、-1 `spark.sql.legacy.sizeOfNull` を返します。 をfalse `spark.sql.legacy.sizeOfNull` に設定すると、null入力の場合はnullを返します。 デフォルトでは、このパ `spark.sql.legacy.sizeOfNull` ラメーターはtrueに設定されています。

例：

```
> SELECT size(array('b', 'd', 'c', 'a'));
 4
> SELECT size(map('a', 1, 'b', 2));
 2
> SELECT size(NULL);
 -1
```

#### スペース

`space(n)`:スペースで構成される文字列を返 `n` します。

例：

```
> SELECT concat(space(2), '1');
   1
```

#### 分割

`split(str, regex)`:一致す `str` る回を囲んで分割しま `regex`す。

例：

```
> SELECT split('oneAtwoBthreeC', '[ABC]');
 ["one","two","three",""]
```

#### substring_index

`substring_index(str, delim, count)`:区切り文字のオカレンスの前のサ `str` ブ文字列 `count` を返します(文字列の前の部分文字列を返しま `delim`す)。 が正 `count` の値の場合、（左から数える）最後の区切り文字の左側のすべてが返されます。 が負の `count` 場合は、最後の区切り文字の右側（右から数える）がすべて返されます。 この関数は、 `substring_index` 検索時に大文字と小文字が区別される一致を実行しま `delim`す。

例：

```
> SELECT substring_index('www.apache.org', '.', 2);
 www.apache
```

#### window

<!-- was blank -->

#### xpath

`xpath(xml, xpath)`:XPathノードと一致するxmlのノード内の値の文字列配列を返します。式

例：

```
> SELECT xpath('<a><b>b1</b><b>b2</b><b>b3</b><c>c1</c><c>c2</c></a>','a/b/text()');
 ['b1','b2','b3']
```

#### xpath_重複

`xpath_double(xml, xpath)`:重複値を返します。一致が見つからない場合は値0を返し、一致が見つかった場合は値が数値以外の場合はNaNを返します。

例：

```
> SELECT xpath_double('<a><b>1</b><b>2</b></a>', 'sum(a/b)');
 3.0
```

#### xpath_float

`xpath_float(xml, xpath)`:一致が見つからない場合は値0を、見つからない場合は値が数値以外の場合はNaNを返します。

例：

```
> SELECT xpath_float('<a><b>1</b><b>2</b></a>', 'sum(a/b)');
 3.0
```

#### xpath_int

`xpath_int(xml, xpath)`:整数値を返します。一致が見つからない場合、または一致が見つかったが数値以外の場合は、値0を返します。

例：

```
> SELECT xpath_int('<a><b>1</b><b>2</b></a>', 'sum(a/b)');
 3
```

#### xpath_long

`xpath_long(xml, xpath)`:長い整数値を返します。一致が見つからない場合、または一致が見つかったが数値以外の場合は、値0を返します。

例：

```
> SELECT xpath_long('<a><b>1</b><b>2</b></a>', 'sum(a/b)');
 3
```

#### xpath_number

`xpath_number(xml, xpath)`:重複値を返します。一致が見つからない場合は値0を返し、一致が見つかった場合は値が数値以外の場合はNaNを返します。

例：

```
> SELECT xpath_number('<a><b>1</b><b>2</b></a>', 'sum(a/b)');
 3.0
```

#### xpath_short

`xpath_short(xml, xpath)`:短い整数値を返します。一致が見つからない場合、または一致が見つかったが数値以外の場合は、値0を返します。

例：

```
> SELECT xpath_short('<a><b>1</b><b>2</b></a>', 'sum(a/b)');
 3
```

#### xpath_string

`xpath_string(xml, xpath)`:XPathノードに一致する最初のxmlノードのテキスト内容を返します。式

例：

```
> SELECT xpath_string('<a><b>b</b><c>cc</c></a>','a/c');
 cc
```

### 現在の情報

#### current_database

`current_database()`:現在のデータベースを返します。

例：

```
> SELECT current_database();
 default
```

#### current_date

`current_date()`:評価の開始時の現在の日付を返します。

次の期間：1.5.0

#### current_timestamp

`current_timestamp()`:評価の開始時の現在のタイムスタンプを返します。

次の期間：1.5.0

#### now

`now()`:評価の開始時の現在のタイムスタンプを返します。

次の期間：1.5.0
