---
keywords: Experience Platform；開発者ガイド；SDK;Data Access SDK;Data Science Workspace；人気のトピック
solution: Experience Platform
title: Adobe Experience Platform SDKを使用したモデルのオーサリング
description: このチュートリアルでは、Python と R の両方で data_access_sdk_python を新しい Python platform_sdk に変換する方法について説明します。
exl-id: 20909cae-5cd2-422b-8dbb-35bc63e69b2a
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '506'
ht-degree: 58%

---

# Adobe [!DNL Experience Platform] SDKを使用したモデルのオーサリング

>[!NOTE]
>
>Data Science Workspaceは購入できなくなりました。
>
>このドキュメントは、Data Science Workspaceの以前の使用権限を持つ既存のお客様を対象としています。

このチュートリアルでは、Python と R の両方で `data_access_sdk_python` を新しい Python `platform_sdk` に変換する方法について説明します。このチュートリアルでは、次の操作について説明します。

- [認証の構築](#build-authentication)
- [データの基本読み取り](#basic-reading-of-data)
- [データの基本的な書き込み](#basic-writing-of-data)

## 認証の構築 {#build-authentication}

認証は [!DNL Adobe Experience Platform] への呼び出しに必要で、API キー、組織 ID、ユーザートークン、サービストークンで構成されています。

### Python

Jupyter ノートブックを使用している場合は、次のコードを使用して、`client_context` を構築してください。

```python
client_context = PLATFORM_SDK_CLIENT_CONTEXT
```

Jupyter Notebook を使用していない場合や、組織を変更する必要がある場合は、以下のコードサンプルを使用してください。

```python
from platform_sdk.client_context import ClientContext
client_context = ClientContext(api_key={API_KEY},
              org_id={ORG_ID},
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

Jupyter Notebook を使用していない場合や、組織を変更する必要がある場合は、次のコードサンプルを使用してください。

```r
library(reticulate)
use_python("/usr/local/bin/ipython")
psdk <- import("platform_sdk")
client_context <- psdk$client_context$ClientContext(api_key={API_KEY},
              org_id={ORG_ID},
              user_token={USER_TOKEN},
              service_token={SERVICE_TOKEN})
```

## データの基本読み取り {#basic-reading-of-data}

新しい [!DNL Experience Platform] SDKでは、最大の読み取りサイズは 32 GB で、最大の読み取り時間は 10 分です。

読み取り時間が長すぎる場合は、次のいずれかのフィルターオプションを使用してみてください。

- [オフセットと制限によるデータのフィルタリング](#filter-by-offset-and-limit)
- [日付によるデータのフィルタリング](#filter-by-date)
- [列によるデータのフィルタリング](#filter-by-selected-columns)
- [並べ替えられた結果を取得しています](#get-sorted-results)

>[!NOTE]
>
>組織は `client_context` 内で設定されます。

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

新しい [!DNL Experience Platform] SDKは、次の操作をサポートします。

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
>組織は `client_context` 内で設定されます。

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

`platform_sdk` データローダを設定すると、データは準備され、`train` データセットと `val` データセットに分割されます。データの準備と機能エンジニアリングについて詳しくは、[!DNL JupyterLab] ノートブックを使用したレシピの作成に関するチュートリアルの [ データの準備と機能エンジニアリング ](../jupyterlab/create-a-model.md#data-preparation-and-feature-engineering) の節を参照してください。
