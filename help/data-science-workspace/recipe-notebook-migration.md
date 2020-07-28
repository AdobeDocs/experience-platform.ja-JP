---
keywords: Experience Platform;Data Science Workspace;popular topics
solution: Experience Platform
title: レシピとノートブックの移行ガイド
topic: Tutorial
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '3311'
ht-degree: 1%

---


# レシピとノートブックの移行ガイド

>[!NOTE]
>ノートブックと [!DNL Python]/Rを使用したレシピは影響を受けません。 移行は、PySpark/[!DNL Spark] (2.3)のレシピとノートブックにのみ適用されます。

次のガイドは、既存のレシピとノートブックの移行に必要な手順と情報を示しています。

- [レシピ移行ガイド](#recipe-migration)
- [ノートブック移行ガイド](#notebook-migration)

## レシピ移行ガイド {#recipe-migration}

最近の変更では、既存のレシピとPySparkレシピを更新する [!DNL Data Science Workspace] 必要 [!DNL Spark] があります。 レシピの移行に役立つワークフローは、次のとおりです。

- [Spark移行ガイド](#spark-migration-guide)
   - [データセットの読み取り/書き込み方法の変更](#read-write-recipe-spark)
   - [サンプルレシピのダウンロード](#download-sample-spark)
   - [ド追加ッカーファイル](#add-dockerfile-spark)
   - [依存関係の確認](#change-dependencies-spark)
   - [ドッカースクリプトの準備](#prepare-docker-spark)
   - [ドッカー付きレシピの作成](#create-recipe-spark)
- [PySpark移行ガイド](#pyspark-migration-guide)
   - [データセットの読み取り/書き込み方法の変更](#pyspark-read-write)
   - [サンプルレシピのダウンロード](#pyspark-download-sample)
   - [ド追加ッカーファイル](#pyspark-add-dockerfile)
   - [ドッカースクリプトの準備](#pyspark-prepare-docker)
   - [ドッカー付きレシピの作成](#pyspark-create-recipe)

## [!DNL Spark] 移行ガイド {#spark-migration-guide}

ビルド手順で生成されるレシピアーティファクトは、.jarバイナリファイルを含むDockerイメージになりました。 また、 [!DNL Platform] SDKを使用したデータセットの読み取りと書き込みに使用される構文が変更され、レシピコードを変更する必要があります。

次のビデオは、レシピに必要な変更点を理解しやすいように設計されてい [!DNL Spark] ます。

>[!VIDEO](https://video.tv.adobe.com/v/33243)

### データセットの読み取りと書き込み([!DNL Spark]) {#read-write-recipe-spark}

Dockerイメージを構築する前に、以下の節に示す、 [!DNL Platform] SDKでのデータセットの読み取りと書き込みの例を確認してください。 既存のレシピを変換する場合は、 [!DNL Platform] SDKコードを更新する必要があります。

#### データセットの読み取り

この節では、Adobeが提供する [helper.scalaの例を使用して、データセットの読み取りに必要な変更点について説明します](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/scala/src/main/scala/com/adobe/platform/ml/helper/Helper.scala) 。

**データセットを読み取る古い方法**

```scala
 var df = sparkSession.read.format("com.adobe.platform.dataset")
    .option(DataSetOptions.orgId, orgId)
    .option(DataSetOptions.serviceToken, serviceToken)
    .option(DataSetOptions.userToken, userToken)
    .option(DataSetOptions.serviceApiKey, apiKey)
    .load(dataSetId)
```

**データセットを読み取る新しい方法**

レシピの更新を行う場合は、 [!DNL Spark] 多数の値を追加および変更する必要があります。 1つ `DataSetOptions` 目は、使用されなくなりました。 Replace `DataSetOptions` with `QSOption`. また、新しい `option` パラメーターが必要です。 との両方 `QSOption.mode` が必要 `QSOption.datasetId` です。 最後に、 `orgId` およびに変更 `serviceApiKey` する必要があ `imsOrg` り `apiKey`ます。 データセットの読み取りに関する比較については、次の例を参照してください。

```scala
import com.adobe.platform.query.QSOption
var df = sparkSession.read.format("com.adobe.platform.query")
  .option(QSOption.userToken", {userToken})
  .option(QSOption.serviceToken, {serviceToken})
  .option(QSOption.imsOrg, {orgId})
  .option(QSOption.apiKey, {apiKey})
  .option(QSOption.mode, "interactive")
  .option(QSOption.datasetId, {dataSetId})
  .load()
```

>[!TIP]
> クエリが10分を超えて実行されている場合、インタラクティブモードはタイムアウトします。 数ギガバイトを超えるデータを取り込む場合は、「バッチ」モードに切り替えることをお勧めします。 バッチモードでは、開始までに時間がかかりますが、大きなデータセットを処理できます。

#### データセットへの書き込み

ここでは、Adobeが提供するScoringDataSaver.scalaの [例を使用してデータセットを書き込む際に必要な変更点について説明します](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/scala/src/main/scala/com/adobe/platform/ml/ScoringDataSaver.scala) 。

**データセットの従来の書き方**

```scala
df.write.format("com.adobe.platform.dataset")
    .option(DataSetOptions.orgId, orgId)
    .option(DataSetOptions.serviceToken, serviceToken)
    .option(DataSetOptions.userToken, userToken)
    .option(DataSetOptions.serviceApiKey, apiKey)
    .save(scoringResultsDataSetId)
```

**データセットの新しい書き方**

レシピの更新を行う場合は、 [!DNL Spark] 多数の値を追加および変更する必要があります。 1つ `DataSetOptions` 目は、使用されなくなりました。 Replace `DataSetOptions` with `QSOption`. また、新しい `option` パラメーターが必要です。 `QSOption.datasetId` が必要で、でを読み込む必要が `{dataSetId}` ありま `.save()`す。 最後に、 `orgId` およびに変更 `serviceApiKey` する必要があ `imsOrg` り `apiKey`ます。 データセットの書き込みに関する比較については、次の例を参照してください。

```scala
import com.adobe.platform.query.QSOption
df.write.format("com.adobe.platform.query")
  .option(QSOption.userToken", {userToken})
  .option(QSOption.serviceToken, {serviceToken})
  .option(QSOption.imsOrg, {orgId})
  .option(QSOption.apiKey, {apiKey})
  .option(QSOption.datasetId, {dataSetId})
  .save()
```

### Dockerベースのソースファイルのパッケージ化([!DNL Spark]) {#package-docker-spark}

開始するには、レシピのあるディレクトリに移動します。

以下の節では、 [Data Science WorkspaceのパブリックGithubリポジトリにある新しいScala Retail Salesレシピを使用します](https://github.com/adobe/experience-platform-dsw-reference)。

### サンプルレシピのダウンロード([!DNL Spark]) {#download-sample-spark}

サンプルレシピには、既存のレシピにコピーする必要のあるファイルが含まれています。 すべてのサンプルレシピを含むパブリックGithubをコピーするには、ターミナルで次のように入力します。

```BASH
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

Scalaレシピは次のディレクトリにあり `experience-platform-dsw-reference/recipes/scala/retail`ます。

### 追加Dockerfile ([!DNL Spark]) {#add-dockerfile-spark}

ドッカーベースのワークフローを使用するには、レシピフォルダーに新しいファイルが必要です。 にあるrecipesフォルダからDockerファイルをコピーして貼り付け `experience-platform-dsw-reference/recipes/scala/Dockerfile`ます。 必要に応じて、以下のコードをコピーして、という名前の新しいファイルに貼り付けることもでき `Dockerfile`ます。

>[!IMPORTANT]
> 以下に示すjarファイルの例 `ml-retail-sample-spark-*-jar-with-dependencies.jar` は、レシピのjarファイルの名前に置き換える必要があります。

```scala
FROM adobe/acp-dsw-ml-runtime-spark:0.0.1

COPY target/ml-retail-sample-spark-*-jar-with-dependencies.jar /application.jar
```

### 依存関係の変更([!DNL Spark]) {#change-dependencies-spark}

既存のレシピを使用している場合、依存関係の変更はpom.xmlファイルで必要です。 モデルオーサリングSDKの依存関係バージョンを2.0.0に変更します。次に、pomファイルの [!DNL Spark] バージョンを2.4.3に更新し、Scalaバージョンを2.11.12に更新します。

```json
<groupId>com.adobe.platform.ml</groupId>
<artifactId>authoring-sdk_2.11</artifactId>
<version>2.0.0</version>
<classifier>jar-with-dependencies</classifier>
```

### Dockerスクリプトの準備([!DNL Spark]) {#prepare-docker-spark}

[!DNL Spark] レシピでは、バイナリアーティファクトを使用せず、Dockerイメージを作成する必要があります。 まだ実行していない場合は、Dockerを [ダウンロードしてインストールします](https://www.docker.com/products/docker-desktop)。

提供されているScalaサンプルレシピのスクリプトは、にあり `login.sh` ま `build.sh` す `experience-platform-dsw-reference/recipes/scala/` 。 これらのファイルをコピーして、既存のレシピに貼り付けます。

フォルダー構造は次の例のようになります（新しく追加されたファイルがハイライト表示されます）。

![フォルダ構造](./images/migration/scala-folder.png)

次の手順は、 [パッケージソースファイルをレシピチュートリアルに従って操作します](./models-recipes/package-source-files-recipe.md) 。 このチュートリアルには、Scala(Spark)レシピ用のドッカー画像の作成について概説した節があります。 完了すると、対応するイメージURLと共にAzureコンテナレジストリにDockerイメージが提供されます。

### レシピの作成 ([!DNL Spark]) {#create-recipe-spark}

レシピを作成するには、まず [パッケージソースファイルのチュートリアルを完了し](./models-recipes/package-source-files-recipe.md) 、ドッカーイメージのURLを準備する必要があります。 UIまたはAPIを使用してレシピを作成できます。

UIを使用してレシピを作成するには、Scalaのパッケージ済みレシピの [読み込み(UI)](./models-recipes/import-packaged-recipe-ui.md) チュートリアルに従ってください。

APIを使用してレシピを作成するには、Scalaのパッケージ済みレシピの [読み込み(API)](./models-recipes/import-packaged-recipe-api.md) チュートリアルに従ってください。

## PySpark移行ガイド {#pyspark-migration-guide}

ビルド手順で生成されるレシピアーティファクトは、.eggバイナリファイルを含むDocker画像になりました。 また、 [!DNL Platform] SDKを使用したデータセットの読み取りと書き込みに使用される構文が変更され、レシピコードを変更する必要があります。

次のビデオは、PySparkのレシピに必要な変更点をより理解しやすくするように設計されています。

>[!VIDEO](https://video.tv.adobe.com/v/33048?learn=on&quality=12)

### 読み取り/書き込みデータセット(PySpark) {#pyspark-read-write}

Dockerイメージを構築する前に、以下の節に示す、 [!DNL Platform] SDKでのデータセットの読み取りと書き込みの例を確認してください。 既存のレシピを変換する場合は、 [!DNL Platform] SDKコードを更新する必要があります。

#### データセットの読み取り

この節では、Adobeが提供する [helper.pyの例を使用してデータセットを読み取る際に必要な変更点について説明します](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/pyspark/pysparkretailapp/helper.py) 。

**データセットを読み取る古い方法**

```python
dataset_options = get_dataset_options(spark.sparkContext)
pd = spark.read.format("com.adobe.platform.dataset") 
  .option(dataset_options.serviceToken(), service_token) 
  .option(dataset_options.userToken(), user_token) 
  .option(dataset_options.orgId(), org_id) 
  .option(dataset_options.serviceApiKey(), api_key)
  .load(dataset_id)
```

**データセットを読み取る新しい方法**

レシピの更新を行う場合は、 [!DNL Spark] 多数の値を追加および変更する必要があります。 1つ `DataSetOptions` 目は、使用されなくなりました。 Replace `DataSetOptions` with `qs_option`. また、新しい `option` パラメーターが必要です。 との両方 `qs_option.mode` が必要 `qs_option.datasetId` です。 最後に、 `orgId` およびに変更 `serviceApiKey` する必要があ `imsOrg` り `apiKey`ます。 データセットの読み取りに関する比較については、次の例を参照してください。

```python
qs_option = spark_context._jvm.com.adobe.platform.query.QSOption
pd = sparkSession.read.format("com.adobe.platform.query") 
  .option(qs_option.userToken, {userToken}) 
  .option(qs_option.serviceToken, {serviceToken}) 
  .option(qs_option.imsOrg, {orgId}) 
  .option(qs_option.apiKey, {apiKey}) 
  .option(qs_option.mode, "interactive") 
  .option(qs_option.datasetId, {dataSetId}) 
  .load()
```

>[!TIP]
> クエリが10分を超えて実行されている場合、インタラクティブモードはタイムアウトします。 数ギガバイトを超えるデータを取り込む場合は、「バッチ」モードに切り替えることをお勧めします。 バッチモードでは、開始までに時間がかかりますが、大きなデータセットを処理できます。

#### データセットへの書き込み

この節では、Adobeが提供する [data_saver.py](https://github.com/adobe/experience-platform-dsw-reference/blob/master/recipes/pyspark/pysparkretailapp/data_saver.py) の例を使ってデータセットを書き込む際に必要な変更点について説明します。

**データセットの従来の書き方**

```python
df.write.format("com.adobe.platform.dataset")
  .option(DataSetOptions.orgId, orgId)
  .option(DataSetOptions.serviceToken, serviceToken)
  .option(DataSetOptions.userToken, userToken)
  .option(DataSetOptions.serviceApiKey, apiKey)
  .save(scoringResultsDataSetId)
```

**データセットの新しい書き方**

PySparkレシピの更新を行うには、多数の値を追加および変更する必要があります。 1つ `DataSetOptions` 目は、使用されなくなりました。 Replace `DataSetOptions` with `qs_option`. また、新しい `option` パラメーターが必要です。  `qs_option.datasetId` が必要で、でを読み込む必要が `{dataSetId}` あり `.save()` ます。 最後に、 `orgId` およびに変更 `serviceApiKey` する必要があ `imsOrg` り `apiKey`ます。 データセットの読み取りに関する比較については、次の例を参照してください。

```python
qs_option = spark_context._jvm.com.adobe.platform.query.QSOption
scored_df.write.format("com.adobe.platform.query") 
  .option(qs_option.userToken, {userToken}) 
  .option(qs_option.serviceToken, {serviceToken}) 
  .option(qs_option.imsOrg, {orgId}) 
  .option(qs_option.apiKey, {apiKey}) 
  .option(qs_option.datasetId, {dataSetId}) 
  .save()
```

### Dockerベースのソースファイルをパッケージ化(PySpark) {#pyspark-package-docker}

開始するには、レシピのあるディレクトリに移動します。

この例では、新しいPySpark Retail Salesレシピが使用され、 [Data Science WorkspaceパブリックGithubリポジトリにあります](https://github.com/adobe/experience-platform-dsw-reference)。

### サンプルレシピ(PySpark)をダウンロードします。 {#pyspark-download-sample}

サンプルレシピには、既存のレシピにコピーする必要のあるファイルが含まれています。 すべてのサンプルレシピを含むパブリック [!DNL Github] のクローンを作成するには、ターミナルで以下を入力します。

```BASH
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

PySparkレシピは次のディレクトリにあり `experience-platform-dsw-reference/recipes/pyspark`ます。

### 追加Dockerfile (PySpark) {#pyspark-add-dockerfile}

ドッカーベースのワークフローを使用するには、レシピフォルダーに新しいファイルが必要です。 にあるrecipesフォルダからDockerファイルをコピーして貼り付け `experience-platform-dsw-reference/recipes/pyspark/Dockerfile`ます。 必要に応じて、下のコードをコピー&amp;ペーストし、という名前の新しいファイルを作成することもでき `Dockerfile`ます。

>[!IMPORTANT]
> 次に示す例のeggファイル `pysparkretailapp-*.egg` は、レシピのeggファイルの名前に置き換えてください。

```scala
FROM adobe/acp-dsw-ml-runtime-pyspark:0.0.1
RUN mkdir /recipe

COPY . /recipe

RUN cd /recipe && \
    ${PYTHON} setup.py clean install && \
    rm -rf /recipe

RUN cp /databricks/conda/envs/${DEFAULT_DATABRICKS_ROOT_CONDA_ENV}/lib/python3.6/site-packages/pysparkretailapp-*.egg /application.egg
```

### Dockerスクリプトの準備(PySpark) {#pyspark-prepare-docker}

PySparkレシピは、Binary Artifactsを使用せず、代わりにDockerイメージを作成する必要があります。 まだ実行していない場合は、 [Dockerをダウンロードしてインストールします](https://www.docker.com/products/docker-desktop)。

PySparkのサンプルレシピには、スクリプトが含まれ `login.sh` ていて、にあ `build.sh` り `experience-platform-dsw-reference/recipes/pyspark` ます。 これらのファイルをコピーして、既存のレシピに貼り付けます。

フォルダー構造は次の例のようになります（新しく追加されたファイルがハイライト表示されます）。

![フォルダ構造](./images/migration/folder.png)

これで、Dockerイメージを使用してレシピを作成する準備が整いました。 次の手順は、 [パッケージソースファイルをレシピチュートリアルに従って操作します](./models-recipes/package-source-files-recipe.md) 。 このチュートリアルには、PySpark(Spark 2.4)レシピ用のドッカーイメージの作成について概説した節があります。 完了すると、対応するイメージURLと共にAzureコンテナレジストリにDockerイメージが提供されます。

### レシピの作成(PySpark) {#pyspark-create-recipe}

レシピを作成するには、まず [パッケージソースファイルのチュートリアルを完了し](./models-recipes/package-source-files-recipe.md) 、ドッカーイメージのURLを準備する必要があります。 UIまたはAPIを使用してレシピを作成できます。

UIを使ってレシピを作成するには、PySpark用のパッケージ済みレシピの [読み込み(UI)](./models-recipes/import-packaged-recipe-ui.md) チュートリアルに従ってください。

APIを使ってレシピを作るには、PySpark用のパッケージレシピの [読み込み(API)](./models-recipes/import-packaged-recipe-api.md) チュートリアルに従ってください。

## ノートブック移行ガイド {#notebook-migration}

ノートブックに対する最近の変更では、既存のPySparkと [!DNL JupyterLab] 2.3のノートブックを2.4に更新する必要があります。この変更に伴い、新しいスターターノートブック [!DNL Spark][!DNL JupyterLab Launcher] が更新されました。 ノートブックの変換手順を説明するガイドを表示するには、次のガイドのいずれかを選択します。

- [PySpark 2.3から2.4への移行ガイド](#pyspark-notebook-migration)
- [Spark 2.3からSpark 2.4(Scala)への移行ガイド](#spark-notebook-migration)

次のビデオは、に必要な変更点を理解しやすくするために設計されていま [!DNL JupyterLab Notebooks]す。

>[!VIDEO](https://video.tv.adobe.com/v/33444?quality=12&learn=on)

## PySpark 2.3から2.4へのノートブック移行ガイド {#pyspark-notebook-migration}

PySpark 2.4がに導入され [!DNL JupyterLab Notebooks]ると、PySpark 2.4を搭載した新しい [!DNL Python] ノートブックは、PySpark 3カーネルではなく [!DNL Python] 3カーネルを使うようになります。 つまり、PySpark 2.3上で動作する既存のコードはPySpark 2.4ではサポートされていません。

>[!IMPORTANT]
>
>PySpark 2.3は非推奨となり、以降のリリースで削除されるように設定されています。 既存の例はすべてPySpark 2.4の例に置き換えるように設定されています。

既存のPySpark 3 ([!DNL Spark][!DNL Spark] 2.3)ノートを2.4に変換するには、次の例に従います。

### カーネル

PySpark 3 ([!DNL Spark] 2.4)ノートブックは、PySpark 3 （Spark 2.3 — 非推奨）ノートブックで使われている非推奨のPySparkカーネルの代わりに、Python 3カーネルを使います。

UIでカーネルを確認または変更するには、ノートブックの右上のナビゲーションバーにあるカーネルボタンを選択します。 [!DNL JupyterLab] あらかじめ定義された起動ノートブックの1つを使用している場合、カーネルは事前に選択されています。 以下の例では、PySpark 3 ([!DNL Spark] 2.4) *Aggregation* Notebookスターターを使用しています。

![カーネルのチェック](./images/migration/pyspark-migration/check-kernel.png)

ドロップダウンメニューを選択すると、利用可能なカーネルのリストが開きます。

![カーネルを選択](./images/migration/pyspark-migration/kernel-click.png)

![カーネルドロップダウン](./images/migration/pyspark-migration/select-kernel.png)

PySpark 3 ([!DNL Spark] 2.4)ノートブックの場合は、Python 3カーネルを選択し、 **Select** ボタンをクリックして確認します。

![カーネルの確認](./images/migration/pyspark-migration/confirm-kernel.png)

## sparkSessionの初期化

すべての2.4ノートブック [!DNL Spark] で、新しいボイラープレートコードを使用してセッションを初期化する必要があります。

<table>
  <th>ノート</th>
  <th>PySpark 3 （[!DNL Spark] 2.3 — 非推奨）</th>
  <th>PySpark 3 ([!DNL Spark] 2.4)</th>
  <tr>
  <th>カーネル</th>
  <td align="center">PySpark 3</td>
  <td align="center">Python 3</td>
  </tr>
  <tr>
  <th>コード</th>
  <td>
  <pre class="JSON language-JSON hljs">
  [!DNL spark]
</pre>
  </td>
  <td>
  <pre class="JSON language-JSON hljs">
from pyspark.sql import SparkSessionspark = SparkSession.builder.getOrCreate()
</pre>
  </td>
  </tr>
</table>

次の図は、PySpark 2.3とPySpark 2.4の設定の違いを示しています。この例では、で提供されている *Aggregation* starterノートブックを使用し [!DNL JupyterLab Launcher]ます。

**2.3（非推奨）の設定例**

![config 1](./images/migration/pyspark-migration/2.3-config.png)![config 2](./images/migration/pyspark-migration/2.3-config-import.png)

**2.4の設定例**

![config 3](./images/migration/pyspark-migration/2.4-config.png)

## %dataset magicの使用 {#magic}

2.4の導入に伴い、新しいPySpark 3 ( [!DNL Spark] 2.4)ノートブック( `%dataset`[!DNL Spark][!DNL Python] 3カーネル)で使用するカスタムマジックが提供されます。

**用途**

`%dataset {action} --datasetId {id} --dataFrame {df}`

**説明**

ノートブック( [!DNL Data Science Workspace][!DNL Python][!DNL Python] 3カーネル)からデータセットを読み取ったり書き込んだりするためのカスタムマジックコマンド。

- **{action}**: データセットに対して実行するアクションのタイプ。 「読み取り」と「書き込み」の2つのアクションを使用できます。
- **—datasetId {id}**: 読み取りまたは書き込みを行うデータセットのIDを指定するために使用します。 これは必須の引数です。
- **—dataFrame {df}**: パンダのデータフレーム。 これは必須の引数です。
   - アクションが「read」の場合、{df}は、データセット読み取り操作の結果を利用できる変数です。
   - アクションが「書き込み」の場合、このデータフレーム{df}はデータセットに書き込まれます。
- **—mode（オプション）**: 使用できるパラメーターは、「batch」および「interactive」です。 デフォルトでは、モードは「interactive」に設定されています。 大量のデータを読み取る場合は、「バッチ」モードを使用することをお勧めします。

**例**

- **次の例を読みます**。 `%dataset read --datasetId 5e68141134492718af974841 --dataFrame pd0`
- **次に例を示します**。 `%dataset write --datasetId 5e68141134492718af974842 --dataFrame pd0`

## LocalContextのデータ・フレームにロード

2.4の導入に伴い [!DNL Spark] 、 [`%dataset`](#magic) カスタムマジックが提供されます。 次の例は、PySpark ([!DNL Spark] 2.3)とPySpark ([!DNL Spark] 2.4 )ノートブックにデータフレームをロードする際の主な違いを示しています。

**PySpark 3 ([!DNL Spark]2.3 — 非推奨)を使う — PySpark 3カーネル**

```python
dataset_options = sc._jvm.com.adobe.platform.dataset.DataSetOptions
pd0 = spark.read.format("com.adobe.platform.dataset")
  .option(dataset_options.orgId(), "310C6D375BA5248F0A494212@AdobeOrg")
  .load("5e68141134492718af974844")
```

**PySpark 3 ([!DNL Spark]2.4)の使用 — Python 3カーネル**

```python
%dataset read --datasetId 5e68141134492718af974844 --dataFrame pd0
```

| 要素 | 説明 |
| ------- | ----------- |
| pd0 | 使用または作成するパンダのデータフレームオブジェクトの名前。 |
| [%dataset](#magic) | 3カーネルでのデータアクセスに関するカスタムマジック [!DNL Python] 。 |

次の図は、PySpark 2.3とPySpark 2.4のデータのロードに関する主な違いを示しています。この例では、で提供されている *Aggregation*[!DNL JupyterLab Launcher]starterノートブックを使用します。

**PySpark 2.3 （Lumaデータセット）でのデータのロード — 廃止**

![ロード1](./images/migration/pyspark-migration/2.3-load.png)

**PySpark 2.4でのデータのロード（Lumaデータセット）**

PySpark 3 (Spark 2.4) `sc = spark.sparkContext` はロード時に定義されます。

![ロード1](./images/migration/pyspark-migration/2.4-load.png)

**PySpark 2.3での[!DNL Experience Cloud Platform]データのロード — 廃止**

![負荷2](./images/migration/pyspark-migration/2.3-load-alt.png)

**PySpark 2.4での[!DNL Experience Cloud Platform]データのロード**

PySpark 3 ([!DNL Spark] 2.4)では、とを定義する必要はありま `org_id``dataset_id` せん。 また、データセット `df = spark.read.format` の読み取りと書き込みを容易にするカスタムマジック [`%dataset`](#magic) に置き換えられました。

![負荷2](./images/migration/pyspark-migration/2.4-load-alt.png)

| 要素 | 説明 |
| ------- | ----------- |
| [%dataset](#magic) | 3カーネルでのデータアクセスに関するカスタムマジック [!DNL Python] 。 |

>[!TIP]
>
>—modeは、またはに設定でき `interactive` ま `batch`す。 —modeのデフォルトはで `interactive`す。 大量のデータを読み取る場合は、 `batch` モードを使用することをお勧めします。

## ローカルデータフレームの作成

PySpark 3 ([!DNL Spark] 2.4)では、 `%%` sparkmagicはサポートされなくなりました。 次の操作は、使用できなくなりました。

- `%%help`
- `%%info`
- `%%cleanup`
- `%%delete`
- `%%configure`
- `%%local`

次の表に、 `%%sql` SparkMagicクエリの変換に必要な変更点を示します。

<table>
  <th>ノート</th>
  <th>PySpark 3 （[!DNL Spark] 2.3 — 非推奨）</th>
  <th>PySpark 3 ([!DNL Spark] 2.4)</th>
  <tr>
  <th>カーネル</th>
  <td align="center">PySpark 3</td>
  <td align="center">[!DNL Python] 3</td>
  </tr>
  <tr>
  <th>コード</th>
      <td>
         <pre class="JSON language-JSON hljs">%%sql -o dfselect * from sparkdf
</pre>
         <pre class="JSON language-JSON hljs"> %%sql -o df -n limitselect * from sparkdf
</pre>
         <pre class="JSON language-JSON hljs">%%sql -o df -qselect * from sparkdf
</pre>
         <pre class="JSON language-JSON hljs"> %%sql -o df -r fractionselect * from sparkdf
</pre>
      </td>
      <td>
         <pre class="JSON language-JSON hljs">
df = spark.sql(" SELECT * FROM sparkdf")
</pre>
         <pre class="JSON language-JSON hljs">
df = spark.sql(" SELECT * FROM sparkdf LIMIT")
</pre>
         <pre class="JSON language-JSON hljs">
df = spark.sql(" SELECT * FROM sparkdf LIMIT")
</pre>
         <pre class="JSON language-JSON hljs">
sample_df = df.sample(fraction)
</pre>
      </td>
   </tr>
</table>

>[!TIP]
>
>また、オプションのシードサンプル(boolean withReplacement、重複分数、長いシードなど)を指定することもできます。

次の図は、PySpark 2.3とPySpark 2.4でローカル・データ・フレームを作成する際の主な違いを示しています。この例では、に用意されている *Aggregation*[!DNL JupyterLab Launcher]starterノートを使用しています。

**ローカル・データ・フレームPySpark 2.3の作成 — 非推奨**

![dataframe 1](./images/migration/pyspark-migration/2.3-dataframe.png)

**ローカルデータフレームPySpark 2.4の作成**

PySpark 3 ([!DNL Spark] 2.4)では `%%sql` Sparkmagicはサポートされなくなり、次の形式に置き換えられました。

![dataframe 2](./images/migration/pyspark-migration/2.4-dataframe.png)

## データセットへの書き込み

2.4の導入に伴い、カスタムマジックが提供され [!DNL Spark][`%dataset`](#magic) 、書き込みデータセットがよりクリーンになります。 データセットに書き込むには、次の [!DNL Spark] 2.4の例を使用します。

**PySpark 3 ([!DNL Spark]2.3 — 非推奨)を使う — PySpark 3カーネル**

```python
userToken = spark.sparkContext.getConf().get("spark.yarn.appMasterEnv.USER_TOKEN")
serviceToken = spark.sparkContext.getConf().get("spark.yarn.appMasterEnv.SERVICE_TOKEN")
serviceApiKey = spark.sparkContext.getConf().get("spark.yarn.appMasterEnv.SERVICE_API_KEY")

dataset_options = sc._jvm.com.adobe.platform.dataset.DataSetOptions

pd0.write.format("com.adobe.platform.dataset")
  .option(dataset_options.orgId(), "310C6D375BA5248F0A494212@AdobeOrg")
  .option(dataset_options.userToken(), userToken)
  .option(dataset_options.serviceToken(), serviceToken)
  .option(dataset_options.serviceApiKey(), serviceApiKey)
  .save("5e68141134492718af974844")
```

**PySpark 3 ([!DNL Spark]2.4)を使う —[!DNL Python]3カーネル**

```python
%dataset write --datasetId 5e68141134492718af974844 --dataFrame pd0
pd0.describe()
pd0.show(10, False)
```

| 要素 | 説明 |
| ------- | ----------- |
| pd0 | 使用または作成するパンダのデータフレームオブジェクトの名前。 |
| [%dataset](#magic) | 3カーネルでのデータアクセスに関するカスタムマジック [!DNL Python] 。 |

>[!TIP]
>
>—modeは、またはに設定でき `interactive` ま `batch`す。 —modeのデフォルトはで `interactive`す。 大量のデータを読み取る場合は、 `batch` モードを使用することをお勧めします。

次の図は、PySpark 2.3とPySpark 2.4にデータを書き戻す際の主な違いを示しています。この例では、に用意されている [!DNL Platform] Aggregation ** starterノートブックを使用し [!DNL JupyterLab Launcher]ます。

**PySpark 2.3へのデータの書き込み[!DNL Platform]— 廃止**

![dataframe 1](./images/migration/pyspark-migration/2.3-write.png)![](./images/migration/pyspark-migration/2.3-write-2.png)![dataframe 1dataframe 1](./images/migration/pyspark-migration/2.3-write-3.png)

**PySpark 2.4へのデータの書き込み[!DNL Platform]**

PySpark 3 ([!DNL Spark] 2.4)を使えば、 `%dataset` カスタムマジックは `userToken`、 `serviceToken`、 `serviceApiKey`、、などの値を定義する必要をなくしま `.option`す。 また、を定義する必要 `orgId` はありません。

![dataframe 2](./images/migration/pyspark-migration/2.4-write.png)![dataframe 2](./images/migration/pyspark-migration/2.4-write-2.png)

## [!DNL Spark] 2.3 ～ [!DNL Spark] 2.4 (Scala)ノートブック移行ガイド {#spark-notebook-migration}

2.4の導入に伴い、既存の [!DNL Spark] ( [!DNL JupyterLab Notebooks]2.3)ノートブックは、 [!DNL Spark][!DNL Spark][!DNL Spark] カーネルの代わりにScalaカーネルを使用しています。 つまり、 [!DNL Spark] ([!DNL Spark] 2.3)で実行されている既存のコードはScala([!DNL Spark] 2.4)ではサポートされていません。 また、新しい [!DNL Spark] ノートブックはすべて、Scala ([!DNL Spark] 2.4)を使用する必要があり [!DNL JupyterLab Launcher]ます。

>[!IMPORTANT]
>
>[!DNL Spark] ([!DNL Spark] 2.3)は非推奨となり、以降のリリースで削除されるように設定されています。 既存の例はすべてScala ([!DNL Spark] 2.4)の例に置き換えるように設定されています。

既存の [!DNL Spark] ([!DNL Spark] 2.3)ノートブックをScala ([!DNL Spark] 2.4)に変換するには、次の例に従います。

## カーネル

Scala (Spark 2.4)ノートブックは、Scala Kernelを使用します。Scala Kernelは、 [!DNL Spark] ( [!DNL Spark][!DNL Spark] 2.3 — 非推奨)ノートブックで使用されている廃止されたカーネルではなく、使用されています。

UIでカーネルを確認または変更するには、ノートブックの右上のナビゲーションバーにあるカーネルボタンを選択します。 [!DNL JupyterLab] [ *Select Kernel* ]ポーバーが表示されます。 あらかじめ定義された起動ノートブックの1つを使用している場合、カーネルは事前に選択されています。 次の例では、のScala *Clustering* ノートブックを使用し [!DNL JupyterLab Launcher]ています。

![カーネルのチェック](./images/migration/spark-scala/scala-kernel.png)

ドロップダウンメニューを選択すると、利用可能なカーネルのリストが開きます。

![カーネルドロップダウン](./images/migration/spark-scala/select-dropdown.png)

![カーネルを選択](./images/migration/spark-scala/dropdown.png)

Scala (Spark 2.4)ノートブックの場合は、Scalaカーネルを選択し、 **Select** （選択）ボタンをクリックして確認します。

![カーネルの確認](./images/migration/spark-scala/select.png)

## SparkSessionを初期化しています {#initialize-sparksession-scala}

すべてのScala ([!DNL Spark] 2.4)ノートブックでは、次のボイラープレートコードを使用してセッションを初期化する必要があります。

<table>
  <th>ノート</th>
  <th>Spark （[!DNL Spark] 2.3 — 非推奨）</th>
  <th>Scala ([!DNL Spark] 2.4)</th>
  <tr>
  <th>カーネル</th>
  <td align="center">[!DNL Spark]</td>
  <td align="center">Scala</td>
  </tr>
  <tr>
  <th>code</th>
  <td align="center">
  コードは不要です
  </td>
  <td>
  <pre class="JSON language-JSON hljs">
org.apache.spark.sqlを読み込みます。{ SparkSession }val spark = SparkSession .builder() .master("local") .getOrCreate()
</pre>
  </td>
  </tr>
</table>

以下のScala ([!DNL Spark] 2.4)イメージは、 [!DNL Spark] 2.3カーネルと2.4 Scalaカーネルを使用してsparkSessionを初期化する際の主な違いを強調して [!DNL Spark][!DNL Spark] います。 この例では、に用意されている *Clustering* starterノートブックを使用し [!DNL JupyterLab Launcher]ます。

**[!DNL Spark]([!DNL Spark]2.3 — 非推奨)**

[!DNL Spark] ([!DNL Spark] 2.3 — 非推奨)は [!DNL Spark] カーネルを使うので、定義する必要はありませんでした [!DNL Spark]。

**Scala ([!DNL Spark]2.4)**

2.4をScalaカーネルと組み合わせて使う場合、読み込み [!DNL Spark] や書き込みを行うた `val spark``SparkSesson` めに、次のように定義してインポートする必要があります。

![sparkの読み込みと定義](./images/migration/spark-scala/start-session.png)

## クエリデータ

Scala([!DNL Spark] 2.4)の `%%` sparkmagicはサポートされなくなりました。 次の操作は、使用できなくなりました。

- `%%help`
- `%%info`
- `%%cleanup`
- `%%delete`
- `%%configure`
- `%%local`

次の表に、 `%%sql` SparkMagicクエリの変換に必要な変更点を示します。

<table>
  <th>ノート</th>
  <th>[!DNL Spark] （[!DNL Spark] 2.3 — 非推奨）</th>
  <th>Scala ([!DNL Spark] 2.4)</th>
  <tr>
  <th>カーネル</th>
  <td align="center">[!DNL Spark]</td>
  <td align="center">Scala</td>
  </tr>
  <tr>
  <th>code</th>
    <td>
       <pre class="JSON language-JSON hljs">
%%sql -o dfselect * from sparkdf
</pre>
         <pre class="JSON language-JSON hljs">
%%sql -o df -n limitselect * from sparkdf
</pre>
         <pre class="JSON language-JSON hljs">
%%sql -o df -qselect * from sparkdf
</pre>
         <pre class="JSON language-JSON hljs">
%%sql -o df -r fractionselect * from sparkdf
</pre>
      </td>
      <td>
         <pre class="JSON language-JSON hljs">
val df = spark.sql(" SELECT * FROM sparkdf")
</pre>
         <pre class="JSON language-JSON hljs">
val df = spark.sql(" SELECT * FROM sparkdf LIMIT")
</pre>
         <pre class="JSON language-JSON hljs">
val df = spark.sql(" SELECT * FROM sparkdf LIMIT")
</pre>
         <pre class="JSON language-JSON hljs">
val sample_df = df.sample(fraction) </pre>
      </td>
   </tr>
</table>

以下のScala([!DNL Spark] 2.4)の図は、2.3カーネルとSpark [!DNL Spark][!DNL Spark] 2.4 Scalaカーネルとのクエリの主な違いを示しています。 この例では、に用意されている *Clustering* starterノートブックを使用し [!DNL JupyterLab Launcher]ます。

**[!DNL Spark]([!DNL Spark]2.3 — 非推奨)**

( [!DNL Spark] 2.3 — 廃止)ノートブックは[!DNL Spark][!DNL Spark] カーネルを使用します。 カー [!DNL Spark] ネルはスパークマジックをサポートし、使用し `%%sql` ます。

![](./images/migration/spark-scala/sql-2.3.png)

**Scala ([!DNL Spark]2.4)**

Scalaカーネルは、 `%%sql` sparkmagicをサポートしなくなりました。 既存のスパークマジックコードを変換する必要があります。

![sparkの読み込みと定義](./images/migration/spark-scala/sql-2.4.png)

## データセットの読み取り {#notebook-read-dataset-spark}

2. [!DNL Spark] 3では、データの読み取りやコードセルでの生の値の使用に使用する `option` 値に対して変数を定義する必要がありました。 Scalaでは、を使用して値を宣言して返すこ `sys.env("PYDASDK_IMS_USER_TOKEN")` とができるので、などの変数を定義する必要がありません `var userToken`。 以下のScala(Spark 2.4)の例では、データセットの読み取りに必要なすべての値を定義して返すために `sys.env` 使用されています。

**Using[!DNL Spark]([!DNL Spark]2.3 — 非推奨) -[!DNL Spark]Kernel**

```scala
import com.adobe.platform.dataset.DataSetOptions
var df1 = spark.read.format("com.adobe.platform.dataset")
  .option(DataSetOptions.orgId, "310C6D375BA5248F0A494212@AdobeOrg")
  .option(DataSetOptions.batchId, "dbe154d3-197a-4e6c-80f8-9b7025eea2b9")
  .load("5e68141134492718af974844")
```

**Scala ([!DNL Spark]2.4)の使用 — Scalaカーネル**

```scala
import org.apache.spark.sql.{Dataset, SparkSession}
val spark = SparkSession.builder().master("local").getOrCreate()
val df1 = spark.read.format("com.adobe.platform.query")
  .option("user-token", sys.env("PYDASDK_IMS_USER_TOKEN"))
  .option("ims-org", sys.env("IMS_ORG_ID"))
  .option("api-key", sys.env("PYDASDK_IMS_CLIENT_ID"))
  .option("service-token", sys.env("PYDASDK_IMS_SERVICE_TOKEN"))
  .option("mode", "interactive")
  .option("dataset-id", "5e68141134492718af974844")
  .load()
```

| element | 説明 |
| ------- | ----------- |
| df1 | データの読み取りと書き込みに使用されるPandasデータフレームを表す変数です。 |
| user-token | を使用して自動的に取得されるユーザートークン `sys.env("PYDASDK_IMS_USER_TOKEN")`。 |
| service-token | を使用して自動的にフェッチされるサービストークン `sys.env("PYDASDK_IMS_SERVICE_TOKEN")`。 |
| ims-org | を使用して自動的に取得されるims-org IDで `sys.env("IMS_ORG_ID")`す。 |
| api-key | を使用して自動的に取得されるAPIキー `sys.env("PYDASDK_IMS_CLIENT_ID")`。 |

次の画像では、2.3と2.4を使用したデータの読み込みに関する主な違いを示して [!DNL Spark] います。この例では、で提供されている [!DNL Spark] Clustering *starterノートブックを使用して*[!DNL JupyterLab Launcher]います。

**[!DNL Spark]([!DNL Spark]2.3 — 非推奨)**

( [!DNL Spark] 2.3 — 廃止)ノートブックは[!DNL Spark][!DNL Spark] カーネルを使用します。 次の2つのセルは、(2019-3-21, 2019-3-29)という日付範囲で、指定したデータセットIDを持つデータセットを読み込む例を示しています。

![spark 2.3のロード](./images/migration/spark-scala/load-2.3.png)

**Scala ([!DNL Spark]2.4)**

Scala ([!DNL Spark] 2.4)ノートブックはScalaカーネルを使用します。Scalaカーネルは、最初のコードセルで強調表示されているように、セットアップ時に必要な値を増やします。 さらに、 `var mdata` 入力する値を増やす必要があり `option` ます。 このノートブックでは、前述のSparkSessionを [初期化するコードがコードのセルに含まれ](#initialize-sparksession-scala)`var mdata` ます。

![spark 2.4のロード](./images/migration/spark-scala/load-2.4.png)

>[!TIP]
>
>Scalaでは、を使用して、内部から値 `sys.env()` を宣言して返すことができ `option`ます。 これにより、変数が1回しか使用されないことがわかっている場合に、変数を定義する必要がなくなります。 次の例では、上の例 `val userToken` を取り込み、インラインで宣言し `option`ます。
> 
```scala
> .option("user-token", sys.env("PYDASDK_IMS_USER_TOKEN"))
> ```

## データセットへの書き込み

データセットの [読み取りと同様](#notebook-read-dataset-spark)、データセットに書き込むには、以下の例で説明する追加の `option` 値が必要です。 Scalaでは、を使用して値を宣言して返すこ `sys.env("PYDASDK_IMS_USER_TOKEN")` とができるので、などの変数を定義する必要がありません `var userToken`。 以下のScalaの例では、 `sys.env` を使用して、データセットへの書き込みに必要なすべての値を定義し、返します。

**Using[!DNL Spark]([!DNL Spark]2.3 — 非推奨) -[!DNL Spark]Kernel**

```scala
import com.adobe.platform.dataset.DataSetOptions

var userToken = spark.sparkContext.getConf.getOption("spark.yarn.appMasterEnv.USER_TOKEN").get
var serviceToken = spark.sparkContext.getConf.getOption("spark.yarn.appMasterEnv.SERVICE_TOKEN").get
var serviceApiKey = spark.sparkContext.getConf.getOption("spark.yarn.appMasterEnv.SERVICE_API_KEY").get

df1.write.format("com.adobe.platform.dataset")
  .option(DataSetOptions.orgId, "310C6D375BA5248F0A494212@AdobeOrg")
  .option(DataSetOptions.userToken, userToken)
  .option(DataSetOptions.serviceToken, serviceToken)
  .option(DataSetOptions.serviceApiKey, serviceApiKey)
  .save("5e68141134492718af974844")
```

**Scala ([!DNL Spark]2.4)の使用 — Scalaカーネル**

```scala
import org.apache.spark.sql.{Dataset, SparkSession}

val spark = SparkSession.builder().master("local").getOrCreate()

df1.write.format("com.adobe.platform.query")
  .option("user-token", sys.env("PYDASDK_IMS_USER_TOKEN"))
  .option("service-token", sys.env("PYDASDK_IMS_SERVICE_TOKEN"))
  .option("ims-org", sys.env("IMS_ORG_ID"))
  .option("api-key", sys.env("PYDASDK_IMS_CLIENT_ID"))
  .option("mode", "interactive")
  .option("dataset-id", "5e68141134492718af974844")
  .save()
```

| element | 説明 |
| ------- | ----------- |
| df1 | データの読み取りと書き込みに使用されるPandasデータフレームを表す変数です。 |
| user-token | を使用して自動的に取得されるユーザートークン `sys.env("PYDASDK_IMS_USER_TOKEN")`。 |
| service-token | を使用して自動的にフェッチされるサービストークン `sys.env("PYDASDK_IMS_SERVICE_TOKEN")`。 |
| ims-org | を使用して自動的に取得されるims-org IDで `sys.env("IMS_ORG_ID")`す。 |
| api-key | を使用して自動的に取得されるAPIキー `sys.env("PYDASDK_IMS_CLIENT_ID")`。 |