---
keywords: Experience Platform;JupyterLab；ノートブック；データサイエンスWorkspace；人気のトピック；データノートブックの分析
solution: Experience Platform
title: ノートブックを使用したデータの分析
type: Tutorial
description: このチュートリアルでは、Data Science Workspace内に作成された Jupyter Notebooks を使用して、データにアクセス、調査、視覚化する方法について重点的に説明します。
exl-id: 3b0148d1-9c08-458b-9601-979cb6c7a0fb
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1730'
ht-degree: 72%

---

# ノートブックを使用したデータの分析

>[!NOTE]
>
>Data Science Workspaceは購入できなくなりました。
>
>このドキュメントは、Data Science Workspaceの以前の使用権限を持つ既存のお客様を対象としています。

このチュートリアルでは、Data Science Workspace内に作成された Jupyter Notebooks を使用して、データにアクセス、調査、視覚化する方法について重点的に説明します。 このチュートリアルを終了すると、Jupyter Notebooks が提供するいくつかの機能を理解でき、データをより深く理解できます。

次の概念が導入されています。

- **[!DNL JupyterLab]:** [[!DNL JupyterLab]](https://blog.jupyter.org/jupyterlab-is-ready-for-users-5a6f039b8906) はプロジェクト Jupyter の次世代 web ベースのインターフェイスであり、[!DNL Adobe Experience Platform] に緊密に統合されています。
- **バッチ**：データセットはいくつかのバッチで構成されます。バッチとは、一定期間に収集され 1 つの単位として一緒に処理される一連のデータです。データセットにデータが追加されると、新しいバッチが作成されます。
- **データアクセス SDK（廃止）**：データアクセス SDK は廃止されました。[[!DNL Experience Platform SDK]](../authoring/platform-sdk.md) ガイドをご利用ください。

## Data Science Workspace でのノートブックの調査

この節では、既に小売販売スキーマに取り込まれているデータを調査します。

Data Science Workspaceを使用すると、[!DNL JupyterLab] Platform を通じて [!DNL Jupyter Notebooks] ータを作成し、機械学習ワークフローを作成および編集できます。 [!DNL JupyterLab] は、ユーザーが web ブラウザーを使用してノートブックドキュメントを編集できる、サーバーとクライアントの共同作業ツールです。 これらのノートブックには、実行可能コードとリッチテキスト要素の両方を含めることができます。この目的のために、分析の説明と実行可能な [!DNL Python] コードに Markdown を使用して、データの探索と分析を実行します。

### ワークスペースの選択

[!DNL JupyterLab] を起動すると、Jupyter Notebooks の web ベースのインターフェイスが表示されます。 選択するノートブックタイプに応じて、対応するカーネルが起動します。

使用する環境を比較する際には、各サービスの制限事項を考慮する必要があります。例えば、[!DNL Python] で [pandas](https://pandas.pydata.org/) ライブラリを使用している場合、通常のユーザーとして使用できる RAM の上限は 2 GB です。 パワーユーザーの場合でも、20 GB の RAM に制限されます。より大きな計算を処理する場合は、すべてのノートブックインスタンスで共有される 1.5 TB を提供する [!DNL Spark] を使用することは理にかなっています。

デフォルトでは、Tensorflow レシピは GPU クラスターで動作し、Python は CPU クラスター内で動作します。

### ノートコンピューターの新規作成

[!DNL Adobe Experience Platform] UI の上部メニューで「[!UICONTROL Data Science]」を選択すると、Data Science Workspaceに移動します。 このページで「[!DNL JupyterLab]」を選択して、[!DNL JupyterLab] ランチャーを開きます。 次のようなページが表示されます。

![](../images/jupyterlab/analyze-data/jupyterlab-launcher-new.png)

このチュートリアルでは、Jupyter Notebook の [!DNL Python] 3 を使用して、データにアクセスし調査する方法を示します。 ランチャーページには、サンプルのノートブックが用意されています。[!DNL Python]3 の小売販売レシピを使用します。

![](../images/jupyterlab/analyze-data/retail_sales.png)

Retail Sales レシピは、同じ Retail Sales データセットを使用して、Jupyter ノートブックでデータを調査し視覚化する方法を示すスタンドアロンの例です。さらに、このノートブックでは、トレーニングと検証をさらに進めます。このノートブックについて詳しくは、[こちらの説明](../walkthrough.md)を参照してください。

### データへのアクセス

>[!NOTE]
>
>この `data_access_sdk_python` は非推奨となり、推奨されなくなりました。 SDK コードを変換するには、[Experience Platform SDKへのデータアクセスの変換 ](../authoring/platform-sdk.md) チュートリアルを参照してください。 このチュートリアルでも、以下と同じ手順を使用します。

[!DNL Adobe Experience Platform] ーザーの内部データへのアクセスと、外部データへのアクセスを見直します。 `data_access_sdk_python` ライブラリを使用して、データセットや XDM スキーマなどの内部データにアクセスします。外部データの場合は、pandas [!DNL Python] ライブラリを使用します。

#### 外部データ

Retail Sales ノートブックを開き、「Load Data」ヘッダーを探します。次の [!DNL Python] コードでは、pandas の `DataFrame` データ構造と [read_csv （） ](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html#pandas.read_csv) 関数を使用して、[!DNL Github] でホストされている CSV を DataFrame に読み込みます。

![](../images/jupyterlab/analyze-data/read_csv.png)

pandas の DataFrame データ構造は、2 次元のラベル付きデータ構造です。データの次元をすばやく確認するには、`df.shape` を使用します。DataFrame の次元を表すタプルが返されます。

![](../images/jupyterlab/analyze-data/df_shape.png)

最後に、どのようなデータであるかを見てみましょう。`df.head(n)` を使用して、DataFrame の最初の `n` 行を表示できます。

![](../images/jupyterlab/analyze-data/df_head.png)

#### [!DNL Experience Platform] データ

次に、[!DNL Experience Platform] のデータへのアクセスについて説明します。

##### データセット ID 別

この節では、Retail Sales データセットを使用します。これは、Retail Sales サンプルノートブックで使用されているのと同じデータセットです。

Jupyter Notebook では、左側の **データ** タブ ![ データタブ ](../images/jupyterlab/analyze-data/dataset-tab.png) からデータにアクセスできます。 タブを選択すると、2 つのフォルダーが表示されます。 **[!UICONTROL データセット]** フォルダーを選択します。

![](../images/jupyterlab/analyze-data/dataset_tab.png)

これで、データセットディレクトリに、取り込まれたすべてのデータセットが表示されます。 ディレクトリにデータセットが大量に入力されている場合は、すべてのエントリの読み込みに 1 分程度かかる場合があります。

データセットは同じなので、外部データを使用した前の節の読み込みデータを置き換えます。「**Load Data**」の下のコードブロックを選択し、キーボードの **D** キーを 2 回押します。フォーカスがテキスト内ではなく、ブロック上にあることを確認します。**D** キーを 2 回押す前に、**Esc** キーを押してテキストフォーカスをエスケープすることができます。

これで、`Retail-Training-<your-alias>` データセットを右クリックし、ドロップダウンの「Explore Data in Notebook」オプションを選択できます。実行可能コードエントリがノートブックに表示されます。

>[!TIP]
>
>コードを変換するには、[[!DNL Experience Platform SDK]](../authoring/platform-sdk.md) ガイドを参照してください。

```PYTHON
from data_access_sdk_python.reader import DataSetReader
from datetime import date
reader = DataSetReader()
df = reader.load(data_set_id="xxxxxxxx", ims_org="xxxxxxxx@AdobeOrg")
df.head()
```

[!DNL Python] 以外のカーネルで作業している場合は、[ このページ ](https://github.com/adobe/acp-data-services-dsw-reference/wiki/Accessing-Data-on-the-Platform) を参照して [!DNL Adobe Experience Platform] ーバー上のデータにアクセスしてください。

実行可能なセルを選択し、ツールバーの再生ボタンを押すと、実行可能コードが実行されます。`head()` の出力は、データセットのキーを列にした、データセットの最初の n 行分のテーブルです。`head()` では、出力する行数を指定する整数の引数を受け取ります。デフォルトでは、これは 5 です。

![](../images/jupyterlab/analyze-data/datasetreader_head.png)

カーネルを再起動し、すべてのセルを再実行した場合は、前回と同じ出力が得られるはずです。

![](../images/jupyterlab/analyze-data/restart_kernel_run.png)


### データの調査

これでデータにアクセスできるようになったので、統計と視覚化を使用して、データそのものに焦点を当てましょう。使用するデータセットは、45 箇所の異なる店舗に関するある日の各種情報を提供する小売データセットです。特定の `date` および `store` の特性の一部を次に示します。
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

Pandas ライブラリ [!DNL Python's] 活用して、各属性のデータタイプを取得できます。 以下の呼び出しの出力では、各列のエントリ数とデータタイプに関する情報が得られます。

```PYTHON
df.info()
```

![](../images/jupyterlab/analyze-data/df_info.png)

各列のデータタイプを知ることで、データの処理方法がわかるので、この情報は役に立ちます。

次に、統計概要を見てみましょう。数値データタイプのみが表示されるので、`date`、`storeType`、`isHoliday` は出力されません。

```PYTHON
df.describe()
```

![](../images/jupyterlab/analyze-data/df_describe.png)

これにより、特性ごとに 6435 個のインスタンスが存在することがわかります。また、平均、標準偏差（std）、最小、最大、四分位数などの統計情報も提供されます。これにより、データの偏差に関する情報が得られます。次の節では、視覚化について説明します。視覚化では、これらの情報を総合してデータの理解を深めることができます。

`store` の最小値と最大値を見ると、データが表す 45 箇所の一意の店舗があることがわかります。店舗の種類を識別する `storeTypes` もあります。次の方法で `storeTypes` の分布を確認できます。

![](../images/jupyterlab/analyze-data/df_groupby.png)

つまり、22 店舗が `storeType` `A`、17 店舗が `storeType` `B`、6 店舗が `storeType``C` です。

#### データの視覚化

データフレームの値がわかったので、これに視覚化を補って、情報をより明確にし、パターンを識別しやすくしたいと思います。結果をオーディエンスに伝える場合はグラフも便利です。ビジュアライゼーションに役立つ [!DNL Python] ライブラリを以下に示します。
- [Matplotlib](https://matplotlib.org/)
- [pandas](https://pandas.pydata.org/)
- [seaborn](https://seaborn.pydata.org/)
- [ggplot](https://ggplot2.tidyverse.org/)

この節では、各ライブラリを使用する利点を簡単に説明します。

[Matplotlib](https://matplotlib.org/) は、最も古い [!DNL Python] ビジュアライゼーションパッケージです。 「容易なことを容易に、難しいことを可能に」することを目的にしています。このパッケージはきわめて強力ですが、同時に複雑でもあるので、その傾向はあります。相当な時間と労力をかけなければ、見栄えのまずまず良いグラフを得るのは必ずしも容易ではありません。

[pandas](https://pandas.pydata.org/) は、統合インデックス付けを使用したデータ操作が可能な DataFrame オブジェクトに主に使用されます。ただし、pandas には、Matplotlib をベースにしたプロット機能も組み込まれています。

[seaborn](https://seaborn.pydata.org/) は、Matplotlib の上に構築されたパッケージです。デフォルトのグラフをより視覚に訴えるものにし、複雑なグラフを簡単に作成できるようにすることが主な目的です。

[ggplot](https://ggplot2.tidyverse.org/) は、Matplotlib の上に構築されたパッケージです。ただし、主な違いは、このツールが R の ggplot2 を移植したものであることです。seaborn と同様に、Matplotlib を改良することが目的です。R の ggplot2 を使い慣れたユーザーは、このライブラリの使用を検討してください。


##### 単一変量グラフ

単一変量グラフは、個々の変数のグラフです。データの視覚化に使用される一般的な単一変量グラフは、ボックスウィスカープロットです。

以前の小売データセットを使って、45 店舗とその週間売上高ごとにボックスウィスカープロットを作成できます。プロットは、`seaborn.boxplot` 関数を使用して生成されます。

![](../images/jupyterlab/analyze-data/box_whisker.png)

ボックスウィスカープロットは、データの分布を示すのに使用されます。プロットの外側の線は上下の四分位数を示し、ボックスは四分位数の範囲に広がります。ボックス内の行は中央値を示します。上四分位数または下四分位数の 1.5 倍以上のデータポイントは、円としてマークされます。これらの点は外れ値と見なされます。

##### 多変量グラフ

多変量プロットは、変数間の相互作用を確認するために使用されます。この視覚化手法を使用すると、データサイエンティストは変数間に相関関係やパターンがあるかどうかを確認できます。一般的に使用される多変量グラフは相関行列です。多変数間の依存関係が相関係数で定量化されます。

同じ小売データセットを使用して、相関行列を生成できます。

![](../images/jupyterlab/analyze-data/correlation_1.png)

対角線上に 1 が並んでいることに注目してください。これは、変数をそれ自身と比較する場合、完全に正の相関関係があることを示しています。強い正の相関は 1 に近い大きさになり、弱い相関は 0 に近い大きさになります。負の相関は、逆の傾向を示す負の係数で示されます。


## 次の手順

このチュートリアルでは、Data Science Workspaceで新しい Jupyter Notebook を作成する方法と、外部および [!DNL Adobe Experience Platform] からデータにアクセスする方法について説明しました。 特に、次の手順を詳しく説明しました。
- 新しい Jupyter Notebook の作成
- データセットとスキーマへのアクセス
- データセットの調査

これで、[次の手順](../models-recipes/package-source-files-recipe.md)に進んで、レシピをパッケージ化し Data Science Workspace にインポートする準備が整いました。
