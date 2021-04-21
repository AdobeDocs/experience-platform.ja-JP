---
keywords: Experience Platform;JupyterLab;notebooks;Data Science Workspace；人気の高いトピック；%dataset;interactive mode;batch mode;Spark sdk;python sdk;access data;notebook data access
solution: Experience Platform
title: Jupyterlabノートブックでのデータアクセス
topic-legacy: Developer Guide
description: このガイドでは、Data Science Workspaceに組み込まれたJupterノートブックを使用してデータにアクセスする方法について説明します。
exl-id: 2035a627-5afc-4b72-9119-158b95a35d32
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '3101'
ht-degree: 25%

---

# [!DNL Jupyterlab]ノートブックのデータアクセス

サポートされる各カーネルは、ノートブック内のデータセットから Platform データを読み取るための組み込み機能を備えてます。現在、JupyterLab(Adobe Experience Platform・データ・サイエンス・ワークスペース)は、[!DNL Python]、R、PySpark、Scala用のノートブックをサポートしています。 ただし、データのページ番号付けのサポートは[!DNL Python]とRノートブックに限定されます。 このガイドでは、JupyterLabノートブックを使用してデータにアクセスする方法について説明します。

## はじめに

このガイドを読む前に、[[!DNL JupyterLab] ユーザーガイド](./overview.md)を参照し、[!DNL JupyterLab]とData Science Workspace内でのその役割の概要を確認してください。

## ノートブックデータの制限{#notebook-data-limits}

>[!IMPORTANT]
>
>PySparkとScalaノートブックの場合は、&quot;Remote RPC client disassociated&quot;という理由でエラーが発生しています。 これは、通常、ドライバまたはエクゼキュータのメモリ不足を意味します。 [&quot;batch&quot;モード](#mode)に切り替えて、このエラーを解決してください。

次の情報は、読み取り可能な最大データ量、使用されたデータの種類、およびデータ読み取りにかかる予測時間枠を定義します。

[!DNL Python]とRでは、40 GBのRAMで構成されたノートブックサーバがベンチマークに使用されました。 PySparkとScalaの場合、64GBのRAM、8コア、2 DBUで構成され、最大4人のワーカーを持つデータバリッククラスタが、以下のベンチマークに使用されました。

使用されるExperienceEventスキーマデータのサイズは、1,000行（1,000行）から10億行（1B行）まで様々です。 PySparkと[!DNL Spark]の指標の場合、XDMデータには10日の期間が使われていました。

アドホックスキーマデータは、[!DNL Query Service]選択としてテーブルを作成(CTAS)を使用して事前に処理されました。 また、1000行(1K)から10億行(1B)まで様々なサイズがあります。

### バッチモードと対話モード{#mode}を使用する場合

PySparkやScalaのノートブックでデータセットを読み込む場合、データセットを読み込む際に、インタラクティブモードまたはバッチモードを使用できます。 バッチモードは大きなデータセット用ですが、インタラクティブモードは高速な結果を得るために作成されます。

- PySparkとScalaのノートブックでは、500万行以上のデータを読み込む場合は、batchモードを使用する必要があります。 各モードの効率性についての詳細は、以下の[PySpark](#pyspark-data-limits)や[Scala](#scala-data-limits)のデータ制限テーブルを参照してください。

### [!DNL Python] ノート型データの制限

**XDM ExperienceEventスキーマ:22分** 以内に、最大200万行（ディスク上の最大6.1 GBのデータ）のXDMデータを読み取ることができます。行を追加すると、エラーが発生する場合があります。

| 行数 | 1K | 1万 | 100,000 | 1M | 2M |
| ----------------------- | ------ | ------ | ----- | ----- | ----- |
| ディスク上のサイズ(MB) | 18.73 | 187.5 | 308 | 3000 | 6050 |
| SDK （秒） | 20.3 | 86.8 | 63 | 659 | 1315 |

**アドホックスキーマ:XDM以外（アドホックデータ）** のデータは、14分以内に最大500万行（ディスク上の最大5.6 GBのデータ）読み取れるはずです。行を追加すると、エラーが発生する場合があります。

| 行数 | 1K | 1万 | 100,000 | 1M | 2M | 3M | 5M |
| ----------------------- | ------- | ------- | ----- | ----- | ----- | ----- | ------ |
| ディスク上のサイズ(MB) | 1.21 | 11.72 | 115 | 1120 | 2250 | 3380 | 5630 |
| SDK （秒） | 7.27 | 9.04 | 27.3 | 180 | 346 | 487 | 819 |

### Rノートブックデータの制限

**XDM ExperienceEventスキーマ:13分** 以内に、最大100万行のXDMデータ（ディスク上の3 GBのデータ）を読み取ることができます。

| 行数 | 1K | 1万 | 100,000 | 1M |
| ----------------------- | ------ | ------ | ----- | ----- |
| ディスク上のサイズ(MB) | 18.73 | 187.5 | 308 | 3000 |
| Rカーネル（秒） | 14.03 | 69.6 | 86.8 | 775 |

**アドホックスキーマ：約10分** で、アドホックデータ（ディスク上のデータは293MB）の最大行300万行を読み取ることができます。

| 行数 | 1K | 1万 | 100,000 | 1M | 2M | 3M |
| ----------------------- | ------- | ------- | ----- | ----- | ----- | ----- |
| ディスク上のサイズ(MB) | 0.082 | 0.612 | 9.0 | 91 | 188 | 293 |
| R SDK （イン秒） | 7.7 | 4.58 | 35.9 | 233 | 470.5 | 603 |

### PySpark （[!DNL Python]カーネル）ノートブックのデータ制限：{#pyspark-data-limits}

**XDM ExperienceEventスキーマ：インタラクティブモード** では、約20分で最大500万行（ディスク上のデータは13.42 GB）のXDMデータを読み取れるようにする必要があります。インタラクティブモードでは、最大500万行までのみサポートされます。 より大きなデータセットを読み取る場合は、バッチモードに切り替えることをお勧めします。 バッチモードでは、約14時間で最大5億行（ディスク上のデータは最大1.31 TB）のXDMデータを読み取れるはずです。

| 行数 | 1K | 1万 | 100,000 | 1M | 2M | 3M | 5M | 10M | 50万 | 1億 | 5億 |
|-------------------------|--------|--------|-------|-------|-------|-------|---------|---------|----------|--------|--------|
| ディスク上のサイズ | 2.93MB | 4.38MB | 29.02 | 2.69 GB | 5.39 GB | 8.09 GB | 13.42 GB | 26.82 GB | 134.24 GB | 268.39 GB | 1.31 TB |
| SDK（インタラクティブモード） | 33秒 | 32.4s | 55.1s | 253.5s | 489.2s | 729.6s | 1206.8s | - | - | - | - |
| SDK（バッチモード） | 815.8s | 492.8s | 379.1s | 637.4s | 624.5s | 869.2s | 1104.1s | 1786s | 5387.2s | 10624.6s | 50547s |

**アドホックスキーマ：イン** タラクティブモードでは、XDM以外のデータの最大500万行（ディスク上の最大5.36 GBのデータ）を3分未満で読み取ることができます。バッチモードでは、約18分で最大10億行（ディスク上のデータは最大1.05 TB）の非XDMデータを読み取れるはずです。

| 行数 | 1K | 1万 | 100,000 | 1M | 2M | 3M | 5M | 10M | 50万 | 1億 | 5億 | 1B |
|--------------|--------|---------|---------|-------|-------|-------|--------|--------|---------|--------|---------|-------|
| ディスク上のサイズ | 1.12MB | 11.24MB | 109.48MB | 2.69 GB | 2.14 GB | 3.21 GB | 5.36 GB | 10.71 GB | 53.58 GB | 107.52 GB | 535.88 GB | 1.05 TB |
| SDKインタラクティブモード（秒） | 28.2s | 18.6s | 20.8s | 20.9秒 | 23.8s | 21.7s | 24.7s | - | - | - | - | - |
| SDKバッチモード（秒） | 428.8s | 578.8s | 641.4s | 538.5s | 630.9s | 467.3s | 411s | 675秒 | 702秒 | 719.2s | 1022.1s | 1122.3s |

### [!DNL Spark] （Scalaカーネル）ノートブックデータの制限：  {#scala-data-limits}

**XDM ExperienceEventスキーマ：インタラクティブモード** では、約18分で最大500万行（ディスク上のデータは約13.42 GB）のXDMデータを読み取ることができます。インタラクティブモードでは、最大500万行までのみサポートされます。 より大きなデータセットを読み取る場合は、バッチモードに切り替えることをお勧めします。 バッチモードでは、約14時間で最大5億行（ディスク上のデータは最大1.31 TB）のXDMデータを読み取れるはずです。

| 行数 | 1K | 1万 | 100,000 | 1M | 2M | 3M | 5M | 10M | 50万 | 1億 | 5億 |
|---------------|--------|--------|-------|-------|-------|-------|---------|---------|----------|--------|--------|
| ディスク上のサイズ | 2.93MB | 4.38MB | 29.02 | 2.69 GB | 5.39 GB | 8.09 GB | 13.42 GB | 26.82 GB | 134.24 GB | 268.39 GB | 1.31 TB |
| SDKインタラクティブモード（秒） | 37.9s | 22.7s | 45.6s | 231.7s | 444.7s | 660.6s | 1,100台 | - | - | - | - |
| SDKバッチモード（秒） | 374.4s | 398.5s | 527s | 487.9s | 588.9s | 829年代 | 939.1s | 1441s | 5473.2s | 10118.8 | 49207.6 |

**アドホックスキーマ：イン** タラクティブモードでは、XDM以外のデータの最大500万行（ディスク上の最大5.36 GBのデータ）を3分未満で読み取ることができます。バッチモードでは、約16分で最大10億行（ディスク上の最大1.05 TBのデータ）の非XDMデータを読み取れるはずです。

| 行数 | 1K | 1万 | 100,000 | 1M | 2M | 3M | 5M | 10M | 50万 | 1億 | 5億 | 1B |
|--------------|--------|---------|---------|-------|-------|-------|---------|---------|---------|--------|---------|-------|
| ディスク上のサイズ | 1.12MB | 11.24MB | 109.48MB | 2.69 GB | 2.14 GB | 3.21 GB | 5.36 GB | 10.71 GB | 53.58 GB | 107.52 GB | 535.88 GB | 1.05 TB |
| SDKインタラクティブモード（秒） | 35.7s | 31秒 | 19.5s | 25.3s | 23秒 | 33.2s | 25.5s | - | - | - | - | - |
| SDKバッチモード（秒） | 448.8s | 459.7s | 519秒 | 475.8s | 599.9s | 347.6s | 407.8s | 397s | 518.8s | 487.9s | 760.2s | 975.4s |

## Pythonノートブック{#python-notebook}

[!DNL Python] ノートブックでは、データセットにアクセスする際にデータをページネーションできます。ページ番号付けの有無に関わらずデータを読み取るコード例を以下に示します。使用可能なスターターPythonノートブックの詳細については、JupyterLabユーザーガイドの[[!DNL JupyterLab] Launcher](./overview.md#launcher)セクションを参照してください。

以下のPythonドキュメントでは、次の概念を概説しています。

- [データセットからの読み取り](#python-read-dataset)
- [データセットへの書き込み](#write-python)
- [クエリデータ](#query-data-python)
- [ExperienceEventデータのフィルター](#python-filter)

### Python {#python-read-dataset}のデータセットから読み取る

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

### Pythonのデータセットに書き込む{#write-python}

JupyterLabノートブックのデータセットに書き込むには、JupterLabの左側のナビゲーションで[Data]アイコンタブ（下のハイライト表示）を選択します。 **[!UICONTROL Datasets]**&#x200B;ディレクトリと&#x200B;**[!UICONTROL スキーマ]**&#x200B;ディレクトリが表示されます。 「**[!UICONTROL データセット]**」を選択して右クリックし、使用するデータセットのドロップダウンメニューから「**[!UICONTROL ノートブックにデータを書き込む]**」オプションを選択します。 ノートブックの下部に実行可能コードのエントリが表示されます。

![](../images/jupyterlab/data-access/write-dataset.png)

- **[!UICONTROL 「Write Data in Notebook]**」を使用して、選択したデータセットを使用した書き込みセルを生成します。
- **[!UICONTROL 「Explore Data in Notebook]**」を使用して、選択したデータセットを含む読み取りセルを生成します。
- ノートブック&#x200B;]**の**[!UICONTROL &#x200B;クエリデータを使用して、選択したデータセットと共に基本的なクエリセルを生成します。

または、次のコードセルをコピーして貼り付けることもできます。 `{DATASET_ID}`と`{PANDA_DATAFRAME}`の両方を置き換えます。

```python
from platform_sdk.models import Dataset
from platform_sdk.dataset_writer import DatasetWriter

dataset = Dataset(get_platform_sdk_client_context()).get_by_id(dataset_id="{DATASET_ID}")
dataset_writer = DatasetWriter(get_platform_sdk_client_context(), dataset)
write_tracker = dataset_writer.write({PANDA_DATAFRAME}, file_format='json')
```

### [!DNL Python] {#query-data-python}の[!DNL Query Service]を使用したクエリデータ

[!DNL JupyterLab][!DNL Platform][!DNL Python] で を使用すると、 ノートブックで SQL を使用して、[Adobe Experience Platform クエリサービス](https://docs.adobe.com/content/help/ja-JP/experience-platform/query/home.html)を通じてデータにアクセスできます。[!DNL Query Service]経由でデータにアクセスすると、実行時間が長いため、大きなデータセットを扱うのに便利です。 [!DNL Query Service]を使用したデータのクエリには、処理時間制限が10分あることをお勧めします。

[!DNL JupyterLab]で[!DNL Query Service]を使用する前に、[[!DNL Query Service] SQL構文](https://docs.adobe.com/content/help/ja-JP/experience-platform/query/home.html#!api-specification/markdown/narrative/technical_overview/query-service/sql/syntax.md)に関する十分な理解を得ておく必要があります。

[!DNL Query Service]を使用してデータをクエリする場合は、ターゲットデータセットの名前を指定する必要があります。 必要なコードセルを生成するには、**[!UICONTROL データエクスプローラー]**&#x200B;を使用して目的のデータセットを見つけます。データセットの一覧を右クリックし、「ノートブック&#x200B;]**のクエリデータ」をクリックして、ノートブックに2つのコードセルを生成します。**[!UICONTROL &#x200B;これらの2つのセルの詳細を下に示します。

![](../images/jupyterlab/data-access/python-query-dataset.png)

[!DNL JupyterLab]で[!DNL Query Service]を利用するには、まず作業中の[!DNL Python]ノートブックと[!DNL Query Service]の間に接続を作成する必要があります。 これは、最初に生成されたセルを実行することで達成できます。

```python
qs_connect()
```

2 番目に生成されたセルでは、最初の行を SQL クエリの前に定義する必要があります。デフォルトでは、生成されたセルは、クエリ結果を Pandas データフレームとして保存するオプションの変数（`df0`）を定義します。<br>この `-c QS_CONNECTION` 引数は必須で、SQLクエリを実行するようカーネルに指示 [!DNL Query Service]します。その他の引数のリストは、[付録](#optional-sql-flags-for-query-service)を参照してください。

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

### [!DNL ExperienceEvent]データのフィルタ{#python-filter}

[!DNL Python]ノートブックの[!DNL ExperienceEvent]データセットにアクセスしてフィルタリングするには、論理演算子を使用して、データセットのID(`{DATASET_ID}`)と、特定の時間範囲を定義するフィルター規則を指定する必要があります。 時間範囲を定義すると、指定されたページ番号は無視され、データセット全体が考慮されます。

フィルタリング操作のリストを以下に示します。

- `eq()`：と等しい
- `gt()`：より大きい
- `ge()`：より大きいか等しい
- `lt()`：より小さい
- `le()`：より小さいか等しい
- `And()`：論理積演算子
- `Or()`：論理和演算子

次のセルフィルターでは、2019年1月1日から2019年12月31日末の間にのみ存在するデータに[!DNL ExperienceEvent]データセットを挿入しています。

```python
# Python

from platform_sdk.dataset_reader import DatasetReader

dataset_reader = DatasetReader(get_platform_sdk_client_context(), dataset_id="{DATASET_ID}")
df = dataset_reader.\
    where(dataset_reader["timestamp"].gt("2019-01-01 00:00:00").\
    And(dataset_reader["timestamp"].lt("2019-12-31 23:59:59"))\
).read()
```

## Rノートブック{#r-notebooks}

Rノートブックでは、データセットにアクセスする際にデータをページネーションできます。 ページ番号付けの有無に関わらずデータを読み取るコード例を以下に示します。使用可能なスターターRノートブックの詳細については、JupyterLabユーザーガイドの[[!DNL JupyterLab] Launcher](./overview.md#launcher)セクションを参照してください。

以下のRドキュメントでは、次の概念について説明しています。

- [データセットからの読み取り](#r-read-dataset)
- [データセットへの書き込み](#write-r)
- [ExperienceEventデータのフィルター](#r-filter)

### R {#r-read-dataset}のデータセットから読み取り

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

### R {#write-r}のデータセットに書き込む

JupyterLabノートブックのデータセットに書き込むには、JupterLabの左側のナビゲーションで[Data]アイコンタブ（下のハイライト表示）を選択します。 **[!UICONTROL Datasets]**&#x200B;ディレクトリと&#x200B;**[!UICONTROL スキーマ]**&#x200B;ディレクトリが表示されます。 「**[!UICONTROL データセット]**」を選択して右クリックし、使用するデータセットのドロップダウンメニューから「**[!UICONTROL ノートブックにデータを書き込む]**」オプションを選択します。 ノートブックの下部に実行可能コードのエントリが表示されます。

![](../images/jupyterlab/data-access/r-write-dataset.png)

- **[!UICONTROL 「Write Data in Notebook]**」を使用して、選択したデータセットを使用した書き込みセルを生成します。
- **[!UICONTROL 「Explore Data in Notebook]**」を使用して、選択したデータセットを含む読み取りセルを生成します。

または、次のコードセルをコピーして貼り付けることができます。

```R
psdk <- import("platform_sdk")
dataset <- psdk$models$Dataset(py$get_platform_sdk_client_context())$get_by_id(dataset_id="{DATASET_ID}")
dataset_writer <- psdk$dataset_writer$DatasetWriter(py$get_platform_sdk_client_context(), dataset)
write_tracker <- dataset_writer$write(df, file_format='json')
```

### [!DNL ExperienceEvent]データのフィルタ{#r-filter}

Rノートブックの[!DNL ExperienceEvent]データセットにアクセスしてフィルタリングするには、論理演算子を使用して特定の時間範囲を定義するフィルター規則と共に、データセットのID(`{DATASET_ID}`)を指定する必要があります。 時間範囲を定義すると、指定されたページ番号は無視され、データセット全体が考慮されます。

フィルタリング操作のリストを以下に示します。

- `eq()`：と等しい
- `gt()`：より大きい
- `ge()`：より大きいか等しい
- `lt()`：より小さい
- `le()`：より小さいか等しい
- `And()`：論理積演算子
- `Or()`：論理和演算子

次のセルフィルターでは、2019年1月1日から2019年12月31日末の間にのみ存在するデータに[!DNL ExperienceEvent]データセットを挿入しています。

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

## PySpark 3ノートブック{#pyspark-notebook}

以下のPySparkドキュメントは、以下の概念を説明しています。

- [sparkSessionの初期化](#spark-initialize)
- [データの読み取りと書き込み](#magic)
- [ローカルデータフレームの作成](#pyspark-create-dataframe)
- [ExperienceEventデータのフィルター](#pyspark-filter-experienceevent)

### sparkSession {#spark-initialize}を初期化しています

すべての[!DNL Spark] 2.4ノートブックは、次のボイラープレートコードを使用してセッションを初期化する必要があります。

```scala
from pyspark.sql import SparkSession
spark = SparkSession.builder.getOrCreate()
```

### %datasetを使ってPySpark 3ノートブック{#magic}で読み書きする

[!DNL Spark] 2.4の導入に伴い、PySpark 3 ([!DNL Spark] 2.4)ノートブックで使う`%dataset`カスタムマジックが提供されます。 IPythonカーネルで使用できるマジックコマンドの詳細については、[IPython magic documentation](https://ipython.readthedocs.io/en/stable/interactive/magics.html)を参照してください。


**用途**

```scala
%dataset {action} --datasetId {id} --dataFrame {df}`
```

**説明**

[!DNL PySpark]ノートブック（[!DNL Python] 3カーネル）からデータセットを読み取ったり書き込んだりするための、カスタム[!DNL Data Science Workspace]マジックコマンド。

| 名前 | 説明 | 必須 |
| --- | --- | --- |
| `{action}` | データセットに対して実行するアクションのタイプ。 「読み取り」と「書き込み」の2つのアクションを使用できます。 | ○ |
| `--datasetId {id}` | 読み取りまたは書き込みを行うデータセットのIDを指定するために使用します。 | ○ |
| `--dataFrame {df}` | パンダのデータフレーム。 <ul><li> アクションが「read」の場合、{df}は、データセット読み取り操作の結果を利用できる変数です。 </li><li> アクションが「書き込み」の場合、このデータフレーム{df}はデータセットに書き込まれます。 </li></ul> | ○ |
| `--mode` | データの読み取り方法を変更する追加のパラメーター。 使用できるパラメーターは、「batch」および「interactive」です。 デフォルトでは、モードは「interactive」に設定されています。 大量のデータを読み取る場合は、「バッチ」モードを使用することをお勧めします。 | × |

>[!TIP]
>
>[ノートブックデータの制限](#notebook-data-limits)セクション内のPySparkテーブルを調べて、`mode`を`interactive`か`batch`に設定すべきかを判断します。

**例**

- **次の例を読みます**。  `%dataset read --datasetId 5e68141134492718af974841 --dataFrame pd0`
- **次に例を示します**。  `%dataset write --datasetId 5e68141134492718af974842 --dataFrame pd0`

次の方法を使用して、JupterLab buyで上記の例を自動生成できます。

JupterLabの左側のナビゲーションで[Data]アイコンタブ（下でハイライト表示）を選択します。 **[!UICONTROL Datasets]**&#x200B;ディレクトリと&#x200B;**[!UICONTROL スキーマ]**&#x200B;ディレクトリが表示されます。 「**[!UICONTROL データセット]**」を選択して右クリックし、使用するデータセットのドロップダウンメニューから「**[!UICONTROL ノートブックにデータを書き込む]**」オプションを選択します。 ノートブックの下部に実行可能コードのエントリが表示されます。

- **[!UICONTROL 「Explore Data in Notebook]**」を使用して、読み取りセルを生成します。
- 書き込みセルを生成するには、**[!UICONTROL 「Write Data in Notebook]**」を使用します。

![](../images/jupyterlab/data-access/pyspark-write-dataset.png)

### ローカルデータフレーム{#pyspark-create-dataframe}を作成

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

### [!DNL ExperienceEvent]データのフィルタ{#pyspark-filter-experienceevent}

PySparkノートブック内の[!DNL ExperienceEvent]データセットにアクセスしてフィルタするには、データセットID(`{DATASET_ID}`)、組織のIMS ID、および特定の時間範囲を定義するフィルタルールを指定する必要があります。 フィルタリング時間範囲は、関数`spark.sql()`を使用して定義します。関数パラメータはSQLクエリ文字列です。

次のセルでは、[!DNL ExperienceEvent]データセットを、2019年1月1日から2019年12月31日末の間にのみ存在するデータにフィルターします。

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

## Scalaノートブック{#scala-notebook}

次のドキュメントに、次の概念の例を示します。

- [sparkSessionの初期化](#scala-initialize)
- [データセットの読み取り](#read-scala-dataset)
- [データセットへの書き込み](#scala-write-dataset)
- [ローカルデータフレームの作成](#scala-create-dataframe)
- [ExperienceEventデータのフィルター](#scala-experienceevent)

### SparkSession {#scala-initialize}を初期化しています

すべてのScalaノートブックで、次のボイラープレートコードを使用してセッションを初期化する必要があります。

```scala
import org.apache.spark.sql.{ SparkSession }
val spark = SparkSession
  .builder()
  .master("local")
  .getOrCreate()
```

### データセット{#read-scala-dataset}を読み取ります

Scalaでは、`clientContext`を読み込んでプラットフォーム値を取得し、返すことができるので、`var userToken`などの変数を定義する必要はありません。 以下のScalaの例では、データセットの読み取りに必要なすべての値を取得し、返すために`clientContext`が使用されています。

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
| user-token | `clientContext.getUserToken()`を使用して自動的に取得されるユーザートークン。 |
| service-token | `clientContext.getServiceToken()`を使用して自動的に取得されるサービストークン。 |
| ims-org | `clientContext.getOrgId()`を使用して自動的に取得されるIMS組織ID。 |
| api-key | `clientContext.getApiKey()`を使用して自動的に取り込まれたAPIキー。 |

>[!TIP]
>
>[ノートブックデータの制限](#notebook-data-limits)セクション内のScalaテーブルを確認し、`mode`を`interactive`に設定するか`batch`に設定するかを判断します。

次の方法を使用して、JupterLab buyで上記の例を自動生成できます。

JupterLabの左側のナビゲーションで[Data]アイコンタブ（下でハイライト表示）を選択します。 **[!UICONTROL Datasets]**&#x200B;ディレクトリと&#x200B;**[!UICONTROL スキーマ]**&#x200B;ディレクトリが表示されます。 「**[!UICONTROL データセット]**」を選択して右クリックし、使用するデータセットのドロップダウンメニューから「**[!UICONTROL ノートブックのデータを探索]**」オプションを選択します。 ノートブックの下部に実行可能コードのエントリが表示されます。
And
- **[!UICONTROL 「Explore Data in Notebook]**」を使用して、読み取りセルを生成します。
- 書き込みセルを生成するには、**[!UICONTROL 「Write Data in Notebook]**」を使用します。

![](../images/jupyterlab/data-access/scala-write-dataset.png)

### データセット{#scala-write-dataset}に書き込む

Scalaでは、`clientContext`を読み込んでプラットフォーム値を取得し、返すことができるので、`var userToken`などの変数を定義する必要はありません。 以下のScalaの例では、`clientContext`を使用して、データセットへの書き込みに必要なすべての値を定義し、返します。

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
| user-token | `clientContext.getUserToken()`を使用して自動的に取得されるユーザートークン。 |
| service-token | `clientContext.getServiceToken()`を使用して自動的に取得されるサービストークン。 |
| ims-org | `clientContext.getOrgId()`を使用して自動的に取得されるIMS組織ID。 |
| api-key | `clientContext.getApiKey()`を使用して自動的に取り込まれたAPIキー。 |

>[!TIP]
>
>[ノートブックデータの制限](#notebook-data-limits)セクション内のScalaテーブルを確認し、`mode`を`interactive`に設定するか`batch`に設定するかを判断します。

### ローカルデータフレーム{#scala-create-dataframe}を作成

Scalaを使用してローカル・データ・フレームを作成するには、SQLクエリが必要です。 次に例を示します。

```scala
sparkdf.createOrReplaceTempView("sparkdf")

val localdf = spark.sql("SELECT * FROM sparkdf LIMIT 1)
```

### [!DNL ExperienceEvent]データのフィルタ{#scala-experienceevent}

Scalaノートブックの[!DNL ExperienceEvent]データセットにアクセスしてフィルタするには、データセットID(`{DATASET_ID}`)、組織のIMS ID、および特定の時間範囲を定義するフィルタルールを指定する必要があります。 時間範囲のフィルタリングは、`spark.sql()` 関数を使用して定義します。関数パラメータは SQL クエリ文字列です。

次のセルでは、[!DNL ExperienceEvent]データセットを、2019年1月1日から2019年12月31日末の間にのみ存在するデータにフィルターします。

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

このドキュメントでは、JupyterLabノートブックを使用したデータセットへのアクセスに関する一般的なガイドラインを説明します。 データセットのクエリに関する詳細な例については、JupyterLabノートブック](./query-service.md)のドキュメントの[クエリサービスを参照してください。 データセットの調査と視覚化の方法について詳しくは、[ノートブック](./analyze-your-data.md)を使用したデータの分析のドキュメントを参照してください。

## [!DNL Query Service] {#optional-sql-flags-for-query-service}のオプションのSQLフラグ

次の表に、[!DNL Query Service]に使用できるオプションのSQLフラグを示します。

| **フラグ** | **説明** |
| --- | --- |
| `-h`、`--help` | ヘルプメッセージを表示し、終了します。 |
| `-n`、`--notify` | 通知のオプションを切り替えてクエリ結果を通知します。 |
| `-a`、`--async` | このフラグを使用すると、クエリが非同期的に実行され、カーネルの実行中にクエリを解放できます。変数にクエリ結果を割り当てる場合、クエリが完了しない場合は定義されない可能性があるので、注意が必要です。 |
| `-d`、`--display` | このフラグを使用すると、結果が表示されなくなります。 |
