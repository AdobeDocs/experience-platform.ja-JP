---
keywords: Experience Platform、開発者ガイド、SDK、データアクセス SDK、Data Science Workspace、人気の高いトピック
solution: Experience Platform
title: Adobe Experience Platform Platform SDK を使用したモデルオーサリング
description: このチュートリアルでは、Python と R の両方で data_access_sdk_python を新しい Python platform_sdk に変換する方法について説明します。
exl-id: 20909cae-5cd2-422b-8dbb-35bc63e69b2a
source-git-commit: 81f48de908b274d836f551bec5693de13c5edaf1
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 69%

---

# Adobe Experience Platformを使用したモデルオーサリング [!DNL Platform] SDK

このチュートリアルでは、Python と R の両方で `data_access_sdk_python` を新しい Python `platform_sdk` に変換する方法について説明します。このチュートリアルでは、次の操作について説明します。

- [認証の構築](#build-authentication)
- [データの基本読み取り](#basic-reading-of-data)
- [データの基本的な書き込み](#basic-writing-of-data)

## 認証の構築 {#build-authentication}

への呼び出しをおこなうには認証が必要です [!DNL Adobe Experience Platform]は、API キー、組織 ID、ユーザートークン、サービストークンで構成されます。

### Python

Jupyter ノートブックを使用している場合は、次のコードを使用して、`client_context` を構築してください。

```python
client_context = PLATFORM_SDK_CLIENT_CONTEXT
```

Jupyter ノートブックを使用していない場合、または組織を変更する必要がある場合は、次のコード例を使用してください。

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

Jupyter ノートブックを使用していない場合、または組織を変更する必要がある場合は、次のコード例を使用してください。

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

新しい [!DNL Platform] SDK の最大読み取りサイズは 32 GB で、最大読み取り時間は 10 分です。

読み取り時間が長すぎる場合は、次のいずれかのフィルターオプションを使用してみてください。

- [オフセットと制限によるデータのフィルター](#filter-by-offset-and-limit)
- [日付によるデータのフィルター](#filter-by-date)
- [列によるデータのフィルター](#filter-by-selected-columns)
- [並べ替え結果の取得](#get-sorted-results)

>[!NOTE]
>
>組織が `client_context`.

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

新しい [!DNL Platform] SDK は、次の操作をサポートします。

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
>組織が `client_context`.

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

`platform_sdk` データローダを設定すると、データは準備され、`train` データセットと `val` データセットに分割されます。データの準備と特徴量エンジニアリングについては、 ノートブックを使用したレシピの作成に関するチュートリアルで、「[データの準備と特徴量エンジニアリング](../jupyterlab/create-a-model.md#data-preparation-and-feature-engineering)」の節を参照してください。[!DNL JupyterLab]
