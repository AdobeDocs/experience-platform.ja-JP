---
keywords: Experience Platform;JupyterLab;notebooks;Data Science Workspace;popular topics;%dataset;interactive mode;batch mode;Spark sdk;python sdk;access data;notebook data access
solution: Experience Platform
title: Jupyterlabノートブックでのデータアクセス
topic: Developer Guide
description: このガイドでは、Data Science Workspaceに組み込まれたJupyterノートブックを使用してデータにアクセスする方法に焦点を当てます。
translation-type: tm+mt
source-git-commit: 645a0f595d268fb08ebfa5652f77a4b1628afcd3
workflow-type: tm+mt
source-wordcount: '3078'
ht-degree: 25%

---


# ノートブックのデータアクセス [!DNL Jupyterlab]

サポートされる各カーネルは、ノートブック内のデータセットから Platform データを読み取るための組み込み機能を備えてます。現在、JupyterLab(Adobe Experience Platform・データ・サイエンス・ワークスペース)は、 [!DNL Python]、R、PySpark、Scala用のノートブックをサポートしています。 However, support for paginating data is limited to [!DNL Python] and R notebooks. このガイドでは、JupyterLabノートブックを使用してデータにアクセスする方法について説明します。

## はじめに

このガイドを読む前に、 [[!DNL JupyterLab] ユーザーガイドを参照し](./overview.md) 、Data Science Workspace内でのの概要とその役割を確認して [!DNL JupyterLab] ください。

## ノートブックデータの制限 {#notebook-data-limits}

>[!IMPORTANT]
>
>PySparkとScalaノートブックの場合は、&quot;Remote RPC client disassociated&quot;という理由でエラーが発生しています。 これは、通常、ドライバまたはエクゼキュータのメモリ不足を意味します。 このエラーを解決するには、 [「バッチ」モードに切り替えてみてください](#mode) 。

次の情報は、読み取り可能な最大データ量、使用されたデータの種類、およびデータ読み取りにかかる予測時間枠を定義します。

お [!DNL Python] よびRの場合、40 GBのRAMで構成されたノートブックサーバがベンチマークに使用されました。 PySparkとScalaの場合、64GBのRAM、8コア、2 DBUで構成され、最大4人のワーカーを持つデータバリッククラスタが、以下のベンチマークに使用されました。

使用されるExperienceEventスキーマデータのサイズは、1,000行（1,000行）から10億行（1B行）まで様々です。 PySparkと [!DNL Spark] 指標の場合、XDMデータには10日の期間が使われていました。

アドホックスキーマデータは、「テーブルを選択として [!DNL Query Service] 作成(CTAS)」を使用して事前に処理されました。 また、1000行(1K)から10億行(1B)まで様々なサイズがあります。

### バッチモードとインタラクティブモードをいつ使用するか {#mode}

PySparkやScalaのノートブックでデータセットを読み込む場合、データセットを読み込む際に、インタラクティブモードまたはバッチモードを使用できます。 バッチモードは大きなデータセット用ですが、インタラクティブモードは高速な結果を得るために作成されます。

- PySparkとScalaのノートブックでは、500万行以上のデータを読み込む場合は、batchモードを使用する必要があります。 各モードの効率性の詳細については、以下のPySpark [または](#pyspark-data-limits) Scala [](#scala-data-limits) data limitの表を参照してください。

### [!DNL Python] ノート型データの制限

**XDM ExperienceEventスキーマ:** 22分以内に、最大200万行（ディスク上の6.1 GBのデータ）のXDMデータを読み取れるはずです。 行を追加すると、エラーが発生する場合があります。

| 行数 | 1K | 10K | 100K | 1M | 2M |
| ----------------------- | ------ | ------ | ----- | ----- | ----- |
| ディスク上のサイズ(MB) | 18.73 | 187.5 | 308 | 3000 | 6050 |
| SDK （秒） | 20.3 | 86.8 | 63 | 659 | 1315 |

**アドホックスキーマ:** 非XDM（アドホック）データの最大500万行（ディスク上の5.6 GBのデータ）を14分未満で読み取ることができます。 行を追加すると、エラーが発生する場合があります。

| 行数 | 1K | 10K | 100K | 1M | 2M | 3M | 5M |
| ----------------------- | ------- | ------- | ----- | ----- | ----- | ----- | ------ |
| ディスク上のサイズ(MB) | 1.21 | 11.72 | 115 | 1120 | 2250 | 3380 | 5630 |
| SDK （秒） | 7.27 | 9.04 | 27.3 | 180 | 346 | 487 | 819 |

### Rノートブックデータの制限

**XDM ExperienceEventスキーマ:** 13分以内に、最大100万行のXDMデータ(3GBのデータをディスクに読み込むことができます。

| 行数 | 1K | 10K | 100K | 1M |
| ----------------------- | ------ | ------ | ----- | ----- |
| ディスク上のサイズ(MB) | 18.73 | 187.5 | 308 | 3000 |
| Rカーネル（秒） | 14.03 | 69.6 | 86.8 | 775 |

**アドホックスキーマ:** 約10分で、アドホックデータ（ディスク上のデータは293 MB）の最大行数300万行を読み取ることができます。

| 行数 | 1K | 10K | 100K | 1M | 2M | 3M |
| ----------------------- | ------- | ------- | ----- | ----- | ----- | ----- |
| ディスク上のサイズ(MB) | 0.082 | 0.612 | 9.0 | 91 | 188 | 293 |
| R SDK （イン秒） | 7.7 | 4.58 | 35.9 | 233 | 470.5 | 603 |

### PySpark ([!DNL Python] カーネル)ノートブックのデータ制限： {#pyspark-data-limits}

**XDM ExperienceEventスキーマ:** インタラクティブモードでは、約20分で最大500万行（ディスク上の13.42 GBのデータ）のXDMデータを読み取れるはずです。 インタラクティブモードでは、最大500万行までのみサポートされます。 より大きなデータセットを読み取る場合は、バッチモードに切り替えることをお勧めします。 バッチモードでは、約14時間で最大5億行（ディスク上のデータは最大1.31 TB）のXDMデータを読み取れるはずです。

| 行数 | 1K | 10K | 100K | 1M | 2M | 3M | 5M | 10M | 50M | 100M | 500M |
|-------------------------|--------|--------|-------|-------|-------|-------|---------|---------|----------|--------|--------|
| ディスク上のサイズ | 2.93MB | 4.38MB | 29.02 | 2.69 GB | 5.39 GB | 8.09 GB | 13.42 GB | 26.82 GB | 134.24 GB | 268.39 GB | 1.31TB |
| SDK（インタラクティブモード） | 33s | 32.4s | 55.1s | 253.5s | 489.2s | 729.6s | 1206.8s | - | - | - | - |
| SDK（バッチモード） | 815.8s | 492.8s | 379.1s | 637.4s | 624.5s | 869.2s | 1104.1s | 1786s | 5387.2s | 10624.6s | 50547s |

**アドホックスキーマ:** Interactiveモードでは、XDM以外のデータを最大500万行（ディスク上の最大5.36 GBのデータ）まで、3分以内に読み取れるはずです。 バッチモードでは、約18分で最大10億行（ディスク上のデータは最大1.05 TB）の非XDMデータを読み取れるはずです。

| 行数 | 1K | 10K | 100K | 1M | 2M | 3M | 5M | 10M | 50M | 100M | 500M | 1B |
|--------------|--------|---------|---------|-------|-------|-------|--------|--------|---------|--------|---------|-------|
| ディスク上のサイズ | 1.12MB | 11.24MB | 109.48MB | 2.69 GB | 2.14 GB | 3.21 GB | 5.36 GB | 10.71 GB | 53.58 GB | 107.52 GB | 535.88 GB | 1.05TB |
| SDKインタラクティブモード（秒） | 28.2s | 18.6s | 20.8s | 20.9s | 23.8s | 21.7s | 24.7s | - | - | - | - | - |
| SDKバッチモード（秒） | 428.8s | 578.8s | 641.4s | 538.5s | 630.9s | 467.3s | 411s | 675s | 702s | 719.2s | 1022.1s | 1122.3s |

### [!DNL Spark] （Scalaカーネル）ノートブックデータの制限： {#scala-data-limits}

**XDM ExperienceEventスキーマ:** インタラクティブモードでは、約18分で最大500万行（ディスク上の13.42 GBのデータ）のXDMデータを読み取れるはずです。 インタラクティブモードでは、最大500万行までのみサポートされます。 より大きなデータセットを読み取る場合は、バッチモードに切り替えることをお勧めします。 バッチモードでは、約14時間で最大5億行（ディスク上のデータは最大1.31 TB）のXDMデータを読み取れるはずです。

| 行数 | 1K | 10K | 100K | 1M | 2M | 3M | 5M | 10M | 50M | 100M | 500M |
|---------------|--------|--------|-------|-------|-------|-------|---------|---------|----------|--------|--------|
| ディスク上のサイズ | 2.93MB | 4.38MB | 29.02 | 2.69 GB | 5.39 GB | 8.09 GB | 13.42 GB | 26.82 GB | 134.24 GB | 268.39 GB | 1.31TB |
| SDKインタラクティブモード（秒） | 37.9s | 22.7s | 45.6s | 231.7s | 444.7s | 660.6s | 1100s | - | - | - | - |
| SDKバッチモード（秒） | 374.4s | 398.5s | 527s | 487.9s | 588.9s | 829s | 939.1s | 1441s | 5473.2s | 10118.8 | 49207.6 |

**アドホックスキーマ:** インタラクティブモードでは、XDM以外のデータを最大500万行（ディスク上のデータは最大5.36 GB）読み取ることができます。読み取りは3分以内で行う必要があります。 バッチモードでは、約16分で最大10億行（ディスク上の最大1.05 TBのデータ）の非XDMデータを読み取れるはずです。

| 行数 | 1K | 10K | 100K | 1M | 2M | 3M | 5M | 10M | 50M | 100M | 500M | 1B |
|--------------|--------|---------|---------|-------|-------|-------|---------|---------|---------|--------|---------|-------|
| ディスク上のサイズ | 1.12MB | 11.24MB | 109.48MB | 2.69 GB | 2.14 GB | 3.21 GB | 5.36 GB | 10.71 GB | 53.58 GB | 107.52 GB | 535.88 GB | 1.05TB |
| SDKインタラクティブモード（秒） | 35.7s | 31s | 19.5s | 25.3s | 23s | 33.2s | 25.5s | - | - | - | - | - |
| SDKバッチモード（秒） | 448.8s | 459.7s | 519s | 475.8s | 599.9s | 347.6s | 407.8s | 397s | 518.8s | 487.9s | 760.2s | 975.4s |

## Pythonノートブック {#python-notebook}

[!DNL Python] ノートブックでは、データセットにアクセスする際にデータをページネーションできます。 ページ番号付けの有無に関わらずデータを読み取るコード例を以下に示します。使用可能なスターターPythonノートブックの詳細については、JupyterLabユーザーガイド内の [[!DNL JupyterLab] Launcher](./overview.md#launcher) セクションを参照してください。

以下のPythonドキュメントでは、次の概念を概説しています。

- [データセットからの読み取り](#python-read-dataset)
- [データセットへの書き込み](#write-python)
- [クエリデータ](#query-data-python)
- [ExperienceEventデータのフィルター](#python-filter)

### Read from a dataset in Python {#python-read-dataset}

**ページ番号なし：**

次のコードを実行すると、データセット全体が読み取られます。実行が成功した場合、データは `df` 変数で参照される Pandas データフレームとして保存されます。

```python
# Python

from platform_sdk.dataset_reader import DatasetReader
dataset_reader = DatasetReader(get_platform_sdk_client_context(), dataset_id="{DATASET_ID}")
df = dataset_reader.read()
df.head()
```

**ページ番号付き：**

次のコードを実行すると、指定したデータセットからデータが読み取られます。ページ番号付けは、`limit()` 関数を使用してデータを制限し、`offset()` 関数を使用してデータをオフセットすることで実現します。データの制限とは、読み取るデータポイントの最大数を指し、オフセットとは、データの読み取り前にスキップするデータポイントの数を指します。読み取り操作が正常に実行された場合、データは `df` 変数が参照する Pandas データフレームとして保存されます。

```python
# Python

from platform_sdk.dataset_reader import DatasetReader

dataset_reader = DatasetReader(get_platform_sdk_client_context(), dataset_id="{DATASET_ID}")
df = dataset_reader.limit(100).offset(10).read()
```

### Pythonでのデータセットへの書き込み {#write-python}

JupyterLabノートブックのデータセットに書き込むには、JupterLabの左側のナビゲーションで[Data]アイコンタブ（下のハイライト表示）を選択します。 「 **[!UICONTROL Datasets]** 」ディレクトリと「 **[!UICONTROL スキーマ]** 」ディレクトリが表示されます。 「 **[!UICONTROL Datasets]** 」を選択して右クリックし、使用するデータセットのドロップダウンメニューから「 **[!UICONTROL Write Data in Notebook]** 」オプションを選択します。 ノートブックの下部に実行可能コードのエントリが表示されます。

![](../images/jupyterlab/data-access/write-dataset.png)

- 「 **[!UICONTROL Write Data in Notebook]** 」を使用して、選択したデータセットを使用して書き込みセルを生成します。
- 「 **[!UICONTROL Explore Data in Notebook]** 」を使用して、選択したデータセットを使用して読み取りセルを生成します。
- 「ノートブックの **[!UICONTROL クエリデータ]** 」を使用して、選択したデータセットの基本的なクエリセルを生成します。

または、次のコードセルをコピーして貼り付けることもできます。 との両方を置き換え `{DATASET_ID}` ま `{PANDA_DATAFRAME}`す。

```python
from platform_sdk.models import Dataset
from platform_sdk.dataset_writer import DatasetWriter

dataset = Dataset(get_platform_sdk_client_context()).get_by_id(dataset_id="{DATASET_ID}")
dataset_writer = DatasetWriter(get_platform_sdk_client_context(), dataset)
write_tracker = dataset_writer.write({PANDA_DATAFRAME}, file_format='json')
```

### で使用するクエリデ [!DNL Query Service] ータ [!DNL Python] {#query-data-python}

[!DNL JupyterLab][!DNL Platform][!DNL Python] で を使用すると、 ノートブックで SQL を使用して、[Adobe Experience Platform クエリサービス](https://docs.adobe.com/content/help/ja-JP/experience-platform/query/home.html)を通じてデータにアクセスできます。Accessing data through [!DNL Query Service] can be useful for dealing with large datasets due to its superior running times. Be advised that querying data using [!DNL Query Service] has a processing time limit of ten minutes.

Before you use [!DNL Query Service] in [!DNL JupyterLab], ensure you have a working understanding of the [[!DNL Query Service] SQL syntax](https://docs.adobe.com/content/help/ja-JP/experience-platform/query/home.html#!api-specification/markdown/narrative/technical_overview/query-service/sql/syntax.md).

Querying data using [!DNL Query Service] requires you to provide the name of the target dataset. 必要なコードセルを生成するには、**[!UICONTROL データエクスプローラー]**&#x200B;を使用して目的のデータセットを見つけます。Right click on the dataset listing and click **[!UICONTROL Query Data in Notebook]** to generate two code cells in your notebook. これらの2つのセルの詳細を下に示します。

![](../images/jupyterlab/data-access/python-query-dataset.png)

In order to utilize [!DNL Query Service] in [!DNL JupyterLab], you must first create a connection between your working [!DNL Python] notebook and [!DNL Query Service]. これは、最初に生成されたセルを実行することで達成できます。

```python
qs_connect()
```

2 番目に生成されたセルでは、最初の行を SQL クエリの前に定義する必要があります。デフォルトでは、生成されたセルは、クエリ結果を Pandas データフレームとして保存するオプションの変数（`df0`）を定義します。<br>この `-c QS_CONNECTION` 引数は必須で、SQLクエリを実行するようカーネルに指示 [!DNL Query Service]します。 その他の引数のリストは、[付録](#optional-sql-flags-for-query-service)を参照してください。

```python
%%read_sql df0 -c QS_CONNECTION
SELECT *
FROM name_of_the_dataset
LIMIT 10
/* Querying table "name_of_the_dataset" (datasetId: {DATASET_ID})*/
```

Python 変数は、次の例に示すように、文字列形式の構文を使用して中括弧（`{}`）で囲むことで、SQL クエリ内で直接参照できます。

```python
table_name = 'name_of_the_dataset'
table_columns = ','.join(['col_1','col_2','col_3'])
```

```python
%%read_sql demo -c QS_CONNECTION
SELECT {table_columns}
FROM {table_name}
```

### Filter [!DNL ExperienceEvent] data {#python-filter}

In order to access and filter an [!DNL ExperienceEvent] dataset in a [!DNL Python] notebook, you must provide the ID of the dataset (`{DATASET_ID}`) along with the filter rules that define a specific time range using logical operators. 時間範囲を定義すると、指定されたページ番号は無視され、データセット全体が考慮されます。

フィルタリング操作のリストを以下に示します。

- `eq()`：と等しい
- `gt()`：より大きい
- `ge()`：より大きいか等しい
- `lt()`：より小さい
- `le()`：より小さいか等しい
- `And()`：論理積演算子
- `Or()`：論理和演算子

The following cell filters an [!DNL ExperienceEvent] dataset to data existing exclusively between January 1, 2019 and the end of December 31, 2019.

```python
# Python

from platform_sdk.dataset_reader import DatasetReader

dataset_reader = DatasetReader(get_platform_sdk_client_context(), dataset_id="{DATASET_ID}")
df = dataset_reader.\
    where(dataset_reader["timestamp"].gt("2019-01-01 00:00:00").\
    And(dataset_reader["timestamp"].lt("2019-12-31 23:59:59"))\
).read()
```

## Rノートパソコン {#r-notebooks}

Rノートブックでは、データセットにアクセスする際にデータをページネーションできます。 ページ番号付けの有無に関わらずデータを読み取るコード例を以下に示します。使用可能なスターターRノートブックの詳細については、JupyterLabユーザーガイド内の [[!DNL JupyterLab] ランチャー](./overview.md#launcher) (Launcher)の項を参照してください。

以下のRドキュメントでは、次の概念について説明しています。

- [データセットからの読み取り](#r-read-dataset)
- [データセットへの書き込み](#write-r)
- [ExperienceEventデータのフィルター](#r-filter)

### Read from a dataset in R {#r-read-dataset}

**ページ番号なし：**

次のコードを実行すると、データセット全体が読み取られます。実行が成功した場合、データは `df0` 変数で参照される Pandas データフレームとして保存されます。

```R
# R

library(reticulate)
use_python("/usr/local/bin/ipython")
psdk <- import("platform_sdk")
datetime <- import("datetime", convert = FALSE)
py_run_file("~/.ipython/profile_default/startup/platform_sdk_context.py")
DatasetReader <- psdk$dataset_reader$DatasetReader
dataset_reader <- DatasetReader(py$get_platform_sdk_client_context(), dataset_id="{DATASET_ID}")
df0 <- dataset_reader$read()
head(df0)
```

**ページ番号付き：**

次のコードを実行すると、指定したデータセットからデータが読み取られます。ページ番号付けは、`limit()` 関数を使用してデータを制限し、`offset()` 関数を使用してデータをオフセットすることで実現します。データの制限とは、読み取るデータポイントの最大数を指し、オフセットとは、データの読み取り前にスキップするデータポイントの数を指します。読み取り操作が正常に実行された場合、データは `df0` 変数が参照する Pandas データフレームとして保存されます。

```R
# R

library(reticulate)
use_python("/usr/local/bin/ipython")
psdk <- import("platform_sdk")
datetime <- import("datetime", convert = FALSE)
py_run_file("~/.ipython/profile_default/startup/platform_sdk_context.py")

DatasetReader <- psdk$dataset_reader$DatasetReader
dataset_reader <- DatasetReader(py$get_platform_sdk_client_context(), dataset_id="{DATASET_ID}") 
df0 <- dataset_reader$limit(100L)$offset(10L)$read()
```

### Rのデータセットに書き込む {#write-r}

JupyterLabノートブックのデータセットに書き込むには、JupterLabの左側のナビゲーションで[Data]アイコンタブ（下のハイライト表示）を選択します。 「 **[!UICONTROL Datasets]** 」ディレクトリと「 **[!UICONTROL スキーマ]** 」ディレクトリが表示されます。 「 **[!UICONTROL Datasets]** 」を選択して右クリックし、使用するデータセットのドロップダウンメニューから「 **[!UICONTROL Write Data in Notebook]** 」オプションを選択します。 ノートブックの下部に実行可能コードのエントリが表示されます。

![](../images/jupyterlab/data-access/r-write-dataset.png)

- 「 **[!UICONTROL Write Data in Notebook]** 」を使用して、選択したデータセットを使用して書き込みセルを生成します。
- 「 **[!UICONTROL Explore Data in Notebook]** 」を使用して、選択したデータセットを使用して読み取りセルを生成します。

または、次のコードセルをコピーして貼り付けることができます。

```R
psdk <- import("platform_sdk")
dataset <- psdk$models$Dataset(py$get_platform_sdk_client_context())$get_by_id(dataset_id="{DATASET_ID}")
dataset_writer <- psdk$dataset_writer$DatasetWriter(py$get_platform_sdk_client_context(), dataset)
write_tracker <- dataset_writer$write(df, file_format='json')
```

### Filter [!DNL ExperienceEvent] data {#r-filter}

In order to access and filter an [!DNL ExperienceEvent] dataset in a R notebook, you must provide the ID of the dataset (`{DATASET_ID}`) along with the filter rules that define a specific time range using logical operators. 時間範囲を定義すると、指定されたページ番号は無視され、データセット全体が考慮されます。

フィルタリング操作のリストを以下に示します。

- `eq()`：と等しい
- `gt()`：より大きい
- `ge()`：より大きいか等しい
- `lt()`：より小さい
- `le()`：より小さいか等しい
- `And()`：論理積演算子
- `Or()`：論理和演算子

The following cell filters an [!DNL ExperienceEvent] dataset to data existing exclusively between January 1, 2019 and the end of December 31, 2019.

```R
# R

library(reticulate)
use_python("/usr/local/bin/ipython")
psdk <- import("platform_sdk")
datetime <- import("datetime", convert = FALSE)
py_run_file("~/.ipython/profile_default/startup/platform_sdk_context.py")

client_context <- py$PLATFORM_SDK_CLIENT_CONTEXT
DatasetReader <- psdk$dataset_reader$DatasetReader
dataset_reader <- DatasetReader(py$get_platform_sdk_client_context(), dataset_id="{DATASET_ID}") 

df0 <- dataset_reader$
    where(dataset_reader["timestamp"]$gt("2019-01-01 00:00:00")$
    And(dataset_reader["timestamp"]$lt("2019-12-31 23:59:59"))
)$read()
```

## PySpark 3ノートブック {#pyspark-notebook}

以下のPySparkドキュメントは、以下の概念を説明しています。

- [sparkSessionの初期化](#spark-initialize)
- [データの読み取りと書き込み](#magic)
- [ローカルデータフレームの作成](#pyspark-create-dataframe)
- [ExperienceEventデータのフィルター](#pyspark-filter-experienceevent)

### sparkSessionの初期化 {#spark-initialize}

すべての [!DNL Spark] 2.4ノートブックでは、次のボイラープレートコードを使用してセッションを初期化する必要があります。

```scala
from pyspark.sql import SparkSession
spark = SparkSession.builder.getOrCreate()
```

### %datasetを使用したPySpark 3ノートブックの読み取りと書き込み {#magic}

2.4の導入に伴い、PySpark 3 ( [!DNL Spark] 2.4)ノートブックで使用する `%dataset`[!DNL Spark] カスタムマジックが提供されます。 IPythonカーネルで使用できるマジックコマンドの詳細については、IPythonの [マジックドキュメントを参照してください](https://ipython.readthedocs.io/en/stable/interactive/magics.html)。


**用途**

```scala
%dataset {action} --datasetId {id} --dataFrame {df}`
```

**説明**

ノートブック( [!DNL Data Science Workspace][!DNL PySpark][!DNL Python] 3カーネル)からデータセットを読み取ったり書き込んだりするためのカスタムマジックコマンド。

| 名前 | 説明 | 必須 |
| --- | --- | --- |
| `{action}` | データセットに対して実行するアクションのタイプ。 「読み取り」と「書き込み」の2つのアクションを使用できます。 | ○ |
| `--datasetId {id}` | 読み取りまたは書き込みを行うデータセットのIDを指定するために使用します。 | ○ |
| `--dataFrame {df}` | パンダのデータフレーム。 <ul><li> アクションが「read」の場合、{df}は、データセット読み取り操作の結果を利用できる変数です。 </li><li> アクションが「書き込み」の場合、このデータフレーム{df}はデータセットに書き込まれます。 </li></ul> | ○ |
| `--mode` | データの読み取り方法を変更する追加のパラメーター。 使用できるパラメーターは、「batch」および「interactive」です。 デフォルトでは、モードは「interactive」に設定されています。 大量のデータを読み取る場合は、「バッチ」モードを使用することをお勧めします。 | × |

>[!TIP]
>
>PySparkテーブルを [ノートPCのデータ制限](#notebook-data-limits) (limits)セクションで確認し、をに設定するか `mode` 、に設定するかを決定し `interactive``batch`ます。

**例**

- **次の例を読みます**。 `%dataset read --datasetId 5e68141134492718af974841 --dataFrame pd0`
- **次に例を示します**。 `%dataset write --datasetId 5e68141134492718af974842 --dataFrame pd0`

次の方法を使用して、JupterLab buyで上記の例を自動生成できます。

JupterLabの左側のナビゲーションで[Data]アイコンタブ（下でハイライト表示）を選択します。 「 **[!UICONTROL Datasets]** 」ディレクトリと「 **[!UICONTROL スキーマ]** 」ディレクトリが表示されます。 「 **[!UICONTROL Datasets]** 」を選択して右クリックし、使用するデータセットのドロップダウンメニューから「 **[!UICONTROL Write Data in Notebook]** 」オプションを選択します。 ノートブックの下部に実行可能コードのエントリが表示されます。

- 読み取りセルを生成するには、 **[!UICONTROL 「Explore Data in Notebook]** 」を使用します。
- 書き込みセルを生成するには、「 **[!UICONTROL Write Data in Notebook]** 」を使用します。

![](../images/jupyterlab/data-access/pyspark-write-dataset.png)

### ローカルデータフレームの作成 {#pyspark-create-dataframe}

PySpark 3を使用してローカル・データ・フレームを作成するには、SQLクエリを使用します。 次に例を示します。

```scala
date_aggregation.createOrReplaceTempView("temp_df")

df = spark.sql('''
  SELECT *
  FROM sparkdf
''')

local_df
```

```scala
df = spark.sql('''
  SELECT *
  FROM sparkdf
  LIMIT limit
''')
```

```scala
sample_df = df.sample(fraction)
```

>[!TIP]
>
>また、オプションのシードサンプル(boolean withReplacement、重複分数、長いシードなど)を指定することもできます。

### Filter [!DNL ExperienceEvent] data {#pyspark-filter-experienceevent}

Accessing and filtering an [!DNL ExperienceEvent] dataset in a PySpark notebook requires you to provide the dataset identity (`{DATASET_ID}`), your organization&#39;s IMS identity, and the filter rules defining a specific time range. A filtering time range is defined by using the function `spark.sql()`, where the function parameter is a SQL query string.

The following cells filter an [!DNL ExperienceEvent] dataset to data existing exclusively between January 1, 2019 and the end of December 31, 2019.

```python
# PySpark 3 (Spark 2.4)

from pyspark.sql import SparkSession
spark = SparkSession.builder.getOrCreate()

%dataset read --datasetId {DATASET_ID} --dataFrame df

df.createOrReplaceTempView("event")
timepd = spark.sql("""
    SELECT *
    FROM event
    WHERE timestamp > CAST('2019-01-01 00:00:00.0' AS TIMESTAMP)
    AND timestamp < CAST('2019-12-31 23:59:59.9' AS TIMESTAMP)
""")
timepd.show()
```

## Scalaノートブック {#scala-notebook}

次のドキュメントに、次の概念の例を示します。

- [sparkSessionの初期化](#scala-initialize)
- [データセットの読み取り](#read-scala-dataset)
- [データセットへの書き込み](#scala-write-dataset)
- [ローカルデータフレームの作成](#scala-create-dataframe)
- [ExperienceEventデータのフィルター](#scala-experienceevent)

### SparkSessionを初期化しています {#scala-initialize}

すべてのScalaノートブックで、次のボイラープレートコードを使用してセッションを初期化する必要があります。

```scala
import org.apache.spark.sql.{ SparkSession }
val spark = SparkSession
  .builder()
  .master("local")
  .getOrCreate()
```

### データセットの読み取り {#read-scala-dataset}

Scalaでは、プラットフォーム値 `clientContext` を取得および返すために読み込むことができるので、などの変数を定義する必要がありません `var userToken`。 以下のScalaの例では、データセットの読み取りに必要な値をすべて取得して返すた `clientContext` めに、が使用されています。

```scala
import org.apache.spark.sql.{Dataset, SparkSession}
import com.adobe.platform.token.ClientContext
val spark = SparkSession.builder().master("local").config("spark.sql.warehouse.dir", "/").getOrCreate()

val clientContext = ClientContext.getClientContext()
val df1 = spark.read.format("com.adobe.platform.query")
  .option("user-token", clientContext.getUserToken())
  .option("ims-org", clientContext.getOrgId())
  .option("api-key", clientContext.getApiKey())
  .option("service-token", clientContext.getServiceToken())
  .option("sandbox-name", clientContext.getSandboxName())
  .option("mode", "interactive")
  .option("dataset-id", "5e68141134492718af974844")
  .load()

df1.printSchema()
df1.show(10)
```

| 要素 | 説明 |
| ------- | ----------- |
| df1 | データの読み取りと書き込みに使用されるPandasデータフレームを表す変数です。 |
| user-token | を使用して自動的に取得されるユーザートークン `clientContext.getUserToken()`。 |
| service-token | を使用して自動的にフェッチされるサービストークン `clientContext.getServiceToken()`。 |
| ims-org | を使用して自動的に取得されるIMS組織ID `clientContext.getOrgId()`。 |
| api-key | を使用して自動的に取得されるAPIキー `clientContext.getApiKey()`。 |

>[!TIP]
>
>[ [ノートブックデータの制限](#notebook-data-limits) ]セクションの[Scala]テーブルを確認し、をに設定す `mode` るか、またはに設定するかを決定し `interactive``batch`ます。

次の方法を使用して、JupterLab buyで上記の例を自動生成できます。

JupterLabの左側のナビゲーションで[Data]アイコンタブ（下でハイライト表示）を選択します。 「 **[!UICONTROL Datasets]** 」ディレクトリと「 **[!UICONTROL スキーマ]** 」ディレクトリが表示されます。 「 **[!UICONTROL Datasets]** 」を選択して右クリックし、使用するデータセットのドロップダウンメニューから「 **[!UICONTROL Explore Data in Notebook]** 」オプションを選択します。 ノートブックの下部に実行可能コードのエントリが表示されます。
And
- 読み取りセルを生成するには、 **[!UICONTROL 「Explore Data in Notebook]** 」を使用します。
- 書き込みセルを生成するには、「 **[!UICONTROL Write Data in Notebook]** 」を使用します。

![](../images/jupyterlab/data-access/scala-write-dataset.png)

### データセットへの書き込み {#scala-write-dataset}

Scalaでは、プラットフォーム値 `clientContext` を取得および返すために読み込むことができるので、などの変数を定義する必要がありません `var userToken`。 以下のScalaの例では、 `clientContext` を使用して、データセットへの書き込みに必要なすべての値を定義し、返します。

```scala
import org.apache.spark.sql.{Dataset, SparkSession}
import com.adobe.platform.token.ClientContext
val spark = SparkSession.builder().master("local").config("spark.sql.warehouse.dir", "/").getOrCreate()

val clientContext = ClientContext.getClientContext()
df1.write.format("com.adobe.platform.query")
  .option("user-token", clientContext.getUserToken())
  .option("service-token", clientContext.getServiceToken())
  .option("ims-org", clientContext.getOrgId())
  .option("api-key", clientContext.getApiKey())
  .option("sandbox-name", clientContext.getSandboxName())
  .option("mode", "interactive")
  .option("dataset-id", "5e68141134492718af974844")
  .save()
```

| element | 説明 |
| ------- | ----------- |
| df1 | データの読み取りと書き込みに使用されるPandasデータフレームを表す変数です。 |
| user-token | を使用して自動的に取得されるユーザートークン `clientContext.getUserToken()`。 |
| service-token | を使用して自動的にフェッチされるサービストークン `clientContext.getServiceToken()`。 |
| ims-org | を使用して自動的に取得されるIMS組織ID `clientContext.getOrgId()`。 |
| api-key | を使用して自動的に取得されるAPIキー `clientContext.getApiKey()`。 |

>[!TIP]
>
>[ [ノートブックデータの制限](#notebook-data-limits) ]セクションの[Scala]テーブルを確認し、をに設定す `mode` るか、またはに設定するかを決定し `interactive``batch`ます。

### ローカルデータフレームの作成 {#scala-create-dataframe}

Scalaを使用してローカル・データ・フレームを作成するには、SQLクエリが必要です。 次に例を示します。

```scala
sparkdf.createOrReplaceTempView("sparkdf")

val localdf = spark.sql("SELECT * FROM sparkdf LIMIT 1)
```

### Filter [!DNL ExperienceEvent] data {#scala-experienceevent}

Accessing and filtering an [!DNL ExperienceEvent] dataset in a Scala notebook requires you to provide the dataset identity (`{DATASET_ID}`), your organization&#39;s IMS identity, and the filter rules defining a specific time range. 時間範囲のフィルタリングは、`spark.sql()` 関数を使用して定義します。関数パラメータは SQL クエリ文字列です。

The following cells filter an [!DNL ExperienceEvent] dataset to data existing exclusively between January 1, 2019 and the end of December 31, 2019.

```scala
// Spark (Spark 2.4)

// Turn off extra logging
import org.apache.log4j.{Level, Logger}
Logger.getLogger("org").setLevel(Level.OFF)
Logger.getLogger("com").setLevel(Level.OFF)

import org.apache.spark.sql.{Dataset, SparkSession}
val spark = org.apache.spark.sql.SparkSession.builder().appName("Notebook")
  .master("local")
  .getOrCreate()

// Stage Exploratory
val dataSetId: String = "{DATASET_ID}"
val orgId: String = sys.env("IMS_ORG_ID")
val clientId: String = sys.env("PYDASDK_IMS_CLIENT_ID")
val userToken: String = sys.env("PYDASDK_IMS_USER_TOKEN")
val serviceToken: String = sys.env("PYDASDK_IMS_SERVICE_TOKEN")
val mode: String = "batch"

var df = spark.read.format("com.adobe.platform.query")
  .option("user-token", userToken)
  .option("ims-org", orgId)
  .option("api-key", clientId)
  .option("mode", mode)
  .option("dataset-id", dataSetId)
  .option("service-token", serviceToken)
  .load()
df.createOrReplaceTempView("event")
val timedf = spark.sql("""
    SELECT * 
    FROM event 
    WHERE timestamp > CAST('2019-01-01 00:00:00.0' AS TIMESTAMP)
    AND timestamp < CAST('2019-12-31 23:59:59.9' AS TIMESTAMP)
""")
timedf.show()
```

## 次の手順

このドキュメントでは、JupyterLabノートブックを使用したデータセットへのアクセスに関する一般的なガイドラインを説明します。 データセットのクエリに関する詳細な例については、JupyterLabノートブック [・ドキュメントの](./query-service.md) クエリ・サービスを参照してください。 データセットの表示と参照の詳細については、ノートブックを使用したデータの [分析に関するドキュメントを参照してください](./analyze-your-data.md)。

## のオプションのSQLフラグ [!DNL Query Service] {#optional-sql-flags-for-query-service}

This table outlines the optional SQL flags that can be used for [!DNL Query Service].

| **フラグ** | **説明** |
| --- | --- |
| `-h`、`--help` | ヘルプメッセージを表示し、終了します。 |
| `-n`、`--notify` | 通知のオプションを切り替えてクエリ結果を通知します。 |
| `-a`、`--async` | このフラグを使用すると、クエリが非同期的に実行され、カーネルの実行中にクエリを解放できます。変数にクエリ結果を割り当てる場合、クエリが完了しない場合は定義されない可能性があるので、注意が必要です。 |
| `-d`、`--display` | このフラグを使用すると、結果が表示されなくなります。 |

