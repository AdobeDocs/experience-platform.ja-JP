---
keywords: Experience Platform、開発者ガイド、Data Science Workspace、人気の高いトピック、リアルタイム機械学習、ノード参照
solution: Experience Platform
title: リアルタイム機械学習ノードリファレンス
description: ノードは、グラフが形成される基本単位です。 各ノードは特定のタスクを実行し、リンクを使用して連結し、ML パイプラインを表すグラフを形成できます。 ノードが実行するタスクは、データやスキーマの変換、機械学習の推論などの入力データに対する操作を表します。 ノードは、変換された値または推測される値を次のノードに出力します。
exl-id: 67fe26b5-ce03-4a9a-ad45-783b2acf8d92
source-git-commit: 86e6924078c115fb032ce39cd678f1d9c622e297
workflow-type: tm+mt
source-wordcount: '678'
ht-degree: 1%

---

# リアルタイム機械学習ノードリファレンス（アルファ）

>[!IMPORTANT]
>
>リアルタイム機械学習は、まだすべてのユーザーが利用できるわけではありません。 この機能はアルファ版で、まだテスト中です。 このドキュメントは変更される場合があります。

ノードは、グラフが形成される基本単位です。 各ノードは特定のタスクを実行し、リンクを使用して連結し、ML パイプラインを表すグラフを形成できます。 ノードが実行するタスクは、データやスキーマの変換、機械学習の推論などの入力データに対する操作を表します。 ノードは、変換された値または推測される値を次のノードに出力します。

次のガイドでは、リアルタイム機械学習でサポートされるノードライブラリの概要を説明します。

## ML パイプラインで使用するノードの検出

次のコードを [!DNL Python] ノートブックを使用して、使用可能なすべてのノードを表示できます。

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

ModelUpload ノードは、model_path を取得し、ローカルモデルパスからリアルタイム機械学習 BLOB ストアにモデルをアップロードする内部Adobeノードです。

```python
model = ModelUpload(params={'model_path': model_path})
  
msg_model = model.process(None, 1)
  
model_id = msg_model.model['model_id']
```

### ONXNode

ONXNode は、モデル ID を使用して事前にトレーニングされた ONNX モデルを取り込み、それを使用して受信データに対するスコアを付ける内部Adobeノードです。

>[!TIP]
>
>ONNX モデルにデータを送信してスコアを付けるのと同じ順序で列を指定します。

```python
node_model_score = ONNXNode(params={"features": ['browser', 'device', 'login_page', 'product_page', 'search_page'], "model_id": model_id})
```

### パンダ {#pandas}

次の Pandas ノードを使用すると、 `pd.DataFrame` メソッドまたは一般的な pandas の最上位機能を使用できます。 Pandas メソッドの詳細については、 [Pandas メソッドドキュメント](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html). トップレベルの関数について詳しくは、 [一般的な関数向けの Pandas API リファレンスガイド](https://pandas.pydata.org/pandas-docs/stable/reference/general_functions.html).

以下のノードはを使用します。 `"import": "map"` メソッド名をパラメーター内の文字列として読み込み、パラメーターをマップ関数として入力します。 以下の例では、 `{"arg": {"Desktop": 1, "Mobile": 0}, "na_action": 0}`. マップを配置した後、次のオプションを設定できます。 `inplace` as `True` または `False`. 設定 `inplace` as `True` または `False` 変換をインプレースで適用するかどうかに基づきます。 デフォルト `"inplace": False` 新しい列を作成します。 新しい列名の提供のサポートは、今後のリリースで追加されるように設定されます。 最後の行 `cols` は、単一の列名または列のリストです。 変換を適用する列を指定します。 この例では、 `device` が指定されている。

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

ScikitLearn ノードを使用すると、任意の ScikitLearn ML モデルまたはスケーラを読み込むことができます。 この例で使用される値の詳細については、次の表を参照してください。

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
| features | モデルにフィーチャを入力します（文字列のリスト）。 <br> 例： `browser`, `device`, `login_page`, `product_page`, `search_page` |
| label | ターゲット列名（文字列）。 |
| mode | トレーニング/テスト（文字列）。 |
| model_path | 保存モデルのパスをローカルで onnx 形式で指定します。 |
| params.model | モデルへの絶対インポートパス（文字列）例： `sklearn.linear_model.LogisticRegression`. |
| params.model_params | モデルハイパーパラメーター (「 [sklearn API(map/dict)](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html) ドキュメントを参照してください。 |
| node_instance.process(data_message_from_previous_node) | メソッド `process()` 前のノードから DataMsg を取得し、変換を適用します。 これは、現在使用されているノードによって異なります。 |

### 分割

次のノードを使用して、を渡すことで、データフレームをトレーニングとテストに分割します。 `train_size` または `test_size`. これにより、複数インデックスを持つデータフレームが返されます。 次の例を使用して、トレーニングとテストのデータフレームにアクセスできます。 `msg5.data.xs("train")`.

```python
splitter = Split(params={"train_size": 0.7})
msg5 = splitter.process(msg4)
```

## 次の手順

次の手順では、リアルタイムの機械学習モデルのスコアリングに使用するノードを作成します。 詳しくは、 [リアルタイムの機械学習ノートブックユーザーガイド](./rtml-authoring-notebook.md).
