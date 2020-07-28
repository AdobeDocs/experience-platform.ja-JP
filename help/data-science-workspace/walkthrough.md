---
keywords: Experience Platform;walkthrough;Data Science Workspace;popular topics
solution: Experience Platform
title: Data Science Workspace　のチュートリアル
topic: Walkthrough
translation-type: tm+mt
source-git-commit: 1e5526b54f3c52b669f9f6a792eda0abfc711fdd
workflow-type: tm+mt
source-wordcount: '1638'
ht-degree: 91%

---


# [!DNL Data Science Workspace] walkthrough

This document provides a walkthrough for Adobe Experience Platform [!DNL Data Science Workspace]. 具体的には、データサイエンティストが扱う一般的なワークフローを調べ、機械学習を使用した問題の解決に取り組みます。

## 前提条件

- 登録済みの Adobe ID アカウント
   - Adobe ID アカウントは、Adobe Experience Platform および　へのアクセス権を持つ組織に追加されている必要があります[!DNL Data Science Workspace]

## データサイエンティストの動機

現在の市場での競争力を維持するために、ある小売業者は多くの課題に直面しています。小売業者の主な懸念事項の 1 つは、製品の最適価格を決定し、販売傾向を予測することです。正確な予測モデルを使用すると、小売業者は、需要と価格設定ポリシーの関係を見つけ出し、販売と売上高を最大化するために最適化された価格決定をおこなうことができます。

## データサイエンティストの解決法

データサイエンティストのソリューションは、小売業者がアクセスできる豊富な履歴データを活用し、将来の傾向を予測し、価格決定を最適化することです。過去の販売データを使って機械学習モデルを訓練し、モデルを使って将来の販売傾向を予測します。これにより、小売業者は価格を変更する際に役立つ洞察を得ることができます。

この概要では、データサイエンティストがデータセットを取得し、週別の売上を予測するモデルを作成する手順を説明します。We will go over the following sections in the Sample Retail Sales Notebook on Adobe Experience Platform [!DNL Data Science Workspace]:

- [セットアップ](#setup)
- [データの調査](#exploring-data)
- [特徴エンジニアリング](#feature-engineering)
- [トレーニングと検証](#training-and-verification)

### ノートブック [!DNL Data Science Workspace]

Firstly, we want to create a [!DNL JupyterLab] notebook to open the &quot;Retail Sales&quot; sample notebook. ノートブック内でデータサイエンティストがおこなった手順に従うことで、一般的なワークフローを理解できます。

In the Adobe Experience Platform UI, click on the Data Science tab in the top menu to take you to the [!DNL Data Science Workspace]. From this page, click on the [!DNL JupyterLab] tab which will open the [!DNL JupyterLab] launcher. 次のようなページが表示されます。

![](./images/walkthrough/jupyterlab_launcher.png)

In our tutorial, we will be using [!DNL Python] 3 in the [!DNL Jupyter Notebook] to show how to access and explore the data. ランチャーページには、サンプルのノートブックが用意されています。We will be using the &quot;Retail Sales&quot; sample for [!DNL Python] 3.

![](./images/walkthrough/retail_sales.png)

### セットアップ {#setup}

小売売上ノートブックを開いた状態で、まずワークフローに必要なライブラリを読み込みます。次のリストでは、それぞれの用途について簡単に説明します。
- **numpy** — 大きな多次元配列と行列のサポートを追加する科学計算ライブラリ
- **pandas** — データの操作と分析に使用するデータ構造と操作を提供するライブラリ
- **matplotlib.pyplot** — プロット時に MATLAB と同じような操作をおこなう印刷ライブラリ
- **seaborn** - matplotlib に基づく高レベルのインターフェイスデータ視覚化ライブラリ
- **sklearn** — 分類、回帰、サポートベクトルおよびクラスターアルゴリズムを備えた機械学習ライブラリ
- **warnings** — 警告メッセージを制御するライブラリ

### データの調査 {#exploring-data}

#### データの読み込み

ライブラリの読み込み後、データの確認を開始できます。The following [!DNL Python] code uses pandas&#39; `DataFrame` data structure and the [read_csv()](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html#pandas.read_csv) function to read the CSV hosted on [!DNL Github] into the pandas DataFrame:

![](./images/walkthrough/read_csv.png)

Pandas の DataFrame データ構造は、2 次元のラベル付きデータ構造です。データの次元をすばやく確認するには、`df.shape` を使用します。DataFrame の次元を表すタプルが返されます。

![](./images/walkthrough/df_shape.png)

最後に、どのようなデータであるかを見てみましょう。`df.head(n)` を使用して、DataFrame の最初の `n` 行を表示できます。

![](./images/walkthrough/df_head.png)

#### 統計概要

We can leverage [!DNL Python's] pandas library to get the data type of each attribute. 以下の呼び出しの出力では、各列のエントリ数とデータタイプに関する情報が得られます。

```PYTHON
df.info()
```

![](./images/walkthrough/df_info.png)

各列のデータタイプを知ることで、データの処理方法がわかるので、この情報は役に立ちます。

次に、統計の概要を見てみましょう。数値データタイプのみが表示されるため、`date`、`storeType`、`isHoliday`は出力されません。

```PYTHON
df.describe()
```

![](./images/walkthrough/df_describe.png)

これにより、各特性に対して 6435 個のインスタンスが存在することがわかります。また、平均、標準偏差（std）、最小、最大、四分位数などの統計情報も表示されます。これにより、データの偏差に関する情報が得られます。次の節では、この情報と連携して機能するビジュアライゼーションを見て、データを完全に理解していきます。

`store` の最小値と最大値を見ると、データが表す一意の店舗が 45 件あることがわかります。店舗の種類を識別する `storeTypes` もあります。`storeTypes` の配布は、次の方法で確認できます。

![](./images/walkthrough/df_groupby.png)

つまり、22 店舗が `storeType A`、17 店舗が `storeType B`、6 店舗が `storeType C` です。

#### データの視覚化

データフレームの値がわかったので、パターンをより明確にし、より簡単に識別できるように、ビジュアライゼーションを使用してこれを補完したいと考えています。これらのグラフは、結果をオーディエンスに伝える場合にも便利です。

#### 一変量グラフ

一変量グラフは、個々の変数のグラフです。データの視覚化に使用される一般的な一変量グラフは、箱ひげ図です。

以前の小売データセットを使って、45 店舗とその週間販売のそれぞれの箱ひげ図を作成できます。プロットは、`seaborn.boxplot` 関数を使用して生成されます。

![](./images/walkthrough/box_whisker.png)

箱ひげ図は、データの分布を示すのに使用されます。箱ひげ図の外側の線は上下の四分位数を示し、ボックスは四分位数の範囲に及びます。ボックス内の線は中央値を示します。上四分位数または下四分位数の 1.5 倍以上のデータポイントは、円としてマークされます。これらの点は外れ値と見なされます。

次に、週別の売り上げを時間と共にプロットします。1 店舗目の出力のみを表示します。ノートブック内のコードは、データセット内の 45 店舗のうち 6 店舗に対応する 6 プロットを生成します。

![](./images/walkthrough/weekly_sales.png)

この図では、2 年間の週別売上高を比較できます。販売のピークやトラフのパターンが時間の経過と共に見えやすくなります。

#### 多変量分析グラフ

多変量プロットは、変数間の相互作用を確認するために使用されます。この視覚化手法を使用すると、データサイエンティストは変数間に相関関係やパターンがあるかどうかを確認できます。一般的に使用される多変量グラフは相関行列です。多変数間の依存関係が相関係数で定量化されます。

同じ小売データセットを使用して、相関行列を生成できます。

![](./images/walkthrough/correlation_1.png)

真ん中に斜めの 1 がそろっていることに注目してください。これは、変数をそれ自体と比較する場合、完全に正の相関関係があることを示しています。強い正の相関は 1 に近い大きさになり、弱い相関は 0 に近い大きさになります。負の相関は、逆の傾向を示す負の係数で示されます。

### 特徴エンジニアリング {#feature-engineering}

この節では、小売データセットを変更します。以下の操作を実行します。

- 週と年の列を足す
- storeType をインジケーター変数に変換する
- isHoliday を数値変数に変換する
- 来週の売り上げを予測する

#### 週と年の列を足す

日付の現在の形式（`2010-02-05`）では、週ごとのデータを区別するのが困難です。このため、日付を週と年に変換します。

![](./images/walkthrough/date_to_week_year.png)

週と日付は次のようになります。

![](./images/walkthrough/date_week_year.png)

#### storeType をインジケーター変数に変換する

次に、storeType 列を各 `storeType` 列を表す列に変換します。店舗タイプは `A`、`B`、`C`の 3 つで、ここから新しい列を作成します。各列に設定された値はブール値になり、「1」は `storeType` に応じて設定され、その他の 2 列に対して `0` が設定されます。

![](./images/walkthrough/storeType.png)

現在の `storeType` 列は削除されます。

#### isHoliday を数値型に変換する

次の修正は、`isHoliday` ブール値を数値表現に変更することです。

![](./images/walkthrough/isHoliday.png)


#### 来週の売上を予測する

次に、各データセットに、前週の売上高と将来の週の売上高を追加します。これは `weeklySales` をオフセットしておこないます。さらに、`weeklySales` の差を計算しています。これは、`weeklySales` で前の週の `weeklySales` 値を引くことでおこなわれます 。

![](./images/walkthrough/weekly_past_future.png)

`weeklySales` データ 45 データセットを前方に、45 データセットを後方にオフセットして新しい列を作成するので、最初と最後の 45 データポイントには NaN 値が設定されます。NaN 値を持つすべての行を削除する `df.dropna()` 関数を使用して、これらのポイントをデータセットから削除できます。

![](./images/walkthrough/dropna.png)

次に、変更後のデータセットの概要を示します。

![](./images/walkthrough/df_info_new.png)

### トレーニングと検証 {#training-and-verification}

次に、データの一部のモデルを作成し、将来の売上を予測するのに最もパフォーマンスの高いモデルを選択します。以下の 5 つのアルゴリズムを評価します。

- 線形回帰
- 決定木
- ランダムフォレスト
- 勾配ブースティング
- K 近傍法

#### データセットのトレーニングおよびテストサブセットへの分割

モデルがどの程度正確に値を予測できるかを知る方法が必要です。この評価は、検証として使用するデータセットの一部を割り当て、残りをトレーニングデータとして割り当てることでおこなうことができます。`weeklySalesAhead` は `weeklySales` の実際の将来値なので、この値を使用して、モデルが値を予測する際の正確さを評価できます。分割は次の手順でおこないます。

![](./images/walkthrough/split_data.png)

これで、モデルを準備するための `X_train` と `y_train`、そして後での評価をおこなうための `X_test` と `y_test` がそろいます。

#### スポットチェックアルゴリズム

この節では、すべてのアルゴリズムを `model` という配列に宣言します。次に、この配列を繰り返し処理し、各アルゴリズムに対して、モデル `mdl` を作成するトレーニングデータを `model.fit()` で入力します。このモデルを使って、`X_test` のデータによって `weeklySalesAhead` を予測します。

![](./images/walkthrough/training_scoring.png)

スコアリングの場合、予測された `weeklySalesAhead` 値と `y_test` データ内の実際の値との間の平均パーセント差を取ります。予測と実際の差を最小に抑えたいので、グラデーション昇圧回帰は最もパフォーマンスの高いモデルです。

#### 予測の視覚化

最後に、予測モデルを実際の週別の売上高値で視覚化します。青い線は実際の数を表し、緑は勾配ブースティングを使用した予測を表します。次のコードは、データセット内の 45 店舗のうち 6 店舗を表す 6 プロットを生成します。ここでは `Store 1` のみが表示されます。

![](./images/walkthrough/visualize_prediction.png)

<!--TODO UI Flow> -->

## まとめ

この概要を踏まえて、データサイエンティストが小売売上の問題を解決するためにおこなうワークフローを調べました。具体的には、次の手順を実行し、将来の週別売上高を予測するソリューションを実現しました。

- [セットアップ](#setup)
- [データの調査](#exploring-data)
- [特徴エンジニアリング](#feature-engineering)
- [トレーニングと検証](#training-and-verification)