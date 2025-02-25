---
title: クラスタリングアルゴリズム
description: 主要なパラメーター、説明、サンプルコードを使用して様々なクラスターアルゴリズムを設定および最適化し、高度な統計モデルを実装する方法を説明します。
role: Developer
exl-id: 273853c6-85d2-43e5-b51a-aa9d20b313ae
source-git-commit: 69c08f688d355e689e78426dd4b0ed1f4934965c
workflow-type: tm+mt
source-wordcount: '1019'
ht-degree: 4%

---

# クラスタリングアルゴリズム {#clustering-algorithms}

クラスターアルゴリズムは、類似性に基づいてデータポイントを個別のクラスターにグループ化し、教師なしの学習によってデータ内のパターンを明らかにすることを可能にします。 クラスターアルゴリズムを作成するには、`OPTIONS` 句の `type` パラメーターを使用して、モデルのトレーニングに使用するアルゴリズムを指定します。 次に、関連するパラメーターをキーと値のペアとして定義し、モデルを微調整します。

>[!NOTE]
>
>選択したアルゴリズムのパラメーター要件を理解していることを確認します。 特定のパラメータをカスタマイズしない場合は、デフォルト設定が適用されます。 各パラメーターの関数とデフォルト値については、関連するドキュメントを参照してください。

## [!DNL K-Means] {#kmeans}

`K-Means` は、データ・ポイントを定義済みの数のクラスタ（k）に分割するクラスタリング・アルゴリズムです。 これは、シンプルさと効率のために、クラスタリングに最も一般的に使用されるアルゴリズムの 1 つです。

**パラメーター**

`K-Means` を使用する場合、次のパラメーターを `OPTIONS` 句で設定できます。

| パラメーター | 説明 | デフォルト値 | 可能な値 |
|---------------------|---------------------------------------------------------------------------------------------------------------|-----------------|----------------------------------|
| `MAX_ITER` | アルゴリズムが実行する反復回数。 | `20` | （>= 0） |
| `TOL` | 収束許容値レベル。 | `0.0001` | （>= 0） |
| `NUM_CLUSTERS` | 作成するクラスターの数（`k`）。 | `2` | （>1） |
| `DISTANCE_TYPE` | 2 点間の距離の計算に使用するアルゴリズムです。 この値は、大文字と小文字を区別します。 | `euclidean` | `euclidean`、`cosine` |
| `KMEANS_INIT_METHOD` | クラスターセンターの初期化アルゴリズム。 | `k-means\|\|` | `random`, `k-means\|\|` （k-means++の並列バージョン） |
| `INIT_STEPS` | `k-means\|\|` 初期化モードのステップ数（`KMEANS_INIT_METHOD` が `k-means\|\|` の場合にのみ適用されます）。 | `2` | （>0） |
| `PREDICTION_COL` | 予測が保存される列の名前。 | `prediction` | 任意の文字列 |
| `SEED` | 再現性のランダムシード。 | `-1689246527` | 任意の 64 ビット番号 |
| `WEIGHT_COL` | インスタンスの重み付けに使用する列の名前。 設定しない場合、すべてのインスタンスが均等に重み付けされます。 | `not set` | なし |

{style="table-layout:auto"}

**例**

```sql
CREATE MODEL modelname 
OPTIONS(
  type = 'kmeans',
  MAX_ITERATIONS = 30,
  NUM_CLUSTERS = 4
) 
AS SELECT col1, col2, col3 FROM training-dataset;
```

## [!DNL Bisecting K-means] {#bisecting-kmeans}

[!DNL Bisecting K-means] は、分割型（または「トップダウン」）アプローチを使用する階層クラスタリングアルゴリズムです。 すべての観測は 1 つのクラスターで開始され、階層の構築時に分割が再帰的に実行されます。 通常の K-means よりも高速な [!DNL Bisecting K-means] が、通常は異なるクラスタ結果が生成される。

**パラメーター**

| パラメーター | 説明 | デフォルト値 | 可能な値 |
|-------------------------------|--------------------------------------------------------------------------------------------------------------------------------|----------------|------------------------------------------------|
| `MAX_ITER` | アルゴリズムが実行するイテレーションの最大数。 | 20 | （>= 0） |
| `WEIGHT_COL` | インスタンスの重み付けの列名。 設定されていない場合や空の場合は、すべてのインスタンスの重みが `1.0` として扱われます。 | 設定されていません | 任意の文字列 |
| `NUM_CLUSTERS` | リーフクラスタの望ましい数。 分割可能なクラスタが残っていない場合は、実際の数は小さくなる可能性があります。 | 4 | （> 1） |
| `SEED` | アルゴリズム内のランダムプロセスの制御に使用されるランダムシードです。 | 設定されていません | 任意の 64 ビット番号 |
| `DISTANCE_MEASURE` | ポイント間の類似度の計算に使用される距離メジャー。 | 「ユークリッド」 | `euclidean`、`cosine` |
| `MIN_DIVISIBLE_CLUSTER_SIZE` | クラスタを分割するのに必要な最小ポイント数（1.0 より大きい場合）、または最小ポイント数（1.0 より小さい場合）。 | 1.0 | （>= 0） |
| `PREDICTION_COL` | 予測出力の列名。 | 「予測」 | 任意の文字列 |

{style="table-layout:auto"}

**例**

```sql
Create MODEL modelname OPTIONS(
  type = 'bisecting_kmeans',
) AS
  select col1, col2, col3 from training-dataset
```

## [!DNL Gaussian Mixture Model] {#gaussian-mixture-model}

[!DNL Gaussian Mixture Model] は、k 個のガウス分布の 1 つからデータ ポイントが作成される複合分布を表します。この分布は、それぞれ独自の確率を持ちます。 複数のガウス分布の混合から生成されると想定されるデータセットのモデル化に使用されます。

**パラメーター**

| パラメーター | 説明 | デフォルト値 | 可能な値 |
|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------|------------------------------------------|
| `MAX_ITER` | アルゴリズムを実行する最大反復数。 | 100 | （>= 0） |
| `WEIGHT_COL` | 列名（例：重み）。 設定されていない場合や空の場合は、すべてのインスタンスの重みが `1.0` として扱われます。 | 設定されていません | 任意の有効な列名または空 |
| `NUM_CLUSTERS` | 混合モデル内の独立ガウス分布の数。 | 2 | （> 1） |
| `SEED` | アルゴリズム内のランダムプロセスを制御するために使用されるランダムシードです。 | 設定されていません | 任意の 64 ビット番号 |
| `AGGREGATION_DEPTH` | このパラメーターは、計算時に使用される集計ツリーの深度を制御します。 | 2 | （>= 1） |
| `PROBABILITY_COL` | 予測されるクラスの条件付き確率の列名。 これらは、正確な確率ではなく信頼性スコアとして扱う必要があります。 | 「確率」 | 任意の文字列 |
| `TOL` | 反復アルゴリズムの収束許容値。 | 0.01 | （>= 0） |
| `PREDICTION_COL` | 予測出力の列名。 | 「予測」 | 任意の文字列 |

{style="table-layout:auto"}

**例**

```sql
Create MODEL modelname OPTIONS(
  type = 'gaussian_mixture',
) AS
  select col1, col2, col3 from training-dataset
```

## [!DNL Latent Dirichlet Allocation] （LDA） {#latent-dirichlet-allocation}

[!DNL Latent Dirichlet Allocation] （LDA）は、ドキュメントのコレクションから基になるトピック構造をキャプチャする確率的モデルです。 これは、単語、トピック、ドキュメントレイヤーを含む 3 レベルの階層ベイジアンモデルです。 LDA は、これらのレイヤーと観察済みのドキュメントを使用して、潜在的なトピック構造を構築します。

**パラメーター**

| パラメーター | 説明 | デフォルト値 | 可能な値 |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------|------------------------------------------|
| `MAX_ITER` | アルゴリズムが実行するイテレーションの最大数。 | 20 | （>= 0） |
| `OPTIMIZER` | LDA モデルの推定に使用されるオプティマイザまたは推論アルゴリズム。 サポートされるオプションは、`"online"` （Online Variational Bayes）と `"em"` （Expected-Maximization）です。 | &quot;オンライン&quot; | `online`、`em` （大文字と小文字を区別しない） |
| `NUM_CLUSTERS` | 作成するクラスタ数（k）。 | 10 | （> 1） |
| `CHECKPOINT_INTERVAL` | キャッシュされたノード ID のチェックポイント頻度を指定します。 | 10 | （>= 1） |
| `DOC_CONCENTRATION` | 濃度パラメーター（「アルファ値」）は、ドキュメント間のトピック分布に関する事前前提を決定します。 デフォルトの動作は、オプティマイザによって決まります。 `EM` オプティマイザーの場合、アルファ値は 1.0 より大きくする必要があり（デフォルトは（50/k） + 1 の範囲で均一に分布）、対称なトピック分布を確保します。 `online` Optimizer では、アルファ値は 0 以上にすることができ（デフォルトは 1.0/k として均等に分散）、より柔軟なトピックの初期化が可能です。 | 自動 | 長さ k の任意の単一の値またはベクトル（EM では値が 1 を超える） |
| `KEEP_LAST_CHECKPOINT` | `em` Optimizer を使用する際に、最後のチェックポイントを保持するかどうかを示します。 チェックポイントを削除すると、データパーティションが失われた場合にエラーが発生する可能性があります。 チェックポイントは、参照カウントによって決定されるように、不要になったときにストレージから自動的に削除されます。 | `true` | `true`、`false` |
| `LEARNING_DECAY` | `online` Optimizer の学習率。`(0.5, 1.0]` 間の指数関数的減衰率として設定します。 | 0.51 | `(0.5, 1.0]` |
| `LEARNING_OFFSET` | 早期の反復を重み付けして早期の反復のカウントを減らす、`online` Optimizer の学習パラメータ。 | 1024 | （> 0） |
| `SEED` | アルゴリズム内のランダムプロセスを制御するためのランダムシード。 | 設定されていません | 任意の 64 ビット番号 |
| `OPTIMIZE_DOC_CONCENTRATION` | `online` Optimizer の場合：トレーニング中に `docConcentration` （ドキュメントトピック配布用の Dirichtlet パラメーター）を最適化するかどうか。 | `false` | `true`、`false` |
| `SUBSAMPLING_RATE` | `online` オプティマイザーの場合：範囲 `(0, 1]` でのミニバッチ グラデーション下降の各反復でサンプリングおよび使用されたコーパスの割合。 | 0.05 | `(0, 1]` |
| `TOPIC_CONCENTRATION` | 濃度パラメーター（「ベータ版」または「ベータ版」）は、各期間にわたるトピックの分布に関する事前前提を定義します。 デフォルト値はオプティマイザーによって決まります。`EM` の場合、値は 1.0 を超えます（デフォルトは 0.1 + 1）。 `online` の場合、値≥0 （デフォルトは 1.0/k）です。 | 自動 | 長さ k の任意の単一の値またはベクトル（EM の場合は値 > 1） |
| `TOPIC_DISTRIBUTION_COL` | このパラメータは、文献では「シータ」と呼ばれることが多い各文書について、推定されるトピック混合分布を出力します。 空のドキュメントの場合は、ゼロのベクトルを返します。 推定値は、変量近似法（「ガンマ」）を使用して導き出されます。 | 設定されていません | 任意の文字列 |

{style="table-layout:auto"}

**例**

```sql
Create MODEL modelname OPTIONS(
  type = 'lda',
) AS
  select col1, col2, col3 from training-dataset
```

## 次の手順

このドキュメントでは、様々なクラスターアルゴリズムの設定方法と使用方法を確認しました。 次に、他のタイプの高度な統計モデルについて詳しくは、[ 分類 ](./classification.md) および [ 回帰 ](./regression.md) に関するドキュメントを参照してください。
