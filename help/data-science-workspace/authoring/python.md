---
keywords: Experience Platform；ホーム；人気のあるトピック；データアクセス；Python sdk；データアクセスapi;Read Python;Write Python
solution: Experience Platform
title: Data Science WorkspaceでPythonを使用してデータにアクセスする
topic-legacy: tutorial
type: Tutorial
description: 次のドキュメントは、Data Science Workspaceで使用するPythonのデータにアクセスする方法の例を示しています。
exl-id: 75aafd58-634a-4df3-a2f0-9311f93deae4
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '420'
ht-degree: 2%

---

# Data Science WorkspaceのPythonを使用したデータへのアクセス

次のドキュメントは、Data Science Workspaceで使用するPythonを使用したデータへのアクセス方法の例を示しています。 JupterLabノートブックを使用したデータへのアクセスについては、[JupyterLabノートブックデータアクセス](../jupyterlab/access-notebook-data.md)のドキュメントを参照してください。

## データセットの読み取り

環境変数を設定し、インストールを完了した後、データセットをパンダのデータフレームに読み込めるようになります。

```python
import pandas as pd
from .utils import get_client_context
from platform_sdk.dataset_reader import DatasetReader

def load(config_properties):

client_context = get_client_context(config_properties)

dataset_reader = DatasetReader(client_context, config_properties['DATASET_ID'])

df = dataset_reader.read()
```

### データセットからSELECT列を

```python
df = dataset_reader.select(['column-a','column-b']).read()
```

### パーティション情報の取得：

```python
client_context = get_client_context(config_properties)

dataset = Dataset(client_context).get_by_id({DATASET_ID})
partitions = dataset.get_partitions_info()
```

### DISTINCT句

DISTINCT句を使用すると、行/列レベルですべての個別の値を取得し、応答からすべての重複値を削除できます。

`distinct()`関数の使用例を次に示します。

```python
df = dataset_reader.select(['column-a']).distinct().read()
```

### WHERE句

Pythonの特定の演算子を使用して、データセットをフィルタリングできます。

>[!NOTE]
>
>フィルタリングに使用される関数は、大文字と小文字が区別されます。

```python
eq() = '='
gt() = '>'
ge() = '>='
lt() = '<'
le() = '<='
And = and operator
Or = or operator
```

これらのフィルター機能の使用例を次に示します。

```python
df = dataset_reader.where(experience_ds['timestamp'].gt(87879779797).And(experience_ds['timestamp'].lt(87879779797)).Or(experience_ds['a'].eq(123)))
```

### ORDER BY句

ORDER BY句を使用すると、受け取った結果を、指定した列で特定の順序（昇順または降順）で並べ替えることができます。 これは`sort()`関数を使って行います。

`sort()`関数の使用例を次に示します。

```python
df = dataset_reader.sort([('column_1', 'asc'), ('column_2', 'desc')])
```

### LIMIT句

LIMIT句を使用すると、データセットから受け取るレコードの数を制限できます。

`limit()`関数の使用例を次に示します。

```python
df = dataset_reader.limit(100).read()
```

### OFFSET句

OFFSET句を使用すると、行を最初から開始にスキップし、後から行を返すことができます。 LIMITと組み合わせて使用すると、ブロック内の行を反復することができます。

`offset()`関数の使用例を次に示します。

```python
df = dataset_reader.offset(100).read()
```

## データセットの書き込み

データセットに書き込むには、パンダのデータフレームをデータセットに指定する必要があります。

### パンダのデータフレームの書き込み

```python
client_context = get_client_context(config_properties)

# To fetch existing dataset
dataset = Dataset(client_context).get_by_id({DATASET_ID})

dataset_writer = DatasetWriter(client_context, dataset)

write_tracker = dataset_writer.write(<your_dataFrame>, file_format='json')
```

## Userspaceディレクトリ（チェックポイント）

より長い時間実行するジョブの場合は、中間ステップの格納が必要になる場合があります。 このような場合、ユーザ空間に対して読み書きが可能です。

>[!NOTE]
>
>データへのパスは&#x200B;****&#x200B;保存されません。 対応するデータのパスを保存する必要があります。

### ユーザースペースへの書き込み

```python
client_context = get_client_context(config_properties)
                               
user_helper = UserSpaceHelper(client_context)
user_helper.write(data_frame=<data_frame>, path=<path_to_directory>, ref_dataset_id=<ref_dataset_id>)
```

### ユーザ空間から読み取る

```python
client_context = get_client_context(config_properties)
                               
user_helper = UserSpaceHelper(client_context)
my_df = user_helper.read(path=<path_to_directory>, ref_dataset_id=<ref_dataset_id>)
```

## 次の手順

Adobe Experience Platformデータサイエンスワークスペースでは、上記のコードサンプルを使用してデータの読み取りと書き込みを行うレシピサンプルを提供しています。 Pythonを使用してデータにアクセスする方法の詳細については、[Data Science Workspace Python GitHub Repository](https://github.com/adobe/experience-platform-dsw-reference/tree/master/recipes/python/retail)を参照してください。
