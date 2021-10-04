---
keywords: Experience Platform；ホーム；人気のあるトピック；クエリサービス；クエリサービス；クエリサービス；spark sql;Spark sql;spark;spark;sql 関数；関数；
solution: Experience Platform
title: クエリサービスの Spark SQL 関数
topic-legacy: spark sql functions
description: このドキュメントには、SQL 機能を拡張する Spark SQL 関数に関する情報が含まれています。
exl-id: 59e6d82b-3317-456d-8c56-3efd5978433a
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '3893'
ht-degree: 2%

---

# [!DNL Spark] SQL 関数

Adobe Experience Platformクエリサービスには、SQL 機能を拡張する Spark SQL の組み込み関数がいくつか用意されています。 このドキュメントでは、クエリサービスでサポートされる Spark SQL 関数を示します。

関数の構文、使用方法、例など、関数の詳細については、[Spark SQL 関数のドキュメント ](https://spark.apache.org/docs/latest/api/sql/index.html) を参照してください。

>[!NOTE]
>
> 外部ドキュメント内のすべての関数がサポートされているわけではありません。

## カテゴリ

- [数学および統計の演算子と関数](#math)
- [論理演算子](#logical-operators)
- [日付/時間関数](#datetime-functions)
- [配列](#arrays)
- [データタイプキャスト関数](#datatype-casting)
- [変換関数と書式設定関数](#conversion)
- [データ評価](#data-evaluation)
- [現在の情報](#current-information)
- [上位関数](#higher-order)

## 数学および統計の演算子と関数 {#math}

| 演算子/関数 | 説明 |
| ----------------- | ----------- |
| [`%`](https://spark.apache.org/docs/latest/api/sql/index.html#_2) | 2 つの数の残りを返します |
| [`*`](https://spark.apache.org/docs/latest/api/sql/index.html#_4) | 2 つの数値を乗算します |
| [`+`](https://spark.apache.org/docs/latest/api/sql/index.html#_5) | 2 つの数を加算します |
| [`-`](https://spark.apache.org/docs/latest/api/sql/index.html#_6) | 2 つの数値を引きます |
| [`/`](https://spark.apache.org/docs/latest/api/sql/index.html#_7) | 2 つの数値を除算します |
| [`abs`](https://spark.apache.org/docs/latest/api/sql/index.html#abs) | 入力の絶対値を返します |
| [`acos`](https://spark.apache.org/docs/latest/api/sql/index.html#acos) | 逆余弦の値を返します |
| [`approx_count_distinct`](https://spark.apache.org/docs/latest/api/sql/index.html#approx_count_distinct) | HyperLogLog++による推定基数を返します |
| [`approx_percentile`](https://spark.apache.org/docs/latest/api/sql/index.html#approx_percentile) | 指定した割合での近似百分位値を返します |
| [`asin`](https://spark.apache.org/docs/latest/api/sql/index.html#asin) | 逆正弦値を返します |
| [`atan`](https://spark.apache.org/docs/latest/api/sql/index.html#atan) | 逆正接値を返します |
| [`atan2`](https://spark.apache.org/docs/latest/api/sql/index.html#atan2) | 正の x 軸平面と座標で指定された点との間の角度を返します |
| [`avg`](https://spark.apache.org/docs/latest/api/sql/index.html#avg) | 平均値を返します |
| [`cbrt`](https://spark.apache.org/docs/latest/api/sql/index.html#cbrt) | 立方根を返します |
| [`ceil`](https://spark.apache.org/docs/latest/api/sql/index.html#ceil) または [`ceiling`](https://spark.apache.org/docs/latest/api/sql/index.html#ceiling) | 入力された値以下の最小の整数を返します |
| [`conv`](https://spark.apache.org/docs/latest/api/sql/index.html#conv) | ベース間の変換 |
| [`corr`](https://spark.apache.org/docs/latest/api/sql/index.html#corr) | 数の間のピアソン係数を返します |
| [`cos`](https://spark.apache.org/docs/latest/api/sql/index.html#cos) | コサイン値を返します |
| [`cosh`](https://spark.apache.org/docs/latest/api/sql/index.html#cosh) | ハイパボリックコサイン値を返します |
| [`cot`](https://spark.apache.org/docs/latest/api/sql/index.html#cot) | 余接値を返します |
| [`dense_rank`](https://spark.apache.org/docs/latest/api/sql/index.html#dense_rank) | 値のグループ内の値のランクを返します |
| [`e`](https://spark.apache.org/docs/latest/api/sql/index.html#e) | オイラー数を返す |
| [`exp`](https://spark.apache.org/docs/latest/api/sql/index.html#exp) | e を値の乗に戻します |
| [`expm1`](https://spark.apache.org/docs/latest/api/sql/index.html#expm1) | e を値 — 1 の累乗に戻します |
| [`factorial`](https://spark.apache.org/docs/latest/api/sql/index.html#factorial) | 値の階乗を返します |
| [`floor`](https://spark.apache.org/docs/latest/api/sql/index.html#floor) | 値以上の最大の整数を返します |
| [`greatest`](https://spark.apache.org/docs/latest/api/sql/index.html#greatest) | すべてのパラメータの最大値を返します |
| [`hypot`](https://spark.apache.org/docs/latest/api/sql/index.html#hypot) | 指定された 2 つの値の斜辺を返します |
| [`kurtosis`](https://spark.apache.org/docs/latest/api/sql/index.html#kurtosis) | グループから尖度の値を返します |
| [`least`](https://spark.apache.org/docs/latest/api/sql/index.html#least) | すべてのパラメータの最小値を返します |
| [`ln`](https://spark.apache.org/docs/latest/api/sql/index.html#ln) | 値の自然対数を返します |
| [`log`](https://spark.apache.org/docs/latest/api/sql/index.html#log) | 値の対数を返します |
| [`log10`](https://spark.apache.org/docs/latest/api/sql/index.html#log10) | 値の 10 を底とする対数を返します |
| [`log1p`](https://spark.apache.org/docs/latest/api/sql/index.html#log1p) | 値に 1 を足した対数を返します |
| [`log2`](https://spark.apache.org/docs/latest/api/sql/index.html#log2) | 値の 2 を底とする対数を返します |
| [`max`](https://spark.apache.org/docs/latest/api/sql/index.html#max) | 式の最大値を返します |
| [`mean`](https://spark.apache.org/docs/latest/api/sql/index.html#mean) | 値から計算された平均値を返します |
| [`min`](https://spark.apache.org/docs/latest/api/sql/index.html#min) | 式の最小値を返します |
| [`monotonically_increasing_id`](https://spark.apache.org/docs/latest/api/sql/index.html#monotonically_increasing_id) | 単調に増加する ID を返す |
| [`negative`](https://spark.apache.org/docs/latest/api/sql/index.html#negative) | 負の値を返します |
| [`percent_rank`](https://spark.apache.org/docs/latest/api/sql/index.html#percent_rank) | 値のパーセントのランクを返します |
| [`percentile`](https://spark.apache.org/docs/latest/api/sql/index.html#percentile) | 指定した割合での正確な百分位を返します |
| [`percentile_approx`](https://spark.apache.org/docs/latest/api/sql/index.html#percentile_approx) | 指定した割合での近似百分位を返します |
| [`pi`](https://spark.apache.org/docs/latest/api/sql/index.html#pi) | pi を返します |
| [`pmod`](https://spark.apache.org/docs/latest/api/sql/index.html#pmod) | 2 つの値の間の正の剰余を返します |
| [`positive`](https://spark.apache.org/docs/latest/api/sql/index.html#positive) | 正の値を返します |
| [`pow`](https://spark.apache.org/docs/latest/api/sql/index.html#pow), [`power`](https://spark.apache.org/docs/latest/api/sql/index.html#power) | 最初の値を 2 番目の値の乗を返します |
| [`radians`](https://spark.apache.org/docs/latest/api/sql/index.html#radians) | 値をラジアンに変換します |
| [`rand`](https://spark.apache.org/docs/latest/api/sql/index.html#rand) | 0 ～ 1 の乱数を返します |
| [`randn`](https://spark.apache.org/docs/latest/api/sql/index.html#randn) | ランダムな値を返します |
| [`rint`](https://spark.apache.org/docs/latest/api/sql/index.html#rint) | 最も近い double 値を返します |
| [`round`](https://spark.apache.org/docs/latest/api/sql/index.html#round) | 最も近い丸め値を返します |
| [`sign`](https://spark.apache.org/docs/latest/api/sql/index.html#sign),  [`signum`](https://spark.apache.org/docs/latest/api/sql/index.html#signum) | 数値の符号を返します |
| [`sin`](https://spark.apache.org/docs/latest/api/sql/index.html#sin) | 値のサインを返します |
| [`sinh`](https://spark.apache.org/docs/latest/api/sql/index.html#sinh) | 値のハイパボリックサインを返します |
| [`sqrt`](https://spark.apache.org/docs/latest/api/sql/index.html#sqrt) | 値の平方根を返します |
| [`stddev`](https://spark.apache.org/docs/latest/api/sql/index.html#stddev) | 値の標準偏差を返します |
| [`sttdev_pop`](https://spark.apache.org/docs/latest/api/sql/index.html#sttdev_pop) | 値の母集団の標準偏差を返します |
| [`stddev_samp`](https://spark.apache.org/docs/latest/api/sql/index.html#stddev_samp) | 値のサンプルの標準偏差を返します |
| [`sum`](https://spark.apache.org/docs/latest/api/sql/index.html#sum) | 値の合計を返します |
| [`tan`](https://spark.apache.org/docs/latest/api/sql/index.html#tan) | 値の正接を返します |
| [`tanh`](https://spark.apache.org/docs/latest/api/sql/index.html#tanh) | 値のハイパボリックタンジェントを返します |
| [`var_pop`](https://spark.apache.org/docs/latest/api/sql/index.html#var_pop) | 計算された母分散を返します |
| [`var_samp`](https://spark.apache.org/docs/latest/api/sql/index.html#var_samp),  [`variance`](https://spark.apache.org/docs/latest/api/sql/index.html#variance) | 計算された標本分散を返します |

### 論理演算子と関数 {#logical-operators}

| 演算子/関数 | 説明 |
| ----------------- | ----------- |
| [`!`](https://spark.apache.org/docs/latest/api/sql/index.html#_1) または [`not`](https://spark.apache.org/docs/latest/api/sql/index.html#not) | 論理否定（not） |
| [`<`](https://spark.apache.org/docs/latest/api/sql/index.html#_7) | より小さい |
| [`<=`](https://spark.apache.org/docs/latest/api/sql/index.html#_8) | 次よりも小さいか等しい |
| [`=`](https://spark.apache.org/docs/latest/api/sql/index.html#_10) | 次と等しい |
| [`>`](https://spark.apache.org/docs/latest/api/sql/index.html#_12) | Greater than |
| [`>=`](https://spark.apache.org/docs/latest/api/sql/index.html#_13) | 以上 |
| [`^`](https://spark.apache.org/docs/latest/api/sql/index.html#_14) | ビット単位 XOR |
| [`>=`](https://spark.apache.org/docs/latest/api/sql/index.html#_13) | 以上 |
| [`|`](https://spark.apache.org/docs/latest/api/sql/index.html#_15) | ビット単位または |
| [`~`](https://spark.apache.org/docs/latest/api/sql/index.html#_16) | ビット単位ではない |
| [`arrays_overlap`](https://spark.apache.org/docs/latest/api/sql/index.html#arrays_overlap) | 共通要素を返します |
| [`assert_true`](https://spark.apache.org/docs/latest/api/sql/index.html#assert_true) | 式が true の場合にアサートします |
| [`if`](https://spark.apache.org/docs/latest/api/sql/index.html#if) | 式が true と評価される場合は、2 番目の式を返します。 それ以外の場合は、3 番目の式を返します。 |
| [`ifnull`](https://spark.apache.org/docs/latest/api/sql/index.html#ifnull) | 式が null の場合は、2 番目の式を返します。 それ以外の場合は、最初の式を返します。 |
| [`in`](https://spark.apache.org/docs/latest/api/sql/index.html#in) | 最初の式が後続の式のいずれかにある場合、true を返します。 |
| [`isnan`](https://spark.apache.org/docs/latest/api/sql/index.html#isnan) | 値が数値でない場合は true を返します |
| [`isnotnull`](https://spark.apache.org/docs/latest/api/sql/index.html#isnotnull) | 値が null でない場合は true を返します |
| [`isnull`](https://spark.apache.org/docs/latest/api/sql/index.html#isnull) | 値が null の場合は true を返します |
| [`nanvl`](https://spark.apache.org/docs/latest/api/sql/index.html#nanvl) | 数値でない場合は最初の式を返し、それ以外の場合は 2 番目の式を返します |
| [`or`](https://spark.apache.org/docs/latest/api/sql/index.html#or) | 論理和 (OR) |
| [`when`](https://spark.apache.org/docs/latest/api/sql/index.html#when) | 比較の分岐条件の作成に使用できるタイミング |
| [`xpath_boolean`](https://spark.apache.org/docs/latest/api/sql/index.html#xpath_boolean) | XPath 式が true と評価される場合、または一致するノードが見つかった場合は true を返します |

### 日付/時間関数 {#datetime-functions}

| 関数 | 説明 |
| -------- | ----------- |
| [`add_months`](https://spark.apache.org/docs/latest/api/sql/index.html#add_months) | 日付に月を追加 |
| [`date_add`](https://spark.apache.org/docs/latest/api/sql/index.html#date_add) | 日付への日数の追加 |
| [`date_format`](https://spark.apache.org/docs/latest/api/sql/index.html#date_format) | 日付形式の変更 |
| [`date_sub`](https://spark.apache.org/docs/latest/api/sql/index.html#date_sub) | 日付から日数を引く |
| [`date_trunc`](https://spark.apache.org/docs/latest/api/sql/index.html#date_trunc) | 指定した単位に切り捨てられた日付を返します |
| [`datediff`](https://spark.apache.org/docs/latest/api/sql/index.html#datediff) | 日付間の差を返します |
| [`day`](https://spark.apache.org/docs/latest/api/sql/index.html#day),  [`dayofmonth`](https://spark.apache.org/docs/latest/api/sql/index.html#dayofmonth) | 月の日を返します |
| [`dayofweek`](https://spark.apache.org/docs/latest/api/sql/index.html#dayofweek) | 曜日を返します (1-7) |
| [`dayofyear`](https://spark.apache.org/docs/latest/api/sql/index.html#dayofyear) | 年の日を返します |
| [`from_unixtime`](https://spark.apache.org/docs/latest/api/sql/index.html#from_unixtime) | 日付を UNIX 時間で返します |
| [`from_utc_timestamp`](https://spark.apache.org/docs/latest/api/sql/index.html#from_utc_timestamp) | UTC 時間で日付を返します |
| [`hour`](https://spark.apache.org/docs/latest/api/sql/index.html#hour) | 入力の時間を返します |
| [`last_day`](https://spark.apache.org/docs/latest/api/sql/index.html#last_day) | 日付が属する月の最終日を返します |
| [`minute`](https://spark.apache.org/docs/latest/api/sql/index.html#minute) | 入力の分を返します |
| [`month`](https://spark.apache.org/docs/latest/api/sql/index.html#month) | 入力された月を返します |
| [`months_between`](https://spark.apache.org/docs/latest/api/sql/index.html#months_between) | 間の月数 |
| [`next_day`](https://spark.apache.org/docs/latest/api/sql/index.html#next_day) | 入力よりも後の最初の日を返します |
| [`quarter`](https://spark.apache.org/docs/latest/api/sql/index.html#quarter) | 入力の四半期を返します |
| [`second`](https://spark.apache.org/docs/latest/api/sql/index.html#second) | 文字列の秒を返します |
| [`to_date`](https://spark.apache.org/docs/latest/api/sql/index.html#to_date) | 文字列を日付に変換します |
| [`to_timestamp`](https://spark.apache.org/docs/latest/api/sql/index.html#to_timestamp) | 文字列をタイムスタンプに変換します |
| [`to_unix_timestamp`](https://spark.apache.org/docs/latest/api/sql/index.html#to_unix_timestamp) | 文字列を Unix タイムスタンプに変換します |
| [`to_utc_timestamp`](https://spark.apache.org/docs/latest/api/sql/index.html#to_utc_timestamp) | 文字列を UTC タイムスタンプに変換します |
| [`trunc`](https://spark.apache.org/docs/latest/api/sql/index.html#trunc) | 日付を切り捨てます |
| [`unix_timestamp`](https://spark.apache.org/docs/latest/api/sql/index.html#unix_timestamp) | Unix タイムスタンプを返します |
| [`weekday`](https://spark.apache.org/docs/latest/api/sql/index.html#weekday) | 曜日 (0 ～ 6) |
| [`weekofyear`](https://spark.apache.org/docs/latest/api/sql/index.html#weekofyear) | 指定した日付の年の週を返します |
| [`year`](https://spark.apache.org/docs/latest/api/sql/index.html#year) | 文字列の年を返します |

### 配列 {#arrays}

| 関数 | 説明 |
| -------- | ----------- |
| [`array`](https://spark.apache.org/docs/latest/api/sql/index.html#array) | 指定された要素を持つ配列を作成します |
| [`array_contains`](https://spark.apache.org/docs/latest/api/sql/index.html#array_contains) | 配列に値が含まれているかどうかを確認します |
| [`array_distinct`](https://spark.apache.org/docs/latest/api/sql/index.html#array_distinct) | 配列から重複値を削除します |
| [`array_except`](https://spark.apache.org/docs/latest/api/sql/index.html#array_except) | 最初の配列の要素の配列を返しますが、2 番目の配列は返しません |
| [`array_intersect`](https://spark.apache.org/docs/latest/api/sql/index.html#array_intersect) | 2 つの配列の積集合を返します |
| [`array_join`](https://spark.apache.org/docs/latest/api/sql/index.html#array_join) | 2 つの配列を結合します |
| [`array_max`](https://spark.apache.org/docs/latest/api/sql/index.html#array_max) | 配列の最大値を返します |
| [`array_min`](https://spark.apache.org/docs/latest/api/sql/index.html#array_min) | 配列の最小値を返します |
| [`array_position`](https://spark.apache.org/docs/latest/api/sql/index.html#array_position) | 要素の 1 から始まる位置を返します |
| [`array_remove`](https://spark.apache.org/docs/latest/api/sql/index.html#array_remove) | 要素と等しい要素をすべて削除します |
| [`array_repeat`](https://spark.apache.org/docs/latest/api/sql/index.html#array_repeat) | カウントされた時間の値を含む配列を作成します |
| [`array_sort`](https://spark.apache.org/docs/latest/api/sql/index.html#array_sort) | 配列を並べ替えます |
| [`array_union`](https://spark.apache.org/docs/latest/api/sql/index.html#array_union) | 重複を伴わずに配列を結合 |
| [`array_zip`](https://spark.apache.org/docs/latest/api/sql/index.html#array_zip) | 郵便番号 |
| [`cardinality`](https://spark.apache.org/docs/latest/api/sql/index.html#cardinality) | 配列のサイズを返す |
| [`element_at`](https://spark.apache.org/docs/latest/api/sql/index.html#element_at) | 位置で要素を返す |
| [`explode`](https://spark.apache.org/docs/latest/api/sql/index.html#explode) | 配列の要素を複数の行に分割（null を除く） |
| [`explode_outer`](https://spark.apache.org/docs/latest/api/sql/index.html#explode_outer) | 配列の要素を複数の行（null を含む）に分割する |
| [`find_in_set`](https://spark.apache.org/docs/latest/api/sql/index.html#find_in_set) | 配列の 1 から始まる位置を返します |
| [`flatten`](https://spark.apache.org/docs/latest/api/sql/index.html#flatten) | 配列の統合 |
| [`inline`](https://spark.apache.org/docs/latest/api/sql/index.html#inline) | 構造体の配列をテーブルに分割（null を除く） |
| [`inline_outer`](https://spark.apache.org/docs/latest/api/sql/index.html#inline_outer) | 構造体の配列を表に分割します（null を含む）。 |
| [`posexplod`](https://spark.apache.org/docs/latest/api/sql/index.html#posexplod) | 配列の要素を、位置を持つ複数の行に分割します（null を除く） |
| [`posexplod`](https://spark.apache.org/docs/latest/api/sql/index.html#posexplod) | 配列の要素を複数の行に分割し、位置（null を含む）を指定します。 |
| [`reverse`](https://spark.apache.org/docs/latest/api/sql/index.html#reverse) | 配列の逆の要素 |
| [`shuffle`](https://spark.apache.org/docs/latest/api/sql/index.html#shuffle) | 配列のランダムな順列を返す |
| [`slice`](https://spark.apache.org/docs/latest/api/sql/index.html#slice) | 配列のサブセット |
| [`sort_array`](https://spark.apache.org/docs/latest/api/sql/index.html#sort_array) | 並べ替え順 |
| [`zip_with`](https://spark.apache.org/docs/latest/api/sql/index.html#zip_with) | 関数を適用する前に、2 つの配列を 1 つの配列に結合します |

### データタイプキャスト関数 {#datatype-casting}

| 関数 | 説明 |
| -------- | ----------- |
| [`bigint`](https://spark.apache.org/docs/latest/api/sql/index.html#bigint) | データ型を bigint に変更します。 |
| [`binary`](https://spark.apache.org/docs/latest/api/sql/index.html#binary) | データ型のバイナリへの変更 |
| [`boolean`](https://spark.apache.org/docs/latest/api/sql/index.html#boolean) | データ型のブール値への変更 |
| [`type`](https://spark.apache.org/docs/latest/api/sql/index.html#type) | データ型を指定した型に変更する |
| [`date`](https://spark.apache.org/docs/latest/api/sql/index.html#date) | データタイプを日付に変更します |
| [`decimal`](https://spark.apache.org/docs/latest/api/sql/index.html#decimal) | データ型の 10 進数への変更 |
| [`double`](https://spark.apache.org/docs/latest/api/sql/index.html#double) | データ型を double に変更します。 |
| [`float`](https://spark.apache.org/docs/latest/api/sql/index.html#float) | データ型を浮動小数に変更します |
| [`int`](https://spark.apache.org/docs/latest/api/sql/index.html#int) | データ型を int に変更します。 |
| [`smallint`](https://spark.apache.org/docs/latest/api/sql/index.html#smallint) | データ型を smallint に変更します。 |
| [`str_to_map`](https://spark.apache.org/docs/latest/api/sql/index.html#str_to_map) | 文字列からのマップの作成 |
| [`string`](https://spark.apache.org/docs/latest/api/sql/index.html#string) | データ型を文字列に変更します |
| [`struct`](https://spark.apache.org/docs/latest/api/sql/index.html#struct) | 構造体の作成 |
| [`tinyint`](https://spark.apache.org/docs/latest/api/sql/index.html#tinyint) | データ型の変更 |

### 変換関数と書式設定関数 {#conversion}

| 関数 | 説明 |
| -------- | ----------- |
| [`ascii`](https://spark.apache.org/docs/latest/api/sql/index.html#ascii) | 数値 (ASCII) 値を返す |
| [`base64`](https://spark.apache.org/docs/latest/api/sql/index.html#base64) | 引数を base64 文字列に変更します |
| [`bin`](https://spark.apache.org/docs/latest/api/sql/index.html#bin) | 引数をバイナリ値に変更する |
| [`bit_length`](https://spark.apache.org/docs/latest/api/sql/index.html#bit_length) | ビット長を返す |
| [`char`](https://spark.apache.org/docs/latest/api/sql/index.html#char),  [`chr`](https://spark.apache.org/docs/latest/api/sql/index.html#chr) | ASCII 文字を返す |
| [`char_length`](https://spark.apache.org/docs/latest/api/sql/index.html#char_length),  [`character_length`](https://spark.apache.org/docs/latest/api/sql/index.html#character_length) | 文字列の長さを返す |
| [`crc32`](https://spark.apache.org/docs/latest/api/sql/index.html#crc32) | 巡回冗長チェック値を返します |
| [`degrees`](https://spark.apache.org/docs/latest/api/sql/index.html#degrees) | ラジアンを度に変換する |
| [`format_number`](https://spark.apache.org/docs/latest/api/sql/index.html#format_number) | 数値の形式を変更する |
| [`from_json`](https://spark.apache.org/docs/latest/api/sql/index.html#from_json),  [`get_json_object`](https://spark.apache.org/docs/latest/api/sql/index.html#get_json_object) | JSON からのデータの取得 |
| [`hash`](https://spark.apache.org/docs/latest/api/sql/index.html#hash) | ハッシュ値を返す |
| [`hex`](https://spark.apache.org/docs/latest/api/sql/index.html#hex) | 引数を 16 進数値に変換する |
| [`initcap`](https://spark.apache.org/docs/latest/api/sql/index.html#initcap) | 文字列をタイトルに基づいて変更 |
| [`lcase`](https://spark.apache.org/docs/latest/api/sql/index.html#lcase),  [`lower`](https://spark.apache.org/docs/latest/api/sql/index.html#lower) | 文字列をすべて小文字に変更します |
| [`lpad`](https://spark.apache.org/docs/latest/api/sql/index.html#lpad) | 文字列の左側を埋め込む |
| [`map`](https://spark.apache.org/docs/latest/api/sql/index.html#map) | マップの作成 |
| [`map_from_arrays`](https://spark.apache.org/docs/latest/api/sql/index.html#map_from_arrays) | 配列からのマップの作成 |
| [`map_from_entries`](https://spark.apache.org/docs/latest/api/sql/index.html#map_from_entries) | 構造体の配列からマップを作成する |
| [`md5`](https://spark.apache.org/docs/latest/api/sql/index.html#md5) | md5 値を返す |
| [`rpad`](https://spark.apache.org/docs/latest/api/sql/index.html#rpad) | 文字列の右側を埋め込む |
| [`rtrim`](https://spark.apache.org/docs/latest/api/sql/index.html#rtrim) | 末尾の空白を削除 |
| [`sha`](https://spark.apache.org/docs/latest/api/sql/index.html#sha),  [`sha1`](https://spark.apache.org/docs/latest/api/sql/index.html#sha1) | SHA1 値を返す |
| [`sha2`](https://spark.apache.org/docs/latest/api/sql/index.html#sha2) | SHA2 値を返す |
| [`soundex`](https://spark.apache.org/docs/latest/api/sql/index.html#soundex) | soundex コードを返す |
| [`stack`](https://spark.apache.org/docs/latest/api/sql/index.html#stack) | 値を行に分割 |
| [`substr`](https://spark.apache.org/docs/latest/api/sql/index.html#substr),  [`substring`](https://spark.apache.org/docs/latest/api/sql/index.html#substring) | サブ文字列を返す |
| [`to_json`](https://spark.apache.org/docs/latest/api/sql/index.html#to_json) | JSON 文字列を返す |
| [`translate`](https://spark.apache.org/docs/latest/api/sql/index.html#translate) | 文字列内の値を置き換える |
| [`trim`](https://spark.apache.org/docs/latest/api/sql/index.html#trim) | 先頭と末尾の文字を削除する |
| [`ucase`](https://spark.apache.org/docs/latest/api/sql/index.html#ucase),  [`upper`](https://spark.apache.org/docs/latest/api/sql/index.html#upper) | 文字列をすべて大文字に変更します。 |
| [`unbase64`](https://spark.apache.org/docs/latest/api/sql/index.html#unbase64) | base64 文字列をバイナリに変換する |
| [`unhex`](https://spark.apache.org/docs/latest/api/sql/index.html#unhex) | 16 進数を 2 進数に変換する |
| [`uuid`](https://spark.apache.org/docs/latest/api/sql/index.html#uuid) | UUID を返す |

### データ評価 {#data-evaluation}

| 関数 | 説明 |
| -------- | ----------- |
| [`coalesce`](https://spark.apache.org/docs/latest/api/sql/index.html#coalesce) | 最初の null 以外の引数を返す |
| [`collect_list`](https://spark.apache.org/docs/latest/api/sql/index.html#collect_list) | 一意でない要素のリストを返す |
| [`collect_set`](https://spark.apache.org/docs/latest/api/sql/index.html#collect_set) | 一意の要素のセットを返す |
| [`concat`](https://spark.apache.org/docs/latest/api/sql/index.html#concat) | 連結 |
| [`concat_ws`](https://spark.apache.org/docs/latest/api/sql/index.html#concat_ws) | 区切り文字を使用した連結 |
| [`count`](https://spark.apache.org/docs/latest/api/sql/index.html#count) | 行の合計数を返します |
| [`decode`](https://spark.apache.org/docs/latest/api/sql/index.html#decode) | 文字セットを使用したデコード |
| [`elt`](https://spark.apache.org/docs/latest/api/sql/index.html#elt) | [`n`](https://spark.apache.org/docs/latest/api/sql/index.html#n) 入力を返す |
| [`encode`](https://spark.apache.org/docs/latest/api/sql/index.html#encode) | 文字セットを使用したエンコード |
| [`first`](https://spark.apache.org/docs/latest/api/sql/index.html#first),  [`first_value`](https://spark.apache.org/docs/latest/api/sql/index.html#first_value) | 最初の値を返します |
| [`grouping`](https://spark.apache.org/docs/latest/api/sql/index.html#grouping) | 列がグループ化されているかどうかを示します |
| [`grouping_id`](https://spark.apache.org/docs/latest/api/sql/index.html#grouping_id) | グループ化のレベルを返します |
| [`instr`](https://spark.apache.org/docs/latest/api/sql/index.html#instr) | 1 から始まる文字の出現箇所のインデックスを返します |
| [`json_tuple`](https://spark.apache.org/docs/latest/api/sql/index.html#json_tuple) | JSON 入力からタプルを返します |
| [`lag`](https://spark.apache.org/docs/latest/api/sql/index.html#lag),  [`lead`](https://spark.apache.org/docs/latest/api/sql/index.html#lead) | オフセットの前の値を返します |
| [`last`](https://spark.apache.org/docs/latest/api/sql/index.html#last),  [`last_value`](https://spark.apache.org/docs/latest/api/sql/index.html#last_value) | 最後の値を返します |
| [`left`](https://spark.apache.org/docs/latest/api/sql/index.html#left) | 最初の [`n`](https://spark.apache.org/docs/latest/api/sql/index.html#n) 文字を返します |
| [`length`](https://spark.apache.org/docs/latest/api/sql/index.html#length) | 文字列の長さを返します |
| [`levenshtein`](https://spark.apache.org/docs/latest/api/sql/index.html#levenshtein) | 文字列間のレーベンシュタイン距離を返します |
| [`locate`](https://spark.apache.org/docs/latest/api/sql/index.html#locate),  [`position`](https://spark.apache.org/docs/latest/api/sql/index.html#position) | サブ文字列の最初の値の位置を返します |
| [`map_concat`](https://spark.apache.org/docs/latest/api/sql/index.html#map_concat) | マップの連結 |
| [`map_keys`](https://spark.apache.org/docs/latest/api/sql/index.html#map_keys) | マップのキーを返す |
| [`map_values`](https://spark.apache.org/docs/latest/api/sql/index.html#map_values) | マップの値を返す |
| [`ntile`](https://spark.apache.org/docs/latest/api/sql/index.html#ntile) | 行を分割する |
| [`nullif`](https://spark.apache.org/docs/latest/api/sql/index.html#nullif) | true の場合は null を返します |
| [`nvl`](https://spark.apache.org/docs/latest/api/sql/index.html#nvl) | null の場合は値を返します |
| [`nvl2`](https://spark.apache.org/docs/latest/api/sql/index.html#nvl2) | null でない場合は値を返します |
| [`parse_url`](https://spark.apache.org/docs/latest/api/sql/index.html#parse_url) | URL の一部を抽出します |
| [`rank`](https://spark.apache.org/docs/latest/api/sql/index.html#rank) | 値のランクを計算します |
| [`regexp_extract`](https://spark.apache.org/docs/latest/api/sql/index.html#regexp_extract) | 正規表現に一致する値を抽出します |
| [`regex_replace`](https://spark.apache.org/docs/latest/api/sql/index.html#regex_replace) | 正規表現に一致するものを置き換える |
| [`repeat`](https://spark.apache.org/docs/latest/api/sql/index.html#repeat) | 繰り返される文字列を返します |
| [`replace`](https://spark.apache.org/docs/latest/api/sql/index.html#replace) | 文字列のすべてのインスタンスを置き換える |
| [`rollup`](https://spark.apache.org/docs/latest/api/sql/index.html#rollup) | 複数次元ロールアップの作成 |
| [`row_number`](https://spark.apache.org/docs/latest/api/sql/index.html#row_number) | 一意の行番号を割り当てます |
| [`schema_of_json`](https://spark.apache.org/docs/latest/api/sql/index.html#schema_of_json) | JSON のスキーマを返します |
| [`sentences`](https://spark.apache.org/docs/latest/api/sql/index.html#sentences) | 文字列を単語の配列に分割します |
| [`sequence`](https://spark.apache.org/docs/latest/api/sql/index.html#sequence) | 要素の配列を生成します |
| [`shiftleft`](https://spark.apache.org/docs/latest/api/sql/index.html#shiftleft) | 符号付きビット単位のシフト左 |
| [`shiftright`](https://spark.apache.org/docs/latest/api/sql/index.html#shiftright) | 符号付きビット単位の右シフト |
| [`shiftrightunsigned`](https://spark.apache.org/docs/latest/api/sql/index.html#shiftrightunsigned) | 符号なしビット単位の右シフト |
| [`size`](https://spark.apache.org/docs/latest/api/sql/index.html#size) | 配列のサイズを返す |
| [`space`](https://spark.apache.org/docs/latest/api/sql/index.html#space) | [`n`](https://spark.apache.org/docs/latest/api/sql/index.html#n) スペースを含む文字列を返す |
| [`split`](https://spark.apache.org/docs/latest/api/sql/index.html#split) | 分割文字列 |
| [`substring_index`](https://spark.apache.org/docs/latest/api/sql/index.html#substring_index) | サブ文字列のインデックスを返す |
| [`window`](https://spark.apache.org/docs/latest/api/sql/index.html#window) | ウィンドウ |
| [`xpath`](https://spark.apache.org/docs/latest/api/sql/index.html#xpath) | XML ノードの解析 |
| [`xpath_double`](https://spark.apache.org/docs/latest/api/sql/index.html#xpath_double),  [`xpath_number`](https://spark.apache.org/docs/latest/api/sql/index.html#xpath_number) | XML ノードをダブルに解析 |
| [`xpath_float`](https://spark.apache.org/docs/latest/api/sql/index.html#xpath_float) | XML ノードを float 用に解析する |
| [`xpath_int`](https://spark.apache.org/docs/latest/api/sql/index.html#xpath_int) | XML ノードを整数に対して解析する |
| [`xpath_long`](https://spark.apache.org/docs/latest/api/sql/index.html#xpath_long) | XML ノードを長時間解析する |
| [`xpath_short`](https://spark.apache.org/docs/latest/api/sql/index.html#xpath_short) | XML ノードを短整数に解析する |
| [`xpath_string`](https://spark.apache.org/docs/latest/api/sql/index.html#xpath_string) | XML ノードを文字列に対して解析する |

### 現在の情報 {#current-information}

| 関数 | 説明 |
| -------- | ----------- |
| [`current_database`](https://spark.apache.org/docs/latest/api/sql/index.html#current_database) | 現在のデータベースを返す |
| [`current_date`](https://spark.apache.org/docs/latest/api/sql/index.html#current_date) | 現在の日付を返します |
| [`current_timestamp`](https://spark.apache.org/docs/latest/api/sql/index.html#current_timestamp),  [`now`](https://spark.apache.org/docs/latest/api/sql/index.html#now) | 現在のタイムスタンプを返す |

### 上位関数 {#higher-order}

| 関数 | 説明 |
| -------- | ----------- |
| [`transform`](https://spark.apache.org/docs/latest/api/sql/index.html#transform) | 配列の要素を変換する |
| [`exists`](https://spark.apache.org/docs/latest/api/sql/index.html#exists) | 要素が存在するかどうかを確認 |
| [`filter`](https://spark.apache.org/docs/latest/api/sql/index.html#filter) | 入力配列のフィルタリング |
| [`aggregate`](https://spark.apache.org/docs/latest/api/sql/index.html#aggregate) | すべての要素にバイナリ演算子を適用する |
