---
title: データセットのサンプル
description: クエリサービスのサンプルデータセットを使用すると、クエリの精度を犠牲にして、処理時間を大幅に短縮し、ビッグデータに関する探索的なクエリを実行できます。 このガイドでは、近似クエリ処理のためのサンプルの管理方法に関する情報を提供します
exl-id: 9e676d7c-c24f-4234-878f-3e57bf57af44
source-git-commit: 13779e619345c228ff2a1981efabf5b1917c4fdb
workflow-type: tm+mt
source-wordcount: '639'
ht-degree: 0%

---

# データセットのサンプル

Adobe Experience Platformクエリサービスは、クエリ処理機能の一部として、サンプルデータセットを提供します。 サンプルデータセットは、既存のサンプルから均一なランダムサンプルを使用して作成されます [!DNL Azure Data Lake Storage] (ADLS) データセット。元のレコードの割合のみを使用します。 この割合は、サンプリングレートと呼ばれます。 精度と処理時間のバランスを制御するためにサンプリングレートを調整すると、クエリの精度を犠牲にして、処理時間を大幅に短縮し、ビッグデータに関する探索的なクエリを実行できます。

データセットに対する集計操作に正確な回答が必要ないユーザーが多いので、近似回答を返すための近似クエリを発行する方が、大規模なデータセットに関する調査クエリでより効率的です。 サンプルデータセットには元のデータセットのデータの割合のみが含まれているので、クエリの精度を取り引いて応答時間を短縮できます。 読み取り時に、クエリサービスでスキャンする行数は、データセット全体をクエリする場合よりも少なく、より迅速に結果を生成する必要があります。

クエリサービスでは、概算クエリ処理のためのサンプルの管理に役立つように、データセットサンプルに対して次の操作をサポートしています。

- [均一なランダムデータセットのサンプルを作成します。](#create-a-sample)
- [必要に応じてフィルタ条件を指定します](##optional-filter-criteria)
- [ADLS テーブルのサンプルのリストを表示します。](#view-list-of-samples)
- [サンプルデータセットを直接クエリします。](#query-sample-datasets)
- [サンプルを削除します。](#delete-a-sample)
- 元の ADLS テーブルが削除されたときに、関連するサンプルを削除します。

## はじめに {#get-started}

このドキュメントで詳しく説明する概算クエリ処理機能の作成と削除をおこなうには、セッションフラグをに設定する必要があります。 `true`. クエリエディターまたは PSQL クライアントのコマンドラインから、 `SET aqp=true;` コマンドを使用します。

>[!NOTE]
>
>Platform にログインするたびに、セッションフラグを有効にする必要があります。

![「SET aqp=true;」コマンドがハイライト表示されたクエリエディタ。](../images/essential-concepts/set-session-flag.png)

## 均一なランダムデータセットのサンプルを作成する {#create-a-sample}

以下を使用： `ANALYZE TABLE <table_name> TABLESAMPLE SAMPLERATE x` コマンドにデータセット名を付けて、そのデータセットから均一なランダムサンプルを作成します。

サンプルレートは、元のデータセットから取得したレコードの割合です。 サンプルレートは、 `TABLESAMPLE SAMPLERATE` キーワード。 この例では、値 5.0 はサンプルレート 50%と等しくなります。 値が 2.5 の場合は 25%と等しくなります。

>[!IMPORTANT]
>
>システムでは、各データセットに対して最大で 5 つのサンプルを使用できます。 6 番目のサンプルデータセットを作成しようとすると、画面に、サンプルの制限に達したことを示すエラーメッセージが表示されます。

```sql
ANALYZE TABLE example_dataset_name TABLESAMPLE SAMPLERATE 5.0;
```

## 必要に応じてフィルタ条件を指定します {#optional-filter-criteria}

均一なランダムサンプルのフィルタ条件を指定できます。 これにより、分析済みテーブルのフィルター済みサブセットに基づいてサンプルを作成できます。

サンプルを作成する際、オプションのフィルターが最初に適用され、次に、サンプルがデータセットのフィルターされた表示から作成されます。 フィルターが適用されたデータセットの例を次に示します。

```sql
ANALYZE TABLE <tableToAnalyze> TABLESAMPLE FILTERCONTEXT (<filter_condition>) SAMPLERATE X.Y;
ANALYZE TABLE <tableToAnalyze> TABLESAMPLE FILTERCONTEXT (<filter_condition_1> AND/OR <filter_condition_2>) SAMPLERATE X.Y;
ANALYZE TABLE <tableToAnalyze> TABLESAMPLE FILTERCONTEXT (<filter_condition_1> AND (<filter_condition_2> OR <filter_condition_3>)) SAMPLERATE X.Y;
```

このタイプのフィルタリングされたサンプルデータセットの実例を次に示します。

```sql
Analyze TABLE large_table TABLESAMPLE FILTERCONTEXT (month(to_timestamp(timestamp)) in ('8', '9')) SAMPLERATE 10;
Analyze TABLE large_table TABLESAMPLE FILTERCONTEXT (month(to_timestamp(timestamp)) in ('8', '9') AND product.name = "product1") SAMPLERATE 10;
Analyze TABLE large_table TABLESAMPLE FILTERCONTEXT (month(to_timestamp(timestamp)) in ('8', '9') AND (product.name = "product1" OR product.name = "product2")) SAMPLERATE 10;
```

この例では、テーブル名は `large_table`に設定されている場合、元のテーブルのフィルター条件は `month(to_timestamp(timestamp)) in ('8', '9')`( この場合、サンプリングレートは（フィルターされたデータの X%）です。 `10`.

## サンプルのリストの表示 {#view-list-of-samples}

以下を使用： `sample_meta()` 関数を使用して、ADLS テーブルに関連付けられたサンプルのリストを表示します。

```sql
SELECT sample_meta('example_dataset_name')
```

データセットサンプルのリストは、次の例の形式で表示されます。

```shell
                  sample_table_name                  |    sample_dataset_id     |    parent_dataset_id     | sample_type | sampling_rate | sample_num_rows |       created      
-----------------------------------------------------+--------------------------+--------------------------+-------------+---------------+-----------------+---------------------
 x5e5cd8ea0a83c418a8ef0928_uniform_4_0_percent_ughk7 | 62ff19853d338f1c07b18965 | 5e5cd8ea0a83c418a8ef0928 | uniform     |           4.0 |             391 | 19/08/2022 05:03:01
(1 row)
```

## サンプルデータセットのクエリ {#query-sample-datasets}

以下を使用： `{EXAMPLE_DATASET_NAME}` を使用して、サンプルテーブルを直接クエリします。 または、 `WITHAPPROXIMATE` キーワードをクエリの最後に追加すると、クエリサービスは最も新しく作成されたサンプルを自動的に使用します。

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
>元の ADLS データセットから複数のサンプルデータセットを取得した場合、元のデータセットがドロップされると、関連するすべてのサンプルも削除されます。
