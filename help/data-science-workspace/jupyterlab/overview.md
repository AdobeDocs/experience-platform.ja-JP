---
keywords: Experience Platform;JupyterLab；ノートブック；Data Science Workspace；人気の高いトピック；jupyterlab
solution: Experience Platform
title: JupyterLab UI の概要
topic-legacy: Overview
description: JupyterLab は、プロジェクト Jupyter の Web ベースのユーザーインターフェイスで、Adobe Experience Platform に緊密に統合されています。これは、データサイエンティストが Jupyter Notebook、コード、データを操作するための、インタラクティブな開発環境を提供します。 このドキュメントでは、JupyterLab とその機能の概要と、一般的なアクションを実行する手順を説明します。
exl-id: 13786fbd-ef16-49cd-8bcf-46320c33e902
source-git-commit: 1d3981c67c86f93394acf49b61bd29154e9653e8
workflow-type: tm+mt
source-wordcount: '1821'
ht-degree: 56%

---

# [!DNL JupyterLab] UI の概要

[!DNL JupyterLab] は、[プロジェクト Jupyter](https://jupyter.org/) の Web ベースのユーザーインターフェイスで、Adobe Experience Platform に緊密に統合されています。これは、データサイエンティストが Jupyter Notebook、コード、データを操作するための、インタラクティブな開発環境を提供します。

このドキュメントでは、 [!DNL JupyterLab] とその機能、および一般的なアクションを実行するための手順を説明します。

## [!DNL JupyterLab] on [!DNL Experience Platform]

Experience Platform の JupyterLab 統合には、アーキテクチャの変更、デザイン上の考慮事項、カスタマイズされたノートブック拡張機能、プリインストールされたライブラリ、アドビをテーマにしたインターフェイスが付属しています。

次のリストでは、Platform 上の JupyterLab に固有の機能の一部を説明します。

| 機能 | 説明 |
| --- | --- |
| **カーネル** | カーネルはノートブックなどを提供します [!DNL JupyterLab] フロントエンドは、異なるプログラミング言語でコードを実行および内観する機能をエンドにします。 [!DNL Experience Platform] での開発をサポートする追加のカーネルを提供します。 [!DNL Python]、R、PySpark、 [!DNL Spark]. 詳しくは「[カーネル](#kernels)」の節を参照してください。 |
| **データアクセス** | 内から直接既存のデータセットにアクセス [!DNL JupyterLab] 読み取り/書き込み機能を完全にサポート |
| **[!DNL Platform]サービス統合** | 組み込みの統合により、他の [!DNL Platform] 内から直接サービスを提供 [!DNL JupyterLab]. サポートされる統合の完全なリストは、「[他の Platform サービスとの統合](#service-integration)」の節に記載されています。 |
| **認証** | <a href="https://jupyter-notebook.readthedocs.io/en/stable/security.html" target="_blank">JupyterLab の組み込みのセキュリティモデル</a>に加えて、Platform のサービス間通信を含む、アプリケーションと Experience Platform の間のすべてのやり取りは、<a href="https://www.adobe.io/authentication/auth-methods.html" target="_blank">[!DNL Adobe Identity Management System]（IMS）</a>を通じて暗号化され、認証されます。 |
| **開発ライブラリ** | In [!DNL Experience Platform], [!DNL JupyterLab] は、次の用に事前にインストールされたライブラリを提供します。 [!DNL Python]、R および PySpark。 サポートされているライブラリの完全なリストについては、[付録](#supported-libraries)を参照してください。 |
| **ライブラリコントローラー** | プリインストールされたライブラリがニーズに合わない場合は、Python と R 用に追加のライブラリをインストールし、の整合性を維持するために、一時的に分離されたコンテナに保存します。 [!DNL Platform] データを安全に保ちます。 詳しくは「[カーネル](#kernels)」の節を参照してください。 |

>[!NOTE]
>
> 追加のライブラリは、インストールされたセッションでのみ使用できます。新しいセッションを開始する際に必要な追加のライブラリを再インストールする必要があります。

## 他との統合 [!DNL Platform] サービス {#service-integration}

標準化と相互運用性は、[!DNL Experience Platform] の背後にある重要な概念です。の統合 [!DNL JupyterLab] オン [!DNL Platform] 埋め込み IDE を使用すると、他の IDE とのやり取りが可能 [!DNL Platform] サービス，利用可能 [!DNL Platform] 完全な可能性に 以下 [!DNL Platform] サービスは、 [!DNL JupyterLab]:

* **[!DNL Catalog Service]:** 読み取りと書き込み機能を使用してデータセットにアクセスし、調査します。
* **[!DNL Query Service]:** SQL を使用してデータセットにアクセスし、調査します。大量のデータを処理する際に、データアクセスのオーバーヘッドが低くなります。
* **[!DNL Sensei ML Framework]:** データのトレーニングとスコア、および 1 回のクリックでレシピを作成できるモデル開発。
* **[!DNL Experience Data Model (XDM)]:** 標準化と相互運用性は、Adobe Experience Platformの背後にある重要な概念です。 [エクスペリエンスデータモデル (XDM)](https://www.adobe.com/go/xdm-home-en)は、Adobeに基づいて推進され、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

>[!NOTE]
>
>一部 [!DNL Platform] でのサービス統合 [!DNL JupyterLab] は特定のカーネルに限定されています。 詳しくは「[カーネル](#kernels)」の節を参照してください。

## 主な機能と一般的な操作

の主な特長に関する情報 [!DNL JupyterLab] 共通の操作を実行する手順については、以下の節を参照してください。

* [JupyterLab へのアクセス](#access-jupyterlab)
* [JupyterLab インターフェイス](#jupyterlab-interface)
* [コードセル](#code-cells)
* [カーネル](#kernels)
* [カーネルセッション](#kernel-sessions)
* [ランチャー](#launcher)

### アクセス [!DNL JupyterLab] {#access-jupyterlab}

In [Adobe Experience Platform](https://platform.adobe.com)を選択します。 **[!UICONTROL ノートブック]** をクリックします。 しばらくの間 [!DNL JupyterLab] を完全に初期化します。

![](../images/jupyterlab/user-guide/access_jupyterlab.png)

### [!DNL JupyterLab] インターフェイス {#jupyterlab-interface}

この [!DNL JupyterLab] インターフェイスは、メニューバー、折りたたみ可能な左サイドバー、および「ドキュメント」タブと「アクティビティ」タブを含むメイン作業領域で構成されます。

**メニューバー**

インターフェイス上部のメニューバーには、で使用可能なアクションを表示するトップレベルのメニューがあります。 [!DNL JupyterLab] のキーボードショートカットを使用できます。

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

タブを選択して機能を表示するか、展開されたタブでを選択して左側のサイドバーを折りたたみます。以下に例を示します。

![](../images/jupyterlab/user-guide/left_sidebar_collapse.gif)

**メイン作業領域**

のメイン作業領域 [!DNL JupyterLab] を使用すると、ドキュメントやその他のアクティビティを、サイズ変更や分割が可能なタブのパネルに配置できます。 タブをタブパネルの中央にドラッグして、タブを移行します。タブをパネルの左、右、上または下にドラッグして、パネルを分割します。

![](../images/jupyterlab/user-guide/main_work_area.gif)

### での GPU とメモリサーバーの設定 [!DNL Python]/R

In [!DNL JupyterLab] 右上隅の歯車アイコンを選択して、 *Notebook サーバーの設定*. GPU をオンに切り替え、スライダーを使用して必要なメモリ量を割り当てることができます。 割り当てるメモリの量は、組織でプロビジョニングされているメモリの量によって異なります。 選択 **[!UICONTROL 設定を更新]** 保存します。

>[!NOTE]
>
>組織ごとに、ノートブック用にプロビジョニングされる GPU は 1 つだけです。 GPU が使用中の場合は、現在 GPU を予約しているユーザーが GPU を解放するのを待つ必要があります。 これは、GPU をログアウトするか、4 時間以上アイドル状態のままにすることで実行できます。

![](../images/jupyterlab/user-guide/notebook-gpu-config.png)

### 終了して再起動 [!DNL JupyterLab]

In [!DNL JupyterLab]を使用している場合は、セッションを終了して、それ以上のリソースが使用されないようにすることができます。 まず、 **電源アイコン** ![電源アイコン](../images/jupyterlab/user-guide/power_button.png)を選択し、「 **[!UICONTROL シャットダウン]** セッションを終了するように見えるポップオーバーから。 ノートブックセッションは、12 時間操作が行われなかった後、自動的に終了します。

再起動する [!DNL JupyterLab]を選択し、 **再起動アイコン** ![再起動アイコン](../images/jupyterlab/user-guide/restart_button.png) 電源アイコンのすぐ左にあるを選択し、 **[!UICONTROL 再起動]** 表示されるポップオーバーから。

![jupyterlab を終了](../images/jupyterlab/user-guide/shutdown-jupyterlab.gif)

### コードセル {#code-cells}

コードセルは、ノートブックの主なコンテンツです。これらには、ノートブックの関連カーネルの言語のソースコードと、コードセルを実行した結果の出力が含まれています。実行回数は、実行順序を表す各コードセルの右側に表示されます。

![](../images/jupyterlab/user-guide/code_cell.png)

一般的なセルのアクションを以下に示します。

* **セルの追加**：ノートブックメニューのプラス記号（**+**）をクリックして、空のセルを追加します。新しいセルは、現在操作中のセルの下に配置されます。特定のセルにフォーカスがない場合は、ノートブックの最後に配置されます。

* **セルの移動**：移動するセルの右側にカーソルを置き、セルをクリックして新しい位置にドラッグします。また、あるノートブックから別のノートブックにセルを移動すると、セルとその内容が複製されます。

* **セルの実行**：実行するセルのボディをクリックし、ノートブックメニューの&#x200B;**再生**&#x200B;アイコン（**▶**）をクリックします。カーネルが実行を処理する際には、セルの実行カウンターにアスタリスク（**\***）が表示され、完了時には整数に置き換えられます。

* **セルの削除**：削除するセルのボディをクリックし、**はさみ**&#x200B;アイコンをクリックします。

### カーネル {#kernels}

ノートのカーネルは、ノートのセルを処理するための言語固有のコンピューティングエンジンです。に加えて [!DNL Python], [!DNL JupyterLab] では、R、PySpark、 [!DNL Spark] (Scala). ノートブックドキュメントを開くと、関連するカーネルが起動します。ノートブックセルが実行されると、カーネルは計算をおこない、結果を生成し、CPU やメモリリソースを大量に消費する可能性があります。割り当てられたメモリは、カーネルがシャットダウンされるまで解放されないことに注意してください。

特定の機能は、以下の表で説明するように、特定のカーネルに限定されています。

| カーネル | ライブラリのインストールサポート | [!DNL Platform] 統合 |
| :----: | :--------------------------: | :-------------------- |
| **[!DNL Python]** | ○ | <ul><li>[!DNL Sensei ML Framework]</li><li>[!DNL Catalog Service]</li><li>[!DNL Query Service]</li></ul> |
| **R** | ○ | <ul><li>[!DNL Sensei ML Framework]</li><li>[!DNL Catalog Service]</li></ul> |
| **Scala** | × | <ul><li>[!DNL Sensei ML Framework]</li><li>[!DNL Catalog Service]</li></ul> |

### カーネルセッション {#kernel-sessions}

各アクティブなノートブックまたはアクティビティ [!DNL JupyterLab] はカーネルセッションを利用します。 すべてのアクティブなセッションは、左側のサイドバーから「**実行中の端末とカーネル**」タブを展開すると見つかります。ノートブックのカーネルのタイプと状態は、ノートブックのインターフェースの右上を見ることで識別できます。下の図では、ノートブックに関連するカーネルは **[!DNL Python]3** で、現在の状態は右側に灰色の円で表されています。中空の円はアイドルカーネルを意味し、実円はビジーカーネルを意味します。

![](../images/jupyterlab/user-guide/kernel_and_state_1.png)

カーネルがシャットダウンされたり、長時間使用されない場合は、 **カーネルなし** が実円と表示されます。カーネルの状態をクリックし、以下に示すように適切なカーネルタイプを選択して、カーネルをアクティブにします。

![](../images/jupyterlab/user-guide/switch_kernel.gif)

### ランチャー {#launcher}

[//]: # (Talk about the different Notebooks, introduce that certain starter notebooks are limited to particular kernels)

カスタマイズされた *Launcher* は、次のような、サポートされているカーネル用の便利なノートブックテンプレートを提供し、タスクを開始するのに役立ちます。

| テンプレート | 説明 |
| --- | --- |
| 空白 | 空のノートブックファイル。 |
| スターター | サンプルデータを使用したデータ調査を示す、事前入力済みのノートブック。 |
| 小売売上 | 次を含む事前入力済みのノートブック [小売販売レシピ](../pre-built-recipes/retail-sales.md) サンプルデータを使用します。 |
| レシピビルダー | でレシピを作成するためのノートブックテンプレート [!DNL JupyterLab]. レシピの作成プロセスを示し、説明するコードとコメントが事前に記入されています。詳細な順を追った説明については、『[ノートブックのレシピチュートリアル](https://docs.adobe.com/content/help/ja-JP/experience-platform/data-science-workspace/jupyterlab/create-a-recipe.html)』を参照してください。 |
| [!DNL Query Service] | の使用方法を示す、事前入力済みのノートブック [!DNL Query Service] 直接 [!DNL JupyterLab] 大規模なデータを分析するサンプルワークフローが用意されています。 |
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
        <th><strong>[!DNL Query Service]</strong></th>
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

## 次の手順

サポートされている各ノートブックの詳細と使用方法については、 [Jupyterlab ノートブックのデータアクセス](./access-notebook-data.md) 開発者ガイド。 このガイドでは、JupyterLab ノートブックを使用して、データの読み取り、書き込み、クエリを含む、データにアクセスする方法に焦点を当てます。 また、このデータアクセスガイドには、サポートされている各ノートブックで読み取れる最大データ量に関する情報も含まれています。

## サポートされるライブラリ {#supported-libraries}

Python、R、PySpark でサポートされているパッケージのリストについては、コピーして貼り付けます。 `!conda list` 新しいセルで、そのセルを実行します。 サポートされているパッケージのリストが、アルファベット順に表示されます。

![例](../images/jupyterlab/user-guide/libraries.PNG)

さらに、次の依存関係が使用されますが、一覧には表示されません。
* CUDA 11.2
* CUDN 8.1

