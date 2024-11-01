---
title: 特徴変換テクニック
description: 統計モデルトレーニング用のデータを準備する、データ変換、エンコーディング、機能スケーリングなどの基本的な前処理手法について説明します。 ここでは、欠落値の処理と、モデルのパフォーマンスと精度を向上させるためのカテゴリデータの変換の重要性について説明します。
role: Developer
source-git-commit: b248e8f8420b617a117d36aabad615e5bbf66b58
workflow-type: tm+mt
source-wordcount: '3437'
ht-degree: 9%

---

# 機能変換手法

変換は、データをモデルのトレーニングや分析に適した形式に変換またはスケールし、最適なパフォーマンスと精度を確保する重要な前処理ステップです。 このドキュメントは、補足的な構文リソースとして機能し、データの前処理に関する主な機能変換手法の詳細を提供します。

機械学習モデルでは、文字列値や null 値を直接処理できないので、データの前処理が不可欠になります。 このガイドでは、様々な変換を使用して、欠落値を取り込み、カテゴリデータを数値形式に変換し、ワンホットエンコーディングやベクトル化などの機能スケーリング手法を適用する方法について説明します。 これらのメソッドを使用すると、モデルはデータを解釈し、データから効果的に学習できるので、最終的にパフォーマンスが向上します。

## 自動フィーチャ変換 {#automatic-transformations}

`CREATE MODEL` コマンドの `TRANSFORM` 句をスキップする場合、フィーチャ変換は自動的に行われます。 自動データ前処理には、NULL 置換と標準機能変換（データタイプに基づく）が含まれます。 数値とテキストの列は自動的に入力され、フィーチャ変換が実行されて、データが機械学習モデルのトレーニングに適した形式になります。 このプロセスには、データ変換とカテゴリ、数値、ブール値の変換の欠落が含まれます。

>[!IMPORTANT]
>
>トレーニング時に使用する特徴変換は、予測および評価時の特徴変換にも使用されます。

次の表では、`CREATE MODEL` コマンド中に `TRANSFORM` 句が省略された場合の様々なデータ型の処理方法を説明します。

### Null 置換 {#automatic-null-replacement}

| データタイプ | Null 置換 |
|-----------------|-----------------------------------------------------|
| 数値 | Null はカラムの平均値に置き換えられます。 |
| 分類 | Null は `ml_unknown` キーワードに置き換えられます。 |
| ブール値 | Null は `FALSE` 値に置き換えられます。 |
| タイムスタンプ | これは、継続的なフィールドであると期待されます。 |
| ネスト/構造体 | 置き換えは、リーフノードのデータタイプによって異なります。 |

### 機能変換 {#automatic-feature-transformation}

| データタイプ | 機能変換 |
|-----------------|-----------------------------------------------------|
| 数値 | 不要 – このデータタイプは機械学習アルゴリズムで理解されます。 |
| 文字列 | 文字列のインデックス作成が発生します。 |
| ブール値 | 文字列のインデックス作成が発生します。 |
| タイムスタンプ | 操作は実行されません。 |
| 構造体 | 値がリーフノードに展開されます。 変換は、リーフノードのデータタイプに基づいて行われます。 |

**例**

```sql
CREATE model modelname options(model_type='logistic_reg', label='rating') AS SELECT * FROM movie_rating;
```

## 手動フィーチャー変換 {#manual-transformations}

`CREATE MODEL` ステートメントでカスタムデータの前処理を定義するには、`TRANSFORM` 句を使用可能な任意の数の変換関数と組み合わせて使用します。 これらの手動による前処理関数は、`TRANSFORM` 句の外部でも使用できます。 [ 以下のトランスフォーマーの節 ](#available-transformations) で説明するすべての変換を使用して、データを手動で前処理できます。

### 主な特徴

前処理関数を定義する際に考慮すべきフィーチャー変換の主な特性を以下に示します。

- **構文**: `TRANSFORM(functionName(colName, parameters) <aliasNAME>)`
   - エイリアス名は構文で必須です。 エイリアス名を指定する必要があります。指定しないと、クエリが失敗します。

- **パラメーター**：パラメーターは位置引数です。 つまり、各パラメーターは特定の値のみを取ることができます。 どの関数がどの引数を取るかについては、関連するドキュメントを参照してください。

- **チェーントランス**：あるトランスの出力が別のトランスの入力になる場合があります。

- **機能の使用状況**：最後の機能変換は、機械学習モデルの機能として使用されます。

**例**

```sql
CREATE MODEL modelname 
TRANSFORM(
  string_imputer(language, 'adding_null') AS imp_language, 
  numeric_imputer(users_count, 'mode') AS imp_users_count, 
  string_indexer(imp_language) AS si_lang,  
  vector_assembler(array(imp_users_count, si_lang, watch_minutes)) AS features
)  
OPTIONS(MODEL_TYPE='logistic_reg', LABEL='rating') 
AS SELECT * FROM df;
```

## 使用可能な変換 {#available-transformations}

利用可能な変換は 19 個あります。 これらの変換は、[ 一般変換 ](#general-transformations)、[ 数値変換 ](#numeric-transformations)、[ カテゴリ変換 ](#categorical-transformations) および [ テキスト変換 ](#textual-transformations) に分割されます。

### 一般的な変換 {#general-transformations}

幅広いデータ型に使用される変圧器の詳細については、この節を参照してください。 この情報は、カテゴリデータやテキストデータに固有でない変換を適用する必要がある場合に不可欠です。

>[!NOTE]
>
>入力データタイプは、代入が適用される列を参照します。 出力データタイプとは、変換が有効になった後に出力として生成される列を指します。

#### 数値インプター {#numeric-imputer}

**数値入力** 変換サービスは、データセット内の欠落値を補完します。 欠落している値が含まれている列の平均値、中央値、モードのいずれかを使用します。 入力列は `DoubleType` または `FloatType` にしてください。 詳細と例については、[Spark アルゴリズムのドキュメント ](https://spark.apache.org/docs/2.2.0/ml-features.html#imputer) を参照してください。

>[!NOTE]
>
>入力列内のすべての null 値は欠落値として扱われるので、結果も返されます。

**データタイプ**

- 入力データタイプ：数値
- 出力データタイプ：数値

**定義**

```sql
transformer(numeric_imputer(hour, 'mean') hour_imputed)
```

**パラメーター**

| パラメーター | 説明 | タイプ | デフォルト | オプション |
| -------- | ------------ | ----- | -------- | -------- |
| `STRATEGY` | 偽装戦略。 使用可能な値は [`mean`、`median`、`mode`] です。 | 文字列 | mean | optional |

{style="table-layout:auto"}

**偽装の前の例**

| ID | hour |
|---|---|
| 0 | 18.0 |
| 1 | null |
| 2 | 8.0 |

**帰属後の例（平均戦略を使用）**

| ID | hour |
|---|---|
| 0 | 18.0 |
| 1 | 13.0 |
| 2 | 8.0 |

#### 文字列演算子 {#string-imputer}

**String imputer** トランスフォーマーは、ユーザーから関数引数として提供された文字列を使用して、データセット内の欠落値を補完します。 入力列と出力列は、`string` のデータタイプである必要があります。

>[!NOTE]
>
>入力列内のすべての null 値は欠落として扱われ、指定された文字列に置き換えられます。

**データタイプ**

- 入力データタイプ：文字列
- 出力データタイプ：文字列

**定義**

```sql
transform(string_imputer(name, 'unknown_name') as name_imputed)
```

**パラメーター**

| パラメーター | 説明 | タイプ | デフォルト | オプション |
| -------- | ------------ | ----- | -------- | -------- |
| `NULL_REPLACEMENT` | null を置き換える値。 | 文字列 | ml_unknown | optional |

{style="table-layout:auto"}

**偽装の前の例**

| ID | name |
|---|---|
| 0 | John |
| 1 | null |
| 2 | アリス |

**変換後の例（「ml_unknown」を置換として使用）**

| ID | name |
|---|---|
| 0 | John |
| 1 | ml_unknown |
| 2 | アリス |

#### ブール演算子 {#imputer}

**ブール演算子** 変換は、ブール値列のデータセットの欠落値を補完します。 入力列と出力列は `Boolean` 型にする必要があります。

>[!NOTE]
>
>入力列内のすべての null 値は欠落として扱われ、指定されたブール値に置き換えられます。

**データタイプ**

- 入力データタイプ：ブール値
- 出力データタイプ：ブール値

**定義**

```sql
transform(boolean_imputer(name, true) as name_imputed)
```

**パラメーター**

| パラメーター | 説明 | タイプ | デフォルト | オプション |
| -------- | ------------ | ----- | -------- | -------- |
| `NULL_REPLACEMENT` | ブール演算子。 使用できる値：[`true`、`false`]。 | ブール値 | false | optional |

**偽装の前の例**

| ID | フラグ |
|---|---|
| 0 | true |
| 1 | null |
| 2 | false |

**変換後の例（「true」を置き換えに使用）**

| ID | フラグ |
|---|---|
| 0 | true |
| 1 | true |
| 2 | false |

#### ベクトルアセンブラ {#vector-assembler}

`VectorAssembler` 変圧器は、指定された入力列のリストを 1 つのベクトル列に組み合わせるので、機械学習モデルの複数の機能を管理しやすくなります。 これは、生の特徴と、異なる特徴トランスフォーマーによって生成された特徴を 1 つの統一された特徴ベクトルにマージする場合に特に便利です。 `VectorAssembler` には、数値、ブール値、およびベクトル型の入力列を使用できます。 各行では、入力列の値が指定した順序でベクトルに連結されます。

<!-- More information and examples can be found in the [Spark algorithm documentation](https://spark.apache.org/docs/2.2.0/ml-features.html#vectorassembler) -->

**データタイプ**

- 入力データタイプ：`array[string]` （数値/配列 [ 数値 ] 値を含む列名）
- 出力データタイプ : `Vector[double]`

**定義**

```sql
transform(vector_assembler(id, hour, mobile, userFeatures) as features)
```

**パラメーター**

| パラメーター | 説明 | タイプ | デフォルト | オプション |
| -------- | ------------ | ----- | -------- | -------- |
| 該当なし | この変圧器には追加のパラメータは必要ありません。 | 該当なし | 該当なし | 該当なし |

{style="table-layout:auto"}

**変換前の例**

| ID | hour | モバイル | userFeature | クリック |
|---|-------|--------|------------------|---------|
| 0 | 18 | 1.0 | [0.0、10.0、0.5] | 1.0 |

{style="table-layout:auto"}

**変換後の例**

| ID | hour | モバイル | userFeature | クリック | features |
|---|------|--------|------------------|---------|-------------------------------|
| 0 | 18 | 1.0 | [0.0、10.0、0.5] | 1.0 | [18.0、1.0、0.0、10.0、0.5] |

{style="table-layout:auto"}

### 数値変換 {#numeric-transformations}

数値データの処理とスケーリングに使用できるトランスフォーマーについては、この節を参照してください。 これらのトランスフォーマーは、データセットの数値機能を処理および最適化するために必要です。

#### 二値化器 {#binarizer}

`Binarizer` 変圧器は、二値化と呼ばれるプロセスによって数値特性を二項（0/1）特性に変換します。 指定したしきい値を超えるフィーチャ値は 1.0 に変換され、しきい値以下のフィーチャ値は 0.0 に変換されます。`Binarizer` は、入力列の `Vector` 型と `Double` 型の両方をサポートします。

<!-- More information and examples can be found in the [Spark algorithm documentation](https://spark.apache.org/docs/2.2.0/ml-features.html#binarizer). -->

**データタイプ**

- 入力データタイプ：数値列
- 出力データタイプ：数値

**定義**

```sql
transform(numeric_imputer(rating, 'mode') rating_imp, binarizer(rating_imp) rating_binarizer)
```

**パラメーター**

| パラメーター | 説明 | タイプ | デフォルト | オプション |
|------------|----------------------------------------------------------------------------------------------------------|----------|----------|----------|
| `THRESHOLD` | 連続特性の二値化に使用されるしきい値のパラメーター。 しきい値を超える機能は 2 値化されて 1.0 になり、しきい値以下の機能は 2 値化されて 0.0 になります。 | int/double | 0.0 | optional |

{style="table-layout:auto"}

**二値化前の入力例**

| ID | 格付け |
|---|---------|
| 0 | -18.0 |
| 1 | 13.0 |
| 2 | 8.0 |

**二値化後の出力例（デフォルトのしきい値は 0.0）**

| ID | 格付け |
|---|---------|
| 0 | 0.0 |
| 1 | 1.0 |
| 2 | 1.0 |

**カスタムしきい値を使用した定義**

```sql
transform(numeric_imputer(age, 'mode') age_imp, binarizer(age_imp, 14.0) age_binarizer)
```

**二値化後の出力例（しきい値が 14.0）**

| ID | 年齢 |
|---|-------|
| 0 | 0.0 |
| 1 | 0.0 |
| 2 | 1.0 |

#### バケタイザー {#bucketizer}

`Bucketizer` 変圧器は、ユーザが指定したしきい値に基づいて、連続するフィーチャの列をフィーチャ グループの列に変換します。 このプロセスは、連続データを個別の bin またはバケットにセグメント化する場合に役立ちます。 `Bucketizer` には、バケットの境界を定義する `splits` パラメーターが必要です。

**データタイプ**

- 入力データタイプ：数値列
- 出力データタイプ：数値（連結された値）

**定義**

```sql
TRANSFORM(binarizer(time_spent, 5.0) as binary, bucketizer(course_duration, array(-440.5, 0.0, 150.0, 1000.7)) as buck_features, vector_assembler(array(buck_features, users_count, binary)) as vec_assembler, max_abs_scaler(vec_assembler) as maxScaling, min_max_scaler(maxScaling) as features)
```

**パラメーター**

| パラメーター | 説明 | タイプ | デフォルト | オプション |
|----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|----------|----------|
| `splits` | 連続したフィーチャをバケットにマッピングするためのパラメータ。 `n+1` 分割では、バケツが `n` 個あります。 分割は厳密に昇順にする必要があり、範囲（x,y）は、y を含む最後のバケットを除いて、各バケットに使用されます。 | array （double） | なし | optional |

{style="table-layout:auto"}

**分割の例**

- 配列（Double.NegativeInfinity, 0.0, 1.0, Double.PositiveInfinity）
- 配列（0.0, 1.0, 2.0）

分割は、Double 値の範囲全体に適用する必要があります。そうしないと、指定した分割以外の値はエラーとして扱われます。

**変換の例**

この例では、連続機能の列（`course_duration`）を取り出し、指定された `splits` に従って bin で結合し、結果のバケットを他の機能と組み立てます。

```sql
TRANSFORM(binarizer(time_spent, 5.0) as binary, bucketizer(course_duration, array(-440.5, 0.0, 150.0, 1000.7)) as buck_features, vector_assembler(array(buck_features, users_count, binary)) as vec_assembler, max_abs_scaler(vec_assembler) as maxScaling, min_max_scaler(maxScaling) as features)
```

#### MinMaxScaler {#minmaxscaler}

`MinMaxScaler` トランスフォーマーは、ベクトル行のデータセット内の各機能を指定された範囲（通常は [0、1] に再スケールします。 これにより、すべてのフィーチャがモデルに等しく作用するようになります。 これは、グラデーション下降ベースのアルゴリズムなど、フィーチャのスケーリングに敏感なモデルで特に便利です。 この `MinMaxScaler` は、次のパラメーターで動作します。

- **min**：すべての特性で共有される変換の下限。 デフォルトは `0.0` です。
- **max**：変換の上限。すべての機能で共有されます。 デフォルトは `1.0` です。

<!-- More information and examples can be found in the [Spark algorithm documentation](https://spark.apache.org/docs/2.2.0/ml-features.html#minmaxscaler).  -->

**データタイプ**

- 入力データ型：`Array[Double]`
- 出力データタイプ : `Array[Double]`

**定義**

```sql
TRANSFORM(binarizer(time_spent, 5.0) as binary, bucketizer(course_duration, array(-440.5, 0.0, 150.0, 1000.7)) as buck_features, vector_assembler(array(buck_features, users_count, binary)) as vec_assembler, max_abs_scaler(vec_assembler) as maxScaling, min_max_scaler(maxScaling) as features)
```

**パラメーター**

| パラメーター | 説明 | タイプ | デフォルト | オプション |
|-----------|--------------------------------------------------------------------------------------------------|------|---------|----------|
| `min` | 変換後の下限。すべての機能で共有されます。 | double | 0.0 | optional |
| `max` | 変換後の上限。すべての機能で共有されます。 | double | 1.0 | optional |

**変換の例**

次の使用例は、他のいくつかの変換を適用した後に、MinMaxScaler を使用してフィーチャのセットを指定された範囲に再スケールして変換します。

```sql
TRANSFORM(binarizer(time_spent, 5.0) as binary, bucketizer(course_duration, array(-440.5, 0.0, 150.0, 1000.7)) as buck_features, vector_assembler(array(buck_features, users_count, binary)) as vec_assembler, max_abs_scaler(vec_assembler) as maxScaling, min_max_scaler(maxScaling) as features)
```

#### MaxAbsScaler {#maxabsscaler}

`MaxAbsScaler` 変圧器は、ベクトル行のデータセット内の各特徴を、各特徴の最大絶対値で割って [-1,1] の範囲に再スケールする。 この変換は、データがシフトしたり中央に配置されたりすることがないので、正の値と負の値の両方を持つデータセットで希薄さを維持するのに最適です。 これにより、距離計算を含むモデルなど、入力フィーチャの尺度に敏感なモデルに特に適した `MaxAbsScaler` ールが得られます。

<!-- More information and examples can be found in the [Spark algorithm documentation](https://spark.apache.org/docs/2.2.0/ml-features.html#maxabsscaler). -->

**データタイプ**

- 入力データ型：`Array[Double]`
- 出力データタイプ : `Array[Double]`

**定義**

```sql
TRANSFORM(binarizer(time_spent, 5.0) as binary, bucketizer(course_duration, array(-440.5, 0.0, 150.0, 1000.7)) as buck_features, vector_assembler(array(buck_features, users_count, binary)) as vec_assembler, max_abs_scaler(vec_assembler) as maxScaling)
```

**パラメーター**

| パラメーター | 説明 | タイプ | デフォルト | オプション |
|-----------|---------------------------------------------------------------------------------------------------------|------|---------|----------|
| 該当なし | MaxAbsScaler の操作には、追加のパラメータは必要ありません。 | 該当なし | 該当なし | 該当なし |

**変換の例**

この例では、`MaxAbsScaler` を含むいくつかの変換を適用して、フィーチャを [-1, 1] の範囲に再スケールします。

```sql
TRANSFORM(binarizer(time_spent, 5.0) as binary, bucketizer(course_duration, array(-440.5, 0.0, 150.0, 1000.7)) as buck_features, vector_assembler(array(buck_features, users_count, binary)) as vec_assembler, max_abs_scaler(vec_assembler) as maxScaling)
```

#### Normalizer {#normalizer}

`Normalizer` は、ベクトル行のデータセット内の各ベクトルを正規化して単位ノルムを持たせるトランスフォーマーです。 このプロセスにより、ベクトルの方向を変更することなく、一貫したスケールが保証されます。 この変換は、特にベクトルの大きさが大きく変化する場合に、距離測定やその他のベクトルベースの計算に依存する機械学習モデルで特に役立ちます。

<!-- More information and examples can be found in the [Spark algorithm documentation](https://spark.apache.org/docs/2.2.0/ml-features.html#normalizer) -->

**データタイプ**

- 入力データタイプ : `array[double]` / `vector[double]`
- 出力データタイプ : `vector[double]`

**定義**

```sql
TRANSFORM(binarizer(time_spent, 5.0) as binary, bucketizer(course_duration, array(-440.5, 0.0, 150.0, 1000.7)) as buck_features, vector_assembler(array(buck_features, users_count, binary)) as vec_assembler, normalizer(vec_assembler, 3) as normalized)
```

**パラメーター**

| パラメーター | 説明 | タイプ | デフォルト | オプション |
|-----------|----------------------------------------------------------------------------------------|---------|---------|----------|
| `p` | 正規化に使用する `p-norm` を指定します（例：`1-norm`、`2-norm` など）。 | 整数 | 2 | optional |

**変換の例**

この例では、`Normalizer` を含む複数の変換を適用して、指定された `p-norm` を使用して一連の機能を正規化する方法を示します。

```sql
TRANSFORM(binarizer(time_spent, 5.0) as binary, bucketizer(course_duration, array(-440.5, 0.0, 150.0, 1000.7)) as buck_features, vector_assembler(array(buck_features, users_count, binary)) as vec_assembler, normalizer(vec_assembler, 3) as normalized)
```

#### QuantileDiscretizer {#quantilediscretizer}

`QuantileDiscretizer` は、連続特性を持つ列を、`numBuckets` パラメーターによって決定された bin の数で、連結されたカテゴリ特性に変換するトランスフォーマーです。 場合によっては、十分な量の数値を作成するのに十分な明確な値が少なすぎる場合、実際のバケット数が指定した数よりも小さくなる可能性があります。

この変換は、連続データの表現を簡略化する場合や、カテゴリ入力でより適切に機能するアルゴリズムに対してデータを準備する場合に特に役立ちます。

**データタイプ**

- 入力データタイプ：数値列
- 出力データタイプ：数値列（カテゴリ順）

**定義**

```sql
TRANSFORM(quantile_discretizer(hour, 3) as result)
```

**パラメーター**

| パラメーター | 説明 | タイプ | デフォルト | オプション |
|--------------|--------------------------------------------------------------------------------------------------------------------------|---------|---------|----------|
| `NUM_BUCKETS` | データポイントがグループ化されるバケット数（分数またはカテゴリ）。 この数は 2 以上である必要があります。 | 整数 | 2 | optional |

**変換の例**

この例では、`QuantileDiscretizer` が連続特性（`hour`）の列を 3 つのカテゴリ別に連結する方法を示しています。

```sql
TRANSFORM(quantile_discretizer(hour, 3) as result)
```

**離散化の前後の例**

| ID | hour | result |
|---|------|--------|
| 0 | 18.0 | 2.0 |
| 1 | 19.0 | 2.0 |
| 2 | 8.0 | 1.0 |
| 3 | 5.0 | 1.0 |
| 4 | 2.2 | 0.0 |

#### StandardScaler {#standardscaler}

`StandardScaler` は、ベクトル行のデータセット内の各特性を正規化して、単位標準偏差やゼロ平均を持つトランスフォーマーです。 このプロセスにより、一貫したスケールでゼロを中心とした特性を想定したアルゴリズムにデータが適するようになります。 この変換は、SVM、ロジスティック回帰、ニューラルネットワークなどの機械学習モデルで特に重要です。これらのモデルでは、標準化されていないデータが収束の問題や精度の低下につながる可能性があります。

<!-- More information and examples can be found in the [Spark algorithm documentation](https://spark.apache.org/docs/2.2.0/ml-features.html#standardscaler).  -->

**データタイプ**

- 入力データタイプ : ベクトル
- 出力データタイプ：ベクトル

**定義**

```sql
TRANSFORM(standard_scaler(feature) as ss_features)
```

**パラメーター**

| パラメーター | 説明 | タイプ | デフォルト | オプション |
|------------|------------------------------------------------------------------------------------------------------|---------|---------|----------|
| `withStd` | 単位標準偏差を持つようにデータをスケールします。 | ブール値 | True | optional |
| `withMean` | スケーリング前に平均でデータを中央に配置します。 このオプションを選択すると出力が密になるので、入力が疎の場合は注意が必要です。 | ブール値 | False | optional |

**変換の例**

この例では、一連のフィーチャに StandardScaler を適用し、それらを単位標準偏差とゼロ平均で正規化する方法を示します。

```sql
TRANSFORM(standard_scaler(feature) as ss_features)
```

### カテゴリ変換 {#categorical-transformations}

機械学習モデルのカテゴリデータを変換し、前処理するために設計された使用可能なトランスフォーマーの概要については、この節を参照してください。 これらの変換は、数値ではなく、個別のカテゴリやラベルを表すデータポイントに対して設計されています。

#### StringIndexer {#stringindexer}

`StringIndexer` は、ラベルの文字列列を数値インデックスの列にエンコードするトランスフォーマーです。 インデックスの範囲は 0 ～ `numLabels` で、ラベルの頻度順に並べられます（最も頻繁に使用されるラベルは 0 のインデックスを受け取ります）。 入力列が数値の場合は、インデックス作成の前に文字列にキャストされます。 ユーザーが指定した場合は、見えないラベルをインデックス `numLabels` に割り当てることができます。

この変換は、カテゴリ文字列データを数値形式に変換する場合に特に役立ち、数値入力が必要な機械学習モデルに適しています。

<!-- More information and examples can be found in the [Spark algorithm documentation](https://spark.apache.org/docs/2.2.0/ml-features.html#stringindexer) -->

**データタイプ**

- 入力データタイプ：文字列
- 出力データタイプ：数値

**定義**

```sql
TRANSFORM(string_indexer(category) as si_category)
```

**パラメーター**

| パラメーター | 説明 | タイプ | デフォルト | オプション |
|-----------|-------------|------|---------|----------|
| 該当なし | `StringIndexer` の操作には、追加のパラメーターは必要ありません。 | 該当なし | 該当なし | 該当なし |

**変換の例**

この例では、`StringIndexer` をカテゴリ特性に適用し、数値インデックスに変換する方法を示します。

```sql
TRANSFORM(string_indexer(category) as si_category)
```

#### OneHotEncoder {#onehotencoder}

`OneHotEncoder` は、ラベルインデックスの列をスパースなバイナリベクトルの列に変換するトランスフォーマーで、各ベクトルは最大で 1 つの値を持ちます。 このエンコーディングは、ロジスティック回帰などの数値入力を必要とするアルゴリズムで、カテゴリデータを効果的に取り込む場合に特に便利です。

<!-- More information and examples can be found in the [Spark algorithm documentation](https://spark.apache.org/docs/2.2.0/ml-features.html#onehotencoder).  -->

**データタイプ**

- 入力データタイプ：数値
- 出力データタイプ：Vector[Int]

**定義**

```sql
TRANSFORM(string_indexer(category) as si_category, one_hot_encoder(si_category) as ohe_category)
```

**パラメーター**

| パラメーター | 説明 | タイプ | デフォルト | オプション |
|-----------|-------------|------|---------|----------|
| 該当なし | OneHotEncoder の操作には、追加のパラメーターは必要ありません。 | 該当なし | 該当なし | 該当なし |

**変換の例**

この例では、最初に `StringIndexer` をカテゴリ特性に適用し、次にその `OneHotEncoder` を使用してインデックス付きの値をバイナリ ベクトルに変換する方法を示します。

```sql
TRANSFORM(string_indexer(category) as si_category, one_hot_encoder(si_category) as ohe_category)
```

### テキスト変換 {#textual-transformations}

この節では、テキストデータの処理と、機械学習モデルで使用可能な形式への変換に使用できるトランスフォーマーについて詳しく説明します。 このセクションは、自然言語データとテキスト分析を扱う開発者にとって重要です。

#### CountVector {#countvectorizer}

`CountVectorizer` は、テキストドキュメントの集まりをトークン数のベクトルに変換し、コーパスから抽出された語彙に基づいてスパース表現を生成するトランスフォーマーです。 この変換は、テキストデータを、各ドキュメント内のトークンの頻度を表すことで、LDA （Lidual Dirichtlet Allocation）などの機械学習アルゴリズムで使用できる数値形式に変換するために不可欠です。

<!-- More information and examples can be found in the [Spark algorithm documentation](https://spark.apache.org/docs/2.2.0/ml-features.html#countvectorizer). -->

**データタイプ**

- 入力データタイプ：配列 [ 文字列 ]
- 出力データタイプ：密ベクトル

**定義**

```sql
TRANSFORM(count_vectorizer(texts) as cv_output)
```

**パラメーター**

| パラメーター | 説明 | タイプ | デフォルト | オプション |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------|---------|----------|
| `VOCAB_SIZE` | 語彙の最大サイズ。 CountVectorizer は、コーパス全体の用語の頻度で並べ替えられた上位 `vocabSize` 用語のみを考慮した語彙を作成します。 | Int | 218 | optional |
| `MIN_DOC_FREQ` | 語彙に含める語句を表示する必要がある異なるドキュメントの最小数を指定します。 絶対数または分数（倍精度浮動小数点数の場合）を指定できます。 | Double | 1.0 | optional |
| `MAX_DOC_FREQ` | 用語が語彙に含まれるように表示されるドキュメントの最大数を指定します。 絶対数または分数（倍精度浮動小数点数の場合）を指定できます。 | Double | （263）–1 | optional |
| `MIN_TERM_FREQ` | ドキュメント内のまれな単語を除外します。 頻度/カウントが指定されたしきい値よりも小さい用語は無視されます。 絶対数またはドキュメントのトークン数の一部を指定できます。 | Double | 1.0 | optional |

{style="table-layout:auto"}

**変換の例**

この例では、CountVectorizer がテキスト配列のコレクションをトークン数のベクトルに変換し、疎表現を生成する方法を示しています。

```sql
TRANSFORM(count_vectorizer(texts) as cv_output)
```

**ベクトル化の前後の例**

| ID | テキスト | cv_output |
|----|---------------------------------|-----------------------------------|
| 0 | 配列（&quot;a&quot;, &quot;b&quot;, &quot;c&quot;） | （3,[0,1,2],[1.0,1.0,1.0]） |
| 1 | 配列（&quot;a&quot;, &quot;b&quot;, &quot;b&quot;, &quot;c&quot;, &quot;a&quot;） | （3,[0,1,2],[2.0,2.0,1.0]） |

{style="table-layout:auto"}

#### NGram {#ngram}

`NGram` は、n-gram のシーケンスを生成するトランスフォーマーであり、n-gram は、ある整数（`𝑛`）の（&#39;??&#39;）トークンのシーケンス（通常は単語）です。 出力は、「??」の連続する単語のスペース区切り文字列で構成されており、機械学習モデル、特に自然言語処理に焦点を当てたモデルの機能として使用できます。

<!-- More information and examples can be found in the [Spark algorithm documentation](https://spark.apache.org/docs/2.2.0/ml-features.html#n-gram). -->

**データタイプ**

- 入力データタイプ：配列 [ 文字列 ]
- 出力データタイプ：配列 [ 文字列 ]

**定義**

```sql
TRANSFORM(tokenizer(review_comments) as token_comments, ngram(token_comments, 3) as n_tokens)
```

**パラメーター**

| パラメーター | 説明 | タイプ | デフォルト | オプション |
|-----------|-----------------------------------------------------------------------------------------------|---------|-------------------|----------|
| `N` | n-gram の最小長は、1 以上である必要があります。 | 整数 | 2 （bigram 機能） | optional |

{style="table-layout:auto"}

**変換の例**

この例では、NGram トランスフォーマーがテキストデータから派生したトークンのリストから 3 グラムのシーケンスを作成する方法を示しています。

```sql
TRANSFORM(tokenizer(review_comments) as token_comments, ngram(token_comments, 3) as n_tokens)
```

**n グラム変換前後の例**

| ID | テキスト | n_tokens |
|----|-------------------------------------------------------|-------------------------------------------------------|
| 0 | [this」「was」「an」「entering」「movie」 ] | [ 「これは面白かった」「面白かった」「面白い映画だった」 ] |

{style="table-layout:auto"}

#### StopWordsRemover {#stopwordsremover}

`StopWordsRemover` は、文字列のシーケンスからストップワードを削除し、重要な意味を持たない一般的な単語をフィルタリングするトランスフォーマーです。 これは、文字列のシーケンス（トークナイザの出力など）を入力として受け取り、`stopWords` パラメータで指定されたすべてのストップワードを削除します。

この変換は、テキストデータの前処理に役立ち、全体的な意味にあまり寄与しない単語を排除することで、ダウンストリーム機械学習モデルの有効性を高めることができます。

<!-- More information and examples can be found in the [Spark algorithm documentation](https://spark.apache.org/docs/2.2.0/ml-features.html#stopwordsremover) -->

**データタイプ**

- 入力データタイプ：配列 [ 文字列 ]
- 出力データタイプ：配列 [ 文字列 ]

**定義**

```sql
TRANSFORM(stop_words_remover(raw) as filtered)
```

**パラメーター**

| パラメーター | 説明 | タイプ | デフォルト | オプション |
|--------------------|--------------------------------------------------------------------------------------------------|---------------|-------------------------|----------|
| `stopWords` | 除外する単語。 | 配列 [ 文字列 ] | デフォルト：英語のストップワード | optional |

{style="table-layout:auto"}

<!-- Q) should this be the `CUSTOM_STOP_WORDS` parameter or the `stopWords` parameter?  -->

**変換の例**

この例では、`StopWordsRemover` がトークンのリストから一般的な英語のストップワードを除外する方法を示しています。

```sql
TRANSFORM(stop_words_remover(raw) as filtered)
```

**ストップワード削除の前後の例**

| ID | 未加工 | filtered |
|----|------------------------------|--------------------------|
| 0 | [ わたし、見た、赤、気球 ] | [ のこぎり、赤、風船 ] |
| 1 | [ メアリ、持っていた、小さな、子羊 ] | [ メアリ、小さな子羊 ] |

**カスタムのストップワードを使用した例**

この例では、ストップワードのカスタム・リストを使用して、入力シーケンスから特定のワードを除外する方法を示します。

```sql
TRANSFORM(stop_words_remover(raw, array("red", "I", "had")) as filtered)
```

**カスタムストップワード削除の前後の例**

| ID | 未加工 | filtered |
|----|------------------------------|--------------------------|
| 0 | [ わたし、見た、赤、気球 ] | [ のこぎり、気球 ] |
| 1 | [ メアリ、持っていた、小さな、子羊 ] | [ メアリ、小さな、子羊 ] |

#### TF-IDF {#tf-idf}

`TF-IDF` （Term Frequency-Inverse Document Frequency）は、コーパスに対する文書内の単語の重要度を測定するために使用されるトランスフォーマーです。 用語頻度（TF）は用語\（t\）が文書\（d\）に現れる回数を表し、文書頻度（DF）はコーパス \（D\）内の\（t\）という用語を含む文書数を測定します。 この方法は、「a」、「the」、「of」など、ユニークな情報をほとんど持たない一般的に使用される単語の影響を軽減するために、テキストマイニングで広く使用されています。

この変換は、ドキュメント内およびコーパス全体での各単語の重要度に数値を割り当てるので、テキストマイニングおよび自然言語処理タスクで特に役立ちます。

<!-- More information and examples can be found in the [Spark algorithm documentation](https://spark.apache.org/docs/2.2.0/ml-features.html#tf-idf) -->

**データタイプ**

- 入力データタイプ：配列 [ 文字列 ]
- 出力データタイプ：Vector[Int]

**定義**

```sql
create table td_idf_model transform(tokenizer(sentence) as token_sentence, tf_idf(token_sentence) as tf_sentence, vector_assembler(array(tf_sentence)) as feature) OPTIONS()
```

**パラメーター**

| パラメーター | 説明 | タイプ | デフォルト | オプション |
|-----------------|----------------------------------------------------------------------------------------|------|---------|----------|
| `NUM_FEATURES` | 生成する機能の数。 0 より大きい値を設定する必要があります。 | Int | 262144 | optional |
| `MIN_DOC_FREQ` | 用語をモデルに含めるように表示する必要があるドキュメントの最小数です。 | Int | 0 | optional |

{style="table-layout:auto"}

**変換の例**

この例では、TF-IDF を使用して、トークン化された文を、コーパス全体のコンテキストでの各用語の重要性を表す機能ベクトルに変換する方法を示します。

```sql
create table td_idf_model transform(tokenizer(sentence) as token_sentence, tf_idf(token_sentence) as tf_sentence, vector_assembler(array(tf_sentence)) as feature) OPTIONS()
```

#### Tokenizer {#tokenizer}

`Tokenizer` は、文などのテキストを個別の用語（通常は単語）に分解するトランスフォーマーです。 文をトークンの配列に変換し、さらなるテキスト分析やモデリング処理のためにデータを準備する、テキストの前処理の基本的な手順を提供します。

<!-- More information and examples can be found in the [Spark algorithm documentation](https://spark.apache.org/docs/2.2.0/ml-features.html#tokenizer) -->

**データタイプ**

- 入力データタイプ：テキスト文
- 出力データタイプ：配列 [ 文字列 ]

**定義**

```sql
create table td_idf_model transform(tokenizer(sentence) as token_sentence, tf_idf(token_sentence) as tf_sentence, vector_assembler(array(tf_sentence)) as feature) OPTIONS()
```

**パラメーター**

| パラメーター | 説明 | タイプ | デフォルト | オプション |
|-----------|-------------|------|---------|----------|
| 該当なし | `Tokenizer` の操作に追加のパラメーターは必要ありません。 | 該当なし | 該当なし | 該当なし |

**変換の例**

この例では、`Tokenizer` がテキスト処理パイプラインの一部として文を個々の単語（トークン）に分割する方法を示しています。

```sql
create table td_idf_model transform(tokenizer(sentence) as token_sentence, tf_idf(token_sentence) as tf_sentence, vector_assembler(array(tf_sentence)) as feature) OPTIONS()
```

#### Word2Vec {#word2vec}

`Word2Vec` は、ドキュメントを表す単語のシーケンスを処理し、文 `Word2VecModel` をトレーニングする推定量です。 このモデルは、各単語を固有の固定サイズベクトルにマッピングし、文書内の全単語のベクトルを平均化することにより、各文書をベクトルに変換する。 `Word2Vec` は、自然言語処理タスクで広く使用されており、意味論的意味を取り込む単語の埋め込みを作成し、テキストデータを単語間の関係を表す数値ベクトルに変換し、より効果的なテキスト分析と機械学習モデルを可能にします。

<!-- More information and examples can be found in the [Spark algorithm documentation](https://spark.apache.org/docs/2.2.0/ml-features.html#word2vec) -->

**データタイプ**

- 入力データタイプ：配列 [ 文字列 ]
- 出力データタイプ：Vector[Double]

**定義**

```sql
TRANSFORM(tokenizer(review) as tokenized, word2Vec(tokenized, 10, 1) as word2Vec)
```

**パラメーター**

| パラメーター | 説明 | タイプ | デフォルト | オプション |
|--------------|-----------------------------------------------------------------------------------------------------|---------|---------|----------|
| `VECTOR_SIZE` | 各単語が変換されるベクトルの次元。 | 整数 | 100 | optional |
| `MIN_COUNT` | トークンが `Word2Vec` モデルのボキャブラリに含まれるように見える最小回数。 | 整数 | 5 | optional |

{style="table-layout:auto"}

**変換の例**

次の例は、トークン化され `Word2Vec` レビューを、文書内の単語ベクトルの平均を表す固定サイズのベクトルに変換する方法を示しています。

```sql
TRANSFORM(tokenizer(review) as tokenized, word2Vec(tokenized, 10, 1) as word2Vec)
```

**Word2Vec 変換の前後の例**

| レビュー | トークン化 | word2Vec |
|-------------------------------|--------------------------------------|---------------------------------|
| これは面白い映画だった | [ これは、面白い、映画だった ] | [-0.025713888928294182,0.00818799751577899,0.0092235435731709,-0.01515385233797133,0.012175946310162545,3.1129065901041035E-4,0.0025145105042611252,0.005757019785232843,-0.021328244300093502,0.009335877187550069] |

{style="table-layout:auto"}
