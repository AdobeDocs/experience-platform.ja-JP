---
keywords: Experience Platform;optimize;model;Data Science Workspace;popular topics
solution: Experience Platform
title: モデルの最適化
topic: Tutorial
translation-type: tm+mt
source-git-commit: 5699022d1f18773c81a0a36d4593393764cb771a
workflow-type: tm+mt
source-wordcount: '1286'
ht-degree: 0%

---


# モデルの最適化


このチュートリアルでは、次の点について説明します。

- レシピコードの設定
- カスタム指標の定義
- 事前にビルドされた評価指標とビジュアライゼーショングラフの使用

このチュートリアルを終了するまでに、レシピコードの設定、カスタム指標の定義、事前に作成された評価指標の使用、デフォルトのビジュアライゼーションチャートを行うことができます。

## 指標とは

モデルの実装とトレーニングの後、データサイエンティストが行う次の手順は、モデルのパフォーマンスを見つけることです。 様々な指標を使用して、モデルが他のモデルと比較してどの程度効果的かを見つけます。 使用される指標の例を以下に示します。
- 分類の正確性
- カーブ下の領域
- 混同行列
- 分類レポート

## モデルインサイトフレームワークとは

モデルインサイトフレームワークは、データサイエンティストにData Science Workspaceのツールを提供し、実験に基づく最適な機械学習モデルに関する情報に基づく迅速な選択を可能にします。 フレームワークは、機械学習ワークフローの速度と効果を向上させるとともに、データ科学者にとっての使いやすさを改善します。 これは、モデルの調整に役立つように、機械学習アルゴリズムのタイプごとにデフォルトのテンプレートを指定することで行います。 その結果、データ科学者と市民データ科学者は、エンドユーザーに対してより良いモデル最適化の判断を行うことができます。

## レシピコードの設定

現在、Model Insights Frameworkは、次のランタイムをサポートしています。
- [スカラ](#scala)
- [Python/Tensorflow](#pythontensorflow)
- [R](#r)

レシピのサンプルコードは、 [experience-platform-dsw-reference](https://github.com/adobe/experience-platform-dsw-reference) リポジトリの下にあ `recipes`ります。 このリポジトリの特定のファイルは、このチュートリアル全体で参照されます。

### スカラ {#scala}

指標をレシピに取り込む方法は2つあります。 1つはSDKが提供するデフォルトの評価指標を使用すること、もう1つはカスタム評価指標を作成することです。

#### Scalaのデフォルトの評価指標

デフォルトの評価は、分類アルゴリズムの一部として計算されます。 現在実装されている評価基準のデフォルト値を次に示します。

| エバリュエータークラス | `evaluation.class` |
--- | ---
| DefaultBinaryClassificationEvaluator | `com.adobe.platform.ml.impl.DefaultBinaryClassificationEvaluator` |
| DefaultMultiClassificationEvaluator | `com.adobe.platform.ml.impl.DefaultMultiClassificationEvaluator` |
| RecommendationsEvaluator | `com.adobe.platform.ml.impl.RecommendationsEvaluator` |

評価基準は、フ [ォルダー内の](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/scala/src/main/resources/application.properties) application.properties `recipe` ファイルのレシピで定義できます。 を有効にするサンプルコード `DefaultBinaryClassificationEvaluator` を以下に示します。

```scala
evaluation.class=com.adobe.platform.ml.impl.DefaultBinaryClassificationEvaluator
evaluation.labelColumn=label
evaluation.predictionColumn=prediction
training.evaluate=true
```

評価基準クラスを有効にした後、デフォルトでは、トレーニング中に多数の指標が計算されます。 デフォルトの指標は、次の行を `application.properties`

```scala
evaluation.metrics.com=com.adobe.platform.ml.impl.Constants.DEFAULT
```

>[!NOTE] 指標が定義されていない場合、デフォルトの指標がアクティブになります。

特定の指標は、の値を変更することで有効にでき `evaluation.metrics.com`ます。 次の例では、F-Score指標が有効です。

```scala
evaluation.metrics=com.adobe.platform.ml.impl.Constants.FSCORE
```

次の表に、各クラスのデフォルト指標を示します。 ユーザーは、 `evaluation.metric` 列の値を使用して特定の指標を有効にすることもできます。

| `evaluator.class` | デフォルトの指標 | `evaluation.metric` |
--- | --- | ---
| `DefaultBinaryClassificationEvaluator` | ・レシ <br>ーバー動作特性下の精度・リコール・ <br>混同行列 <br>-Fスコア・ <br>精度・レシーバー動作特性 <br><br>領域 | -`PRECISION` <br>-`RECALL` <br>-`CONFUSION_MATRIX` <br>-`FSCORE` <br>-`ACCURACY` <br>-`ROC` <br>-`AUROC` |
| `DefaultMultiClassificationEvaluator` | ・レシ <br>ーバー動作特性下の精度・リコール・ <br>混同行列 <br>-Fスコア・ <br>精度・レシーバー動作特性 <br><br>領域 | -`PRECISION` <br>-`RECALL` <br>-`CONFUSION_MATRIX` <br>-`FSCORE` <br>-`ACCURACY` <br>-`ROC` <br>-`AUROC` |
| `RecommendationsEvaluator` |  — 平均平均精度(MAP) <br>— 正規化割引累積ゲイン <br>平均逆数ランク <br>指標K | -`MEAN_AVERAGE_PRECISION` <br>-`NDCG` <br>-`MRR` <br>-`METRIC_K` |


#### Scalaのカスタム評価指標

カスタム評価基準は、フ `MLEvaluator.scala``Evaluator.scala` ァイル内のののインターフェイスを拡張することで提供できます。 例の [Evaluator.scala](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/scala/src/main/scala/com/adobe/platform/ml/Evaluator.scala) ファイルでは、カスタム `split()` および `evaluate()` 関数を定義します。 この `split()``evaluate()` 関数は、データをランダムに8:2の比率で分割し、3つの指標を定義して返します。 MAPE、MAE、RMSE。

>[!IMPORTANT] このクラスの場合は、新しい評価指標を作成する `MLMetric` 際には使用しないでください。作成し `"measures"``valueType``MLMetric` ない場合は、指標がカスタム評価指標テーブルに入力されません。
>  
> 実行する手順： `metrics.add(new MLMetric("MAPE", mape, "double"))`\
> 次の条件を満たさない： `metrics.add(new MLMetric("MAPE", mape, "measures"))`


レシピで定義した後は、次のステップでレシピで有効にします。 これは、プロジェクトの [フォルダー内の](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/scala/src/main/resources/application.properties) application.properties `resources` ファイルで行います。 ここ `evaluation.class``Evaluator` で、は `Evaluator.scala`

```properties
evaluation.class=com.adobe.platform.ml.Evaluator
```

データサイエンスワークスペースで、テストページの「評価指標」タブにインサイトを表示できます。

### Python/Tensorflow {#pythontensorflow}

現在のところ、PythonやTensorflowのデフォルトの評価指標はありません。 したがって、PythonまたはTensorflowの評価指標を取得するには、カスタム評価指標を作成する必要があります。 これは、 `Evaluator` クラスを実装することで実行できます。

#### Pythonのカスタム評価指標

カスタム評価指標の場合、評価基準に実装する必要がある主なメソッドは2つあります。 `split()` と `evaluate()`。

Pythonの場合、これらのメソッドは、 [クラスの](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/python/retail/retail/evaluator.py) evaluator.py `Evaluator` に定義されます。 の例については、 [evaluator.py](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/python/retail/retail/evaluator.py) リンクに従ってく `Evaluator`ださい。

Pythonで評価指標を作成するには、とのメソッドをユーザが実装する必要があ `evaluate()` り `split()` ます。

この `evaluate()` メソッドは、、、およびのプロパティを持つ指標オブジェクトの配列を含む指標オブジェクトを返し `name`ま `value``valueType`す。

この `split()` 方法の目的は、データの入力、トレーニングとテストのデータセットの出力を行うことです。 この例では、この `split()` メソッドは `DataSetReader` SDKを使用してデータを入力し、関連のない列を削除してデータをクリーンアップします。 データ内の既存の生のフィーチャから追加のフィーチャが作成されます。

この `split()` メソッドは、トレーニングとテストのデータフレームを返す必要があります。このデータフレームは、MLモデルのトレーニングとテスト `pipeline()` のための方法で使用されます。

#### テンソーフローのカスタム評価指標

Tensorflowの場合は、Pythonと同様に、クラス内のメソッド `evaluate()` とメソッド `split()` を実装する必要があり `Evaluator` ます。 例え `evaluate()`ば、トレインおよびテストのデータセットを返す際に、指標 `split()` を返す必要があります。

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

現在のところ、Rにはデフォルトの評価指標はありません。したがって、Rの評価指標を取得するには、 `applicationEvaluator` クラスをレシピの一部として定義する必要があります。

#### Rのカスタム評価指標

の主な目的 `applicationEvaluator` は、指標のキーと値のペアを含むJSONオブジェクトを返すことです。

この [applicationEvaluator.R](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/R/Retail%20-%20GradientBoosting/R/applicationEvaluator.R) は、例として使用できます。 この例では、は3つのよくあるセクション `applicationEvaluator` に分かれています。
- データの読み込み
- データの準備/機能のエンジニアリング
- 保存されたモデルを検索して評価

データは、 [retail.config.jsonで定義されているソースからデータセットに最初に読み込まれます](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/R/Retail%20-%20GradientBoosting/retail.config.json)。 そこから、機械学習モデルに合わせてデータがクリーニングされ、エンジニアリングされます。 最後に、このモデルを使用してデータセットを使用して予測を行い、予測値と実際の値から指標を計算します。 この場合、MAPE、MAE、RMSEが定義され、 `metrics` オブジェクト内で返されます。

## 事前ビルド指標とビジュアライゼーショングラフの使用

Senesie Model Insights Frameworkは、機械学習アルゴリズムの種類ごとに1つのデフォルトテンプレートをサポートします。 次の表に、一般的な高レベルの機械学習アルゴリズムクラスと、対応する評価指標およびビジュアライゼーションを示します。

| MLアルゴリズムの種類 | 評価指標 | ビジュアライゼーション |
--- | --- | ---
| 回帰 | - RMSE<br>- MAPE<br>- MASE<br>- MAE | 予測値と実際の値のオーバーレイ曲線 |
| バイナリ分類 |  — 混同行列<br>— 精度再現<br>— 精度<br>- F — スコア（特にF1、F2）<br>- AUC<br>- ROC | ROC曲線と混同行列 |
| 複数クラスの分類 |  — 混同行列 <br>— 各クラス： <br>— 精度再現精度 <br>- Fスコア（特にF1、F2） | ROC曲線と混同行列 |
| クラスタリング（真実を含む） | ・NMI（正規化相互情報スコア）、AMI（調整相互情報スコア）<br>- RI（ランド指数）、ARI（調整ランド指数）<br>— 等質スコア、完全性スコア、V-measure<br>- FMI（フォールケス — マローズ指数）<br>— 純度<br>— ジャカード指数 | クラスタ内にあるデータポイントを反映した相対クラスタサイズを持つクラスタと図心を示すクラスタプロット |
| クラスタリング（実例を除く） |  — 慣性<br>— シルエット係数<br>- CHI （カリンスキー — ハラバズ指数）<br>- DBI （デイビス — ブルディン指数）<br>— ダン指数 | クラスタ内にあるデータポイントを反映した相対クラスタサイズを持つクラスタと図心を示すクラスタプロット |
| 推奨 |  — 平均平均精度(MAP) <br>— 正規化割引累積ゲイン <br>平均逆数ランク <br>指標K | TBD |
| TensorFlowの使用例 | TensorFlowモデル分析(TFMA) | ニューラルネットワークモデルの比較/視覚化の詳細比較 |
| その他/エラーのキャプチャメカニズム | モデル作成者が定義したカスタム指標ロジック（および対応する評価グラフ）。 テンプレートが一致しない場合の正常なエラー処理 | 評価指標のキーと値のペアを含む表 |
