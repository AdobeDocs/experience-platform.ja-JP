---
keywords: Experience Platform；開発者ガイド；Data Science Workspace；人気の高いトピック；リアルタイムの機械学習；ノード参照；
solution: Experience Platform
title: リアルタイム機械学習ノートブックの管理
topic-legacy: Training and scoring a ML model
description: 次のガイドは、JupterLabAdobe Experience PlatformでReal-time Machine Learningアプリケーションを構築するために必要な手順を説明しています。
exl-id: 604c4739-5a07-4b5a-b3b4-a46fd69e3aeb
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1669'
ht-degree: 0%

---

# リアルタイム機械学習ノートブック（アルファ）の管理

>[!IMPORTANT]
>
>リアルタイム機械学習は、まだすべてのユーザーが利用できるわけではありません。 この機能はアルファベットで、まだテスト中です。 このドキュメントは変更される可能性があります。

次のガイドでは、リアルタイム機械学習アプリケーションを作成するために必要な手順について説明します。 **[!UICONTROL Real-time ML]** PythonノートパソコンのAdobeを使って、モデルのトレーニング、DSLの作成、DSLのエッジへの公開、リクエストのスコアリングを行います。 リアルタイム機械学習モデルの導入に進むにつれて、データセットのニーズに合わせてテンプレートを変更することが期待されます。

## リアルタイム機械学習ノートブックの作成

Adobe Experience PlatformのUIで、**データサイエンス**&#x200B;内から「**[!UICONTROL ノートブック]**」を選択します。 次に、**[!UICONTROL JupyterLab]**&#x200B;を選択し、環境の読み込みに時間をかけます。

![JupyterLabを開く](../images/rtml/open-jupyterlab.png)

[!DNL JupyterLab]ランチャーが表示されます。 「*Real-Time Machine Learning*」まで下にスクロールし、**[!UICONTROL Real-time XML]**&#x200B;ノートブックを選択します。 データセットの例が記載されたノートブックセルを含むテンプレートが開きます。

![空白のPython](../images/rtml/authoring-notebook.png)

## ノードの読み込みと検出

モデルに必要なすべてのパッケージを読み込むことで開始します。 ノードのオーサリングに使用する予定のパッケージが読み込まれていることを確認します。

>[!NOTE]
>
>インポートのリストは、作成するモデルによって異なる場合があります。 新しいノードが時間の経過と共に追加されるので、このリストは変更されます。 使用可能なノードの完全なリストについては、[ノードリファレンスガイド](./node-reference.md)を参照してください。

```python
from pprint import pprint
import pandas as pd
import numpy as np
import json
import uuid
from shutil import copyfile
from pathlib import Path
from datetime import date, datetime, timedelta
from platform_sdk.dataset_reader import DatasetReader

from rtml_nodelibs.nodes.standard.preprocessing.json_to_df import JsonToDataframe
from rtml_sdk.edge.utils import EdgeUtils
from rtml_sdk.graph.utils import GraphBuilder
from rtml_nodelibs.nodes.standard.ml.onnx import ONNXNode
from rtml_nodelibs.core.nodefactory import NodeFactory as nf
from rtml_nodelibs.nodes.standard.preprocessing.pandasnode import Pandas
from rtml_nodelibs.nodes.standard.preprocessing.one_hot_encoder import OneHotEncoder
from rtml_nodelibs.nodes.standard.ml.artifact_utils import ModelUpload
from rtml_nodelibs.core.nodefactory import NodeFactory as nf
from rtml_nodelibs.core.datamsg import DataMsg
```

次のコードセルは、使用可能なノードのリストを出力します。

```python
# Discover Nodes
pprint(nf.discover_nodes())
```

![注記のリスト](../images/rtml/node-list.png)

## リアルタイム機械学習モデルのトレーニング

次のいずれかのオプションを使用して、[!DNL Python]コードを記述し、データの読み取り、前処理、分析を行います。 次に、独自のMLモデルをトレーニングし、ONNX形式にシリアル化して、Real-time Machine Learningモデルストアにアップロードする必要があります。

- [JupterLabノートブックでの独自のモデルのトレーニング](#training-your-own-model)
- [JupterLabノートブックに、トレーニング済みのONNXモデルをアップロード](#pre-trained-model-upload)

### 独自のモデルのトレーニング{#training-your-own-model}

開始を設定します。

>[!NOTE]
>
>**リアルタイムML**&#x200B;テンプレートでは、[車保険のCSVデータセット](https://github.com/adobe/experience-platform-dsw-reference/tree/master/datasets/insurance)が[!DNL Github]から取得されます。

![トレーニングデータの読み込み](../images/rtml/load_training.png)

Adobe Experience Platform内のデータセットを使用する場合は、下のセルのコメントを解除します。 次に、`DATASET_ID`を適切な値に置き換える必要があります。

![rtmlデータセット](../images/rtml/rtml-dataset.png)

[!DNL JupyterLab]ノートブックのデータセットにアクセスするには、[!DNL JupyterLab]の左側のナビゲーションにある「**データ**」タブを選択します。 **[!UICONTROL Datasets]**&#x200B;ディレクトリと&#x200B;**[!UICONTROL スキーマ]**&#x200B;ディレクトリが表示されます。 「**[!UICONTROL データセット]**」を選択して右クリックし、使用するデータセットのドロップダウンメニューから「**[!UICONTROL ノートブックのデータを探索]**」オプションを選択します。 ノートブックの下部に実行可能コードのエントリが表示されます。 このセルには`dataset_id`が入っています。

![データセットアクセス](../images/rtml/access-dataset.png)

完了したら、右クリックして、ノートブックの下部で生成したセルを削除します。

### トレーニングのプロパティ

表示されたテンプレートを使用して、`config_properties`内のトレーニングプロパティを変更します。

```python
config_properties = {
    "train_records_limit":1000000,
    "n_estimators": "80",
    "max_depth": "5",
    "ten_id": "_experienceplatform"  
}
```

### モデルの準備

**[!UICONTROL リアルタイムML]**&#x200B;テンプレートを使用する場合は、MLモデルの分析、事前処理、トレーニング、評価を行う必要があります。 これは、データ変換を適用し、トレーニングパイプラインを構築することで行います。

**データ変換**

**[!UICONTROL リアルタイムML]**&#x200B;テンプレート&#x200B;**データ変換**&#x200B;セルは、独自のデータセットを使用できるように変更する必要があります。 通常は、列名の変更、データのロールアップ、データの準備/機能の設計に関係します。

>[!NOTE]
>
>次の例は、`[ ... ]`を使用して読みやすくまとめたものです。 表示して、完全なコードセルの&#x200B;*リアルタイムML*&#x200B;テンプレートのデータ変換セクションを展開してください。

```python
df1.rename(columns = {config_properties['ten_id']+'.identification.ecid' : 'ecid',
                     [ ... ]}, inplace=True)
df1 = df1[['ecid', 'km', 'cartype', 'age', 'gender', 'carbrand', 'leasing', 'city', 
       'country', 'nationality', 'primaryuser', 'purchase', 'pricequote', 'timestamp']]
print("df1 shape 1", df1.shape)
#########################################
# Data Rollup
######################################### 
df1['timestamp'] = pd.to_datetime(df1.timestamp)
df1['hour'] = df1['timestamp'].dt.hour.astype(int)
df1['dayofweek'] = df1['timestamp'].dt.dayofweek

df1.loc[(df1['purchase'] == 'yes'), 'purchase'] = 1
df1.purchase.fillna(0, inplace=True)
df1['purchase'] = df1['purchase'].astype(int)

[ ... ]

print("df1 shape 2", df1.shape)

#########################################
# Data Preparation/Feature Engineering
#########################################      

df1['carbrand'] = df1['carbrand'].str.lower()
df1['country'] = df1['country'].str.lower()
df1.loc[(df1['carbrand'] == 'vw'), 'carbrand'] = 'volkswagen'

[ ... ]

df1['age'].fillna(df1['age'].median(), inplace=True)
df1['gender'].fillna('notgiven', inplace=True)

[ ... ]

df1['city'] = df1.groupby('country')['city'].transform(lambda x : x.fillna(x.mode()))
df1.dropna(subset = ['pricequote'], inplace=True)
print("df1 shape 3", df1.shape)
print(df1)

#grouping
grouping_cols = ['carbrand', 'cartype', 'city', 'country']

for col in grouping_cols:
    df_idx = pd.DataFrame(df1[col].value_counts().head(6))

    def grouping(x):
        if x in df_idx.index:
            return x
        else:
            return "Others"
    df1[col] = df1[col].apply(lambda x: grouping(x))

def age(x):
    if x < 20:
        return "u20"
    elif x > 19 and x < 29:
    [ ... ]
    else: 
        return "Others"

df1['age'] = df1['age'].astype(int)
df1['age_bucket'] = df1['age'].apply(lambda x: age(x))

df_final = df1[['hour', 'dayofweek','age_bucket', 'gender', 'city',  
   'country', 'carbrand', 'cartype', 'leasing', 'pricequote', 'purchase']]
print("df final", df_final.shape)

cat_cols = ['age_bucket', 'gender', 'city', 'dayofweek', 'country', 'carbrand', 'cartype', 'leasing']
df_final = pd.get_dummies(df_final, columns = cat_cols)
```

指定したセルを実行して、結果の例を表示します。 `carinsurancedataset.csv`データセットから返される出力テーブルは、定義した変更を返します。

![データ変換の例](../images/rtml/table-return.png)

**トレーニングパイプライン**

次に、トレーニングパイプラインを作成する必要があります。 これは、ONNXファイルを変換して生成する必要がある以外は、他のトレーニングパイプラインファイルと同様です。

前のセルで定義したデータ変換を使用して、テンプレートを変更します。 以下に示すコードは、機能パイプラインでONNXファイルを生成する際に使用します。 *リアルタイムML*&#x200B;テンプレートをパイプラインコードの完全なセルに表示してください。

```python
#for generating onnx
def generate_onnx_resources(self):        
    install_dir = os.path.expanduser('~/my-workspace')
    print("Generating Onnx")
        
    from skl2onnx import convert_sklearn
    from skl2onnx.common.data_types import FloatTensorType
        
    # ONNX-ification
    initial_type = [('float_input', FloatTensorType([None, self.feature_len]))]

    print("Converting Model to Onnx")
    onx = convert_sklearn(self.model, initial_types=initial_type)
             
    with open("model.onnx", "wb") as f:
        f.write(onx.SerializeToString())
            
    print("Model onnx created")
```

トレーニングパイプラインを完了し、データ変換を使用してデータを変更したら、次のセルを使用してトレーニングを実行します。

```python
model = train(config_properties, df_final)
```

### ONNXモデルの生成とアップロード

トレーニングの実施が完了したら、ONNXモデルを生成し、トレーニングを受けたモデルをReal-time Machine Learningモデルストアにアップロードする必要があります。 次のセルを実行すると、ONNXモデルは、他のすべてのノートブックと共に左側のレールに表示されます。

```python
import os
import skl2onnx, subprocess

model.generate_onnx_resources()
```

>[!NOTE]
>
>`model_path`文字列値(`model.onnx`)を変更して、モデルの名前を変更します。

```python
model_path = "model.onnx"
```

>[!NOTE]
>
>次のセルは編集も削除もできず、Real-time Machine Learningアプリケーションが動作するために必要です。

```python
model = ModelUpload(params={'model_path': model_path})
msg_model = model.process(None, 1)
model_id = msg_model.model['model_id']
 
print("Model ID : ", model_id)
```

![ONNXモデル](../images/rtml/onnx-model-rail.png)

### トレーニングを受けたONNXモデル{#pre-trained-model-upload}をアップロード

[!DNL JupyterLab]ノートブックにある「アップロード」ボタンを使用して、トレーニングを受けたONNXモデルを[!DNL Data Science Workspace]ノートブック環境にアップロードします。

![アップロードアイコン](../images/rtml/upload.png)

次に、*リアルタイムML*&#x200B;ノートブックの`model_path`文字列値を、ONNXモデル名と一致するように変更します。 完了したら、*Set model path*&#x200B;セルを実行し、*Upload your model to RTML Model Store*&#x200B;セルを実行します。 成功した場合、モデルの場所とモデルIDはどちらも応答に返されます。

![自分のモデルのアップロード](../images/rtml/upload-own-model.png)

## ドメイン固有言語(DSL)の作成

この節では、DSLの作成について概説します。 ONNXノードと共に、データの前処理を含むノードを作成します。 次に、ノードとエッジを用いてDSLグラフを作成する。 エッジは、タプルベースのフォーマット(node_1、node_2)を使用してノードを接続します。 グラフにサイクルは含めないでください。

>[!IMPORTANT]
>
>ONNXノードの使用は必須です。 ONNXノードがないと、アプリケーションは失敗します。

### ノードオーサリング

>[!NOTE]
>
> 使用するデータのタイプに基づいて複数のノードを持つ場合があります。 次の例は、*リアルタイムML*&#x200B;テンプレート内の1つのノードのみを示しています。 *リアルタイムML*&#x200B;テンプレート&#x200B;*ノードオーサリング*&#x200B;のセクションを完全なコードセルに表示してください。

以下のPandasノードは、`"import": "map"`を使用して、メソッド名をパラメーター内の文字列として読み込み、次にパラメーターをmap関数として入力します。 以下の例では、`{'arg': {'dataLayerNull': 'notgiven', 'no': 'no', 'yes': 'yes', 'notgiven': 'notgiven'}}`を使用してこれを行います。 マップを配置した後、`inplace`を`True`または`False`に設定するオプションがあります。 変換をインプレイスで適用するかどうかに基づいて、`inplace`を`True`または`False`に設定します。 デフォルトでは`"inplace": False`は新しい列を作成します。 新しい列名の提供のサポートは、以降のリリースで追加されるように設定されています。 最後の行`cols`は、1つの列名または列のリストにすることができます。 変換を適用する列を指定します。 この例では`leasing`が指定されています。 使用可能なノードとその使用方法について詳しくは、[ノードリファレンスガイド](./node-reference.md)を参照してください。

```python
# Renaming leasing column using Pandas Node
leasing_mapper_node = Pandas(params={'import': 'map',
                                'kwargs': {'arg': {
                                    'dataLayerNull': 'notgiven', 
                                    'no': 'no', 
                                    'yes': 'yes', 
                                    'notgiven': 'notgiven'}},
                                'inplace': True,
                                'cols': 'leasing'})
```

### DSLグラフの作成

ノードを作成した後、次の手順は、ノードを連結してグラフを作成することです。

開始を行います。

```python
nodes = [json_df_node, 
        to_datetime_node,
        hour_node,
        dayofweek_node,
        age_fillna_node,
        carbrand_fillna_node,
        country_fillna_node,
        cartype_primary_nationality_km_fillna_node,
        carbrand_mapper_node,
        cartype_mapper_node,
        country_mapper_node,
        gender_mapper_node,
        leasing_mapper_node,
        age_to_int_node,
        age_bins_node,
        dummies_node, 
        onnx_node]
```

次に、ノードをエッジで接続します。 各タプルは[!DNL Edge]接続です。

>[!TIP]
>
> ノードは互いに線形的に依存しているので（各ノードは前のノードの出力に依存しています）、単純なPythonリストの理解を使ってリンクを作成できます。 ノードが複数の入力に依存する場合は、独自の接続を追加してください。

```python
edges = [(nodes[i], nodes[i+1]) for i in range(len(nodes)-1)]
```

ノードを接続したら、グラフを作成します。 下のセルは必須で、編集または削除できません。

```python
dsl = GraphBuilder.generate_dsl(nodes=nodes, edges=edges)
pprint(json.loads(dsl))
```

完了すると、各ノードとそれらにマッピングされたパラメーターを含む`edge`オブジェクトが返されます。

![エッジリターン](../images/rtml/edge-return.png)

## Edgeに公開（ハブ）

>[!NOTE]
>
>リアルタイム機械学習は、Adobe Experience Platformハブに一時的に導入され、管理されます。 詳しくは、[リアルタイム機械学習アーキテクチャ](./home.md#architecture)の概要セクションを参照してください。

DSLグラフを作成したら、[!DNL Edge]にグラフを配信できます。

>[!IMPORTANT]
>
>頻繁に[!DNL Edge]に発行しないでください。これは[!DNL Edge]ノードに大きな負荷を与える可能性があります。 同じモデルを複数回パブリッシュすることはお勧めしません。

```python
edge_utils = EdgeUtils()
(edge_location, service_id) = edge_utils.publish_to_edge(dsl=dsl)
print(f'Edge Location: {edge_location}')
print(f'Service ID: {service_id}')
```

### DSLの更新とEdgeへの再公開（オプション）

DSLを更新する必要がない場合は、[スコア](#scoring)にスキップできます。

>[!NOTE]
>
>次のセルは、Edgeに公開された既存のDSLを更新する場合にのみ必要です。

モデルは開発を続ける可能性が高い。 新しいサービスを作成する代わりに、既存のサービスを新しいモデルで更新できます。 更新したいノードを定義し、新しいIDを割り当ててから、新しいDSLを[!DNL Edge]に再度アップロードすることができます。

次の例では、ノード0は新しいIDで更新されます。

```python
# Update the id of Node 0 with a random uuid.

dsl_dict = json.loads(dsl)
print(f"ID of Node 0 in current DSL: {dsl_dict['edge']['applicationDsl']['nodes'][0]['id']}")

new_node_id = str(uuid.uuid4())
print(f'Updated Node ID: {new_node_id}')

dsl_dict['edge']['applicationDsl']['nodes'][0]['id'] = new_node_id
```

![更新されたノード](../images/rtml/updated-node.png)

ノードIDを更新した後、更新されたDSLをEdgeに再公開できます。

```python
# Republish the updated DSL to Edge
(edge_location_ret, service_id, updated_dsl) = edge_utils.update_deployment(dsl=json.dumps(dsl_dict), service_id=service_id)
print(f'Updated dsl: {updated_dsl}')
```

更新されたDSLが返されます。

![更新されたDSL](../images/rtml/updated-dsl.png)

## スコアリング {#scoring}

[!DNL Edge]に投稿した後、スコアリングはクライアントからのPOSTリクエストによって行われます。 通常、これはMLスコアを必要とするクライアントアプリケーションから実行できます。 ポストマンからもできる。 **[!UICONTROL リアルタイムML]**&#x200B;テンプレートは、EdgeUtilsを使用してこのプロセスを示します。

>[!NOTE]
>
>スコアリング開始の前に、少しの処理時間が必要です。

```python
# Wait for the app to come up
import time
time.sleep(20)
```

トレーニングで使用したのと同じスキーマを使用して、サンプルスコアリングデータが生成されます。 このデータは、スコアリングデータフレームを作成し、スコアリングディクショナリに変換するために使用されます。 *リアルタイムML*&#x200B;テンプレートを完全なコードセルに表示してください。

![スコアリングデータ](../images/rtml/generate-score-data.png)

### エッジエンドポイントに対するスコア

*リアルタイムML*&#x200B;テンプレート内の次のセルを使用して、[!DNL Edge]サービスに対してスコアを付けます。

![エッジに対するスコア](../images/rtml/scoring-edge.png)

スコアリングが完了すると、[!DNL Edge] URL、ペイロード、および[!DNL Edge]からのスコア出力が返されます。

## [!DNL Edge]からデプロイ済みのアプリをリスト

[!DNL Edge]上に現在デプロイされているアプリのリストを生成するには、次のコードセルを実行します。 このセルは編集または削除できません。

```python
services = edge_utils.list_deployed_services()
print(services)
```

返される応答は、デプロイ済みのサービスの配列です。

```json
[
    {
        "created": "2020-05-25T19:18:52.731Z",
        "deprecated": false,
        "id": "40eq76c0-1c6f-427a-8f8f-54y9cdf041b7",
        "type": "edge",
        "updated": "2020-05-25T19:18:52.731Z"
    }
]
```

## デプロイ済みのアプリケーションまたはサービスIDを[!DNL Edge]から削除する（オプション）

>[!CAUTION]
>
>このセルは、デプロイ済みのEdgeアプリケーションを削除するために使用します。 デプロイ済みの[!DNL Edge]アプリケーションを削除する必要がない場合は、次のセルを使用しないでください。

```python
if edge_utils.delete_from_edge(service_id=service_id):
    print(f"Deleted service id {service_id} successfully")
else:
    print(f"Failed to delete service id {service_id}")
```

## 次の手順

上記のチュートリアルに従うことで、ONNXモデルのトレーニングとReal-time Machine Learningモデルストアへのアップロードに成功しました。 さらに、リアルタイム機械学習モデルにスコアを割り当て、導入しています。 モデルオーサリングに使用できるノードの詳細については、[ノードリファレンスガイド](./node-reference.md)を参照してください。
