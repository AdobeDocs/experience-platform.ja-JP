---
title: データセット統計の計算
description: このドキュメントでは、SQL コマンドを使用して Azure Data Lake Storage（ADLS）データセットに関する列レベルの統計を計算する方法を説明します。
source-git-commit: c42a7cd46f79bb144176450eafb00c2f81409380
workflow-type: ht
source-wordcount: '785'
ht-degree: 100%

---

# データセット統計の計算

`COMPUTE STATISTICS` および `SHOW STATISTICS` SQL コマンドを使用して、[!DNL Azure Data Lake Storage]（ADLS）データセットに関する列レベルの統計を計算できるようになりました。データセット統計を計算する SQL コマンドは、`ANALYZE TABLE` コマンドの拡張機能です。`ANALYZE TABLE` コマンドについて詳しくは、[SQL リファレンスドキュメント](../sql/syntax.md#analyze-table)を参照してください。

>[!NOTE]
>
>現在、生成された統計はそのセッションに対してのみ有効であり、セッション間では永続的ではありません。異なる PSQL セッション間でアクセスすることはできません。

`SHOW STATISTICS FOR <alias_name>` コマンドを使用すると、`ANALYZE TABLE COMPUTE STATISTICS` コマンドで計算された統計を確認できます。これらのコマンドを組み合わせることにより、データセット全体、データセットのサブセット、すべての列、列のサブセットのいずれかに関する列の統計を計算できるようになりました。

>[!IMPORTANT]
>
>`COMPUTE STATISTICS`、`FILTERCONTEXT`、`FOR COLUMNS` および `SHOW STATISTICS` コマンドは、データウェアハウステーブルではサポートされていません。`ANALYZE TABLE` コマンドのこれらの拡張機能は、現在 ADLS テーブルでのみサポートされています。詳細については、SQL 構文ガイドの [ANALYZE TABLE](../sql/syntax.md#analyze-table) の節を参照してください。

このガイドは、ADLS データセットの列の統計を計算できるようにクエリを構造化するのに役立ちます。これらのコマンドを使用すると、SQL クエリを使用して PSQL クライアントを通じてセッションで生成された統計を確認できます。

## 統計を計算 {#compute-statistics}

`ANALYZE TABLE` コマンドに追加の構造が追加され、**データセットのサブセットおよび特定の列の統計を計算**&#x200B;できるようになりました。これを行うには、`ANALYZE TABLE <tableName> COMPUTE STATISTICS` 形式を使用する必要があります。

>[!IMPORTANT]
>
>デフォルトの動作では、**データセット全体**&#x200B;と&#x200B;**すべての列**&#x200B;の統計が計算されます。すべての列の統計を計算するには、クエリ形式 `ANALYZE TABLE COMPUTE STATISTICS` を使用します。データセットのサイズが非常に大きくなる可能性がある（ペタバイト単位のデータになる可能性がある）ので、ADLS データセットで使用することはお勧めし&#x200B;**ません**。代わりに、`FILTERCONTEXT` と指定された列のリストを使用して分析コマンドを実行することを常に検討する必要があります。詳しくは、[分析された列の制限](#limit-included-columns)および[フィルター条件の追加](#filter-condition)を参照してください。

以下に示す例では、`adc_geometric` データセットとデータセット内の&#x200B;**すべて**&#x200B;の列の統計を計算します。

```sql
ANALYZE TABLE adc_geometric COMPUTE STATISTICS;
```

>[!NOTE]
>
>`COMPUTE STATISTICS` は、配列またはマップのデータタイプをサポートしません。入力データフレームに配列およびマップデータタイプを含む列がある場合に、通知を受信するかエラーが発生するように、`skip_stats_for_complex_datatypes` フラグを設定できます。デフォルトでは、フラグは true に設定されています。通知またはエラーを有効にするには、コマンド `SET skip_stats_for_complex_datatypes = false` を使用します。

<!-- Commented out until the <alias_name> feature is released.
This second example, is a more real-world example as it uses an alias name. See the [alias name section](#alias-name) for more details on this feature.

```sql
ANALYZE TABLE adc_geometric COMPUTE STATISTICS as <alias_name>;
``` -->

コンソール出力には、分析テーブル計算統計コマンドに応じた統計は表示されません。代わりに、コンソールには、結果を参照するためのユニバーサル固有識別子（UUID）を持つ `Statistics ID` の 1 行の列が表示されます。`COMPUTE STATISTICS` クエリが正常に完了すると、結果が次のように表示されます。

```console
| Statistics ID    | 
| ---------------- |
| QqMtDfHQOdYJpZlb |
(1 row)
```

出力を確認するには、`SHOW STATISTICS` コマンドを使用する必要があります。[統計を表示する方法](#show-statistics)については、ドキュメントの後半で説明します。

## 含める列の制限 {#limit-included-columns}

特定のデータセット列の統計を計算するには、名前で参照します。特定の列をターゲットにするには、`FOR COLUMNS (<col1>, <col2>)` 構文を使用します。以下の例では、データセット `tableName` の列 `commerce`、`id` および `timestamp` の統計を計算します。

```sql
ANALYZE TABLE tableName COMPUTE STATISTICS FOR columns (commerce, id, timestamp);
```

任意のルートレベルまたはネストされた列の統計を計算できます。次の例は、これらの参照を示しています。

```sql
ANALYZE TABLE adcgeometric COMPUTE STATISTICS FOR columns (commerce, commerce.purchases.value, commerce.productListAdds.value);
```

## タイムスタンプフィルター条件の追加 {#filter-condition}

タイムスタンプフィルター条件を追加して、列の分析に焦点を当てることができます。これを使用して、履歴データを除外したり、特定の期間に焦点を当ててデータ分析を行うことができます。`FILTERCONTEXT` コマンドは、指定したフィルター条件に基づいてデータセットのサブセットに関する統計を計算します。

以下の例では、データセット `tableName` のすべての列に関する統計が計算されます。列のタイムスタンプの値は、指定された範囲 `2023-04-01 00:00:00` から `2023-04-05 00:00:00` までです。

```sql
ANALYZE TABLE tableName FILTERCONTEXT (timestamp >= to_timestamp('2023-04-01 00:00:00') and timestamp <= to_timestamp('2023-04-05 00:00:00')) COMPUTE STATISTICS FOR ALL COLUMNS;
```

列の制限とフィルターを組み合わせて、データセット列に対して非常に具体的な計算クエリを作成できます。例えば、次のクエリは、データセット `tableName` の列 `commerce`、`id` および `timestamp` に関する統計を計算します。列のタイムスタンプの値は、指定された範囲 `2023-04-01 00:00:00` から `2023-04-05 00:00:00` までです。

```sql
ANALYZE TABLE tableName FILTERCONTEXT (timestamp >= to_timestamp('2023-04-01 00:00:00') and timestamp <= to_timestamp('2023-04-05 00:00:00')) COMPUTE STATISTICS FOR columns (commerce, id, timestamp);
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

## 統計の表示 {#show-statistics}

<!-- Commented out until the <alias_name> feature is released.
The alias name used in the query is available as soon as the `ANALYZE TABLE` command has been run.  -->

フィルター条件と列リストを使用しても、計算のターゲットとなるデータは大量になる可能性があります。クエリサービスは、この計算された情報を保存するために統計 ID のユニバーサル固有識別子（UUID）を生成します。その後、この統計 ID を使用して、セッション内でいつでも `SHOW STATISTICS` コマンドで計算された統計を検索できます。

統計 ID と生成された統計は、この特定のセッションに対してのみ有効であり、異なる PSQL セッション間でアクセスすることはできません。計算された統計は、現在、永続的ではありません。 統計を表示するには、次に示すコマンドを使用します。

```sql
SHOW STATISTICS FOR <STATISTICS_ID>;
```

出力は、次の例のようになります。

```console
                         columnName                         |      mean      |      max       |      min       | standardDeviation | approxDistinctCount | nullCount | dataType  
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

## 次の手順 {#next-steps}

このドキュメントを参照することで、SQL クエリを使用して ADLS データセットから列レベルの統計を生成する方法をより深く理解できるようになります。Adobe Experience Platform クエリサービスの機能について詳しくは、[SQL 構文ガイド](../sql/syntax.md)を参照することをお勧めします。
