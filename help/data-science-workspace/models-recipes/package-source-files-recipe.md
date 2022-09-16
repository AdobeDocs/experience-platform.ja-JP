---
keywords: Experience Platform；ソースファイルのパッケージ化；Data Science Workspace；人気の高いトピック；Docker;Docker イメージ
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

このチュートリアルでは、提供された Retail Sales のサンプルソースファイルをアーカイブファイルにパッケージ化する方法を説明します。アーカイブファイルは、Adobe Experience Platformでレシピを作成するために使用できます [!DNL Data Science Workspace] （UI または API を使用してレシピのインポートワークフローに従う）

理解しておくべき概念：

- **レシピ**：レシピはアドビのモデル仕様の用語です。トレーニングされたモデルを作成および実行してビジネス上の特定の問題を解決するために必要な特定の機械学習アルゴリズム、人工知能アルゴリズムまたはアルゴリズムのアンサンブル、処理ロジック、設定を表すトップレベルのコンテナです。
- **ソースファイル**：レシピのロジックを格納した、プロジェクト内の個々のファイルです。

## 前提条件

- [[!DNL Docker]](https://docs.docker.com/install/#supported-platforms)
- [[!DNL Python 3 and pip]](https://docs.conda.io/en/latest/miniconda.html)
- [[!DNL Scala]](https://www.scala-sbt.org/download.html?_ga=2.42231906.690987621.1558478883-2004067584.1558478883)
- [[!DNL Maven]](https://maven.apache.org/install.html)

## レシピの作成

レシピを作成するには、まず、ソースファイルをパッケージ化してアーカイブファイルを作成します。ソースファイルは、現在抱えている特定の問題の解決に使用される機械学習ロジックとアルゴリズムを定義し、次のいずれかの方法で記述されます。 [!DNL Python]、R、PySpark、Scala のいずれかです。 作成されたアーカイブファイルは、Docker イメージの形式を取ります。 パッケージ化されたアーカイブファイルは、作成後、 [!DNL Data Science Workspace] レシピを作成するには [UI 内](./import-packaged-recipe-ui.md) または [API の使用](./import-packaged-recipe-api.md).

### Docker ベースのモデルオーサリング {#docker-based-model-authoring}

Docker イメージを使用すると、開発者は、ライブラリや他の依存コンポーネントなど、必要なすべての構成要素を含めてアプリケーションをパッケージ化し、1 つのパッケージとして提供できます。

作成された Docker イメージは、レシピ作成ワークフロー時に提供された資格情報を使用して、Azure Container Registry にプッシュされます。

Azure Container Registry の資格情報を取得するには、[Adobe Experience Platform](https://platform.adobe.com) にログインします。左側のナビゲーション列で、「**[!UICONTROL Workflows]**」に移動します。選択 **[!UICONTROL レシピを読み込む]** 続いて選択する **[!UICONTROL 起動]**. 以下のスクリーンショットを参照してください。

![](../images/models-recipes/package-source-files/import.png)

この **[!UICONTROL 設定]** ページが開きます。 適切なレシピ名（「Retail Sales recipe」など）を「**[!UICONTROL Recipe name]**」に入力し、オプションで説明やドキュメント URL を入力します。完了したら、「**[!UICONTROL Next]**」をクリックします。

![](../images/models-recipes/package-source-files/configure.png)

適切な *ランタイム*&#x200B;を選択し、 **[!UICONTROL 分類]** 対象 *タイプ*. Azure Container Registry の資格情報は、完了すると生成されます。

>[!NOTE]
>
>*タイプ* は、レシピが設計される機械学習の問題のクラスで、トレーニングの後に使用され、トレーニングの実行状況の評価に役立ちます。

>[!TIP]
>
>- の場合 [!DNL Python] レシピ選択 **[!UICONTROL Python]** ランタイム。
>- R レシピの場合は、 **[!UICONTROL R]** ランタイム。
>- PySpark のレシピの場合、 **[!UICONTROL PySpark]** ランタイム。 アーティファクトタイプが自動入力されます。
>- Scala レシピの場合は、 **[!UICONTROL Spark]** ランタイム。 アーティファクトタイプが自動入力されます。


![](../images/models-recipes/package-source-files/docker-creds.png)

Docker ホスト、ユーザー名、パスワードの値をメモします。 これらは、 [!DNL Docker] 以下に概要を示すワークフローの画像を参照してください。

>[!NOTE]
>
>ソース URL は、以下の手順を完了すると表示されます。 設定ファイルについては、 [次の手順](#next-steps).

### ソースファイルのパッケージ化

まず、<a href="https://github.com/adobe/experience-platform-dsw-reference" target="_blank">Experience Platform Data Science Workspace Reference</a> リポジトリーにあるサンプルコードベースを取得します。

- [Python Docker イメージの作成](#python-docker)
- [R Docker イメージの作成](#r-docker)
- [PySpark Docker イメージの作成](#pyspark-docker)
- [Scala(Spark)Docker イメージの作成](#scala-docker)

### ビルド [!DNL Python] Docker 画像 {#python-docker}

まだクローンしていない場合は、 [!DNL GitHub] 次のコマンドを使用して、ローカルシステムにリポジトリーを設定します。

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

`experience-platform-dsw-reference/recipes/python/retail` ディレクトリに移動します。ここに、スクリプトがあります `login.sh` および `build.sh` Docker にログインし、 [!DNL Python Docker] 画像。 [Docker の資格情報](#docker-based-model-authoring)が既にある場合は、次のコマンドを順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for building Docker image
./build.sh
```

ログインスクリプトを実行する際には、Docker のホスト、ユーザー名、パスワードを指定する必要があります。 イメージの作成時には、Docker ホストとビルドのバージョンタグを入力する必要があります。

ビルドスクリプトが完了したら、Docker ソースファイルの URL がコンソール出力に表示されます。この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retailsales-python:{VERSION_TAG}
```

この URL をコピーして、[次の手順](#next-steps)に進みます。

### ビルド R [!DNL Docker] 画像 {#r-docker}

まだクローンしていない場合は、 [!DNL GitHub] 次のコマンドを使用して、ローカルシステムにリポジトリーを設定します。

```BASH
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

クローンリポジトリー内の `experience-platform-dsw-reference/recipes/R/Retail - GradientBoosting` ディレクトリに移動します。ここで、ファイルが見つかります `login.sh` および `build.sh` Docker にログインする際や、R Docker イメージを作成する際に使用する [Docker の資格情報](#docker-based-model-authoring)が既にある場合は、次のコマンドを順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for build Docker image
./build.sh
```

ログインスクリプトを実行する際には、Docker のホスト、ユーザー名、パスワードを指定する必要があります。 イメージの作成時には、Docker ホストとビルドのバージョンタグを入力する必要があります。

ビルドスクリプトが完了したら、Docker ソースファイルの URL がコンソール出力に表示されます。この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retail-r:{VERSION_TAG}
```

この URL をコピーして、[次の手順](#next-steps)に進みます。

### PySpark Docker イメージの作成 {#pyspark-docker}

最初に、 [!DNL GitHub] 次のコマンドを使用して、ローカルシステムにリポジトリーを設定します。

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

`experience-platform-dsw-reference/recipes/pyspark/retail` ディレクトリに移動します。スクリプト `login.sh` および `build.sh` はここにあり、Docker にログインしたり、Docker イメージを作成したりするために使用されます。 [Docker の資格情報](#docker-based-model-authoring)が既にある場合は、次のコマンドを順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for building Docker image
./build.sh
```

ログインスクリプトを実行する際には、Docker のホスト、ユーザー名、パスワードを指定する必要があります。 イメージの作成時には、Docker ホストとビルドのバージョンタグを入力する必要があります。

ビルドスクリプトが完了したら、Docker ソースファイルの URL がコンソール出力に表示されます。この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retailsales-pyspark:{VERSION_TAG}
```

この URL をコピーして、[次の手順](#next-steps)に進みます。

### Scala Docker イメージの作成 {#scala-docker}

最初に、 [!DNL GitHub] ターミナルで次のコマンドを使用して、ローカルシステムにリポジトリを追加します。

```shell
git clone https://github.com/adobe/experience-platform-dsw-reference.git
```

次に、ディレクトリに移動します。 `experience-platform-dsw-reference/recipes/scala` スクリプトの場所 `login.sh` および `build.sh`. これらのスクリプトは、Docker にログインし、Docker イメージを作成するために使用されます。 次の条件を満たしている場合、 [Docker 資格情報](#docker-based-model-authoring) 準備が整ったら、次のコマンドをターミナルに順に入力します。

```BASH
# for logging in to Docker
./login.sh
 
# for building Docker image
./build.sh
```

>[!TIP]
>
>を使用して Docker にログインしようとした際に、権限エラーが表示される場合は、 `login.sh` スクリプトを使用する場合は、コマンドを使用してみてください `bash login.sh`.

ログインスクリプトを実行する際に、Docker のホスト、ユーザー名、パスワードを指定する必要があります。 イメージの作成時には、Docker ホストとビルドのバージョンタグを入力する必要があります。

ビルドスクリプトが完了したら、Docker ソースファイルの URL がコンソール出力に表示されます。この例では、次のようになります。

```BASH
# URL format: 
{DOCKER_HOST}/ml-retailsales-spark:{VERSION_TAG}
```

この URL をコピーして、[次の手順](#next-steps)に進みます。

## 次の手順 {#next-steps}

このチュートリアルでは、ソースファイルをレシピにパッケージ化する方法について説明しました。これは、レシピをにインポートするための前提条件の手順です。 [!DNL Data Science Workspace]. これで、Azure Container Registry に Docker イメージと対応するイメージ URL が作成されました。 これで、パッケージ化されたレシピをにインポートする方法に関するチュートリアルを開始する準備が整いました。 [!DNL Data Science Workspace]. 以下のチュートリアルリンクの 1 つを選択して、作業を開始します。

- [パッケージ化されたレシピのインポート（UI 版）](./import-packaged-recipe-ui.md)
- [パッケージ化されたレシピのインポート（API 版）](./import-packaged-recipe-api.md)
