---
keywords: Experience Platform;optimize;model;Data Science Workspace;popular topics
solution: Experience Platform
title: モデルの最適化
topic: Tutorial
translation-type: tm+mt
source-git-commit: 5699022d1f18773c81a0a36d4593393764cb771a

---


# モデルの最適化


このチュートリアルでは、次の事項について説明します。

- レシピコードの設定
- カスタム指標の定義
- 事前に作成された評価指標とビジュアライゼーショングラフの使用

このチュートリアルの最後までに、レシピコードの設定、カスタム指標の定義、事前に作成された評価指標の使用、デフォルトの視覚化チャートを行うことができます。

## 指標とは

モデルの実装とトレーニングの後、データサイエンティストが行う次の手順は、モデルのパフォーマンスを見つけることです。 様々な指標を使用して、モデルが他のモデルと比較してどの程度効果的かを見つけます。 使用される指標の例を次に示します。
- 分類の正確性
- カーブ下の領域
- 混同行列
- 分類レポート

## Model Insights Frameworkとは

Model Insights Frameworkは、Data Science Workspaceのツールをデータサイエンティストに提供し、実験に基づく最適な機械学習モデルのための迅速で十分な情報に基づいた選択を可能にします。 フレームワークは、機械学習ワークフローの速度と効果を向上させ、データ科学者の使いやすさを向上させます。 これは、モデルの調整に役立つ各機械学習アルゴリズムタイプのデフォルトのテンプレートを提供することで行われます。 その結果、データ科学者や市民データ科学者は、エンド顧客に対してより良いモデル最適化の判断を行うことができます。

## レシピコードの設定

現在、Model Insights Frameworkは次のランタイムをサポートしています。
- [スカラ](#scala)
- [Python/Tensorflow](#pythontensorflow)
- [R](#r)

レシピのサンプルコードは、 [experience-platform-dsw-referenceリポジトリの下にあり](https://github.com/adobe/experience-platform-dsw-reference) ます `recipes`。 このリポジトリの特定のファイルは、このチュートリアル全体で参照されます。

### スカラ {#scala}

指標をレシピに取り込む方法は2つあります。 1つはSDKが提供するデフォルトの評価指標を使用し、もう1つはカスタム評価指標を作成することです。

#### Scalaのデフォルトの評価指標

デフォルトの評価は、分類アルゴリズムの一部として計算されます。 現在実装されている評価演算子のデフォルト値を次に示します。

| エバリュエータークラス | `evaluation.class` |
--- | ---
| DefaultBinaryClassificationEvaluator | `com.adobe.platform.ml.impl.DefaultBinaryClassificationEvaluator` |
| DefaultMultiClassificationEvaluator | `com.adobe.platform.ml.impl.DefaultMultiClassificationEvaluator` |
| RecommendationsEvaluator | `com.adobe.platform.ml.impl.RecommendationsEvaluator` |

評価基準は、フォルダー内の [application.propertiesファイルのレシピで](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/scala/src/main/resources/application.properties) 定義でき `recipe` ます。 を有効にするサンプルコー `DefaultBinaryClassificationEvaluator` ドを以下に示します。

```scala
evaluation.class=com.adobe.platform.ml.impl.DefaultBinaryClassificationEvaluator
evaluation.labelColumn=label
evaluation.predictionColumn=prediction
training.evaluate=true
```

エバリュエータークラスが有効になると、トレーニング中に多数の指標がデフォルトで計算されます。 デフォルトの指標は、次の行を `application.properties`

```scala
evaluation.metrics.com=com.adobe.platform.ml.impl.Constants.DEFAULT
```

>[!NOTE] 指標が定義されていない場合、デフォルトの指標がアクティブになります。

特定の指標は、の値を変更することで有効にできま `evaluation.metrics.com`す。 次の例では、F-Score指標が有効になっています。

```scala
evaluation.metrics=com.adobe.platform.ml.impl.Constants.FSCORE
```

次の表に、各クラスのデフォルト指標を示します。 また、列の値を使用して、特定の指標を有 `evaluation.metric` 効にすることもできます。

| `evaluator.class` | デフォルトの指標 | `evaluation.metric` |
--- | --- | ---
| `DefaultBinaryClassificationEvaluator` | ・レシー <br>バ動作特性の下の <br>精度・リコール・混同行列 <br>Fスコア・正解率・レシ <br><br><br>ーバ動作特性領域 | -`PRECISION` <br>-`RECALL` <br>-`CONFUSION_MATRIX` <br>-`FSCORE` <br>-`ACCURACY` <br>-`ROC` <br>-`AUROC` |
| `DefaultMultiClassificationEvaluator` | ・レシー <br>バ動作特性の下の <br>精度・リコール・混同行列 <br>Fスコア・正解率・レシ <br><br><br>ーバ動作特性領域 | -`PRECISION` <br>-`RECALL` <br>-`CONFUSION_MATRIX` <br>-`FSCORE` <br>-`ACCURACY` <br>-`ROC` <br>-`AUROC` |
| `RecommendationsEvaluator` |  — 平均平均精度(MAP) <br>— 正規化割引累積ゲ <br>イン平均逆数ラ <br>ンク指標K | -`MEAN_AVERAGE_PRECISION` <br>-`NDCG` <br>-`MRR` <br>-`METRIC_K` |


#### Scalaのカスタム評価指標

カスタム評価基準は、ファイル内ののインターフェイスを拡張することで `MLEvaluator.scala` 提供で `Evaluator.scala` きます。 この例の [Evaluator.scalaファイルで](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/scala/src/main/scala/com/adobe/platform/ml/Evaluator.scala) 、カスタムと関数を `split()` 定義し `evaluate()` ます。 この関 `split()` 数は、データをランダムに8:2の比率で分割し、3つの指標を `evaluate()` 定義して返します。MAPE、MAE、RMSE。

>[!IMPORTANT] クラスの `MLMetric` 場合は、新しい指標を作 `"measures"` 成す `valueType` る際に「for」を使用しないでください。そうしないと、指標はカスタム評価指 `MLMetric` 標のテーブルに入力されません。
>  
> 実行する操作： `metrics.add(new MLMetric("MAPE", mape, "double"))`\
> これではありません： `metrics.add(new MLMetric("MAPE", mape, "measures"))`


レシピで定義した後は、レシピでレシピを有効にします。 これは、プロジェクトの [フォルダー内の](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/scala/src/main/resources/application.properties) application.propertiesファイルで行わ `resources` れます。 ここでは、 `evaluation.class``Evaluator` が `Evaluator.scala`

```properties
evaluation.class=com.adobe.platform.ml.Evaluator
```

Data Science Workspaceで、ユーザーはテストページの「評価指標」タブでインサイトを確認できます。

### Python/Tensorflow {#pythontensorflow}

現時点では、PythonやTensorflowのデフォルトの評価指標は存在しません。 したがって、PythonやTensorflowの評価指標を取得するには、カスタム評価指標を作成する必要があります。 これは、クラスを実装することで行うことがで `Evaluator` きます。

#### Pythonのカスタム評価指標

カスタム評価指標の場合、評価基準に実装する必要がある主なメソッドは2つあります。 `split()` と `evaluate()`

Pythonの場合、これらのメソッドはクラスの [evaluator.pyに定義され](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/python/retail/retail/evaluator.py) ま `Evaluator` す。 evaluator.py [リンクを使用し](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/python/retail/retail/evaluator.py)`Evaluator`、

Pythonで評価指標を作成するには、とメソッドを実装する必要 `evaluate()` があ `split()` ります。

このメ `evaluate()` ソッドは、、およびのプロパティを持つ指標オブジェクトの配列を含む指標オ `name`ブジェク `value`トを返しま `valueType`す。

この方法の目的は、デ `split()` ータを入力し、トレーニングとテスト用のデータセットを出力することです。 この例では、メソッドは `split()``DataSetReader` SDKを使用してデータを入力し、関連のない列を削除してデータをクリーンアップします。 ここから、データ内の既存の生のフィーチャから追加のフィーチャが作成されます。

このメソ `split()` ッドは、トレーニングとテストのデータフレームを返し、MLモデルのトレーニングとテ `pipeline()` ストを行う方法で使用される必要があります。

#### テンソーフローのカスタム評価指標

Pythonと同様に、Tensorflowの場合は、クラス内のメソ `evaluate()` ッドと `split()` を実装 `Evaluator` する必要があります。 例え `evaluate()`ば、トレインとテストのデータセットを返す `split()` 際に、指標を返す必要があります。

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

現時点では、Rのデフォルトの評価指標はありません。したがって、Rの評価指標を取得するには、レシピの一部としてクラスを定 `applicationEvaluator` 義する必要があります。

#### Rのカスタム評価指標

の主な目的は、指標 `applicationEvaluator` のキーと値のペアを含むJSONオブジェクトを返すことです。

この [applicationEvaluator.Rは](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/R/Retail%20-%20GradientBoosting/R/applicationEvaluator.R) 、例として使用できます。 この例では、が3つのよく知られ `applicationEvaluator` たセクションに分かれています。
- データの読み込み
- データの準備/機能のエンジニアリング
- 保存されたモデルを検索して評価

データは、 [retail.config.jsonで定義されているソースからデータセットに最初に読み込まれます](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/R/Retail%20-%20GradientBoosting/retail.config.json)。 そこからデータが消去され、機械学習モデルに合わせて設計されます。 最後に、モデルを使用してデータセットを使用して予測を行い、予測値と実際の値から指標を計算します。 この場合、MAPE、MAE、RMSEが定義され、オブジェクト内で返され `metrics` ます。

## 事前に作成された指標とビジュアライゼーショングラフの使用

Sensei Model Insights Frameworkは、機械学習アルゴリズムのタイプごとに1つのデフォルトテンプレートをサポートします。 次の表に、一般的な高レベルの機械学習アルゴリズムクラスと、対応する評価指標およびビジュアライゼーションを示します。

| MLアルゴリズムの種類 | 評価指標 | ビジュアライゼーション |
--- | --- | ---
| 回帰 | - RMSE<br>- MAPE<br>- MASE<br>- MAE | 予測値と実際の値のオーバーレイ曲線 |
| バイナリ分類 |  — 混同行列<br>- Precision-recall<br>- Accuracy<br>- F-score（具体的にはF1、F2）<br>- AUC<br>- ROC | ROC曲線と混同行列 |
| 複数クラスの分類 |  — 混同行列 <br>— 各クラス： <br>— 精度再現精度 <br>- Fスコア（特にF1、F2） | ROC曲線と混同行列 |
| クラスタリング（地上真実を含む） | ・NMI（正規化された相互情報スコア）、AMI（調整された相互情報スコア）<br>- RI（ランド指数）、ARI（調整されたランド指数）<br>— 均質性スコア、完全性スコア、V-measure<br>-FMI（フォークス — マロー指数） — 純度<br><br>— ジャカード指数 | クラスタ内のデータポイントを反映した相対クラスタサイズのクラスタと図心を示すクラスタプロット |
| クラスタリング（真実なし） |  — 慣性<br>— シルエット係数<br>- CHI（カリンスキー — ハラバズ指数）<br>- DBI（ダヴィス — ブルディン指数）<br>— ダンインデックス | クラスタ内のデータポイントを反映した相対クラスタサイズのクラスタと図心を示すクラスタプロット |
| 推奨 |  — 平均平均精度(MAP) <br>— 正規化割引累積ゲ <br>イン平均逆数ラ <br>ンク指標K | 未定 |
| TensorFlowの使用例 | TensorFlowモデル分析(TFMA) | ニューラルネットワークモデルの比較/視覚化の詳細化 |
| その他/エラーのキャプチャメカニズム | モデル作成者が定義したカスタム指標ロジック（および対応する評価グラフ）。 テンプレートが一致しない場合の正常なエラー処理 | 評価指標のキーと値のペアを含む表 |
