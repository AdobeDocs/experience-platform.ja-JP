---
keywords: Experience Platform;developer guide;Data Science Workspace;popular topics;Real-time Machine Learning;node reference;
solution: Experience Platform
title: Real-time Machine Learningノードリファレンスガイド
topic: Nodes reference
translation-type: tm+mt
source-git-commit: a37a3dd0c8f784d69da25ac503ad388b57b05ed9
workflow-type: tm+mt
source-wordcount: '537'
ht-degree: 1%

---


# Real-time Machine Learningノードリファレンスガイド

>[!IMPORTANT]
>リアルタイム機械学習は、まだすべてのユーザーが利用できるわけではありません。 この機能はアルファベットで、まだテスト中です。 このドキュメントは変更される可能性があります。

次のガイドは、リアルタイム機械学習でサポートされるノードライブラリの概要を示しています。

## MLパイプラインで使用するノードを検出しています

次のコードをPythonノートブックにコピーして、使用可能なすべてのノードを表示します。

```python
from pprint import pprint
 
from rtml_nodelibs.core.nodefactory import NodeFactory as nf
```

```python
# Discover Nodes
pprint(nf.discover_nodes())
```

**レスポンスの例**

```python
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

標準ノードは、PandasやScikitLearnなどのオープンソースのデータサイエンスライブラリを基に構築されます。

### ModelUpload

ModelUploadノードは、model_pathを取り、ローカルモデルパスからReal-time Machine Learning blobストアにモデルをアップロードする内部Adobeノードです。

```python
model = ModelUpload(params={'model_path': model_path})
  
msg_model = model.process(None, 1)
  
model_id = msg_model.model['model_id']
```

### ONXNode

ONXNodeは、モデルIDを使用して、トレーニング済みのONNXモデルを取り込み、それを使用して受信データのスコアリングを行う内部Adobeノードです。

>[!TIP]
>ONNXモデルにデータを送信してスコアを求めるのと同じ順序で列を指定します。

```python
node_model_score = ONNXNode(params={"features": ['browser', 'device', 'login_page', 'product_page', 'search_page'], "model_id": model_id})
```

### パンダス {#pandas}

次のPandasノードを使用すると、任意の `pd.DataFrame` メソッドまたは任意の一般的なPandasトップレベル関数を読み込むことができます。 Pandasの方法の詳細については、Pandasの方法に関するドキュメントを参照して [ください](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html)。 トップレベルの関数について詳しくは、一般的な関数の [Pandas APIリファレンスガイドを参照してください](https://pandas.pydata.org/pandas-docs/stable/reference/general_functions.html)。

以下のノードでは、を使用 `"import": "map"` してメソッド名をパラメーター内の文字列として読み込み、続いてパラメーターをmap関数として入力します。 次の例では、を使用してこれを行い `{"arg": {"Desktop": 1, "Mobile": 0}, "na_action": 0}`ます。 マップを配置した後、trueまたはfalseに設定するオプション `inplace` があります。 変換 `inplace` をインプレイスで適用するかどうかを、に `True``False` 基づいて設定します。 デフォルトでは、新しい列 `"inplace": False` が作成されます。 新しい列名の提供のサポートは、以降のリリースで追加されるように設定されています。 最後の行 `cols` は、1つの列名または列のリストにすることができます。 変換を適用する列を指定します。 この例では、を指定し `device` ます。

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

ScikitLearnノードを使用すると、任意のScikitLearn MLモデルまたはスケーラーを読み込むことができます。 この例で使用されている値の詳細については、次の表を参照してください。

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
| 機能 | モデルに入力フィーチャを追加します(文字列のリスト)。 <br> 次に例を示します。 「browser」、「device」、「login_page」、「product_page」、「search_page」 |
| label | ターゲットの列名（文字列） |
| mode | トレーニング/テスト（文字列） |
| model_path | onnx形式でローカルに保存したモデルのパス。 |
| params.model | モデルへの絶対インポートパス（文字列）: &quot;sklearn.linear_model.LogisticRegression&quot;. |
| params.model_params | モデルハイパーパラメーターについて詳しくは、 [sklearn API(map/dict)](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html) のドキュメントを参照してください。 |
| node_instance.process(data_message_from_previous_node) | このメソッドは、前のノードからDataMsgを取得し、変換を適用します。 `process()` これは、現在使用されているノードに依存します。 |

### 分割

次のノードを使用して、またはに合格してデータフレームをトレインに分割し、テスト `train_size` し `test_size`ます。 複数のインデックスを持つデータ・フレームを返します。 次の例を使用して、列車とテストのデータフレームにアクセスでき `msg5.data.xs(“train”)`ます。

```python
splitter = Split(params={"train_size": 0.7})
msg5 = splitter.process(msg4)
```

## 次の手順

次の手順は、リアルタイム機械学習モデルのスコアリングに使用するノードを作成することです。 詳しくは、リアルタイム機械学習モデルの [スコアリングに関するチュートリアル](./node-reference.md) を参照してください。
