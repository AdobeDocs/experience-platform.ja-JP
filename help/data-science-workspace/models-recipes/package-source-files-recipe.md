---
keywords: Experience Platform;package source files;Data Science Workspace;popular topics
solution: Experience Platform
title: ソースファイルのレシピへのパッケージ化
topic: Tutorial
translation-type: tm+mt
source-git-commit: 91c7b7e285a4745da20ea146f2334510ca11b994

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

レシピ作成開始と、アーカイブファイルを作成するためにソースファイルをパッケージ化する。 ソースファイルは、機械学習のロジックとアルゴリズムを定義し、Python、R、PySpark、またはScala Sparkで書かれています。 ソースファイルが書き込まれる言語に応じて、構築されたアーカイブファイルはDockerイメージまたはバイナリファイルになります。 パッケージ化されたアーカイブファイルは、構築後、Data Science Workspaceに読み込まれ、UIまたはAPI [を使用して](./import-packaged-recipe-ui.md) 、レシピ [を作成します](./import-packaged-recipe-api.md)。

### Dockerベースのモデルオーサリング

Dockerイメージを使用すると、開発者は、ライブラリや他の依存関係など、必要なすべてのパーツを含むアプリケーションをパッケージ化し、1つのパッケージとして出荷できます。

作成されたDockerコンテナは、レシピ作成ワークフローの間に提供された資格情報を使用してAzure Image Registryにプッシュされます。

>[!NOTE] **Python**、 **R**、Tensorflowで書き込まれたソースファイルの **みが** Azureコンテナレジストリ資格情報を必要とします。

Azure Experience Platformにログインして、Azureコンテナレジストリの資格情報を <a href="https://platform.adobe.com" target="_blank">取得します</a>。 左側のナビゲーション列で、「 **ワークフロー**」 「ソース **ファイルからレシピを読み込み**」を選択し、 **新しい読み込み手順を** 「開始」します。 以下のスクリーンショットを参照してください。

![](../images/models-recipes/package-source-files/workflow_ss.png)

適切なレシピ **名(例：**「小売販売レシピ」)を入力し、オプションで説明またはドキュメントURLを入力します。 完了したら、「次へ」をクリ **ックしま**&#x200B;す。

![](../images/models-recipes/package-source-files/recipe_info.png)

適切なランタイム **を選択**&#x200B;し、「タイプ」 **で「分類** 」を選 **択します**。 Azureコンテナレジストリ資格情報が生成されます。

![](../images/models-recipes/package-source-files/recipe_workflow_recipe_source.png)

「 **Docker Host**」、「 **Username**」、「 **Password」の値をメモし**&#x200B;ます。 これらは、後でDockerイメージの作成とプッシュに使用されます。

プッシュすると、ユーザーや他のユーザーはURLを使用して画像にアクセスできます。 「 **Source File** 」フィールドには、このURLが入力として含まれている必要があります。

### バイナリベースのモデルオーサリング

ScalaまたはPySparkで書き込まれたソースファイルの場合、バイナリファイルが生成されます。 バイナリファイルの作成は、提供されたビルドスクリプトを実行するのと同じくらい簡単です。
>[!NOTE] ScalaSparkまたはPySparkで書き込まれたソースファイルだけが、ビルドスクリプトの実行時にバイナリファイルを生成します。

### ソースファイルのパッケージ化

開始を作成するには、 <a href="https://github.com/adobe/experience-platform-dsw-reference" target="_blank">Experience Platform Data Science Workspace Referenceリポジトリにあるサンプルコードベースを取得します</a> 。 サンプルソースファイルが書き込まれるプログラミング言語に応じて、それぞれのアーカイブファイルの作成手順が異なります。

- [Python Dockerイメージの作成](#build-python-docker-image)
- [Build R Docker Image](#build-r-docker-image)
- [PySparkバイナリの構築](#build-pyspark-binaries)
- [Scalaバイナリの構築](#build-scala-binaries)

#### Python Dockerイメージの作成

まだ実行していない場合は、次のコマンドを使用して、githubリポジトリをローカルシステムにコピーします。

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

Navigate to the directory `experience-platform-dsw-reference/recipes/python/retail`. ここには、Dockerにログインし、Python Docker `login.sh` イメ `build.sh` ージを構築するために使用するスクリプトが含まれています。 [Dockerの資格情報を準備できている場合](#docker-based-model-authoring) 、次のコマンドを順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for building Docker image
./build.sh
```

ログインスクリプトを実行する場合、Dockerのホスト、ユーザー名、パスワードを指定する必要があります。 構築時には、Dockerホストとビルドのバージョンタグを指定する必要があります。

ビルドスクリプトが完了すると、コンソール出力にDockerソースファイルのURLが表示されます。 この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retailsales-python:{VERSION_TAG}
```

このURLをコピーして次の手順に進 [みます](#next-steps)。

#### Build R Docker Image

まだ実行していない場合は、次のコマンドを使用して、githubリポジトリをローカルシステムにコピーします。

```BASH
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

複製されたリポジトリ内の `experience-platform-dsw-reference/recipes/R/Retail - GradientBoosting` ディレクトリに移動します。 ここでは、DockerへのログインとR Docker `login.sh` イメ `build.sh` ージの構築に使用するファイルとファイルを見つけます。 [Dockerの資格情報を準備できている場合](#docker-based-model-authoring) 、次のコマンドを順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for build Docker image
./build.sh
```

ログインスクリプトを実行する場合、Dockerのホスト、ユーザー名、パスワードを指定する必要があります。 構築時には、Dockerホストとビルドのバージョンタグを指定する必要があります。

ビルドスクリプトが完了すると、コンソール出力にDockerソースファイルのURLが表示されます。 この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retail-r:{VERSION_TAG}
```

このURLをコピーして次の手順に進 [みます](#next-steps)。

#### PySparkバイナリの構築

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

#### Scalaバイナリの構築

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

## 次の手順

このチュートリアルでは、ソースファイルのレシピへのパッケージ化について説明しました。レシピは、レシピをData Science Workspaceに読み込むための前提条件の手順です。 これで、AzureコンテナレジストリにDockerイメージが、対応するイメージURLまたはバイナリファイルと共に、ファイルシステムにローカルに保存される必要があります。 これで、パッケージ化されたレシピのデータサイエンスワ **ークスペースへの読み込みに関するチュートリアルを開始する準備が整いまし**&#x200B;た。 以下のチュートリアルリンクの1つを選択して、作業を開始してください。

- [パッケージ化されたレシピをUIに読み込む](./import-packaged-recipe-ui.md)
- [APIを使用したパッケージ化されたレシピの読み込み](./import-packaged-recipe-api.md)