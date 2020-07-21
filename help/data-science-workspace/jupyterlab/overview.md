---
keywords: Experience Platform;JupyterLab;notebooks;Data Science Workspace;popular topics
solution: Experience Platform
title: JupterLabユーザガイド
topic: Overview
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '3647'
ht-degree: 12%

---


# [!DNL JupyterLab] ユーザーガイド

[!DNL JupyterLab] は、 <a href="https://jupyter.org/" target="_blank">Project Jupyter向けのWebベースのユーザインターフェイスで</a> 、に緊密に統合されて [!DNL Adobe Experience Platform]います。 これは、データ科学者がJupterのノート、コード、データを扱うための対話型の開発環境を提供します。

このドキュメントでは、とその機能の概要 [!DNL JupyterLab] と、一般的なアクションを実行する手順を説明します。

## [!DNL JupyterLab] on [!DNL Experience Platform]

Experience PlatformのJupyterLab統合には、アーキテクチャの変更、設計上の考慮事項、カスタマイズされたノートブック拡張機能、事前にインストールされたライブラリ、Adobe主題のインターフェイスが含まれます。

次のリストは、Platformに関するJupterLabに固有の機能の一部を示しています。

| 機能 | 説明 |
| --- | --- |
| **カーネル** | カーネルは、ノートブックや他のフロントエンドで、異なるプログラミング言語でコードを実行したり内観したりする機能を提供します。 [!DNL JupyterLab] [!DNL Experience Platform] は、 [!DNL Python]、R、PySpark、およびでの開発をサポートする追加カーネルを提供 [!DNL Spark]します。 詳細は [カーネルの節を参照してください](#kernels) 。 |
| **データアクセス** | 読み取り/書き込み機能を完全にサポートし、既存のデータセット [!DNL JupyterLab] に内部から直接アクセスできます。 |
| **[!DNL Platform]サービス統合&#x200B;** | 組み込みの統合により、内部から他の [!DNL Platform] サービスを直接利用でき [!DNL JupyterLab]ます。 サポートされる統合の完全なリストは、「他のPlatformサービスとの [統合](#service-integration)」の節に記載されています。 |
| **認証** | JupyterLabの組み込みのセキュリティ・モデルに加え <a href="https://jupyter-notebook.readthedocs.io/en/latest/security.html" target="_blank">て</a>、Platform・サービス間の通信を含む、アプリケーションとExperience Platform間のすべてのやり取りは暗号化され、 <a href="https://www.adobe.io/authentication/auth-methods.html" target="_blank">[!DNL Adobe Identity Management System] (IMS)を介して認証され</a>ます。 |
| **開発ライブラリ** | では、 [!DNL Experience Platform]、R、PySpark用に事前にインストールされたライブラリを [!DNL JupyterLab][!DNL Python]提供します。 サポートされているライブラリの完全なリストについては、 [付録](#supported-libraries) を参照してください。 |
| **ライブラリコントローラー** | プレインストールされたライブラリがニーズに合わない場合は、PythonとR用に追加のライブラリをインストールし、個別のコンテナに一時的に保存して、データの整合性を維持し、データを安全に保つこ [!DNL Platform] とができます。 詳細は [カーネルの節を参照してください](#kernels) 。 |

>[!NOTE]
>
>追加のライブラリは、そのライブラリがインストールされたセッションでのみ使用できます。 新しいセッションを開始する際に必要な追加のライブラリを再インストールする必要があります。

## 他の [!DNL Platform] サービスとの統合 {#service-integration}

標準化と相互運用性は、重要な概念 [!DNL Experience Platform]です。 組み込みIDE [!DNL JupyterLab] としてのONの統合により、他の [!DNL Platform] サービスとのやり取りが可能になり、その潜在能力を最大限に活用 [!DNL Platform][!DNL Platform] できます。 では、次の [!DNL Platform] サービスを使用でき [!DNL JupyterLab]ます。

* **[!DNL Catalog Service]:**読み取り/書き込み機能を備えたデータセットへのアクセスと調査
* **[!DNL Query Service]:**SQLを使用してデータセットにアクセスし、データセットを調査します。大量のデータを処理する際に、データ・アクセスのオーバーヘッドが低くなります。
* **[!DNL Sensei ML Framework]:**データのトレーニングとスコア機能を備えたモデル開発と、1回のクリックでレシピを作成。
* **[!DNL Experience Data Model (XDM)]:**標準化と相互運用性は、Adobe Experience Platformの背後にある重要な概念です。[アドビが推進するExperience Data Model(XDM)](https://www.adobe.com/go/xdm-home-en)は、カスタマーエクスペリエンスデータを標準化し、カスタマーエクスペリエンス管理のスキーマを定義する取り組みです。

>[!NOTE]
>
>上の一部の [!DNL Platform] サービス統合 [!DNL JupyterLab] は、特定のカーネルに限定されます。 詳細は [カーネルの節を参照して](#kernels) ください。

## 主な機能と一般的な操作

の主な機能 [!DNL JupyterLab] と、共通の操作を実行する手順については、次の節で説明します。

* [JupyterLabにアクセス](#access-jupyterlab)
* [JupterLabインタフェース](#jupyterlab-interface)
* [コードセル](#code-cells)
* [カーネル](#kernels)
* [カーネルセッション](#kernel-sessions)
* [PySpark/Spark実行リソース](#execution-resource)
* [ランチャー](#launcher)

### Access [!DNL JupyterLab] {#access-jupyterlab}

「 [Adobe Experience Platform](https://platform.adobe.com)」で、左のナビゲーション列から「 **ノートブック** 」を選択します。 が完全に初期化されるま [!DNL JupyterLab] でしばらく待ちます。

![](../images/jupyterlab/user-guide/access_jupyterlab.png)

### [!DNL JupyterLab] インターフェイス {#jupyterlab-interface}

インター [!DNL JupyterLab] フェイスは、メニューバー、折りたたみ可能な左側のサイドバー、およびドキュメントとアクティビティのタブを含むメイン作業領域で構成されます。

**メニューバー**

インターフェイス上部のメニューバーには、で使用できるアクションをキーボードショートカットで表示するトップレベルのメニュー [!DNL JupyterLab] があります。

* **ファイル：** ファイルとディレクトリに関連するアクション
* **編集：** ドキュメントおよびその他のアクティビティの編集に関する操作
* **表示:** 外観を変更するアクション [!DNL JupyterLab]
* **実行：** ノートブックやコードコンソールなど、異なるアクティビティでコードを実行するためのアクション
* **カーネル：** カーネル管理のアクション
* **タブ：** オープンなドキュメントとアクティビティのリスト
* **設定：** 共通設定と詳細設定エディター
* **ヘルプ：** とカーネルのヘルプリンク [!DNL JupyterLab] のリスト

**左サイドバー**

左側のサイドバーには、次の機能にアクセスできるクリック可能なタブが含まれています。

* **ファイルブラウザー：** 保存されたノートブックドキュメントとディレクトリのリスト
* **データエクスプローラー：** データセットとスキーマの参照、アクセス、参照
* **カーネルと端末の実行：** アクティブなカーネルとターミナルセッションのリストと終了機能
* **コマンド：** 便利なコマンドのリスト
* **セルインスペクタ：** プレゼンテーション用にノートブックを設定する際に役立つツールやメタデータにアクセスできるセルエディター
* **タブ：** 開いたタブのリスト

タブをクリックしてその機能を表示するか、展開されたタブをクリックして左側のサイドバーを折りたたみます。次に例を示します。

![](../images/jupyterlab/user-guide/left_sidebar_collapse.gif)

**メイン作業領域**

のメイン作業領域 [!DNL JupyterLab] では、ドキュメントやその他のアクティビティを、サイズ変更や再分割が可能なタブのパネルに配置できます。 タブをタブパネルの中央にドラッグして、タブを移行します。 パネルを分割するには、タブをパネルの左、右、上または下にドラッグします。

![](../images/jupyterlab/user-guide/main_work_area.gif)

### コードセル {#code-cells}

コードセルは、ノートブックの主な内容です。 これらは、ノートブックの関連カーネルの言語のソースコードと、コードセルの実行結果としての出力を含む。 実行回数は、実行順序を表す各コードセルの右側に表示されます。

![](../images/jupyterlab/user-guide/code_cell.png)

一般的なセルの操作を次に示します。

* **セル追加:** 空のセルを追加するには、ノートブックメニューのプラス記号(**+**)をクリックします。 新しいセルは、現在操作中のセルの下に配置されます。特定のセルにフォーカスがない場合は、ノートブックの最後に配置されます。

* **セルの移動：** 移動するセルの右側にカーソルを置き、セルをクリックしてドラッグし、新しい位置に移動します。 また、1つのノートブックから別のノートブックにセルを移動すると、セルとその内容が複製されます。

* **セルの実行：** 実行するセルの本文をクリックし、ノートブックメニューから **再生** アイコン(****)をクリックします。 カーネルが実行を処理する際に、セルの実行カウンターにアスタリスク(**\***)が表示され、完了時に整数に置き換えられます。

* **セルの削除：** 削除するセルのボディをクリックし、 **はさみ** アイコンをクリックします。

### カーネル {#kernels}

ノート型カーネルは、ノート型セルを処理するための言語固有のコンピューティングエンジンです。 さらに、R、PySpark、 [!DNL Python]および [!DNL JupyterLab][!DNL Spark] (Scala)で追加の言語サポートを提供します。 ノートブックドキュメントを開くと、関連付けられたカーネルが起動します。 ノート型セルを実行すると、カーネルは計算を行い、結果を生み出し、CPUやメモリのリソースを大量に消費する可能性がある。 割り当てられたメモリは、カーネルがシャットダウンするまで解放されないことに注意してください。

特定の機能は、以下の表に示すように特定のカーネルに限定されています。

| カーネル | ライブラリのインストールサポート | [!DNL Platform] 統合 |
| :----: | :--------------------------: | :-------------------- |
| **[!DNL Python]** | ○ | <ul><li>[!DNL Sensei ML Framework]</li><li>[!DNL Catalog Service]</li><li>[!DNL Query Service]</li></ul> |
| **R** | ○ | <ul><li>[!DNL Sensei ML Framework]</li><li>[!DNL Catalog Service]</li></ul> |
| **スカラ** | × | <ul><li>[!DNL Sensei ML Framework]</li><li>[!DNL Catalog Service]</li></ul> |

### カーネルセッション {#kernel-sessions}

のアクティブなノートブックまたはアクティビティは、それぞれカーネルセッション [!DNL JupyterLab] を使用します。 すべてのアクティブなセッションは、 **実行中の端末とカーネル** 」タブを左側のサイドバーから展開すると見つかります。 ノートブックのカーネルの種類と状態は、ノートブックインターフェイスの右上を見ることで識別できます。 下の図では、ノートブックの関連カーネルは **[!DNL Python]3 **で、現在の状態は右側に灰色の円で表されています。 中空の円は空回りのカーネルを意味し、実円はビジーカーネルを意味します。

![](../images/jupyterlab/user-guide/kernel_and_state_1.png)

カーネルがシャットダウンされたり、長時間非アクティブになったりした場合は、 **No Kernel!** に実線の円が表示されます。 カーネルの状態をクリックし、以下に示すように適切なカーネルタイプを選択して、カーネルをアクティブにします。

![](../images/jupyterlab/user-guide/switch_kernel.gif)

### ランチャー {#launcher}

[//]: # (Talk about the different Notebooks, introduce that certain starter notebooks are limited to particular kernels)

カスタマイズされた *ランチャーは* 、次のようなタスクのキックスタートに役立つ、サポート対象のカーネルの便利なノートブックテンプレートを提供します。

| テンプレート | 説明 |
| --- | --- |
| 空白 | 空のノートブックファイルです。 |
| スターター | サンプルデータを使用したデータ調査を示す、事前入力済みのノートブック。 |
| 小売売上高 | サンプルデータを使用した <a href="https://adobe.ly/2wOgO3L" target="_blank">小売販売レシピを含む事前入力済みのノートブック</a> 。 |
| レシピビルダ | でレシピを作成するためのノートブックテンプレート [!DNL JupyterLab]。 レシピ作成プロセスの説明や説明を行うコードや注釈が事前に入力されています。 詳細なチュートリアルについては、 <a href="https://www.adobe.com/go/data-science-create-recipe-notebook-tutorial-en" target="_blank">『レシピのチュートリアル</a> 』を参照してください。 |
| [!DNL Query Service] | データをスケールで分析するサンプルワークフローを用意し、 [!DNL Query Service] 直接使用を示す事前入 [!DNL JupyterLab] 力ノートブック。 |
| XDMイベント | データ構造全体に共通の機能に重点を置いた、価値の高いエクスペリエンスイベントデータに関するデータ調査をデモする、事前入力済みのノートパソコン。 |
| XDMクエリ | エクスペリエンスイベントデータに関するサンプルのビジネスクエリを示す、事前入力されたノートブック。 |
| 集計 | 大量のデータを小さく管理しやすいチャンクに集計するためのサンプルワークフローを示す、事前入力済みのノートブック。 |
| クラスタリング | クラスタリングアルゴリズムを使用したエンドツーエンドの機械学習モデリングプロセスを示す、事前入力済みのノートパソコン。 |

一部のノートブックテンプレートは、特定のカーネルに限定されています。 各カーネルのテンプレートの可用性は、次の表にマッピングされます。

<table>
    <tr>
        <td></td>
        <th><strong>空白</strong></th>
        <th><strong>スターター</strong></th>
        <th><strong>小売売上高</strong></th>
        <th><strong>レシピビルダ</strong></th>
        <th><strong>[!DNLクエリサービス]</strong></th>
        <th><strong>XDMイベント</strong></th>
        <th><strong>XDMクエリ</strong></th>
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
        <th ><strong>スカラ</strong></th>
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

新しいランチャーを開くには *、*&#x200B;ファイル **/新しいランチャーをクリックします**。 または、左のサイドバーで **ファイルブラウザー** を展開し、プラス記号(**+**)をクリックします。

![](../images/jupyterlab/user-guide/new_launcher.gif)

### GPUと [!DNL Python]/Rでのメモリサーバーの設定

で、右上隅の歯車アイコンを [!DNL JupyterLab] 選択して、 *ノートブックサーバー設定を開きます*。 GPUのオン/オフを切り替え、スライダーを使用して必要なメモリ量を割り当てることができます。 割り当て可能なメモリの量は、組織でプロビジョニングされているメモリの量によって異なります。 「 **[!UICONTROL 設定を更新]** 」を選択して保存します。

>[!NOTE]
>1つの組織でノートブック用にプロビジョニングされるGPUは1つだけです。 GPUが使用中の場合は、現在GPUを予約しているユーザーがGPUを解放するのを待つ必要があります。 これは、GPUをログアウトするか、4時間以上アイドル状態のままにすることで実行できます。

![](../images/jupyterlab/user-guide/notebook-gpu-config.png)

## ノートブックを使用して [!DNL Platform] データにアクセスする

サポートされる各カーネルは組み込み機能を備えており、ノートブック内のデータセットから [!DNL Platform] データを読み取ることができます。 ただし、データのページ番号付けのサポートは、 [!DNL Python] およびRノートブックに限定されます。

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

### [!DNL Python]/R内のデータセットから読み取る

[!DNL Python] また、Rノートブックでは、データセットにアクセスする際にデータをページネーションできます。 ページネーションの有無に関係なくデータを読み取るサンプルコードを以下に示します。

[//]: # (In the following samples, the first step is currently required but once the SDK is complete, users are no longer required to explicitly define client_context)

#### ページ番号を付けずに、 [!DNL Python]/Rのデータセットから読み取る

次のコードを実行すると、データセット全体が読み取られます。 実行が成功した場合、データは変数が参照するPandasデータフレームとして保存され `df`ます。

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

* `{DATASET_ID}`: アクセスするデータセットの固有のID

#### ページネーションを使用した [!DNL Python]/R内のデータセットからの読み取り

次のコードを実行すると、指定したデータセットからデータが読み取られます。 ページネーションは、関数を使用してデータを制限し、関数を使用してデータをオフセットするこ `limit()` とで達成 `offset()` されます。 データを制限すると、読み取るデータポイントの最大数を指し、オフセットを設定すると、データを読み取る前にスキップするデータポイントの数を指します。 読み取り操作が正常に実行されると、データは、変数が参照するPandasデータフレームとして保存され `df`ます。

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

* `{DATASET_ID}`: アクセスするデータセットの固有のID

### PySpark/[!DNL Spark]/Scalaのデータセットから読み込む

アクティブなPySparkまたはScalaノートブックを開き、左のサイドバーから「 **Data Explorer** 」タブを展開し、重複が「 **Datasets** 」をクリックして、使用可能なデータセットのリストを表示します。 アクセスするデータセットのリストを右クリックし、「Explore Data in Notebook ****」をクリックします。 次のコードセルが生成されます。

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
>Scalaでは、を使用して、内部から値 `sys.env()` を宣言して返すことができ `option`ます。

### PySpark 3 ([!DNL Spark] 2.4)ノートブックで%dataset magicを使う {#magic}

2.4の導入に伴い、新しいPySpark 3 ( [!DNL Spark] 2.4)ノートブック( `%dataset`[!DNL Spark][!DNL Python] 3カーネル)で使用するカスタムマジックが提供されます。

**用途**

`%dataset {action} --datasetId {id} --dataFrame {df}`

**説明**

ノートブック( [!DNL Data Science Workspace][!DNL Python][!DNL Python] 3カーネル)からデータセットを読み取ったり書き込んだりするためのカスタムマジックコマンド。

* **{action}**: データセットに対して実行するアクションのタイプ。 「読み取り」と「書き込み」の2つのアクションを使用できます。
* **—datasetId {id}**: 読み取りまたは書き込みを行うデータセットのIDを指定するために使用します。 これは必須の引数です。
* **—dataFrame {df}**: パンダのデータフレーム。 これは必須の引数です。
   * アクションが「read」の場合、{df}は、データセット読み取り操作の結果を利用できる変数です。
   * アクションが「書き込み」の場合、このデータフレーム{df}はデータセットに書き込まれます。
* **—mode（オプション）**: 使用できるパラメーターは、「batch」および「interactive」です。 デフォルトでは、モードは「interactive」に設定されています。 大量のデータを読み取る場合は、「バッチ」モードを使用することをお勧めします。

**例**

* **次の例を読みます**。 `%dataset read --datasetId 5e68141134492718af974841 --dataFrame pd0`
* **次に例を示します**。 `%dataset write --datasetId 5e68141134492718af974842 --dataFrame pd0`

### で使用するクエリデ [!DNL Query Service] ータ [!DNL Python]

[!DNL JupyterLab] [] [!DNL Platform] は、ノートブックでSQLを使用して、 [!DNL Python] Adobe Experience Platformクエリサービスを通じてデータにアクセスでき <a href="https://www.adobe.com/go/query-service-home-en" target="_blank"></a>ます。 を通じてデータにアクセスする [!DNL Query Service] と、実行時間が長いため、大規模なデータセットを処理する場合に役立ちます。 を使用したデータのクエリには、処理時間制限 [!DNL Query Service] が10分あることをお勧めします。

に進む前 [!DNL Query Service] に、 [!DNL JupyterLab]SQL構文を理解しているこ <a href="https://www.adobe.com/go/query-service-sql-syntax-en" target="_blank">[!DNL Query Service] とを確認してください</a>。

を使用してデータをクエリする場合 [!DNL Query Service] は、ターゲットデータセットの名前を指定する必要があります。 必要なコードセルを生成するには、 **データエクスプローラーを使用して目的のデータセットを探します**。 データセットの一覧を右クリックし、「ノートブックの **クエリデータ** 」をクリックして、ノートブックに次の2つのコードセルを生成します。


を利用するに [!DNL Query Service] は、まず作業用 [!DNL JupyterLab]ノートブックとの接続を作成する必要があり [!DNL Python][!DNL Query Service]ます。 これは、最初に生成されたセルを実行することで達成できます。

```python
qs_connect()
```

2番目に生成されたセルでは、最初の行をSQLクエリの前に定義する必要があります。 デフォルトでは、生成されたセルに、クエリ結果をPandasデータフレームとして保存するオプションの変数(`df0`)が定義されます。 <br>この `-c QS_CONNECTION` 引数は必須で、SQLクエリを実行するようカーネルに指示 [!DNL Query Service]します。 その他の引数のリストについては、 [付録](#optional-sql-flags-for-query-service) を参照してください。

```python
%%read_sql df0 -c QS_CONNECTION
SELECT *
FROM name_of_the_dataset
LIMIT 10
/* Querying table "name_of_the_dataset" (datasetId: {DATASET_ID})*/
```

Python変数は、次の例のように、文字列形式の構文を使用し、変数を波括弧(`{}`)で囲むことで、SQLクエリ内で直接参照できます。

```python
table_name = 'name_of_the_dataset'
table_columns = ','.join(['col_1','col_2','col_3'])
```

```python
%%read_sql demo -c QS_CONNECTION
SELECT {table_columns}
FROM {table_name}
```

### / [!DNL Python]RでのExperienceEventデータのフィルタ

ま [!DNL Python] たはRノートブックのExperienceEventデータセットにアクセスしてフィルタリングするには、論理演算子を使用して、データセット(`{DATASET_ID}`)のIDと、特定の時間範囲を定義するフィルタールールを指定する必要があります。 時間範囲を定義すると、指定したページ番号は無視され、データセット全体が考慮されます。

フィルター演算子のリストを次に示します。

* `eq()`: 次と等しい
* `gt()`: より大きい
* `ge()`: 次よりも大きいか等しい
* `lt()`: より小さい
* `le()`: 次よりも小さいか等しい
* `And()`: 論理積演算子
* `Or()`: 論理和演算子

次のセルでは、2019年1月1日から2019年12月31日末の間にのみ存在するデータに対してExperienceEventデータセットをフィルタリングします。

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

### PySparkのExperienceEventデータをフィルタ/[!DNL Spark]

PySparkまたはScalaノートブック内のExperienceEventデータセットにアクセスしてフィルタするには、データセットID(`{DATASET_ID}`)、組織のIMS ID、および特定の時間範囲を定義するフィルタルールを指定する必要があります。 フィルタ時間範囲は、関数を使用して定義します。関数パラメータ `spark.sql()`はSQLクエリ文字列です。

次のセルでは、2019年1月1日から2019年12月31日末の間にのみ存在するデータに対してExperienceEventデータセットをフィルタリングします。

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
>
>Scalaでは、を使用して、内部から値 `sys.env()` を宣言して返すことができ `option`ます。 これにより、変数が1回しか使用されないことがわかっている場合に、変数を定義する必要がなくなります。 次の例は、上記の例 `val userToken` から取り出し、インライン内で代替として宣言 `option` します。
> 
```scala
> .option("user-token", sys.env("PYDASDK_IMS_USER_TOKEN"))
> ```

## サポートされるライブラリ {#supported-libraries}

### [!DNL Python] / R

| ライブラリ | バージョン |
| :------ | :------ |
| ノート | 6.0.0 |
| リクエスト | 2.22.0 |
| 巧妙に | 4.0.0 |
| 葉 | 0.10.0 |
| ipywidgets | 7.5.1 |
| ボケ | 1.3.1 |
| 幻 | 3.7.3 |
| 直並列の | 0.5.2 |
| jq | 1.6 |
| ケラス | 2.2.4 |
| nltk | 3.2.5 |
| パンダ | 0.22.0 |
| pandasql | 0.7.3 |
| 枕 | 6.0.0 |
| scikit-image | 0.15.0 |
| サイキット学習 | 0.21.3 |
| 汚い | 1.3.0 |
| 擦り傷 | 1.3.0 |
| 海辺 | 0.9.0 |
| statsmodels | 0.10.1 |
| 弾性の | 5.1.0.17 |
| ggplot | 0.11.5 |
| py-xgboost | 0.90 |
| opencv | 3.4.1 |
| 火花 | 2.4.3 |
| ピトー | 1.0.1 |
| wxpython | 4.0.6 |
| 色彩愛好家 | 0.3.0 |
| ジオパンダ | 0.5.1 |
| pysh | 2.1.0 |
| 格好良い | 1.6.4 |
| rpy2 | 2.9.4 |
| r-essentials | 3.6 |
| ルール | 1.6_3 |
| r-fpc | 2.2_3 |
| r-e1071 | 1.7_2 |
| r-gam | 1.16.1 |
| r-gbm | 2.1.5 |
| r-gthemes | 4.2.0 |
| r-ggvis | 0.4.4 |
| R字 | 1.2.4.1 |
| r-leaps | 3.0 |
| r操作 | 1.0.1 |
| r-rocr | 1.0_7 |
| r-rmysql | 0.10.17 |
| r-rodbc | 1.3_15 |
| r-rsqlite | 2.1.2 |
| r-rstan | 2.19.2 |
| r-sqldf | 0.4_11 |
| r生存 | 2.44_1.1 |
| 動物園 | 1.8_6 |
| r-stringdist | 0.9.5.2 |
| r-quadprog | 1.5_7 |
| r-rjson | 0.2.20 |
| r-forecast | 8.7 |
| r-rsolnp | 1.16 |
| r網状の | 1.12 |
| r-mlr | 2.14.0 |
| r-viridis | 0.5.1 |
| r-corplot | 0.84 |
| r-fnn | 1.1.3 |
| ルブリダート | 1.7.4 |
| ランダムフォレスト | 4.6_14 |
| r-tidyverse | 1.2.1 |
| rツリー | 1.0_39 |
| ピモンゴ | 3.8.0 |
| 矢 | 0.14.1 |
| boto3 | 1.9.199 |
| 10分の1 | 0.5.2 |
| 速い | 0.3.2 |
| ニシキイ | 0.5.4 |
| ipywebrtc | 0.5.0 |
| jupyter_client | 5.3.1 |
| wordcloud | 1.5.0 |
| graphiz | 2.40.1 |
| python-graphviz | 0.11.1 |
| ストレージ | 0.36.0 |
| [!DNL jupyterlab] | 1.0.4 |
| pandas_ml | 0.6.1 |
| tensorflow-gpu | 1.14.0 |
| nodejs | 12.3.0 |
| 偽物 | 3.0.5 |
| ipympl | 0.3.3 |
| fonts-anacond | 1.0 |
| psycopg2 | 2.8.3 |
| 鼻 | 1.3.7 |
| autovizwidget | 0.12.9 |
| アルタイル | 3.1.0 |
| vega_datasets | 0.7.0 |
| 製粉所 | 1.0.1 |
| sql_magic | 0.0.4 |
| iso3166 | 1.0 |
| nbimporter | 0.3.1 |

### PySpark

| ライブラリ | バージョン |
| :------ | :------ |
| リクエスト | 2.18.4 |
| 幻 | リリース 2.3.0 |
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
| azure-ストレージ-blob | 1.4.0 |
| [!DNL python] | 3.6.7 |
| mkl-rt | 11.1 |

## のオプションのSQLフラグ [!DNL Query Service] {#optional-sql-flags-for-query-service}

次の表に、使用できるオプションのSQLフラグを示し [!DNL Query Service]ます。

| **Flag** | **説明** |
| --- | --- |
| `-h`、`--help` | ヘルプメッセージを表示し、終了します。 |
| `-n`、`--notify` | クエリ結果を通知するオプションを切り替えます。 |
| `-a`、`--async` | このフラグを使用すると、クエリが非同期的に実行され、クエリの実行中にカーネルを解放できます。 変数にクエリ結果を割り当てる場合は、クエリが完了していないと未定義になる可能性があるので、注意が必要です。 |
| `-d`、`--display` | このフラグを使用すると、結果が表示されなくなります。 |

