---
keywords: Experience Platform;package source files;Data Science Workspace;popular topics;Docker;docker image
solution: Experience Platform
title: ソースファイルのレシピへのパッケージ化
topic: tutorial
type: Tutorial
description: このチュートリアルでは、用意されている Retail Sales サンプルソースファイルをアーカイブファイルにパッケージ化する方法について説明します。このアーカイブファイルは、UI のレシピインポートワークフローに従うか API を使用して、Adobe Experience Platform の Data Science Workspace でレシピを作成するのに使用できます。
translation-type: tm+mt
source-git-commit: 8c94d3631296c1c3cc97501ccf1a3ed995ec3cab
workflow-type: tm+mt
source-wordcount: '1146'
ht-degree: 48%

---


# ソースファイルのレシピへのパッケージ化

This tutorial provides instructions on how you can package the provided Retail Sales sample source files into an archive file, which can be used to create a recipe in Adobe Experience Platform [!DNL Data Science Workspace] by following the recipe import workflow either in the UI or using the API.

理解しておくべき概念：

- **レシピ**：レシピはアドビのモデル仕様の用語です。トレーニングされたモデルを作成および実行してビジネス上の特定の問題を解決するために必要な特定の機械学習アルゴリズム、人工知能アルゴリズムまたはアルゴリズムのアンサンブル、処理ロジック、設定を表すトップレベルのコンテナです。
- **ソースファイル**：レシピのロジックを格納した、プロジェクト内の個々のファイルです。

## 前提条件

- [[!DNL Docker]](https://docs.docker.com/install/#supported-platforms)
- [[!DNL Python 3 and pip]](https://docs.conda.io/en/latest/miniconda.html)
- [[!DNL Scala]](https://www.scala-sbt.org/download.html?_ga=2.42231906.690987621.1558478883-2004067584.1558478883)
- [[!DNL Maven]](https://maven.apache.org/install.html)

## レシピの作成

レシピを作成するには、まず、ソースファイルをパッケージ化してアーカイブファイルを作成します。Source files define the machine learning logic and algorithms used to solve a specific problem at hand, and are written in either [!DNL Python], R, PySpark, or Scala. 構築されたアーカイブファイルは、Dockerイメージの形式をとります。 Once built, the packaged archive file is imported into [!DNL Data Science Workspace] to create a recipe [in the UI](./import-packaged-recipe-ui.md) or [using the API](./import-packaged-recipe-api.md).

### Docker ベースのモデルオーサリング {#docker-based-model-authoring}

Docker イメージを使用すると、開発者は、ライブラリや他の依存コンポーネントなど、必要なすべての構成要素を含めてアプリケーションをパッケージ化し、1 つのパッケージとして提供できます。

作成されたDockerイメージは、レシピ作成ワークフローで指定された資格情報を使用してAzureコンテナレジストリにプッシュされます。

Azure Container Registry の資格情報を取得するには、[Adobe Experience Platform](https://platform.adobe.com) にログインします。左側のナビゲーション列で、「**[!UICONTROL Workflows]**」に移動します。「 **[!UICONTROL レシピの]** 読み込み **[!UICONTROL 」を選択し、「]**&#x200B;開始」を選択します。 以下のスクリーンショットを参照してください。

![](../images/models-recipes/package-source-files/import.png)

「 **[!UICONTROL 設定]** 」ページが開きます。 適切なレシピ名（「Retail Sales recipe」など）を「**[!UICONTROL Recipe name]**」に入力し、オプションで説明やドキュメント URL を入力します。完了したら、「**[!UICONTROL Next]**」をクリックします。

![](../images/models-recipes/package-source-files/configure.png)

Select the appropriate *Runtime*, then choose a **[!UICONTROL Classification]** for *Type*. Azureコンテナレジストリ資格情報は、完了すると生成されます。

>[!NOTE]
>
>*「タイプ* 」は、レシピが設計する機械学習の問題のクラスで、トレーニングの後に使用され、トレーニングの実施状況をカスタマイズするのに役立ちます。

>[!TIP]
>
>- レシピの場合 [!DNL Python] は、 **[!UICONTROL Python]** ランタイムを選択します。
>- Rレシピの場合は、 **[!UICONTROL R]** runtimeを選択します。
>- PySparkレシピの場合は、 **[!UICONTROL PySpark]** ランタイムを選択します。 アーティファクトタイプが自動入力されます。
>- Scalaレシピの場合は、 **[!UICONTROL Spark]** ランタイムを選択します。 アーティファクトタイプが自動入力されます。


![](../images/models-recipes/package-source-files/docker-creds.png)

Dockerホスト、ユーザー名、パスワードの値をメモしておきます。 これらは、以下に説明するワークフローで [!DNL Docker] 画像を作成およびプッシュするために使用されます。

>[!NOTE]
>
>ソースURLは、次の手順を完了した後に提供されます。 設定ファイルについては、 [次の手順で説明する後続のチュートリアルで説明します](#next-steps)。

### ソースファイルのパッケージ化

まず、<a href="https://github.com/adobe/experience-platform-dsw-reference" target="_blank">Experience Platform Data Science Workspace Reference</a> リポジトリーにあるサンプルコードベースを取得します。

- [Python Docker イメージの作成](#python-docker)
- [R Docker イメージの作成](#r-docker)
- [PySpark Dockerイメージの構築](#pyspark-docker)
- [Scala (Spark) Dockerイメージの構築](#scala-docker)

### Build [!DNL Python] Docker image {#python-docker}

If you have not done so, clone the [!DNL GitHub] repository onto your local system with the following command:

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

`experience-platform-dsw-reference/recipes/python/retail` ディレクトリに移動します。ここでは、スクリプトが見つかり、Dockerにログインし `login.sh` て `build.sh` イメージを作成するために [!DNL Python Docker] 使用されます。 [Docker の資格情報](#docker-based-model-authoring)が既にある場合は、次のコマンドを順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for building Docker image
./build.sh
```

ログインスクリプトを実行する場合は、Dockerホスト、ユーザー名、パスワードを指定する必要があります。 イメージの作成時には、Docker ホストとビルドのバージョンタグを入力する必要があります。

ビルドスクリプトが完了したら、Docker ソースファイルの URL がコンソール出力に表示されます。この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retailsales-python:{VERSION_TAG}
```

この URL をコピーして、[次の手順](#next-steps)に進みます。

### Build R [!DNL Docker] image {#r-docker}

If you have not done so, clone the [!DNL GitHub] repository onto your local system with the following command:

```BASH
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

クローンリポジトリー内の `experience-platform-dsw-reference/recipes/R/Retail - GradientBoosting` ディレクトリに移動します。Here, you&#39;ll find the files `login.sh` and `build.sh` which you will use to login to Docker and to build the R Docker image. [Docker の資格情報](#docker-based-model-authoring)が既にある場合は、次のコマンドを順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for build Docker image
./build.sh
```

ログインスクリプトを実行する場合は、Dockerホスト、ユーザー名、パスワードを指定する必要があります。 イメージの作成時には、Docker ホストとビルドのバージョンタグを入力する必要があります。

ビルドスクリプトが完了したら、Docker ソースファイルの URL がコンソール出力に表示されます。この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retail-r:{VERSION_TAG}
```

この URL をコピーして、[次の手順](#next-steps)に進みます。

### PySpark Dockerイメージの構築 {#pyspark-docker}

次のコマンドを使用して、 [!DNL GitHub] リポジトリをローカル・システムにクローンして開始します。

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

`experience-platform-dsw-reference/recipes/pyspark/retail` ディレクトリに移動します。スクリプト `login.sh` とスクリプト `build.sh` はここにあり、DockerにログインしてDockerイメージを作成する際に使用します。 [Docker の資格情報](#docker-based-model-authoring)が既にある場合は、次のコマンドを順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for building Docker image
./build.sh
```

ログインスクリプトを実行する場合は、Dockerホスト、ユーザー名、パスワードを指定する必要があります。 イメージの作成時には、Docker ホストとビルドのバージョンタグを入力する必要があります。

ビルドスクリプトが完了したら、Docker ソースファイルの URL がコンソール出力に表示されます。この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retailsales-pyspark:{VERSION_TAG}
```

この URL をコピーして、[次の手順](#next-steps)に進みます。

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
>
>スクリプトを使用してDockerにログインしようとする際にアクセス許可エラーが発生した場合は、 `login.sh` コマンドを使用してみ `bash login.sh`ます。

ログインスクリプトを実行する場合、Dockerホスト、ユーザー名、パスワードを指定する必要があります。 イメージの作成時には、Docker ホストとビルドのバージョンタグを入力する必要があります。

ビルドスクリプトが完了したら、Docker ソースファイルの URL がコンソール出力に表示されます。この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retailsales-spark:{VERSION_TAG}
```

この URL をコピーして、[次の手順](#next-steps)に進みます。

## 次の手順 {#next-steps}

This tutorial went over packaging source files into a Recipe, the prerequisite step for importing a Recipe into [!DNL Data Science Workspace]. これで、Azureコンテナレジストリに、対応するイメージURLと共にDockerイメージが存在するはずです。 You are now ready to begin the tutorial on importing a packaged recipe into [!DNL Data Science Workspace]. 開始するには、次のチュートリアルリンクの1つを選択してください。

- [パッケージ化されたレシピのインポート（UI 版）](./import-packaged-recipe-ui.md)
- [パッケージ化されたレシピのインポート（API 版）](./import-packaged-recipe-api.md)