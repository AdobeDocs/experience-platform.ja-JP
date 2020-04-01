---
keywords: Experience Platform;JupyterLab;notebooks;Data Science Workspace;popular topics
solution: Experience Platform
title: ノートブックを使用してデータを分析する
topic: Tutorial
translation-type: tm+mt
source-git-commit: 3dc16c835cb6ab0949fc0d4bf18349d415d680ed

---


# ノートブックを使用してデータを分析する

このチュートリアルでは、Data Science Workspaceに組み込まれたJupyterノートブックを使用して、データにアクセス、調査、視覚化する方法について説明します。 このチュートリアルの最後までに、データをよりよく理解するために、Jupyterのノートブックオファーの機能の一部を理解しておく必要があります。

次の概念が導入されました。

- **JupyterLab:** JupyterLabは [](https://blog.jupyter.org/jupyterlab-is-ready-for-users-5a6f039b8906) 、Project Jupyterの次世代Webベースのインターフェイスで、Adobe Experience Platformと緊密に統合されています。
- **バッチ：** データセットは、バッチで構成されます。 バッチとは、一定期間に収集され、1つの単位として一緒に処理される一連のデータです。 データセットにデータが追加されると、新しいバッチが作成されます。
- **Data Access SDK（非推奨）:** データアクセスSDKは廃止されました。 プラットフォームSDKガ [イドを使用し](../authoring/platform-sdk.md) てください。

## Data Science Workspaceでのノートブックの検索

この節では、以前に小売販売スキーマに取り込まれたデータについて説明します。

Data Science Workspaceを使用すると、JupyterLabプラットフォームを通じてJupyterノートブックを作成し、機械学習ワークフローを作成および編集できます。 JupyterLabは、Webブラウザを使用してノートブックドキュメントを編集できるサーバクライアントコラボレーションツールです。 これらのノートブックには、実行可能コードとリッチテキスト要素の両方を含めることができます。 ここでは、データの探索と分析を行うために、分析の説明と実行可能なPythonコードにMarkdownを使用します。

### ワークスペースの選択

JupyterLabを起動すると、Jupyter Notebooks用のWebベースのインタフェースが表示されます。 どの種類のノートブックを選ぶかによって、対応するカーネルが起動します。

使用する環境を比較する際には、各サービスの制限事項を考慮する必要があります。 たとえば、Pythonでpandasライブラリを使 [用している場合](https://pandas.pydata.org/) 、通常のユーザとしてRAMの上限は2 GBです。 パワーユーザーの場合でも、20 GBのRAMに制限されます。 より大きなオファーを扱う場合は、すべてのノートブックインスタンスで共有される1.5 TBの計算をSparkで使用すると効果的です。

デフォルトでは、TensorflowレシピはGPUクラスタで動作し、PythonはCPUクラスタ内で動作します。

### 新しいノートブックを作成する

Adobe Experience Platform UIで、上部メニューの「データサイエンス」タブをクリックし、Data Science Workspaceに移動します。 このページで、「JupyterLab」タブをクリックし、JupyterLabランチャーを開きます。 次のようなページが表示されます。

![](../images/jupyterlab/analyze-data/jupyterlab_launcher.png)

このチュートリアルでは、Jupyter NotebookのPython 3を使用して、データにアクセスして調査する方法を示します。 ランチャーページには、サンプルのノートブックが用意されています。 Python 3の小売販売レシピを使用します。

![](../images/jupyterlab/analyze-data/retail_sales.png)

小売販売のレシピは、同じ小売販売データセットを使用して、ジュピター・ノートブックでデータをどのように調査し、視覚化できるかを示す、独立した例です。 さらに、このノートブックは、トレーニングと検証に関してさらに深く掘り下げられています。 このノートブックの詳細については、このチュートリアルを参照して [ください](../walkthrough.md)。

### データへのアクセス

>[!NOTE] は非推 `data_access_sdk_python` 奨となり、お勧めしません。 コードを変換するには、「デ [ータアクセスSDKのプラットフォームSDKへの変換](../authoring/platform-sdk.md) 」チュートリアルを参照してください。 このチュートリアルでは、次の手順と同じ手順を引き続き使用します。

Adobe Experience Platformの内部データと外部データへのアクセスを順に繰り返します。 データセットやXDMスキーマなど `data_access_sdk_python` の内部データにアクセスするために、ライブラリを使用します。 外部データの場合は、Pandas Pythonライブラリを使用します。

#### 外部データ

小売売上ノートブックを開き、「データの読み込み」ヘッダーを探します。 次のPythonコードは、pandasのデータ構 `DataFrame` 造と [read_csv()関数を使用して](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html#pandas.read_csv) 、GithubでホストされるCSVをDataFrameに読み込みます。

![](../images/jupyterlab/analyze-data/read_csv.png)

PandasのDataFrameデータ構造は、2次元のラベル付きデータ構造です。 データの次元をすばやく確認するには、を使用しま `df.shape`す。 DataFrameの次元を表すタプルが返されます。

![](../images/jupyterlab/analyze-data/df_shape.png)

最後に、データがどのように見えるかを見てみましょう。 を使用して、DataFrame `df.head(n)` の最初の行を `n` 表示できます。

![](../images/jupyterlab/analyze-data/df_head.png)

#### エクスペリエンスプラットフォームデータ

次に、エクスペリエンスプラットフォームのデータへのアクセスについて説明します。

##### データセットID別

この節では、小売売上データセットを使用します。このデータセットは、小売売上サンプルノートブックで使用されているのと同じデータセットです。

「Jupyter Notebook」で、左側の「 **Data** 」タブからデータにアクセスできます。 タブをクリックすると、データセットのリストが表示されます。

![](../images/jupyterlab/analyze-data/dataset_tab.png)

これで、Datasetsディレクトリで、取り込まれたすべてのデータセットを確認できます。 ディレクトリにデータセットが大量に入力されている場合は、すべてのエントリの読み込みに1分かかる場合があります。

データセットは同じなので、前のセクションで外部データを使用した読み込みデータを置き換えます。 「データを読み込み」の下のコ **ードブロックを** 選択し、キーボ **ードのDキーを** 2回押します。 フォーカスがテキスト内ではなく、ブロック上にあることを確認します。 2回押す前に、 **Escキーを押し** てテキストフォーカスをエスケー **プすることができ** ます。

これで、データセットを右クリックし、ド `Retail-Training-<your-alias>` ロップダウンの「Explore Data in Notebook」オプションを選択できます。 実行可能なコードエントリがノートブックに表示されます。

>[!TIP] コードを変換するに [は、『プラットフォーム](../authoring/platform-sdk.md) SDK』ガイドを参照してください。

```PYTHON
from data_access_sdk_python.reader import DataSetReader
from datetime import date
reader = DataSetReader()
df = reader.load(data_set_id="xxxxxxxx", ims_org="xxxxxxxx@AdobeOrg")
df.head()
```

Python以外のカーネルで作業している場合は、このページを参照し [て](https://github.com/adobe/acp-data-services-dsw-reference/wiki/Accessing-Data-on-the-Platform) 、Adobe Experience Platformのデータにアクセスしてください。

実行可能なセルを選択し、ツールバーの再生ボタンを押すと、実行可能なコードが実行されます。 の出力は、デ `head()` ータセットのキーを列、データセットの最初のn行にしたテーブルです。 `head()` 出力する行数を指定する整数の引数を受け取ります。 デフォルトでは5です。

![](../images/jupyterlab/analyze-data/datasetreader_head.png)

カーネルを再起動し、全てのセルを再び実行した場合は、以前と同じ出力を得るべきです。

![](../images/jupyterlab/analyze-data/restart_kernel_run.png)


### データの調査

データにアクセスできるようになったので、統計と視覚化を使って、データ自体に焦点を当ててみましょう。 使用しているデータセットは、ある日に45の店舗に関するその他の情報を提供する小売データセットです。 特定のオブジェクトの特性の一 `date` 部を次 `store` に示します。
- `storeType`
- `weeklySales`
- `storeSize`
- `temperature`
- `regionalFuelPrice`
- `markDown`
- `cpi`
- `unemployment`
- `isHoliday`

#### 統計概要

Pythonのパンダライブラリを利用して、各属性のデータ型を取得できます。 次の呼び出しの出力では、各列のエントリ数とデータタイプに関する情報が得られます。

```PYTHON
df.info()
```

![](../images/jupyterlab/analyze-data/df_info.png)

この情報は、各列のデータタイプを知ることで、データの処理方法を知ることができるので役に立ちます。

次に、統計の概要を見てみましょう。 数値データ型のみが表示され、そ `date`の、 `storeType`および `isHoliday` は出力されません。

```PYTHON
df.describe()
```

![](../images/jupyterlab/analyze-data/df_describe.png)

これにより、各特性に対して6435個のインスタンスが存在することがわかります。 また、平均、標準偏差(std)、最小、最大、四分位数などの統計情報も提供されます。 これにより、データの偏差に関する情報が得られます。 次の節では、この情報と連携して機能するビジュアライゼーションを見て、データをよく理解していきます。

の最小値と最大値を見ると、デ `store`ータが表す一意のストアが45個あることがわかります。 店とは何かを区 `storeTypes` 別する店もある。 の配布は、次の方法で確 `storeTypes` 認できます。

![](../images/jupyterlab/analyze-data/df_groupby.png)

つまり、22店舗の数は `storeType` 17 `A`店舗、 `storeType` 6店舗 `B`の数は6店舗 `storeType``C`。

#### データの視覚化

データフレームの値がわかったので、パターンをより明確にし、より簡単に識別できるように、ビジュアライゼーションを使用してこれを補完したいと考えています。 グラフは、結果をグラフに表示する場合にも便利です。オーディエンス 視覚化に役立つPythonライブラリには、次のものがあります。
- [Matplotlib](https://matplotlib.org/)
- [パンダ](https://pandas.pydata.org/)
- [海辺](https://seaborn.pydata.org/)
- [gplot](https://ggplot2.tidyverse.org/)

この節では、各ライブラリを使用する利点を簡単に説明します。

[Matplotlibは](https://matplotlib.org/) 、最も古いPythonビジュアライゼーションパッケージです。 彼らの目標は、「簡単で難しいものを可能にする」ことだ。 これは、パッケージが非常に強力であることから、多くの人が複雑になりがちです。 相当な時間と労力を要さずに、合理的なグラフを得るのは必ずしも容易ではない。

[Pandasは主に](https://pandas.pydata.org/) 、統合インデックス付けを使用してデータを操作できるDataFrameオブジェクトに使用されます。 ただし、 pandasには、 matplotlibに基づくプロット機能も組み込まれています。

[seabornは](https://seaborn.pydata.org/) 、matplotlibの上に構築されたパッケージです。 主な目的は、デフォルトのグラフをより視覚的にわかりやすくし、複雑なグラフの作成を簡素化することです。

[ggplotは](https://ggplot2.tidyverse.org/) 、matplotlibの上に構築されたパッケージです。 ただし、主な違いは、ツールがR用のggplot2のポートであることです。seabornと同様、目標はmatplotlibを使って改善することです。 R向けのggplot2に詳しいユーザは、このライブラリを検討してください。


##### 一変グラフ

一変量グラフは、個々の変数のグラフです。 一般的な一変量グラフを使用して、ボックスとウィスカーのプロットをデータを視覚化します。

以前の小売データセットを使って、45店舗とその週間販売のそれぞれの箱ひげ図を作成できます。 プロットは、関数を使用して生成さ `seaborn.boxplot` れます。

![](../images/jupyterlab/analyze-data/box_whisker.png)

ボックスとウィスカープロットは、データの分布を示すのに使用されます。 プロットの外側の線は上下の四分位数を示し、ボックスは四分位数の範囲に及びます。 ボックス内の行は中央値を示します。 上四分位数または下四分位数の1.5倍以上のデータポイントは、円としてマークされます。 これらの点は外れ値と見なされます。

##### 多変量分析グラフ

多変量分析プロットは、変数間の相互作用を確認するために使用されます。 このビジュアライゼーションを使用すると、データ科学者は変数間に相関関係やパターンがあるかどうかを確認できます。 一般的な多変量分析グラフは、相関行列です。 相関行列を用いて、相関係数を用いて複数の変数間の依存関係を定量化する。

同じ小売データセットを使用して、相関行列を生成できます。

![](../images/jupyterlab/analyze-data/correlation_1.png)

中央から斜めの1が下にあることに注目してください。 これは、変数をそれ自体と比較する場合、完全に正の相関関係があることを示しています。 強い正の相関は1に近い大きさになり、弱い相関は0に近い大きさになります。 負の相関は、逆の傾向を示す負の係数で示されます。


## 次の手順

このチュートリアルでは、Data Science Workspaceで新しいJupyterノートブックを作成する方法と、Adobe Experience Platformから外部のデータにアクセスする方法について説明しました。 具体的には、次の手順を繰り返しました。
- 新しいジュピターノートブックを作成する
- データセットとスキーマ
- データセットの調査

次のセクションに進んで、レシピをパッケ [ージ化し](../models-recipes/package-source-files-recipe.md) 、Data Science Workspaceにインポートする準備が整いました。