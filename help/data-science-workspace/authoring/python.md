---
keywords: Experience Platform；ホーム；人気のトピック；data access;python sdk;data access api;python の読み取り；python の書き込み
solution: Experience Platform
title: Data Science Workspaceで Python を使用してデータにアクセスする
type: Tutorial
description: 次のドキュメントでは、Data Science Workspaceで使用する Python のデータにアクセスする方法の例を示します。
exl-id: 75aafd58-634a-4df3-a2f0-9311f93deae4
source-git-commit: 5d98dc0cbfaf3d17c909464311a33a03ea77f237
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 0%

---

# Data Science Workspaceでの Python を使用したデータへのアクセス

>[!NOTE]
>
>Data Science Workspaceは購入できなくなりました。
>
>このドキュメントは、Data Science Workspaceの以前の使用権限を持つ既存のお客様を対象としています。

次のドキュメントでは、Data Science Workspaceで使用する Python を使用してデータにアクセスする方法の例を示します。 JupyterLab ノートブックを使用したデータへのアクセスについて詳しくは、[JupyterLab ノートブックのデータアクセス &#x200B;](../jupyterlab/access-notebook-data.md) のドキュメントを参照してください。

## データセットの読み取り

環境変数を設定し、インストールを完了すると、データセットを pandas データフレームに読み込むことができるようになります。

```python
import pandas as pd
from .utils import get_client_context
from platform_sdk.dataset_reader import DatasetReader

def load(config_properties):

client_context = get_client_context(config_properties)

dataset_reader = DatasetReader(client_context, config_properties['DATASET_ID'])

df = dataset_reader.read()
```

### データセットから列を選択

```python
df = dataset_reader.select(['column-a','column-b']).read()
```

### パーティション情報の取得：

```python
client_context = get_client_context(config_properties)

dataset = Dataset(client_context).get_by_id({DATASET_ID})
partitions = dataset.get_partitions_info()
```

### DISTINCT 句

DISTINCT 句を使用すると、行/列レベルでユニークな値をすべて取得し、応答から重複する値をすべて削除できます。

`distinct()` 関数の使用例を次に示します。

```python
df = dataset_reader.select(['column-a']).distinct().read()
```

### WHERE 句

Python では、データセットのフィルタリングに役立つ特定の演算子を使用できます。

>[!NOTE]
>
>フィルタリングに使用する関数では、大文字と小文字が区別されます。

```python
eq() = '='
gt() = '>'
ge() = '>='
lt() = '<'
le() = '<='
And = and operator
Or = or operator
```

これらのフィルター関数の使用例を次に示します。

```python
df = dataset_reader.where(experience_ds['timestamp'].gt(87879779797).And(experience_ds['timestamp'].lt(87879779797)).Or(experience_ds['a'].eq(123)))
```

### ORDER BY 句

ORDER BY 句を使用すると、指定した列を指定した順序（昇順または降順）で並べ替えて、受信した結果を表示できます。 これを行うには、`sort()` 関数を使用します。

`sort()` 関数の使用例を次に示します。

```python
df = dataset_reader.sort([('column_1', 'asc'), ('column_2', 'desc')])
```

### LIMIT 句

LIMIT 句を使用すると、データセットから受信するレコードの数を制限できます。

`limit()` 関数の使用例を次に示します。

```python
df = dataset_reader.limit(100).read()
```

### OFFSET 句

OFFSET 句を使用すると、先頭から行をスキップして、後の時点から行を返すことができます。 LIMIT と組み合わせて、これを使用してブロック内の行を繰り返すことができます。

`offset()` 関数の使用例を次に示します。

```python
df = dataset_reader.offset(100).read()
```

## データセットの記述

データセットに書き込むには、データセットに pandas データフレームを指定する必要があります。

### Pandas データフレームの作成

```python
client_context = get_client_context(config_properties)

# To fetch existing dataset
dataset = Dataset(client_context).get_by_id({DATASET_ID})

dataset_writer = DatasetWriter(client_context, dataset)

write_tracker = dataset_writer.write(<your_dataFrame>, file_format='json')
```

## ユーザー空間ディレクトリ （チェックポイント）

ジョブを長時間実行する場合は、中間ステップの保存が必要になる場合があります。 このような場合、ユーザー空間に対して読み書きを行うことができます。

>[!NOTE]
>
>データへのパスは保存 **されません**。 対応するパスをそれぞれのデータに保存する必要があります。

### Userspace に書き込む

```python
client_context = get_client_context(config_properties)
                               
user_helper = UserSpaceHelper(client_context)
user_helper.write(data_frame=<data_frame>, path=<path_to_directory>, ref_dataset_id=<ref_dataset_id>)
```

### ユーザー空間から読み取り

```python
client_context = get_client_context(config_properties)
                               
user_helper = UserSpaceHelper(client_context)
my_df = user_helper.read(path=<path_to_directory>, ref_dataset_id=<ref_dataset_id>)
```

## 次の手順

Adobe Experience Platform Data Science Workspaceは、上記のコードサンプルを使用してデータの読み取りと書き込みを行うレシピサンプルを提供します。 Python を使用してデータにアクセスする方法について詳しくは、[Data Science Workspace Python GitHub リポジトリ &#x200B;](https://github.com/adobe/experience-platform-dsw-reference/tree/master/recipes/python/retail) を参照してください。
