---
keywords: Experience Platform;walkthrough;Data Science Workspace;popular topics
solution: Experience Platform
title: Data Science Workspaceのチュートリアル
topic: Walkthrough
translation-type: tm+mt
source-git-commit: 1e5526b54f3c52b669f9f6a792eda0abfc711fdd
workflow-type: tm+mt
source-wordcount: '1638'
ht-degree: 0%

---


# [!DNL Data Science Workspace] walkthrough

このドキュメントでは、Adobe Experience Platformのチュートリアルを提供 [!DNL Data Science Workspace]します。 具体的には、データサイエンティストが通る一般的なワークフローを調べ、機械学習を使用した問題の解決を図ります。

## 前提条件

- 登録されたAdobe IDアカウント
   - Adobe IDアカウントは、Adobe Experience Platformおよび [!DNL Data Science Workspace]

## データサイエンティストの動機

ある小売業者は、現在の市場での競争力を維持するための多くの課題に直面しています。 小売業者の主な懸念の1つは、商品の最適価格を決定し、販売傾向を予測することです。 正確な予測モデルを使用すると、小売業者は、需要と価格設定ポリシーの関係を見つけ出し、販売と売上高を最大限にするために最適化された価格決定を行うことができます。

## データサイエンティストの解

データサイエンティストのソリューションは、小売業者がアクセスできる豊富な履歴データを活用し、将来のトレンドを予測し、価格決定を最適化することです。 過去の販売データを使って機械学習モデルをトレーニングし、モデルを使用して将来の販売傾向を予測します。 これにより、小売業者は価格を変更する際に役立つインサイトを得ることができます。

この概要では、データ科学者がデータセットを取得し、週別の売上を予測するモデルを作成する手順について説明します。 Adobe Experience Platform版小売売上ノートブックの例では、以下のセクションを参照し [!DNL Data Science Workspace]ます。

- [セットアップ](#setup)
- [データの調査](#exploring-data)
- [機能エンジニアリング](#feature-engineering)
- [トレーニングと検証](#training-and-verification)

### ノートブック [!DNL Data Science Workspace]

まず、「小売売上」サンプルノートブックを開く [!DNL JupyterLab] ノートブックを作成します。 ノートブック内のデータサイエンティストが行った手順に従って、一般的なワークフローを理解できます。

Adobe Experience PlatformUIで、上部メニューの[データ科学]タブをクリックして、に移動し [!DNL Data Science Workspace]ます。 このページで、ランチャーを開く [!DNL JupyterLab] タブをクリックし [!DNL JupyterLab] ます。 次のようなページが表示されます。

![](./images/walkthrough/jupyterlab_launcher.png)

このチュートリアルでは、の3を使用して、データにアクセスして調査する方法 [!DNL Python][!DNL Jupyter Notebook] を示します。 ランチャーページには、サンプルノートブックが用意されています。 ここでは、 [!DNL Python] 3.に「小売売上」サンプルを使用します。

![](./images/walkthrough/retail_sales.png)

### セットアップ {#setup}

小売売上ノートブックを開いた状態で、まずワークフローに必要なライブラリを読み込みます。 次のリストでは、それぞれの用途を簡単に説明します。
- **numpy** — 大きな多次元の配列と行列のサポートを追加する科学計算ライブラリ
- **pandas** — データ操作と分析に使用されるデータ構造と操作をオファーするライブラリ
- **matplotlib.pyplot** — 印刷時にMATLABと同じ経験を提供する印刷ライブラリ
- **seaborn** - matplotlibに基づく高レベルのインターフェイスデータ視覚化ライブラリ
- **sklearn** — 分類、回帰、サポートベクトルおよびクラスターアルゴリズムを備えた機械学習ライブラリ
- **警告** — 警告メッセージを制御するライブラリ

### データの調査 {#exploring-data}

#### データの読み込み

ライブラリの読み込み後、開始でデータを確認できます。 次の [!DNL Python] コードは、pandasの `DataFrame` データ構造と [read_csv()](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html#pandas.read_csv)[!DNL Github] 関数を使用して、pandas DataFrameでホストされているCSVを読み取ります。

![](./images/walkthrough/read_csv.png)

PandasのDataFrameデータ構造は、2次元のラベル付きデータ構造です。 データの次元をすばやく見るには、を使用し `df.shape`ます。 次に、DataFrameの次元を表すタプルを返します。

![](./images/walkthrough/df_shape.png)

最後に、データがどのように表示されるかを試すことができます。 を使用して、DataFrame `df.head(n)` の最初の `n` 行を表示できます。

![](./images/walkthrough/df_head.png)

#### 統計概要

各属性のデータタイプを取得するために、 [!DNL Python's] pandasライブラリを利用できます。 次の呼び出しの出力は、各列のエントリ数とデータタイプに関する情報を提供します。

```PYTHON
df.info()
```

![](./images/walkthrough/df_info.png)

この情報は、各列のデータタイプを知ることで、データの処理方法を知ることができるので役立ちます。

統計的概要を見てみましょう。 次の数値データ型のみが表示され `date`、、 `storeType`および `isHoliday` は出力されません。

```PYTHON
df.describe()
```

![](./images/walkthrough/df_describe.png)

これにより、各特性に対して6435個のインスタンスが存在することがわかります。 また、平均、標準偏差(std)、最小、最大、四分位数などの統計情報も提供されます。 これにより、データの偏差に関する情報が得られます。 次の節では、ビジュアライゼーションについて説明します。このビジュアライゼーションとこの情報を組み合わせて、データの完全な理解を得ることができます。

の最小値と最大値を調べると `store`、データが表す一意のストアが45個あることがわかります。 店とは何かを区別する `storeTypes` ものもある。 の配布は、次の手順を実行す `storeTypes` ると確認できます。

![](./images/walkthrough/df_groupby.png)

これは、22店舗の店舗数が `storeType A` 、17店舗、 `storeType B`6店舗の店舗数を表 `storeType C`す。

#### データの視覚化

データフレームの値がわかったので、ビジュアライゼーションを使用してこれを補足し、パターンをより明確にし、より簡単に識別できるようにします。 これらのグラフは、結果をオーディエンスに伝える際にも役立ちます。

#### 一変グラフ

単変数グラフは、個々の変数をグラフ化したものです。 データの視覚化に使用される一般的な単変量グラフは、ボックスとウィスカープロットです。

以前の小売データセットを使って、45店舗とその週間売上高のそれぞれに対するボックスとウィスカープロットを生成できます。 プロットは、この `seaborn.boxplot` 関数を使用して生成されます。

![](./images/walkthrough/box_whisker.png)

ボックスとウィスカープロットは、データの分布を示すのに使用されます。 プロットの外側の線は上下の四分位数を示し、ボックスは四分位数の範囲に及びます。 ボックス内の行は中央値を示しています。 四分位数または四分位数の1.5を超えるデータポイントは、すべて円としてマークされます。 これらの点は外れ値と見なされます。

次に、週別の売り上げを時間とともにプロットします。 ファーストストアの出力だけを表示します。 ノートブックのコードは、データセット内の45店舗のうち6店舗に対応する6プロットを生成します。

![](./images/walkthrough/weekly_sales.png)

この図を使って、2年間の週別売上高を比較できます。 販売のピークや谷のパターンを時間の経過とともに確認するのは容易です。

#### 多変量分析グラフ

多変量分析プロットは、変数間の相互作用を確認するために使用されます。 このビジュアライゼーションを使用すると、データ科学者は変数間に相関関係やパターンがあるかどうかを確認できます。 一般的な多変量分析グラフは相関行列です。 相関行列を用いて、相関係数を用いて、複数の変数間の依存関係を定量化する。

同じ小売データセットを使用して、相関行列を生成できます。

![](./images/walkthrough/correlation_1.png)

中央の斜めの部分に注目します。 これは、変数をそれ自体と比較する場合、変数に完全な正の相関関係があることを示しています。 強い正の相関関係は1に近い大きさになり、弱い相関関係は0に近い大きさになります。 負の相関は、逆の傾向を示す負の係数と共に表示されます。

### 機能エンジニアリング {#feature-engineering}

この節では、小売データセットを変更します。 我々は以下の作業を行う。

- 週と年の列を追加する
- storeTypeをインジケーター変数に変換
- isHolidayを数値変数に変換します。
- 来週の売上を予測する

#### 追加週列と年列

日付(`2010-02-05`)の現在の形式では、毎週のデータを区別するのが難しくなります。 このため、日付を週および年に変換します。

![](./images/walkthrough/date_to_week_year.png)

今度は、週と日付が次のようになります。

![](./images/walkthrough/date_week_year.png)

#### storeTypeをインジケーター変数に変換

次に、storeType列をそれぞれを表す列に変換し `storeType`ます。 ストアタイプは(`A`、 `B`、 `C`)という3種類あり、そこから新しい列を3つ作成します。 各列に設定される値はboolean値になり、その他の2列に対して&#39;1&#39;が設定 `storeType` さ `0` れます。

![](./images/walkthrough/storeType.png)

現在の `storeType` 列は削除されます。

#### isHolidayを数値型に変換

次の変更は、ブール値を数値表現に変更す `isHoliday` ることです。

![](./images/walkthrough/isHoliday.png)


#### 次の週の売上予測

次に、各データセットに対して、前回と将来の週別売上高を追加します。 我々は、我々のオフセットを行うことでこれを行ってい `weeklySales`る。 さらに、その `weeklySales` 差異を計算しています。 これは、前の週 `weeklySales` を減算して行われ `weeklySales`ます。

![](./images/walkthrough/weekly_past_future.png)

データ45のデータセットを前方にオフセットし、45のデータセットを後方にオフセットして新しい列を作成するので、最初と最後の45のデータポイントにはNaN値が割り当てられます。 `weeklySales` NaN値を持つすべての行を削除する `df.dropna()` 関数を使用すると、データセットからこれらのポイントを削除できます。

![](./images/walkthrough/dropna.png)

変更後のデータセットの概要を以下に示します。

![](./images/walkthrough/df_info_new.png)

### トレーニングと検証 {#training-and-verification}

次に、データのいくつかのモデルを作成し、今後の売上を予測するのに最もパフォーマンスの高いモデルを選択します。 以下の5つのアルゴリズムを評価します。

- 線形回帰
- デシジョンツリー
- ランダムフォレスト
- グラデーションの倍率
- K隣人

#### データセットのトレーニングおよびテストのサブセットへの分割

モデルがどの程度正確に値を予測できるかを知る方法が必要です この評価は、検証に使用するデータセットの一部を割り当て、残りをトレーニングデータとして割り当てることで実行できます。 はの実際 `weeklySalesAhead``weeklySales`の将来値なので、この値を使用して、モデルが値を予測する際の正確さを評価できます。 分割は次のとおりです。

![](./images/walkthrough/split_data.png)

モデルの作成 `X_train` と評価のた `y_train` めの機能が、今では用意され `X_test``y_test` ています。

#### スポットチェックアルゴリズム

この節では、すべてのアルゴリズムをという配列に宣言し `model`ます。 次に、この配列を繰り返し実行し、各アルゴリズムに対して、モデルを作成するトレーニングデータ `model.fit()` を入力し `mdl`ます。 このモデルを使って、私たちの `weeklySalesAhead``X_test` データで予測する。

![](./images/walkthrough/training_scoring.png)

スコアリングに関して、予測された値とデータ内の実際の値との間の平均パーセント差 `weeklySalesAhead` を取り `y_test` ます。 予測と実際の差を最小限に抑えたいので、グラデーション昇圧回帰は最もパフォーマンスの高いモデルです。

#### 予測の視覚化

最後に、予測モデルを週別の実際の売上高値で視覚化します。 青い線は実際の数を表し、緑はグラデーション倍率を使用した予測を表します。 次のコードは、データセット内の45店舗のうち6店舗を表す6プロットを生成します。 以下 `Store 1` にのみ示します。

![](./images/walkthrough/visualize_prediction.png)

<!--TODO UI Flow> -->

## まとめ

この概要を踏まえて、データサイエンティストが小売売上の問題を解決するために経験するワークフローを調べました。 具体的には、次の手順を実行し、将来の週別売上を予測するソリューションを実現しました。

- [セットアップ](#setup)
- [データの調査](#exploring-data)
- [機能エンジニアリング](#feature-engineering)
- [トレーニングと検証](#training-and-verification)