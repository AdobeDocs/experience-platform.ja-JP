---
keywords: Experience Platform;developer guide;SDK;Data Access SDK;Data Science Workspace;popular topics
solution: Experience Platform
title: Platform SDK ガイド
topic: SDK authoring
description: このチュートリアルでは、PythonとRの両方でdata_access_sdk_pythonを新しいPythonプラットフォームSdkに変換する方法について説明します。
translation-type: tm+mt
source-git-commit: 7615476c4b728b451638f51cfaa8e8f3b432d659
workflow-type: tm+mt
source-wordcount: '470'
ht-degree: 84%

---


# [!DNL Platform] SDKガイド

このチュートリアルでは、Python と R の両方で `data_access_sdk_python` を新しい Python `platform_sdk` に変換する方法について説明します。このチュートリアルでは、次の操作について説明します。

- [認証の構築](#build-authentication)
- [データの基本読み取り](#basic-reading-of-data)
- [データの基本書き込み](#basic-writing-of-data)

## 認証の構築 {#build-authentication}

Authentication is required to make calls to [!DNL Adobe Experience Platform], and is comprised of API Key, IMS Org ID, a user token, and a service token.

### Python

Jupyter ノートブックを使用している場合は、次のコードを使用して、`client_context` を構築してください。

```python
client_context = PLATFORM_SDK_CLIENT_CONTEXT
```

Jupyter ノートブックを使用していない場合、または IMS 組織を変更する必要がある場合は、次のコード例を使用してください。

```python
from platform_sdk.client_context import ClientContext
client_context = ClientContext(api_key={API_KEY},
              org_id={IMS_ORG},
              user_token={USER_TOKEN},
              service_token={SERVICE_TOKEN})
```

### R

Jupyter ノートブックを使用している場合は、次のコードを使用して、`client_context` を構築してください。

```r
library(reticulate)
use_python("/usr/local/bin/ipython")
psdk <- import("platform_sdk")

py_run_file("../.ipython/profile_default/startup/platform_sdk_context.py")
client_context <- py$PLATFORM_SDK_CLIENT_CONTEXT
```

Jupyter ノートブックを使用していない場合、または IMS 組織を変更する必要がある場合は、次のコード例を使用してください。

```r
library(reticulate)
use_python("/usr/local/bin/ipython")
psdk <- import("platform_sdk")
client_context <- psdk$client_context$ClientContext(api_key={API_KEY},
              org_id={IMS_ORG},
              user_token={USER_TOKEN},
              service_token={SERVICE_TOKEN})
```

## データの基本読み取り {#basic-reading-of-data}

With the new [!DNL Platform] SDK, the maximum read size is 32 GB, with a maximum read time of 10 minutes.

読み取り時間が長すぎる場合は、次のいずれかのフィルターオプションを使用してみてください。

- [オフセットと制限によるデータのフィルター](#filter-by-offset-and-limit)
- [日付によるデータのフィルター](#filter-by-date)
- [列によるデータのフィルター](#filter-by-selected-columns)
- [並べ替え結果の取得](#get-sorted-results)

>[!NOTE]
>
> IMS 組織は、`client_context` 内で設定されます 。

### Python

Python でデータを読み込むには、以下のコード例を使用してください。

```python
from platform_sdk.dataset_reader import DatasetReader
dataset_reader = DatasetReader(client_context, "{DATASET_ID}")
df = dataset_reader.limit(100).read()
df.head()
```

### R

R でデータを読み取るには、以下のコード例を使用してください。

```r
DatasetReader <- psdk$dataset_reader$DatasetReader
dataset_reader <- DatasetReader(client_context, "{DATASET_ID}") 
df <- dataset_reader$read() 
df
```

## オフセットと制限によるフィルター {#filter-by-offset-and-limit}

バッチ ID によるフィルタリングはサポートされなくなったので、データの読み取りの範囲を絞るには、`offset` と `limit` を使用する必要があります。

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

## 日付によるフィルター {#filter-by-date}

日付フィルターの精度が、日別に設定されるのではなく、タイムスタンプによって定義されるようになりました。

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

The new [!DNL Platform] SDK supports the following operations:

| 操作 | 関数 |
| --------- | -------- |
| 次と等しい（`=`） | `eq()` |
| より大きい（`>`） | `gt()` |
| 次よりも大きいか等しい（`>=`） | `ge()` |
| より小さい（`<`） | `lt()` |
| 次よりも小さいか等しい（`<=`） | `le()` |
| および（`&`） | `And()` |
| または（`|`） | `Or()` |

## 選択した列によるフィルター {#filter-by-selected-columns}

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

受け取った結果は、それぞれ、ターゲットデータセットの指定した列と、その順序（昇順または降順）で並べ替えることができます。

次の例では、データフレームが「column-a」で昇順に並べ替えられています。次に、「column-a」に同じ値を持つ行は「column-b」で降順に並べ替えられます。

### Python

```python
df = dataset_reader.sort([('column-a', 'asc'), ('column-b', 'desc')])
```

### R

```r
df <- dataset_reader$sort(c(('column-a', 'asc'), ('column-b', 'desc')))$read()
```

## データの基本的な書き込み {#basic-writing-of-data}

>[!NOTE]
>
> IMS 組織は、`client_context` 内で設定されます 。

Python と R でデータを書き込むには、次の例を使用します。

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

`platform_sdk` データローダを設定すると、データは準備され、`train` データセットと `val` データセットに分割されます。データの準備と特徴量エンジニアリングについては、 ノートブックを使用したレシピの作成に関するチュートリアルで、「[データの準備と特徴量エンジニアリング](../jupyterlab/create-a-recipe.md#data-preparation-and-feature-engineering)」の節を参照してください。[!DNL JupyterLab]