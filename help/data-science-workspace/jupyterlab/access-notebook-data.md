---
keywords: Experience Platform;JupyterLab;ノートブック;データ科学ワークスペース;よく読まれるトピック;%データセット;インタラクティブモード;バッチモード;Spark SDK;Python SDK;データにアクセスする。ノートブックのデータアクセス
solution: Experience Platform
title: Jupyterlab Notebook での データ アクセス
description: このガイドでは、Data Science Workspace内に作成された Jupyter Notebooks を使用してデータにアクセスする方法を重点的に説明します。
exl-id: 2035a627-5afc-4b72-9119-158b95a35d32
source-git-commit: 5d98dc0cbfaf3d17c909464311a33a03ea77f237
workflow-type: tm+mt
source-wordcount: '3343'
ht-degree: 23%

---

# [!DNL Jupyterlab] ノートブックでのデータアクセス

>[!NOTE]
>
>Data Science Workspaceは購入できなくなりました。
>
>このドキュメントは、Data Science Workspaceの以前の使用権限を持つ既存のお客様を対象としています。

サポートされる各カーネルは、ノートブック内のデータセットから Platform データを読み取るための組み込み機能を備えてます。現在、Adobe Experience Platform Data Science Workspaceの JupyterLab は、[!DNL Python]、R、PySpark、Scala 用のノートブックをサポートしています。 ただし、データのページ番号付けのサポートは、[!DNL Python] および R ノートブックに制限されています。 このガイドでは、JupyterLab ノートブックを使用してデータにアクセスする方法を重点的に説明します。

## はじめに

このガイドを読む前に、[[!DNL JupyterLab]  ユーザーガイド ](./overview.md) を参照して、[!DNL JupyterLab] の概要とデータサイエンスWorkspace内での役割を確認してください。

## ノートブックのデータ制限 {#notebook-data-limits}

>[!IMPORTANT]
>
>PySpark と Scala ノートブックの場合、「リモート RPC クライアントの関連付けが解除されました」という理由でエラーが表示されます。 これは、通常、ドライバーまたはエグゼキューターのメモリが不足していることを意味します。 このエラーを解決するには [バッチ」モードに切り替えてみてください](#mode) 。

次の情報は、読み取ることができるデータの最大量、使用されたデータの種類、およびデータの読み取りにかかる推定期間を定義します。

[!DNL Python]とRについては、40GBのRAMで構成されたノートブックサーバーがベンチマークに使用されました。PySpark と Scala では、64 GB の RAM、8 コア、2 DBU (最大 4 ワーカー) で構成されたデータブリック クラスターを、以下に概説するベンチマークに使用しました。

使用される ExperienceEvent スキーマ データのサイズは、1,000 行から 10 億行までさまざまでした。 PySpark および [!DNL Spark] メトリックでは、XDMデータに 10 日間の日付範囲が使用されていたことに注意してください。

アドホックスキーマデータは、「テーブル [!DNL Query Service] 選択として作成」（CTAS）を使用して前処理されました。 このデータのサイズも、1,000 (1K) 行から最大 10 億 (1B) 行までさまざまでした。

### バッチモードとインタラクティブモードを使用する場合 {#mode}

PySpark および Scala ノートブックでデータセットを読み取る場合、対話モードまたはバッチ モードを使用してデータセットを読み取るオプションがあります。 インタラクティブは迅速な結果を得るために作られていますが、バッチモードは大規模なデータセット用です。

- PySpark と Scala ノートブックの場合、500 万行以上のデータが読み取られている場合はバッチモードを使用する必要があります。 各モードの効率について詳しくは、以下の [PySpark](#pyspark-data-limits) または [Scala](#scala-data-limits) データ制限テーブルを参照してください。

### [!DNL Python] notebook のデータ制限

**XDM ExperienceEvent スキーマ：** XDM データの最大 200 万行（ディスク上の最大 6.1 GB データ）を 22 分以内に読み取ることができるはずです。 行を追加すると、エラーが発生する場合があります。

| 行数 | 1K | 10K | 100,000 | 1M | 2M |
| ----------------------- | ------ | ------ | ----- | ----- | ----- |
| ディスク上のサイズ （MB） | 18.73 | 187.5 | 308 | 3000 | 6050 |
| SDK (秒) | 20.3 | 86.8 | 63 | 659 | 1315 |

**アドホックスキーマ：** 非 XDM （アドホック）データの最大 500 万行（ディスク上の最大 5.6 GB のデータ）を 14 分以内に読み取ることができるはずです。 行を追加すると、エラーが発生する場合があります。

| 行数 | 1K | 10K | 100,000 | 1M | 2M | 3M | 5M |
| ----------------------- | ------- | ------- | ----- | ----- | ----- | ----- | ------ |
| ディスク上のサイズ (MB) | 1.21 | 11.72 | 115 | 1120 | 2250 | 3380 | 5630 |
| SDK (秒) | 7.27 | 9.04 | 27.3 | 180 | 346 | 487 | 819 |

### R ノートブックのデータ制限

**XDM ExperienceEvent スキーマ：** 最大 100 万行の XDM データ（ディスク上の 3 GB データ）を 13 分以内に読み取ることができるはずです。

| 行数 | 1K | 10キロメートル競走 | 100,000 | 1M |
| ----------------------- | ------ | ------ | ----- | ----- |
| ディスク上のサイズ (MB) | 18.73 | 187.5 | 308 | 3000 |
| R カーネル （秒） | 14.03 | 69.6 | 86.8 | 775 |

**アドホックスキーマ：** 約 10 分で最大 300 万行のアドホックスキーマ（ディスク上の 293 MB のデータ）を読み取れるはずです。

| 行数 | 1K | 10キロメートル競走 | 100,000 | 1M | 2M | 3M |
| ----------------------- | ------- | ------- | ----- | ----- | ----- | ----- |
| ディスク上のサイズ （MB） | 0.082 | 0.612 | 9.0 | 91 | 188 | 293 |
| R SDK （秒） | 7.7 | 4.58 | 35.9 | 233 | 470.5 | 603 |

### PySpark （[!DNL Python] カーネル）ノートブックのデータ制限： {#pyspark-data-limits}

**XDM ExperienceEvent スキーマ：** インタラクティブモードでは、約 20 分で XDM データの最大 500 万行（ディスク上の最大 13.42 GB データ）を読み取れるはずです。 インタラクティブモードでサポートできる行は 500 万行までです。 より大きなデータセットを読み取る場合は、バッチモードに切り替えることをお勧めします。 バッチモードでは、約 14 時間で最大 5 億行（ディスク上の最大 1.31 TB のデータ）の XDM データを読み取ることができます。

| 行数字 | 1K | 10キロメートル競走 | 100,000 | 1M | 2M | 3M | 5M | 10M | 50M | 100M | 500M |
|-------------------------|--------|--------|-------|-------|-------|-------|---------|---------|----------|--------|--------|
| ディスク上のサイズ | 2.93MB | 4.38MB | 29.02 | 2.69GB | 5.39GB | 8.09GB | 13.42GB | 26.82GB | 134.24GB | 268.39GB | 1.31 TB |
| SDK(インタラクティブモード) | 33 秒 | 32.4秒 | 55.1s | 253.5秒 | 489.2秒 | 729.6s | 1206.8 秒 | - | - | - | - |
| SDK （バッチモード） | 815.8 秒 | 492.8 秒 | 379.1s | 637.4秒 | 624.5秒 | 869.2秒 | 1104.1秒 | 1786年代 | 5387.2 秒 | 10624.6s | 50547s |

**広告-hoc スキーマ:** インタラクティブ モードでは、最大 500 万行 (ディスク上に ~5.36 GB のデータ) の非 XDM データを 3 分以内に読み取ることができます。 バッチモードでは、約 18 分で最大 10 億行 (ディスク上に ~1.05 TB のデータ) の非 XDM データを読み取ることができます。

| 行数 | 1K | 10K | 100,000 | 1M | 2M | 3M | 5M | 10M | 50M | 100M | 500メートル | 1B |
|--------------|--------|---------|---------|-------|-------|-------|--------|--------|---------|--------|---------|-------|
| ディスク上のサイズ | 1.12MB | 11.24MB | 109.48MB | 2.69GB | 2.14GB | 3.21GB | 5.36GB | 10.71GB | 53.58GB | 107.52GB | 535.88GB | 1.05 TB |
| SDK インタラクティブモード （秒） | 28.2 秒 | 18.6s | 20.8s | 20.9s | 23.8s | 21.7s | 24.7秒 | - | - | - | - | - |
| SDK バッチモード （秒） | 428.8 秒 | 578.8 秒 | 641.4 秒 | 538.5 秒 | 630.9 秒 | 467.3 秒 | 411s | 675s | 702 秒 | 719.2 秒 | 1022.1s | 1122.3秒 |

### [!DNL Spark] (Scala カーネル)ノートブックのデータ制限: {#scala-data-limits}

**XDM エクスペリエンス イベント スキーマ:** 対話モードでは、約 18 分で最大 500 万行 (ディスク上に ~13.42 GB のデータ) のXDMデータを読み取ることができます。 インタラクティブモードでは、最大 500 万行のみがサポートされます。 より大きなデータセットを読み取る場合は、バッチモードに切り替えることをお勧めします。 バッチモードでは、約14時間で最大5億行(ディスク上に~1.31TBのデータ)のXDMデータを読み取ることができます。

| 行数 | 1K | 10K | 100,000 | 1M | 2M | 3M | 5M | 10M | 50M | 100M | 500M |
|---------------|--------|--------|-------|-------|-------|-------|---------|---------|----------|--------|--------|
| ディスク上のサイズ | 2.93MB | 4.38MB | 29.02 | 2.69GB | 5.39GB | 8.09GB | 13.42GB | 26.82GB | 134.24GB | 268.39GB | 1.31 TB |
| SDK インタラクティブモード (秒) | 37.9秒 | 22.7秒 | 45.6s | 231.7 秒 | 444.7 秒 | 660.6秒 | 1100番台 | - | - | - | - |
| SDK バッチモード （秒） | 374.4 秒 | 398.5 秒 | 527番台 | 487.9 秒 | 588.9 秒 | 829s | 939.1s | 1441s | 5473.2 秒 | 10118.8 | 49207.6 |

**アドホックスキーマ：** インタラクティブモードでは、非 XDM データの最大 500 万行（ディスク上の最大 5.36 GB データ）を 3 分以内に読み取ることができます。 バッチモードでは、約 16 分で最大 10 億行（ディスク上の約 1.05 TB のデータ）の非 XDM データを読み取ることができます。

| 行数字 | 1K | 10K | 100,000 | 1M | 2M | 3M | 5M | 10M | 50M | 100M | 500M | 1B |
|--------------|--------|---------|---------|-------|-------|-------|---------|---------|---------|--------|---------|-------|
| ディスク上のサイズ | 1.12MB | 11.24MB | 109.48MB | 2.69GB | 2.14GB | 3.21GB | 5.36GB | 10.71GB | 53.58GB | 107.52GB | 535.88GB | 1.05 TB |
| SDK インタラクティブモード （秒） | 35.7秒 | 31 秒 | 19.5秒 | 25.3秒 | 23 秒 | 33.2秒 | 25.5秒 | - | - | - | - | - |
| SDK バッチモード （秒） | 448.8秒 | 459.7秒 | 519番台 | 475.8秒 | 599.9秒 | 347.6s | 407.8 秒 | 397s | 518.8 秒 | 487.9秒 | 760.2 秒 | 975.4 秒 |

## Python ノートブック {#python-notebook}

[!DNL Python] ノートブックを使用すると、データセットにアクセスする際に、データにページ番号を付けることができます。 ページネーションを使用してデータを読み取るサンプルコードと使用しないサンプルデータを以下に示します。 使用可能なスターター Python ノートブックについて詳しくは、JupyterLab ユーザーガイドの [[!DNL JupyterLab]  ランチャー ](./overview.md#launcher) の節を参照してください。

以下の Python ドキュメントでは、以下の概念の概要を説明しています。

- [データセットからの読み取り](#python-read-dataset)
- [データセットへの書き込み](#write-python)
- [クエリデータ](#query-data-python)
- [ExperienceEvent データのフィルタリング](#python-filter)

### Python でデータセットから読み取る {#python-read-dataset}

**ページネーションなし：**

次のコードを実行すると、データセット全体が読み取られます。実行が成功した場合、データは `df` 変数で参照される Pandas データフレームとして保存されます。

```python
# Python

from platform_sdk.dataset_reader import DatasetReader
dataset_reader = DatasetReader(get_platform_sdk_client_context(), dataset_id="{DATASET_ID}")
df = dataset_reader.read()
df.head()
```

**ページネーションを使用：**

次のコードを実行すると、指定したデータセットからデータが読み取られます。ページ番号付けは、`limit()` 関数を使用してデータを制限し、`offset()` 関数を使用してデータをオフセットすることで実現します。データの制限とは、読み取るデータポイントの最大数を指し、オフセットとは、データの読み取り前にスキップするデータポイントの数を指します。読み取り操作が正常に実行された場合、データは `df` 変数が参照する Pandas データフレームとして保存されます。

```python
# Python

from platform_sdk.dataset_reader import DatasetReader

dataset_reader = DatasetReader(get_platform_sdk_client_context(), dataset_id="{DATASET_ID}")
df = dataset_reader.limit(100).offset(10).read()
```

### Python でデータセットに書き込む {#write-python}

JupyterLab ノートブックのデータセットに書き込むには、JupyterLab の左ナビゲーションにあるデータアイコンタブ (以下で強調表示) を選択します。 **[!UICONTROL データセット]**&#x200B;ディレクトリと&#x200B;**[!UICONTROL スキーマ]**&#x200B;ディレクトリが表示されます。 [ **[!UICONTROL データセット]** を選択して右クリックし、使用するデータセットのドロップダウンメニューから **[!UICONTROL ノートブックにデータを書き込む]** オプションを選択します。 実行可能コード エントリがノートブックの下部に表示されます。

![](../images/jupyterlab/data-access/write-dataset.png)

- **[!UICONTROL ノートブックにデータを書き込む]**&#x200B;を使用して、選択したデータセットで書き込みセルを生成します。
- **[!UICONTROL ノートブックのデータを調査]** を使用して、選択したデータセットを含む読み取りセルを生成します。
- **[!UICONTROL ノートブックのデータをクエリ]** を使用して、選択したデータセットを含む基本的なクエリセルを生成します。

または、次のコードセルをコピーして貼り付けることもできます。 `{DATASET_ID}`と`{PANDA_DATAFRAME}`の両方置換。

```python
from platform_sdk.models import Dataset
from platform_sdk.dataset_writer import DatasetWriter

dataset = Dataset(get_platform_sdk_client_context()).get_by_id(dataset_id="{DATASET_ID}")
dataset_writer = DatasetWriter(get_platform_sdk_client_context(), dataset)
write_tracker = dataset_writer.write({PANDA_DATAFRAME}, file_format='json')
```

### の [!DNL Query Service] を使用してデータをクエリ [!DNL Python] {#query-data-python}

[!DNL JupyterLab] on [!DNL Platform] を使用すると、 [!DNL Python] ノートブックで SQL を使用して [Adobe Experience Platformクエリ サービス](https://experienceleague.adobe.com/docs/experience-platform/query/home.html?lang=ja)を介してデータにアクセスできます。 [!DNL Query Service] を通じたデータへのアクセスは実行時間が短いので、大規模なデータセットの処理に役立ちます。[!DNL Query Service] を使用したデータのクエリには 10 分間の処理時間制限があることに注意してください。

[!DNL JupyterLab] で [!DNL Query Service] を使用する前に、[[!DNL Query Service] SQL 構文](https://experienceleague.adobe.com/docs/experience-platform/query/sql/syntax.html?lang=ja)について実践的に理解していることを確認してください。

[!DNL Query Service] を使用したデータのクエリには、ターゲットデータセットの名前を指定する必要があります。 必要なコードセルを生成するには、**[!UICONTROL データエクスプローラー]**&#x200B;を使用して目的のデータセットを見つけます。データセットリストを右クリックし、「**[!UICONTROL ノートブックのデータをクエリ]**」をクリックして、ノートブックに 2 つのコードセルを生成します。 これら 2 つのセルについて、以下で詳しく説明します。

![](../images/jupyterlab/data-access/python-query-dataset.png)

[!DNL Query Service] を [!DNL JupyterLab] で利用するには、まず作業中の [!DNL Python] ノートブックと [!DNL Query Service] の間の接続を作成する必要があります。 これは、最初に生成されたセルを実行することで達成できます。

```python
qs_connect()
```

2 番目に生成されたセルでは、最初の行を SQL クエリの前に定義する必要があります。デフォルトでは、生成されたセルは、クエリ結果を Pandas データフレームとして保存するオプションの変数（`df0`）を定義します。<br>`-c QS_CONNECTION` 引数は必須で、[!DNL Query Service] に対して SQL クエリを実行するようにカーネルに指示します。 その他の引数のリストは、[付録](#optional-sql-flags-for-query-service)を参照してください。

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

### データのフィルター [!DNL ExperienceEvent] {#python-filter}

[!DNL Python] ノートブック内の[!DNL ExperienceEvent] データセットにアクセスしてフィルター処理するには、論理演算子を使用して特定の時間範囲を定義するフィルター規則と共に、データセット の ID (`{DATASET_ID}`) を指定する必要があります。時間範囲を定義すると、指定されたページ番号は無視され、データセット全体が考慮されます。

フィルタリング操作のリストを以下に示します。

- `eq()`：と等しい
- `gt()`：より大きい
- `ge()`：より大きいか等しい
- `lt()`：より小さい
- `le()`：より小さいか等しい
- `And()`：論理積演算子
- `Or()`：論理和演算子

次のセルでは、[!DNL ExperienceEvent] データセットを、2019 年 1 月 1 日～2019 年 12 月 31 日（PT）の間にのみ存在するデータにフィルタリングします。

```python
# Python

from platform_sdk.dataset_reader import DatasetReader

dataset_reader = DatasetReader(get_platform_sdk_client_context(), dataset_id="{DATASET_ID}")
df = dataset_reader.\
    where(dataset_reader["timestamp"].gt("2019-01-01 00:00:00").\
    And(dataset_reader["timestamp"].lt("2019-12-31 23:59:59"))\
).read()
```

## R ノートブック {#r-notebooks}

r ノートブックを使用すると、データセットにアクセスする際にデータのページ番号を付けることができます。 ページネーションを使用してデータを読み取るサンプルコードと使用しないサンプルデータを以下に示します。 使用可能なスターター R ノートブックの詳細については、JupyterLab ユーザー ガイド内の [[!DNL JupyterLab] ランチャー](./overview.md#launcher) セクション訪問してください。

以下の R ドキュメントでは、次の概念の概要について説明します。

- [データセットからの読み取り](#r-read-dataset)
- [データセットに書き込む](#write-r)
- [ExperienceEvent データフィルター](#r-filter)

### R のデータセットからの読み取り {#r-read-dataset}

**ページネーションなし：**

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

**ページネーションを使用：**

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

### R のデータセットへの書き込み {#write-r}

JupyterLab ノートブックのデータセットに書き込むには、JupyterLab の左側のナビゲーションにある「データ」アイコンタブ（以下でハイライト表示）を選択します。 **[!UICONTROL データセット]**&#x200B;ディレクトリと&#x200B;**[!UICONTROL スキーマ]**&#x200B;ディレクトリが表示されます。 「**[!UICONTROL データセット]**」を選択して右クリックし、使用するデータセットのドロップダウンメニューから「**[!UICONTROL ノートブックにデータを書き込む]**」オプションを選択します。 ノートブックの下部に実行可能コードエントリが表示されます。

![](../images/jupyterlab/data-access/r-write-dataset.png)

- **[!UICONTROL Notebook にデータを書き込み]** を使用して、選択したデータセットで書き込みセルを生成します。
- **[!UICONTROL ノートブックのデータを調査]** を使用して、選択したデータセットを含む読み取りセルを生成します。

または、次のコードセルをコピーして貼り付けることもできます。

```R
psdk <- import("platform_sdk")
dataset <- psdk$models$Dataset(py$get_platform_sdk_client_context())$get_by_id(dataset_id="{DATASET_ID}")
dataset_writer <- psdk$dataset_writer$DatasetWriter(py$get_platform_sdk_client_context(), dataset)
write_tracker <- dataset_writer$write(df, file_format='json')
```

### [!DNL ExperienceEvent] データのフィルタリング {#r-filter}

R ノートブックの [!DNL ExperienceEvent] データセットにアクセスしてフィルタリングするには、データセット（`{DATASET_ID}`）の ID と、論理演算子を使用して特定の時間範囲を定義するフィルタールールを指定する必要があります。 時間範囲を定義すると、指定されたページ番号は無視され、データセット全体が考慮されます。

フィルタリング操作のリストを以下に示します。

- `eq()`：と等しい
- `gt()`：より大きい
- `ge()`：より大きいか等しい
- `lt()`：より小さい
- `le()`：より小さいか等しい
- `And()`：論理積演算子
- `Or()`：論理和演算子

次のセルフィルター、2019 年 1 月 1 日から 2019 年 12 月 31 日までの間に排他的に存在するデータへの [!DNL ExperienceEvent] データセットです。

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

## PySpark 3 ノートブック {#pyspark-notebook}

以下の PySpark のドキュメントでは、次の概念の概要について説明しています。

- [SparkSession を初期化する](#spark-initialize)
- [データの読み取りと書き込み](#magic)
- [ローカルデータフレーム作成](#pyspark-create-dataframe)
- [ExperienceEvent データのフィルタリング](#pyspark-filter-experienceevent)

### sparkSession の初期化 {#spark-initialize}

すべての [!DNL Spark] 2.4 ノートブックでは、次のボイラープレートコードを使用してセッションを初期化する必要があります。

```scala
from pyspark.sql import SparkSession
spark = SparkSession.builder.getOrCreate()
```

### %dataset を使用した PySpark 3 ノートブックの読み取りと書き込み {#magic}

[!DNL Spark] 2.4 の導入に伴い、PySpark 3 （[!DNL Spark] 2.4）ノートブックで使用するた `%dataset` のカスタムマジックが提供されています。 IPython カーネルで利用可能なマジックコマンドについて詳しくは、[IPython マジックドキュメント ](https://ipython.readthedocs.io/en/stable/interactive/magics.html) を参照してください。


**用途**

```scala
%dataset {action} --datasetId {id} --dataFrame {df} --mode batch
```

**説明**

[!DNL PySpark] notebook （[!DNL Python] 3 カーネル）からデータセットを読み取ったり書き込んだりするためのカスタム [!DNL Data Science Workspace] magic コマンド。

| 名前 | 説明 | 必須 |
| --- | --- | --- |
| `{action}` | データセットに対して実行するアクションの種類。 「読み取り」と「書き込み」の 2 つのアクションがあります。 | ○ |
| `--datasetId {id}` | 読み取りまたは書き込みするデータセットの ID を指定するために使用します。 | ○ |
| `--dataFrame {df}` | パンダのデータフレーム。 <ul><li> アクションが &quot;read&quot; の場合、 {df} は、データセット読み取り操作の結果 (データフレームなど) を使用できる変数です。 </li><li> アクションが &quot;書き込み&quot; の場合、このデータフレーム {df} はデータセットに書き込まれます。 </li></ul> | ○ |
| `--mode` | データの読み取り方法を変更する追加のパラメーター。 使用できるパラメーターは、「batch」および「interactive」です。 デフォルトでは、モードは「バッチ」に設定されています。<br> 小さなデータセットでのクエリパフォーマンスを向上させるには、「インタラクティブ」モードを使用することをお勧めします。 | ○ |

>[!TIP]
>
>[notebook data limits](#notebook-data-limits) セクション内の PySpark テーブルを確認して、`mode` を `interactive` または `batch` に設定する必要があるかどうかを判断します。

**例**

- **例を読む**: `%dataset read --datasetId 5e68141134492718af974841 --dataFrame pd0 --mode batch`
- **例を書く**: `%dataset write --datasetId 5e68141134492718af974842 --dataFrame pd0 --mode batch`

>[!IMPORTANT]
>
> データを書き込む前に `df.cache()` を使用してデータをキャッシュすると、ノートブックのパフォーマンスが大幅に向上します。 これは、次のいずれかのエラーを受け取った場合に役立ちます。
> 
> - ステージの失敗によりジョブが中止されました … 各パーティション内の要素数が同一の RDD のみを圧縮できます。
> - リモート RPC クライアントの関連付けが解除され、その他のメモリエラーが発生しました。
> - データセットの読み取り時や書き込み時のパフォーマンスが低下しています。
> 
> 詳しくは、[ トラブルシューティングガイド ](../troubleshooting-guide.md) を参照してください。

JupyterLab buy では、次の方法を使用して、上記の例を自動生成できます。

JupyterLab の左側のナビゲーションにある「データ」アイコンタブ（以下で強調表示）を選択します。 **[!UICONTROL データセット]**&#x200B;ディレクトリと&#x200B;**[!UICONTROL スキーマ]**&#x200B;ディレクトリが表示されます。 「**[!UICONTROL データセット]**」を選択して右クリックし、使用するデータセットのドロップダウンメニューから「**[!UICONTROL ノートブックにデータを書き込む]**」オプションを選択します。 ノートブックの下部に実行可能コードエントリが表示されます。

- **[!UICONTROL ノートブックのデータを調査]** を使用して、読み取りセルを生成します。
- **[!UICONTROL Notebook にデータを書き込む]** を使用して、書き込みセルを生成します。

![](../images/jupyterlab/data-access/pyspark-write-dataset.png)

### ローカルデータフレームの作成 {#pyspark-create-dataframe}

PySpark 3 を使用してローカルデータフレームを作成するには、SQL クエリを使用します。 例：

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
>また、ブール値 withReplacement、double fraction、long seed など、オプションのシードサンプルを指定することもできます。

### [!DNL ExperienceEvent] データのフィルタリング {#pyspark-filter-experienceevent}

PySpark ノートブックの [!DNL ExperienceEvent] データセットへのアクセスとフィルタリングには、データセット ID （`{DATASET_ID}`）、組織の IMS ID、特定の時間範囲を定義するフィルタールールを指定する必要があります。 フィルター時間範囲は、関数 `spark.sql()` を使用して定義します。関数パラメーターは SQL クエリ文字列です。

次のセルでは、[!DNL ExperienceEvent] データセットを、2019 年 1 月 1 日～2019 年 12 月 31 日（PT）の間にのみ存在するデータにフィルタリングします。

```python
# PySpark 3 (Spark 2.4)

from pyspark.sql import SparkSession
spark = SparkSession.builder.getOrCreate()

%dataset read --datasetId {DATASET_ID} --dataFrame df --mode batch

df.createOrReplaceTempView("event")
timepd = spark.sql("""
    SELECT *
    FROM event
    WHERE timestamp > CAST('2019-01-01 00:00:00.0' AS TIMESTAMP)
    AND timestamp < CAST('2019-12-31 23:59:59.9' AS TIMESTAMP)
""")
timepd.show()
```

## Scala ノートブック {#scala-notebook}

以下のドキュメントには、次の概念の例が含まれています。

- [sparkSession の初期化](#scala-initialize)
- [データセットの読み取り](#read-scala-dataset)
- [データセットへの書き込み](#scala-write-dataset)
- [ローカルデータフレーム作成](#scala-create-dataframe)
- [ExperienceEvent データフィルター](#scala-experienceevent)

### SparkSession の初期化 {#scala-initialize}

すべての Scala ノートブックでは、以下のボイラープレートコードを使用してセッションを初期化する必要があります。

```scala
import org.apache.spark.sql.{ SparkSession }
val spark = SparkSession
  .builder()
  .master("local")
  .getOrCreate()
```

### データセットの読み取り {#read-scala-dataset}

Scalaでは、 `clientContext` をインポートしてPlatform値を取得および返すことができるため、 `var userToken`などの変数を定義する必要がなくなります。 以下の Scala の例では、 `clientContext` を使用して、データセットを読み取るために必要なすべての値を取得して返します。

>[!IMPORTANT]
>
> データを書き込む前に `df.cache()` を使用してデータをキャッシュすると、ノートブックのパフォーマンスが大幅に向上します。 これは、次のいずれかのエラーを受け取った場合に役立ちます。
> 
> - ステージの失敗によりジョブが中止されました … 各パーティション内の要素数が同一の RDD のみを圧縮できます。
> - リモート RPC クライアントの関連付けが解除され、その他のメモリエラーが発生しました。
> - データセットの読み取り時や書き込み時のパフォーマンスが低下しています。
> 
> 詳しくは、[ トラブルシューティングガイド ](../troubleshooting-guide.md) を参照してください。

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
  .option("mode", "batch")
  .option("dataset-id", "5e68141134492718af974844")
  .load()

df1.printSchema()
df1.show(10)
```

| 要素 | 説明 |
| ------- | ----------- |
| df1 | データの読み取りと書き込みに使用する Pandas データフレームを表す変数です。 |
| user-token | `clientContext.getUserToken()` を使用して自動的に取得されるユーザートークン。 |
| service-token | `clientContext.getServiceToken()` を使用して自動的に取得されるサービストークン。 |
| ims-org | `clientContext.getOrgId()` を使用して自動的に取得される組織 ID。 |
| api-key | `clientContext.getApiKey()` を使用して自動的に取得される API キー。 |

>[!TIP]
>
>[ノートブックのデータ制限](#notebook-data-limits)セクション内の Scala テーブルを確認して、`mode` を `interactive` または `batch` に設定する必要があるかどうかを判断します。

上記の例は、次の方法を使用して JupyterLab 購入で自動生成できます。

JupyterLab の左ナビゲーションにあるデータアイコンタブ(以下でハイライト表示)を選択します。 **[!UICONTROL データセット]**&#x200B;ディレクトリと&#x200B;**[!UICONTROL スキーマ]**&#x200B;ディレクトリが表示されます。 「**[!UICONTROL データセット]**」を選択して右クリックし、使用するデータセットのドロップダウンメニューから「**[!UICONTROL ノートブックのデータを調査]**」オプションを選択します。実行可能コード エントリがノートブックの下部に表示されます。および
- **[!UICONTROL ノートブックでデータを参照]**&#x200B;を使用して、読み取りセルを生成します。
- **[!UICONTROL Notebook にデータを書き込む]** を使用して、書き込みセルを生成します。

![](../images/jupyterlab/data-access/scala-write-dataset.png)

### データセットへの書き込み {#scala-write-dataset}

Scala では、`clientContext` を読み込んで Platform の値を取得したり返したりできるので、`var userToken` などの変数を定義する必要はありません。 以下の Scala の例では、`clientContext` を使用して、データセットへの書き込みに必要な必要な値をすべて定義して返します。

>[!IMPORTANT]
>
> データを書き込む前に `df.cache()` を使用してデータをキャッシュすると、ノートブックのパフォーマンスが大幅に向上します。 これは、次のいずれかのエラーが発生した場合に役立ちます。
> 
> - ステージの失敗によりジョブが中止されました … 各パーティション内の要素数が同一の RDD のみを圧縮できます。
> - リモート RPC クライアントの関連付けが解除され、その他のメモリエラーが発生しました。
> - データセットの読み取り時や書き込み時のパフォーマンスが低下しています。
> 
> 詳しくは、 [トラブルシューティングガイド](../troubleshooting-guide.md) を参照してください。

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
  .option("mode", "batch")
  .option("dataset-id", "5e68141134492718af974844")
  .save()
```

| 要素 | 説明 |
| ------- | ----------- |
| df1 | データの読み取りと書き込みに使用する Pandas データフレームを表す変数です。 |
| user-token | `clientContext.getUserToken()` を使用して自動的に取得されるユーザートークン。 |
| service-token | `clientContext.getServiceToken()` を使用して自動的に取得されるサービストークン。 |
| ims-org | `clientContext.getOrgId()` を使用して自動的に取得される組織 ID。 |
| api-key | `clientContext.getApiKey()` を使用して自動的に取得された API キー。 |

>[!TIP]
>
>[ノートブックのデータ制限](#notebook-data-limits)セクション内の Scala テーブルを確認して、`mode` を `interactive` または `batch` に設定する必要があるかどうかを判断します。

### ローカルデータフレームを作成する {#scala-create-dataframe}

Scala を使用してローカルデータフレームを作成するには、SQL クエリが必要です。 例：

```scala
sparkdf.createOrReplaceTempView("sparkdf")

val localdf = spark.sql("SELECT * FROM sparkdf LIMIT 1)
```

### [!DNL ExperienceEvent] データのフィルタリング {#scala-experienceevent}

Scala ノートブックの [!DNL ExperienceEvent] データセットにアクセスしてフィルタリングするには、データセット ID （`{DATASET_ID}`）、組織の IMS ID、特定の時間範囲を定義するフィルタールールを指定する必要があります。 時間範囲のフィルタリングは、`spark.sql()` 関数を使用して定義します。関数パラメータは SQL クエリ文字列です。

次のセルでは、[!DNL ExperienceEvent] データセットを、2019 年 1 月 1 日～2019 年 12 月 31 日（PT）の間にのみ存在するデータにフィルタリングします。

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

このドキュメントでは、JupyterLab ノートブックを使用したデータセットへのアクセスに関する一般的なガイドラインを説明しました。 データセットのクエリに関する詳細な例については、 [JupyterLab ノートブックのクエリサービス](./query-service.md) のドキュメント訪問してください。 データセットを探索して視覚化する方法の詳細については、 [ノートブックを使用したデータの分析](./analyze-your-data.md)に関するドキュメント訪問してください。

## オプションの SQL フラグ [!DNL Query Service] {#optional-sql-flags-for-query-service}

次の表に、 [!DNL Query Service]に使用できるオプションの SQL フラグの概要を示します。

| **フラグ** | **説明** |
| --- | --- |
| `-h`、`--help` | ヘルプメッセージを表示し、終了します。 |
| `-n`、`--notify` | 通知のオプションを切り替えてクエリ結果を通知します。 |
| `-a`、`--async` | このフラグを使用すると、クエリが非同期的に実行され、カーネルの実行中にクエリを解放できます。変数にクエリ結果を割り当てる場合、クエリが完了しない場合は定義されない可能性があるので、注意が必要です。 |
| `-d`、`--display` | このフラグを使用すると、結果が表示されなくなります。 |
