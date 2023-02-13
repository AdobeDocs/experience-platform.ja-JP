---
keywords: Experience Platform;開発者ガイド;データサイエンスワークスペース;人気のトピック;リアルタイム機械学習;ノードリファレンス;
solution: Experience Platform
title: リアルタイム機械学習ノードリファレンス
description: ノードは、グラフを構成する基本単位です。 各ノードでは特定のタスクを実行し、リンクを使用してノードを連結して、ML パイプラインを表すグラフを構成できます。 ノードで実行されるタスクは、入力データに対する操作（データやスキーマの変換など）や機械学習の推論を表します。 ノードは、変換された値や推論された値を次のノード（複数の場合あり）に出力します。
exl-id: 67fe26b5-ce03-4a9a-ad45-783b2acf8d92
source-git-commit: 86e6924078c115fb032ce39cd678f1d9c622e297
workflow-type: ht
source-wordcount: '678'
ht-degree: 100%

---

# リアルタイム機械学習ノードリファレンス（アルファ版）

>[!IMPORTANT]
>
>リアルタイム機械学習は、まだすべてのユーザーが利用できるわけではありません。この機能はアルファ版であり、まだテスト中です。このドキュメントは変更される場合があります。

ノードは、グラフを構成する基本単位です。 各ノードでは特定のタスクを実行し、リンクを使用してノードを連結して、ML パイプラインを表すグラフを構成できます。 ノードで実行されるタスクは、入力データに対する操作（データやスキーマの変換など）や機械学習の推論を表します。 ノードは、変換された値や推論された値を次のノード（複数の場合あり）に出力します。

次のガイドでは、リアルタイム機械学習でサポートされているノードライブラリの概要を説明します。

## ML パイプラインで使用するノードの検出

次のコードを [!DNL Python] ノートブックにコピーすると、使用可能なすべてのノードを表示できます。

```python
from pprint import pprint
 
from rtml_nodelibs.core.nodefactory import NodeFactory as nf
```

```python
# Discover Nodes
pprint(nf.discover_nodes())
```

**応答の例**

```json
{'FieldOps': 'rtml_nodelibs.nodes.standard.preprocessing.fieldops.FieldOps',
 'FieldTrans': 'rtml_nodelibs.nodes.standard.preprocessing.fieldtrans.FieldTrans',
 'ModelUpload': 'rtml_nodelibs.nodes.standard.ml.artifact_utils.ModelUpload',
 'ONNXNode': 'rtml_nodelibs.nodes.standard.ml.onnx.ONNXNode',
 'Pandas': 'rtml_nodelibs.nodes.standard.preprocessing.pandasnode.Pandas',
 'Processor': 'rtml_nodelibs.nodes.standard.preprocessing.processor.Processor',
 'Receiver': 'rtml_nodelibs.nodes.standard.preprocessing.receiver.Receiver',
 'ScikitLearn': 'rtml_nodelibs.nodes.standard.ml.scikitlearn.ScikitLearn',
 'Simulator': 'rtml_nodelibs.nodes.standard.preprocessing.simulator.Simulator',
 'Split': 'rtml_nodelibs.nodes.standard.preprocessing.splitter.Split'}
```

## 標準ノード

標準ノードは、Pandas や ScikitLearn などのオープンソースデータサイエンスライブラリを基に構築されています。

### ModelUpload

ModelUpload ノードは、model_path を取得し、ローカルモデルパスからリアルタイム機械学習 BLOB ストアにモデルをアップロードするアドビ内部ノードです。

```python
model = ModelUpload(params={'model_path': model_path})
  
msg_model = model.process(None, 1)
  
model_id = msg_model.model['model_id']
```

### ONNXNode

ONNXNode は、モデル ID を受け取って事前トレーニング済みの ONNX モデルを取り込み、それを使用して受信データにスコアを付けるアドビ内部ノードです。

>[!TIP]
>
>ONNX モデルにデータを送信してスコアを付けるのと同じ順序で列を指定します。

```python
node_model_score = ONNXNode(params={"features": ['browser', 'device', 'login_page', 'product_page', 'search_page'], "model_id": model_id})
```

### Pandas {#pandas}

次の Pandas ノードでは、 `pd.DataFrame` メソッドまたは Pandas の任意の汎用トップレベル関数を読み込むことができます。Pandas のメソッドについて詳しくは、[Pandas メソッドのドキュメント](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html)を参照してください。トップレベル関数について詳しくは、[汎用関数の Pandas API リファレンスガイド](https://pandas.pydata.org/pandas-docs/stable/reference/general_functions.html)を参照してください。

以下のノードでは、`"import": "map"` を使用して、メソッド名を文字列としてパラメーターに読み込んだあと、パラメーターをマップ関数として入力します。 以下の例では、これを行うのに `{"arg": {"Desktop": 1, "Mobile": 0}, "na_action": 0}` を使用しています。マップを用意したら、 `inplace` を `True` または `False` に設定することができます。変換をインプレースで適用するかどうかに応じて、`inplace` を `True` または `False` に設定します。`"inplace": False` の場合は、デフォルトで新しい列が作成されます。 新しい列名を指定する機能のサポートは、今後のリリースで追加される予定です。最後の行の `cols` は、単一の列名でも列のリストでも構いません。 変換を適用する列を指定します。 この例では、`device` が指定されています。

```python
#  df["device"] = df["device"].map({"Desktop":1, "Mobile":0}, na_action=0)
 
node_device_apply = Pandas(params={"import": "map",
    "kwargs": {"arg": {"Desktop": 1, "Mobile": 0}, "na_action": 0},
    "inplace": True,
    "cols": "device"})
 
 
# df["browser"] = df["browser"].map({"chrome": 1, "firefox": 0}, "na_action": 0})
 
node_browser_apply = Pandas(params={"import": "map",
    "kwargs": {"arg": {"chrome": 1, "firefox": 0}, "na_action": 0},
    "inplace": True,
    "cols": "browser"})
```

### ScikitLearn

ScikitLearn ノードでは、任意の ScikitLearn ML モデルまたはスケーラーを読み込むことができます。 この例で使用されている値の詳細については、次の表を参照してください。

```python
model_train = ScikitLearn(params={
    "features":['browser', 'device', 'login_page', 'product_page', 'search_page'],
    "label": "payment_confirm",
    "mode": "train",
    "model_path": "resources/model.onnx",
    "params": {
        "model": "sklearn.linear_model.LogisticRegression",
        "model_params": {"solver": 'lbfgs'}
    }})
msg6 = model_train.process(msg5)
```

| 値 | 説明 |
| --- | --- |
| features | モデルに入力されるフィーチャ（文字列のリスト）。 <br>例：`browser`、`device`、`login_page`、`product_page`、`search_page` |
| label | ターゲット列名（文字列）。 |
| mode | トレーニング／テスト（文字列）。 |
| model_path | onnx 形式でローカルにモデルを保存するパス。 |
| params.model | モデルへの絶対読み込みパス（文字列）。例： `sklearn.linear_model.LogisticRegression`。 |
| params.model_params | モデルのハイパーパラメーター。詳しくは、[sklearn API（map／dict）](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html)のドキュメントを参照してください。 |
| node_instance.process(data_message_from_previous_node) | メソッド `process()` は、前のノードから DataMsg を受け取り、変換を適用します。これは、使用中の現在のノードによって異なります。 |

### 分割

次のノードを使用して、データフレームをトレーニングとテストに分割するには、`train_size` または `test_size` を渡します。これは、マルチインデックスを持つデータフレームを返します。次の例 `msg5.data.xs("train")` を使用して、トレーニングとテストのデータフレームにアクセスできます。

```python
splitter = Split(params={"train_size": 0.7})
msg5 = splitter.process(msg4)
```

## 次の手順

次の手順では、リアルタイム機械学習モデルのスコアリングで使用するノードを作成します。詳しくは、[リアルタイム機械学習ノートブックユーザーガイド](./rtml-authoring-notebook.md)を参照してください。
