---
keywords: Experience Platform;package source files;Data Science Workspace;popular topics
solution: Experience Platform
title: ソースファイルのレシピへのパッケージ化
topic: Tutorial
translation-type: tm+mt
source-git-commit: e08460bc76d79920bbc12c7665a1416d69993f34

---


# ソースファイルのレシピへのパッケージ化

このチュートリアルでは、提供された小売販売のサンプルソースファイルをアーカイブファイルにパッケージ化する方法について説明します。アーカイブファイルは、UIでのレシピの読み込みワークフローに従うか、APIを使用して、Adobe Experience Platform Data Science Workspaceでレシピを作成するのに使用できます。

理解すべき概念：

- **レシピ**: レシピは、アドビのモデル仕様の用語で、トレーニングを受けたモデルを作成して実行し、特定のビジネス上の問題の解決に役立てるために必要な、特定の機械学習、人工知能アルゴリズム、アルゴリズムのアンサンブル、処理ロジック、設定を表す最上位コンテナです。
- **ソースファイル**: プロジェクト内の、レシピのロジックを含む個々のファイル。

## 前提条件

- [ドッカー](https://docs.docker.com/install/#supported-platforms)
- [Python 3およびpip](https://docs.conda.io/en/latest/miniconda.html)
- [スカラ](https://www.scala-sbt.org/download.html?_ga=2.42231906.690987621.1558478883-2004067584.1558478883)
- [Maven](https://maven.apache.org/install.html)

## レシピの作成

レシピ作成開始と、アーカイブファイルを作成するためのソースファイルのパッケージ化。 ソースファイルは、機械学習のロジックとアルゴリズムを定義し、Python、R、PySpark、Scalaで書かれています。 構築されたアーカイブファイルは、Dockerイメージの形式をとります。 パッケージ化されたアーカイブファイルは、構築後、Data Science Workspaceに読み込まれ、UI [またはAPI](./import-packaged-recipe-ui.md)[](./import-packaged-recipe-api.md)を使用してレシピが作成されます。

### Dockerベースのモデルオーサリング {#docker-based-model-authoring}

Dockerイメージを使用すると、開発者は、ライブラリやその他の依存関係など必要なすべての部分を含むアプリケーションをパッケージ化し、それを1つのパッケージとして出荷できます。

作成されたDockerイメージは、レシピ作成ワークフローで指定された資格情報を使用してAzureコンテナレジストリにプッシュされます。

Azureコンテナレジストリ資格情報を取得するには、 <a href="https://platform.adobe.com" target="_blank">Adobe Experience Platformにログインし</a>ます。 左側のナビゲーション列で、に移動し **[!UICONTROL Workflows]**&#x200B;ます。 を選択 **[!UICONTROL Import Recipe]** し、を選択し **[!UICONTROL Launch]**&#x200B;ます。 以下のスクリーンショットを参照してください。

![](../images/models-recipes/package-source-files/import.png)

「 *設定* 」ページが開きます。 適切な *レシピ名を指定します(例：「小売売上高レシピ*」)。オプションで説明またはドキュメントURLを入力します。 完了したら、をクリックし **[!UICONTROL Next]**&#x200B;ます。

![](../images/models-recipes/package-source-files/configure.png)

適切な *Runtime*&#x200B;を選択し、「 **[!UICONTROL Classification]** Type *」で「a」を選択します*。 Azureコンテナレジストリ資格情報は、完了すると生成されます。

>[!NOTE]
>*「タイプ&#x200B;*」は、レシピが設計する機械学習の問題のクラスで、トレーニングの後に使用され、トレーニングの実施状況をカスタマイズするのに役立ちます。

>[!TIP]
>- Pythonレシピの場合は、 **[!UICONTROL Python]** ランタイムを選択します。
>- Rレシピの場合は、 **[!UICONTROL R]** ランタイムを選択します。
>- PySparkレシピの場合は、 **[!UICONTROL PySpark]** ランタイムを選択します。 アーティファクトタイプが自動入力されます。
>- Scalaレシピの場合は、 **[!UICONTROL Spark]** ランタイムを選択します。 アーティファクトタイプが自動入力されます。


![](../images/models-recipes/package-source-files/docker-creds.png)

「 *Docker Host*」、「 *Username*」、「 *Password*」の値をメモしておきます。 これらは、以下に説明するワークフローで、Dockerイメージを作成およびプッシュするために使用されます。

>[!NOTE]
>ソースURLは、次の手順を完了した後に提供されます。 設定ファイルについては、 [次の手順で説明する後続のチュートリアルで説明します](#next-steps)。

### ソースファイルのパッケージ化

Experience Platform Data Science Workspace Reference <a href="https://github.com/adobe/experience-platform-dsw-reference" target="_blank"></a> リポジトリにあるサンプルコードベースを取得して開始します。

- [Python Dockerイメージの作成](#python-docker)
- [Build R Dockerイメージ](#r-docker)
- [PySpark Dockerイメージの構築](#pyspark-docker)
- [Scala (Spark) Dockerイメージの構築](#scala-docker)

### Python Dockerイメージの作成 {#python-docker}

まだ作成していない場合は、次のコマンドを使用して、githubリポジトリをローカルシステムにコピーします。

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

Navigate to the directory `experience-platform-dsw-reference/recipes/python/retail`. ここでは、スクリプトを見つけて、Dockerにログインし `login.sh` てPython Dockerイメージを作成するために `build.sh` 使用します。 [Docker資格情報の準備ができている場合は](#docker-based-model-authoring) 、次のコマンドを順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for building Docker image
./build.sh
```

ログインスクリプトを実行する場合は、Dockerホスト、ユーザー名、パスワードを指定する必要があります。 構築時には、Dockerホストと、構築用のバージョンタグを指定する必要があります。

ビルドスクリプトが完了すると、コンソール出力にDockerソースファイルのURLが与えられます。 この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retailsales-python:{VERSION_TAG}
```

このURLをコピーして、 [次の手順に進みます](#next-steps)。

### Build R Dockerイメージ {#r-docker}

まだ作成していない場合は、次のコマンドを使用して、githubリポジトリをローカルシステムにコピーします。

```BASH
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

クローン・リポジトリ `experience-platform-dsw-reference/recipes/R/Retail - GradientBoosting` 内のディレクトリに移動します。 ここでは、Dockerにログインし、R Dockerイメージを作成するた `login.sh` めに使用するファイル `build.sh` とファイルを見つけます。 [Docker資格情報の準備ができている場合は](#docker-based-model-authoring) 、次のコマンドを順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for build Docker image
./build.sh
```

ログインスクリプトを実行する場合は、Dockerホスト、ユーザー名、パスワードを指定する必要があります。 構築時には、Dockerホストと、構築用のバージョンタグを指定する必要があります。

ビルドスクリプトが完了すると、コンソール出力にDockerソースファイルのURLが与えられます。 この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retail-r:{VERSION_TAG}
```

このURLをコピーして、 [次の手順に進みます](#next-steps)。

### PySpark Dockerイメージの構築 {#pyspark-docker}

次のコマンドを使用して、githubリポジトリをローカル・システム上にクローンし、開始します。

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

Navigate to the directory `experience-platform-dsw-reference/recipes/pyspark/retail`. スクリプト `login.sh` とスクリプト `build.sh` はここにあり、DockerにログインしてDockerイメージを作成する際に使用します。 [Docker資格情報の準備ができている場合は](#docker-based-model-authoring) 、次のコマンドを順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for building Docker image
./build.sh
```

ログインスクリプトを実行する場合は、Dockerホスト、ユーザー名、パスワードを指定する必要があります。 構築時には、Dockerホストと、構築用のバージョンタグを指定する必要があります。

ビルドスクリプトが完了すると、コンソール出力にDockerソースファイルのURLが与えられます。 この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retailsales-pyspark:{VERSION_TAG}
```

このURLをコピーして、 [次の手順に進みます](#next-steps)。

### Scala Dockerイメージの作成 {#scala-docker}

ターミナルで次のコマンドを使用して、githubリポジトリをローカルシステムにクローンし、開始します。

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

次に、スクリプトとを検索でき `experience-platform-dsw-reference/recipes/scala/retail` るディレクトリに移動 `login.sh` し `build.sh`ます。 これらのスクリプトは、DockerにログインしてDockerイメージを作成するために使用されます。 [Docker資格情報の準備ができている場合は](#docker-based-model-authoring) 、次のコマンドを端末に順番に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for building Docker image
./build.sh
```

ログインスクリプトを実行する場合、Dockerホスト、ユーザー名、パスワードを指定する必要があります。 構築時には、Dockerホストと、構築用のバージョンタグを指定する必要があります。

ビルドスクリプトが完了すると、コンソール出力にDockerソースファイルのURLが与えられます。 この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retailsales-spark:{VERSION_TAG}
```

このURLをコピーして、 [次の手順に進みます](#next-steps)。

## 次の手順 {#next-steps}

このチュートリアルでは、ソースファイルのレシピへのパッケージ化について、Data Science Workspaceにレシピをインポートする前提条件の手順を説明しました。 これで、Azureコンテナレジストリに、対応するイメージURLと共にDockerイメージが存在するはずです。 これで、パッケージ化されたレシピをData Science Workspaceに読み込む方法のチュートリアルを開始する準備が整いました。 開始するには、次のチュートリアルリンクの1つを選択してください。

- [パッケージ化されたレシピをUIに読み込む](./import-packaged-recipe-ui.md)
- [APIを使用したパッケージ化されたレシピの読み込み](./import-packaged-recipe-api.md)

## バイナリの構築（非推奨）

>[!CAUTION]
> バイナリは新しいPySparkおよびScalaレシピではサポートされておらず、将来のリリースで削除されるように設定されています。 PySparkとScalaを使用する場合は、 [Dockerワークフローに従ってください](#docker-based-model-authoring) 。 次のワークフローは、Spark 2.3レシピのみに適用できます。

### PySparkバイナリの構築（非推奨）

まだ作成していない場合は、次のコマンドを使用して、githubリポジトリをローカルシステムにコピーします。

```BASH
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

ローカルシステム上のクローン・リポジトリに移動し、次のコマンドを実行してPySparkレシピをインポートするために必要な `.egg` ファイルを作成します。

```BASH
cd recipes/pyspark
./build.sh
```

フ `.egg` ァイルがフォルダーに生成され `dist` ます。

次の手順に進むことができ [ます](#next-steps)。

#### Scalaバイナリの構築（廃止）

まだGitHubリポジトリをローカルシステムにコピーしていない場合は、次のコマンドを実行します。

```BASH
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

Scalaレシピのインポートに使用するアーティファクトを作成するには、クローン・リポジトリに移動し、次の手順に従います。 `.jar`

```BASH
cd recipes/scala/
./build.sh
```

依存関係を持つ生成された `.jar` アーティファクトが `/target` ディレクトリに見つかります。

次の手順に進むことができ [ます](#next-steps)。