---
keywords: Experience Platform；開発者ガイド；Data Science Workspace；よく読まれるトピック；リアルタイム機械学習；ノードリファレンス；
solution: Experience Platform
title: リアルタイム機械学習ノードリファレンス
topic-legacy: Nodes reference
description: ノードは、グラフを形成する基本単位です。 各ノードは特定のタスクを実行し、リンクを使用してMLパイプラインを表すグラフを形成して、互いにチェーンできます。 タスクは、データやスキーマの変換、機械学習推論などの入力データに対する操作を表す。 ノードは、変換された値または推定された値を次のノードに出力します。
exl-id: 67fe26b5-ce03-4a9a-ad45-783b2acf8d92
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '678'
ht-degree: 1%

---

# リアルタイム機械学習ノードリファレンス（アルファ）

>[!IMPORTANT]
>
>リアルタイム機械学習は、まだすべてのユーザーが利用できるわけではありません。 この機能はアルファベットで、まだテスト中です。 このドキュメントは変更される可能性があります。

ノードは、グラフを形成する基本単位です。 各ノードは特定のタスクを実行し、リンクを使用してMLパイプラインを表すグラフを形成して、互いにチェーンできます。 タスクは、データやスキーマの変換、機械学習推論などの入力データに対する操作を表す。 ノードは、変換された値または推定された値を次のノードに出力します。

次のガイドは、リアルタイム機械学習でサポートされるノードライブラリの概要を示しています。

## MLパイプラインで使用するノードを検出しています

次のコードを[!DNL Python]ノートブックにコピーして、使用可能なすべてのノードを表示します。

```python
from pprint import pprint
 
from rtml_nodelibs.core.nodefactory import NodeFactory as nf
```

```python
# Discover Nodes
pprint(nf.discover_nodes())
```

**レスポンスの例**

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

標準ノードは、PandasやScikitLearnなどのオープンソースのデータサイエンスライブラリを基に構築されます。

### ModelUpload

ModelUploadノードは、model_pathを使用し、ローカルモデルパスからReal-time Machine Learning blobストアにモデルをアップロードする内部Adobeノードです。

```python
model = ModelUpload(params={'model_path': model_path})
  
msg_model = model.process(None, 1)
  
model_id = msg_model.model['model_id']
```

### ONXNode

ONXNodeは、モデルIDを使用して事前にトレーニングを受けたONNXモデルを引き出し、それを使用して受信データのスコアを算出する内部Adobeノードです。

>[!TIP]
>
>ONNXモデルにデータを送信してスコアを求めるのと同じ順序で列を指定します。

```python
node_model_score = ONNXNode(params={"features": ['browser', 'device', 'login_page', 'product_page', 'search_page'], "model_id": model_id})
```

### パンダ{#pandas}

次のPandasノードを使用すると、任意の`pd.DataFrame`メソッドまたは任意の一般的なpandasトップレベル関数を読み込むことができます。 Pandasのメソッドの詳細については、[Pandasメソッドドキュメント](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html)を参照してください。 トップレベルの関数について詳しくは、[Pandas APIリファレンスガイド（一般的な関数](https://pandas.pydata.org/pandas-docs/stable/reference/general_functions.html)）を参照してください。

以下のノードでは、`"import": "map"`を使用してメソッド名をパラメーター内の文字列として読み込み、次にパラメーターをマップ関数として入力します。 以下の例では、`{"arg": {"Desktop": 1, "Mobile": 0}, "na_action": 0}`を使用してこれを行います。 マップを配置した後、`inplace`を`True`または`False`に設定するオプションがあります。 変換をインプレイスで適用するかどうかに基づいて、`inplace`を`True`または`False`に設定します。 デフォルトでは`"inplace": False`は新しい列を作成します。 新しい列名の提供のサポートは、以降のリリースで追加されるように設定されています。 最後の行`cols`は、1つの列名または列のリストにすることができます。 変換を適用する列を指定します。 この例では`device`が指定されています。

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

ScikitLearnノードを使用すると、任意のScikitLearn MLモデルまたはスケーラを読み込むことができます。 この例で使用されている値の詳細については、次の表を参照してください。

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
| features | モデルに入力フィーチャを追加します(文字列のリスト)。 <br> 次に例を示します。 `browser`,  `device`,  `login_page`,  `product_page`,  `search_page` |
| label | ターゲットの列名（文字列） |
| mode | トレーニング/テスト（文字列） |
| model_path | onnx形式でローカルに保存したモデルのパス。 |
| params.model | モデルへの絶対インポートパス（文字列）:`sklearn.linear_model.LogisticRegression`. |
| params.model_params | モデルハイパーパラメーターについて詳しくは、[sklearn API (map/dict)](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html)のドキュメントを参照してください。 |
| node_instance.process(data_message_from_previous_node) | `process()`メソッドは、前のノードからDataMsgを取得し、変換を適用します。 これは、現在使用されているノードに依存します。 |

### Split

次のノードを使用して、`train_size`または`test_size`を渡して、データフレームをトレインに分割し、テストします。 複数のインデックスを持つデータ・フレームを返します。 列車とテストのデータフレームには、次の例`msg5.data.xs(“train”)`を使用してアクセスできます。

```python
splitter = Split(params={"train_size": 0.7})
msg5 = splitter.process(msg4)
```

## 次の手順

次の手順は、リアルタイム機械学習モデルのスコアリングに使用するノードを作成することです。 詳細については、『[リアルタイム機械学習ノートブックユーザガイド](./rtml-authoring-notebook.md)』を参照してください。
