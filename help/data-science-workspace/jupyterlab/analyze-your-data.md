---
keywords: Experience Platform;JupyterLab;notebooks;Data Science Workspace;popular topics
solution: Experience Platform
title: ノートブックを使用してデータを分析する
topic: Tutorial
translation-type: tm+mt
source-git-commit: 606ae8784760e54a597b189958889199f85ebd0d
workflow-type: tm+mt
source-wordcount: '1746'
ht-degree: 0%

---


# ノートブックを使用してデータを分析する

このチュートリアルでは、Data Science Workspaceに組み込まれたJupyterノートブックを使用して、データにアクセス、調査、視覚化する方法に焦点を当てます。 このチュートリアルを終えるまでに、データをよりよく理解するために、Jupyterのノートブックオファーの機能の一部を理解しておく必要があります。

次の概念について説明します。

- **JupterLab:** [JupyterLab](https://blog.jupyter.org/jupyterlab-is-ready-for-users-5a6f039b8906) は、Project Jupyterの次世代Webベースのインターフェイスで、Adobe Experience Platformと緊密に統合されています。
- **バッチ：** データセットはバッチで構成されます。 バッチとは、ある期間に収集され、1つの単位として一緒に処理される一連のデータです。 データセットにデータが追加されると、新しいバッチが作成されます。
- **Data Access SDK（非推奨）:** データアクセスSDKは廃止されました。 『 [プラットフォームSDK](../authoring/platform-sdk.md) 』ガイドを使用してください。

## Data Science Workspaceのノートブックの参照

この節では、以前小売販売スキーマに取り込まれたデータについて説明します。

Data Science Workspaceを使用すると、JupterLabプラットフォームを通じてJupyter Notebooksを作成し、機械学習ワークフローを作成および編集できます。 JupterLabは、Webブラウザを使用してノートブックドキュメントを編集できるサーバクライアントコラボレーションツールです。 これらのノートブックには、実行可能コードとリッチテキストの両方の要素を含めることができます。 当社の目的では、分析の説明と実行可能なPythonコードにMarkdownを使用して、データの探索と分析を行います。

### ワークスペースの選択

JupyterLabを起動すると、Jupyter Notebooks用のWebベースのインターフェースが提供されます。 どの種類のノートブックを選ぶかによって、対応するカーネルが起動します。

どの環境を使用するかを比較する際には、各サービスの制限事項を考慮する必要があります。 たとえば、Pythonで [pandas](https://pandas.pydata.org/) libraryを使用している場合、通常のユーザーとしてRAMの制限は2 GBです。 パワーユーザーの場合でも、20 GBのRAMに制限されます。 より大きな計算を扱う場合は、すべてのノートブックインスタンスで共有されるオファー1.5 TBのSparkを使用すると効果的です。

デフォルトでは、TensorflowレシピはGPUクラスタで動作し、PythonはCPUクラスタ内で動作します。

### 新しいノートブックを作成する

Adobe Experience Platform UIで、上部のメニューの「Data Science」タブをクリックし、Data Science Workspaceに移動します。 このページで、[ JupyterLab ]タブをクリックし、 JupyterLabランチャーを開きます。 次のようなページが表示されます。

![](../images/jupyterlab/analyze-data/jupyterlab_launcher.png)

このチュートリアルでは、Jupyter NotebookのPython 3を使用して、データにアクセスし、データを調べる方法を示します。 ランチャーページには、サンプルノートブックが用意されています。 Python 3向けの小売売上レシピを使用します。

![](../images/jupyterlab/analyze-data/retail_sales.png)

小売売上のレシピは、同じ小売売上データセットを使用して、ジュピター・ノートブックでデータをどのように検証、視覚化できるかを示す、独立した例です。 さらに、トレーニングと検証に関してさらに深く掘り下げています。 このノートブックの詳細については、この [チュートリアルを参照してください](../walkthrough.md)。

### データへのアクセス

>[!NOTE] は非推奨 `data_access_sdk_python` で、推奨されなくなりました。 コードを変換するには、「 [データアクセスSDKからプラットフォームSDKへの変換](../authoring/platform-sdk.md) 」チュートリアルを参照してください。 このチュートリアルでは、次と同じ手順を引き続き使用します。

Adobe Experience Platformの内部データと外部データへのアクセスを調べます。 このライブラリを使用して、データセットやXDMスキーマなどの内部データにアクセスします。 `data_access_sdk_python` 外部データの場合は、Pandas Pythonライブラリを使用します。

#### 外部データ {#external-data}

小売売上ノートブックを開き、「Load Data」ヘッダーを探します。 以下のPythonコードは、GithubでホストされるCSVをDataFrameに読み込むために、pandasの `DataFrame` データ構造と [read_csv()](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html#pandas.read_csv) 関数を使用しています。

![](../images/jupyterlab/analyze-data/read_csv.png)

PandasのDataFrameデータ構造は、2次元のラベル付きデータ構造です。 データの次元をすばやく見るために、を使用でき `df.shape`ます。 次に、DataFrameの次元を表すタプルを返します。

![](../images/jupyterlab/analyze-data/df_shape.png)

最後に、データがどのように表示されるかを試すことができます。 を使用して、DataFrame `df.head(n)` の最初の `n` 行を表示できます。

![](../images/jupyterlab/analyze-data/df_head.png)

#### エクスペリエンスプラットフォームデータ

次に、エクスペリエンスプラットフォームのデータへのアクセスについて説明します。

#### データセットID別

この節では、小売売上データセットを使用します。これは、小売売上サンプルノートブックで使用されているものと同じデータセットです。

「Jupyter Notebook」で、左側の「 **Data** 」タブからデータにアクセスできます。 タブをクリックすると、データセットのリストを表示できます。

![](../images/jupyterlab/analyze-data/dataset_tab.png)

Datasetsディレクトリで、取り込まれたすべてのデータセットを確認できます。 ディレクトリにデータセットが大量に格納されている場合は、すべてのエントリの読み込みに1分かかる場合があります。

データセットは同じなので、前の節で外部データを使用した読み込みデータを置き換えます。 「 **データを読み込み** 」でコードブロックを選択し、キーボードの **** Dキーを2回押します。 フォーカスがテキスト内ではなく、ブロック内にあることを確認します。 2回押す前に、 **Escキーを押し** てテキストフォーカスをエスケープ **でき** ます。

これで、データセットを右クリックし、ドロップダウンの「Explore Data in Notebook」オプションを選択でき `Retail-Training-<your-alias>` ます。 実行可能なコードエントリがノートブックに表示されます。

>[!TIP] コードを変換するには、『 [プラットフォームSDK](../authoring/platform-sdk.md) 』ガイドを参照してください。

```PYTHON
from data_access_sdk_python.reader import DataSetReader
from datetime import date
reader = DataSetReader()
df = reader.load(data_set_id="xxxxxxxx", ims_org="xxxxxxxx@AdobeOrg")
df.head()
```

Python以外のカーネルで作業している場合は、 [このページを参照して](https://github.com/adobe/acp-data-services-dsw-reference/wiki/Accessing-Data-on-the-Platform) 、Adobe Experience Platformのデータにアクセスしてください。

実行可能セルを選択し、ツールバーの再生ボタンを押すと、実行可能コードが実行されます。 の出力は、データセットのキー `head()` を列、データセット内の最初のn行に含むテーブルになります。 `head()` 出力する行数を指定する整数引数を受け取ります。 デフォルトは5です。

![](../images/jupyterlab/analyze-data/datasetreader_head.png)

カーネルを再起動して、すべてのセルを再び実行した場合は、以前と同じ出力を得る必要があります。

![](../images/jupyterlab/analyze-data/restart_kernel_run.png)


### データの調査

データにアクセスできるようになったので、統計と視覚化を使用して、データ自体に焦点を当ててみましょう。 使用しているデータセットは、ある特定の日に45の店舗に関するその他の情報を提供する小売データセットです。 特定のオブジェクトのいくつかの特性 `date` と、次 `store` のものがあります。
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

Pythonのパンダライブラリを利用して、各属性のデータ型を取得できます。 次の呼び出しの出力は、各列のエントリ数とデータタイプに関する情報を提供します。

```PYTHON
df.info()
```

![](../images/jupyterlab/analyze-data/df_info.png)

この情報は、各列のデータタイプを知ることで、データの処理方法を知ることができるので役立ちます。

統計的概要を見てみましょう。 数値データ型のみが表示され、そ `date`の、 `storeType`および `isHoliday` は出力されません。

```PYTHON
df.describe()
```

![](../images/jupyterlab/analyze-data/df_describe.png)

これにより、各特性に対して6435個のインスタンスが存在することがわかります。 また、平均、標準偏差(std)、最小、最大、四分位数などの統計情報も提供されます。 これにより、データの偏差に関する情報が得られます。 次の節では、ビジュアライゼーションについて説明します。ビジュアライゼーションは、この情報と組み合わせて、データに関する十分な理解を得るために役立ちます。

の最小値と最大値を調べると `store`、データが表す一意のストアが45個あることがわかります。 店とは何かを区別する `storeTypes` ものもある。 の配布は、次の手順を実行す `storeTypes` ると確認できます。

![](../images/jupyterlab/analyze-data/df_groupby.png)

これは、22店舗が、17店舗 `storeType` 、 `A`6店舗であるこ `storeType` とを意味する `B``storeType``C`。

### データの視覚化

データフレームの値がわかったので、ビジュアライゼーションを使用してこれを補足し、パターンをより明確にし、より簡単に識別できるようにします。 グラフは、結果をオーディエンスに伝える場合にも便利です。 視覚化に役立つPythonライブラリには、次のものがあります。
- [Matplotlib](https://matplotlib.org/)
- [パンダ](https://pandas.pydata.org/)
- [海辺](https://seaborn.pydata.org/)
- [ggplot](https://ggplot2.tidyverse.org/)

この節では、各ライブラリの使用に関する利点を簡単に説明します。

[Matplotlib](https://matplotlib.org/) は、最も古いPythonビジュアライゼーションパッケージです。 彼らの目標は、「簡単で難しいことを可能にする」ことだ。 これは非常に強力なパッケージであり、複雑さも伴うためです。 相当な時間と労力をかけずに合理的なグラフを得るのは、必ずしも簡単ではない。

[Pandas](https://pandas.pydata.org/) は主にDataFrameオブジェクトに使用され、統合インデックスを使用したデータ操作が可能です。 ただし、pandasにはmatplotlibに基づく組み込みの印刷機能も含まれています。

[seaborn](https://seaborn.pydata.org/) は、matplotlibの上に構築されたパッケージです。 主な目的は、デフォルトのグラフをより視覚的に訴えるようにし、複雑なグラフを簡単に作成することです。

[ggplot](https://ggplot2.tidyverse.org/) はmatplotlibの上にも構築されたパッケージです。 ただし、主な違いは、ツールがR用のggplot2のポートであることです。seabornと同様に、matplotlibを使用して改善することです。 R向けのggplot2に詳しいユーザは、このライブラリを検討する必要があります。


### 一変グラフ

単変数グラフは、個々の変数をグラフ化したものです。 一般的な単変量グラフを使用してデータを視覚化するのは、ボックスとウィスカーのプロットです。

以前の小売データセットを使って、45店舗とその週間売上高のそれぞれに対するボックスとウィスカープロットを生成できます。 プロットは、この `seaborn.boxplot` 関数を使用して生成されます。

![](../images/jupyterlab/analyze-data/box_whisker.png)

ボックスとウィスカープロットは、データの分布を示すのに使用されます。 プロットの外側の線は上下の四分位数を示し、ボックスは四分位数の範囲に及びます。 ボックス内の行は中央値を示しています。 四分位数または四分位数の1.5を超えるデータポイントは、すべて円としてマークされます。 これらの点は外れ値と見なされます。

### 多変量分析グラフ

多変量分析プロットは、変数間の相互作用を確認するために使用されます。 このビジュアライゼーションを使用すると、データ科学者は変数間に相関関係やパターンがあるかどうかを確認できます。 一般的な多変量分析グラフは相関行列です。 相関行列を用いて、相関係数を用いて、複数の変数間の依存関係を定量化する。

同じ小売データセットを使用して、相関行列を生成できます。

![](../images/jupyterlab/analyze-data/correlation_1.png)

中心から斜めの1が下にあることに注目してください。 これは、変数をそれ自体と比較する場合、変数に完全な正の相関関係があることを示しています。 強い正の相関関係は1に近い大きさになり、弱い相関関係は0に近い大きさになります。 負の相関は、逆の傾向を示す負の係数と共に表示されます。


## 次の手順

このチュートリアルでは、Data Science Workspaceで新しいジャプターノートブックを作成する方法、およびAdobe Experience Platformから外部データにアクセスする方法について説明しました。 具体的には、次の手順に従いました。
- 新しいジュピターノートブックを作成する
- データセットとスキーマへのアクセス
- データセットの調査

これで、 [次のセクションに進んで、レシピをパッケージ化し](../models-recipes/package-source-files-recipe.md) 、Data Science Workspaceにインポートする準備が整いました。