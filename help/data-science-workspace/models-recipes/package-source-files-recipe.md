---
keywords: Experience Platform;package source files;Data Science Workspace;popular topics
solution: Experience Platform
title: ソースファイルのレシピへのパッケージ化
topic: Tutorial
translation-type: tm+mt
source-git-commit: 4001e4fd6a2e04a04e7ea594175d9e3e5c8a00d6

---


# ソースファイルのレシピへのパッケージ化

このチュートリアルでは、提供されたRetail Salesサンプルソースファイルをアーカイブファイルにパッケージ化する方法について説明します。アーカイブファイルは、UIのレシピのインポートワークフローに従うか、APIを使用して、Adobe Experience Platform Data Science Workspaceでレシピを作成するのに使用できます。

理解すべき概念：

- **レシピ**:レシピは、アドビのモデル仕様を表す用語で、トップレベルのコンテナで、トレーニングを受けたモデルを作成して実行し、特定のビジネス問題の解決に役立つ、特定の機械学習、人工知能アルゴリズム、アルゴリズムのアンサンブルを表します。
- **ソースファイル**:プロジェクト内の、レシピのロジックを含む個々のファイル。

## 前提条件

- [ドッカー](https://docs.docker.com/install/#supported-platforms)
- [Python 3およびpip](https://docs.conda.io/en/latest/miniconda.html)
- [スカラ](https://www.scala-sbt.org/download.html?_ga=2.42231906.690987621.1558478883-2004067584.1558478883)
- [Maven](https://maven.apache.org/install.html)

## レシピの作成

レシピ作成開始と、アーカイブファイルを作成するためにソースファイルをパッケージ化する。 ソースファイルは、機械学習のロジックとアルゴリズムを定義し、Python、R、PySpark、Scalaのいずれかで書かれています。 構築されたアーカイブファイルは、Dockerイメージの形式をとります。 パッケージ化されたアーカイブファイルは、構築後、Data Science Workspaceに読み込まれ、UIまたはAPI [を使用して](./import-packaged-recipe-ui.md) 、レシピ [を作成します](./import-packaged-recipe-api.md)。

### Dockerベースのモデルオーサリング

Dockerイメージを使用すると、開発者は、ライブラリや他の依存関係など、必要なすべてのパーツを含むアプリケーションをパッケージ化し、1つのパッケージとして出荷できます。

構築されたDockerイメージは、レシピ作成ワークフローで提供された資格情報を使用してAzureコンテナレジストリにプッシュされます。

Azure Experience Platformにログインして、Azureコンテナレジストリの資格情報を <a href="https://platform.adobe.com" target="_blank">取得します</a>。 左側のナビゲーション列で、「 **ワークフロー**」 「レシピ **の読み込み** 」を選択し、「起動」を **選択します**。 以下のスクリーンショットを参照してください。

![](../images/models-recipes/package-source-files/import.png)

「設定 ** 」ページが開きます。 適切なレシピ **名(例：**「小売販売レシピ」)を入力し、オプションで説明またはドキュメントURLを入力します。 完了したら、「次へ」をクリ **ックしま**&#x200B;す。

![](../images/models-recipes/package-source-files/configure.png)

適切なランタイム *を選択*&#x200B;し、「タイプ」で **「分類** 」を選 *択します*。 Azureコンテナレジストリの資格情報は、完了すると生成されます。

>[!NOTE]
>*「タイプ&#x200B;*」は、レシピが設計される機械学習の問題のクラスで、トレーニングの後に使用され、トレーニングの実行状況の評価に役立ちます。

>[!TIP]
>- Pythonレシピの場合は、 **Pythonランタイム** を選択します。
>- Rレシピの場合は、 **Rランタイム** を選択します。
>- PySparkレシピの場合は、 **PySparkランタイムを選択します** 。 アーティファクトタイプが自動入力されます。
>- Scalaレシピの場合は、 **Spark** Runtimeを選択します。 アーティファクトタイプが自動入力されます。


![](../images/models-recipes/package-source-files/docker-creds.png)

「 *Docker Host*」、「 *Username*」、「 *Password」の値をメモし*&#x200B;ます。 これらは、以下に説明するワークフローで、Dockerイメージを作成およびプッシュするために使用されます。

>[!NOTE]
>ソースURLは、以下の手順を完了した後に提供されます。 設定ファイルについては、次の手順で見つかる以降のチュートリアルで説 [明していま](#next-steps)す。

### ソースファイルのパッケージ化

開始を作成するには、 <a href="https://github.com/adobe/experience-platform-dsw-reference" target="_blank">Experience Platform Data Science Workspace Referenceリポジトリにあるサンプルコードベースを取得します</a> 。

- [Python Dockerイメージの作成](#python-docker)
- [Build R Docker Image](#r-docker)
- [PySpark Dockerイメージの構築](#pyspark-docker)
- [Scala (Spark) Docker画像のビルド](#scala-docker)

### Python Dockerイメージの作成 {#python-docker}

まだ実行していない場合は、次のコマンドを使用して、githubリポジトリをローカルシステムにコピーします。

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

Navigate to the directory `experience-platform-dsw-reference/recipes/python/retail`. ここでは、Dockerにログインし、Python Docker `login.sh` イメージを `build.sh` 構築するために使用するスクリプトを見つけます。 [Dockerの資格情報を準備できている場合](#docker-based-model-authoring) 、次のコマンドを順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for building Docker image
./build.sh
```

ログインスクリプトを実行する場合は、Dockerのホスト、ユーザー名、パスワードを指定する必要があります。 構築時には、Dockerホストとビルドのバージョンタグを指定する必要があります。

ビルドスクリプトが完了すると、コンソール出力にDockerソースファイルのURLが表示されます。 この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retailsales-python:{VERSION_TAG}
```

このURLをコピーして次の手順に進 [みます](#next-steps)。

### Build R Docker Image {#r-docker}

まだ実行していない場合は、次のコマンドを使用して、githubリポジトリをローカルシステムにコピーします。

```BASH
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

複製されたリポジトリ内の `experience-platform-dsw-reference/recipes/R/Retail - GradientBoosting` ディレクトリに移動します。 ここでは、Dockerにログインし、R Docker `login.sh` イメ `build.sh` ージを構築する際に使用するファイルとファイルを見つけます。 [Dockerの資格情報を準備できている場合](#docker-based-model-authoring) 、次のコマンドを順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for build Docker image
./build.sh
```

ログインスクリプトを実行する場合は、Dockerのホスト、ユーザー名、パスワードを指定する必要があります。 構築時には、Dockerホストとビルドのバージョンタグを指定する必要があります。

ビルドスクリプトが完了すると、コンソール出力にDockerソースファイルのURLが表示されます。 この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retail-r:{VERSION_TAG}
```

このURLをコピーして次の手順に進 [みます](#next-steps)。

### PySpark Dockerイメージの構築 {#pyspark-docker}

次のコマンドを使用して、githubリポジトリをローカル・システムにクローンし、開始します。

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

Navigate to the directory `experience-platform-dsw-reference/recipes/pyspark/retail`. スクリプトと `login.sh` はこ `build.sh` こにあり、DockerにログインしてDockerイメージを作成するために使用されます。 [Dockerの資格情報を準備できている場合](#docker-based-model-authoring) 、次のコマンドを順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for building Docker image
./build.sh
```

ログインスクリプトを実行する場合は、Dockerのホスト、ユーザー名、パスワードを指定する必要があります。 構築時には、Dockerホストとビルドのバージョンタグを指定する必要があります。

ビルドスクリプトが完了すると、コンソール出力にDockerソースファイルのURLが表示されます。 この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retailsales-pyspark:{VERSION_TAG}
```

このURLをコピーして次の手順に進 [みます](#next-steps)。

### Scala Dockerイメージの作成 {#scala-docker}

開始を行います。

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

次に、スクリプトとを見つ `experience-platform-dsw-reference/recipes/scala/retail` けられるディレクトリに移動 `login.sh` します `build.sh`。 これらのスクリプトは、Dockerにログインし、Dockerイメージを構築するために使用されます。 [Dockerの資格情報を準備できている場合](#docker-based-model-authoring) 、ターミナルに次のコマンドを順番に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for building Docker image
./build.sh
```

ログインスクリプトを実行する場合、Dockerホスト、ユーザー名、パスワードを指定する必要があります。 構築時には、Dockerホストとビルドのバージョンタグを指定する必要があります。

ビルドスクリプトが完了すると、コンソール出力にDockerソースファイルのURLが表示されます。 この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retailsales-spark:{VERSION_TAG}
```

このURLをコピーして次の手順に進 [みます](#next-steps)。

## 次の手順 {#next-steps}

このチュートリアルでは、ソースファイルのレシピへのパッケージ化について説明しました。レシピは、レシピをData Science Workspaceに読み込むための前提条件の手順です。 これで、対応するイメージURLと共にAzureコンテナレジストリにDockerイメージが作成されます。 これで、パッケージ化されたレシピのデータサイエンスワ **ークスペースへの読み込みに関するチュートリアルを開始する準備が整いまし**&#x200B;た。 以下のチュートリアルリンクの1つを選択して、作業を開始してください。

- [パッケージ化されたレシピをUIに読み込む](./import-packaged-recipe-ui.md)
- [APIを使用したパッケージ化されたレシピの読み込み](./import-packaged-recipe-api.md)

## バイナリの構築（廃止）

>[!CAUTION]
> バイナリは新しいPySparkとScalaレシピではサポートされておらず、将来のリリースで削除されるように設定されています。 PySparkとScalaを使う時 [は](#docker-based-model-authoring) 、Dockerワークフローに従ってください。 次のワークフローは、Spark 2.3レシピにのみ適用できます。

### PySparkバイナリを構築する（非推奨）

まだ実行していない場合は、次のコマンドを使用して、githubリポジトリをローカルシステムにコピーします。

```BASH
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

ローカルシステム上の複製されたリポジトリに移動し、次のコマンドを実行して、PySparkレシピを読み込むために必要なフ `.egg` ァイルを作成します。

```BASH
cd recipes/pyspark
./build.sh
```

ファイ `.egg` ルがフォルダーに生成さ `dist` れます。

次の手順に進め [ます](#next-steps)。

#### Scalaバイナリの構築（廃止）

まだローカルシステムにGithubリポジトリを複製していない場合は、次のコマンドを実行します。

```BASH
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

Scalaレシピのインポ `.jar` ートに使用するアーティファクトを作成するには、コピーしたリポジトリに移動し、次の手順に従います。

```BASH
cd recipes/scala/
./build.sh
```

依存関係を持つ `.jar` 生成されたアーティファクトがディレクトリ内で見つ `/target` かります。

次の手順に進め [ます](#next-steps)。