---
keywords: Experience Platform;JupyterLab;notebooks;Data Science Workspace；人気のあるトピック；jupyterlab
solution: Experience Platform
title: JupterLab UIの概要
topic: Overview
description: JupyterLab は、プロジェクト Jupyter の Web ベースのユーザーインターフェイスで、Adobe Experience Platform に緊密に統合されています。これは、データ科学者がJupter Notebooks、コード、データを扱うための対話型の開発環境を提供します。 このドキュメントでは、JupyterLab とその機能の概要と、一般的なアクションを実行する手順を説明します。
translation-type: tm+mt
source-git-commit: 698639d6c2f7897f0eb4cce2a1f265a0f7bb57c9
workflow-type: tm+mt
source-wordcount: '1950'
ht-degree: 67%

---


# [!DNL JupyterLab] UI の概要

[!DNL JupyterLab] は、[プロジェクト Jupyter](https://jupyter.org/) の Web ベースのユーザーインターフェイスで、Adobe Experience Platform に緊密に統合されています。これは、データ科学者がJupter Notebooks、コード、データを扱うための対話型の開発環境を提供します。

このドキュメントでは、[!DNL JupyterLab]とその機能の概要と、一般的な操作を実行する手順を説明します。

## [!DNL JupyterLab] on [!DNL Experience Platform]

Experience Platform の JupyterLab 統合には、アーキテクチャの変更、デザイン上の考慮事項、カスタマイズされたノートブック拡張機能、プリインストールされたライブラリ、アドビをテーマにしたインターフェイスが付属しています。

次のリストでは、Platform 上の JupyterLab に固有の機能の一部を説明します。

| 機能 | 説明 |
| --- | --- |
| **カーネル** | カーネルは、ノートブックや他の[!DNL JupyterLab]フロントエンドに、異なるプログラミング言語でコードを実行したり内観したりする機能を提供します。 [!DNL Experience Platform] は、 [!DNL Python]、R、PySpark、およびでの開発をサポートする追加カーネルを提供 [!DNL Spark]します。詳しくは「[カーネル](#kernels)」の節を参照してください。 |
| **データアクセス** | 読み取り/書き込み機能を完全にサポートし、既存のデータセットに[!DNL JupyterLab]内から直接アクセスできます。 |
| **[!DNL Platform]サービス統合** | 組み込みの統合により、[!DNL JupyterLab]内から直接他の[!DNL Platform]サービスを利用できます。 サポートされる統合の完全なリストは、「[他の Platform サービスとの統合](#service-integration)」の節に記載されています。 |
| **認証** | <a href="https://jupyter-notebook.readthedocs.io/en/latest/security.html" target="_blank">JupyterLab の組み込みのセキュリティモデル</a>に加えて、Platform のサービス間通信を含む、アプリケーションと Experience Platform の間のすべてのやり取りは、<a href="https://www.adobe.io/authentication/auth-methods.html" target="_blank">[!DNL Adobe Identity Management System]（IMS）</a>を通じて暗号化され、認証されます。 |
| **開発ライブラリ** | [!DNL Experience Platform]では、[!DNL JupyterLab]は、[!DNL Python]、R、PySpark用のプリインストールライブラリを提供します。 サポートされているライブラリの完全なリストについては、[付録](#supported-libraries)を参照してください。 |
| **ライブラリコントローラー** | プレインストールされたライブラリがニーズに合わない場合は、PythonとR用に追加のライブラリをインストールし、[!DNL Platform]の整合性を維持し、データを安全に保つために、一時的に独立したコンテナに保存します。 詳しくは「[カーネル](#kernels)」の節を参照してください。 |

>[!NOTE]
>
> 追加のライブラリは、インストールされたセッションでのみ使用できます。新しいセッションを開始する際に必要な追加のライブラリを再インストールする必要があります。

## 他の[!DNL Platform]サービスとの統合{#service-integration}

標準化と相互運用性は、[!DNL Experience Platform]の背後にある重要な概念です。 [!DNL JupyterLab]を組み込みIDEとして[!DNL Platform]に統合すると、他の[!DNL Platform]サービスとやり取りでき、[!DNL Platform]を最大限に活用できます。 [!DNL JupyterLab]では、次の[!DNL Platform]サービスを利用できます。

* **[!DNL Catalog Service]：読み取り/書き込み機能を備えたデータセットに** アクセスし、データセットを表示します。
* **[!DNL Query Service]:SQLを使用してデータセットに** アクセスし、データ量の多いデータを処理する際に、データアクセスのオーバーヘッドを低減できます。
* **[!DNL Sensei ML Framework]：データのトレーニングとスコア付け、および1回のクリックでレシピを作成できるモデル開発。** 
* **[!DNL Experience Data Model (XDM)]:** 標準化と相互運用性は、Adobe Experience Platformの主な概念です。[Adobeに基づくExperience Data Model(XDM)](https://www.adobe.com/go/xdm-home-en)は、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

>[!NOTE]
>
>[!DNL Platform]上の一部の[!DNL JupyterLab]サービス統合は、特定のカーネルに限定されています。 詳しくは「[カーネル](#kernels)」の節を参照してください。

## 主な機能と一般的な操作

[!DNL JupyterLab]の主な機能に関する情報と、一般的な操作を実行する手順については、以下の節で説明します。

* [JupyterLab へのアクセス](#access-jupyterlab)
* [JupyterLab インターフェイス](#jupyterlab-interface)
* [コードセル](#code-cells)
* [カーネル](#kernels)
* [カーネルセッション](#kernel-sessions)
* [ランチャー](#launcher)

### アクセス [!DNL JupyterLab] {#access-jupyterlab}

[Adobe Experience Platform](https://platform.adobe.com)で、左側のナビゲーション列から&#x200B;**ノートブック**&#x200B;を選択します。 [!DNL JupyterLab]が完全に初期化されるまでしばらく待ちます。

![](../images/jupyterlab/user-guide/access_jupyterlab.png)

### [!DNL JupyterLab] インターフェイス {#jupyterlab-interface}

[!DNL JupyterLab]インターフェイスは、メニューバー、折りたたみ可能な左側のサイドバー、およびドキュメントとアクティビティのタブを含むメイン作業領域で構成されます。

**メニューバー**

インターフェイス上部のメニューバーには、[!DNL JupyterLab]で使用可能なアクションをキーボードショートカットと共に公開するトップレベルのメニューがあります。

* **ファイル**：ファイルとディレクトリに関連するアクション
* **編集**：編集に関するアクションおよびドキュメントのアクティビティ
* **表示** ： の外観を変更するアクション[!DNL JupyterLab]
* **実行**：ノートブックやコードコンソールなど、異なるアクティビティでコードを実行するアクション
* **カーネル** ：カーネル管理のアクション
* **タブ**：開いているドキュメントとアクティビティのリスト
* **設定**：共通設定と詳細設定エディター
* **ヘルプ**[!DNL JupyterLab]： とカーネルのヘルプリンクのリスト

**左サイドバー**

左側のサイドバーには、次の機能にアクセスできるクリック可能なタブが含まれています。

* **ファイルブラウザー**：保存されたノートブックドキュメントとディレクトリのリスト
* **データエクスプローラー**：データセットとスキーマ
* **カーネルとターミナルの実行**：終了する機能を持つアクティブなカーネルとターミナルセッションのリスト
* **コマンド**：便利なコマンドのリスト
* **セルインスペクター** ：プレゼンテーション用にノートブックを設定する際に役立つツールやメタデータにアクセスできるセルエディター。
* **タブ**：開いたタブのリスト

タブをクリックしてその機能を表示するか、展開されたタブをクリックして左側のサイドバーを折りたたみます。以下に例を示します。

![](../images/jupyterlab/user-guide/left_sidebar_collapse.gif)

**メイン作業領域**

[!DNL JupyterLab]のメイン作業領域を使用すると、ドキュメントや他のアクティビティを、サイズ変更や再分割が可能なタブのパネルに配置できます。 タブをタブパネルの中央にドラッグして、タブを移行します。タブをパネルの左、右、上または下にドラッグして、パネルを分割します。

![](../images/jupyterlab/user-guide/main_work_area.gif)

### コードセル {#code-cells}

コードセルは、ノートブックの主なコンテンツです。これらには、ノートブックの関連カーネルの言語のソースコードと、コードセルを実行した結果の出力が含まれています。実行回数は、実行順序を表す各コードセルの右側に表示されます。

![](../images/jupyterlab/user-guide/code_cell.png)

一般的なセルのアクションを以下に示します。

* **セルの追加**：ノートブックメニューのプラス記号（**+**）をクリックして、空のセルを追加します。新しいセルは、現在操作中のセルの下に配置されます。特定のセルにフォーカスがない場合は、ノートブックの最後に配置されます。

* **セルの移動**：移動するセルの右側にカーソルを置き、セルをクリックして新しい位置にドラッグします。また、あるノートブックから別のノートブックにセルを移動すると、セルとその内容が複製されます。

* **セルの実行**：実行するセルのボディをクリックし、ノートブックメニューの&#x200B;**再生**&#x200B;アイコン（**▶**）をクリックします。カーネルが実行を処理する際には、セルの実行カウンターにアスタリスク（**\***）が表示され、完了時には整数に置き換えられます。

* **セルの削除**：削除するセルのボディをクリックし、**はさみ**&#x200B;アイコンをクリックします。

### カーネル {#kernels}

ノートのカーネルは、ノートのセルを処理するための言語固有のコンピューティングエンジンです。[!DNL Python]に加えて、[!DNL JupyterLab]はR, PySpark, [!DNL Spark] (Scala)で追加の言語サポートを提供します。 ノートブックドキュメントを開くと、関連するカーネルが起動します。ノートブックセルが実行されると、カーネルは計算をおこない、結果を生成し、CPU やメモリリソースを大量に消費する可能性があります。割り当てられたメモリは、カーネルがシャットダウンされるまで解放されないことに注意してください。

特定の機能は、以下の表で説明するように、特定のカーネルに限定されています。

| カーネル | ライブラリのインストールサポート | [!DNL Platform] 統合 |
| :----: | :--------------------------: | :-------------------- |
| **[!DNL Python]** | ○ | <ul><li>[!DNL Sensei ML Framework]</li><li>[!DNL Catalog Service]</li><li>[!DNL Query Service]</li></ul> |
| **R** | ○ | <ul><li>[!DNL Sensei ML Framework]</li><li>[!DNL Catalog Service]</li></ul> |
| **Scala** | × | <ul><li>[!DNL Sensei ML Framework]</li><li>[!DNL Catalog Service]</li></ul> |

### カーネルセッション {#kernel-sessions}

[!DNL JupyterLab]上のアクティブなノートブックまたはアクティビティは、それぞれカーネルセッションを利用します。 すべてのアクティブなセッションは、左側のサイドバーから「**実行中の端末とカーネル**」タブを展開すると見つかります。ノートブックのカーネルのタイプと状態は、ノートブックのインターフェースの右上を見ることで識別できます。下の図では、ノートブックに関連するカーネルは **[!DNL Python]3** で、現在の状態は右側に灰色の円で表されています。中空の円はアイドルカーネルを意味し、実円はビジーカーネルを意味します。

![](../images/jupyterlab/user-guide/kernel_and_state_1.png)

カーネルがシャットダウンされたり、長時間使用されない場合は、**カーネルなし** が実円と表示されます。カーネルの状態をクリックし、以下に示すように適切なカーネルタイプを選択して、カーネルをアクティブにします。

![](../images/jupyterlab/user-guide/switch_kernel.gif)

### ランチャー {#launcher}

[//]: # (Talk about the different Notebooks, introduce that certain starter notebooks are limited to particular kernels)

カスタマイズされた *Launcher* は、次のような、サポートされているカーネル用の便利なノートブックテンプレートを提供し、タスクを開始するのに役立ちます。

| テンプレート | 説明 |
| --- | --- |
| 空白 | 空のノートブックファイル。 |
| スターター | サンプルデータを使用したデータ調査を示す、事前入力済みのノートブック。 |
| 小売売上 | サンプルデータを使用した[小売売上のレシピ](https://docs.adobe.com/content/help/ja-JP/experience-platform/data-science-workspace/home.translate.html#!api-specification/markdown/narrative/technical_overview/data_science_workspace_overview/dsw_prebuilt_recipes/retail_sales_recipe/retail_sales_recipe.md)を扱う、事前入力済みのノートブック。 |
| レシピビルダー | [!DNL JupyterLab]でレシピを作成するためのノートブックテンプレート。 レシピの作成プロセスを示し、説明するコードとコメントが事前に記入されています。詳細な順を追った説明については、『[ノートブックのレシピチュートリアル](https://www.adobe.com/go/data-science-create-recipe-notebook-tutorial-en)』を参照してください。 |
| [!DNL Query Service] | [!DNL Query Service]を[!DNL JupyterLab]で直接使用していることを示す事前入力ノートブック。データをスケールで分析するサンプルワークフローが付属しています。 |
| XDM イベント | データ構造全体に共通の機能に焦点を当てた、ポストバリューエクスペリエンスイベントデータのデータ探索を示す、事前入力済みのノートブック。 |
| XDM クエリ | エクスペリエンスのイベントデータに関するサンプルのビジネスクエリを示す、事前入力済みのノートブック。 |
| 集計 | 大量のデータをより小さく管理しやすいチャンクに集約するサンプルワークフローを示す、事前入力済みのノートブック。 |
| クラスタリング | クラスタリングアルゴリズムを使用したエンドツーエンドの機械学習モデリングプロセスを示す、事前入力済みのノートブック。 |

一部のノートブックテンプレートは、特定のカーネルに限定されています。各カーネルのテンプレートの可用性は、次の表にマッピングされます。

<table>
    <tr>
        <td></td>
        <th><strong>空白</strong></th>
        <th><strong>スターター</strong></th>
        <th><strong>小売売上</strong></th>
        <th><strong>レシピビルダー</strong></th>
        <th><strong>[!DNLクエリサービス]</strong></th>
        <th><strong>XDM イベント</strong></th>
        <th><strong>XDM クエリ</strong></th>
        <th><strong>集計</strong></th>
        <th><strong>クラスタリング</strong></th>
    </tr>
    <tr>
        <th><strong>[!DNL Python]</strong></th>
        <td >○</td>
        <td >○</td>
        <td >○</td>
        <td >○</td>
        <td >○</td>
        <td >○</td>
        <td >いいえ</td>
        <td >いいえ</td>
        <td >いいえ</td>
    </tr>
    <tr>
        <th ><strong>R</strong></th>
        <td >○</td>
        <td >○</td>
        <td >○</td>
        <td >いいえ</td>
        <td >いいえ</td>
        <td >いいえ</td>
        <td >いいえ</td>
        <td >いいえ</td>
        <td >いいえ</td>
    </tr>
      <tr>
        <th  ><strong>PySpark 3 ([!DNL Spark] 2.4)</strong></th>
        <td >いいえ</td>
        <td >○</td>
        <td >いいえ</td>
        <td >いいえ</td>
        <td >いいえ</td>
        <td >いいえ</td>
        <td >○</td>
        <td >○</td>
        <td >いいえ</td>
    </tr>
    <tr>
        <th ><strong>Scala</strong></th>
        <td >○</td>
        <td >○</td>
        <td >いいえ</td>
        <td >いいえ</td>
        <td >いいえ</td>
        <td >いいえ</td>
        <td >いいえ</td>
        <td >いいえ</td>
        <td >○</td>
    </tr>
</table>

新しい&#x200B;*ランチャー*&#x200B;を開くには、**ファイル／新規ランチャー**&#x200B;をクリックします。または、左のサイドバーで&#x200B;**ファイルブラウザー**&#x200B;を展開し、プラス記号（**+**）をクリックします。

![](../images/jupyterlab/user-guide/new_launcher.gif)

### [!DNL Python]/RのGPUとメモリサーバーの設定

[!DNL JupyterLab]で、右上隅のギアアイコンを選択し、*ノートブックサーバーの設定*&#x200B;を開きます。 GPUのオン/オフを切り替え、スライダーを使用して必要なメモリ量を割り当てることができます。 割り当て可能なメモリの量は、組織でプロビジョニングされているメモリの量によって異なります。 「**[!UICONTROL 設定を更新]**」を選択して保存します。

>[!NOTE]
>
>1つの組織でノートブック用にプロビジョニングされるGPUは1つだけです。 GPUが使用中の場合は、現在GPUを予約しているユーザーがGPUを解放するのを待つ必要があります。 これは、GPUをログアウトするか、4時間以上アイドル状態のままにすることで実行できます。

![](../images/jupyterlab/user-guide/notebook-gpu-config.png)

## 次の手順

サポートされる各ノートブックとその使用方法の詳細については、[Jupyterlab notebooks data access](./access-notebook-data.md)開発者ガイドを参照してください。 このガイドでは、JupyterLabノートブックを使用して、データの読み取り、書き込み、クエリなど、データにアクセスする方法について説明します。 また、サポートされる各ノートブックで読み取れる最大データ量に関する情報も記載されています。

## サポートされるライブラリ {#supported-libraries}

### [!DNL Python] / R

| ライブラリ | バージョン |
| :------ | :------ |
| notebook | 6.0.0 |
| requests | 2.22.0 |
| plotly | 4.0.0 |
| folium | 0.10.0 |
| ipywidgets | 7.5.1 |
| bokeh | 1.3.1 |
| gensim | 3.7.3 |
| ipyparallel | 0.5.2 |
| jq | 1.6 |
| keras | 2.2.4 |
| nltk | 3.2.5 |
| pandas | 0.22.0 |
| pandasql | 0.7.3 |
| pillow | 6.0.0 |
| scikit-image | 0.15.0 |
| scikit-learn | 0.21.3 |
| scipy | 1.3.0 |
| scrapy | 1.3.0 |
| seaborn | 0.9.0 |
| statsmodels | 0.10.1 |
| elastic | 5.1.0.17 |
| ggplot | 0.11.5 |
| py-xgboost | 0.90 |
| opencv | 3.4.1 |
| pyspark | 2.4.3 |
| pytorch | 1.0.1 |
| wxpython | 4.0.6 |
| colorlover | 0.3.0 |
| geopandas | 0.5.1 |
| pyshp | 2.1.0 |
| shapely | 1.6.4 |
| rpy2 | 2.9.4 |
| r-essentials | 3.6 |
| r-arules | 1.6_3 |
| r-fpc | 2.2_3 |
| r-e1071 | 1.7_2 |
| r-gam | 1.16.1 |
| r-gbm | 2.1.5 |
| r-ggthemes | 4.2.0 |
| r-ggvis | 0.4.4 |
| r-igraph | 1.2.4.1 |
| r-leaps | 3.0 |
| r-manipulate | 1.0.1 |
| r-rocr | 1.0_7 |
| r-rmysql | 0.10.17 |
| r-rodbc | 1.3_15 |
| r-rsqlite | 2.1.2 |
| r-rstan | 2.19.2 |
| r-sqldf | 0.4_11 |
| r-survival | 2.44_1.1 |
| r-zoo | 1.8_6 |
| r-stringdist | 0.9.5.2 |
| r-quadprog | 1.5_7 |
| r-rjson | 0.2.20 |
| r-forecast | 8.7 |
| r-rsolnp | 1.16 |
| r-reticulate | 1.12 |
| r-mlr | 2.14.0 |
| r-viridis | 0.5.1 |
| r-corrplot | 0.84 |
| r-fnn | 1.1.3 |
| r-lubridate | 1.7.4 |
| r-randomforest | 4.6_14 |
| r-tidyverse | 1.2.1 |
| r-tree | 1.0_39 |
| pymongo | 3.8.0 |
| pyarrow | 0.14.1 |
| boto3 | 1.9.199 |
| ipyvolume | 0.5.2 |
| fastparquet | 0.3.2 |
| python-snappy | 0.5.4 |
| ipywebrtc | 0.5.0 |
| jupyter_client | 5.3.1 |
| wordcloud | 1.5.0 |
| graphviz | 2.40.1 |
| python-graphviz | 0.11.1 |
| azure-storage | 0.36.0 |
| [!DNL jupyterlab] | 1.0.4 |
| pandas_ml | 0.6.1 |
| tensorflow-gpu | 1.14.0 |
| nodejs | 12.3.0 |
| mock | 3.0.5 |
| ipympl | 0.3.3 |
| fonts-anacond | 1.0 |
| psycopg2 | 2.8.3 |
| nose | 1.3.7 |
| autovizwidget | 0.12.9 |
| altair | 3.1.0 |
| vega_datasets | 0.7.0 |
| papermill | 1.0.1 |
| sql_magic | 0.0.4 |
| iso3166 | 1.0 |
| nbimporter | 0.3.1 |

### PySpark

| ライブラリ | バージョン |
| :------ | :------ |
| リクエスト | 2.18.4 |
| 幻 | 2.3.0 |
| ケラス | 2.0.6 |
| nltk | 3.2.4 |
| パンダ | 0.20.1 |
| pandasql | 0.7.3 |
| 枕 | 5.3.0 |
| scikit-image | 0.13.0 |
| サイキット学習 | 0.19.0 |
| 汚い | 0.19.1 |
| 擦り傷 | 1.3.3 |
| statsmodels | 0.8.0 |
| 弾性の | 4.0.30.44 |
| py-xgboost | 0.60 |
| opencv | 3.1.0 |
| 矢 | 0.8.0 |
| boto3 | 1.5.18 |
| azure-storage-blob | 1.4.0 |
| [!DNL python] | 3.6.7 |
| mkl-rt | 11.1 |