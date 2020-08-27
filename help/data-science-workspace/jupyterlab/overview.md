---
keywords: Experience Platform;JupyterLab;notebooks;Data Science Workspace;popular topics;jupyterlab
solution: Experience Platform
title: JupyterLab ユーザーガイド
topic: Overview
description: JupyterLab は、プロジェクト Jupyter の Web ベースのユーザーインターフェイスで、Adobe Experience Platform に緊密に統合されています。これは、データ科学者が Jupyter のノートブック、コード、データを扱うための、インタラクティブな開発環境を提供します。このドキュメントでは、JupyterLab とその機能の概要と、一般的なアクションを実行する手順を説明します。
translation-type: tm+mt
source-git-commit: 78f080fd7598799825c59a4fdfdcaf7d294560a3
workflow-type: tm+mt
source-wordcount: '3702'
ht-degree: 55%

---


# [!DNL JupyterLab] ユーザーガイド

[!DNL JupyterLab] は、[プロジェクト Jupyter](https://jupyter.org/) の Web ベースのユーザーインターフェイスで、Adobe Experience Platform に緊密に統合されています。これは、データ科学者が Jupyter のノートブック、コード、データを扱うための、インタラクティブな開発環境を提供します。

This document provides an overview of [!DNL JupyterLab] and its features as well as instructions to perform common actions.

## [!DNL JupyterLab] on [!DNL Experience Platform]

Experience Platform の JupyterLab 統合には、アーキテクチャの変更、デザイン上の考慮事項、カスタマイズされたノートブック拡張機能、プリインストールされたライブラリ、アドビをテーマにしたインターフェイスが付属しています。

次のリストでは、Platform 上の JupyterLab に固有の機能の一部を説明します。

| 機能 | 説明 |
| --- | --- |
| **カーネル** | Kernels provide notebook and other [!DNL JupyterLab] front-ends the ability to execute and introspect code in different programming languages. [!DNL Experience Platform] は、 [!DNL Python]、R、PySpark、およびでの開発をサポートする追加カーネルを提供 [!DNL Spark]します。 詳しくは「[カーネル](#kernels)」の節を参照してください。 |
| **データアクセス** | Access existing datasets directly from within [!DNL JupyterLab] with full support for read and write capabilities. |
| **[!DNL Platform]サービス統合** | Built-in integrations allows you to utilize other [!DNL Platform] services directly from within [!DNL JupyterLab]. サポートされる統合の完全なリストは、「[他の Platform サービスとの統合](#service-integration)」の節に記載されています。 |
| **認証** | <a href="https://jupyter-notebook.readthedocs.io/en/latest/security.html" target="_blank">JupyterLab の組み込みのセキュリティモデル</a>に加えて、Platform のサービス間通信を含む、アプリケーションと Experience Platform の間のすべてのやり取りは、<a href="https://www.adobe.io/authentication/auth-methods.html" target="_blank">[!DNL Adobe Identity Management System]（IMS）</a>を通じて暗号化され、認証されます。 |
| **開発ライブラリ** | では、 [!DNL Experience Platform]、R、PySpark用に事前にインストールされたライブラリを [!DNL JupyterLab][!DNL Python]提供します。 サポートされているライブラリの完全なリストについては、[付録](#supported-libraries)を参照してください。 |
| **ライブラリコントローラー** | When the the pre-installed libraries are lacking for your needs, additional libraries can be installed for Python and R, and are temporarily stored in isolated containers to maintain the integrity of [!DNL Platform] and keep your data safe. 詳しくは「[カーネル](#kernels)」の節を参照してください。 |

>[!NOTE]
>
> 追加のライブラリは、インストールされたセッションでのみ使用できます。新しいセッションを開始する際に必要な追加のライブラリを再インストールする必要があります。

## Integration with other [!DNL Platform] services {#service-integration}

Standardization and interoperability are key concepts behind [!DNL Experience Platform]. The integration of [!DNL JupyterLab] on [!DNL Platform] as an embedded IDE allows it to interact with other [!DNL Platform] services, enabling you to utilize [!DNL Platform] to its full potential. The following [!DNL Platform] services are available in [!DNL JupyterLab]:

* **[!DNL Catalog Service]:** 読み取り/書き込み機能を備えたデータセットへのアクセスと調査
* **[!DNL Query Service]:** SQLを使用してデータセットにアクセスし、データセットを調査します。大量のデータを処理する際に、データ・アクセスのオーバーヘッドが低くなります。
* **[!DNL Sensei ML Framework]:** データのトレーニングとスコア機能を備えたモデル開発と、1回のクリックでレシピを作成。
* **[!DNL Experience Data Model (XDM)]:** 標準化と相互運用性は、Adobe Experience Platformの主な概念です。 [Adobeに基づくExperience Data Model(XDM)](https://www.adobe.com/go/xdm-home-en)は、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

>[!NOTE]
>
>Some [!DNL Platform] service integrations on [!DNL JupyterLab] are limited to specific kernels. 詳しくは「[カーネル](#kernels)」の節を参照してください。

## 主な機能と一般的な操作

Information regarding key features of [!DNL JupyterLab] and instructions on performing common operations are provided in the sections below:

* [JupyterLab へのアクセス](#access-jupyterlab)
* [JupyterLab インターフェイス](#jupyterlab-interface)
* [コードセル](#code-cells)
* [カーネル](#kernels)
* [カーネルセッション](#kernel-sessions)
* [PySpark／Spark 実行リソース](#execution-resource)
* [ランチャー](#launcher)

### Access [!DNL JupyterLab] {#access-jupyterlab}

「 [Adobe Experience Platform](https://platform.adobe.com)」で、左のナビゲーション列から「 **ノートブック** 」を選択します。 Allow some time for [!DNL JupyterLab] to fully initialize.

![](../images/jupyterlab/user-guide/access_jupyterlab.png)

### [!DNL JupyterLab] インターフェイス {#jupyterlab-interface}

The [!DNL JupyterLab] interface consists of a menu bar, a collapsible left sidebar, and the main work area containing tabs of documents and activities.

**メニューバー**

The menu bar at the top of the interface has top-level menus that expose actions available in [!DNL JupyterLab] with their keyboard shortcuts:

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

The main work area in [!DNL JupyterLab] enables you to arrange documents and other activities into panels of tabs that can be resized or subdivided. タブをタブパネルの中央にドラッグして、タブを移行します。タブをパネルの左、右、上または下にドラッグして、パネルを分割します。

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

ノートのカーネルは、ノートのセルを処理するための言語固有のコンピューティングエンジンです。In addition to [!DNL Python], [!DNL JupyterLab] provides additional language support in R, PySpark, and [!DNL Spark] (Scala). ノートブックドキュメントを開くと、関連するカーネルが起動します。ノートブックセルが実行されると、カーネルは計算をおこない、結果を生成し、CPU やメモリリソースを大量に消費する可能性があります。割り当てられたメモリは、カーネルがシャットダウンされるまで解放されないことに注意してください。

特定の機能は、以下の表で説明するように、特定のカーネルに限定されています。

| カーネル | ライブラリのインストールサポート | [!DNL Platform] 統合 |
| :----: | :--------------------------: | :-------------------- |
| **[!DNL Python]** | ○ | <ul><li>[!DNL Sensei ML Framework]</li><li>[!DNL Catalog Service]</li><li>[!DNL Query Service]</li></ul> |
| **R** | ○ | <ul><li>[!DNL Sensei ML Framework]</li><li>[!DNL Catalog Service]</li></ul> |
| **Scala** | × | <ul><li>[!DNL Sensei ML Framework]</li><li>[!DNL Catalog Service]</li></ul> |

### カーネルセッション {#kernel-sessions}

Each active notebook or activity on [!DNL JupyterLab] utilizes a kernel session. すべてのアクティブなセッションは、左側のサイドバーから「**実行中の端末とカーネル**」タブを展開すると見つかります。ノートブックのカーネルのタイプと状態は、ノートブックのインターフェースの右上を見ることで識別できます。下の図では、ノートブックに関連するカーネルは **[!DNL Python]3** で、現在の状態は右側に灰色の円で表されています。中空の円はアイドルカーネルを意味し、実円はビジーカーネルを意味します。

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
| 小売売上 | A pre-filled notebook featuring the [retail sales recipe](https://docs.adobe.com/content/help/ja-JP/experience-platform/data-science-workspace/home.translate.html#!api-specification/markdown/narrative/technical_overview/data_science_workspace_overview/dsw_prebuilt_recipes/retail_sales_recipe/retail_sales_recipe.md) using sample data. |
| レシピビルダー | A notebook template for creating a recipe in [!DNL JupyterLab]. レシピの作成プロセスを示し、説明するコードとコメントが事前に記入されています。詳細な順を追った説明については、『[ノートブックのレシピチュートリアル](https://docs.adobe.com/content/help/ja-JP/experience-platform/data-science-workspace/jupyterlab/create-a-recipe.html)』を参照してください。 |
| [!DNL Query Service] | A pre-filled notebook demonstrating the usage of [!DNL Query Service] directly in [!DNL JupyterLab] with provided sample workflows that analyzes data at scale. |
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

### GPUと [!DNL Python]/Rでのメモリサーバーの設定

で、右上隅の歯車アイコンを [!DNL JupyterLab] 選択して、 *ノートブックサーバー設定を開きます*。 GPUのオン/オフを切り替え、スライダーを使用して必要なメモリ量を割り当てることができます。 割り当て可能なメモリの量は、組織でプロビジョニングされているメモリの量によって異なります。 「 **[!UICONTROL 設定を更新]** 」を選択して保存します。

>[!NOTE]
>
>1つの組織でノートブック用にプロビジョニングされるGPUは1つだけです。 GPUが使用中の場合は、現在GPUを予約しているユーザーがGPUを解放するのを待つ必要があります。 これは、GPUをログアウトするか、4時間以上アイドル状態のままにすることで実行できます。

![](../images/jupyterlab/user-guide/notebook-gpu-config.png)

## Access [!DNL Platform] data using Notebooks

Each supported kernel provides built-in functionalities that allow you to read [!DNL Platform] data from a dataset within a notebook. However, support for paginating data is limited to [!DNL Python] and R notebooks.

### ノートブックデータの制限

次の情報は、読み取り可能な最大データ量、使用されたデータの種類、およびデータ読み取りにかかる予測時間枠を定義します。 お [!DNL Python] よびRの場合、40 GBのRAMで構成されたノートブックサーバがベンチマークに使用されました。 PySparkとScalaの場合、64GBのRAM、8コア、2 DBUで構成され、最大4人のワーカーを持つデータバリッククラスタが、以下のベンチマークに使用されました。

使用されるExperienceEventスキーマデータのサイズは、1,000行（1,000行）から10億行（1B行）まで様々です。 PySparkと [!DNL Spark] 指標の場合、XDMデータには10日の期間が使われていました。

アドホックスキーマデータは、「テーブルを選択として [!DNL Query Service] 作成(CTAS)」を使用して事前に処理されました。 また、1000行(1K)から10億行(1B)まで様々なサイズがあります。

#### [!DNL Python] ノート型データの制限

**XDM ExperienceEventスキーマ:** 22分以内に、最大200万行（ディスク上の6.1 GBのデータ）のXDMデータを読み取れるはずです。 行を追加すると、エラーが発生する場合があります。

| 行数 | 1K | 10K | 100K | 1M | 2M |
| ----------------------- | ------ | ------ | ----- | ----- | ----- |
| ディスク上のサイズ(MB) | 18.73 | 187.5 | 308 | 3000 | 6050 |
| SDK （秒） | 20.3 | 86.8 | 63 | 659 | 1315 |

**アドホックスキーマ:** 非XDM（アドホック）データの最大500万行（ディスク上の5.6 GBのデータ）を14分未満で読み取ることができます。 行を追加すると、エラーが発生する場合があります。

| 行数 | 1K | 10K | 100K | 1M | 2M | 3M | 5M |
| ----------------------- | ------- | ------- | ----- | ----- | ----- | ----- | ------ |
| ディスク上のサイズ(MB) | 1.21 | 11.72 | 115 | 1120 | 2250 | 3380 | 5630 |
| SDK （秒） | 7.27 | 9.04 | 27.3 | 180 | 346 | 487 | 819 |

#### Rノートブックデータの制限

**XDM ExperienceEventスキーマ:** 13分以内に、最大100万行のXDMデータ(3GBのデータをディスクに読み込むことができます。

| 行数 | 1K | 10K | 100K | 1M |
| ----------------------- | ------ | ------ | ----- | ----- |
| ディスク上のサイズ(MB) | 18.73 | 187.5 | 308 | 3000 |
| Rカーネル（秒） | 14.03 | 69.6 | 86.8 | 775 |

**アドホックスキーマ:** 約10分で、アドホックデータ（ディスク上のデータは293 MB）の最大行数300万行を読み取ることができます。

| 行数 | 1K | 10K | 100K | 1M | 2M | 3M |
| ----------------------- | ------- | ------- | ----- | ----- | ----- | ----- |
| ディスク上のサイズ(MB) | 0.082 | 0.612 | 9.0 | 91 | 188 | 293 |
| R SDK （イン秒） | 7.7 | 4.58 | 35.9 | 233 | 470.5 | 603 |

#### PySpark ([!DNL Python] カーネル)ノートブックのデータ制限：

**XDM ExperienceEventスキーマ:** Interactiveモードでは、約20分で最大500万行（ディスク上の13.42 GBのデータ）のXDMデータを読み取れるはずです。 インタラクティブモードでは、最大500万行までの行のみサポートされます。 より大きなデータセットを読み取る場合は、バッチモードに切り替えることをお勧めします。 バッチモードでは、約14時間で最大5億行（ディスク上のデータは最大1.31 TB）のXDMデータを読み取れるはずです。

| 行数 | 1K | 10K | 100K | 1M | 2M | 3M | 5M | 10M | 50M | 100M | 500M |
|-------------------------|--------|--------|-------|-------|-------|-------|---------|---------|----------|--------|--------|
| ディスク上のサイズ | 2.93MB | 4.38MB | 29.02 | 2.69 GB | 5.39 GB | 8.09 GB | 13.42 GB | 26.82 GB | 134.24 GB | 268.39 GB | 1.31TB |
| SDK（インタラクティブモード） | 33s | 32.4s | 55.1s | 253.5s | 489.2s | 729.6s | 1206.8s | - | - | - | - |
| SDK（バッチモード） | 815.8s | 492.8s | 379.1s | 637.4s | 624.5s | 869.2s | 1104.1s | 1786s | 5387.2s | 10624.6s | 50547s |

**アドホックスキーマ:** Interactiveモードでは、XDM以外のデータを最大10億行（ディスク上のデータは最大1.05 TB）読み取ることができます（3分以内）。 バッチモードでは、約18分で最大10億行（ディスク上のデータは最大1.05 TB）の非XDMデータを読み取れるはずです。

| 行数 | 1K | 10K | 100K | 1M | 2M | 3M | 5M | 10M | 50M | 100M | 500M | 1B |
|--------------|--------|---------|---------|-------|-------|-------|--------|--------|---------|--------|---------|-------|
| ディスク上のサイズ | 1.12MB | 11.24MB | 109.48MB | 2.69 GB | 2.14 GB | 3.21 GB | 5.36 GB | 10.71 GB | 53.58 GB | 107.52 GB | 535.88 GB | 1.05TB |
| SDKインタラクティブモード（秒） | 28.2s | 18.6s | 20.8s | 20.9s | 23.8s | 21.7s | 24.7s | 22s | 28.4s | 40s | 97.4s | 154.5s |
| SDKバッチモード（秒） | 428.8s | 578.8s | 641.4s | 538.5s | 630.9s | 467.3s | 411s | 675s | 702s | 719.2s | 1022.1s | 1122.3s |

#### [!DNL Spark] （Scalaカーネル）ノートブックデータの制限：

**XDM ExperienceEventスキーマ:** Interactiveモードでは、約18分で最大500万行（ディスク上の13.42 GBのデータ）のXDMデータを読み取れるはずです。 インタラクティブモードでは、最大500万行までの行のみサポートされます。 より大きなデータセットを読み取る場合は、バッチモードに切り替えることをお勧めします。 バッチモードでは、約14時間で最大5億行（ディスク上のデータは最大1.31 TB）のXDMデータを読み取れるはずです。

| 行数 | 1K | 10K | 100K | 1M | 2M | 3M | 5M | 10M | 50M | 100M | 500M |
|---------------|--------|--------|-------|-------|-------|-------|---------|---------|----------|--------|--------|
| ディスク上のサイズ | 2.93MB | 4.38MB | 29.02 | 2.69 GB | 5.39 GB | 8.09 GB | 13.42 GB | 26.82 GB | 134.24 GB | 268.39 GB | 1.31TB |
| SDKインタラクティブモード（秒） | 37.9s | 22.7s | 45.6s | 231.7s | 444.7s | 660.6s | 1100s | - | - | - | - |
| SDKバッチモード（秒） | 374.4s | 398.5s | 527s | 487.9s | 588.9s | 829s | 939.1s | 1441s | 5473.2s | 10118.8 | 49207.6 |

**アドホックスキーマ:** Interactiveモードでは、XDM以外のデータを最大10億行（ディスク上のデータは最大1.05 TB）読み取ることができます（3分以内）。 バッチモードでは、約16分で最大10億行（ディスク上の最大1.05 TBのデータ）の非XDMデータを読み取れるはずです。

| 行数 | 1K | 10K | 100K | 1M | 2M | 3M | 5M | 10M | 50M | 100M | 500M | 1B |
|--------------|--------|---------|---------|-------|-------|-------|---------|---------|---------|--------|---------|-------|
| ディスク上のサイズ | 1.12MB | 11.24MB | 109.48MB | 2.69 GB | 2.14 GB | 3.21 GB | 5.36 GB | 10.71 GB | 53.58 GB | 107.52 GB | 535.88 GB | 1.05TB |
| SDKインタラクティブモード（秒） | 35.7s | 31s | 19.5s | 25.3s | 23s | 33.2s | 25.5s | 29.2s | 29.7s | 36.9s | 83.5s | 139s |
| SDKバッチモード（秒） | 448.8s | 459.7s | 519s | 475.8s | 599.9s | 347.6s | 407.8s | 397s | 518.8s | 487.9s | 760.2s | 975.4s |

### Read from a dataset in [!DNL Python]/R

[!DNL Python] ノートブックと R ノートブックでは、データセットにアクセスする際にデータをページ番号付けできます。ページ番号付けの有無に関わらずデータを読み取るコード例を以下に示します。

[//]: # (In the following samples, the first step is currently required but once the SDK is complete, users are no longer required to explicitly define client_context)

#### Read from a dataset in [!DNL Python]/R without pagination

次のコードを実行すると、データセット全体が読み取られます。実行が成功した場合、データは `df` 変数で参照される Pandas データフレームとして保存されます。

```python
# Python

client_context = PLATFORM_SDK_CLIENT_CONTEXT
from platform_sdk.dataset_reader import DatasetReader
dataset_reader = DatasetReader(client_context, "{DATASET_ID}")
df = dataset_reader.read()
df.head()
```

```R
# R

library(reticulate)
use_python("/usr/local/bin/ipython")
psdk <- import("platform_sdk")
py_run_file("../.ipython/profile_default/startup/platform_sdk_context.py")
client_context <- py$PLATFORM_SDK_CLIENT_CONTEXT
DatasetReader <- psdk$dataset_reader$DatasetReader
dataset_reader <- DatasetReader(client_context, "{DATASET_ID}") 
df <- dataset_reader$read() 
df
```

* `{DATASET_ID}`：アクセスするデータセットの固有の ID

#### Read from a dataset in [!DNL Python]/R with pagination

次のコードを実行すると、指定したデータセットからデータが読み取られます。ページ番号付けは、`limit()` 関数を使用してデータを制限し、`offset()` 関数を使用してデータをオフセットすることで実現します。データの制限とは、読み取るデータポイントの最大数を指し、オフセットとは、データの読み取り前にスキップするデータポイントの数を指します。読み取り操作が正常に実行された場合、データは `df` 変数が参照する Pandas データフレームとして保存されます。

```python
# Python

client_context = PLATFORM_SDK_CLIENT_CONTEXT
from platform_sdk.dataset_reader import DatasetReader

dataset_reader = DatasetReader(client_context, "{DATASET_ID}")
df = dataset_reader.limit(100).offset(10).read()
```

```R
# R

library(reticulate)
use_python("/usr/local/bin/ipython")
psdk <- import("platform_sdk")
py_run_file("../.ipython/profile_default/startup/platform_sdk_context.py")
client_context <- py$PLATFORM_SDK_CLIENT_CONTEXT

DatasetReader <- psdk$dataset_reader$DatasetReader
dataset_reader <- DatasetReader(client_context, "{DATASET_ID}") 
df <- dataset_reader$limit(100L)$offset(10L)$read() 
```

* `{DATASET_ID}`：アクセスするデータセットの固有の ID

### Read from a dataset in PySpark/[!DNL Spark]/Scala

With an an active PySpark or Scala notebook opened, expand the **Data Explorer** tab from the left sidebar and double click **Datasets** to view a list of available datasets. Right-click on the dataset listing you wish to access and click **Explore Data in Notebook**. 次のコードセルが生成されます。

#### PySpark ([!DNL Spark] 2.4) {#pyspark2.4}

Spark 2.4の導入に伴い、 [`%dataset`](#magic) カスタムマジックが提供されます。

```python
# PySpark 3 (Spark 2.4)

%dataset read --datasetId {DATASET_ID} --dataFrame pd0
pd0.describe()
pd0.show(10, False)
```

#### Scala ([!DNL Spark] 2.4) {#spark2.4}

```scala
// Scala (Spark 2.4)

// initialize the session
import org.apache.spark.sql.{Dataset, SparkSession}
val spark = SparkSession.builder().master("local").getOrCreate()

val dataFrame = spark.read.format("com.adobe.platform.query")
    .option("user-token", sys.env("PYDASDK_IMS_USER_TOKEN"))
    .option("ims-org", sys.env("IMS_ORG_ID"))
    .option("api-key", sys.env("PYDASDK_IMS_CLIENT_ID"))
    .option("service-token", sys.env("PYDASDK_IMS_SERVICE_TOKEN"))
    .option("mode", "batch")
    .option("dataset-id", "{DATASET_ID}")
    .load()
dataFrame.printSchema()
dataFrame.show()
```

>[!TIP]
>
>Scalaでは、を使用して、内から値 `sys.env()` を宣言して返すことができ `option`ます。

### PySpark 3 ([!DNL Spark] 2.4)ノートブックで%dataset magicを使う {#magic}

2.4の導入に伴い、新しいPySpark 3 ( [!DNL Spark] 2.4)ノートブック( `%dataset`[!DNL Spark][!DNL Python] 3カーネル)で使用するカスタムマジックが提供されます。

**用途**

`%dataset {action} --datasetId {id} --dataFrame {df}`

**説明**

ノートブック( [!DNL Data Science Workspace][!DNL Python][!DNL Python] 3カーネル)からデータセットを読み取ったり書き込んだりするためのカスタムマジックコマンド。

* **{action}**:データセットに対して実行するアクションのタイプ。 「読み取り」と「書き込み」の2つのアクションを使用できます。
* **—datasetId {id}**:読み取りまたは書き込みを行うデータセットのIDを指定するために使用します。 これは必須の引数です。
* **—dataFrame {df}**:パンダのデータフレーム。 これは必須の引数です。
   * アクションが「read」の場合、{df}は、データセット読み取り操作の結果を利用できる変数です。
   * アクションが「書き込み」の場合、このデータフレーム{df}はデータセットに書き込まれます。
* **—mode（オプション）**:使用できるパラメーターは、「batch」および「interactive」です。 デフォルトでは、モードは「interactive」に設定されています。 大量のデータを読み取る場合は、「バッチ」モードを使用することをお勧めします。

**例**

* **次の例を読みます**。 `%dataset read --datasetId 5e68141134492718af974841 --dataFrame pd0`
* **次に例を示します**。 `%dataset write --datasetId 5e68141134492718af974842 --dataFrame pd0`

### で使用するクエリデ [!DNL Query Service] ータ [!DNL Python]

[!DNL JupyterLab][!DNL Platform][!DNL Python] で を使用すると、 ノートブックで SQL を使用して、[Adobe Experience Platform クエリサービス](https://docs.adobe.com/content/help/ja-JP/experience-platform/query/home.html)を通じてデータにアクセスできます。Accessing data through [!DNL Query Service] can be useful for dealing with large datasets due to its superior running times. Be advised that querying data using [!DNL Query Service] has a processing time limit of ten minutes.

Before you use [!DNL Query Service] in [!DNL JupyterLab], ensure you have a working understanding of the [[!DNL Query Service] SQL syntax](https://docs.adobe.com/content/help/ja-JP/experience-platform/query/home.html#!api-specification/markdown/narrative/technical_overview/query-service/sql/syntax.md).

Querying data using [!DNL Query Service] requires you to provide the name of the target dataset. 必要なコードセルを生成するには、**データエクスプローラー**&#x200B;を使用して目的のデータセットを見つけます。データセットの一覧を右クリックし、「**ノートブック内のデータをクエリ** 」をクリックすると、ノートブックに次の 2 つのコードセルが生成されます。


In order to utilize [!DNL Query Service] in [!DNL JupyterLab], you must first create a connection between your working [!DNL Python] notebook and [!DNL Query Service]. これは、最初に生成されたセルを実行することで達成できます。

```python
qs_connect()
```

2 番目に生成されたセルでは、最初の行を SQL クエリの前に定義する必要があります。デフォルトでは、生成されたセルは、クエリ結果を Pandas データフレームとして保存するオプションの変数（`df0`）を定義します。<br>この `-c QS_CONNECTION` 引数は必須で、SQLクエリを実行するようカーネルに指示 [!DNL Query Service]します。 その他の引数のリストは、[付録](#optional-sql-flags-for-query-service)を参照してください。

```python
%%read_sql df0 -c QS_CONNECTION
SELECT *
FROM name_of_the_dataset
LIMIT 10
/* Querying table "name_of_the_dataset" (datasetId: {DATASET_ID})*/
```

Python 変数は、次の例に示すように、文字列形式の構文を使用して中括弧（`{}`）で囲むことで、SQL クエリ内で直接参照できます。

```python
table_name = 'name_of_the_dataset'
table_columns = ','.join(['col_1','col_2','col_3'])
```

```python
%%read_sql demo -c QS_CONNECTION
SELECT {table_columns}
FROM {table_name}
```

### Filter ExperienceEvent data in [!DNL Python]/R

In order to access and filter an ExperienceEvent dataset in a [!DNL Python] or R notebook, you must provide the ID of the dataset (`{DATASET_ID}`) along with the filter rules that define a specific time range using logical operators. 時間範囲を定義すると、指定されたページ番号は無視され、データセット全体が考慮されます。

フィルタリング操作のリストを以下に示します。

* `eq()`：と等しい
* `gt()`：より大きい
* `ge()`：より大きいか等しい
* `lt()`：より小さい
* `le()`：より小さいか等しい
* `And()`：論理積演算子
* `Or()`：論理和演算子

次のセルでは、ExperienceEvent データセットを、2019 年 1 月 1 日から 2019 年 12 月 31 日の終わりまでの間にのみ存在するデータにフィルターします。

```python
# Python

client_context = PLATFORM_SDK_CLIENT_CONTEXT
from platform_sdk.dataset_reader import DatasetReader

dataset_reader = DatasetReader(client_context, "{DATASET_ID}")
df = dataset_reader.\
    where(dataset_reader["timestamp"].gt("2019-01-01 00:00:00").\
    And(dataset_reader["timestamp"].lt("2019-12-31 23:59:59"))\
).read()
```

```R
# R

library(reticulate)
use_python("/usr/local/bin/ipython")
psdk <- import("platform_sdk")
py_run_file("../.ipython/profile_default/startup/platform_sdk_context.py")
client_context <- py$PLATFORM_SDK_CLIENT_CONTEXT

DatasetReader <- psdk$dataset_reader$DatasetReader
dataset_reader <- DatasetReader(client_context, "{DATASET_ID}") 
df <- dataset_reader$
    where(dataset_reader["timestamp"]$gt("2019-01-01 00:00:00")$
    And(dataset_reader["timestamp"]$lt("2019-12-31 23:59:59"))
)$read()
```

### PySpark／ の ExperienceEvent データのフィルター[!DNL Spark]

Accessing and filtering an ExperienceEvent dataset in a PySpark or Scala notebook requires you to provide the dataset identity (`{DATASET_ID}`), your organization&#39;s IMS identity, and the filter rules defining a specific time range. 時間範囲のフィルタリングは、`spark.sql()` 関数を使用して定義します。関数パラメータは SQL クエリ文字列です。

次のセルでは、ExperienceEvent データセットを、2019 年 1 月 1 日から 2019 年 12 月 31 日の終わりまでの間にのみ存在するデータにフィルターします。

#### PySpark 3 ([!DNL Spark] 2.4) {#pyspark3-spark2.4}

```python
# PySpark 3 (Spark 2.4)

from pyspark.sql import SparkSession
spark = SparkSession.builder.getOrCreate()

%dataset read --datasetId {DATASET_ID} --dataFrame df

df.createOrReplaceTempView("event")
timepd = spark.sql("""
    SELECT *
    FROM event
    WHERE timestamp > CAST('2019-01-01 00:00:00.0' AS TIMESTAMP)
    AND timestamp < CAST('2019-12-31 23:59:59.9' AS TIMESTAMP)
""")
timepd.show()
```

#### Scala ([!DNL Spark] 2.4) {#scala-spark}

```scala
// Spark (Spark 2.4)

// Turn off extra logging
import org.apache.log4j.{Level, Logger}
Logger.getLogger("org").setLevel(Level.OFF)
Logger.getLogger("com").setLevel(Level.OFF)

import org.apache.spark.sql.{Dataset, SparkSession}
val spark = org.apache.spark.sql.SparkSession.builder().appName("Notebook")
  .master("local")
  .getOrCreate()

// Stage Exploratory
val dataSetId: String = "{DATASET_ID}"
val orgId: String = sys.env("IMS_ORG_ID")
val clientId: String = sys.env("PYDASDK_IMS_CLIENT_ID")
val userToken: String = sys.env("PYDASDK_IMS_USER_TOKEN")
val serviceToken: String = sys.env("PYDASDK_IMS_SERVICE_TOKEN")
val mode: String = "batch"

var df = spark.read.format("com.adobe.platform.query")
  .option("user-token", userToken)
  .option("ims-org", orgId)
  .option("api-key", clientId)
  .option("mode", mode)
  .option("dataset-id", dataSetId)
  .option("service-token", serviceToken)
  .load()
df.createOrReplaceTempView("event")
val timedf = spark.sql("""
    SELECT * 
    FROM event 
    WHERE timestamp > CAST('2019-01-01 00:00:00.0' AS TIMESTAMP)
    AND timestamp < CAST('2019-12-31 23:59:59.9' AS TIMESTAMP)
""")
timedf.show()
```

>[!TIP]
>
>Scalaでは、を使用して、内から値 `sys.env()` を宣言して返すことができ `option`ます。 これにより、変数が1回しか使用されないことがわかっている場合に、変数を定義する必要がなくなります。 次の例は、上記の例 `val userToken` から取り出し、インライン内で代替として宣言 `option` します。
> 
```scala
> .option("user-token", sys.env("PYDASDK_IMS_USER_TOKEN"))
> ```

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
| requests | 2.18.4 |
| gensim | 2.3.0 |
| keras | 2.0.6 |
| nltk | 3.2.4 |
| pandas | 0.20.1 |
| pandasql | 0.7.3 |
| pillow | 5.3.0 |
| scikit-image | 0.13.0 |
| scikit-learn | 0.19.0 |
| scipy | 0.19.1 |
| scrapy | 1.3.3 |
| statsmodels | 0.8.0 |
| elastic | 4.0.30.44 |
| py-xgboost | 0.60 |
| opencv | 3.1.0 |
| pyarrow | 0.8.0 |
| boto3 | 1.5.18 |
| azure-storage-blob | 1.4.0 |
| [!DNL python] | 3.6.7 |
| mkl-rt | 11.1 |

## のオプションのSQLフラグ [!DNL Query Service] {#optional-sql-flags-for-query-service}

This table outlines the optional SQL flags that can be used for [!DNL Query Service].

| **フラグ** | **説明** |
| --- | --- |
| `-h`、`--help` | ヘルプメッセージを表示し、終了します。 |
| `-n`、`--notify` | 通知のオプションを切り替えてクエリ結果を通知します。 |
| `-a`、`--async` | このフラグを使用すると、クエリが非同期的に実行され、カーネルの実行中にクエリを解放できます。変数にクエリ結果を割り当てる場合、クエリが完了しない場合は定義されない可能性があるので、注意が必要です。 |
| `-d`、`--display` | このフラグを使用すると、結果が表示されなくなります。 |

