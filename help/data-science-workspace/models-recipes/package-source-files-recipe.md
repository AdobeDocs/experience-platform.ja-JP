---
keywords: Experience Platform；パッケージソースファイル；Data Science Workspace；人気の高いトピック；Docker;Docker画像
solution: Experience Platform
title: ソースファイルのレシピへのパッケージ化
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、用意されている Retail Sales サンプルソースファイルをアーカイブファイルにパッケージ化する方法について説明します。このアーカイブファイルは、UI のレシピインポートワークフローに従うか API を使用して、Adobe Experience Platform の Data Science Workspace でレシピを作成するのに使用できます。
exl-id: 199b8127-4f1b-43a4-82e6-58cb70fcdc08
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1159'
ht-degree: 47%

---

# ソースファイルのレシピへのパッケージ化

このチュートリアルでは、提供されたRetail Salesサンプルソースファイルをアーカイブファイルにパッケージ化する方法について説明します。アーカイブファイルは、UIまたはAPIのレシピインポートワークフローに従って、Adobe Experience Platform[!DNL Data Science Workspace]でレシピを作成するのに使用できます。

理解しておくべき概念：

- **レシピ**：レシピはアドビのモデル仕様の用語です。トレーニングされたモデルを作成および実行してビジネス上の特定の問題を解決するために必要な特定の機械学習アルゴリズム、人工知能アルゴリズムまたはアルゴリズムのアンサンブル、処理ロジック、設定を表すトップレベルのコンテナです。
- **ソースファイル**：レシピのロジックを格納した、プロジェクト内の個々のファイルです。

## 前提条件

- [[!DNL Docker]](https://docs.docker.com/install/#supported-platforms)
- [[!DNL Python 3 and pip]](https://docs.conda.io/en/latest/miniconda.html)
- [[!DNL Scala]](https://www.scala-sbt.org/download.html?_ga=2.42231906.690987621.1558478883-2004067584.1558478883)
- [[!DNL Maven]](https://maven.apache.org/install.html)

## レシピの作成

レシピを作成するには、まず、ソースファイルをパッケージ化してアーカイブファイルを作成します。ソースファイルは、機械学習のロジックとアルゴリズムを定義し、実際に問題を解決するために使用され、[!DNL Python]、R、PySpark、Scalaのいずれかに書き込まれます。 構築されたアーカイブファイルは、Dockerイメージの形式をとります。 構築が完了すると、パッケージ化されたアーカイブファイルが[!DNL Data Science Workspace]に読み込まれ、API](./import-packaged-recipe-api.md)を使用してUI](./import-packaged-recipe-ui.md)または[にレシピ[が作成されます。

### Docker ベースのモデルオーサリング {#docker-based-model-authoring}

Docker イメージを使用すると、開発者は、ライブラリや他の依存コンポーネントなど、必要なすべての構成要素を含めてアプリケーションをパッケージ化し、1 つのパッケージとして提供できます。

作成されたDockerイメージは、レシピ作成ワークフローで指定された資格情報を使用してAzureコンテナレジストリにプッシュされます。

Azure Container Registry の資格情報を取得するには、[Adobe Experience Platform](https://platform.adobe.com) にログインします。左側のナビゲーション列で、「**[!UICONTROL Workflows]**」に移動します。「**[!UICONTROL レシピを読み込み]**」を選択し、「****&#x200B;を起動」を選択します。 以下のスクリーンショットを参照してください。

![](../images/models-recipes/package-source-files/import.png)

**[!UICONTROL 設定]**&#x200B;ページが開きます。 適切なレシピ名（「Retail Sales recipe」など）を「**[!UICONTROL Recipe name]**」に入力し、オプションで説明やドキュメント URL を入力します。完了したら、「**[!UICONTROL Next]**」をクリックします。

![](../images/models-recipes/package-source-files/configure.png)

適切な&#x200B;*ランタイム*&#x200B;を選択し、*タイプ*&#x200B;に対して&#x200B;**[!UICONTROL 分類]**&#x200B;を選択します。 Azureコンテナレジストリ資格情報は、完了すると生成されます。

>[!NOTE]
>
>** Typeは、レシピが設計された機械学習の問題のクラスで、トレーニングの実施状況をカスタマイズするのに役立つように、トレーニングの後に使用されます。

>[!TIP]
>
>- [!DNL Python]レシピの場合は、**[!UICONTROL Python]**&#x200B;ランタイムを選択します。
>- Rレシピの場合は、**[!UICONTROL R]**&#x200B;ランタイムを選択します。
>- PySparkレシピの場合は、**[!UICONTROL PySpark]**&#x200B;ランタイムを選択します。 アーティファクトタイプが自動入力されます。
>- Scalaレシピの場合は、**[!UICONTROL Spark]**&#x200B;ランタイムを選択します。 アーティファクトタイプが自動入力されます。


![](../images/models-recipes/package-source-files/docker-creds.png)

Dockerホスト、ユーザー名、パスワードの値をメモしておきます。 これらは、以下に説明するワークフローで[!DNL Docker]イメージを作成し、プッシュするために使用されます。

>[!NOTE]
>
>ソースURLは、次の手順を完了した後に提供されます。 設定ファイルについては、次の手順](#next-steps)にある以降のチュートリアルで説明します。[

### ソースファイルのパッケージ化

まず、<a href="https://github.com/adobe/experience-platform-dsw-reference" target="_blank">Experience Platform Data Science Workspace Reference</a> リポジトリーにあるサンプルコードベースを取得します。

- [Python Docker イメージの作成](#python-docker)
- [R Docker イメージの作成](#r-docker)
- [PySpark Dockerイメージの構築](#pyspark-docker)
- [Scala (Spark) Dockerイメージの構築](#scala-docker)

### [!DNL Python] Dockerイメージ{#python-docker}の構築

まだ作成していない場合は、次のコマンドを使用して[!DNL GitHub]リポジトリをローカルシステムにコピーします。

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

`experience-platform-dsw-reference/recipes/python/retail` ディレクトリに移動します。ここでは、Dockerにログインし、[!DNL Python Docker]イメージを構築するために使用するスクリプト`login.sh`と`build.sh`が見つかります。 [Docker の資格情報](#docker-based-model-authoring)が既にある場合は、次のコマンドを順に入力します。

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

### ビルドR [!DNL Docker]イメージ{#r-docker}

まだ作成していない場合は、次のコマンドを使用して[!DNL GitHub]リポジトリをローカルシステムにコピーします。

```BASH
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

クローンリポジトリー内の `experience-platform-dsw-reference/recipes/R/Retail - GradientBoosting` ディレクトリに移動します。ここでは、Dockerにログインし、R Dockerイメージを作成する際に使用するファイル`login.sh`と`build.sh`が見つかります。 [Docker の資格情報](#docker-based-model-authoring)が既にある場合は、次のコマンドを順に入力します。

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

### PySpark Dockerイメージを構築{#pyspark-docker}

次のコマンドを使用して、[!DNL GitHub]リポジトリをローカルシステムにクローン化し、開始します。

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

`experience-platform-dsw-reference/recipes/pyspark/retail` ディレクトリに移動します。スクリプト`login.sh`と`build.sh`はここにあり、DockerにログインしてDockerイメージを作成するために使用します。 [Docker の資格情報](#docker-based-model-authoring)が既にある場合は、次のコマンドを順に入力します。

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

### Scala Dockerイメージの構築{#scala-docker}

ターミナルで次のコマンドを使用して、[!DNL GitHub]リポジトリをローカルシステムにクローンし、開始します。

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

次に、`experience-platform-dsw-reference/recipes/scala`ディレクトリに移動し、`login.sh`と`build.sh`のスクリプトを探します。 これらのスクリプトは、DockerにログインしてDockerイメージを作成するために使用されます。 [Dockerの資格情報](#docker-based-model-authoring)の準備ができている場合は、次のコマンドを端末に順番に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for building Docker image
./build.sh
```

>[!TIP]
>
>`login.sh`スクリプトを使用してDockerにログインしようとすると、権限エラーが発生する場合は、`bash login.sh`コマンドを使用してみてください。

ログインスクリプトを実行する場合、Dockerホスト、ユーザー名、パスワードを指定する必要があります。 イメージの作成時には、Docker ホストとビルドのバージョンタグを入力する必要があります。

ビルドスクリプトが完了したら、Docker ソースファイルの URL がコンソール出力に表示されます。この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retailsales-spark:{VERSION_TAG}
```

この URL をコピーして、[次の手順](#next-steps)に進みます。

## 次の手順 {#next-steps}

このチュートリアルでは、ソースファイルのパッケージ化をレシピに進めました。レシピは、レシピを[!DNL Data Science Workspace]にインポートするための前提条件です。 これで、Azureコンテナレジストリに、対応するイメージURLと共にDockerイメージが存在するはずです。 これで、パッケージ化されたレシピを[!DNL Data Science Workspace]に読み込む方法のチュートリアルを開始する準備が整いました。 開始するには、次のチュートリアルリンクの1つを選択してください。

- [パッケージ化されたレシピのインポート（UI 版）](./import-packaged-recipe-ui.md)
- [パッケージ化されたレシピのインポート（API 版）](./import-packaged-recipe-api.md)
