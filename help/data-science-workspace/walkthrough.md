---
keywords: Experience Platform；ウォークスルー；Data Science Workspace；よく読まれるトピック
solution: Experience Platform
title: Data Science Workspaceのチュートリアル
topic-legacy: Walkthrough
description: このドキュメントでは、Adobe Experience Platform Data Science Workspace　のチュートリアルを提供します。特に、データサイエンティストが通る一般的なワークフローは、機械学習を使用した問題の解決に役立ちます。
exl-id: d814846e-52a9-46c6-831a-3399241959f2
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1716'
ht-degree: 33%

---

# [!DNL Data Science Workspace] walkthrough

このドキュメントでは、Adobe Experience Platform[!DNL Data Science Workspace]のチュートリアルを提供します。 このチュートリアルでは、一般的なデータサイエンティストのワークフローと、機械学習を使用した問題へのアプローチ方法および解決方法について説明します。

## 前提条件

- 登録済みの Adobe ID アカウント
   - Adobe IDアカウントは、Adobe Experience Platformと[!DNL Data Science Workspace]にアクセスできる組織に追加されている必要があります。

## 小売の使用例

現在の市場での競争力を維持するために、ある小売業者は多くの課題に直面しています。小売業者の主な懸念の1つは、商品の最適価格を決定し、販売傾向を予測することです。 正確な予測モデルを使用すると、小売業者は、需要と価格設定ポリシーの関係を見つけ、販売と売上高を最大化するために最適化された価格決定を行うことができます。

## データサイエンティストの解決法

データサイエンティストのソリューションは、小売業者が提供する豊富な履歴情報を活用して、将来の傾向を予測し、価格決定を最適化することです。 このチュートリアルでは、過去の販売データを使用して機械学習モデルをトレーニングし、モデルを使用して将来の販売傾向を予測します。 これにより、最適な価格の変更に役立つインサイトを生成できます。

この概要は、データサイエンティストがデータセットを取り込み、週別の売上を予測するモデルを作成する手順を反映しています。 このチュートリアルでは、Adobe Experience Platform[!DNL Data Science Workspace]のサンプル小売売上ノートブックの以下の節について説明します。

- [セットアップ](#setup)
- [データの調査](#exploring-data)
- [特徴エンジニアリング](#feature-engineering)
- [トレーニングと検証](#training-and-verification)

### [!DNL Data Science Workspace]のノートブック

Adobe Experience PlatformのUIで、「**[!UICONTROL データサイエンス]**」タブから「**[!UICONTROL ノートブック]**」を選択して、[!UICONTROL ノートブック]の概要ページに移動します。 このページから「[!DNL JupyterLab]」タブを選択して[!DNL JupyterLab]環境を起動します。 [!DNL JupyterLab]のデフォルトのランディングページは&#x200B;**[!UICONTROL ランチャー]**&#x200B;です。

![](./images/walkthrough/notebooks.png)

![](./images/walkthrough/jupyterlab_launcher.png)

このチュートリアルでは、[!DNL JupyterLab Notebooks]の[!DNL Python] 3を使用して、データにアクセスし、調査する方法を示します。 ランチャーページには、サンプルのノートブックが用意されています。**[!UICONTROL 小売売上]**&#x200B;サンプルノートブックは、次に示す例で使用されています。

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

ライブラリの読み込み後、開始がデータを確認できます。 次の[!DNL Python]コードは、pandas&#39; `DataFrame`データ構造と[read_csv()](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html#pandas.read_csv)関数を使用して、[!DNL Github]でホストされているCSVをpandas DataFrameに読み込みます。

![](./images/walkthrough/read_csv.png)

Pandas の DataFrame データ構造は、2 次元のラベル付きデータ構造です。データの次元をすばやく確認するには、`df.shape`を使用します。 DataFrame の次元を表すタプルが返されます。

![](./images/walkthrough/df_shape.png)

最後に、データの外観をプレビューできます。 `df.head(n)`を使用して、DataFrameの最初の`n`行を表示できます。

![](./images/walkthrough/df_head.png)

#### 統計概要

[!DNL Python's] pandasライブラリを利用して、各属性のデータタイプを取得できます。 以下の呼び出しの出力では、各列のエントリ数とデータタイプに関する情報が得られます。

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

`store`の最小値と最大値を調べると、45の一意のストアが表していることがわかります。 店舗の種類を識別する `storeTypes` もあります。`storeTypes`の配布は、次のようにして確認できます。

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

### 特徴エンジニアリング  {#feature-engineering}

この節では、以下の操作を実行して小売データセットを変更する際に、機能エンジニアリングを使用します。

- 週と年の列を足す
- storeTypeをインジケーター変数に変換
- isHolidayを数値変数に変換します
- 来週の売上を予測する

#### 週と年の列を足す

現在の日付形式(`2010-02-05`)では、毎週のデータを区別するのが難しくなります。 このため、日付には週と年を含めるように変換する必要があります。

![](./images/walkthrough/date_to_week_year.png)

週と日付は次のようになります。

![](./images/walkthrough/date_week_year.png)

#### storeType をインジケーター変数に変換する

次に、storeType列を各`storeType`を表す列に変換します。 ストアには3つのタイプ(`A`、`B`、`C`)があり、ここで3つの新しい列を作成します。 それぞれに設定される値はboolean値で、他の2列の`storeType`と`0`に応じて&#39;1&#39;が設定されます。

![](./images/walkthrough/storeType.png)

現在の`storeType`列は削除されます。

#### isHoliday を数値型に変換する

次の修正は、`isHoliday` ブール値を数値表現に変更することです。

![](./images/walkthrough/isHoliday.png)

#### 来週の売上を予測する

次に、データセットごとに、前回と将来の週別の売上を追加します。 `weeklySales`をオフセットすると、これを行うことができます。 また、`weeklySales`の差も計算されます。 これは、`weeklySales` で前の週の `weeklySales` 値を引くことでおこなわれます 。

![](./images/walkthrough/weekly_past_future.png)

`weeklySales`データ45データセットをオフセットして、45データセットを後ろに戻して新しい列を作成するので、最初と最後の45データポイントにはNaN値が割り当てられます。 これらのポイントをデータセットから削除するには、`df.dropna()`関数を使用し、NaN値を持つすべての行を削除します。

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

モデルがどの程度正確に値を予測できるかを知る方法が必要です。 この評価は、検証として使用するデータセットの一部を割り当て、残りをトレーニングデータとして割り当てることでおこなうことができます。`weeklySalesAhead`は`weeklySales`の実際の未来値なので、この値を使用して、モデルが値を予測する際の正確さを評価できます。 分割は次の手順でおこないます。

![](./images/walkthrough/split_data.png)

これで、モデルを準備する`X_train`と`y_train`、後で評価する`X_test`と`y_test`ができます。

#### スポットチェックアルゴリズム

この節では、すべてのアルゴリズムを`model`という配列に宣言します。 次に、この配列を繰り返し処理し、各アルゴリズムに対して、トレーニングデータを`model.fit()`で入力します。これにより、モデル`mdl`が作成されます。 このモデルを使って、`weeklySalesAhead`を`X_test`データで予測できます。

![](./images/walkthrough/training_scoring.png)

スコアリングの場合、予測された`weeklySalesAhead`と`y_test`データの実際の値との間の平均差率を計算します。 予測と実際の結果との差を最小限に抑えたいので、グラデーション昇圧回帰は最もパフォーマンスの高いモデルです。

#### 予測の視覚化

最後に、予測モデルと実際の週別売上高値を視覚化します。 青い線は実際の数を表し、緑はグラデーションブーストを使用した予測を表します。 次のコードは、データセット内の45のストアのうち6つを表す6つのプロットを生成します。 ここでは `Store 1` のみが表示されます。

![](./images/walkthrough/visualize_prediction.png)

## 次の手順

このドキュメントは、小売売上の問題を解決するための一般的なデータサイエンティストワークフローをカバーしました。 まとめると、

- ワークフローに必要なライブラリを読み込みます。
- ライブラリの読み込み後、統計概要、ビジュアライゼーション、グラフを使用して、開始でデータを確認できます。
- 次に、機能エンジニアリングを使用して小売データセットを変更します。
- 最後に、データのモデルを作成し、今後の売上を予測するのに最もパフォーマンスの高いモデルを選択します。

準備が整ったら、『JupterLab user guide](./jupyterlab/overview.md)』を読み、Adobe Experience Platform・データ・サイエンス・ワークスペースのノートブックの概要を簡単に確認して、開始を行ってください。 [さらに、モデルとレシピの詳細について知りたい場合は、[小売売上スキーマとデータセット](./models-recipes/create-retails-sales-dataset.md)のチュートリアルを読んで開始してください。 このチュートリアルでは、Data Science Workspaceのチュートリアル（Data Science Workspaceの[チュートリアルページ](../tutorials/data-science-workspace.md)で参照可能）の準備をします。
