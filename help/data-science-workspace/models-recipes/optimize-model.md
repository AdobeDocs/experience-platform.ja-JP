---
keywords: Experience Platform；最適化；モデル；Data Science Workspace；人気の高いトピック；モデルインサイト
solution: Experience Platform
title: モデルインサイトフレームワークを使用したモデルの最適化
type: Tutorial
description: モデルインサイトフレームワークは、Data Science Workspace　のツールをデータサイエンティストに提供し、実験に基づく最適な機械学習モデルのための迅速で十分な情報に基づいた選択を可能にします。
exl-id: f989a3f1-6322-47c6-b7d6-6a828766053f
source-git-commit: 86e6924078c115fb032ce39cd678f1d9c622e297
workflow-type: tm+mt
source-wordcount: '1265'
ht-degree: 88%

---

# モデルインサイトフレームワークを使用したモデルの最適化

モデルインサイトフレームワークは、データサイエンティストに [!DNL Data Science Workspace] 実験に基づく最適な機械学習モデルの選択を迅速かつ十分な情報に基づいて行う。 このフレームワークにより、機械学習ワークフローの速度と効果だけでなく、データサイエンティストによる使いやすさも向上します。これは、モデルの調整を支援するために、機械学習アルゴリズムのタイプごとにデフォルトのテンプレートを提供することによっておこなわれます。結果的に、データサイエンティストと市民データサイエンティストは、エンドカスタマーに対してより適切なモデル最適化の決定をおこなうことができます。

## 指標とは

モデルの実装とトレーニングの後、データサイエンティストが実行する次の手順は、モデルのパフォーマンスを確認することです。様々な指標を使用して、モデルが他のモデルと比較してどの程度効果的かが確認されます。使用される指標の例を次に示します。
- 分類の正確性
- カーブ下の領域
- 混同行列
- 分類レポート

## レシピコードの設定

現在、モデルインサイトフレームワークは次のランタイムをサポートしています。
- [Scala](#scala)
- [Python／Tensorflow](#pythontensorflow)
- [R](#r)

レシピのコード例は、`recipes` の [experience-platform-dsw-reference](https://github.com/adobe/experience-platform-dsw-reference) リポジトリーにあります。このリポジトリーの特定のファイルは、このチュートリアル全体で参照されます。

### Scala {#scala}

指標をレシピに取り込む方法は 2 つあります。1 つは SDK が提供するデフォルトの評価指標を使用すること、もう 1 つはカスタム評価指標を作成することです。

#### Scala のデフォルト評価指標

デフォルトの評価は、分類アルゴリズムの一部として計算されます。現在実装されている評価演算子のデフォルト値を次に示します。

| エバリュエータークラス | `evaluation.class` |
|--- | ---|
| DefaultBinaryClassificationEvaluator | `com.adobe.platform.ml.impl.DefaultBinaryClassificationEvaluator` |
| DefaultMultiClassificationEvaluator | `com.adobe.platform.ml.impl.DefaultMultiClassificationEvaluator` |
| RecommendationsEvaluator | `com.adobe.platform.ml.impl.RecommendationsEvaluator` |

エバリュエーターは、`recipe` フォルダー内の [application.properties](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/scala/src/main/resources/application.properties) ファイルのレシピで定義できます。`DefaultBinaryClassificationEvaluator` を有効にするコード例を以下に示します。

```scala
evaluation.class=com.adobe.platform.ml.impl.DefaultBinaryClassificationEvaluator
evaluation.labelColumn=label
evaluation.predictionColumn=prediction
training.evaluate=true
```

エバリュエータークラスが有効になると、トレーニング中に多数の指標がデフォルトで計算されます。デフォルトの指標は、`application.properties` に次の行を追加して宣言できます。

```scala
evaluation.metrics.com=com.adobe.platform.ml.impl.Constants.DEFAULT
```

>[!NOTE]
>
> 指標が定義されていない場合、デフォルトの指標がアクティブになります。

特定の指標は、`evaluation.metrics.com` の値を変更することで有効にできます。次の例では、F-Score 指標が有効になっています。

```scala
evaluation.metrics=com.adobe.platform.ml.impl.Constants.FSCORE
```

次の表に、各クラスのデフォルト指標を示します。また、`evaluation.metric` 列の値を使用して、特定の指標を有効にすることもできます。

| `evaluator.class` | デフォルトの指標 | `evaluation.metric` |
| --- | --- | --- |
| `DefaultBinaryClassificationEvaluator` | - 精度<br>- リコール<br>- 混同行列 <br>- Fスコア<br>- 正解率 <br>- レシーバー動作特性<br>- レシーバー動作特性の領域 | -`PRECISION` <br>-`RECALL` <br>-`CONFUSION_MATRIX` <br>-`FSCORE` <br>-`ACCURACY` <br>-`ROC` <br>-`AUROC` |
| `DefaultMultiClassificationEvaluator` | - 精度<br>- リコール<br>- 混同行列 <br>- Fスコア<br>- 正解率 <br>- レシーバー動作特性<br>- レシーバー動作特性の領域 | -`PRECISION` <br>-`RECALL` <br>-`CONFUSION_MATRIX` <br>-`FSCORE` <br>-`ACCURACY` <br>-`ROC` <br>-`AUROC` |
| `RecommendationsEvaluator` |  - MAP（Mean Average Precision）<br>- 正規割引累積利益<br>- 平均逆数ランク<br>- 指標 K | -`MEAN_AVERAGE_PRECISION` <br>-`NDCG` <br>-`MRR` <br>-`METRIC_K` |


#### Scala のカスタム評価指標

カスタムエバリュエーターは、`Evaluator.scala` ファイルで `MLEvaluator.scala` のインターフェイスを拡張することで提供できます。この例の [Evaluator.scala](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/scala/src/main/scala/com/adobe/platform/ml/Evaluator.scala) ファイルで 、カスタム `split()` と `evaluate()` 関数を定義します。`split()` 関数は、データをランダムに 8:2 の比率で分割し、`evaluate()` 関数は、MAPE、MAE、RMSE の 3 つの指標を定義して返します。

>[!IMPORTANT]
>
> `MLMetric` クラスの場合は、新しい `"measures"` を作成する際に `valueType` に `MLMetric` を使用しないでください。さもないと、指標はカスタム評価指標のテーブルに入力されません。
>  
> 以下を実行してください。 `metrics.add(new MLMetric("MAPE", mape, "double"))`\
> 以下は実行しないでください。 `metrics.add(new MLMetric("MAPE", mape, "measures"))`


レシピで定義した後は、レシピで有効にします。これは、プロジェクトの `resources` フォルダーの [ application.properties](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/scala/src/main/resources/application.properties) ファイルで実行されます。ここでは、`evaluation.class` が `Evaluator.scala` で定義された `Evaluator` に設定されます。

```scala
evaluation.class=com.adobe.platform.ml.Evaluator
```

内 [!DNL Data Science Workspace]を参照すると、実験ページの「評価指標」タブでインサイトを確認できます。

### [!DNL Python/Tensorflow] {#pythontensorflow}

現時点では、 [!DNL Python] または [!DNL Tensorflow]. したがって、 [!DNL Python] または [!DNL Tensorflow]に値を入力する場合は、カスタム評価指標を作成する必要があります。 これは、`Evaluator` クラスを実装することで実行できます。

####  のカスタム評価指標[!DNL Python]

カスタム評価指標の場合、評価基準に実装する必要がある主なメソッドは `split()` と `evaluate()` の 2 つです。

の場合 [!DNL Python]の場合、これらのメソッドはで定義されます。 [evaluator.py](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/python/retail/retail/evaluator.py) の `Evaluator` クラス。 `Evaluator` の例については、[evaluator.py](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/python/retail/retail/evaluator.py) リンクを参照してください。

での評価指標の作成 [!DNL Python] ユーザーが `evaluate()` および `split()` メソッド。

`evaluate()` メソッドは、`name`、`value`、`valueType` プロパティを持つ指標オブジェクトの配列を含む指標オブジェクトを返します。

`split()` メソッドの目的は、データを入力し、トレーニングとテスト用のデータセットを出力することです。この例では、`split()` メソッドは `DataSetReader` SDK を使用してデータを入力し、関連のない列を削除してデータをクリーンアップします。ここから、データ内既存の生の特徴から追加の特徴が作成されます。

`split()` メソッドは、トレーニングとテストのデータフレームを返します。これは、`pipeline()` メソッドで、ML モデルのトレーニングとテストで使用されます。

#### Tensorflow のカスタム評価指標

の場合 [!DNL Tensorflow]（に類似） [!DNL Python]，メソッド `evaluate()` および `split()` 内 `Evaluator` クラスを実装する必要があります。 `evaluate()` については指標を返し、`split()` はトレーニングとテストのデータセットを返す必要があります。

```PYTHON
from ml.runtime.python.Interfaces.AbstractEvaluator import AbstractEvaluator

class Evaluator(AbstractEvaluator):
    def __init__(self):
       print ("initiate")

    def evaluate(self, data=[], model={}, config={}):

        return metrics

    def split(self, config={}):

       return 'train', 'test'
```

### R {#r}

現時点では、R のデフォルトの評価指標はありません。したがって、R の評価指標を取得するには、レシピの一部として `applicationEvaluator` クラスを定義する必要があります。

#### R のカスタム評価指標

`applicationEvaluator` の主な目的は、指標のキーと値のペアを含む JSON オブジェクトを返すことです。

この [applicationEvaluator.R](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/R/Retail%20-%20GradientBoosting/R/applicationEvaluator.R) は、例として使用できます。この例では、`applicationEvaluator` が 3 つのよく知られたセクションに分かれています。
- データの読み込み
- データの準備と特徴量エンジニアリング
- 保存されたモデルの取得と評価

データは、まず、[retail.config.json](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/R/Retail%20-%20GradientBoosting/retail.config.json) で定義されているようにソースからデータセットに読み込まれます。その後、データが消去され、機械学習モデルに合わせて設計されます。最後に、モデルはデータセットを使用して予測をおこなうために使用され、予測値と実際の値から指標が計算されます。この場合、MAPE、MAE、RMSE が定義され、`metrics` オブジェクト内で返されます。

## 事前に作成された指標とビジュアライゼーショングラフの使用

この [!DNL Sensei Model Insights Framework] は、機械学習アルゴリズムのタイプごとに 1 つのデフォルトテンプレートをサポートします。 次の表に、一般的な高レベルの機械学習アルゴリズムクラスと、対応する評価指標およびビジュアライゼーションを示します。

| ML アルゴリズムの種類 | 評価指標 | ビジュアライゼーション |
| --- | --- | --- |
| 回帰 | - RMSE<br>- MAPE<br>- MASE<br>- MAE | 予測値と実際の値のオーバーレイ曲線 |
| バイナリ分類 |  — 混同行列<br>- Precision-recall<br>- 精度<br>- F　スコア（具体的にはF1、F2）<br>- AUC<br>- ROC | ROC 曲線と混同行列 |
| 複数クラスの分類 |  — 混同行列<br>— 各クラス：<br>— precision-recall accuracy <br>- Fスコア（特にF1、F2） | ROC 曲線と混同行列 |
| クラスタリング（グラウンドトゥルースあり） | - NMI（正規化相互情報量スコア）、AMI（調整相互情報量スコア）<br>- RI（ランド指数）、ARI（調整ランド指数）<br>— 均質性スコア、完全性スコア、V-measure<br>-FMI（Fowlkes-Mallows 指数） — <br>純度<br>— Jaccard 係数 | クラスタ内のデータポイントを反映した相対クラスタサイズのクラスタと図心を示すクラスタプロット |
| クラスタリング（グラウンドトゥルースなし） |  — 慣性<br>— シルエット係数<br>- CHI（Calinski-Harabaz 係数）<br>- DBI（Davies–Bouldin 係数）<br>— ダンインデックス | クラスタ内のデータポイントを反映した相対クラスタサイズのクラスタと図心を示すクラスタプロット |
| 推奨 |  - MAP（Mean Average Precision）<br>- 正規割引累積利益<br>- 平均逆数ランク<br>- 指標 K | 未定 |
| TensorFlow の使用例 | TensorFlow モデル分析（TFMA） | ニューラルネットワークモデルの比較と視覚化の詳細化 |
| その他／エラーのキャプチャメカニズム | モデル作成者が定義したカスタム指標ロジック（および対応する評価グラフ）。テンプレートが一致しない場合のグレースフルなエラー処理 | 評価指標のキーと値のペアを含む表 |
