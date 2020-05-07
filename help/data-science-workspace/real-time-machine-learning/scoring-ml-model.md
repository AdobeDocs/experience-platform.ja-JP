---
keywords: Experience Platform;developer guide;Data Science Workspace;popular topics;Real time machine learning;node reference;
solution: Experience Platform
title: リアルタイム機械学習モデルのスコア
topic: Scoring a ML model
translation-type: tm+mt
source-git-commit: dd2c9714c410d00ef2ba06e9f9728747fc6f989a
workflow-type: tm+mt
source-wordcount: '537'
ht-degree: 0%

---


# リアルタイム機械学習モデルのスコア

>[!IMPORTANT]
>リアルタイム機械学習は、まだすべてのユーザーが利用できるわけではありません。 この機能はアルファベットで、まだテスト中です。 このドキュメントは変更される可能性があります。

このチュートリアルでは、リアルタイム機械学習ノードを使用して、受信データを事前処理し、ONNXモデルに対してスコアを付ける方法を説明します。

>[!IMPORTANT]
> - ノードで使用される関数はシリアライズできません。 例えば、パンダノードで使用されるラムダ関数です。
> - エッジのデプロイメントを手動で行った後、60秒のスリープ状態が発生します。 これは、EdgeUtilsのscore_edgeメソッドに転送できます。


>[!NOTE]
>リアルタイム機械学習で使用するモデルにスコアを付けるには、リアルタイム機械学習のモデルのト [レーニングに関する前のチュートリアルを完了している必要があります](./training-ml-model.md)

## 新しいノートブックを作成する

Adobe Experience Platform UIの「 **[!UICONTROL Data Science]** 」で「ノートブック **」を選択します。 次に、 **[!UICONTROL JupyterLabを選択し]** 、環境の読み込みに時間をかけます。

![JupyterLabを開く](../images/rtml/open-jupyterlab.png)

JupterLabランチャ内から **空のPython 3ノートブック** を選択して開始します。

![空白のPython](../images/rtml/python-blank.png)

## ノードの読み込みと検出

モデルに必要なすべてのパッケージを読み込むことで開始します。 ノードのオーサリングに使用する予定のパッケージが読み込まれていることを確認します。

>[!NOTE]
>インポートのリストは、作成したモデルによって異なる場合があります。

```python
from pprint import pprint
import json
from rtml_nodelibs.core.nodefactory import NodeFactory as nf
from rtml_sdk.graph.utils import GraphBuilder
from rtml_sdk.edge.utils import EdgeUtils

from rtml_nodelibs.nodes.standard.preprocessing.pandasnode import Pandas
from rtml_nodelibs.nodes.standard.ml.onnx import ONNXNode
from rtml_nodelibs.nodes.standard.preprocessing.json_to_df import JsonToDataframe
from rtml_nodelibs.core.datamsg import DataMsg
import uuid
```

使用可能なノードのリストを確認するには、次の例をコピーしてノートブックに貼り付けます。

```python
# Discover Nodes
pprint(nf.discover_nodes())
```

受け取った応答はノードのリストです。

![注記のリスト](../images/rtml/node-list.png)

## エッジスコアリングのノードオーサリング

次に、エッジスコアリングのノードを定義し、作成する必要があります。 この例 `model_id` のを、 [前のチュートリアルでのトレーニングの完了時に受け取ったモデルIDに置き換えます](./training-ml-model.md)。 この例では、Pandasノードを使用して、pd.DataDrameメソッドまたは一般的なpandas最上位関数を読み込みます。 マップ関数を読み込んで、2つのノードを作成します。 使用可能なノードとその使用方法の詳細については、 [ノードリファレンスガイドを参照してください](./node-reference.md)。

```python
model_id = '{your_model_id}' # Get Model ID from Training Notebook.

data = {'sepal_length': {0: 5.8},
        'sepal_width': {0: 2.8},
        'petal_length': {0: 5.1},
        'petal_width': {0: 2.4}}

json2DF_node = JsonToDataframe(params={"json_payload":json.dumps(data)})
fill_na_node = Pandas(params={"import": "fillna", "kwargs":{"value":0}})
model_score_node = ONNXNode(params={"features": ['sepal_length', 'sepal_width', 'petal_length', 'petal_width'],
                                    "model_id": model_id})
```

## グラフDSLの構築

ノードを作成した後、次の手順は、ノードを連結してグラフを作成することです。

開始を行います。

```python
nodes = [node_device_apply, node_browser_apply, node_model_score]
```

次に、ノードをエッジで接続します。 各タプルはエッジ接続です。

```python
edges = [ (node_device_apply, node_browser_apply), (node_browser_apply, node_model_score)]
```

ノードを接続したら、グラフを作成します。

```python
dsl = GraphBuilder.generate_dsl(nodes=nodes, edges=edges)
```

## Edgeに公開

グラフを作成したら、グラフの端に配置できます。

>[!IMPORTANT]
>頻繁にエッジにパブリッシュしないでください。これは、エッジノードに負荷がかかる可能性があります。 同じモデルを複数回パブリッシュすることはお勧めしません。

```python
edge_utils = EdgeUtils()
(edge_location, service_id) = edge_utils.publish_to_edge(dsl=dsl)
```

## Edgeクライアント

エッジに投稿した後、スコアリングはクライアントからのPOSTリクエストによって実行されます。 通常、これはMLスコアを必要とするクライアントアプリケーションから実行できます。 ポストマンからもできる。 ここでは、EdgeUtilsを使用してプロセスを示します。

```python
import time
time.sleep(600)
```

```python
data = {"sepal_length": {0: 5.8}, "sepal_width": {0: 2.8}, "petal_length": {0: 5.1}, "petal_width": {0: 2.4}}
```

```python
scored_output = edge_utils.score_edge(edge_location=edge_location,
                              service_id=service_id,
                              scoring_data=data)
print(f'Scored Output from Edge: {scored_output}')
```

スコアリングが完了すると、エッジURL、ペイロード、およびエッジからのスコアリング出力が返されます。

![採点完了](../images/rtml/scoring-complete.png)

## デプロイ済みのアプリをedgeから削除（オプション）

>!![CAUTION]
このセルは、デプロイ済みのエッジアプリケーションを削除するために使用されます。 デプロイ済みのエッジアプリケーションを削除する必要がある場合を除き、次のセルを使用しないでください。

```python
if edge_utils.delete_from_edge(service_id=service_id):
    print(f"Deleted service id {service_id} successfully")
else:
    print(f"Failed to delete service id {service_id}")
```

## 次の手順

上記のチュートリアルに従うことで、リアルタイム機械学習モデルのスコアと導入に成功しました。 モデルオーサリングに使用できるノードの詳細については、 [ノードリファレンスガイドを参照してください](./node-reference.md)。



