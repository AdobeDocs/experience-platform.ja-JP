---
keywords: Experience Platform;package source files;Data Science Workspace;popular topics
solution: Experience Platform
title: ソースファイルのレシピへのパッケージ化
topic: Tutorial
translation-type: tm+mt
source-git-commit: 45461e3420f3b7e227f80fe775d80b8442a1069c
workflow-type: tm+mt
source-wordcount: '1097'
ht-degree: 0%

---


# ソースファイルのレシピへのパッケージ化

このチュートリアルでは、Retail Salesサンプルソースファイルをアーカイブファイルにパッケージ化する方法について説明します。アーカイブファイルは、UIまたはAPIのレシピインポートワークフローに従ってAdobe Experience Platformでレシピを作成するのに使用します。 [!DNL Data Science Workspace]

理解すべき概念：

- **レシピ**: レシピは、アドビのモデル仕様の用語で、トレーニングを受けたモデルを作成して実行し、特定のビジネス上の問題の解決に役立てるために必要な、特定の機械学習、人工知能アルゴリズム、アルゴリズムのアンサンブル、処理ロジック、設定を表す最上位コンテナです。
- **ソースファイル**: プロジェクト内の、レシピのロジックを含む個々のファイル。

## 前提条件

- [!DNL Docker](https://docs.docker.com/install/#supported-platforms)
- [!DNL Python 3 and pip](https://docs.conda.io/en/latest/miniconda.html)
- [!DNL Scala](https://www.scala-sbt.org/download.html?_ga=2.42231906.690987621.1558478883-2004067584.1558478883)
- [!DNL Maven](https://maven.apache.org/install.html)

## レシピの作成

レシピ作成開始と、アーカイブファイルを作成するためのソースファイルのパッケージ化。 ソースファイルは、機械学習のロジックとアルゴリズムを定義し、現在の特定の問題を解決するために使用され、R、PySpark、 [!DNL Python]Scalaのいずれかに書き込まれます。 構築されたアーカイブファイルは、Dockerイメージの形式をとります。 作成したパッケージアーカイブファイルは、に読み込まれ、UI [!DNL Data Science Workspace] またはAPI [](./import-packaged-recipe-ui.md) を使用してレシピが作成されます [](./import-packaged-recipe-api.md)。

### Dockerベースのモデルオーサリング {#docker-based-model-authoring}

Dockerイメージを使用すると、開発者は、ライブラリやその他の依存関係など必要なすべての部分を含むアプリケーションをパッケージ化し、それを1つのパッケージとして出荷できます。

作成されたDockerイメージは、レシピ作成ワークフローで指定された資格情報を使用してAzureコンテナレジストリにプッシュされます。

Azureコンテナレジストリ資格情報を取得するには、 <a href="https://platform.adobe.com" target="_blank">Adobe Experience Platformにログインします</a>。 左側のナビゲーション列で、 **[!UICONTROL ワークフローに移動します]**。 「 **[!UICONTROL レシピの]** 読み込み **[!UICONTROL 」を選択し、「]**&#x200B;開始」を選択します。 以下のスクリーンショットを参照してください。

![](../images/models-recipes/package-source-files/import.png)

「 *設定* 」ページが開きます。 適切な *レシピ名を指定します(例：「小売売上高レシピ*」)。オプションで説明またはドキュメントURLを入力します。 完了したら、「 **[!UICONTROL 次へ]**」をクリックします。

![](../images/models-recipes/package-source-files/configure.png)

適切な *ランタイムを選択し*、「 **[!UICONTROL タイプ]** 」で「 *分類*」を選択します。 Azureコンテナレジストリ資格情報は、完了すると生成されます。

>[!NOTE]
>*「タイプ&#x200B;*」は、レシピが設計する機械学習の問題のクラスで、トレーニングの後に使用され、トレーニングの実施状況をカスタマイズするのに役立ちます。

>[!TIP]
>- レシピの場合 [!DNL Python] は、 **[!UICONTROL Python]** ランタイムを選択します。
>- Rレシピの場合は、 **[!UICONTROL R]** runtimeを選択します。
>- PySparkレシピの場合は、 **[!UICONTROL PySpark]** ランタイムを選択します。 アーティファクトタイプが自動入力されます。
>- Scalaレシピの場合は、 **[!UICONTROL Spark]** ランタイムを選択します。 アーティファクトタイプが自動入力されます。


![](../images/models-recipes/package-source-files/docker-creds.png)

「 *Docker Host*」、「 *Username*」、「 *Password*」の値をメモしておきます。 これらは、以下に説明するワークフローで [!DNL Docker] 画像を作成およびプッシュするために使用されます。

>[!NOTE]
>ソースURLは、次の手順を完了した後に提供されます。 設定ファイルについては、 [次の手順で説明する後続のチュートリアルで説明します](#next-steps)。

### ソースファイルのパッケージ化

Data Science Workspace Reference <a href="https://github.com/adobe/experience-platform-dsw-reference" target="_blank"></a> Repositoryにあるサンプルコードベースを取得して開始します。

- [Python Dockerイメージの作成](#python-docker)
- [Build R Dockerイメージ](#r-docker)
- [PySpark Dockerイメージの構築](#pyspark-docker)
- [Scala (Spark) Dockerイメージの構築](#scala-docker)

### Docker [!DNL Python] イメージの作成 {#python-docker}

まだ作成していない場合は、次のコマンドを使用して、 [!DNL GitHub] リポジトリをローカルシステムにコピーします。

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

Navigate to the directory `experience-platform-dsw-reference/recipes/python/retail`. ここでは、スクリプトが見つかり、Dockerにログインし `login.sh` て `build.sh` イメージを作成するために [!DNL Python Docker] 使用されます。 [Docker資格情報の準備ができている場合は](#docker-based-model-authoring) 、次のコマンドを順に入力します。

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

### ビルドR [!DNL Docker] イメージ {#r-docker}

まだ作成していない場合は、次のコマンドを使用して、 [!DNL GitHub] リポジトリをローカルシステムにコピーします。

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

次のコマンドを使用して、 [!DNL GitHub] リポジトリをローカル・システムにクローンして開始します。

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

ターミナルで次のコマンドを使用して、 [!DNL GitHub] リポジトリをローカル・システムにクローンして開始します。

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

次に、スクリプトとを検索でき `experience-platform-dsw-reference/recipes/scala` るディレクトリに移動 `login.sh` し `build.sh`ます。 これらのスクリプトは、DockerにログインしてDockerイメージを作成するために使用されます。 [Docker資格情報の準備ができている場合は](#docker-based-model-authoring) 、次のコマンドを端末に順番に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for building Docker image
./build.sh
```

>[!TIP]
>スクリプトを使用してDockerにログインしようとする際にアクセス許可エラーが発生した場合は、 `login.sh` コマンドを使用してみ `bash login.sh`ます。

ログインスクリプトを実行する場合、Dockerホスト、ユーザー名、パスワードを指定する必要があります。 構築時には、Dockerホストと、構築用のバージョンタグを指定する必要があります。

ビルドスクリプトが完了すると、コンソール出力にDockerソースファイルのURLが与えられます。 この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retailsales-spark:{VERSION_TAG}
```

このURLをコピーして、 [次の手順に進みます](#next-steps)。

## 次の手順 {#next-steps}

このチュートリアルでは、ソースファイルのレシピへのパッケージ化について説明しました。このレシピは、にレシピを読み込むための前提条件の手順で [!DNL Data Science Workspace]す。 これで、Azureコンテナレジストリに、対応するイメージURLと共にDockerイメージが存在するはずです。 これで、パッケージ化されたレシピのに対する読み込みに関するチュートリアルを開始する準備が整い [!DNL Data Science Workspace]ました。 開始するには、次のチュートリアルリンクの1つを選択してください。

- [パッケージ化されたレシピをUIに読み込む](./import-packaged-recipe-ui.md)
- [APIを使用したパッケージ化されたレシピの読み込み](./import-packaged-recipe-api.md)