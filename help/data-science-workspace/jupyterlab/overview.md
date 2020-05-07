---
keywords: Experience Platform;JupyterLab;notebooks;Data Science Workspace;popular topics
solution: Experience Platform
title: JupterLabユーザガイド
topic: Overview
translation-type: tm+mt
source-git-commit: 606ae8784760e54a597b189958889199f85ebd0d
workflow-type: tm+mt
source-wordcount: '3356'
ht-degree: 6%

---


# JupterLabユーザガイド

JupyterLabは、 <a href="https://jupyter.org/" target="_blank">Project JupyterのWebベースのユーザーインターフェイスで</a> 、Adobe Experience Platformと緊密に統合されています。 これは、データ科学者がJupterのノート、コード、データを扱うための対話型開発環境を提供します。

このドキュメントでは、JupyterLabとその機能の概要と、一般的な操作の実行方法を説明します。

## エクスペリエンスプラットフォームのJupterLab

Experience PlatformのJupterLab統合には、アーキテクチャの変更、設計上の考慮事項、カスタマイズされたノートブック拡張機能、事前インストールされたライブラリ、アドビ主題のインターフェイスが含まれます。

次のリストは、JupterLab on Platformに固有の機能の一部を示しています。

| 機能 | 説明 |
| --- | --- |
| **カーネル** | カーネルは、ノートやJupterLabのフロントエンドで、異なるプログラミング言語でコードを実行したり内観したりする機能を提供します。 Experience Platformは、Python、R、PySpark、Sparkでの開発をサポートする追加のカーネルを提供します。 詳細は [カーネルの節を参照してください](#kernels) 。 |
| **データアクセス** | 読み取り/書き込み機能を完全にサポートし、既存のデータセットにJupterLab内から直接アクセス |
| **プラットフォームサービスの統合** | 組み込みの統合により、JupterLab内から他のプラットフォーム・サービスを直接利用できます。 サポートされる統合の完全なリストは、「他のプラットフォームサービスとの [統合](#service-integration)」の節に記載されています。 |
| **認証** | JupyterLabの組み込みのセキュリティモデルに加え <a href="https://jupyter-notebook.readthedocs.io/en/latest/security.html" target="_blank">て</a>、プラットフォームのサービス間通信を含む、アプリケーションとエクスペリエンスプラットフォーム間のすべての対話は暗号化され、 <a href="https://www.adobe.io/authentication/auth-methods.html" target="_blank">Adobe Identity Management System(IMS)を介して認証され</a>ます。 |
| **開発ライブラリ** | Experience Platformでは、JupyterLabはPython、R、PySpark用に事前にインストールされたライブラリを提供します。 サポートされているライブラリの完全なリストについては、 [付録](#supported-libraries) を参照してください。 |
| **ライブラリコントローラー** | プレインストールされたライブラリがニーズに合わない場合は、PythonとR用に追加のライブラリをインストールし、プラットフォームの整合性を維持し、データを安全に保つために、一時的に独立したコンテナに保存します。 詳細は [カーネルの節を参照してください](#kernels) 。 |

>[!NOTE] 追加のライブラリは、そのライブラリがインストールされたセッションでのみ使用できます。 新しいセッションを開始する際に必要な追加のライブラリを再インストールする必要があります。

## 他のプラットフォームサービスとの統合 {#service-integration}

標準化と相互運用性は、エクスペリエンスプラットフォームの背後にある重要な概念です。 組み込みIDEとしてJupyterLabをプラットフォームに統合すると、他のプラットフォームサービスとやり取りでき、プラットフォームを最大限に活用できます。 JupterLabでは、以下のプラットフォームサービスを利用できます。

* **カタログサービス：** 読み取り/書き込み機能を備えたデータセットへのアクセスと調査
* **クエリサービス：** SQLを使用してデータセットにアクセスし、データセットを調査します。大量のデータを処理する際に、データ・アクセスのオーバーヘッドが低くなります。
* **先生MLフレームワーク：** データのトレーニングとスコア機能を備えたモデル開発と、1回のクリックでレシピを作成。

>[!NOTE] JupyterLabでの一部のプラットフォームサービスの統合は、特定のカーネルに限定されています。 詳細は [カーネルの節を参照して](#kernels) ください。

## 主な機能と一般的な操作

JupyterLabの主な機能と共通の操作を実行する手順に関する情報は、以下の節で説明します。

* [JupyterLabにアクセス](#access-jupyterlab)
* [JupterLabインタフェース](#jupyterlab-interface)
* [コードセル](#code-cells)
* [カーネル](#kernels)
* [カーネルセッション](#kernel-sessions)
* [PySpark/Spark実行リソース](#pyspark-spark-execution-resource)
* [ランチャー](#launcher)

### JupyterLabにアクセス {#access-jupyterlab}

「 [Adobe Experience Platform](https://platform.adobe.com)」で、左のナビゲーション列から「 **ノートブック** 」を選択します。 JupyterLabが完全に初期化されるまで、しばらく待ちます。

![](../images/jupyterlab/user-guide/access_jupyterlab.png)

### JupterLabインタフェース {#jupyterlab-interface}

JupterLabインターフェイスは、メニューバー、折りたたみ可能な左側のサイドバー、およびドキュメントとアクティビティのタブを含むメイン作業領域で構成されます。

**メニューバー**

インターフェースの上部にあるメニューバーには、JupterLabで使用可能なアクションをキーボードショートカットで公開するトップレベルのメニューがあります。

* **ファイル：** ファイルとディレクトリに関連するアクション
* **編集：** ドキュメントおよびその他のアクティビティの編集に関する操作
* **表示:** JupterLabの外観を変更するアクション
* **実行：** ノートブックやコードコンソールなど、異なるアクティビティでコードを実行するためのアクション
* **カーネル：** カーネル管理のアクション
* **タブ：** オープンなドキュメントとアクティビティのリスト
* **設定：** 共通設定と詳細設定エディター
* **ヘルプ：** JupyterLabとカーネルのヘルプリンクのリスト

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

JupterLabの主な作業領域を使用すると、ドキュメントやその他のアクティビティを、サイズ変更や再分割が可能なタブのパネルに配置できます。 タブをタブパネルの中央にドラッグして、タブを移行します。 パネルを分割するには、タブをパネルの左、右、上または下にドラッグします。

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

ノート型カーネルは、ノート型セルを処理するための言語固有のコンピューティングエンジンです。 Pythonに加えて、JupyterLabはR、PySpark、Sparkで追加の言語サポートを提供します。 ノートブックドキュメントを開くと、関連付けられたカーネルが起動します。 ノート型セルを実行すると、カーネルは計算を行い、結果を生み出し、CPUやメモリのリソースを大量に消費する可能性がある。 割り当てられたメモリは、カーネルがシャットダウンするまで解放されないことに注意してください。

>[!IMPORTANT] Spark 2.3からSpark 2.4に更新されたJupterLab Launcher。 SparkとPySparkカーネルは、Spark 2.4ノートブックではサポートされなくなりました。

特定の機能は、以下の表に示すように特定のカーネルに限定されています。

| カーネル | ライブラリのインストールサポート | プラットフォームの統合 |
| :----: | :--------------------------: | :-------------------- |
| **Python** | ○ | <ul><li>先生MLフレームワーク</li><li>カタログサービス</li><li>クエリサービス</li></ul> |
| **R** | ○ | <ul><li>先生MLフレームワーク</li><li>カタログサービス</li></ul> |
| **PySpark — 非推奨** | × | <ul><li>先生MLフレームワーク</li><li>カタログサービス</li></ul> |
| **Spark — 非推奨** | × | <ul><li>先生MLフレームワーク</li><li>カタログサービス</li></ul> |
| **スカラ** | × | <ul><li>先生MLフレームワーク</li><li>カタログサービス</li></ul> |

### カーネルセッション {#kernel-sessions}

JupyterLabのアクティブなノートブックまたはアクティビティは、それぞれカーネル・セッションを利用します。 すべてのアクティブなセッションは、 **実行中の端末とカーネル** 」タブを左側のサイドバーから展開すると見つかります。 ノートブックのカーネルの種類と状態は、ノートブックインターフェイスの右上を見ることで識別できます。 下の図では、ノートブックに関連付けられたカーネルは **Python 3** で、現在の状態は右側に灰色の円で表されています。 中空の円は空回りのカーネルを意味し、実円はビジーカーネルを意味します。

![](../images/jupyterlab/user-guide/kernel_and_state_1.png)

カーネルがシャットダウンされたり、長時間非アクティブになったりした場合は、 **No Kernel!** に実線の円が表示されます。 カーネルの状態をクリックし、以下に示すように適切なカーネルタイプを選択して、カーネルをアクティブにします。

![](../images/jupyterlab/user-guide/switch_kernel.gif)

### PySpark/Spark実行リソース {#pyspark-spark-execution-resource}

>[!IMPORTANT]
>Spark 2.3からSpark 2.4へのトランジションでは、SparkとPySparkの両方のカーネルは廃止されています。
>
>新しいPySpark 3 (Spark 2.4)ノートブックはPython3 Kernelを使用します。 既存のノートブックの更新に関する詳細なチュートリアルについては、 [Pyspark 3 (Spark 2.3)をPySpark 3 (Spark 2.4)](../recipe-notebook-migration.md) に変換する際のガイドを参照してください。
>
>新しいSparkノートブックはScalaカーネルを利用する必要があります。 既存のノートブックの更新に関する詳しいチュートリアルについては、 [Spark 2.3をScala (Spark 2.4)に変換する際のガイドを参照してください](../recipe-notebook-migration.md) 。

PySparkとSparkカーネルは、PySparkまたはSparkノートブック内でSparkクラスタリソースを設定するために、configureコマンド(`%%configure`)を使用し、設定のリストを提供します。 これらの設定は、Sparkアプリケーションの初期化前に定義するのが理想的です。 Sparkアプリケーションがアクティブな間に設定を変更するには、次に示すように、コマンド(`%%configure -f`)の後にforceフラグを追加する必要があります。コマンドは、変更を適用するためにアプリケーションを再起動します。

>[!CAUTION]
>PySpark 3 (Spark 2.4)とScala (Spark 2.4)ノートブックでは、Sparkmagicはサポートされなくなりました。 `%%` 次の操作は、使用できなくなりました。
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

次の表に、設定可能なすべてのプロパティを示します。

| プロパティ | 説明 | タイプ |
| :------- | :---------- | :-----:|
| 親切な | セッションの種類（必須） | `session kind`_ |
| proxyUser | このセッションを実行する、偽装するユーザー（bobなど） | 文字列 |
| jars | Javaに配置するファイル `classpath` | パスのリスト |
| pyFiles | ファイルを `PYTHONPATH` | パスのリスト |
| ファイル | 実行者の作業ディレクトリに配置するファイル | パスのリスト |
| driverMemory | MBまたはGB単位のドライバのメモリ（例：1000M、2G） | 文字列 |
| driverCores | ドライバが使用するコア数（YARNモードのみ） | int |
| executorMemory | MBまたはGB単位のエグゼキューターのメモリ（例：1000M、2G） | 文字列 |
| executorCores | 実行者が使用するコア数 | int |
| numExecutors | エグゼクター数（YARNモードのみ） | int |
| アーカイブ | エクゼクターの作業ディレクトリで圧縮解除するアーカイブ（YARNモードのみ） | パスのリスト |
| キュー | 送信するYARNキュー（YARNモードのみ） | 文字列 |
| name | アプリケーションの名前 | 文字列 |
| conf | Spark設定プロパティ | key=valのマップ |

### ランチャー {#launcher}

[//]: # (Talk about the different Notebooks, introduce that certain starter notebooks are limited to particular kernels)

カスタマイズされた *ランチャーは* 、次のようなタスクのキックスタートに役立つ、サポート対象のカーネルの便利なノートブックテンプレートを提供します。

| テンプレート | 説明 |
| --- | --- |
| 空白 | 空のノートブックファイルです。 |
| スターター | サンプルデータを使用したデータ調査を示す、事前入力済みのノートブック。 |
| 小売売上高 | サンプルデータを使用した <a href="https://adobe.ly/2wOgO3L" target="_blank">小売販売レシピを含む事前入力済みのノートブック</a> 。 |
| レシピビルダ | JupterLabでレシピを作成するためのノートブックテンプレート。 レシピ作成プロセスの説明や説明を行うコードや注釈が事前に入力されています。 詳細なチュートリアルについては、 <a href="https://www.adobe.com/go/data-science-create-recipe-notebook-tutorial-en" target="_blank">『レシピのチュートリアル</a> 』を参照してください。 |
| クエリサービス | クエリサービスの使用をJupterLabに直接示す事前入力ノートブック。データを規模別に分析するサンプルワークフローが付属しています。 |
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

新しいランチャーを開くには *、*&#x200B;ファイル **/新しいランチャーをクリックします**。 または、左のサイドバーで **ファイルブラウザー** を展開し、プラス記号(**+**)をクリックします。

![](../images/jupyterlab/user-guide/new_launcher.gif)

## ノートブックを使用したプラットフォームデータへのアクセス

サポートされる各カーネルは組み込み機能を備えており、ノートブック内のデータセットからプラットフォームデータを読み取ることができます。 ただし、データのページ番号付けはPythonとRノートブックに限定されます。

### Python/Rでのデータセットの読み取り

PythonおよびRノートブックでは、データセットにアクセスする際にデータをページネーションできます。 ページネーションの有無に関係なくデータを読み取るサンプルコードを以下に示します。

[//]: # (In the following samples, the first step is currently required but once the SDK is complete, users are no longer required to explicitly define client_context)

#### ページ番号を付けずにPython/Rのデータセットから読み取る

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

#### Python/Rでのデータセットの読み取り（ページ番号付き）

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

### PySpark/Spark/Scalaのデータセットから読み込む

>[!IMPORTANT]
>Spark 2.3からSpark 2.4へのトランジションでは、SparkとPySparkの両方のカーネルは廃止されています。
>
>新しいPySpark 3 (Spark 2.4)ノートブックはPython3 Kernelを使用します。 既存のSpark 2.3コードを変換する場合は、 [Pyspark 3 (Spark 2.3)をPySpark 3 (Spark 2.4)に変換する際のガイドを参照してください](../recipe-notebook-migration.md) 。 新しいノートブックは、 [以下のPySpark 3 (Spark 2.4)](#pyspark2.4) の例に従う必要があります。
>
>新しいSparkノートブックはScalaカーネルを利用する必要があります。 既存のSpark 2.3コードを変換する場合は、 [Spark 2.3をScala(Spark 2.4)に変換する方法に関するガイドを参照してください](../recipe-notebook-migration.md) 。 新しいノートブックは、 [Scala (Spark 2.4)](#spark2.4) の例に従う必要があります。

アクティブなPySparkまたはSparkノートブックを開いた状態で、左のサイドバーから **Data Explorer** タブを展開し、重複が **Datasets** （データセット）をクリックして、使用可能なデータセットのリストを表示します。 アクセスするデータセットのリストを右クリックし、「Explore Data in Notebook ****」をクリックします。 次のコードセルが生成されます。

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

Spark 2.4の導入に伴い、 [`%dataset`](#magic) カスタムマジックが提供されます。

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
>Scalaでは、を使用して、内部から値 `sys.env()` を宣言して返すことができ `option`ます。

### PySpark 3 (Spark 2.4)ノートブックで%dataset magicを使用する {#magic}

Spark 2.4の導入に伴い、新しいPySpark 3 (Spark 2.4)ノートブック（Python 3カーネル）で使える `%dataset` カスタムマジックが提供されます。

**用途**

`%dataset {action} --datasetId {id} --dataFrame {df}`

**説明**

Pythonノートブック（Python 3カーネル）からデータセットを読み取ったり書き込んだりするための、カスタムData Science Workspaceマジックコマンド。

* **{action}**: データセットに対して実行するアクションのタイプ。 「読み取り」と「書き込み」の2つのアクションを使用できます。
* **—datasetId {id}**: 読み取りまたは書き込みを行うデータセットのIDを指定するために使用します。 これは必須の引数です。
* **—dataFrame {df}**: パンダのデータフレーム。 これは必須の引数です。
   * アクションが「read」の場合、{df}は、データセット読み取り操作の結果を利用できる変数です。
   * アクションが「書き込み」の場合、このデータフレーム{df}はデータセットに書き込まれます。
* **—mode（オプション）**: 使用できるパラメーターは、「batch」および「interactive」です。 デフォルトでは、モードは「interactive」に設定されています。 大量のデータを読み取る場合は、「バッチ」モードを使用することをお勧めします。

**例**

* **次の例を読みます**。 `%dataset read --datasetId 5e68141134492718af974841 --dataFrame pd0`
* **次に例を示します**。 `%dataset write --datasetId 5e68141134492718af974842 --dataFrame pd0`

### Pythonでのクエリサービスを使用したクエリデータ

JupyterLab on Platformを使用すると、PythonノートブックでSQLを使用して、 <a href="https://www.adobe.com/go/query-service-home-en" target="_blank">Adobe Experience Platformクエリサービスを通じてデータにアクセスできます</a>。 クエリサービスを通じてデータにアクセスすると、実行時間が長いため、大規模なデータセットを扱う場合に便利です。 クエリサービスを使用したデータのクエリの処理時間は10分に制限されています。

JupyterLabのクエリサービスを使用する前に、 <a href="https://www.adobe.com/go/query-service-sql-syntax-en" target="_blank">クエリサービスのSQL構文を十分に理解しておく必要があります</a>。

クエリサービスを使用してデータをクエリする場合は、ターゲットデータセットの名前を指定する必要があります。 必要なコードセルを生成するには、 **データエクスプローラーを使用して目的のデータセットを探します**。 データセットの一覧を右クリックし、「ノートブックの **クエリデータ** 」をクリックして、ノートブックに次の2つのコードセルを生成します。


JupyterLabでクエリサービスを利用するには、まず作業中のPythonノートブックとクエリサービスとの間の接続を作成する必要があります。 これは、最初に生成されたセルを実行することで達成できます。

```python
qs_connect()
```

2番目に生成されたセルでは、最初の行をSQLクエリの前に定義する必要があります。 デフォルトでは、生成されたセルに、クエリ結果をPandasデータフレームとして保存するオプションの変数(`df0`)が定義されます。 <br>この `-c QS_CONNECTION` 引数は必須で、クエリサービスに対してSQLクエリを実行するようカーネルに指示します。 その他の引数のリストについては、 [付録](#optional-sql-flags-for-query-service) を参照してください。

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

### Python/RでのExperienceEventデータのフィルタ

PythonまたはRノートブックのExperienceEventデータセットにアクセスしてフィルタリングするには、論理演算子を使用して特定の時間範囲を定義するフィルター規則と共に、データセット(`{DATASET_ID}`)のIDを指定する必要があります。 時間範囲を定義すると、指定したページ番号は無視され、データセット全体が考慮されます。

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

### PySpark/Spark内のExperienceEventデータのフィルタ

>[!IMPORTANT]
>Spark 2.3からSpark 2.4へのトランジションでは、SparkとPySparkの両方のカーネルは廃止されています。
>
>新しいPySpark 3 (Spark 2.4)ノートブックはPython3 Kernelを使用します。 既存のコードの変換の詳細については、 [Pyspark 3 (Spark 2.3)をPySpark 3 (Spark 2.4)に変換する際のガイドを参照してください](../recipe-notebook-migration.md) 。 新しいPySparkノートブックを作成する場合は、ExperienceEventデータをフィルタする [ためにPySpark 3 (spark 2.4)](#pyspark3-spark2.4) 例を使用します。
>
>新しいSparkノートブックはScalaカーネルを利用する必要があります。 既存のコードの変換の詳細については、 [Spark 2.3からScala (Spark 2.4)](../recipe-notebook-migration.md) への変換に関するガイドを参照してください。 新しいSparkノートブックを作成する場合は、 [Scala (spark 2.4)](#scala-spark) （ExperienceEventデータのフィルタリング例）を使用します。

PySparkまたはSparkノートブック内のExperienceEventデータセットにアクセスしてフィルタするには、データセットID(`{DATASET_ID}`)、組織のIMS ID、および特定の時間範囲を定義するフィルタルールを指定する必要があります。 フィルタ時間範囲は、関数を使用して定義します。関数パラメータ `spark.sql()`はSQLクエリ文字列です。

次のセルでは、2019年1月1日から2019年12月31日末の間にのみ存在するデータに対してExperienceEventデータセットをフィルタリングします。

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
>Scalaでは、を使用して、内部から値 `sys.env()` を宣言して返すことができ `option`ます。 これにより、変数が1回しか使用されないことがわかっている場合に、変数を定義する必要がなくなります。 次の例は、上記の例 `val userToken` から取り出し、インライン内で代替として宣言 `option` します。
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
| 2階の | 1.0.4 |
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
| Python | 3.6.7 |
| mkl-rt | 11.1 |

## クエリサービスのオプションのSQLフラグ {#optional-sql-flags-for-query-service}

次の表に、クエリ・サービスに使用できるオプションのSQLフラグを示します。

| **Flag** | **説明** |
| --- | --- |
| `-h`、`--help` | ヘルプメッセージを表示し、終了します。 |
| `-n`、`--notify` | クエリ結果を通知するオプションを切り替えます。 |
| `-a`、`--async` | このフラグを使用すると、クエリが非同期的に実行され、クエリの実行中にカーネルを解放できます。 変数にクエリ結果を割り当てる場合は、クエリが完了していないと未定義になる可能性があるので、注意が必要です。 |
| `-d`、`--display` | このフラグを使用すると、結果が表示されなくなります。 |

