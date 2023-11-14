---
title: データセットのサンプル
description: クエリサービスのサンプルデータセットを使用すると、クエリの精度を犠牲にする代わりに、処理時間を大幅に短縮し、ビッグデータに関する探索的なクエリを実行できます。このガイドでは、近似クエリ処理のサンプルを管理する方法について説明します
exl-id: 9e676d7c-c24f-4234-878f-3e57bf57af44
source-git-commit: ef71371b04746bbf12ac58e91c9ecb5806f7e771
workflow-type: tm+mt
source-wordcount: '639'
ht-degree: 100%

---

# データセットのサンプル

Adobe Experience Platform クエリサービスは、近似クエリ処理機能の一部としてサンプルデータセットを提供します。サンプルデータセットは、元のレコードのパーセンテージのみを使用して、既存の [!DNL Azure Data Lake Storage]（ADLS）データセットからの均一なランダムサンプルを使用して作成されます。このパーセンテージは、サンプリングレートと呼ばれます。精度と処理時間のバランスを制御するためにサンプリングレートを調整すると、クエリの精度を犠牲にする代わりに、処理時間を大幅に短縮し、ビッグデータに関する探索的なクエリを実行できます。

多くのユーザーは、データセットに対する集計操作に対して正確な回答を必要としないので、大規模なデータセットに関する探索的なクエリでは、近似クエリを発行して近似回答を返す方が効率的です。サンプルデータセットには元のデータセットのデータの割合しか含まれていないので、クエリの精度と引き換えに応答時間を向上させることができます。読み取り時に、クエリサービスでスキャンする必要がある行が少なくなるので、データセット全体に対してクエリを実行する場合よりも速く結果が得られます。

クエリサービスでは、近似クエリ処理のためのサンプルの管理に役立つように、データセットサンプルに対して次の操作をサポートしています。

- [均一なランダムデータセットのサンプルの作成。](#create-a-sample)
- [フィルター条件の指定（オプション）](##optional-filter-criteria)
- [ADLS テーブルに対するサンプルのリストの表示。](#view-list-of-samples)
- [サンプルデータセットに対する直接クエリの実行。](#query-sample-datasets)
- [サンプルの削除。](#delete-a-sample)
- 元の ADLS テーブルが削除された場合の関連するサンプルの削除。

## はじめに {#get-started}

このドキュメントで説明している近似クエリ処理機能の作成および削除を使用するには、セッションフラグを `true` に設定する必要があります。クエリエディターまたは PSQL クライアントのコマンドラインで `SET aqp=true;` コマンドを入力します。

>[!NOTE]
>
>Platform にログインするたびにセッションフラグを有効にする必要があります。

![「SET aqp=true;」コマンドがハイライト表示されたクエリエディター。](../images/essential-concepts/set-session-flag.png)

## 均一なランダムデータセットのサンプルの作成 {#create-a-sample}

データセット名を指定して `ANALYZE TABLE <table_name> TABLESAMPLE SAMPLERATE x` コマンドを使用し、そのデータセットから均一なランダムサンプルを作成します。

サンプルレートは、元のデータセットから取得したレコードの割合です。`TABLESAMPLE SAMPLERATE` キーワードを使用して、サンプルレートを制御できます。この例では、値 5.0 は 50％のサンプルレートと等しくなります。値が 2.5 の場合は 25％と等しくなります。

>[!IMPORTANT]
>
>システムでは、各データセットに対して最大で 5 つのサンプルを使用できます。6 番目のサンプルデータセットを作成しようとすると、サンプルの制限に達したことを示すエラーメッセージが画面に表示されます。

```sql
ANALYZE TABLE example_dataset_name TABLESAMPLE SAMPLERATE 5.0;
```

## フィルター条件の指定（オプション） {#optional-filter-criteria}

均一なランダムサンプルのフィルター条件を指定できます。 これにより、分析済みテーブルのフィルター済みサブセットに基づいてサンプルを作成できます。

サンプルを作成する際、まずオプションのフィルターが適用され、次に、データセットのフィルター済みビューからサンプルが作成されます。 フィルターが適用されたデータセットサンプルは、次のクエリ形式に従います。

```sql
ANALYZE TABLE <tableToAnalyze> TABLESAMPLE FILTERCONTEXT (<filter_condition>) SAMPLERATE X.Y;
ANALYZE TABLE <tableToAnalyze> TABLESAMPLE FILTERCONTEXT (<filter_condition_1> AND/OR <filter_condition_2>) SAMPLERATE X.Y;
ANALYZE TABLE <tableToAnalyze> TABLESAMPLE FILTERCONTEXT (<filter_condition_1> AND (<filter_condition_2> OR <filter_condition_3>)) SAMPLERATE X.Y;
```

このタイプのフィルター済みサンプルデータセットの実例を次に示します。

```sql
ANALYZE TABLE large_table TABLESAMPLE FILTERCONTEXT (month(to_timestamp(timestamp)) in ('8', '9')) SAMPLERATE 10;
ANALYZE TABLE large_table TABLESAMPLE FILTERCONTEXT (month(to_timestamp(timestamp)) in ('8', '9') AND product.name = "product1") SAMPLERATE 10;
ANALYZE TABLE large_table TABLESAMPLE FILTERCONTEXT (month(to_timestamp(timestamp)) in ('8', '9') AND (product.name = "product1" OR product.name = "product2")) SAMPLERATE 10;
```

この例では、テーブル名は `large_table`、元のテーブルに対するフィルター条件は `month(to_timestamp(timestamp)) in ('8', '9')`、サンプリングレート（フィルター済みデータの X％）は、この場合は `10` です。

## サンプルのリストの表示 {#view-list-of-samples}

`sample_meta()` 関数を使用して、ADLS テーブルに関連付けられたサンプルのリストを表示します。

```sql
SELECT sample_meta('example_dataset_name')
```

データセットサンプルのリストは、以下の例の形式で表示されます。

```shell
                  sample_table_name                  |    sample_dataset_id     |    parent_dataset_id     | sample_type | sampling_rate | sample_num_rows |       created      
-----------------------------------------------------+--------------------------+--------------------------+-------------+---------------+-----------------+---------------------
 x5e5cd8ea0a83c418a8ef0928_uniform_4_0_percent_ughk7 | 62ff19853d338f1c07b18965 | 5e5cd8ea0a83c418a8ef0928 | uniform     |           4.0 |             391 | 19/08/2022 05:03:01
(1 row)
```

## サンプルデータセットのクエリ {#query-sample-datasets}

`{EXAMPLE_DATASET_NAME}` を使用して、サンプルテーブルに対して直接クエリを実行します。 または、`WITHAPPROXIMATE` キーワードをクエリの最後に追加すると、クエリサービスは、最も新しく作成されたサンプルを自動的に使用します。

```sql
SELECT * FROM example_dataset_name WITHAPPROXIMATE;
```

## データセットサンプルの削除 {#delete-a-sample}

削除操作を使用すると、データセットサンプルの最大数が 5 個に達した場合に、新しいサンプルを作成できます。

```sql
DROP TABLE SAMPLE x5e5cd8ea0a83c418a8ef0928_uniform_2_0_percent_bnhmc;
```

>[!NOTE]
>
>元の ADLS データセットから派生した複数のサンプルデータセットがある場合、元のデータセットをドロップすると、関連するすべてのサンプルも削除されます。
