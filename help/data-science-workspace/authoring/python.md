---
keywords: Experience Platform；ホーム；人気のトピック；データアクセス；python sdk；データアクセス api;read python;write python
solution: Experience Platform
title: Data Science Workspace の Python を使用したデータへのアクセス
topic-legacy: tutorial
type: Tutorial
description: 次のドキュメントには、Python で Data Science Workspace で使用するデータにアクセスする方法の例が含まれています。
exl-id: 75aafd58-634a-4df3-a2f0-9311f93deae4
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '420'
ht-degree: 2%

---

# Data Science Workspace の Python を使用したデータへのアクセス

次のドキュメントには、Data Science Workspace で Python を使用してデータにアクセスする方法の例が含まれています。 JupyterLab ノートブックを使用したデータへのアクセスについては、 [JupyterLab ノートブックのデータアクセス](../jupyterlab/access-notebook-data.md) ドキュメント。

## データセットの読み取り

環境変数を設定し、インストールを完了すると、データセットを pandas データフレームに読み取れるようになります。

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

### パーティション情報を取得する：

```python
client_context = get_client_context(config_properties)

dataset = Dataset(client_context).get_by_id({DATASET_ID})
partitions = dataset.get_partitions_info()
```

### DISTINCT 句

DISTINCT 句を使用すると、行/列レベルですべてのユニーク値を取得し、応答からすべての重複値を削除できます。

の使用例 `distinct()` 関数は次のように表示されます。

```python
df = dataset_reader.select(['column-a']).distinct().read()
```

### WHERE 句

Python の特定の演算子を使用して、データセットをフィルタリングできます。

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

これらのフィルタリング関数の使用例を以下に示します。

```python
df = dataset_reader.where(experience_ds['timestamp'].gt(87879779797).And(experience_ds['timestamp'].lt(87879779797)).Or(experience_ds['a'].eq(123)))
```

### ORDER BY 句

ORDER BY 句を使用すると、受け取った結果を特定の順序（昇順または降順）で指定した列で並べ替えることができます。 これは、 `sort()` 関数に置き換えます。

の使用例 `sort()` 関数は次のように表示されます。

```python
df = dataset_reader.sort([('column_1', 'asc'), ('column_2', 'desc')])
```

### LIMIT 句

LIMIT 句を使用すると、データセットから受け取るレコードの数を制限できます。

の使用例 `limit()` 関数は次のように表示されます。

```python
df = dataset_reader.limit(100).read()
```

### OFFSET 句

OFFSET 句を使用すると、行を先頭からスキップし、後から行を返すようにできます。 LIMIT と組み合わせると、ブロック内の行を繰り返すのに使用できます。

の使用例 `offset()` 関数は次のように表示されます。

```python
df = dataset_reader.offset(100).read()
```

## データセットの書き込み

データセットに書き込むには、データセットに pandas データフレームを指定する必要があります。

### pandas データフレームの書き込み

```python
client_context = get_client_context(config_properties)

# To fetch existing dataset
dataset = Dataset(client_context).get_by_id({DATASET_ID})

dataset_writer = DatasetWriter(client_context, dataset)

write_tracker = dataset_writer.write(<your_dataFrame>, file_format='json')
```

## Userspace ディレクトリ（チェックポイント）

ジョブの実行時間を長くするには、中間ステップの保存が必要になる場合があります。 このような場合、ユーザースペースに対して読み書きを行うことができます。

>[!NOTE]
>
>データへのパスは **not** 保存済み。 対応するパスをそれぞれのデータに保存する必要があります。

### ユーザースペースに書き込み

```python
client_context = get_client_context(config_properties)
                               
user_helper = UserSpaceHelper(client_context)
user_helper.write(data_frame=<data_frame>, path=<path_to_directory>, ref_dataset_id=<ref_dataset_id>)
```

### ユーザースペースから読み取り

```python
client_context = get_client_context(config_properties)
                               
user_helper = UserSpaceHelper(client_context)
my_df = user_helper.read(path=<path_to_directory>, ref_dataset_id=<ref_dataset_id>)
```

## 次の手順

Adobe Experience Platform Data Science Workspace は、上記のコードサンプルを使用してデータの読み取りと書き込みをおこなうレシピサンプルを提供しています。 Python を使用してデータにアクセスする方法の詳細については、 [Data Science Workspace Python GitHub リポジトリ](https://github.com/adobe/experience-platform-dsw-reference/tree/master/recipes/python/retail).
