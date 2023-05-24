---
title: データセット統計の計算
description: このドキュメントでは、SQL コマンドを使用して Azure Data Lake Storage(ADLS) データセットの列レベルの統計を計算する方法について説明します。
source-git-commit: b063bcf7b3d2079715ac18fde55f47cea078b609
workflow-type: tm+mt
source-wordcount: '788'
ht-degree: 0%

---

# データセット統計の計算

次の項目に関する列レベルの統計を計算できるようになりました： [!DNL Azure Data Lake Storage] (ADLS) データセットと `COMPUTE STATISTICS` および `SHOW STATISTICS` SQL コマンド データセット統計を計算する SQL コマンドは、 `ANALYZE TABLE` コマンドを使用します。 詳細は、 `ANALYZE TABLE` コマンドは、 [SQL リファレンスドキュメント](../sql/syntax.md#analyze-table).

>[!NOTE]
>
>現在、生成された統計は、そのセッションに対してのみ有効で、セッション間で保持されません。 異なる PSQL セッションをまたいでアクセスすることはできません。

を使用 `SHOW STATISTICS FOR <alias_name>` コマンドを使用すると、 `ANALYZE TABLE COMPUTE STATISTICS` コマンドを使用します。 これらのコマンドを組み合わせることで、データセット全体、データセットのサブセット、すべての列、または列のサブセットに関する列の統計を計算できるようになりました。

>[!IMPORTANT]
>
>この `COMPUTE STATISTICS`, `FILTERCONTEXT`, `FOR COLUMNS`、および `SHOW STATISTICS` data warehouse テーブルでは、コマンドはサポートされていません。 これらの `ANALYZE TABLE` コマンドは、現在、ADLS テーブルでのみサポートされています。 詳しくは、 [テーブルの分析セクション](../sql/syntax.md#analyze-table) 」を参照してください。

このガイドでは、ADLS データセットの列の統計を計算できるように、クエリを構造化する方法について説明します。 これらのコマンドを使用すると、SQL クエリを使用して PSQL クライアントを通じて、セッションで生成された統計を確認できます。

## 統計を計算 {#compute-statistics}

追加の構成が `ANALYZE TABLE` 次の操作を行うためのコマンド **データセットのサブセットと特定の列の統計を計算する**. これをおこなうには、 `ANALYZE TABLE <tableName> COMPUTE STATISTICS` 形式

>[!IMPORTANT]
>
>デフォルトの動作では、 **データセット全体** および **すべての列**. すべての列の統計を計算するには、クエリフォーマットを使用します `ANALYZE TABLE COMPUTE STATISTICS`. あなたは **not** データセットのサイズは非常に大きい（ペタバイトのデータになる可能性がある）可能性があるので、ADLS データセットでこれを使用することをお勧めします。 代わりに、次を使用して analyze コマンドを実行することを常に検討してください。 `FILTERCONTEXT` および指定した列のリスト。 詳しくは、 [分析された列の制限](#limit-included-columns) および [フィルター条件の追加](#filter-condition) を参照してください。

以下の例では、 `adc_geometric` データセットと **すべて** 列を含める必要があります。

```sql
ANALYZE TABLE adc_geometric COMPUTE STATISTICS;
```

>[!NOTE]
>
>`COMPUTE STATISTICS` は、配列またはマップのデータ型をサポートしていません。 次の項目を設定できます。 `skip_stats_for_complex_datatypes` フラグを指定します。 デフォルトでは、このフラグは true に設定されています。 通知またはエラーを有効にするには、次のコマンドを使用します。 `SET skip_stats_for_complex_datatypes = false`.

<!-- Commented out until the <alias_name> feature is released.
This second example, is a more real-world example as it uses an alias name. See the [alias name section](#alias-name) for more details on this feature.

```sql
ANALYZE TABLE adc_geometric COMPUTE STATISTICS as <alias_name>;
``` -->

コンソール出力では、分析テーブル計算統計コマンドに応じて統計が表示されません。 代わりに、コンソールには次の 1 行の列が表示されます： `Statistics ID` 結果を参照する UUID（汎用固有識別子） の正常な完了時 `COMPUTE STATISTICS` クエリの場合、結果は次のように表示されます。

```console
| Statistics ID    | 
| ---------------- |
| QqMtDfHQOdYJpZlb |
(1 row)
```

出力を表示するには、 `SHOW STATISTICS` コマンドを使用します。 手順： [統計の表示方法](#show-statistics) は、ドキュメントの後半で提供されます。

## 含める列を制限 {#limit-included-columns}

特定のデータセット列の統計を計算するには、名前で参照します。 以下を使用： `FOR COLUMNS (<col1>, <col2>)` ターゲット固有の列の構文。 次の例では、列の統計を計算します  `commerce`, `id`、および `timestamp` （データセット用） `tableName`.

```sql
ANALYZE TABLE tableName COMPUTE STATISTICS FOR columns (commerce, id, timestamp);
```

任意のルートレベルまたはネストされた列の統計を計算できます。 次の例は、これらの参照を示しています。

```sql
ANALYZE TABLE adcgeometric COMPUTE STATISTICS FOR columns (commerce, commerce.purchases.value, commerce.productListAdds.value);
```

## タイムスタンプフィルター条件の追加 {#filter-condition}

タイムスタンプフィルター条件を追加して、列の分析に焦点を当てることができます。 これを使用して、履歴データを除外したり、特定の期間にデータ分析を絞り込んだりできます。 この `FILTERCONTEXT` コマンドは、指定したフィルター条件に基づいて、データセットのサブセットに関する統計を計算します。

次の例では、データセットのすべての列に関する統計が計算されます `tableName`( 列のタイムスタンプには、指定した `2023-04-01 00:00:00` および `2023-04-05 00:00:00`.

```sql
ANALYZE TABLE tableName FILTERCONTEXT (timestamp >= to_timestamp('2023-04-01 00:00:00') and timestamp <= to_timestamp('2023-04-05 00:00:00')) COMPUTE STATISTICS FOR ALL COLUMNS;
```

列の制限とフィルターを組み合わせて、データセット列に対して非常に具体的な計算クエリを作成できます。 例えば、次のクエリは、列の統計を計算します `commerce`, `id`、および `timestamp` （データセット用） `tableName`( 列のタイムスタンプには、指定した `2023-04-01 00:00:00` および `2023-04-05 00:00:00`.

```sql
ANALYZE TABLE tableName FILTERCONTEXT (timestamp >= to_timestamp('2023-04-01 00:00:00') and timestamp <= to_timestamp('2023-04-05 00:00:00')) COMPUTE STATISTICS FOR (columns commerce, id, timestamp);
```

<!-- ## Create an alias name {#alias-name}

Since the filter condition and the column list can target a large amount of data, it is unrealistic to remember the exact values. Instead, you can provide an `<alias_name>` to store this calculated information. If you do not provide an alias name for these calculations, Query Service generates a universally unique identifier for the alias ID. You can then use this alias ID to look up the computed statistics with the `SHOW STATISTICS` command. 

>[!NOTE]
>
>Although alias names are optional, you are recommended to use them as best practice.

The example below stores the output computed statistics in the `alias_name` for later reference.

```sql
ANALYZE TABLE adc_geometric COMPUTE STATISTICS FOR ALL COLUMNS as alias_name;
```

The output for the above example is `SUCCESSFULLY COMPLETED, alias_name`. The console output does not display the statistics in the response of the analyze table compute statistics command. To see the output, you must use the `SHOW STATISTICS` command discussed below. -->

## 統計を表示 {#show-statistics}

<!-- Commented out until the <alias_name> feature is released.
The alias name used in the query is available as soon as the `ANALYZE TABLE` command has been run.  -->

フィルター条件や列リストがあっても、計算では大量のデータをターゲットにすることができます。 クエリサービスは、この計算情報を保存するために、統計 ID の汎用的に一意の識別子を生成します。 この統計 ID を使用して、 `SHOW STATISTICS` コマンドを実行できます。

生成された統計 ID と統計は、この特定のセッションでのみ有効で、異なる PSQL セッションをまたいでアクセスすることはできません。 計算された統計は、現在、永続的ではありません。 統計を表示するには、次に示すコマンドを使用します。

```sql
SHOW STATISTICS FOR <STATISTICS_ID>;
```

出力は、次の例のようになります。

```console
                         columnName                         |      mean      |      max       |      min       | standardDeviation | approxDistinctCount | nullCount | dataType  
------------------------------------------------------------+----------------+----------------+----------------+-------------------+---------------------+-----------+-----------
 marketing.trackingcode                                     |            0.0 |            0.0 |            0.0 |               0.0 |              1213.0 |         0 | String
 _experience.analytics.session.timestamp                    |            450 |          -2313 |          21903 |               7.0 |                 0.0 |         0 | Long
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
(13 rows)
```

## 次の手順 {#next-steps}

このドキュメントでは、SQL クエリを使用して ADLS データセットから列レベルの統計を生成する方法について、より深く理解しています。 次を読むことをお勧めします。 [SQl 構文ガイド](../sql/syntax.md) をクリックして、Adobe Experience Platform Query Service のその他の機能を確認します。
