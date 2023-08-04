---
title: データセット統計の計算
description: このドキュメントでは、SQL コマンドを使用して Azure Data Lake Storage（ADLS）データセットに関する列レベルの統計を計算する方法を説明します。
source-git-commit: b94536be6e92354e237b99d36af13adf5a49afa7
workflow-type: tm+mt
source-wordcount: '1085'
ht-degree: 37%

---

# データセット統計の計算

次の項目に関する列レベルの統計を計算できるようになりました： [!DNL Azure Data Lake Storage] (ADLS) データセットと `COMPUTE STATISTICS` SQL コマンド。 データセット統計を計算する SQL コマンドは、`ANALYZE TABLE` コマンドの拡張機能です。`ANALYZE TABLE` コマンドについて詳しくは、[SQL リファレンスドキュメント](../sql/syntax.md#analyze-table)を参照してください。

>[!NOTE]
>
>計算済みの統計は、セッションレベルの永続性を持つ一時テーブルに格納されます。 計算結果は、そのセッション中にいつでもアクセスできます。 異なる PSQL セッション間でアクセスすることはできません。

を使用して計算された統計を表示するには、以下を実行します。 `ANALYZE TABLE COMPUTE STATISTICS` コマンドを使用すると、エイリアス名または統計 ID に対して SELECT クエリを使用できます。 統計分析の範囲は、データセット全体、データセットのサブセット、すべての列、または列のサブセットに制限することもできます。

>[!IMPORTANT]
>
>The `COMPUTE STATISTICS`, `FILTERCONTEXT`、および `FOR COLUMNS` 高速ストアテーブルでは、コマンドはサポートされません。 `ANALYZE TABLE` コマンドのこれらの拡張機能は、現在 ADLS テーブルでのみサポートされています。詳細については、SQL 構文ガイドの [ANALYZE TABLE](../sql/syntax.md#analyze-table) の節を参照してください。

このガイドは、ADLS データセットの列の統計を計算できるようにクエリを構造化するのに役立ちます。これらのコマンドを使用すると、SQL クエリを使用して PSQL クライアントを通じてセッションで生成された統計を確認できます。

## 統計を計算 {#compute-statistics}

追加の構成が `ANALYZE TABLE` 次の操作を行うコマンド **データセットのサブセットと特定の列の統計を計算する**. データセットの統計を計算するには、 `ANALYZE TABLE <tableName> COMPUTE STATISTICS` 形式を使用します。

>[!IMPORTANT]
>
>デフォルトの動作では、**データセット全体**&#x200B;と&#x200B;**すべての列**&#x200B;の統計が計算されます。すべての列の統計を計算するには、クエリ形式 `ANALYZE TABLE COMPUTE STATISTICS` を使用します。あなたは **not** 使用を推奨 `COMPUTE STATISTICS` コマンドを使用せずに ADLS データセットに対してフィルターを設定することもできます。データセットのサイズが非常に大きい（ペタバイトのデータになる可能性がある）可能性があるからです。 代わりに、`FILTERCONTEXT` と指定された列のリストを使用して分析コマンドを実行することを常に検討する必要があります。詳しくは、[分析された列の制限](#limit-included-columns)および[フィルター条件の追加](#filter-condition)を参照してください。

以下に示す例では、`adc_geometric` データセットとデータセット内の&#x200B;**すべて**&#x200B;の列の統計を計算します。

```sql
ANALYZE TABLE adc_geometric COMPUTE STATISTICS;
```

>[!NOTE]
>
>The `COMPUTE STATISTICS` コマンドは、配列またはマップのデータ型をサポートしていません。 次の項目を設定すると、 `skip_stats_for_complex_datatypes` フラグを設定します。 デフォルトでは、フラグは true に設定されています。通知またはエラーを有効にするには、コマンド `SET skip_stats_for_complex_datatypes = false` を使用します。

## エイリアス名の作成 {#alias-name}

計算の結果は大量のデータになる場合があるので、計算したデータをコンソール出力に直接返すのは不合理です。 エイリアス名はオプションですが、統計を計算する際には、ベストプラクティスとしてエイリアス名を使用することをお勧めします。 SQL クエリで結果を記述的に参照するエイリアス名を文に指定します。 または、自動生成された `Statistics ID` が生成され、計算された情報の保存に使用されます。

次の例では、出力の計算済み統計を `alias_name` （後で参照）。 クエリで使用されるエイリアス名は、 `ANALYZE TABLE` コマンドが実行されました。

```sql
ANALYZE TABLE adc_geometric COMPUTE STATISTICS AS alias_name;
```

上記の例の出力は、 `SUCCESSFULLY COMPLETED, alias_name`. コンソール出力では、分析テーブル計算統計コマンドに対する応答で統計は表示されません。 詳細な結果を表示するには、エイリアス名または統計 ID に対して SELECT クエリを使用する必要があります。

## 計算済み統計の出力を表示 {#view-output-of-computed-statistics}

事前にエイリアス名を指定しない場合、クエリサービスは自動的に `Statistics ID` ～の形式に従う `<tableName_stats_{incremental_number}>`. エイリアス名を指定した場合は、 `Statistics ID` 列。

の出力例 `COMPUTE STATISTICS` クエリは次のようになります。

```console
| Statistics ID         | 
| --------------------- |
| adc_geometric_stats_1 |
(1 row)
```

次の操作が可能です。 **計算された統計を直接クエリ** を参照して `Statistics ID`. 以下の文の例を使用すると、 `Statistics ID` またはエイリアス名。

```sql
SELECT * FROM adc_geometric_stats_1; 
```

計算済みの統計出力は、次の例のようになります。

```console
 columnName                                                 |      mean      |      max       |      min       | standardDeviation | approxDistinctCount | nullCount | dataType  
------------------------------------------------------------+----------------+----------------+----------------+-------------------+---------------------+-----------+-----------
 marketing.trackingcode                                     |            0.0 |            0.0 |            0.0 |               0.0 |              1213.0 |         0 | String
 _experience.analytics.customdimensions.evars.evar13        |            0.0 |            0.0 |            0.0 |               0.0 |              8765.0 |        20 | String
 _experience.analytics.customdimensions.evars.evar74        |            0.0 |            0.0 |            0.0 |               0.0 |                11.0 |         0 | String
 web.webpagedetails.name                                    |            0.0 |            0.0 |            0.0 |               0.0 |                 1.0 |         0 | String
 _experience.analytics.event1to100.event8.value             |            5.0 |         9077.0 |          123.0 |              10.0 |              1001.0 |        80 | Double
 search.ispaid                                              |            0.0 |            0.0 |            0.0 |               0.0 |                 1.0 |         0 | Boolean
 commerce.productlistviews.value                            |            0.0 |            0.0 |            0.0 |               0.0 |                 0.0 |        10 | Double
 device.typeid                                              |            0.0 |            0.0 |            0.0 |               0.0 |                 0.0 |        10 | String
 commerce.purchases.value                                   |          765.0 |        98760.0 |         -980.0 |              32.0 |                99.0 |        90 | Double
 _experience.analytics.customdimensions.props.prop45        |            0.0 |            0.0 |            0.0 |               0.0 |                 1.0 |         0 | String
 environment.browserdetails.javaenabled                     |            0.0 |            0.0 |            0.0 |               0.0 |                 1.0 |         0 | Boolean
 timestamp                                                  |            0.0 |            0.0 |            0.0 |               0.0 |                98.0 |         3 | Timestamp
(12 rows)
```

## 統計分析のメタデータを表示 {#show-statistics}

以下を使用すると、 `SHOW STATISTICS` コマンドを使用して、セッションで生成されたすべての一時的な統計のメタデータを表示します。 このコマンドを使用すると、統計分析の範囲を絞り込むことができます。

の出力例 `SHOW STATISTICS` を以下に示します。

```console
      statsId         |   tableName   | columnSet |         filterContext       |      timestamp
----------------------+---------------+-----------+-----------------------------+--------------------
adc_geometric_stats_1 | adc_geometric |   (age)   |                             | 25/06/2023 09:22:26
demo_table_stats_1    |  demo_table   |    (*)    |       ((age > 25))          | 25/06/2023 12:50:26
age_stats             | castedtitanic |   (age)   | ((age > 25) AND (age < 40)) | 25/06/2023 09:22:26
```

メタデータ列名の説明を以下に示します。

| 列名 | 説明 |
|---|---|
| `statsId` | この ID は、 `COMPUTE STATISTICS` コマンドを使用します。 |
| `tableName` | 分析に使用する元のテーブル。 |
| `columnSet` | 分析用に特に選択された列のリスト。 値が空の場合は、すべての列が分析されたことを示します。 詳しくは、 [列の制限](#limit-included-columns) を参照してください。 |
| `filterContext` | 分析に適用されるフィルターのリスト。 |
| `timestamp` | 特定の期間に焦点を当てるために、データ分析に適用された時系列フィルター。 詳しくは、 [timestamp filter condition セクション [timestamp filter condition せいセクション ]](#filter-condition) を参照してください。 |

統計 ID またはエイリアス名を使用して、計算済みの統計を SELECT 文でそのセッション内でいつでも検索できます。 統計 ID と生成された統計は、この特定のセッションに対してのみ有効であり、異なる PSQL セッション間でアクセスすることはできません。計算された統計は、現在、永続的ではありません。 方法に関する節を参照してください。 [計算済み統計の出力を表示](#view-output-of-computed-statistics) を参照してください。

## 含める列の制限 {#limit-included-columns}

分析に焦点を当てるには、名前で参照することで、特定のデータセット列の統計を計算します。 特定の列をターゲットにするには、`FOR COLUMNS (<col1>, <col2>)` 構文を使用します。以下の例では、データセット `commerce` の列 `id`、`timestamp` および `tableName` の統計を計算します。

```sql
ANALYZE TABLE tableName COMPUTE STATISTICS FOR columns (commerce, id, timestamp);
```

任意のルートレベルまたはネストされた列の統計を計算できます。次の例は、これらの参照を示しています。

```sql
ANALYZE TABLE adcgeometric COMPUTE STATISTICS FOR columns (commerce, commerce.purchases.value, commerce.productListAdds.value);
```

## タイムスタンプフィルター条件の追加 {#filter-condition}

年表に基づく列の分析に焦点を当てるには、タイムスタンプフィルター条件を追加します。 この条件を使用して、履歴データを除外したり、特定の期間にデータ分析を絞り込んだりできます。 The `FILTERCONTEXT` コマンドは、指定したフィルター条件に基づいて、データセットのサブセットに関する統計を計算します。

以下の例では、データセット `tableName` のすべての列に関する統計が計算されます。列のタイムスタンプの値は、指定された範囲 `2023-04-01 00:00:00` から `2023-04-05 00:00:00` までです。

```sql
ANALYZE TABLE tableName FILTERCONTEXT (timestamp >= to_timestamp('2023-04-01 00:00:00') and timestamp <= to_timestamp('2023-04-05 00:00:00')) COMPUTE STATISTICS FOR ALL COLUMNS;
```

列の制限とフィルターを組み合わせて、データセット列に対して非常に具体的な計算クエリを作成できます。例えば、次のクエリは、データセット `commerce` の列 `id`、`timestamp` および `tableName` に関する統計を計算します。列のタイムスタンプの値は、指定された範囲 `2023-04-01 00:00:00` から `2023-04-05 00:00:00` までです。

```sql
ANALYZE TABLE tableName FILTERCONTEXT (timestamp >= to_timestamp('2023-04-01 00:00:00') and timestamp <= to_timestamp('2023-04-05 00:00:00')) COMPUTE STATISTICS FOR columns (commerce, id, timestamp);
```

## 次の手順 {#next-steps}

このドキュメントを参照することで、SQL クエリを使用して ADLS データセットから列レベルの統計を生成する方法をより深く理解できるようになります。Adobe Experience Platform クエリサービスの機能について詳しくは、[SQL 構文ガイド](../sql/syntax.md)を参照することをお勧めします。
