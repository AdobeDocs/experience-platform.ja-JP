---
title: データセット統計の計算
description: このドキュメントでは、SQL コマンドを使用して Azure Data Lake Storage（ADLS）データセットに関する列レベルの統計を計算する方法を説明します。
exl-id: 66f11cd4-b115-40b8-ba8a-c4bb3606bbbf
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '1085'
ht-degree: 100%

---

# データセット統計の計算

`COMPUTE STATISTICS` SQL コマンドを使用して、[!DNL Azure Data Lake Storage]（ADLS）データセットに関する列レベルの統計を計算できるようになりました。データセット統計を計算する SQL コマンドは、`ANALYZE TABLE` コマンドの拡張機能です。`ANALYZE TABLE` コマンドについて詳しくは、[SQL リファレンスドキュメント](../sql/syntax.md#analyze-table)を参照してください。

>[!NOTE]
>
>計算済みの統計は、セッションレベルの永続性を持つ一時テーブルに格納されます。計算結果には、そのセッション中にいつでもアクセスできます。異なる PSQL セッション間でアクセスすることはできません。

`ANALYZE TABLE COMPUTE STATISTICS` コマンドを使用して計算された統計を表示するには、エイリアス名または統計 ID に対して SELECT クエリを使用できます。統計分析の範囲を、データセット全体、データセットのサブセット、すべての列、または列のサブセットのいずれかに制限することもできます。

>[!IMPORTANT]
>
>`COMPUTE STATISTICS`、`FILTERCONTEXT`、`FOR COLUMNS` コマンドは、高速化ストアテーブルではサポートされていません。`ANALYZE TABLE` コマンドのこれらの拡張機能は、現在 ADLS テーブルでのみサポートされています。詳細については、SQL 構文ガイドの [ANALYZE TABLE](../sql/syntax.md#analyze-table) の節を参照してください。

このガイドは、ADLS データセットの列の統計を計算できるようにクエリを構造化するのに役立ちます。これらのコマンドを使用すると、SQL クエリを使用して PSQL クライアントを通じてセッションで生成された統計を確認できます。

## 統計を計算 {#compute-statistics}

`ANALYZE TABLE` コマンドに追加の構造が追加され、**データセットのサブセットおよび特定の列の統計を計算**&#x200B;できるようになりました。データセット統計を計算するには、`ANALYZE TABLE <tableName> COMPUTE STATISTICS` 形式を使用する必要があります。

>[!IMPORTANT]
>
>デフォルトの動作では、**データセット全体**&#x200B;と&#x200B;**すべての列**&#x200B;の統計が計算されます。すべての列の統計を計算するには、クエリ形式 `ANALYZE TABLE COMPUTE STATISTICS` を使用します。データセットのサイズは非常に大きくなる（ペタバイト単位のデータになる）可能性があるので、ADLS データセットに対してフィルターを使用せずに `COMPUTE STATISTICS` コマンドを使用することは&#x200B;**お勧めしません**。代わりに、`FILTERCONTEXT` と指定された列のリストを使用して分析コマンドを実行することを常に検討する必要があります。詳しくは、[分析された列の制限](#limit-included-columns)および[フィルター条件の追加](#filter-condition)を参照してください。

以下に示す例では、`adc_geometric` データセットとデータセット内の&#x200B;**すべて**&#x200B;の列の統計を計算します。

```sql
ANALYZE TABLE adc_geometric COMPUTE STATISTICS;
```

>[!NOTE]
>
>`COMPUTE STATISTICS` コマンドは、配列またはマップのデータタイプをサポートしていません。入力データフレームに配列およびマップデータタイプを含む列がある場合に、通知を受信するかエラーが発生するように、`skip_stats_for_complex_datatypes` フラグを設定できます。デフォルトでは、フラグは true に設定されています。通知またはエラーを有効にするには、コマンド `SET skip_stats_for_complex_datatypes = false` を使用します。

## エイリアスの作成 {#alias-name}

計算結果は膨大なデータになる場合があるので、計算したデータをコンソール出力で直接返すのは不合理です。エイリアス名はオプションですが、統計を計算する際には、ベストプラクティスとしてエイリアス名を使用することをお勧めします。SQL クエリで結果を記述的に参照するには、ステートメントにエイリアス名を指定します。または、自動生成された `Statistics ID` を使用して、計算済みの情報を格納します。

次の例では、後で参照できるように、出力された計算済みの統計情報を `alias_name` に格納します。クエリで使用されるエイリアス名は、`ANALYZE TABLE` コマンドが実行されるとすぐに参照できるようになります。

```sql
ANALYZE TABLE adc_geometric COMPUTE STATISTICS AS alias_name;
```

上記の例の出力は `SUCCESSFULLY COMPLETED, alias_name` です。コンソール出力には、「analyze table computestatistics」コマンドに応じた統計は表示されません。詳細な結果を表示するには、エイリアス名または統計 ID に対して SELECT クエリを使用する必要があります。

## 計算済み統計の出力を表示 {#view-output-of-computed-statistics}

事前にエイリアス名を指定しない場合、クエリサービスは `<tableName_stats_{incremental_number}>` の形式に従う `Statistics ID` の名前を自動的に生成します。エイリアス名を指定した場合は、`Statistics ID` 列に表示されます。

`COMPUTE STATISTICS` クエリの出力例は次のとおりです。

```console
| Statistics ID         | 
| --------------------- |
| adc_geometric_stats_1 |
(1 row)
```

`Statistics ID` を参照することで、**計算された統計を直接クエリ**&#x200B;できます。以下の例のステートメントを `Statistics ID` またはエイリアス名とともに使用すると、出力を完全に表示できます。

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

`SHOW STATISTICS` コマンドを使用すると、セッションで生成されたすべての一時的な統計のメタデータを表示できます。このコマンドを使用すると、統計分析の範囲を絞り込むことができます。

`SHOW STATISTICS` の出力例を次に示します。

```console
      statsId         |   tableName   | columnSet |         filterContext       |      timestamp
----------------------+---------------+-----------+-----------------------------+--------------------
adc_geometric_stats_1 | adc_geometric |   (age)   |                             | 25/06/2023 09:22:26
demo_table_stats_1    |  demo_table   |    (*)    |       ((age > 25))          | 25/06/2023 12:50:26
age_stats             | castedtitanic |   (age)   | ((age > 25) AND (age < 40)) | 25/06/2023 09:22:26
```

メタデータ列名の説明を次に示します。

| 列名 | 説明 |
|---|---|
| `statsId` | この ID は、`COMPUTE STATISTICS` コマンドによって生成された一時的な統計のテーブルを参照します。 |
| `tableName` | 分析に使用する元のテーブル。 |
| `columnSet` | 分析用に特別に選択された列のリスト。すべての列が分析済みの場合は、値が空になります。詳しくは、[列の制限](#limit-included-columns)に関する節を参照してください。 |
| `filterContext` | 分析に適用される任意のフィルターのリスト。 |
| `timestamp` | 特定の期間に焦点を当てるために、データ分析に適用される時系列フィルター。詳しくは、[タイムスタンプフィルター条件に関する節](#filter-condition)を参照してください。 |

統計 ID またはエイリアス名を使用して、セッション内でいつでも SELECT ステートメントで計算された統計を検索できます。統計 ID と生成された統計は、この特定のセッションに対してのみ有効であり、異なる PSQL セッション間でアクセスすることはできません。計算された統計は、現在、永続的ではありません。 詳しくは、[計算済みの統計の出力を表示する](#view-output-of-computed-statistics)方法に関する節を参照してください。

## 含める列の制限 {#limit-included-columns}

分析に焦点を当てるには、特定のデータセット列を名前で参照して、その列の統計を計算します。特定の列をターゲットにするには、`FOR COLUMNS (<col1>, <col2>)` 構文を使用します。以下の例では、データセット `commerce` の列 `id`、`timestamp` および `tableName` の統計を計算します。

```sql
ANALYZE TABLE tableName COMPUTE STATISTICS FOR columns (commerce, id, timestamp);
```

任意のルートレベルまたはネストされた列の統計を計算できます。次の例は、これらの参照を示しています。

```sql
ANALYZE TABLE adcgeometric COMPUTE STATISTICS FOR columns (commerce, commerce.purchases.value, commerce.productListAdds.value);
```

## タイムスタンプフィルター条件の追加 {#filter-condition}

時系列に基づいて列の分析に焦点を当てるには、タイムスタンプフィルター条件を追加します。この条件を使用して、履歴データを除外したり、特定の期間に焦点を当ててデータ分析を実行したりできます。`FILTERCONTEXT` コマンドを使用して、指定したフィルター条件に基づいてデータセットのサブセットに関する統計を計算します。

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
