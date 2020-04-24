---
keywords: Experience Platform;JupyterLab;notebooks;Data Science Workspace;popular topics
solution: Experience Platform
title: JupyterLabユーザガイド
topic: Overview
translation-type: tm+mt
source-git-commit: 19823c7cf0459e045366f0baae2bd8a98416154c

---


# JupyterLabユーザガイド

JupyterLabは、 <a href="https://jupyter.org/" target="_blank">Project JupyterのWebベースのユーザーインターフェイスで</a> 、Adobe Experience Platformに緊密に統合されています。 これは、データ科学者がJupyterのノート、コード、データを扱うための、対話型の開発環境を提供します。

このドキュメントでは、JupyterLabとその機能の概要と、一般的なアクションを実行する手順を説明します。

## エクスペリエンスプラットフォームのJupyterLab

Experience PlatformのJupyterLab統合には、アーキテクチャの変更、設計上の考慮事項、カスタマイズされたノートブック拡張機能、事前にインストールされたライブラリ、アドビが主題とするインターフェイスが含まれます。

次のリストでは、プラットフォーム上のJupyterLabに固有の機能の一部を説明します。

| 機能 | 説明 |
| --- | --- |
| **カーネル** | カーネルは、ノートブックや他のJupyterLabフロントエンドで、異なるプログラミング言語でコードを実行し、内観する機能を提供します。 Experience Platformは、Python、R、PySpark、Sparkでの開発をサポートする追加のカーネルを提供します。 詳細はカーネ [ルの節](#kernels) を参照してください。 |
| **データアクセス** | 読み取り/書き込み機能を完全にサポートし、既存のデータセットにJupyterLab内から直接アクセスする。 |
| **プラットフォームサービスの統合** | 組み込み統合により、JupyterLab内から他のプラットフォーム・サービスを直接利用できます。 サポートされる統合の完全なリストは、他のプラットフォームサービスとの統合の節 [に記載されています](#service-integration)。 |
| **認証** | JupyterLabの組み込みのセキュリテ <a href="https://jupyter-notebook.readthedocs.io/en/latest/security.html" target="_blank">ィモデルに加えて</a>、Platformのサービス間通信を含む、アプリケーションとExperience Platformの間のすべてのやり取りは、 <a href="https://www.adobe.io/authentication/auth-methods.html" target="_blank">Adobe Identity Management System(IMS)を通じて暗号化され、認証されます</a>。 |
| **開発ライブラリ** | Experience Platformでは、JupyterLabはPython、R、PySpark用に事前にインストールされたライブラリを提供します。 サポートされて [いるライブラリ](#supported-libraries) の完全なリストについては、付録を参照してください。 |
| **ライブラリコントローラ** | プリインストールされたライブラリがニーズに合わない場合は、PythonとR用に追加のライブラリをインストールし、プラットフォームの整合性を維持し、データを安全に保つために、一時的に分離されたコンテナに保存します。 詳細はカーネ [ルの節](#kernels) を参照してください。 |

>[!NOTE] 追加のライブラリは、インストールされたセッションでのみ使用できます。 新しいセッションを開始する際に必要な追加のライブラリを再インストールする必要があります。

## 他のプラットフォームサービスとの統合 {#service-integration}

標準化と相互運用性は、Experience Platformの背後にある重要な概念です。 組み込みIDEとしてJupyterLabをプラットフォームに統合することで、他のプラットフォームサービスとのやり取りが可能になり、プラットフォームを最大限に活用できます。 JupyterLabでは、以下のプラットフォームサービスを利用できます。

* **カタログサービス：** 読み取り/書き込み機能を備えたデータセットへのアクセスと調査
* **クエリサービス：** SQLを使用してデータセットにアクセスし、調査します。大量のデータを処理する際に、データ・アクセスのオーバーヘッドが低くなります。
* **先生MLフレームワーク：** データのトレーニングとスコア、および1回のクリックでレシピを作成できるモデル開発。

>[!NOTE] JupyterLabでの一部のプラットフォームサービス統合は、特定のカーネルに限定されています。 詳細はカーネルの節 [を参照](#kernels) 。

## 主な機能と一般的な操作

JupyterLabの主な機能と、共通の操作を実行する手順に関する情報を以下の節で説明します。

* [JupyterLabへのアクセス](#access-jupyterlab)
* [JupyterLabインタフェース](#jupyterlab-interface)
* [コードセル](#code-cells)
* [カーネル](#kernels)
* [カーネルセッション](#kernel-sessions)
* [PySpark/Spark実行リソース](#execution-resource)
* [ランチャー](#launcher)

### JupyterLabへのアクセス {#access-jupyterlab}

「 [Adobe Experience Platform](https://platform.adobe.com)」で、左のナビゲ **ーション列から「ノートブック** 」を選択します。 JupyterLabが完全に初期化されるまでしばらく待ちます。

![](../images/jupyterlab/user-guide/access_jupyterlab.png)

### JupyterLabインタフェース {#jupyterlab-interface}

JupyterLabインターフェイスは、メニューバー、折りたたみ可能な左サイドバー、およびドキュメントとアクティビティのタブを含むメイン作業領域で構成されます。

**メニューバー**

インタフェースの上部にあるメニューバーには、JupyterLabで使用可能なアクションをキーボードショートカットで公開するトップレベルのメニューがあります。

* **ファイル：** ファイルとディレクトリに関連するアクション
* **編集：** 編集に関するアクションおよびドキュメントのアクティビティ
* **表示:** JupyterLabの外観を変更するアクション
* **実行：** ノートブックやコードコンソールなど、異なるアクティビティでコードを実行するアクション
* **カーネル：** カーネル管理のアクション
* **タブ：** オープンリストとオープンドキュメントのアクティビティ
* **設定：** 共通設定と詳細設定エディター
* **ヘルプ：** JupyterLabとカーネルのヘルプリンクのリスト

**左サイドバー**

左側のサイドバーには、次の機能にアクセスできるクリック可能なタブが含まれています。

* **ファイルブラウザー：** 保存されたノートブックリストとドキュメントのディレクトリ
* **データエクスプローラー：** データセットとスキーマ
* **カーネルと端末の実行：** アクティブなリストと、終了する機能を持つターミナルセッションのセッション
* **コマンド：** 便利なコマンドのリスト
* **セルインスペクタ：** プレゼンテーション用にノートブックを設定する際に役立つツールやメタデータにアクセスできるセルエディター。
* **タブ：** 開いたタブのリスト

タブをクリックしてその機能を表示するか、展開されたタブをクリックして左側のサイドバーを折りたたみます。以下に例を示します。

![](../images/jupyterlab/user-guide/left_sidebar_collapse.gif)

**メイン作業領域**

JupyterLabのメイン作業領域では、ドキュメントや他のアクティビティを、サイズ変更や分割が可能なタブのパネルに配置できます。 タブをタブパネルの中央にドラッグして、タブを移行します。 タブをパネルの左、右、上または下にドラッグして、パネルを分割します。

![](../images/jupyterlab/user-guide/main_work_area.gif)

### コードセル {#code-cells}

コードセルは、ノートブックの主なコンテンツです。 これらは、ノートブックの関連するカーネルの言語のソースコードと、コードセルを実行した結果の出力を含む。 実行回数は、実行順序を表す各コードセルの右側に表示されます。

![](../images/jupyterlab/user-guide/code_cell.png)

一般的なセルのアクションを以下に示します。

* **追加セル：** [ノートブック]メニューのプラ&#x200B;**ス記号(**+)をクリックして、空のセルを追加します。 新しいセルは、現在操作中のセルの下に配置されます。特定のセルにフォーカスがない場合は、ノートブックの最後に配置されます。

* **セルの移動：** 移動するセルの右側にカーソルを置き、セルをクリックして新しい位置にドラッグします。 また、あるノートブックから別のノートブックにセルを移動すると、セルとその内容が複製されます。

* **セルの実行：** 実行するセルの本文をクリックし、[ノートブック]メニューの **** [再生&#x200B;**]アイコン(**)をクリックします。 カーネルが実行を処理する際には、セルの実行カウンターにアスタリスク(**\***)が表示され、完了時には整数に置き換えられます。

* **セルの削除：** 削除するセルのボディをクリックし、はさみアイコンをクリッ **クします** 。

### カーネル {#kernels}

ノート型カーネルは、ノート型セルを処理するための言語固有のコンピューティングエンジンです。 JupyterLabは、Pythonに加えて、R、PySpark、Sparkで追加の言語サポートを提供します。 ノートブック・ドキュメントを開くと、関連するカーネルが起動します。 ノートブックセルが実行されると、カーネルは計算を行い、結果を生成し、CPUやメモリリソースを大量に消費する可能性があります。 割り当てられたメモリは、カーネルがシャットダウンされるまで解放されないことに注意してください。

>[!IMPORTANT] JupyterLab LauncherがSpark 2.3からSpark 2.4に更新されました。SparkとPySparkのカーネルは、Spark 2.4ノートブックではサポートされなくなりました。

特定の機能は、以下の表で説明するように、特定のカーネルに限定されています。

| カーネル | ライブラリのインストールのサポート | プラットフォームの統合 |
| :----: | :--------------------------: | :-------------------- |
| **Python** | ○ | <ul><li>先生MLフレームワーク</li><li>カタログサービス</li><li>クエリサービス</li></ul> |
| **R** | ○ | <ul><li>先生MLフレームワーク</li><li>カタログサービス</li></ul> |
| **PySpark — 非推奨** | × | <ul><li>先生MLフレームワーク</li><li>カタログサービス</li></ul> |
| **Spark — 非推奨** | × | <ul><li>先生MLフレームワーク</li><li>カタログサービス</li></ul> |
| **スカラ** | × | <ul><li>先生MLフレームワーク</li><li>カタログサービス</li></ul> |

### カーネルセッション {#kernel-sessions}

JupyterLab上のアクティブなノートブックやアクティビティは、それぞれカーネルセッションを利用します。 すべてのアクティブなセッションは、左側のサイドバーから **Running terminals and kernels** （実行中の端末とカーネル）タブを展開すると見つかります。 ノートブックのカーネルのタイプと状態は、ノートブックのインターフェースの右上を見ることで識別できます。 下の図では、ノートブックに関連するカーネルは **Python 3** で、現在の状態は右側に灰色の円で表されています。 中空の円はアイドルカーネルを意味し、実円はビジーカーネルを意味します。

![](../images/jupyterlab/user-guide/kernel_and_state_1.png)

カーネルがシャットダウンされたり、長時間使用されない場合は、カーネル **がない！** 円がベタ塗りで表示されます。 カーネルの状態をクリックし、以下に示すように適切なカーネルタイプを選択して、カーネルをアクティブにします。

![](../images/jupyterlab/user-guide/switch_kernel.gif)

### PySpark/Spark実行リソース {#execution-resource}

>[!IMPORTANT]
>Spark 2.3からSpark 2.4へのトランジションでは、SparkとPySparkの両方のカーネルが廃止されました。
>
>新しいPySpark 3 (Spark 2.4)ノートブックはPython3カーネルを使っています。 既存のノートブックの更新に関する詳細なチュートリアルについては、 [](../recipe-notebook-migration.md) Pyspark 3(Spark 2.3)をPySpark 3(Spark 2.4)に変換する際のガイドを参照してください。
>
>新しいSparkノートブックはScalaカーネルを利用する必要があります。 既存のノートブックの更新に関する詳細なチュートリアルについては、 [Spark 2.3からScala(Spark 2.4)への変換に関するガイドを参照してください](../recipe-notebook-migration.md) 。

PySparkとSparkカーネルは、PySparkまたはSparkノートブック内にSparkクラスタリソースを設定するために、configureコマンド(`%%configure`)を使って設定リストを提供します。 これらの設定は、Sparkアプリケーションの初期化前に定義するのが理想的です。 Sparkアプリケーションがアクティブな状態で設定を変更するには、次に示すように、コマンド(`%%configure -f`)の後にforceフラグを追加する必要があります。このフラグは、変更を適用するためにアプリケーションを再起動します。

>[!CAUTION]
>PySpark 3 (Spark 2.4)とScala (Spark 2.4)ノートブックでは、Sparkmagicはサポートさ `%%` れなくなりました。 次の操作は使用できなくなりました。
* `%%help`
* `%%info`
* `%%cleanup`
* `%%delete`
* `%%configure`
* `%%local`

```python
%%configure -f 
{
    "numExecutors": 10,
    "executorMemory": "8G",
    "executorCores":4,
    "driverMemory":"2G",
    "driverCores":2,
    "conf": {
        "spark.cores.max": "40"
    }
}
```

次の表に、すべての設定可能なプロパティを示します。

| プロパティ | 説明 | タイプ |
| :------- | :---------- | :-----:|
| 親切な | セッションの種類（必須） | `session kind`_ |
| proxyUser | このセッションを実行するユーザー（bobなど） | 文字列 |
| jars | Javaに配置するファイル `classpath` | リスト |
| pyFiles | ファイルを `PYTHONPATH` | リスト |
| ファイル | 実行者の作業ディレクトリに配置するファイル | リスト |
| driverMemory | ドライバのメモリ（MBまたはGB）（例：1000M、2G） | 文字列 |
| driverCores | ドライバが使用するコアの数（YARNモードのみ） | int |
| executorMemory | MBまたはGB単位の実行者のメモリ（例：1000M、2G） | 文字列 |
| executorCores | 実行者が使用するコアの数 | int |
| numExecutors | 実行者数（YARNモードのみ） | int |
| アーカイブ | エクゼキューターの作業ディレクトリで圧縮解除するアーカイブ（HAIRNモードのみ） | リスト |
| キュー | 送信するYARNキュー（YARNモードのみ） | 文字列 |
| name | アプリの名前 | 文字列 |
| conf | Spark設定プロパティ | キーのマップ=val |

### ランチャー {#launcher}

[//]: # (Talk about the different Notebooks, introduce that certain starter notebooks are limited to particular kernels)

カスタマイズされた *Launcherは* 、次のような、サポート対象のカーネルの便利なノートブックテンプレートを提供し、タスクを開始するのに役立ちます。

| テンプレート | 説明 |
| --- | --- |
| 空白 | 空のノートブックファイル。 |
| スターター | サンプルデータを使用したデータ調査を示す、事前入力済みのノートブック。 |
| 小売売上 | サンプルデータを使用した小売販売のレシピ <a href="https://adobe.ly/2wOgO3L" target="_blank">を含む事前入力済みのノート</a> ブック。 |
| レシピビルダー | JupyterLabでレシピを作成するためのノートブックテンプレート。 レシピの作成プロセスを示し、説明するコードとコメントが事前に記入されています。 詳細なチュートリア <a href="https://www.adobe.com/go/data-science-create-recipe-notebook-tutorial-en" target="_blank">ルについては、『ノートブック</a> 』のレシピチュートリアルを参照してください。 |
| クエリサービス | クエリ・サービスの使用をJupterLabで直接示した事前入力済みのノートブック。データを規模別に分析するサンプル・ワークフローが用意されています。 |
| XDMイベント | データ構造全体で共通の機能に重点を置いた、後価値のエクスペリエンスイベントデータに関するデータ調査を示す、事前入力済みのノートブック。 |
| XDMクエリ | エクスペリエンスのイベントデータに関するサンプルのビジネスクエリを示す、事前入力済みのノートブック。 |
| 集計 | 大量のデータを小さく扱いやすい塊に集計するためのサンプルワークフローをデモする、事前入力済みのノートブック。 |
| クラスタリング | クラスタリングアルゴリズムを使用したエンドツーエンドの機械学習モデリングプロセスを示す、事前入力済みのノートブック。 |

一部のノートブックテンプレートは、特定のカーネルに限定されています。 各カーネルのテンプレートの可用性は、次の表にマッピングされます。

<table>
    <tr>
        <td></td>
        <th><strong>空白</strong></th>
        <th><strong>スターター</strong></th>
        <th><strong>小売売上</strong></th>
        <th><strong>レシピビルダー</strong></th>
        <th><strong>クエリサービス</strong></th>
        <th><strong>XDMイベント</strong></th>
        <th><strong>XDMクエリ</strong></th>
        <th><strong>集計</strong></th>
        <th><strong>クラスタリング</strong></th>
    </tr>
    <tr>
        <th><strong>Python</strong></th>
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
        <th  ><strong>PySpark 3（Spark 2.3 — 非推奨）</strong></th>
        <td >○</td>
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
        <th ><strong>Spark（Spark 2.3 — 非推奨）</strong></th>
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
      <tr>
        <th  ><strong>PySpark 3 (Spark 2.4)</strong></th>
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

新しいランチャーを開くに *は*、ファイル/ **新規ランチャーをクリックします**。 または、左のサイドバ **ーでファイル** ブラウザーを展開し、プラス記号(**+**)をクリックします。

![](../images/jupyterlab/user-guide/new_launcher.gif)

## ノートブックを使用したプラットフォームデータへのアクセス

サポートされる各カーネルは組み込み機能を提供し、ノートブック内のデータセットからプラットフォームデータを読み取ることができます。 ただし、ページ番号付けデータのサポートはPythonとRノートブックに限られています。

### Python/Rでのデータセットの読み取り

PythonおよびRノートブックでは、データセットにアクセスする際にデータをページネートできます。 ページネーションの有無に関わらずデータを読み取るサンプルコードを以下に示します。

[//]: # (In the following samples, the first step is currently required but once the SDK is complete, users are no longer required to explicitly define client_context)

#### Python/Rでのデータセットからの読み取り（ページ番号なし）

次のコードを実行すると、データセット全体が読み取られます。 実行が成功した場合、データは変数で参照されるPandasデータフレームとして保存されま `df`す。

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

* `{DATASET_ID}`:アクセスするデータセットの固有のID

#### Python/Rでのデータセットからの読み取り（ページ番号付き）

次のコードを実行すると、指定したデータセットからデータが読み取られます。 ページネーションは、関数を使用してデータを制限し、関数を使用してデータをオフセットす `limit()` ることで達 `offset()` 成されます。 データの制限とは、読み取るデータポイントの最大数を指し、オフセットとは、データの読み取り前にスキップするデータポイントの数を指します。 読み取り操作が正常に実行された場合、データは変数が参照するPandasデータフレームとして保存されま `df`す。

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

* `{DATASET_ID}`:アクセスするデータセットの固有のID

### PySpark/Spark/Scalaのデータセットから読み込む

>[!IMPORTANT]
>Spark 2.3からSpark 2.4へのトランジションでは、SparkとPySparkの両方のカーネルが廃止されました。
>
>新しいPySpark 3 (Spark 2.4)ノートブックはPython3カーネルを使っています。 既存のSpark 2.3コードを変換する場合は、 [Pyspark 3(Spark 2.3)をPySpark 3(Spark 2.4)に変換する方法のガイドを参照してください](../recipe-notebook-migration.md) 。 新しいノートブックは、 [以下のPySpark 3 (Spark 2.4)の例に従う必要があります](#pyspark2.4) 。
>
>新しいSparkノートブックはScalaカーネルを利用する必要があります。 既存のSpark 2.3コードを変換する場合は、 [Spark 2.3をScala(Spark 2.4)に変換する方法に関するガイドを参照してください](../recipe-notebook-migration.md) 。 新しいノートブックは、 [Scala (Spark 2.4)の例に従う必要があります](#spark2.4) 。

アクティブなPySparkまたはSparkノートブックが開いた状態で、左側のサイドバーから **Data Explorer** （データエクスプローラ）タブを展開し、重複が「 **Datasets** s（データセット）」をクリックして、使用可能なデータセットのリストを表示します。 アクセスするデータセットのリストを右クリックし、「Explore Data in **Notebook」をクリックします**。 次のコードセルが生成されます。

#### PySpark（Spark 2.3 — 非推奨）

```python
# PySpark 3 (Spark 2.3 - deprecated)

pd0 = spark.read.format("com.adobe.platform.dataset").\
    option('orgId', "YOUR_IMS_ORG_ID@AdobeOrg").\
    load("{DATASET_ID}")
pd0.describe()
pd0.show(10, False)
```

#### PySpark (Spark 2.4) {#pyspark2.4}

Spark 2.4の導入に伴い、カスタムマジッ [`%dataset`](#magic) クが提供されます。

```python
# PySpark 3 (Spark 2.4)

%dataset read --datasetId {DATASET_ID} --dataFrame pd0
pd0.describe()
pd0.show(10, False)
```

#### Spark（Spark 2.3 — 非推奨）

```scala
// Spark (Spark 2.3 - deprecated)

import com.adobe.platform.dataset.DataSetOptions
val dataFrame = spark.read.
    format("com.adobe.platform.dataset").
    option(DataSetOptions.orgId, "YOUR_IMS_ORG_ID@AdobeOrg").
    load("{DATASET_ID}")
dataFrame.printSchema()
dataFrame.show()
```

#### Scala(Spark 2.4) {#spark2.4}

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
>Scalaでは、を使用して、内から値 `sys.env()` を宣言し、返すことができま `option`す。

### PySpark 3 (Spark 2.4)ノートブックで%dataset magicを使用する {#magic}

Spark 2.4の導入に伴い、新しいPySpark 3 (Spark 2.4) `%dataset` ノートブック（Python 3カーネル）で使用するためのカスタムマジックが提供されます。

**用途**

`%dataset {action} --datasetId {id} --dataFrame {df}`

**説明**

Pythonノートブック（Python 3カーネル）からデータセットを読み書きするためのカスタムData Science Workspaceマジックコマンド。

* **{action}**:データセットに対して実行するアクションのタイプ。 「読み取り」と「書き込み」の2つのアクションを使用できます。
* **—datasetId {id}**:読み取りまたは書き込みを行うデータセットのIDを指定するために使用します。 これは必須の引数です。
* **—dataFrame {df}**:パンダのデータフレーム。 これは必須の引数です。
   * アクションが「read」の場合、{df}はデータセット読み取り操作の結果を利用できる変数です。
   * アクションが「書き込み」の場合、このデータフレーム{df}がデータセットに書き込まれます。
* **—mode（オプション）**:使用できるパラメーターは、「batch」および「interactive」です。 デフォルトでは、モードは「interactive」に設定されています。 大量のデータを読み取る場合は、「バッチ」モードを使用することをお勧めします。

**例**

* **例を読みます**。 `%dataset read --datasetId 5e68141134492718af974841 --dataFrame pd0`
* **例を記述**: `%dataset write --datasetId 5e68141134492718af974842 --dataFrame pd0`

### Pythonでクエリサービスを使用するクエリデータ

JupyterLab on Platformを使用すると、PythonノートブックでSQLを使用して、 <a href="https://www.adobe.com/go/query-service-home-en" target="_blank">Adobe Experience Platformクエリサービスを通じてデータにアクセスできます</a>。 クエリサービスを通じてデータにアクセスすると、実行時間が長いため、大量のデータセットを扱う場合に役立ちます。 クエリサービスを使用したデータのクエリには、10分の処理時間制限があることをお勧めします。

JupyterLabでクエリサービスを使用する前に、 <a href="https://www.adobe.com/go/query-service-sql-syntax-en" target="_blank">クエリサービスSQL構文を理解しておく必要があります</a>。

クエリサービスを使用してデータを照会する場合は、データセットの名前を指定するターゲットが必要です。 必要なコードセルを生成するには、データエクスプローラーを使用して目的のデータセットを見つ **けま**&#x200B;す。 データセットの一覧を右クリックし、「 **Notebook** のクエリデータ」をクリックして、ノートブックに次の2つのコードセルを生成します。


JupyterLabのクエリサービスを利用するには、まず作業中のPythonノートブックとクエリサービスの間の接続を作成する必要があります。 これは、最初に生成されたセルを実行することで達成できます。

```python
qs_connect()
```

2番目に生成されたセルでは、最初の行をSQLクエリの前に定義する必要があります。 デフォルトでは、生成されたセルはオプションの変数(`df0`)を定義し、クエリ結果をPandasデータフレームとして保存します。 <br>この引 `-c QS_CONNECTION` 数は必須で、カーネルに対してSQLクエリを実行するように指示します。 その他の引数の [リストは](#optional-sql-flags-for-query-service) 、付録を参照してください。

```python
%%read_sql df0 -c QS_CONNECTION
SELECT *
FROM name_of_the_dataset
LIMIT 10
/* Querying table "name_of_the_dataset" (datasetId: {DATASET_ID})*/
```

Python変数は、次の例に示すように、文字列形式の構文を使用し、中括弧(`{}`)で囲むことで、SQLクエリ内で直接参照できます。

```python
table_name = 'name_of_the_dataset'
table_columns = ','.join(['col_1','col_2','col_3'])
```

```python
%%read_sql demo -c QS_CONNECTION
SELECT {table_columns}
FROM {table_name}
```

### Python/RでのExperienceEventデータのフィルタ

PythonまたはRノートブックのExperienceEventデータセットにアクセスしてフィルタリングするには、データセットのID(`{DATASET_ID}`)と、論理演算子を使用して特定の時間範囲を定義するフィルタールールを指定する必要があります。 時間範囲を定義すると、指定したページ番号は無視され、データセット全体が考慮されます。

フィルタリングリストを以下に示します。

* `eq()`: 次と等しい
* `gt()`: より大きい
* `ge()`: 次よりも大きいか等しい
* `lt()`: より小さい
* `le()`: 次よりも小さいか等しい
* `And()`:論理積演算子
* `Or()`:論理和演算子

次のセルでは、ExperienceEventデータセットを、2019年1月1日から2019年12月31日の終わりまでの間にのみ存在するデータにフィルターします。

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

### PySpark/SparkのExperienceEventデータのフィルタ

>[!IMPORTANT]
>Spark 2.3からSpark 2.4へのトランジションでは、SparkとPySparkの両方のカーネルが廃止されました。
>
>新しいPySpark 3 (Spark 2.4)ノートブックはPython3カーネルを使っています。 既存のコードの変換について詳しくは、 [Pyspark 3(Spark 2.3)をPySpark 3(Spark 2.4)に変換する際のガイドを参照してください](../recipe-notebook-migration.md) 。 新しいPySparkノートブックを作成する場合は、ExperienceEventデータのフィルタリングに [PySpark 3 (spark 2.4)](#pyspark3-spark2.4) の例を使用します。
>
>新しいSparkノートブックはScalaカーネルを利用する必要があります。 既存のコードの変換について詳しくは、 [Spark 2.3からScala(Spark 2.4)への変換に関するガイドを参照してください](../recipe-notebook-migration.md) 。 新しいSparkノートブックを作成する場合は、Scala( [spark 2.4)の例を使用してExperienceEventデータをフィルタリングします](#scala-spark) 。

PySparkまたはSparkノートブック内のExperienceEventデータセットにアクセスしてフィルタするには、データセットID(`{DATASET_ID}`)、組織のIMS ID、および特定の時間範囲を定義するフィルタルールを指定する必要があります。 Filteringの時間範囲は、関数を使用して定義します。関 `spark.sql()`数パラメータはSQLクエリ文字列です。

次のセルでは、ExperienceEventデータセットを、2019年1月1日から2019年12月31日の終わりまでの間にのみ存在するデータにフィルターします。

#### PySpark 3（Spark 2.3 — 非推奨）

```python
# PySpark 3 (Spark 2.3 - deprecated)

pd = spark.read.format("com.adobe.platform.dataset").\
    option("orgId", "YOUR_IMS_ORG_ID@AdobeOrg").\
    load("{DATASET_ID}")

pd.createOrReplaceTempView("event")
timepd = spark.sql("""
    SELECT *
    FROM event
    WHERE timestamp > CAST('2019-01-01 00:00:00.0' AS TIMESTAMP)
    AND timestamp < CAST('2019-12-31 23:59:59.9' AS TIMESTAMP)
""")
```

#### PySpark 3 (Spark 2.4) {#pyspark3-spark2.4}

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

#### Spark（Spark 2.3 — 非推奨）

```scala
// Spark (Spark 2.3 - deprecated)

import com.adobe.platform.dataset.DataSetOptions
val dataFrame = spark.read.
    format("com.adobe.platform.dataset").
    option(DataSetOptions.orgId, "YOUR_IMS_ORG_ID@AdobeOrg").
    load("{DATASET_ID}")

dataFrame.createOrReplaceTempView("event")
val timedf = spark.sql("""
    SELECT * 
    FROM event 
    WHERE timestamp > CAST('2019-01-01 00:00:00.0' AS TIMESTAMP)
    AND timestamp < CAST('2019-12-31 23:59:59.9' AS TIMESTAMP)
""")
```

#### Scala(Spark 2.4) {#scala-spark}

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
>Scalaでは、を使用して、内から値 `sys.env()` を宣言し、返すことができま `option`す。 これにより、変数が1回しか使用されないことがわかっている場合に、変数を定義する必要がなくなります。 次の例では、上記の例 `val userToken` を使用し、それを別の例としてインラインで宣言 `option` しています。
> 
```scala
> .option("user-token", sys.env("PYDASDK_IMS_USER_TOKEN"))
> ```

## サポートされるライブラリ {#supported-libraries}

### Python / R

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
| カラス | 2.2.4 |
| nltk | 3.2.5 |
| パンダ | 0.22.0 |
| pandasql | 0.7.3 |
| 枕 | 6.0.0 |
| サイキット画像 | 0.15.0 |
| 科学を学ぶ | 0.21.3 |
| スシピ | 1.3.0 |
| 擦り傷 | 1.3.0 |
| 海辺 | 0.9.0 |
| statsmodels | 0.10.1 |
| 弾性 | 5.1.0.17 |
| gplot | 0.11.5 |
| py-xgboost | 0.90 |
| opencv | 3.4.1 |
| 火花 | 2.4.3 |
| パイトーチ | 1.0.1 |
| wxpython | 4.0.6 |
| 色彩愛好家 | 0.3.0 |
| ジオパンダ | 0.5.1 |
| パイシュ | 2.1.0 |
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
| R字グラフ | 1.2.4.1 |
| R字飛び | 3.0 |
| 再操作 | 1.0.1 |
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
| r予測 | 8.7 |
| r-rsolnp | 1.16 |
| 網状の | 1.12 |
| r-mlr | 2.14.0 |
| リビジス | 0.5.1 |
| r-corplot | 0.84 |
| r-fnn | 1.1.3 |
| ルブリダート | 1.7.4 |
| ランダムフォレスト | 4.6_14 |
| 反逆の | 1.2.1 |
| rツリー | 1.0_39 |
| ピモンゴ | 3.8.0 |
| 矢 | 0.14.1 |
| boto3 | 1.9.199 |
| 100万 | 0.5.2 |
| 速い | 0.3.2 |
| ひどい | 0.5.4 |
| ipywebrtc | 0.5.0 |
| jupyter_client | 5.3.1 |
| WordCloud | 1.5.0 |
| グラフ | 2.40.1 |
| python-graphviz | 0.11.1 |
| アズーストレージ | 0.36.0 |
| 飛び跳ねる | 1.0.4 |
| pandas_ml | 0.6.1 |
| tensorflow-gpu | 1.14.0 |
| nodejs | 12.3.0 |
| 偽物 | 3.0.5 |
| ipympl | 0.3.3 |
| fonts-anacond | 1.0 |
| psycopg2 | 2.8.3 |
| 鼻 | 1.3.7 |
| autovizwidget | 0.12.9 |
| アルタイア | 3.1.0 |
| vega_datasets | 0.7.0 |
| 製粉工場 | 1.0.1 |
| sql_magic | 0.0.4 |
| iso3166 | 1.0 |
| nbimporter | 0.3.1 |

### PySpark

| ライブラリ | バージョン |
| :------ | :------ |
| リクエスト | 2.18.4 |
| 幻 | リリース 2.3.0 |
| カラス | 2.0.6 |
| nltk | 3.2.4 |
| パンダ | 0.20.1 |
| pandasql | 0.7.3 |
| 枕 | 5.3.0 |
| サイキット画像 | 0.13.0 |
| 科学を学ぶ | 0.19.0 |
| スシピ | 0.19.1 |
| 擦り傷 | 1.3.3 |
| statsmodels | 0.8.0 |
| 弾性 | 4.0.30.44 |
| py-xgboost | 0.60 |
| opencv | 3.1.0 |
| 矢 | 0.8.0 |
| boto3 | 1.5.18 |
| azure-ストレージ-blob | 1.4.0 |
| Python | 3.6.7 |
| mkl-rt | 11.1 |

## オプションのクエリサービスのSQLフラグ {#optional-sql-flags-for-query-service}

次の表に、クエリ・サービスに使用できるオプションのSQLフラグの概要を示します。

| **Flag** | **説明** |
| --- | --- |
| `-h`、`--help` | ヘルプメッセージを表示し、終了します。 |
| `-n`、`--notify` | 通知のオプションを切り替えてクエリ結果を通知します。 |
| `-a`、`--async` | このフラグを使用すると、クエリが非同期的に実行され、カーネルの実行中にクエリを解放できます。 変数にクエリ結果を割り当てる場合は、クエリが完了しない場合は定義されない可能性があるので、注意が必要です。 |
| `-d`、`--display` | このフラグを使用すると、結果が表示されなくなります。 |

