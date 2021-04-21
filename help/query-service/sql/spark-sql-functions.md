---
keywords: Experience Platform；ホーム；人気のあるトピック；クエリサービス；クエリサービス；Spark SQL;Spark SQL;Spark SQL関数；関数；
solution: Experience Platform
title: クエリサービスのSpark SQL関数
topic-legacy: spark sql functions
description: このドキュメントには、SQL機能を拡張するSpark SQL関数に関する情報が含まれています。
exl-id: 59e6d82b-3317-456d-8c56-3efd5978433a
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '3893'
ht-degree: 2%

---

# [!DNL Spark] SQL関数

Adobe Experience Platformクエリサービスは、SQLの機能を拡張するSpark SQL関数をいくつか組み込んで提供しています。 このドキュメントは、クエリサービスでサポートされるSpark SQL関数をリストします。

関数の構文、使用方法、例など、関数の詳細については、[Spark SQL関数のドキュメント](https://spark.apache.org/docs/latest/api/sql/index.html)を参照してください。

>[!NOTE]
>
> 外部ドキュメント内のすべての関数がサポートされているわけではありません。

## カテゴリ

- [数学および統計の演算子と関数](#math)
- [論理演算子](#logical-operators)
- [日付／時間関数](#datetime-functions)
- [配列](#arrays)
- [データタイプキャスト関数](#datatype-casting)
- [変換関数と書式設定関数](#conversion)
- [データ評価](#data-evaluation)
- [現在の情報](#current-information)
- [上位関数](#higher-order)

## 数学および統計の演算子と関数 {#math}

| 演算子/関数 | 説明 |
| ----------------- | ----------- |
| [`%`](https://spark.apache.org/docs/latest/api/sql/index.html#_2) | 2つの数値の剰余を返します |
| [`*`](https://spark.apache.org/docs/latest/api/sql/index.html#_4) | 2つの数値を乗算します。 |
| [`+`](https://spark.apache.org/docs/latest/api/sql/index.html#_5) | 2つの数値を加算します。 |
| [`-`](https://spark.apache.org/docs/latest/api/sql/index.html#_6) | 2つの数値を減算します |
| [`/`](https://spark.apache.org/docs/latest/api/sql/index.html#_7) | 2つの数値を除算します。 |
| [`abs`](https://spark.apache.org/docs/latest/api/sql/index.html#abs) | 入力の絶対値を返します |
| [`acos`](https://spark.apache.org/docs/latest/api/sql/index.html#acos) | 逆コサイン値を返します |
| [`approx_count_distinct`](https://spark.apache.org/docs/latest/api/sql/index.html#approx_count_distinct) | HyperLogLog++による推定基数を返します |
| [`approx_percentile`](https://spark.apache.org/docs/latest/api/sql/index.html#approx_percentile) | 指定した割合での近似パーセンタイル値を返します |
| [`asin`](https://spark.apache.org/docs/latest/api/sql/index.html#asin) | 逆サイン値を返す |
| [`atan`](https://spark.apache.org/docs/latest/api/sql/index.html#atan) | 逆正接値を返します。 |
| [`atan2`](https://spark.apache.org/docs/latest/api/sql/index.html#atan2) | 正のx軸平面と座標で指定された点との間の角度を返します |
| [`avg`](https://spark.apache.org/docs/latest/api/sql/index.html#avg) | 平均値を返す |
| [`cbrt`](https://spark.apache.org/docs/latest/api/sql/index.html#cbrt) | キューブルートを返します。 |
| [`ceil`](https://spark.apache.org/docs/latest/api/sql/index.html#ceil) または [`ceiling`](https://spark.apache.org/docs/latest/api/sql/index.html#ceiling) | 入力された値以下の最小の整数を返します |
| [`conv`](https://spark.apache.org/docs/latest/api/sql/index.html#conv) | ベース間の変換 |
| [`corr`](https://spark.apache.org/docs/latest/api/sql/index.html#corr) | 数値の間のピアソン係数を返します。 |
| [`cos`](https://spark.apache.org/docs/latest/api/sql/index.html#cos) | コサイン値を返す |
| [`cosh`](https://spark.apache.org/docs/latest/api/sql/index.html#cosh) | ハイパボリックコサイン値を返します。 |
| [`cot`](https://spark.apache.org/docs/latest/api/sql/index.html#cot) | コタンジェント値を返します。 |
| [`dense_rank`](https://spark.apache.org/docs/latest/api/sql/index.html#dense_rank) | 値のグループ内の値のランクを返します |
| [`e`](https://spark.apache.org/docs/latest/api/sql/index.html#e) | オイラー数を返す |
| [`exp`](https://spark.apache.org/docs/latest/api/sql/index.html#exp) | 値の累乗にeを返します |
| [`expm1`](https://spark.apache.org/docs/latest/api/sql/index.html#expm1) | 値から1を引いた値の累乗にeを返します。 |
| [`factorial`](https://spark.apache.org/docs/latest/api/sql/index.html#factorial) | 値の階乗を返します |
| [`floor`](https://spark.apache.org/docs/latest/api/sql/index.html#floor) | 値以上の最大の整数を返します |
| [`greatest`](https://spark.apache.org/docs/latest/api/sql/index.html#greatest) | すべてのパラメーターの最大値を返します |
| [`hypot`](https://spark.apache.org/docs/latest/api/sql/index.html#hypot) | 渡された2つの値の斜辺を返します |
| [`kurtosis`](https://spark.apache.org/docs/latest/api/sql/index.html#kurtosis) | グループから尖度の値を返します |
| [`least`](https://spark.apache.org/docs/latest/api/sql/index.html#least) | すべてのパラメーターの最小値を返します |
| [`ln`](https://spark.apache.org/docs/latest/api/sql/index.html#ln) | 値の自然対数を返します |
| [`log`](https://spark.apache.org/docs/latest/api/sql/index.html#log) | 値の対数を返します |
| [`log10`](https://spark.apache.org/docs/latest/api/sql/index.html#log10) | 値の対数（10を底とする）を返します |
| [`log1p`](https://spark.apache.org/docs/latest/api/sql/index.html#log1p) | 1を足した値の対数を返します。 |
| [`log2`](https://spark.apache.org/docs/latest/api/sql/index.html#log2) | 値の対数（2を底とする）を返します |
| [`max`](https://spark.apache.org/docs/latest/api/sql/index.html#max) | 式の最大値を返します。 |
| [`mean`](https://spark.apache.org/docs/latest/api/sql/index.html#mean) | 値から計算された平均値を返します |
| [`min`](https://spark.apache.org/docs/latest/api/sql/index.html#min) | 式の最小値を返します。 |
| [`monotonically_increasing_id`](https://spark.apache.org/docs/latest/api/sql/index.html#monotonically_increasing_id) | 単調に増加するIDを返す |
| [`negative`](https://spark.apache.org/docs/latest/api/sql/index.html#negative) | 負の値を返す |
| [`percent_rank`](https://spark.apache.org/docs/latest/api/sql/index.html#percent_rank) | 値のパーセンテージのランクを返します |
| [`percentile`](https://spark.apache.org/docs/latest/api/sql/index.html#percentile) | 特定の割合での正確なパーセンタイルを返します |
| [`percentile_approx`](https://spark.apache.org/docs/latest/api/sql/index.html#percentile_approx) | 指定した割合での近似パーセンタイルを返します |
| [`pi`](https://spark.apache.org/docs/latest/api/sql/index.html#pi) | piを返す |
| [`pmod`](https://spark.apache.org/docs/latest/api/sql/index.html#pmod) | 2つの値の間の正のモジュロを返します |
| [`positive`](https://spark.apache.org/docs/latest/api/sql/index.html#positive) | 正の値を返す |
| [`pow`](https://spark.apache.org/docs/latest/api/sql/index.html#pow),  [`power`](https://spark.apache.org/docs/latest/api/sql/index.html#power) | 1つ目の値を2つ目の値の累乗値に返します。 |
| [`radians`](https://spark.apache.org/docs/latest/api/sql/index.html#radians) | 値をラジアンに変換します。 |
| [`rand`](https://spark.apache.org/docs/latest/api/sql/index.html#rand) | 0 ～ 1の乱数を返します。 |
| [`randn`](https://spark.apache.org/docs/latest/api/sql/index.html#randn) | 乱数を返す |
| [`rint`](https://spark.apache.org/docs/latest/api/sql/index.html#rint) | 最も近い重複値を返します。 |
| [`round`](https://spark.apache.org/docs/latest/api/sql/index.html#round) | 最も近い丸められた値を返します |
| [`sign`](https://spark.apache.org/docs/latest/api/sql/index.html#sign),  [`signum`](https://spark.apache.org/docs/latest/api/sql/index.html#signum) | 数値の符号を返します |
| [`sin`](https://spark.apache.org/docs/latest/api/sql/index.html#sin) | 値のサインを返します |
| [`sinh`](https://spark.apache.org/docs/latest/api/sql/index.html#sinh) | 値のハイパボリックサインを返します。 |
| [`sqrt`](https://spark.apache.org/docs/latest/api/sql/index.html#sqrt) | 値の平方根を返します |
| [`stddev`](https://spark.apache.org/docs/latest/api/sql/index.html#stddev) | 値の標準偏差を返します。 |
| [`sttdev_pop`](https://spark.apache.org/docs/latest/api/sql/index.html#sttdev_pop) | 値の母集団の標準偏差を返します |
| [`stddev_samp`](https://spark.apache.org/docs/latest/api/sql/index.html#stddev_samp) | 値の標準偏差のサンプルを返します。 |
| [`sum`](https://spark.apache.org/docs/latest/api/sql/index.html#sum) | 値の合計を返します。 |
| [`tan`](https://spark.apache.org/docs/latest/api/sql/index.html#tan) | 値のタンジェントを返します |
| [`tanh`](https://spark.apache.org/docs/latest/api/sql/index.html#tanh) | 値のハイパボリックタンジェントを返します。 |
| [`var_pop`](https://spark.apache.org/docs/latest/api/sql/index.html#var_pop) | 計算された母集団の平方偏差を返します |
| [`var_samp`](https://spark.apache.org/docs/latest/api/sql/index.html#var_samp),  [`variance`](https://spark.apache.org/docs/latest/api/sql/index.html#variance) | 計算されたサンプルの平方偏差を返します |

### 論理演算子と関数{#logical-operators}

| 演算子/関数 | 説明 |
| ----------------- | ----------- |
| [`!`](https://spark.apache.org/docs/latest/api/sql/index.html#_1) または [`not`](https://spark.apache.org/docs/latest/api/sql/index.html#not) | 論理否定（not） |
| [`<`](https://spark.apache.org/docs/latest/api/sql/index.html#_7) | より小さい |
| [`<=`](https://spark.apache.org/docs/latest/api/sql/index.html#_8) | 次よりも小さいか等しい |
| [`=`](https://spark.apache.org/docs/latest/api/sql/index.html#_10) | 次と等しい |
| [`>`](https://spark.apache.org/docs/latest/api/sql/index.html#_12) | より大きい |
| [`>=`](https://spark.apache.org/docs/latest/api/sql/index.html#_13) | 以上 |
| [`^`](https://spark.apache.org/docs/latest/api/sql/index.html#_14) | ビット単位 XOR |
| [`>=`](https://spark.apache.org/docs/latest/api/sql/index.html#_13) | 以上 |
| [`|`](https://spark.apache.org/docs/latest/api/sql/index.html#_15) | ビット単位または |
| [`~`](https://spark.apache.org/docs/latest/api/sql/index.html#_16) | ビット単位でない |
| [`arrays_overlap`](https://spark.apache.org/docs/latest/api/sql/index.html#arrays_overlap) | 共通の要素を返す |
| [`assert_true`](https://spark.apache.org/docs/latest/api/sql/index.html#assert_true) | 式がtrueの場合にアサートします。 |
| [`if`](https://spark.apache.org/docs/latest/api/sql/index.html#if) | 式がtrueと評価された場合は、2番目の式を返します。 それ以外の場合は、3番目の式を返します。 |
| [`ifnull`](https://spark.apache.org/docs/latest/api/sql/index.html#ifnull) | 式がnullの場合は、2番目の式を返します。 それ以外の場合は、最初の式を返します。 |
| [`in`](https://spark.apache.org/docs/latest/api/sql/index.html#in) | 最初の式が後続の式のいずれかにある場合、trueを返します。 |
| [`isnan`](https://spark.apache.org/docs/latest/api/sql/index.html#isnan) | 値が数値でない場合はtrueを返します |
| [`isnotnull`](https://spark.apache.org/docs/latest/api/sql/index.html#isnotnull) | 値がnullでない場合はtrueを返します |
| [`isnull`](https://spark.apache.org/docs/latest/api/sql/index.html#isnull) | 値がnullの場合はtrueを返します |
| [`nanvl`](https://spark.apache.org/docs/latest/api/sql/index.html#nanvl) | 数値以外の場合は最初の式を返し、それ以外の場合は2番目の式を返します |
| [`or`](https://spark.apache.org/docs/latest/api/sql/index.html#or) | 論理和 |
| [`when`](https://spark.apache.org/docs/latest/api/sql/index.html#when) | 比較のためのブランチ条件の作成に使用できるタイミング |
| [`xpath_boolean`](https://spark.apache.org/docs/latest/api/sql/index.html#xpath_boolean) | XPath式がtrueと評価される場合、または一致するノードが見つかる場合にtrueを返します |

### 日付/時間関数 {#datetime-functions}

| 関数 | 説明 |
| -------- | ----------- |
| [`add_months`](https://spark.apache.org/docs/latest/api/sql/index.html#add_months) | 今追加日までの月数 |
| [`date_add`](https://spark.apache.org/docs/latest/api/sql/index.html#date_add) | 日追加々 |
| [`date_format`](https://spark.apache.org/docs/latest/api/sql/index.html#date_format) | 日付形式の変更 |
| [`date_sub`](https://spark.apache.org/docs/latest/api/sql/index.html#date_sub) | 日付から日数を引く |
| [`date_trunc`](https://spark.apache.org/docs/latest/api/sql/index.html#date_trunc) | 指定した単位に切り捨てられた日付を返します。 |
| [`datediff`](https://spark.apache.org/docs/latest/api/sql/index.html#datediff) | 日単位の日付の差を返します |
| [`day`](https://spark.apache.org/docs/latest/api/sql/index.html#day),  [`dayofmonth`](https://spark.apache.org/docs/latest/api/sql/index.html#dayofmonth) | 月の日を返します。 |
| [`dayofweek`](https://spark.apache.org/docs/latest/api/sql/index.html#dayofweek) | 曜日を返します(1 ～ 7) |
| [`dayofyear`](https://spark.apache.org/docs/latest/api/sql/index.html#dayofyear) | 年の日を返します。 |
| [`from_unixtime`](https://spark.apache.org/docs/latest/api/sql/index.html#from_unixtime) | Unix時間で日付を返す |
| [`from_utc_timestamp`](https://spark.apache.org/docs/latest/api/sql/index.html#from_utc_timestamp) | 日付をUTC時刻で返します。 |
| [`hour`](https://spark.apache.org/docs/latest/api/sql/index.html#hour) | 入力された時間を返します |
| [`last_day`](https://spark.apache.org/docs/latest/api/sql/index.html#last_day) | 日付が属する月の最後の日を返します |
| [`minute`](https://spark.apache.org/docs/latest/api/sql/index.html#minute) | 入力された分を返します。 |
| [`month`](https://spark.apache.org/docs/latest/api/sql/index.html#month) | 入力された月を返します |
| [`months_between`](https://spark.apache.org/docs/latest/api/sql/index.html#months_between) | 間の月数 |
| [`next_day`](https://spark.apache.org/docs/latest/api/sql/index.html#next_day) | 入力値より後の最初の日を返します |
| [`quarter`](https://spark.apache.org/docs/latest/api/sql/index.html#quarter) | 入力の四半期を返します |
| [`second`](https://spark.apache.org/docs/latest/api/sql/index.html#second) | 文字列の2番目の値を返します |
| [`to_date`](https://spark.apache.org/docs/latest/api/sql/index.html#to_date) | 文字列を日付に変換します。 |
| [`to_timestamp`](https://spark.apache.org/docs/latest/api/sql/index.html#to_timestamp) | 文字列をタイムスタンプに変換します。 |
| [`to_unix_timestamp`](https://spark.apache.org/docs/latest/api/sql/index.html#to_unix_timestamp) | 文字列をUnixタイムスタンプに変換します。 |
| [`to_utc_timestamp`](https://spark.apache.org/docs/latest/api/sql/index.html#to_utc_timestamp) | 文字列をUTCタイムスタンプに変換します。 |
| [`trunc`](https://spark.apache.org/docs/latest/api/sql/index.html#trunc) | 日付を切り詰めます。 |
| [`unix_timestamp`](https://spark.apache.org/docs/latest/api/sql/index.html#unix_timestamp) | Unixタイムスタンプを返す |
| [`weekday`](https://spark.apache.org/docs/latest/api/sql/index.html#weekday) | 曜日(0 ～ 6) |
| [`weekofyear`](https://spark.apache.org/docs/latest/api/sql/index.html#weekofyear) | 指定した日付の年の何週目かを返します |
| [`year`](https://spark.apache.org/docs/latest/api/sql/index.html#year) | 文字列の年を返します。 |

### 配列 {#arrays}

| 関数 | 説明 |
| -------- | ----------- |
| [`array`](https://spark.apache.org/docs/latest/api/sql/index.html#array) | 指定した要素を持つ配列を作成します |
| [`array_contains`](https://spark.apache.org/docs/latest/api/sql/index.html#array_contains) | 配列に値が含まれているかどうかを確認します。 |
| [`array_distinct`](https://spark.apache.org/docs/latest/api/sql/index.html#array_distinct) | 配列から重複値を削除します。 |
| [`array_except`](https://spark.apache.org/docs/latest/api/sql/index.html#array_except) | 最初の配列の要素の配列を返しますが、2番目の配列は返しません |
| [`array_intersect`](https://spark.apache.org/docs/latest/api/sql/index.html#array_intersect) | 2つの配列の交点を返します |
| [`array_join`](https://spark.apache.org/docs/latest/api/sql/index.html#array_join) | 2つのアレイを結合 |
| [`array_max`](https://spark.apache.org/docs/latest/api/sql/index.html#array_max) | 配列の最大値を返します。 |
| [`array_min`](https://spark.apache.org/docs/latest/api/sql/index.html#array_min) | 配列の最小値を返します。 |
| [`array_position`](https://spark.apache.org/docs/latest/api/sql/index.html#array_position) | 要素の1を基準とする位置を返します |
| [`array_remove`](https://spark.apache.org/docs/latest/api/sql/index.html#array_remove) | その要素に等しい要素をすべて削除します |
| [`array_repeat`](https://spark.apache.org/docs/latest/api/sql/index.html#array_repeat) | カウントされた値を含む配列を作成します |
| [`array_sort`](https://spark.apache.org/docs/latest/api/sql/index.html#array_sort) | 配列を並べ替えます。 |
| [`array_union`](https://spark.apache.org/docs/latest/api/sql/index.html#array_union) | 重複なしで配列を結合 |
| [`array_zip`](https://spark.apache.org/docs/latest/api/sql/index.html#array_zip) | 郵便番号 |
| [`cardinality`](https://spark.apache.org/docs/latest/api/sql/index.html#cardinality) | 配列のサイズを返す |
| [`element_at`](https://spark.apache.org/docs/latest/api/sql/index.html#element_at) | 位置にある要素を返す |
| [`explode`](https://spark.apache.org/docs/latest/api/sql/index.html#explode) | 配列の要素を複数の行に分割（nullは除く） |
| [`explode_outer`](https://spark.apache.org/docs/latest/api/sql/index.html#explode_outer) | 配列の要素を複数の行（nullを含む）に分割する |
| [`find_in_set`](https://spark.apache.org/docs/latest/api/sql/index.html#find_in_set) | 配列の1を基準とする位置を返します |
| [`flatten`](https://spark.apache.org/docs/latest/api/sql/index.html#flatten) | 配列を分割・統合 |
| [`inline`](https://spark.apache.org/docs/latest/api/sql/index.html#inline) | 構造体の配列をテーブルに分割（nullを除く） |
| [`inline_outer`](https://spark.apache.org/docs/latest/api/sql/index.html#inline_outer) | 構造体の配列をnullを含むテーブルに分割する |
| [`posexplod`](https://spark.apache.org/docs/latest/api/sql/index.html#posexplod) | 配列の要素を複数の行に分割し、位置を持たせます（nullは除く） |
| [`posexplod`](https://spark.apache.org/docs/latest/api/sql/index.html#posexplod) | 配列の要素を、nullを含む位置を持つ複数の行に分割する |
| [`reverse`](https://spark.apache.org/docs/latest/api/sql/index.html#reverse) | 配列の要素を反転する |
| [`shuffle`](https://spark.apache.org/docs/latest/api/sql/index.html#shuffle) | 配列のランダム順列を返す |
| [`slice`](https://spark.apache.org/docs/latest/api/sql/index.html#slice) | 配列のサブセット |
| [`sort_array`](https://spark.apache.org/docs/latest/api/sql/index.html#sort_array) | 並べ替え順に従って配列を並べ替える |
| [`zip_with`](https://spark.apache.org/docs/latest/api/sql/index.html#zip_with) | 関数を適用する前に、2つの配列を1つの配列に結合します。 |

### データタイプキャスト関数 {#datatype-casting}

| 関数 | 説明 |
| -------- | ----------- |
| [`bigint`](https://spark.apache.org/docs/latest/api/sql/index.html#bigint) | データ型をbigintに変更します |
| [`binary`](https://spark.apache.org/docs/latest/api/sql/index.html#binary) | データ型をバイナリに変更します |
| [`boolean`](https://spark.apache.org/docs/latest/api/sql/index.html#boolean) | データ型のブール値への変更 |
| [`type`](https://spark.apache.org/docs/latest/api/sql/index.html#type) | データ型を指定した型に変更します |
| [`date`](https://spark.apache.org/docs/latest/api/sql/index.html#date) | データタイプを日付に変更する |
| [`decimal`](https://spark.apache.org/docs/latest/api/sql/index.html#decimal) | データ型の変更：小数 |
| [`double`](https://spark.apache.org/docs/latest/api/sql/index.html#double) | データタイプの重複への変更 |
| [`float`](https://spark.apache.org/docs/latest/api/sql/index.html#float) | データタイプをfloatに変更します。 |
| [`int`](https://spark.apache.org/docs/latest/api/sql/index.html#int) | データ型の変更をintにします |
| [`smallint`](https://spark.apache.org/docs/latest/api/sql/index.html#smallint) | データ型の変更をsmallintにします |
| [`str_to_map`](https://spark.apache.org/docs/latest/api/sql/index.html#str_to_map) | 文字列からのマップの作成 |
| [`string`](https://spark.apache.org/docs/latest/api/sql/index.html#string) | データタイプを文字列に変更します |
| [`struct`](https://spark.apache.org/docs/latest/api/sql/index.html#struct) | 構造体を作成する |
| [`tinyint`](https://spark.apache.org/docs/latest/api/sql/index.html#tinyint) | データ型の変更をtinyintにします |

### 変換関数と書式設定関数 {#conversion}

| 関数 | 説明 |
| -------- | ----------- |
| [`ascii`](https://spark.apache.org/docs/latest/api/sql/index.html#ascii) | 数値(ASCII)を返す |
| [`base64`](https://spark.apache.org/docs/latest/api/sql/index.html#base64) | 引数をbase64文字列に変更します |
| [`bin`](https://spark.apache.org/docs/latest/api/sql/index.html#bin) | 引数をバイナリ値に変更します |
| [`bit_length`](https://spark.apache.org/docs/latest/api/sql/index.html#bit_length) | ビット長を返す |
| [`char`](https://spark.apache.org/docs/latest/api/sql/index.html#char),  [`chr`](https://spark.apache.org/docs/latest/api/sql/index.html#chr) | ASCII文字を返す |
| [`char_length`](https://spark.apache.org/docs/latest/api/sql/index.html#char_length),  [`character_length`](https://spark.apache.org/docs/latest/api/sql/index.html#character_length) | 文字列の長さを返す |
| [`crc32`](https://spark.apache.org/docs/latest/api/sql/index.html#crc32) | 循環冗長チェック値を返す |
| [`degrees`](https://spark.apache.org/docs/latest/api/sql/index.html#degrees) | ラジアンを度に変換 |
| [`format_number`](https://spark.apache.org/docs/latest/api/sql/index.html#format_number) | 数値の形式を変更する |
| [`from_json`](https://spark.apache.org/docs/latest/api/sql/index.html#from_json),  [`get_json_object`](https://spark.apache.org/docs/latest/api/sql/index.html#get_json_object) | JSONからデータを取得 |
| [`hash`](https://spark.apache.org/docs/latest/api/sql/index.html#hash) | ハッシュ値を返す |
| [`hex`](https://spark.apache.org/docs/latest/api/sql/index.html#hex) | 引数を16進値に変換します |
| [`initcap`](https://spark.apache.org/docs/latest/api/sql/index.html#initcap) | 文字列をタイトルに基づいて変更します。 |
| [`lcase`](https://spark.apache.org/docs/latest/api/sql/index.html#lcase),  [`lower`](https://spark.apache.org/docs/latest/api/sql/index.html#lower) | 文字列をすべて小文字に変更します。 |
| [`lpad`](https://spark.apache.org/docs/latest/api/sql/index.html#lpad) | 文字列の左側をパディングします。 |
| [`map`](https://spark.apache.org/docs/latest/api/sql/index.html#map) | マップの作成 |
| [`map_from_arrays`](https://spark.apache.org/docs/latest/api/sql/index.html#map_from_arrays) | 配列からマップを作成する |
| [`map_from_entries`](https://spark.apache.org/docs/latest/api/sql/index.html#map_from_entries) | 構造体の配列からマップを作成する |
| [`md5`](https://spark.apache.org/docs/latest/api/sql/index.html#md5) | md5値を返す |
| [`rpad`](https://spark.apache.org/docs/latest/api/sql/index.html#rpad) | 文字列の右側をパディングします。 |
| [`rtrim`](https://spark.apache.org/docs/latest/api/sql/index.html#rtrim) | 末尾の空白を削除 |
| [`sha`](https://spark.apache.org/docs/latest/api/sql/index.html#sha),  [`sha1`](https://spark.apache.org/docs/latest/api/sql/index.html#sha1) | SHA1値を返す |
| [`sha2`](https://spark.apache.org/docs/latest/api/sql/index.html#sha2) | SHA2値を返す |
| [`soundex`](https://spark.apache.org/docs/latest/api/sql/index.html#soundex) | soundexコードを返す |
| [`stack`](https://spark.apache.org/docs/latest/api/sql/index.html#stack) | 値を行に分割する |
| [`substr`](https://spark.apache.org/docs/latest/api/sql/index.html#substr),  [`substring`](https://spark.apache.org/docs/latest/api/sql/index.html#substring) | サブ文字列を返す |
| [`to_json`](https://spark.apache.org/docs/latest/api/sql/index.html#to_json) | JSON文字列を返す |
| [`translate`](https://spark.apache.org/docs/latest/api/sql/index.html#translate) | 文字列内の値を置換 |
| [`trim`](https://spark.apache.org/docs/latest/api/sql/index.html#trim) | 先頭および末尾の文字の削除 |
| [`ucase`](https://spark.apache.org/docs/latest/api/sql/index.html#ucase),  [`upper`](https://spark.apache.org/docs/latest/api/sql/index.html#upper) | 文字列をすべて大文字に変更する |
| [`unbase64`](https://spark.apache.org/docs/latest/api/sql/index.html#unbase64) | base64文字列をバイナリに変換します |
| [`unhex`](https://spark.apache.org/docs/latest/api/sql/index.html#unhex) | 16進数をバイナリに変換します |
| [`uuid`](https://spark.apache.org/docs/latest/api/sql/index.html#uuid) | UUIDを返す |

### データ評価 {#data-evaluation}

| 関数 | 説明 |
| -------- | ----------- |
| [`coalesce`](https://spark.apache.org/docs/latest/api/sql/index.html#coalesce) | nullでない最初の引数を返す |
| [`collect_list`](https://spark.apache.org/docs/latest/api/sql/index.html#collect_list) | 一意でない要素のリストを返す |
| [`collect_set`](https://spark.apache.org/docs/latest/api/sql/index.html#collect_set) | 一意な要素のセットを返す |
| [`concat`](https://spark.apache.org/docs/latest/api/sql/index.html#concat) | 連結 |
| [`concat_ws`](https://spark.apache.org/docs/latest/api/sql/index.html#concat_ws) | 区切り文字を使用した連結 |
| [`count`](https://spark.apache.org/docs/latest/api/sql/index.html#count) | 行の合計数を返します。 |
| [`decode`](https://spark.apache.org/docs/latest/api/sql/index.html#decode) | 文字セットを使用したデコード |
| [`elt`](https://spark.apache.org/docs/latest/api/sql/index.html#elt) | [`n`](https://spark.apache.org/docs/latest/api/sql/index.html#n)番目の入力を返す |
| [`encode`](https://spark.apache.org/docs/latest/api/sql/index.html#encode) | 文字セットを使用したエンコード |
| [`first`](https://spark.apache.org/docs/latest/api/sql/index.html#first),  [`first_value`](https://spark.apache.org/docs/latest/api/sql/index.html#first_value) | 最初の値を返す |
| [`grouping`](https://spark.apache.org/docs/latest/api/sql/index.html#grouping) | 列がグループ化されているかどうかを示します。 |
| [`grouping_id`](https://spark.apache.org/docs/latest/api/sql/index.html#grouping_id) | グループ化のレベルを返します。 |
| [`instr`](https://spark.apache.org/docs/latest/api/sql/index.html#instr) | 1を基準とする文字の出現箇所のインデックスを返します |
| [`json_tuple`](https://spark.apache.org/docs/latest/api/sql/index.html#json_tuple) | JSON入力からタプルを返す |
| [`lag`](https://spark.apache.org/docs/latest/api/sql/index.html#lag),  [`lead`](https://spark.apache.org/docs/latest/api/sql/index.html#lead) | オフセットの前の値を返す |
| [`last`](https://spark.apache.org/docs/latest/api/sql/index.html#last),  [`last_value`](https://spark.apache.org/docs/latest/api/sql/index.html#last_value) | 最後の値を返します |
| [`left`](https://spark.apache.org/docs/latest/api/sql/index.html#left) | 最初の[`n`](https://spark.apache.org/docs/latest/api/sql/index.html#n)文字を返します |
| [`length`](https://spark.apache.org/docs/latest/api/sql/index.html#length) | 文字列の長さを返します |
| [`levenshtein`](https://spark.apache.org/docs/latest/api/sql/index.html#levenshtein) | 文字列間のレベンシュテイン距離を返す |
| [`locate`](https://spark.apache.org/docs/latest/api/sql/index.html#locate),  [`position`](https://spark.apache.org/docs/latest/api/sql/index.html#position) | サブ文字列の最初の出現箇所の位置を返します |
| [`map_concat`](https://spark.apache.org/docs/latest/api/sql/index.html#map_concat) | マップの連結 |
| [`map_keys`](https://spark.apache.org/docs/latest/api/sql/index.html#map_keys) | マップのキーを返す |
| [`map_values`](https://spark.apache.org/docs/latest/api/sql/index.html#map_values) | マップの値を返す |
| [`ntile`](https://spark.apache.org/docs/latest/api/sql/index.html#ntile) | 行を分割に分割 |
| [`nullif`](https://spark.apache.org/docs/latest/api/sql/index.html#nullif) | trueの場合はnullを返す |
| [`nvl`](https://spark.apache.org/docs/latest/api/sql/index.html#nvl) | nullの場合に値を返す |
| [`nvl2`](https://spark.apache.org/docs/latest/api/sql/index.html#nvl2) | nullでない場合は値を返す |
| [`parse_url`](https://spark.apache.org/docs/latest/api/sql/index.html#parse_url) | URLの一部を抽出します |
| [`rank`](https://spark.apache.org/docs/latest/api/sql/index.html#rank) | 値のランクを計算します |
| [`regexp_extract`](https://spark.apache.org/docs/latest/api/sql/index.html#regexp_extract) | 正規表現に一致するものを抽出します |
| [`regex_replace`](https://spark.apache.org/docs/latest/api/sql/index.html#regex_replace) | regexに一致するものを置き換えます。 |
| [`repeat`](https://spark.apache.org/docs/latest/api/sql/index.html#repeat) | 繰り返す文字列を返します |
| [`replace`](https://spark.apache.org/docs/latest/api/sql/index.html#replace) | 文字列のすべてのインスタンスを置換 |
| [`rollup`](https://spark.apache.org/docs/latest/api/sql/index.html#rollup) | 複数次元のロールアップの作成 |
| [`row_number`](https://spark.apache.org/docs/latest/api/sql/index.html#row_number) | 一意の行番号を割り当てます。 |
| [`schema_of_json`](https://spark.apache.org/docs/latest/api/sql/index.html#schema_of_json) | JSONのスキーマを返す |
| [`sentences`](https://spark.apache.org/docs/latest/api/sql/index.html#sentences) | 文字列を単語の配列に分割します。 |
| [`sequence`](https://spark.apache.org/docs/latest/api/sql/index.html#sequence) | 要素の配列を生成します |
| [`shiftleft`](https://spark.apache.org/docs/latest/api/sql/index.html#shiftleft) | 符号付きビット単位の左シフト |
| [`shiftright`](https://spark.apache.org/docs/latest/api/sql/index.html#shiftright) | 符号付きビット単位の右シフト |
| [`shiftrightunsigned`](https://spark.apache.org/docs/latest/api/sql/index.html#shiftrightunsigned) | 符号なしビット単位の右シフト |
| [`size`](https://spark.apache.org/docs/latest/api/sql/index.html#size) | 配列のサイズを返す |
| [`space`](https://spark.apache.org/docs/latest/api/sql/index.html#space) | [`n`](https://spark.apache.org/docs/latest/api/sql/index.html#n)スペースを含む文字列を返す |
| [`split`](https://spark.apache.org/docs/latest/api/sql/index.html#split) | 文字列の分割 |
| [`substring_index`](https://spark.apache.org/docs/latest/api/sql/index.html#substring_index) | サブ文字列のインデックスを返す |
| [`window`](https://spark.apache.org/docs/latest/api/sql/index.html#window) | ウィンドウ |
| [`xpath`](https://spark.apache.org/docs/latest/api/sql/index.html#xpath) | XMLノードの解析 |
| [`xpath_double`](https://spark.apache.org/docs/latest/api/sql/index.html#xpath_double),  [`xpath_number`](https://spark.apache.org/docs/latest/api/sql/index.html#xpath_number) | XMLノードの重複の解析 |
| [`xpath_float`](https://spark.apache.org/docs/latest/api/sql/index.html#xpath_float) | XMLノードのfloatの解析 |
| [`xpath_int`](https://spark.apache.org/docs/latest/api/sql/index.html#xpath_int) | XMLノードを整数に解析 |
| [`xpath_long`](https://spark.apache.org/docs/latest/api/sql/index.html#xpath_long) | XMLノードを長時間解析する |
| [`xpath_short`](https://spark.apache.org/docs/latest/api/sql/index.html#xpath_short) | XMLノードを解析して短い整数にする |
| [`xpath_string`](https://spark.apache.org/docs/latest/api/sql/index.html#xpath_string) | XMLノードの文字列の解析 |

### 現在の情報{#current-information}

| 関数 | 説明 |
| -------- | ----------- |
| [`current_database`](https://spark.apache.org/docs/latest/api/sql/index.html#current_database) | 現在のデータベースを返す |
| [`current_date`](https://spark.apache.org/docs/latest/api/sql/index.html#current_date) | 現在の日付を返します |
| [`current_timestamp`](https://spark.apache.org/docs/latest/api/sql/index.html#current_timestamp),  [`now`](https://spark.apache.org/docs/latest/api/sql/index.html#now) | 現在のタイムスタンプを返す |

### 上位関数{#higher-order}

| 関数 | 説明 |
| -------- | ----------- |
| [`transform`](https://spark.apache.org/docs/latest/api/sql/index.html#transform) | 配列の要素を変換する |
| [`exists`](https://spark.apache.org/docs/latest/api/sql/index.html#exists) | 要素が存在するかどうかを確認する |
| [`filter`](https://spark.apache.org/docs/latest/api/sql/index.html#filter) | 入力配列のフィルタリング |
| [`aggregate`](https://spark.apache.org/docs/latest/api/sql/index.html#aggregate) | すべての要素にバイナリ演算子を適用する |
