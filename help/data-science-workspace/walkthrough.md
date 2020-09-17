---
keywords: Experience Platform;walkthrough;Data Science Workspace;popular topics
solution: Experience Platform
title: Data Science Workspace　のチュートリアル
topic: Walkthrough
description: このドキュメントでは、Adobe Experience Platform Data Science Workspace　のチュートリアルを提供します。特に、データサイエンティストが通る一般的なワークフローは、機械学習を使用した問題の解決に役立ちます。
translation-type: tm+mt
source-git-commit: 0d76b14599bc6b6089f9c760ef6a6be3a19243d4
workflow-type: tm+mt
source-wordcount: '1708'
ht-degree: 33%

---


# [!DNL Data Science Workspace] walkthrough

This document provides a walkthrough for Adobe Experience Platform [!DNL Data Science Workspace]. このチュートリアルでは、一般的なデータサイエンティストのワークフローと、機械学習を使用した問題へのアプローチ方法および解決方法について説明します。

## 前提条件

- 登録済みの Adobe ID アカウント
   - The Adobe ID account must have been added to an Organization with access to Adobe Experience Platform and [!DNL Data Science Workspace].

## 小売の使用例

現在の市場での競争力を維持するために、ある小売業者は多くの課題に直面しています。小売業者の主な懸念の1つは、商品の最適価格を決定し、販売傾向を予測することです。 正確な予測モデルを使用すると、小売業者は、需要と価格設定ポリシーの関係を見つけ、販売と売上高を最大化するために最適化された価格決定を行うことができます。

## データサイエンティストの解決法

データサイエンティストのソリューションは、小売業者が提供する豊富な履歴情報を活用して、将来の傾向を予測し、価格決定を最適化することです。 このチュートリアルでは、過去の販売データを使用して機械学習モデルをトレーニングし、モデルを使用して将来の販売傾向を予測します。 これにより、最適な価格の変更に役立つインサイトを生成できます。

この概要は、データサイエンティストがデータセットを取り込み、週別の売上を予測するモデルを作成する手順を反映しています。 このチュートリアルでは、Adobe Experience Platformの小売売上ノートブックの例で、次の節について説明し [!DNL Data Science Workspace]ます。

- [セットアップ](#setup)
- [データの調査](#exploring-data)
- [特徴エンジニアリング](#feature-engineering)
- [トレーニングと検証](#training-and-verification)

### ノートブック [!DNL Data Science Workspace]

[Adobe Experience PlatformUI]で、[ **[!UICONTROL Data Science]** ]タブから[ **[!UICONTROL Notebooks]** ]を選択し、[ [!UICONTROL Notebooks] Overview]ページに移動します。 このページから、 [!DNL JupyterLab] タブを選択して [!DNL JupyterLab] 環境を起動します。 のデフォルトのランディングページ [!DNL JupyterLab] は、 **[!UICONTROL ランチャーです]**。

![](./images/walkthrough/notebooks.png)

![](./images/walkthrough/jupyterlab_launcher.png)

このチュートリアルでは、の [!DNL Python] 3を使用 [!DNL JupyterLab Notebooks] して、データにアクセスして調査する方法を示します。 ランチャーページには、サンプルのノートブックが用意されています。以下に示す例では、 **[!UICONTROL 小売売上]** (RSA)のサンプルノートブックを使用しています。

### セットアップ {#setup}

小売売上ノートブックを開いた状態で、まずはワークフローに必要なライブラリを読み込む必要があります。 次のリストでは、後の手順の例で使用する各ライブラリについて簡単に説明します。

- **numpy**:大きな多次元の配列と行列のサポートを追加する科学計算ライブラリ
- **パンダ**:データ操作と分析に使用されるデータ構造と操作をオファーするライブラリ
- **matplotlib.pyplot**:印刷時にMATLABと同じ操作性を提供する印刷ライブラリ
- **seaborn** :matplotlibに基づく高レベルのインターフェイスデータ視覚化ライブラリ
- **sklearn**:分類、回帰、サポートベクトルおよびクラスターアルゴリズムを備えた機械学習ライブラリ
- **warnings**:警告メッセージを制御するライブラリ

### データの調査 {#exploring-data}

#### データの読み込み

ライブラリの読み込み後、開始がデータを確認できます。 The following [!DNL Python] code uses pandas&#39; `DataFrame` data structure and the [read_csv()](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html#pandas.read_csv) function to read the CSV hosted on [!DNL Github] into the pandas DataFrame:

![](./images/walkthrough/read_csv.png)

Pandas の DataFrame データ構造は、2 次元のラベル付きデータ構造です。To quickly see the dimensions of your data, you can use `df.shape`. DataFrame の次元を表すタプルが返されます。

![](./images/walkthrough/df_shape.png)

最後に、データの外観をプレビューできます。 You can use `df.head(n)` to view the first `n` rows of the DataFrame:

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

これにより、各特性に対して6435個のインスタンスが存在することがわかります。 また、平均、標準偏差（std）、最小、最大、四分位数などの統計情報も表示されます。これにより、データの偏差に関する情報が得られます。次の節では、ビジュアライゼーションについて説明します。このビジュアライゼーションは、この情報と組み合わせて、データに関する完全な理解を得るために役立ちます。

Looking at the minimum and maximum values for `store`, you can see that there are 45 unique stores the data represents. 店舗の種類を識別する `storeTypes` もあります。you can see the distribution of `storeTypes` by doing the following:

![](./images/walkthrough/df_groupby.png)

つまり、22 店舗が `storeType A`、17 店舗が `storeType B`、6 店舗が `storeType C` です。

#### データの視覚化

データフレームの値がわかったら、ビジュアライゼーションを使用してこれを補足し、パターンをより明確に識別しやすくします。 これらのグラフは、結果をオーディエンスに伝える場合にも便利です。

#### 一変量グラフ

一変量グラフは、個々の変数のグラフです。データの視覚化に使用される一般的な一変量グラフは、箱ひげ図です。

以前の小売データセットを使用して、45店舗とその週間販売のそれぞれに対するボックスとウィスカープロットを生成できます。 プロットは、`seaborn.boxplot` 関数を使用して生成されます。

![](./images/walkthrough/box_whisker.png)

箱ひげ図は、データの分布を示すのに使用されます。箱ひげ図の外側の線は上下の四分位数を示し、ボックスは四分位数の範囲に及びます。ボックス内の線は中央値を示します。上四分位数または下四分位数の 1.5 倍以上のデータポイントは、円としてマークされます。これらの点は外れ値と見なされます。

次に、週別売上高を時間とともにプロットできます。 最初のストアの出力のみを表示します。 ノートブック内のコードは、データセット内の 45 店舗のうち 6 店舗に対応する 6 プロットを生成します。

![](./images/walkthrough/weekly_sales.png)

この図では、2年間の週別売上高を比較できます。 販売のピークやトラフのパターンが時間の経過と共に見えやすくなります。

#### 多変量分析グラフ

多変量プロットは、変数間の相互作用を確認するために使用されます。この視覚化手法を使用すると、データサイエンティストは変数間に相関関係やパターンがあるかどうかを確認できます。一般的に使用される多変量グラフは相関行列です。多変数間の依存関係が相関係数で定量化されます。

同じ小売データセットを使用して、相関行列を生成できます。

![](./images/walkthrough/correlation_1.png)

真ん中に斜めの 1 がそろっていることに注目してください。これは、変数をそれ自体と比較する場合、完全に正の相関関係があることを示しています。強い正の相関は 1 に近い大きさになり、弱い相関は 0 に近い大きさになります。負の相関は、逆の傾向を示す負の係数で示されます。

### 特徴エンジニアリング {#feature-engineering}

この節では、以下の操作を実行して小売データセットを変更する際に、機能エンジニアリングを使用します。

- 週と年の列を足す
- storeTypeをインジケーター変数に変換
- isHolidayを数値変数に変換します
- 来週の売上を予測する

#### 週と年の列を足す

The current format for date (`2010-02-05`) can make it hard to differentiate that the data is for every week. このため、日付には週と年を含めるように変換する必要があります。

![](./images/walkthrough/date_to_week_year.png)

週と日付は次のようになります。

![](./images/walkthrough/date_week_year.png)

#### storeType をインジケーター変数に変換する

Next, you want to convert the storeType column to columns representing each `storeType`. There are 3 store types, (`A`, `B`, `C`), from which you are creating 3 new columns. The value set in each is a boolean value where a &#39;1&#39; is set depending on what the `storeType` was and `0` for the other 2 columns.

![](./images/walkthrough/storeType.png)

現在の `storeType` 列は削除されます。

#### isHoliday を数値型に変換する

次の修正は、`isHoliday` ブール値を数値表現に変更することです。

![](./images/walkthrough/isHoliday.png)

#### 来週の売上を予測する

次に、データセットごとに、前回と将来の週別の売上を追加します。 これを行うには、をオフセットし `weeklySales`ます。 さらに、 `weeklySales` 差が計算されます。 これは、`weeklySales` で前の週の `weeklySales` 値を引くことでおこなわれます 。

![](./images/walkthrough/weekly_past_future.png)

Since you are offsetting the `weeklySales` data 45 datasets forwards and 45 datasets backwards to create new columns, the first and last 45 data points have NaN values. You can remove these points from your dataset by using the `df.dropna()` function which removes all rows that have NaN values.

![](./images/walkthrough/dropna.png)

変更後のデータセットの概要を以下に示します。

![](./images/walkthrough/df_info_new.png)

### トレーニングと検証 {#training-and-verification}

次に、データの一部のモデルを作成し、将来の売上を予測するのに最もパフォーマンスの高いモデルを選択します。以下の5つのアルゴリズムを評価します。

- 線形回帰
- 決定木
- ランダムフォレスト
- 勾配ブースティング
- K 近傍法

#### データセットのトレーニングおよびテストサブセットへの分割

モデルがどの程度正確に値を予測できるかを知る方法が必要です。 この評価は、検証として使用するデータセットの一部を割り当て、残りをトレーニングデータとして割り当てることでおこなうことができます。Since `weeklySalesAhead` is the actual future values of `weeklySales`, you can use this to evaluate how accurate the model is at predicting the value. 分割は次の手順でおこないます。

![](./images/walkthrough/split_data.png)

You now have `X_train` and `y_train` for preparing the models and `X_test` and `y_test` for evaluation later.

#### スポットチェックアルゴリズム

In this section, you declare all the algorithms into an array called `model`. Next, you iterate through this array and for each algorithm, input your training data with `model.fit()` which creates a model `mdl`. このモデルを使って、自分の `weeklySalesAhead``X_test` データで予測できます。

![](./images/walkthrough/training_scoring.png)

For the scoring, you are taking the mean percentage difference between the predicted `weeklySalesAhead` with the actual values in the `y_test` data. 予測と実際の結果との差を最小限に抑えたいので、グラデーション昇圧回帰は最もパフォーマンスの高いモデルです。

#### 予測の視覚化

最後に、予測モデルと実際の週別売上高値を視覚化します。 青い線は実際の数を表し、緑はグラデーションブーストを使用した予測を表します。 次のコードは、データセット内の45のストアのうち6つを表す6つのプロットを生成します。 ここでは `Store 1` のみが表示されます。

![](./images/walkthrough/visualize_prediction.png)

## 次の手順

このドキュメントは、小売売上の問題を解決するための一般的なデータサイエンティストワークフローをカバーしました。 まとめると、

- ワークフローに必要なライブラリを読み込みます。
- ライブラリの読み込み後、統計概要、ビジュアライゼーション、グラフを使用して、開始でデータを確認できます。
- 次に、機能エンジニアリングを使用して小売データセットを変更します。
- 最後に、データのモデルを作成し、今後の売上を予測するのに最もパフォーマンスの高いモデルを選択します。

準備が整ったら、『 [JupterLab開始ガイド](./jupyterlab/overview.md) 』を読み、Adobe Experience Platform・データ・サイエンス・ワークスペースのノートブックの概要を簡単に確認します。 さらに、モデルとレシピの詳細について知りたい場合は、 [小売の販売スキーマとデータセット](./models-recipes/create-retails-sales-dataset.md) ・チュートリアルを読んで開始してください。 このチュートリアルでは、Data Science Workspaceのチュートリアルページで確認できる、以降のData Science Workspaceのチュートリアルについて準備 [します](../tutorials/data-science-workspace.md)。