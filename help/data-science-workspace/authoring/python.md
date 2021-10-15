---
keywords: Experience Platform；ホーム；人気のあるトピック；データアクセス；python sdk；データアクセス api；読み取り python；書き込み python
solution: Experience Platform
title: Data Science Workspace で Python を使用したデータへのアクセス
topic-legacy: tutorial
type: Tutorial
description: 次のドキュメントには、Python で Data Science Workspace で使用するデータにアクセスする方法の例が含まれています。
exl-id: 75aafd58-634a-4df3-a2f0-9311f93deae4
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '420'
ht-degree: 2%

---

# Data Science Workspace で Python を使用したデータへのアクセス

次のドキュメントには、Python を使用してデータにアクセスし、Data Science Workspace で使用する方法の例が含まれています。 JupyterLab ノートブックを使用したデータへのアクセスについては、 [JupyterLab ノートブックデータアクセス ](../jupyterlab/access-notebook-data.md) のドキュメントを参照してください。

## データセットの読み取り

環境変数を設定してインストールを完了すると、データセットを pandas データフレームに読み取れるようになります。

```python
import pandas as pd
from .utils import get_client_context
from platform_sdk.dataset_reader import DatasetReader

def load(config_properties):

client_context = get_client_context(config_properties)

dataset_reader = DatasetReader(client_context, config_properties['DATASET_ID'])

df = dataset_reader.read()
```

### データセットからの SELECT 列

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

DISTINCT 句を使用すると、行/列レベルですべてのユニーク値をフェッチし、重複する値をすべてレスポンスから削除できます。

`distinct()` 関数の使用例を次に示します。

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

ORDER BY 句を使用すると、受け取った結果を特定の列で特定の順序（昇順または降順）で並べ替えることができます。 これは `sort()` 関数を使って行います。

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

OFFSET 句を使用すると、行を最初からスキップして、後の時点から行を戻すことができます。 LIMIT と組み合わせると、ブロック内の行を繰り返し処理するのに使用できます。

`offset()` 関数の使用例を次に示します。

```python
df = dataset_reader.offset(100).read()
```

## データセットの書き込み

データセットに書き込むには、pandas データフレームをデータセットに指定する必要があります。

### pandas データフレームの書き込み

```python
client_context = get_client_context(config_properties)

# To fetch existing dataset
dataset = Dataset(client_context).get_by_id({DATASET_ID})

dataset_writer = DatasetWriter(client_context, dataset)

write_tracker = dataset_writer.write(<your_dataFrame>, file_format='json')
```

## ユーザー領域ディレクトリ（チェックポインティング）

実行時間が長いジョブの場合は、中間ステップを保存する必要が生じる場合があります。 このような場合は、ユーザースペースに対して読み書きを行うことができます。

>[!NOTE]
>
>データへのパスは **保存されません**。 対応するパスをそれぞれのデータに保存する必要があります。

### ユーザー領域への書き込み

```python
client_context = get_client_context(config_properties)
                               
user_helper = UserSpaceHelper(client_context)
user_helper.write(data_frame=<data_frame>, path=<path_to_directory>, ref_dataset_id=<ref_dataset_id>)
```

### ユーザー空間から読み取る

```python
client_context = get_client_context(config_properties)
                               
user_helper = UserSpaceHelper(client_context)
my_df = user_helper.read(path=<path_to_directory>, ref_dataset_id=<ref_dataset_id>)
```

## 次の手順

Adobe Experience Platform Data Science Workspace は、上記のコードサンプルを使用してデータの読み取りと書き込みをおこなうレシピサンプルを提供しています。 Python を使用してデータにアクセスする方法の詳細については、[Data Science Workspace Python GitHub リポジトリ ](https://github.com/adobe/experience-platform-dsw-reference/tree/master/recipes/python/retail) を参照してください。
