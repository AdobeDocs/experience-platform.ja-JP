---
keywords: Experience Platform；ソースファイルのパッケージ化；Data Science Workspace；人気のあるトピック；Docker;Docker イメージ
solution: Experience Platform
title: ソースファイルのレシピへのパッケージ化
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、用意されている Retail Sales サンプルソースファイルをアーカイブファイルにパッケージ化する方法について説明します。このアーカイブファイルは、UI のレシピインポートワークフローに従うか API を使用して、Adobe Experience Platform の Data Science Workspace でレシピを作成するのに使用できます。
exl-id: 199b8127-4f1b-43a4-82e6-58cb70fcdc08
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1159'
ht-degree: 47%

---

# ソースファイルのレシピへのパッケージ化

このチュートリアルでは、提供された Retail Sales サンプルのソースファイルをアーカイブファイルにパッケージ化する方法を説明します。アーカイブファイルは、UI または API のレシピインポートワークフローに従って、Adobe Experience Platform [!DNL Data Science Workspace] でレシピを作成できます。

理解しておくべき概念：

- **レシピ**：レシピはアドビのモデル仕様の用語です。トレーニングされたモデルを作成および実行してビジネス上の特定の問題を解決するために必要な特定の機械学習アルゴリズム、人工知能アルゴリズムまたはアルゴリズムのアンサンブル、処理ロジック、設定を表すトップレベルのコンテナです。
- **ソースファイル**：レシピのロジックを格納した、プロジェクト内の個々のファイルです。

## 前提条件

- [[!DNL Docker]](https://docs.docker.com/install/#supported-platforms)
- [[!DNL Python 3 and pip]](https://docs.conda.io/en/latest/miniconda.html)
- [[!DNL Scala]](https://www.scala-sbt.org/download.html?_ga=2.42231906.690987621.1558478883-2004067584.1558478883)
- [[!DNL Maven]](https://maven.apache.org/install.html)

## レシピの作成

レシピを作成するには、まず、ソースファイルをパッケージ化してアーカイブファイルを作成します。ソースファイルは、現在抱えている特定の問題の解決に使用する機械学習ロジックとアルゴリズムを定義し、[!DNL Python]、R、PySpark、Scala のいずれかで記述されます。 作成されたアーカイブファイルは、Docker イメージの形式を取ります。 パッケージ化されたアーカイブファイルは、作成後、[!DNL Data Science Workspace] に読み込まれ、API](./import-packaged-recipe-ui.md) または [ を使用して UI](./import-packaged-recipe-api.md) または  にレシピ [ を作成します。

### Docker ベースのモデルオーサリング {#docker-based-model-authoring}

Docker イメージを使用すると、開発者は、ライブラリや他の依存コンポーネントなど、必要なすべての構成要素を含めてアプリケーションをパッケージ化し、1 つのパッケージとして提供できます。

作成された Docker イメージは、レシピ作成ワークフロー中に提供された資格情報を使用して Azure Container Registry にプッシュされます。

Azure Container Registry の資格情報を取得するには、[Adobe Experience Platform](https://platform.adobe.com) にログインします。左側のナビゲーション列で、「**[!UICONTROL Workflows]**」に移動します。「**[!UICONTROL レシピをインポート]**」を選択し、「**** を起動」を選択します。 以下のスクリーンショットを参照してください。

![](../images/models-recipes/package-source-files/import.png)

**[!UICONTROL 設定]** ページが開きます。 適切なレシピ名（「Retail Sales recipe」など）を「**[!UICONTROL Recipe name]**」に入力し、オプションで説明やドキュメント URL を入力します。完了したら、「**[!UICONTROL Next]**」をクリックします。

![](../images/models-recipes/package-source-files/configure.png)

適切な「*Runtime*」を選択し、「*型*」に **[!UICONTROL 分類]** を選択します。 Azure Container Registry の資格情報は、完了すると生成されます。

>[!NOTE]
>
>** Type は、レシピが設計される機械学習の問題のクラスで、トレーニングの後に使用され、トレーニングの実行状況の評価に役立ちます。

>[!TIP]
>
>- [!DNL Python] レシピの場合は、**[!UICONTROL Python]** ランタイムを選択します。
>- R レシピの場合は、**[!UICONTROL R]** ランタイムを選択します。
>- PySpark レシピの場合は、**[!UICONTROL PySpark]** ランタイムを選択します。 アーティファクトタイプが自動入力されます。
>- Scala レシピの場合は、**[!UICONTROL Spark]** ランタイムを選択します。 アーティファクトタイプが自動入力されます。


![](../images/models-recipes/package-source-files/docker-creds.png)

Docker のホスト、ユーザー名、パスワードの値をメモしておきます。 これらは、以下に説明するワークフローで [!DNL Docker] 画像を作成およびプッシュするために使用されます。

>[!NOTE]
>
>ソース URL は、以下の手順を完了すると表示されます。 設定ファイルについては、[ 次の手順 ](#next-steps) で見つかる以降のチュートリアルで説明します。

### ソースファイルのパッケージ化

まず、<a href="https://github.com/adobe/experience-platform-dsw-reference" target="_blank">Experience Platform Data Science Workspace Reference</a> リポジトリーにあるサンプルコードベースを取得します。

- [Python Docker イメージの作成](#python-docker)
- [R Docker イメージの作成](#r-docker)
- [PySpark Docker イメージの作成](#pyspark-docker)
- [Scala(Spark)Docker イメージの作成](#scala-docker)

### [!DNL Python] Docker イメージの作成 {#python-docker}

まだ実行していない場合は、次のコマンドを使用して、[!DNL GitHub] リポジトリをローカルシステムにクローンします。

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

`experience-platform-dsw-reference/recipes/python/retail` ディレクトリに移動します。ここには、Docker にログインし、[!DNL Python Docker] イメージを作成するためのスクリプト `login.sh` と `build.sh` が含まれています。 [Docker の資格情報](#docker-based-model-authoring)が既にある場合は、次のコマンドを順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for building Docker image
./build.sh
```

ログインスクリプトを実行する場合、Docker のホスト、ユーザー名、パスワードを指定する必要があります。 イメージの作成時には、Docker ホストとビルドのバージョンタグを入力する必要があります。

ビルドスクリプトが完了したら、Docker ソースファイルの URL がコンソール出力に表示されます。この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retailsales-python:{VERSION_TAG}
```

この URL をコピーして、[次の手順](#next-steps)に進みます。

### R [!DNL Docker] イメージを構築 {#r-docker}

まだ実行していない場合は、次のコマンドを使用して、[!DNL GitHub] リポジトリをローカルシステムにクローンします。

```BASH
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

クローンリポジトリー内の `experience-platform-dsw-reference/recipes/R/Retail - GradientBoosting` ディレクトリに移動します。ここには、Docker にログインし、R Docker イメージを作成するために使用する `login.sh` と `build.sh` ファイルがあります。 [Docker の資格情報](#docker-based-model-authoring)が既にある場合は、次のコマンドを順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for build Docker image
./build.sh
```

ログインスクリプトを実行する場合、Docker のホスト、ユーザー名、パスワードを指定する必要があります。 イメージの作成時には、Docker ホストとビルドのバージョンタグを入力する必要があります。

ビルドスクリプトが完了したら、Docker ソースファイルの URL がコンソール出力に表示されます。この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retail-r:{VERSION_TAG}
```

この URL をコピーして、[次の手順](#next-steps)に進みます。

### PySpark Docker イメージの作成 {#pyspark-docker}

まず、次のコマンドを使用して、[!DNL GitHub] リポジトリをローカルシステムにクローンします。

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

`experience-platform-dsw-reference/recipes/pyspark/retail` ディレクトリに移動します。スクリプト `login.sh` と `build.sh` はここに配置され、Docker にログインして Docker イメージを作成するために使用されます。 [Docker の資格情報](#docker-based-model-authoring)が既にある場合は、次のコマンドを順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for building Docker image
./build.sh
```

ログインスクリプトを実行する場合、Docker のホスト、ユーザー名、パスワードを指定する必要があります。 イメージの作成時には、Docker ホストとビルドのバージョンタグを入力する必要があります。

ビルドスクリプトが完了したら、Docker ソースファイルの URL がコンソール出力に表示されます。この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retailsales-pyspark:{VERSION_TAG}
```

この URL をコピーして、[次の手順](#next-steps)に進みます。

### Scala Docker イメージの作成 {#scala-docker}

まず、ターミナルで次のコマンドを使用して、[!DNL GitHub] リポジトリをローカルシステムにクローンします。

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

次に、スクリプト `login.sh` と `build.sh` を検索できるディレクトリ `experience-platform-dsw-reference/recipes/scala` に移動します。 これらのスクリプトは、Docker にログインし、Docker イメージを作成するために使用します。 [Docker の資格情報 ](#docker-based-model-authoring) が既にある場合は、次のコマンドをターミナルに順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for building Docker image
./build.sh
```

>[!TIP]
>
>`login.sh` スクリプトを使用して Docker にログインしようとすると、権限エラーが発生する場合は、コマンド `bash login.sh` を使用してみてください。

ログインスクリプトを実行する場合、Docker のホスト、ユーザー名、パスワードを指定する必要があります。 イメージの作成時には、Docker ホストとビルドのバージョンタグを入力する必要があります。

ビルドスクリプトが完了したら、Docker ソースファイルの URL がコンソール出力に表示されます。この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retailsales-spark:{VERSION_TAG}
```

この URL をコピーして、[次の手順](#next-steps)に進みます。

## 次の手順 {#next-steps}

このチュートリアルでは、ソースファイルをレシピにパッケージ化する方法について説明しました。レシピを [!DNL Data Science Workspace] にインポートするための前提条件の手順です。 これで、Azure Container Registry に Docker イメージが作成され、対応するイメージ URL も取得できます。 これで、パッケージ化されたレシピを [!DNL Data Science Workspace] にインポートする方法に関するチュートリアルを開始する準備が整いました。 使い始めるには、以下のチュートリアルリンクの 1 つを選択してください。

- [パッケージ化されたレシピのインポート（UI 版）](./import-packaged-recipe-ui.md)
- [パッケージ化されたレシピのインポート（API 版）](./import-packaged-recipe-api.md)
