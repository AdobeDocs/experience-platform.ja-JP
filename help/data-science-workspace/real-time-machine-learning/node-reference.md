---
keywords: Experience Platform；開発者ガイド；Data Science Workspace；人気のあるトピック；リアルタイム機械学習；ノード参照；
solution: Experience Platform
title: リアルタイム機械学習ノードリファレンス
topic-legacy: Nodes reference
description: ノードは、グラフが形成される基本単位です。 各ノードは特定のタスクを実行し、リンクを使用して連結し、ML パイプラインを表すグラフを形成できます。 ノードが実行するタスクは、データやスキーマの変換、機械学習の推論などの入力データに対する操作を表す。 ノードは、変換または推論された値を次のノードに出力します。
exl-id: 67fe26b5-ce03-4a9a-ad45-783b2acf8d92
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '678'
ht-degree: 1%

---

# リアルタイム機械学習ノードリファレンス（アルファ）

>[!IMPORTANT]
>
>リアルタイム機械学習は、まだすべてのユーザーが利用できるわけではありません。 この機能はアルファ版で、まだテスト中です。 このドキュメントは変更される場合があります。

ノードは、グラフが形成される基本単位です。 各ノードは特定のタスクを実行し、リンクを使用して連結し、ML パイプラインを表すグラフを形成できます。 ノードが実行するタスクは、データやスキーマの変換、機械学習の推論などの入力データに対する操作を表す。 ノードは、変換または推論された値を次のノードに出力します。

次のガイドでは、リアルタイム機械学習でサポートされるノードライブラリの概要を説明します。

## ML パイプラインで使用するノードの検出

次のコードを [!DNL Python] ノートブックにコピーして、使用可能なすべてのノードを表示します。

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

標準ノードは、Pandas や ScikitLearn などのオープンソースデータサイエンスライブラリを基に構築されます。

### ModelUpload

ModelUpload ノードは、 model_path を取得し、ローカルモデルパスからリアルタイム機械学習 BLOB ストアにモデルをアップロードする内部Adobeノードです。

```python
model = ModelUpload(params={'model_path': model_path})
  
msg_model = model.process(None, 1)
  
model_id = msg_model.model['model_id']
```

### ONXNode

ONXNode は、モデル ID を取得して、事前にトレーニングされた ONNX モデルを取り込み、それを使用して受信データに対するスコアを付ける内部Adobeノードです。

>[!TIP]
>
>ONNX モデルにスコアを付けるデータの送信順と同じ順序で列を指定します。

```python
node_model_score = ONNXNode(params={"features": ['browser', 'device', 'login_page', 'product_page', 'search_page'], "model_id": model_id})
```

### パンダス {#pandas}

次の Pandas ノードを使用して、任意の `pd.DataFrame` メソッドまたは一般的な pandas の最上位機能をインポートできます。 Pandas メソッドの詳細については、[Pandas メソッドのドキュメント ](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html) を参照してください。 トップレベルの関数について詳しくは、[ 一般的な関数の Pandas API リファレンスガイド ](https://pandas.pydata.org/pandas-docs/stable/reference/general_functions.html) を参照してください。

以下のノードは、`"import": "map"` を使用して、メソッド名をパラメーターに文字列として読み込み、その後、パラメーターをマップ関数として入力します。 以下の例では、`{"arg": {"Desktop": 1, "Mobile": 0}, "na_action": 0}` を使用してこれを行います。 マップを配置した後、`inplace` を `True` または `False` に設定するオプションがあります。 変換をインプレースで適用するかどうかに基づいて、 `inplace` を `True` または `False` に設定します。 デフォルトでは、`"inplace": False` は新しい列を作成します。 新しい列名の提供のサポートは、今後のリリースで追加される予定です。 最後の行 `cols` は、1 つの列名または列のリストにすることができます。 変換を適用する列を指定します。 この例では、`device` が指定されています。

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

ScikitLearn ノードを使用すると、任意の ScikitLearn ML モデルまたはスケーラを読み込むことができます。 この例で使用されている値の詳細については、次の表を参照してください。

```python
model_train = ScikitLearn(params={
    "features":['browser', 'device', 'login_page', 'product_page', 'search_page'],
    "label": "payment_confirm",
    "mode": "train",
    "model_path": "resources/model.onnx",
    "params": {
        "model": "sklearn.linear_model.LogisticRegression",
        "model_params": {"solver" : 'lbfgs'}
    }})
msg6 = model_train.process(msg5)
```

| 値 | 説明 |
| --- | --- |
| features | モデルに対する入力フィーチャ（文字列のリスト）。 <br> 例： `browser`,  `device`,  `login_page`,  `product_page`,  `search_page` |
| label | ターゲット列名（文字列） |
| mode | トレーニング/テスト（文字列） |
| model_path | onnx 形式でローカルに保存モデルのパス。 |
| params.model | モデルの絶対読み込みパス（文字列）:`sklearn.linear_model.LogisticRegression`. |
| params.model_params | モデルハイパーパラメーターについては、 [sklearn API (map/dict)](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html) のドキュメントを参照してください。 |
| node_instance.process(data_message_from_previous_node) | メソッド `process()` は、前のノードから DataMsg を取得し、変換を適用します。 これは、現在使用されているノードによって異なります。 |

### Split

次のノードを使用して、`train_size` または `test_size` を渡して、データフレームをトレーニングとテストに分割します。 これにより、複数インデックスを持つデータフレームが返されます。 `msg5.data.xs(“train”)` の例を使用して、トレーニングとテストのデータフレームにアクセスできます。

```python
splitter = Split(params={"train_size": 0.7})
msg5 = splitter.process(msg4)
```

## 次の手順

次の手順では、リアルタイム機械学習モデルのスコアリングに使用するノードを作成します。 詳しくは、[ リアルタイム機械学習ノートブックのユーザーガイド ](./rtml-authoring-notebook.md) を参照してください。
