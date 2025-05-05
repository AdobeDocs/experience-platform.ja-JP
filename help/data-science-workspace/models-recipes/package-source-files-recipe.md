---
keywords: Experience Platform；パッケージソースファイル；Data Science Workspace；人気のトピック；Docker;docker image
solution: Experience Platform
title: Source ファイルのレシピへのパッケージ化
type: Tutorial
description: このチュートリアルでは、用意されている Retail Sales サンプルソースファイルをアーカイブファイルにパッケージ化する方法について説明します。このアーカイブファイルは、UI のレシピインポートワークフローに従うか API を使用して、Adobe Experience Platform の Data Science Workspace でレシピを作成するのに使用できます。
exl-id: 199b8127-4f1b-43a4-82e6-58cb70fcdc08
source-git-commit: 5d98dc0cbfaf3d17c909464311a33a03ea77f237
workflow-type: tm+mt
source-wordcount: '1166'
ht-degree: 43%

---

# ソースファイルのレシピへのパッケージ化

>[!NOTE]
>
>Data Science Workspaceは購入できなくなりました。
>
>このドキュメントは、Data Science Workspaceの以前の使用権限を持つ既存のお客様を対象としています。

このチュートリアルでは、提供された Retail Sales サンプルソースファイルをアーカイブファイルにパッケージ化する方法について説明します。このファイルを使用して、UI または API でレシピの読み込みワークフローに従って、Adobe Experience Platform [!DNL Data Science Workspace] でレシピを作成できます。

理解しておくべき概念：

- **レシピ**：レシピはアドビのモデル仕様の用語です。トレーニングされたモデルを作成および実行してビジネス上の特定の問題を解決するために必要な特定の機械学習アルゴリズム、人工知能アルゴリズムまたはアルゴリズムのアンサンブル、処理ロジック、設定を表すトップレベルのコンテナです。
- **ソースファイル**：レシピのロジックを格納した、プロジェクト内の個々のファイルです。

## 前提条件

- [[!DNL Docker]](https://docs.docker.com/install/#supported-platforms)
- [[!DNL Python 3 and pip]](https://docs.conda.io/en/latest/miniconda.html)
- [[!DNL Scala]](https://www.scala-sbt.org/download.html?_ga=2.42231906.690987621.1558478883-2004067584.1558478883)
- [[!DNL Maven]](https://maven.apache.org/install.html)

## レシピの作成

レシピを作成するには、まず、ソースファイルをパッケージ化してアーカイブファイルを作成します。Source ファイルは、特定の問題を目の前で解決するために使用される機械学習ロジックとアルゴリズムを定義し、[!DNL Python]、R、PySpark、Scala のいずれかで記述されます。 構築されたアーカイブファイルは、Docker イメージの形式を取ります。 ビルドが完了すると、パッケージ化されたアーカイブファイルが [!DNL Data Science Workspace] に読み込まれ、[UI で ](./import-packaged-recipe-ui.md) または [API を使用して ](./import-packaged-recipe-api.md) レシピが作成されます。

### Docker ベースのモデルオーサリング {#docker-based-model-authoring}

Docker イメージを使用すると、開発者は、ライブラリや他の依存コンポーネントなど、必要なすべての構成要素を含めてアプリケーションをパッケージ化し、1 つのパッケージとして提供できます。

ビルドされた Docker イメージは、レシピ作成ワークフロー中に提供された資格情報を使用して、Azure コンテナレジストリにプッシュされます。

Azure Container Registry の資格情報を取得するには、[Adobe Experience Platform](https://platform.adobe.com) にログインします。左側のナビゲーション列で、「**[!UICONTROL Workflows]**」に移動します。**[!UICONTROL レシピを読み込み]** を選択してから、「**[!UICONTROL 起動]** を選択します。 以下のスクリーンショットを参照してください。

![](../images/models-recipes/package-source-files/import.png)

**[!UICONTROL 設定]** ページが開きます。 適切なレシピ名（「Retail Sales recipe」など）を「**[!UICONTROL Recipe name]**」に入力し、オプションで説明やドキュメント URL を入力します。完了したら、「**[!UICONTROL Next]**」をクリックします。

![](../images/models-recipes/package-source-files/configure.png)

適切な *Runtime* を選択してから、「タイプ *に&#x200B;**[!UICONTROL 分類]**&#x200B;を選択し* す。 Azure コンテナレジストリ資格情報は、完了すると生成されます。

>[!NOTE]
>
>*タイプ* は、レシピの設計対象である機械学習問題のクラスで、トレーニング後にトレーニング実行の評価を調整するために使用されます。

>[!TIP]
>
>- [!DNL Python] のレシピには、**[!UICONTROL Python]** ランタイムを選択します。
>- R レシピの場合は、**[!UICONTROL R]** ランタイムを選択します。
>- PySpark レシピの場合は、**[!UICONTROL PySpark]** ランタイムを選択します。 アーティファクトタイプが自動入力されます。
>- Scala レシピの場合は、**[!UICONTROL Spark]** ランタイムを選択します。 アーティファクトタイプが自動入力されます。

![](../images/models-recipes/package-source-files/docker-creds.png)

Docker ホスト、ユーザー名、パスワードの値を書き留めます。 これらは、以下に概要を示すワークフローで、[!DNL Docker] イメージを作成およびプッシュするために使用されます。

>[!NOTE]
>
>Sourceの URL は、以下に概説されている手順を完了した後に提供されます。 設定ファイルについては、以降の [ 次の手順 ](#next-steps) のチュートリアルで説明します。

### ソースファイルのパッケージ化

まず、<a href="https://github.com/adobe/experience-platform-dsw-reference" target="_blank">Experience PlatformデータサイエンスWorkspace リファレンス </a> リポジトリにあるサンプルコードベースを取得します。

- [Python Docker イメージの作成](#python-docker)
- [R Docker イメージの作成](#r-docker)
- [PySpark Docker イメージのビルド](#pyspark-docker)
- [Scala （Spark） Docker イメージの作成](#scala-docker)

### Docker イメージ [!DNL Python] ビルド {#python-docker}

まだ行っていない場合は、次のコマンドを使用して、[!DNL GitHub] リポジトリをローカルシステムにクローンします。

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

`experience-platform-dsw-reference/recipes/python/retail` ディレクトリに移動します。ここには、Docker へのログインと [!DNL Python Docker] イメージの構築に使用されるスクリプト `login.sh` と `build.sh` があります。 [Docker の資格情報](#docker-based-model-authoring)が既にある場合は、次のコマンドを順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for building Docker image
./build.sh
```

ログインスクリプトを実行する際は、Docker のホスト、ユーザー名およびパスワードを指定する必要があります。 イメージの作成時には、Docker ホストとビルドのバージョンタグを入力する必要があります。

ビルドスクリプトが完了したら、Docker ソースファイルの URL がコンソール出力に表示されます。この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retailsales-python:{VERSION_TAG}
```

この URL をコピーして、[次の手順](#next-steps)に進みます。

### ビルド R [!DNL Docker] イメージ {#r-docker}

まだ行っていない場合は、次のコマンドを使用して、[!DNL GitHub] リポジトリをローカルシステムにクローンします。

```BASH
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

クローンリポジトリー内の `experience-platform-dsw-reference/recipes/R/Retail - GradientBoosting` ディレクトリに移動します。ここでは、Docker へのログインと R Docker イメージの構築に使用するファイル `login.sh` と `build.sh` を見つけます。 [Docker の資格情報](#docker-based-model-authoring)が既にある場合は、次のコマンドを順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for build Docker image
./build.sh
```

ログインスクリプトを実行する際は、Docker のホスト、ユーザー名およびパスワードを指定する必要があります。 イメージの作成時には、Docker ホストとビルドのバージョンタグを入力する必要があります。

ビルドスクリプトが完了したら、Docker ソースファイルの URL がコンソール出力に表示されます。この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retail-r:{VERSION_TAG}
```

この URL をコピーして、[次の手順](#next-steps)に進みます。

### PySpark Docker イメージのビルド {#pyspark-docker}

まず、次のコマンドを使用して、[!DNL GitHub] リポジトリをローカルシステムにクローンします。

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

`experience-platform-dsw-reference/recipes/pyspark/retail` ディレクトリに移動します。スクリプト `login.sh` と `build.sh` は、Docker へのログインと Docker イメージの構築に使用されます。 [Docker の資格情報](#docker-based-model-authoring)が既にある場合は、次のコマンドを順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for building Docker image
./build.sh
```

ログインスクリプトを実行する際は、Docker のホスト、ユーザー名およびパスワードを指定する必要があります。 イメージの作成時には、Docker ホストとビルドのバージョンタグを入力する必要があります。

ビルドスクリプトが完了したら、Docker ソースファイルの URL がコンソール出力に表示されます。この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retailsales-pyspark:{VERSION_TAG}
```

この URL をコピーして、[次の手順](#next-steps)に進みます。

### Scala Docker イメージのビルド {#scala-docker}

ターミナルで次のコマンドを使用して、[!DNL GitHub] リポジトリをローカルシステムにクローンして開始します。

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

次に、ディレクトリ `experience-platform-dsw-reference/recipes/scala` に移動すると、スクリプト `login.sh` および `build.sh` が見つかります。 これらのスクリプトは、Docker へのログインと Docker イメージのビルドに使用されます。 [Docker 資格情報 ](#docker-based-model-authoring) の準備ができている場合は、次のコマンドをターミナルに順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for building Docker image
./build.sh
```

>[!TIP]
>
>`login.sh` スクリプトを使用して Docker にログインしようとすると権限エラーが発生した場合は、コマンド `bash login.sh` を使用してみてください。

ログインスクリプトを実行する際は、Docker のホスト、ユーザー名、パスワードを指定する必要があります。 イメージの作成時には、Docker ホストとビルドのバージョンタグを入力する必要があります。

ビルドスクリプトが完了したら、Docker ソースファイルの URL がコンソール出力に表示されます。この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retailsales-spark:{VERSION_TAG}
```

この URL をコピーして、[次の手順](#next-steps)に進みます。

## 次の手順 {#next-steps}

このチュートリアルでは、ソースファイルのレシピへのパッケージ化を確認しました。これは、レシピを [!DNL Data Science Workspace] に読み込むための前提条件です。 これで、Azure コンテナレジストリに、対応する画像 URL と共に Docker イメージが作成されました。 これで、パッケージ化されたレシピの [!DNL Data Science Workspace] への読み込みに関するチュートリアルを開始する準備が整いました。 開始するには、以下のいずれかのチュートリアルリンクを選択します。

- [UI でのパッケージ化されたレシピの読み込み](./import-packaged-recipe-ui.md)
- [API を使用したパッケージ化されたレシピの読み込み](./import-packaged-recipe-api.md)
