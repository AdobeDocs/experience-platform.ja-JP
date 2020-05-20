---
keywords: Experience Platform;developer guide;SDK;Data Access SDK;Data Science Workspace;popular topics
solution: Experience Platform
title: プラットフォームSDKガイド
topic: SDK authoring
translation-type: tm+mt
source-git-commit: 19823c7cf0459e045366f0baae2bd8a98416154c
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 5%

---


# プラットフォームSDKガイド

このチュートリアルでは、PythonとRの両方で新しいPython `data_access_sdk_python` に変換する方法 `platform_sdk` について説明します。このチュートリアルでは、次の操作に関する情報を提供します。

- [認証の構築](#build-authentication)
- [データの基本的な読み取り](#basic-reading-of-data)
- [データの基本的な記述](#basic-writing-of-data)

## 認証の構築 {#build-authentication}

認証は、Adobe Experience Platformの呼び出しを行うために必要で、APIキー、IMS組織ID、ユーザートークンおよびサービストークンで構成されます。

### Python

Jupyterノートブックを使用している場合は、次のコードを使用してを構築してください `client_context`。

```python
client_context = PLATFORM_SDK_CLIENT_CONTEXT
```

Jupyterノートブックを使用していない場合、またはIMS組織を変更する必要がある場合は、以下のコード例を使用してください。

```python
from platform_sdk.client_context import ClientContext
client_context = ClientContext(api_key={API_KEY},
              org_id={IMS_ORG},
              user_token={USER_TOKEN},
              service_token={SERVICE_TOKEN})
```

### R

Jupyterノートブックを使用している場合は、次のコードを使用してを構築してください `client_context`。

```r
library(reticulate)
use_python("/usr/local/bin/ipython")
psdk <- import("platform_sdk")

py_run_file("../.ipython/profile_default/startup/platform_sdk_context.py")
client_context <- py$PLATFORM_SDK_CLIENT_CONTEXT
```

Jupyterノートブックを使用していない場合、またはIMS組織を変更する必要がある場合は、以下のコード例を使用してください。

```r
library(reticulate)
use_python("/usr/local/bin/ipython")
psdk <- import("platform_sdk")
client_context <- psdk$client_context$ClientContext(api_key={API_KEY},
              org_id={IMS_ORG},
              user_token={USER_TOKEN},
              service_token={SERVICE_TOKEN})
```

## データの基本的な読み取り {#basic-reading-of-data}

新しいプラットフォームSDKでは、最大読み取りサイズは32 GBで、最大読み取り時間は10分です。

読み取り時間が長すぎる場合は、次のいずれかのフィルターオプションを使用してみてください。

- [オフセットと制限によるデータのフィルタリング](#filter-by-offset-and-limit)
- [日付によるデータのフィルタリング](#filter-by-date)
- [列によるデータのフィルタ](#filter-by-selected-columns)
- [並べ替え結果の取得](#get-sorted-results)

>[!NOTE] IMS組織は、内で設定され `client_context`ます。

### Python

Pythonでデータを読み込むには、次のコードサンプルを使用してください。

```python
from platform_sdk.dataset_reader import DatasetReader
dataset_reader = DatasetReader(client_context, "{DATASET_ID}")
df = dataset_reader.limit(100).read()
df.head()
```

### R

Rのデータを読み取るには、次のコードサンプルを使用してください。

```r
DatasetReader <- psdk$dataset_reader$DatasetReader
dataset_reader <- DatasetReader(client_context, "{DATASET_ID}") 
df <- dataset_reader$read() 
df
```

## オフセットと制限でフィルタ {#filter-by-offset-and-limit}

バッチIDによるフィルタリングはサポートされなくなったので、データの読み取りをスコープ化するには、 `offset` およびを使用する必要があり `limit`ます。

### Python

```python
df = dataset_reader.limit(100).offset(1).read()
df.head
```

### R

```r
df <- dataset_reader$limit(100L)$offset(1L)$read() 
df
```

## 日付でフィルター {#filter-by-date}

日付フィルターの精度は、日別に設定されるのではなく、タイムスタンプによって定義されるようになりました。

### Python

```python
df = dataset_reader.where(\
    dataset_reader['timestamp'].gt('2019-04-10 15:00:00').\
    And(dataset_reader['timestamp'].lt('2019-04-10 17:00:00'))\
).read()
df.head()
```

### R

```r
df2 <- dataset_reader$where(
    dataset_reader['timestamp']$gt('2018-12-10 15:00:00')$
    And(dataset_reader['timestamp']$lt('2019-04-10 17:00:00'))
)$read()
df2
```

新しいプラットフォームSDKは、次の操作をサポートします。

| 操作 | 関数 |
| --------- | -------- |
| 次と等しい (`=`) | `eq()` |
| より大きい (`>`) | `gt()` |
| 次よりも大きいか等しい (`>=`) | `ge()` |
| より小さい (`<`) | `lt()` |
| 次よりも小さいか等しい (`<=`) | `le()` |
| And (`&`) | `And()` |
| または (`|`) | `Or()` |

## 選択した列でフィルタ {#filter-by-selected-columns}

データの読み取りをさらに絞り込むために、列名でフィルターすることもできます。

### Python

```python
df = dataset_reader.select(['column-a','column-b']).read()
```

### R

```r
df <- dataset_reader$select(c('column-a','column-b'))$read() 
```

## 並べ替え結果の取得 {#get-sorted-results}

受け取った結果は、ターゲットデータセットの指定した列と、その順序(asc/desc)で並べ替えることができます。

次の例では、データフレームが「column-a」で昇順で並べ替えられています。 次に、「column-a」に同じ値を持つ行を「column-b」で降順に並べ替えます。

### Python

```python
df = dataset_reader.sort([('column-a', 'asc'), ('column-b', 'desc')])
```

### R

```r
df <- dataset_reader$sort(c(('column-a', 'asc'), ('column-b', 'desc')))$read()
```

## データの基本的な記述 {#basic-writing-of-data}

>[!NOTE] IMS組織は、内で設定され `client_context`ます。

PythonとRでデータを書き込むには、次の例のいずれかを使用します。

### Python

```python
from platform_sdk.models import Dataset
from platform_sdk.dataset_writer import DatasetWriter

dataset = Dataset(client_context).get_by_id("{DATASET_ID}")
dataset_writer = DatasetWriter(client_context, dataset)
write_tracker = dataset_writer.write({PANDA_DATAFRAME}, file_format='json')
```

### R

```r
dataset <- psdk$models$Dataset(client_context)$get_by_id("{DATASET_ID}")
dataset_writer <- psdk$dataset_writer$DatasetWriter(client_context, dataset)
write_tracker <- dataset_writer$write({PANDA_DATAFRAME}, file_format='json')
```

## 次の手順

データローダーを設定すると、 `platform_sdk` データは準備が完了し、データセットとデータセットに分割され `train` ま `val` す。 データの準備と機能のエンジニアリングについては、JupyterLabノートブックを使用したレシピの作成に関するチュートリアルの [データ準備と機能のエンジニアリングに関する節を参照してください](../jupyterlab/create-a-recipe.md#data-preparation-and-feature-engineering) 。