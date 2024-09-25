---
title: Experience Query Service のハイパーキューブを使用した効率的なビッグデータ分析
description: Adobe Experience Platform クエリサービスでハイパーキューブを使用して、近似ユニークカウント機能でビッグデータ分析を最適化し、完全なデータの再処理の必要性を減らす方法について説明します。
source-git-commit: e21eab682b2d29da4441a32d7307af7cf873f295
workflow-type: tm+mt
source-wordcount: '1499'
ht-degree: 2%

---

# ハイパーキューブによる効率的なビッグデータ分析

>[!AVAILABILITY]
>
>この機能は、[Data Distiller SKU](./data-distiller/overview.md) を購入したユーザーのみが利用できます。 詳しくは、Adobe担当者にお問い合わせください。

Adobe Experience Platform Experience Query Service でハイパーキューブを使用して、効率を向上させた高度なデータ分析を実行する方法を説明します。 このドキュメントでは、[[!DNL Apache Datasketches]  ライブラリ ](https://datasketches.apache.org/) の高度な関数を使用して、履歴データを毎回再処理しなくても、個別のカウントと複雑な計算を増分的に処理する方法を説明します。

ビッグデータ分析では、個別カウント、分位数、最頻項目、結合、グラフ分析などの指標の生成に、加算を伴わないカウントが必要になることがよくあります（結果をサブグループから単純に合計できない場合）。 従来の方法では、すべての履歴データを再処理する必要があり、リソースに負担がかかり、時間がかかる場合があります。 スケッチ（大きなデータセットを表す確率を使用するコンパクトな要約）と、高度なクエリサービス機能を使用して、再計算の必要性を減らすことで、このプロセスを合理化します。

## ハイパーキューブの主な機能 {#key-functions}

Hypercubes は、データ分析の効率と柔軟性を向上させるいくつかの強力な機能を提供します。

1. **一意のユーザーまたは個別クエリをカウント**:SQL 機能を使用して、生データを繰り返し再分析することなく、製品表示、サイト訪問、コマースアクティビティなど、データの様々なディメンションを操作するユーザーの一意のカウントを生成します。
2. **増分処理**：増分更新を実行すると、すべてのデータを最初から再計算することなく、ディメンションや時間をまたいでデータポイントを折りたたんだり結合したりできます。
3. **多次元分析**：ハイパーキューブにより、多次元フィルタリングとデータの並べ替えが可能になり、ディメンションの組み合わせを表すサマリー行を作成できます。 その後、これらの要約を使用して、最小限の計算オーバーヘッドでインサイトを生成できます。

## ハイパーキューブのユースケース {#use-cases}

ハイパーキューブを使用すると、データを毎回完全に再計算することなく、様々なユーザーインタラクションに対して個別のカウントを効率的に生成できます。 次に、使用に関する実用的なシナリオを示します。

- 定義された期間に特定の製品を表示するユニーク訪問者を分析します。
- 特定の期間に複数の製品を操作するユーザーを特定して、クロスセル分析を強化します。
- 時間の経過と共に別の製品ではなく、ある製品に関わっているユーザーを区別して、環境設定パターンを明らかにします。
- オンラインとオフラインのインタラクションデータを組み合わせて、特定の期間におけるユーザーの行動の包括的なビューを取得します。
- イベント内の様々なアクティビティをまたいだユーザーの移動を追跡して、レイアウトとサービスを最適化します。

## ハイパーキューブを使用するメリット

このような状況では、特定のカテゴリの基本情報を事前に計算できます。 ただし、複数のディメンションと期間にわたってデータを分析する場合は、生データからすべてを再計算するか、クエリサービスのハイパーキューブを使用する必要があります。 ハイパーキューブは、データを効率的に整理することでプロセスを合理化します。これにより、再処理を行うことなく、柔軟なフィルタリングと多次元分析が可能になります。 高度な機能を使用して結果を迅速かつ正確に推定することで、処理効率の向上、スケーラビリティの向上、複雑な分析タスクへの適応性などの主要なメリットを提供します。

### クエリ処理のデータサイズ効率

クエリサービスでは、数百万または数十億のデータポイント（ユーザー ID など）を、スケッチと呼ばれるコンパクトな形式に圧縮できます。 このスケッチでは、クエリ処理のデータサイズが大幅に削減されているので、スケーラビリティが維持され、操作がより簡単かつ迅速になります。 元のデータのサイズがどんなに大きくなっても、スケッチのサイズは小さいままなので、ビッグデータの分析がより管理しやすく効率的になります。

次の図は、Commerce、Product Info および Web ディメンション ExperienceEvents がスケッチに処理され、その後、一意のカウントを近似するために使用される方法を示しています。

![ クエリサービスを使用したスケッチの作成を示すインフォグラフィック。 この図は、Commerce、商品情報および web ディメンションを持つ ExperienceEvents が、スケッチに処理され、その後、一意のカウントを近似するために使用される方法を示しています。](./images/hypercubes/hypercube-overview.png)

### スケッチを結合して、データ分析を迅速かつ容易にする

再計算を避けて処理速度を上げるには、異なるカテゴリやグループのスケッチを結合します。 また、クエリサービスは、データをハイパーキューブに整理して、各行がスケッチ列と共にそのパーティションの概要（ディメンションのコレクション）になることで、設計を簡略化します。 ハイパーキューブの各行にはディメンションの組み合わせが含まれていますが、生データはありません。 クエリを実行する際は、追加指標の作成に使用するディメンション列を指定し、それらの行のスケッチを結合します。

![ この図は、様々な ExperienceEvents のスケッチを結合して、様々なディメンションでほぼ一意のカウントを作成する方法を示しています。](./images/hypercubes/merge-sketches.png)

### 費用対効果 {#cost-effectiveness}

顧客データは大規模であることが多いですが、増分処理を使用することで、履歴データを再処理する必要がなくなります。 スケッチははるかに小さく、より迅速でリアルタイムな結果を可能にしながら、計算リソースとコストを節約します。 このデータ変換により、インタラクティブクエリがより現実的かつ効率的になります。

## 関数の概要

ここでは、スケッチとハイパーキューブを効率的に使用して、各関数がデータ処理をどのように最適化し、分析機能を強化するかについて説明します。 その目的、例の構文、パラメーターおよび期待される出力について詳しく説明します。

### HLL スケッチを使用してユニーク数の見積もりを作成

`hll_build_agg` は、HLL （HyperLogLog）スケッチを作成する集計関数です。 この関数は、グループ化されたデータセット内の列または式内の一意の値の数を推定するためのコンパクトな確率論的方法です。

#### 関数の定義

```sql
hll_build_agg(column [, lgConfigK])
```

**使用方法：**

次の例は、クエリ内で関数を構造化する方法を示しています。

```sql
SELECT
   [dim1, dim2 ... ,] hll_build_agg(coalesce(col1, col2, col3)) AS sketch_col
FROM fact_sketch_table
  [GROUP BY dimension1, dimension2 ...]
```

#### パラメーター

| パラメーター | 説明 |
|---------------------------|---------------------------------------|
| `column` | スケッチを作成する列名。 |
| `lgConfigK` | *Int* （任意） K の log-base-2。K は HLL スケッチのバケットまたはスロットの数です。 最小値：4. 最大値：12。 デフォルト値：12。 |

#### 出力

| 出力列 | 説明 |
|---------------------------|---------------------------------------|
| `sketch_res` | 弦付き HLL スケッチを含む文字列型の列。 |

#### SQL の例

次の例では、`customer_id` の列に集約スケッチを作成します。

```sql
SELECT
  country,
  hll_build_agg(customer_id, 10) AS sketch
FROM
  EXPLODE(
    ARRAY<STRUCT<country STRING, customer_id STRING, invoice_id STRING>>[
      ('UA', 'customer_id_1', 'invoice_id_11'),
      ('CZ', 'customer_id_2', 'invoice_id_22'),
      ('CZ', 'customer_id_2', 'invoice_id_23'),
      ('BR', 'customer_id_3', 'invoice_id_31'),
      ('UA', 'customer_id_2', 'invoice_id_24')
    ])
GROUP BY country;
```

**SQL 出力例：**

| 国 | スケッチ |
|---------|------------------------------------------------------------|
| UA | AgEHBAMAAgCR9mUEulKCCQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA== |
| CZ | AgEHBAMAAQC6UooJAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA== |
| BR | AgEHBAMAAQCcmH0HAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA== |

### HLL スケッチを使用した個別カウントの推定

`hll_estimate` は、データセットの各行内の個別のカウントの概算を提供するスカラー関数です。 集計関数とは異なり、`hll_estimate` は行単位で動作し、個々の行内のスケッチから個別のカウントを推定するために使用されます。

>[!NOTE]
>
>この関数は、集計関数として使用できません。 集計された数には、`sketch_count` を使用します。

#### 関数の定義

```sql
hll_estimate(sketch_col)
```

**使用方法：**

次の例は、クエリ内で関数を構造化する方法を示しています。

```sql
SELECT
   [col1, col2 ... ,] hll_estimate(sketch_column) AS estimate
FROM fact_sketch_table
```

#### パラメーター

| パラメーター | 説明 |
|---------------------------|---------------------------------------|
| `sketch_column` | 側桁の HLL スケッチを含む列。 各行のスケッチの個別のカウントを見積もります。 |

#### 出力

| 出力列 | 説明 |
|---------------------------|---------------------------------------|
| `estimate` | スケッチの概算を提供する double 型の列で、小数点以下 2 桁に丸められます。 |

#### SQL の例

次の例では、HLL スケッチの `hll_estimate` 関数を使用して、国別の顧客のユニーク数を推定しています。

```sql
SELECT
  country,
  hll_estimate(hll_build_agg(customer_id, 10)) AS distinct_customers_by_country
FROM
  (
    SELECT
      country,
      hll_build_agg(customer_id, 10) AS sketch
    FROM 
      EXPLODE(
        ARRAY<STRUCT<country STRING, customer_id STRING, invoice_id STRING>>[
          ('UA', 'customer_id_1', 'invoice_id_11'),
          ('CZ', 'customer_id_2', 'invoice_id_22'),
          ('CZ', 'customer_id_2', 'invoice_id_23'),
          ('BR', 'customer_id_3', 'invoice_id_31'),
          ('UA', 'customer_id_2', 'invoice_id_24')
        ])
    GROUP BY country
  );
```

**SQL 出力例：**

| 国 | distinct_customers_by_country |
|---------|-------------------------------|
| UA | 2.00 |
| CZ | 1.00 |
| BR | 1.00 |

### 複数の HLL スケッチを `hll_merge_agg` と結合

`hll_merge_agg` は、グループ内の複数の HLL スケッチを結合し、出力として新しいスケッチを生成する集計関数です。 複数のパーティションやディメンションにまたがるスケッチの組み合わせが可能になり、データ分析の柔軟性が向上します。

#### 関数の定義

```sql
hll_merge_agg(sketch_col [, allowDifferentLgConfigK])
```

**使用方法：**

次の例は、クエリ内で関数を構造化する方法を示しています。

```sql
SELECT
   [dim1, dim2 ... ,] hll_merge_agg(sketch_column.sketch) AS estimate
FROM fact_sketch_table
  [GROUP BY dimension1, dimension2 ...]
```

#### パラメーター

| パラメーター | 説明 |
|---------------------------|---------------------------------------|
| `sketch_column` | 側桁の HLL スケッチを含む列。 |
| `allowDifferentLgConfigK` | *ブール値* （オプション） true に設定した場合、異なる `lgConfigK` 値を持つスケッチを結合できます。 デフォルト値は false です。 値が false で、スケッチの `lgConfigK` 値が異なる場合は、例外が発生します。 |

>[!NOTE]
>
>`allowDifferentLgConfigK` が false に設定されている場合、異なる `lgConfigK` 値を持つスケッチを結合すると、`UnsupportedOperationException` が発生します。

#### 出力

| 出力列 | 説明 |
|----------------|-------------------------------------------------|
| `sketch_res` | つなぎ結合された HLL スケッチを含む HLL スケッチ タイプの柱。 |

#### SQL の例

次の例では、`customer_id` 列の複数の HLL スケッチを結合します。

```sql
SELECT
   hll_merge_agg(hll_sketch) AS uniq_customers_with_invoice
FROM
  (
    SELECT
      country,
      hll_build_agg(customer_id) AS hll_sketch
    FROM
      EXPLODE(
        ARRAY<STRUCT<country STRING, customer_id STRING, invoice_id STRING>>[
          ('UA', 'customer_id_1', 'invoice_id_11'),
          ('BR', 'customer_id_3', 'invoice_id_31'),
          ('CZ', 'customer_id_2', 'invoice_id_22'),
          ('CZ', 'customer_id_2', 'invoice_id_23'),
          ('BR', 'customer_id_3', 'invoice_id_31'),
          ('UA', 'customer_id_2', 'invoice_id_24')
        ])
    GROUP BY country
    UNION
    SELECT
      country,
      hll_build_agg(customer_id) AS hll_sketch
    FROM
      EXPLODE(
        ARRAY<STRUCT<country STRING, customer_id STRING, invoice_id STRING>>[
          ('UA', 'customer_id_1', 'invoice_id_21'),
          ('MX', 'customer_id_3', 'invoice_id_31'),
          ('MX', 'customer_id_2', 'invoice_id_21')
        ])
    GROUP BY country
  )
GROUP BY customer_id;
```

**SQL 出力例：**

| 国 | hll_merge_agg （sketch, true） |
|---------|--------------------------------------------|
| UA | AgEHDAMAAwiR9mUEulKKCQAAAAAAAAAAAAAAAAAAAA== |
| CZ | AgEHDAMAAQi6UooJAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA== |
| BR | AgEHDAMAAQicmH0HAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA== |
| MX | AgEHFQMAAgiGL/kNdAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA== |

### `hll_merge_count_agg` を使用した基数の推定

`hll_merge_count_agg` は、列内の 1 つ以上のスケッチからカーディナリティ（一意の要素の数）を推定する集計関数です。 グループ化内で発生したすべてのスケッチの単一の見積もりを返します。 この関数は、スケッチの集計に使用され、行ごとの変換としては使用できません。 行ごとの推定には、`sketch_estimate` を使用します。

#### 関数の定義

```sql
hll_merge_count_agg(sketch_col [, allowDifferentLgConfigK])
```

**使用方法：**

次の例は、クエリ内で関数を構造化する方法を示しています。

```sql
SELECT
   [dim1, dim2 ... ,] hll_merge_count_agg(sketch_column) AS estimate
FROM fact_sketch_table
  [GROUP BY dimension1, dimension2 ...]
```

#### パラメーター

| パラメーター | 説明 |
|-------------------------|----------------------------------------------|
| `sketch_column` | 側桁の HLL スケッチを含む柱。 |
| `allowDifferentLgConfigK` | *ブール値* （オプション）デフォルト値は false です。 true に設定すると、異なる `lgConfigK` 値を持つスケッチを結合できます。 それ以外の場合は、`UnsupportedOperationException` がスローされます。 |

#### 出力

| 出力列 | 説明 |
|---------------|----------------------------------------------------------|
| `estimate` | スケッチの概算を提供する Double タイプの列。 |

#### SQL の例

次の例では、`hll_merge_count_agg` 関数を使用して、請求書のある一意の顧客の数を推定しています。

```sql
SELECT
   hll_merge_count_agg(hll_sketch) AS uniq_customers_with_invoice
FROM
  (
    SELECT
      country,
      hll_build_agg(customer_id) AS hll_sketch
    FROM
      EXPLODE(
        ARRAY<STRUCT<country STRING, customer_id STRING, invoice_id STRING>>[
          ('UA', 'customer_id_1', 'invoice_id_11'),
          ('BR', 'customer_id_3', 'invoice_id_31'),
          ('CZ', 'customer_id_2', 'invoice_id_22'),
          ('CZ', 'customer_id_2', 'invoice_id_23'),
          ('BR', 'customer_id_3', 'invoice_id_31'),
          ('UA', 'customer_id_2', 'invoice_id_24')
        ])
    GROUP BY country
    UNION
    SELECT
      country,
      hll_build_agg(customer_id) AS hll_sketch
    FROM
      EXPLODE(
        ARRAY<STRUCT<country STRING, customer_id STRING, invoice_id STRING>>[
          ('UA', 'customer_id_1', 'invoice_id_21'),
          ('MX', 'customer_id_3', 'invoice_id_31'),
          ('MX', 'customer_id_2', 'invoice_id_21')
        ])
    GROUP BY country
  )
GROUP BY customer_id;
```

**SQL 出力例：**

| 国 | hll_merge_count_agg （sketch, true） |
|---------|----------------------------------|
| UA | 2.0 |
| CZ | 1.0 |
| BR | 1.0 |
| MX | 2.0 |

## 制限事項

現在、スケッチは作成後に更新できません。 今後の更新では、スケッチを更新する機能が導入される予定です。 この機能を使用すると、失敗した実行や遅延した到着データをより効果的に処理できます。

## 次の手順

このドキュメントでは、ハイパーキューブおよび関連するスケッチ機能を使用して、履歴データを再処理しなくても、複雑な多次元分析で効率的なデータ処理を実行する方法を確認しました。 このアプローチは、時間の節約、コストの削減、リアルタイムのインタラクティブクエリに必要な柔軟性を提供し、Adobe Experience Platformでのビッグデータ分析に役立ちます。

次に、[ 増分読み込み ](./key-concepts/incremental-load.md) および [ データ重複排除 ](./key-concepts/deduplication.md) などの他の主要概念を調べて、これらの関数を特定のデータニーズに効果的に使用する方法の理解を深めます。


