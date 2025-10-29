---
title: 統計と機械学習を使用したボットフィルタリング
description: Data Distillerの統計情報と機械学習を使用してボットアクティビティを特定およびフィルタリングし、正確な分析とデータ整合性の向上を実現する方法について説明します。
exl-id: 30d98281-7d15-47a6-b365-3baa07356010
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '1623'
ht-degree: 0%

---

# 統計と機械学習を使用したボットフィルタリング

ボットフィルタリングの実用化は、様々な業界に広がっています。 e コマースでは、コンバージョン率指標の信頼性が向上し、ニュースサイトでは誤ったエンゲージメント指標の軽減が有益になり、広告ネットワークでは公平な請求が保証されます。 クリックストリームまたは web トラフィックデータで正確な分析を維持し、データの整合性を確保するには、ボットアクティビティに対処する必要があります。 Data Distillerを使用して効果的なボットフィルタリングを実装し、不要なトラフィックを排除することで、高品質の分析データを確保できます。

このドキュメントでは、SQL と機械学習の手法を使用して、ボットアクティビティを特定およびフィルタリングするための包括的なガイドを提供します。 基本的なフィルタリングから始まり、機械学習ベースの検出と評価に進む、補完的なアプローチの進歩を紹介します。 この堅牢なフレームワークを採用すると、ボットの検出を強化し、データの整合性を維持できます。

## ボットアクティビティについて {#understand-bot-activity}

ボットアクティビティは、特定の期間内のユーザー操作のスパイクを検出することで識別できます。 例えば、1 人のユーザーが短時間に行った過剰なクリックは、ボットの動作を示す可能性があります。 ボットフィルタリングで使用される 2 つの主要な属性は次のとおりです。

- **ECID （Experience Cloud訪問者 ID）:** 訪問者を識別する永続的な汎用 ID。
- **タイムスタンプ：** Web サイトでアクティビティが発生した日時。

以下の例は、SQL と機械学習の手法を使用して、ボットアクティビティを特定、調整、予測する方法を示しています。 これらの方法を使用して、データの整合性を改善し、実用的な分析を確保します。

## SQL ベースのボットフィルタリング {#sql-based-bot-filtering}

この SQL ベースのボットフィルタリングの例では、SQL クエリを使用してしきい値を定義し、事前定義されたルールに基づいてボットアクティビティを検出する方法を示します。 この基本的なアプローチは、異常なアクティビティを削除することで、web トラフィックの異常を特定するのに役立ちます。 しきい値と間隔を定義して検出ルールをカスタマイズすることで、特定のトラフィックパターンに合わせてボットフィルタリングを効果的に調整できます。

### ボットアクティビティのしきい値の定義 {#define-thresholds}

まず、データセットを分析して、ユーザー行動を特定および分類します。 ECID、タイムスタンプ、`webPageDetails.name` （訪問した web ページの名前）などの属性に焦点を当てて、ユーザーのアクションをグループ化し、ボットアクティビティを示すパターンを検出します。

次の SQL クエリでは、疑わしいアクティビティを特定するために、60 クリックを超えるしきい値を 1 分間で適用する方法を示しています。

```sql
SELECT *
FROM analytics_events_table
WHERE enduserids._experience.ecid NOT IN (
    SELECT enduserids._experience.ecid
    FROM analytics_events_table
    GROUP BY Unix_timestamp(timestamp) / 60, enduserids._experience.ecid
    HAVING Count(*) > 60
);
```

### 複数の間隔に展開 {#expand-to-multiple-intervals}

次に、しきい値に対して異なる時間間隔を定義します。 これらの間隔には、次のものが含まれます。

- **1 分間隔：** 最大 60 回のクリック。
- **5 分間隔：** 最大 300 クリック。
- **30 分間隔：** 最大 1,800 クリック。

次の SQL クエリでは、複数の間隔にわたるしきい値を処理する `analytics_events_clicks_count_criteria` という名前のビューを作成します。 このステートメントは、1 分、5 分、30 分間隔のクリック数を構造化されたデータセットに統合し、事前に定義されたしきい値に基づいて潜在的なボットアクティビティをフラグ付けします。

```sql
CREATE VIEW analytics_events_clicks_count_criteria as  
SELECT struct (
        cast(count_1_min AS int) one_minute,
        cast(count_5_mins AS int) five_minute,
        cast(count_30_mins AS int) thirty_minute
       ) count_per_id,
       id,
      struct (
        struct (name) webpagedetails
      ) web,
      CASE
        WHEN count.one_minute > 50 THEN 1
        ELSE 0
      END AS isBot
FROM (
  SELECT table_count_1_min.mcid AS id,
         count_1_min,
         count_5_mins,
         count_30_mins,
         table_count_1_min.name AS name
  FROM (
      (SELECT mcid, Max(count_1_min) AS count_1_min, name
       FROM (SELECT enduserids._experience.mcid.id AS mcid,
                    Count(*) AS count_1_min,
                    web.webPageDetails.name AS name
             FROM delta_table
             WHERE TIMESTAMP BETWEEN TO_TIMESTAMP('2019-09-01 00:00:00')
                               AND TO_TIMESTAMP('2019-09-01 23:00:00')
             GROUP BY UNIX_TIMESTAMP(timestamp) / 60,
                      enduserids._experience.mcid.id,
                      web.webPageDetails.name)
       GROUP BY mcid, name) AS table_count_1_min
       LEFT JOIN
       (SELECT mcid, Max(count_5_mins) AS count_5_mins, name
        FROM (SELECT enduserids._experience.mcid.id AS mcid,
                     Count(*) AS count_5_mins,
                     web.webPageDetails.name AS name
              FROM delta_table
              WHERE TIMESTAMP BETWEEN TO_TIMESTAMP('2019-09-01 00:00:00')
                                AND TO_TIMESTAMP('2019-09-01 23:00:00')
              GROUP BY UNIX_TIMESTAMP(timestamp) / 300,
                       enduserids._experience.mcid.id,
                       web.webPageDetails.name)
        GROUP BY mcid, name) AS table_count_5_mins
       ON table_count_1_min.mcid = table_count_5_mins.mcid
       LEFT JOIN
       (SELECT mcid, Max(count_30_mins) AS count_30_mins, name
        FROM (SELECT enduserids._experience.mcid.id AS mcid,
                     Count(*) AS count_30_mins,
                     web.webPageDetails.name AS name
              FROM delta_table
              WHERE TIMESTAMP BETWEEN TO_TIMESTAMP('2019-09-01 00:00:00')
                                AND TO_TIMESTAMP('2019-09-01 23:00:00')
              GROUP BY UNIX_TIMESTAMP(timestamp) / 1800,
                       enduserids._experience.mcid.id,
                       web.webPageDetails.name)
        GROUP BY mcid, name) AS table_count_30_mins
       ON table_count_1_min.mcid = table_count_30_mins.mcid
  )
)
```

ステートメントは、`table_count_1_min` 値と web ページを使用して、`table_count_5_mins`、`table_count_30_mins`、`mcid` からデータを結合します。 次に、複数の時間間隔にわたって各ユーザーのクリック数を統合し、ユーザーアクティビティの完全な表示を提供します。 最後に、フラグ設定ロジックによって、1 分間に 50 クリックを超えたユーザーが識別され、ボットとしてマークされます（`isBot = 1`）。

### 出力データセット構造

出力データセットは、フラットフィールドとネストされたフィールドの両方で構造化されています。 この構造により、異常なトラフィックを検出する際の柔軟性が確保され、SQL と機械学習を使用した高度なフィルタリング戦略がサポートされます。 ネストされたフィールドには、アクティビティのしきい値と Web ページに関する詳細をカプセル化する `count` および `web` が含まれています。 これらの指標をキャプチャすることで、トレーニングや予測のタスクで特徴を簡単に抽出できます。

次のスキーマ図は、結果のデータセットの構造の概要と、ネストされたフィールドとフラットなフィールドを使用して、処理とボット検出を効率的に行う方法を示しています。

```console
root
 |-- count: struct (nullable = false)
 |    |-- one_minute: integer (nullable = true)
 |    |-- five_minute: integer (nullable = true)
 |    |-- thirty_minute: integer (nullable = true)
 |-- id: string (nullable = true)
 |-- web: struct (nullable = false)
 |    |-- webpagedetails: struct (nullable = false)
 |    |    |-- name: string (nullable = true)
 |-- isBot: integer (nullable = false)
```

### トレーニングに使用する出力データセット

この式の結果は、次の表のようになります。 テーブルの `isBot` 列は、ボットアクティビティとボット以外のアクティビティを区別するラベルとして機能します。

```console
| `id`         | `count_per_id`                                      |`isBot`| `web`                                                                                                                    |
|--------------|-----------------------------------------------------|-------|------------------------------------------------------------------------------------------------------------------------|
| 2.5532E+18   | {"one_minute":99,"five_minute":1,"thirty_minute":1} | 1     | {"webpagedetails":{"name":"KR+CC8TQzPyK4ord6w1PfJay1+h6snSF++xFERc4ogrEX4clJROgzkGgnSTSGWWZfNS/Ouz2K0VtkHG77vwoTg=="}} |
| 2.5532E+18   | {"one_minute":99,"five_minute":1,"thirty_minute":1} | 1     | {"webpagedetails":{"name":"KR+CC8TQzPyK4ord6w1PfJay1+h6snSF++xFERc4ogrEX4clJROgzkGgnSTSGWWZfNS/Ouz2K0VtkHG77vwoTg=="}} |
| 2.5532E+18   | {"one_minute":99,"five_minute":1,"thirty_minute":1} | 1     | {"webpagedetails":{"name":"KR+CC8TQzPyK4ord6w1PfJay1+h6snSF++xFERc4ogrEX4clJROgzkGgnSTSGWWZfNS/Ouz2K0VtkHG77vwoTg=="}} |
| 2.5532E+18   | {"one_minute":99,"five_minute":1,"thirty_minute":99}| 1     | {"webpagedetails":{"name":"KR+CC8TQzPyK4ord6w1PfJay1+h6snSF++xFERc4ogrEX4clJROgzkGgnSTSGWWZfNS/Ouz2K0VtkHG77vwoTg=="}} |
| 2.5532E+18   | {"one_minute":99,"five_minute":1,"thirty_minute":1} | 1     | {"webpagedetails":{"name":"KR+CC8TQzPyK4ord6w1PfJay1+h6snSF++xFERc4ogrEX4clJROgzkGgnSTSGWWZfNS/Ouz2K0VtkHG77vwoTg=="}} |
| 2.5532E+18   | {"one_minute":99,"five_minute":1,"thirty_minute":1} | 1     | {"webpagedetails":{"name":"KR+CC8TQzPyK4ord6w1PfJay1+h6snSF++xFERc4ogrEX4clJROgzkGgnSTSGWWZfNS/Ouz2K0VtkHG77vwoTg=="}} |
| 2.5532E+18   | {"one_minute":99,"five_minute":1,"thirty_minute":1} | 1     | {"webpagedetails":{"name":"KR+CC8TQzPyK4ord6w1PfJay1+h6snSF++xFERc4ogrEX4clJROgzkGgnSTSGWWZfNS/Ouz2K0VtkHG77vwoTg=="}} |
| 2.5532E+18   | {"one_minute":99,"five_minute":1,"thirty_minute":1} | 1     | {"webpagedetails":{"name":"KR+CC8TQzPyK4ord6w1PfJay1+h6snSF++xFERc4ogrEX4clJROgzkGgnSTSGWWZfNS/Ouz2K0VtkHG77vwoTg=="}} |
| 2.5532E+18   | {"one_minute":99,"five_minute":1,"thirty_minute":1} | 1     | {"webpagedetails":{"name":"KR+CC8TQzPyK4ord6w1PfJay1+h6snSF++xFERc4ogrEX4clJROgzkGgnSTSGWWZfNS/Ouz2K0VtkHG77vwoTg=="}} |
| 2.5532E+18   | {"one_minute":99,"five_minute":1,"thirty_minute":1} | 1     | {"webpagedetails":{"name":"KR+CC8TQzPyK4ord6w1PfJay1+h6snSF++xFERc4ogrEX4clJROgzkGgnSTSGWWZfNS/Ouz2K0VtkHG77vwoTg=="}} |
| 1E+18        | {"one_minute":1,"five_minute":1,"thirty_minute":1}  | 0     | {"webpagedetails":{"name":"KR+CC8TQzPyMOE/bk7EGgN3lSvP8OsxeI2aLaVrbaeLn8XK3y3zok2ryVyZoiBu3"}}                       |
| 1.00007E+18  | {"one_minute":1,"five_minute":1,"thirty_minute":1}  | 0     | {"webpagedetails":{"name":"8DN0dM4rlvJxt4oByYLKZ/wuHyq/8CvsWNyXvGYnImytXn/bjUizfRSl86vmju7MFMXxnhTBoCWLHtyVSWro9LYg0MhN8jGbswLRLXoOIyh2wduVbc9XeN8yyQElkJm3AW3zcqC7iXNVv2eBS8vwGg=="}} |
| 1.00008E+18  | {"one_minute":1,"five_minute":1,"thirty_minute":1}  | 0     | {"webpagedetails":{"name":"KR+CC8TQzPyMOE/bk7EGgN3lSvP8OsxeI2aLaVrbaeLn8XK3y3zok2ryVyZoiBu3"}}                       |
```

## ボットフィルタリング用の高度な統計関数 {#statistical-functions-for-bot-filtering}

この 2 番目の例は、しきい値を調整し、フィルタリングの精度を向上させる機械学習技術を組み込むことで、基本的な SQL フィルタリングに基づいています。 このアプローチでは、回帰分析やクラスターアルゴリズムなどの高度な統計関数を使用して、予測機能を導入します。この機能を使用すると、複雑なデータセットを高精度で処理するためのモデルを開発できます。

### トレーニングデータセットの作成 {#build-a-training-dataset}

まず、機械学習モデルが使用できる、フラットなネストされた構造を持つデータセットを準備します（上記を参照）。 これを行う方法について詳しくは、[ ネストされたデータ構造の操作ドキュメント ](../../key-concepts/nested-data-structures.md) を参照してください。 タイムスタンプ、ユーザー ID および web ページ名でデータをグループ化し、ボットアクティビティのパターンを識別します。

### モデルの作成に TRANSFORM 句とOPTIONS句を使用 {#transform-and-preprocess}

データセットを変換して機械学習モデルを効果的に設定するには、次の手順に従います。 このステップでは、Null 値の処理方法、特性の準備方法、最適なパフォーマンスを得るためのモデルのパラメーターの定義方法について詳しく説明します。

>[!TIP]
>
>変換の使用とデータの前処理について詳しくは、[ 機能変換テクニックのドキュメント ](../feature-transformation.md) を参照してください。

1. 数値、文字列、ブールの列に null 値を入力するには、それぞれ `numeric_imputer`、`string_imputer`、`boolean_imputer` 関数を使用します。 この手順により、機械学習アルゴリズムでエラーなくデータを処理できるようになります。
2. フィーチャ変換を適用して、モデリング用のデータを準備します。 `binarized`、`quantile_discretizer`、`string_indexer` を適用して、列を分類または標準化します。 次に、インプター（`numeric_imputer` と `string_imputer`）の出力を `string_indexer` や `quantile_discretizer` などの後続のトランスフォーマーにフィードして、意味のある機能を作成します。
3. `vector_assembler` 関数を使用して、変換後の列を 1 つのフィーチャ列に結合します。 次に、`min_max_scaler` を使用してフィーチャーをスケールし、値を正規化してモデルのパフォーマンスを向上させます。 注意：SQL の例では、TRANSFORM 句内で言及されている最後の変換が、機械学習モデルで使用される機能列になります。
4. OPTIONS句でモデルタイプとその他のハイパーパラメーターを指定します。 例えば、これは分類の問題なので、`decision_tree_classifier` が選択されました。 `max_depth` などの他のパラメーターは、パフォーマンスを向上させるためにモデルを調整するために調整されました（`MAX_DEPTH=4`）。
5. フィーチャを結合し、出力データにラベルを付けます。 SELECT 句を使用して、トレーニング用のデータセットを指定します。 この句には、機能列（`count_per_id`、`web`、`id`）とラベル列（`isBot`）の両方を含める必要があります。これは、アクションがボットである可能性があるかどうかを示します。

ステートメントは、次の例のようになります。

```sql
CREATE MODEL bot_filtering_model
TRANSFORM (
  numeric_imputer(count_per_id.one_minute, 'mean') imputed_one_minute,
  numeric_imputer(count_per_id.five_minute, 'mode') imputed_five_minute,
  numeric_imputer(count_per_id.thirty_minute) imputed_thirty_minute,
  string_imputer(id, 'unknown') imputed_id,
  string_indexer(imputed_id) si_id,
  quantile_discretizer(imputed_five_minute) buckets_five,
  string_indexer(web.webpagedetails.NAME) si_name,
  quantile_discretizer(imputed_thirty_minute) buckets_thirty,
  vector_assembler(array(si_id, imputed_one_minute, buckets_five, si_name, buckets_thirty)) features,
  min_max_scaler(features) scaled_features
)
OPTIONS (model_type='decision_tree_classifier', max_depth=4, label='isBot')
AS
SELECT count_per_id, isBot, web, id FROM analytics_events_clicks_count_criteria;
```

**結果**

以下に示す結果では、一意の ID、名前、バージョンを使用してモデル `bot_filtering_model` が正常に作成されています。 この出力は、モデルのトラッキングと管理のためのリファレンスとして機能します。 これらの参照を使用して、予測または評価に必要な構成およびバージョンの正確な特定

```console
           Created Model ID           |       Created Model       | Version
|--------------------------------------+---------------------------+---------
 2fb4b49e-d35c-44cf-af19-cc210e7dc72c | bot_filtering_model       |       1
```

### トレーニング済みモデルの評価 {#evaluate-trained-model}

モデルを作成したら、`MODEL_EVALUATE` コマンドを使用してモデルのパフォーマンスを評価します。 この手順により、ボットアクティビティを検出するための精度とパフォーマンスの要件をモデルが満たしていることを確認します。

SQL コマンドで次の引数を使用してモデルを評価します。

1. 評価するモデルを示すモデル名（`bot_filtering_model`）を指定します。 この名前は、前に `CREATE MODEL` コマンドで作成した名前と一致する必要があります。
2. 2 番目の引数にモデルバージョン（`1`）を指定して、評価するモデルのバージョンを指定します。 複数のバージョンが存在する場合は、正しいバージョンが使用されます。
3. 評価データセットを含めて、評価用のデータセットを定義します。 データセットに、トレーニング時に使用したのと同じ機能列（`count_per_id`、`web`、`id`）とラベル列（`isBot`）が含まれていることを確認します。

>[!NOTE]
>
>モデルトレーニング中に適用された変換は、評価中に自動的に適用されます。

```sql
SELECT *
FROM   model_evaluate(bot_filtering_model, 1,
                      SELECT count_per_id, isBot, web, id
                      FROM   analytics_events_clicks_count_criteria);
```

**結果**

応答には、精度、精度、リコール、AUC-ROC などの指標が含まれます。 その結果、モデルが良好に動作したかどうかを確認します。

>[!NOTE]
>
>0 ～ 1 の範囲の値は比率または確率を表し、1.0 は完全なパフォーマンスを示します。

```console
auc_roc | accuracy | precision | recall
|---------+----------+-----------+--------
     1.0 |      1.0 |       1.0 |    1.0
```

| **指標** | **説明** |
|--------------|-----------------------------------------------------------------------------------------------------|
| `auc_roc` | この指標は、モデルがボットと非ボットを効果的に分類できるかを示します。 分類モデルの評価に広く使用されています。 |
| `accuracy` | モデルによって行われた正しい予測の割合。 |
| `precision` | 予測されたすべてのボットのうち、真のボット予測の割合。 |
| `recall` | 実際のすべてのボットのうち、検出された真のボットの割合。 |

>[!TIP]
>
>実稼動用サンドボックスで使用する場合は、効果的に一般化されるように、テストデータセットでモデルを評価することを検討してください。

### ボットアクティビティを予測 {#predict-bot-activity}

トレーニング済みモデルと共に `MODEL_PREDICT` コマンドを使用して、どのユーザー（`id`）がボットかを特定します。 ボットアクティビティを識別するための予測を生成するには、次の手順に従います。

1. 最初の引数でモデル名（`bot_filtering_model`）を使用して、予測に使用するモデルを指定します。
2. 正しいバージョンのモデルが使用されていることを確認するには、2 番目の引数にモデルのバージョン （`1`）を指定します。
3. 予測の正しいデータを指定するには、SELECT 文を使用してフィーチャ列（`count_per_id`、`web`、`id`）を指定します。 モデルがこのフィールドの予測を生成するので、ラベル列（`isBot`）を含めないでください。

```sql
SELECT *
FROM model_predict(bot_filtering_model, 1,
    SELECT count_per_id, web, id FROM analytics_events_clicks_count_criteria
);
```

**結果**

応答には、各ユーザー（`id`）の予測が、ユーザーのアクティビティの詳細およびモデルの分類結果と共に含まれます。 この出力により、ユーザー行動とボットアクティビティのモデルの分類を詳細に調査できます。

<!-- Q) Anil, why is there no ID in the first two rows? Can we get that info? Or should it be the same as the other IDs? -->

```console
         id          | count.one_minute | count.five_minute | count.thirty_minute |                                                                  web.webpagedetails.name                                                                  | prediction
|---------------------+------------------+-------------------+---------------------+-------+----------------------------------------------------------------------------------------------------------------------------------------------------+------------
                     |              110 |                   |                     |   4UNDilcY5VAgu2pRmX4/gtVnj+YxDDQaJd1G8p8WX46//wYcrHy+APUN0I556E80j1gIzFmilA6DV4s0Zcs4ruiP36gLgC7bj4TH0q6LU0E=                                             |        1.0  
                     |              105 |                   |                     |   lrSaZk04Yq+5P9+6l4BohwXik0s0/XeW9X28ZgWt1yj1QQztiAt9Qgt2WYrWcAeoGZChAJw/l8e4ojZDT5WHCjteSt35S01Vv1JzDGPAg+IyhIzMTsVyLpW8WWpXjJoMCt6Tv7fFdF73EIH+IrK5fA== |        1.0
 2553215812530219515 |               99 |                 1 |                   1 |   KR+CC8TQzPyK4ord6w1PfJay1+h6snSF++xFERc4ogrEX4clJROgzkGgnSTSGWWZfNS/Ouz2K0VtkHG77vwoTg==                                                                 |        1.0
 2553215812530219515 |               99 |                 1 |                   1 |   KR+CC8TQzPyK4ord6w1PfJay1+h6snSF++xFERc4ogrEX4clJROgzkGgnSTSGWWZfNS/Ouz2K0VtkHG77vwoTg==                                                                 |        1.0
```

各指標の説明を次の表に示します。

| 列名 | 説明 |
|---------------------------|----------------------------------------------------------------------------------------------------------|
| `id` | 各ユーザーの一意の ID。 |
| `count.one_minute` | 集計されたクリックは、1 分間隔でカウントされます。 |
| `count.five_minute` | 集計されたクリックは 5 分間隔でカウントされます。 |
| `count.thirty_minute` | 集計されたクリックは、30 分間隔でカウントされます。 |
| `web.webpagedetails.name` | 訪問した web ページの名前。 これにより、ユーザーアクティビティのコンテキストが提供されます。 |
| `prediction` | モデルの予測。 `1.0` の結果は、ユーザーがアクティビティパターンに基づいてボットとしてフラグ付けされていることを示しています。 |

## モデルの管理 {#manage-models}

機械学習モデルを管理するには、以下の節に示す SQL キーワードを使用します。

### 使用可能なモデルのリスト {#list-available-models}

`SHOW MODELS;` コマンドを使用して、モデルを効率的に管理および確認します。 このコマンドは、現在のワークスペースで作成されたすべての機械学習モデルを一覧表示します。 出力には、使用可能なモデルの概要が示され、その名前、バージョン、その他のメタデータが含まれます。

```sql
SHOW MODELS;
```

### モデルを削除 {#delete-models}

リソースを解放し、関連するモデルのみを維持するには、`DROP MODEL` コマンドを使用して、古いモデルや不要なモデルを削除します。 このコマンドは、キーワードの後に指定された機械学習モデルをすべて削除します。 次の例では、`bot_filtering_model` がシステムから削除されます。

```sql
DROP MODEL bot_filtering_model;
```

## 次の手順

このドキュメントを読むことで、SQL を使用してボットアクティビティを特定およびフィルタリングする方法と、Data Distillerを使用して機械学習の手法を学びました。 次に、これらの概念をデータセットに適用し、継続的な改善のためにモデルの再トレーニングを自動化します。
