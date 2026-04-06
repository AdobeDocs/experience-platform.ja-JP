---
title: 機能エンジニアリングのSQL拡張機能
description: 高度な統計モデリングのためにデータを前処理するData Distiller機能エンジニアリング SQL拡張機能について説明します。 使用可能なフィーチャの抽出、変換、選択方法について説明します。
role: Developer
exl-id: 622c8ef3-9651-46b3-ad22-021a93190149
source-git-commit: e4ee4accdb28dafda7e37625eb84062bb6e53644
workflow-type: tm+mt
source-wordcount: '735'
ht-degree: 1%

---

# 機能エンジニアリング SQL拡張機能

>[!AVAILABILITY]
>
>この機能は、Data Distiller アドオンを購入したお客様が利用できます。 詳しくは、アドビ担当者にお問い合わせください。

機能エンジニアリングのニーズを満たすために、SQL トランスフォーマ拡張機能を使用してデータ前処理を簡素化および自動化します。 この拡張機能を使用して機能を構築し、モデルとの関連付けなど、様々な機能エンジニアリング手法をシームレスに試すことができます。 分散コンピューティング向けに設計されており、大規模なデータセットに対して並列かつスケーラブルな方法で機能エンジニアリングを実行でき、Data Distiller機能エンジニアリング SQL拡張機能を使用してデータ前処理に必要な時間を大幅に短縮できます。

## テクニックの概要 {#technique-overview}

機能エンジニアリング機能は、機能抽出、機能変換、機能選択の3つの主要な領域をカバーしています。 各領域には、データの前処理を抽出、変換、フォーカス、改善するために設計された特定の機能が含まれています。

### 特徴抽出 {#feature-extraction}

データ、特にテキストデータから関連情報を抽出し、サポートされているモデルがデータセットを使用または変換して導き出すことができる数値形式に変換します。 特徴量の抽出を実行するには、次の関数を使用します。

- **[テキストトランスフォーマ](./feature-transformation.md#textual-transformations)**：テキストデータを数値の特徴に変換します。
- **[ベクターをカウント](./feature-transformation.md#countvectorizer)**: テキストドキュメントのコレクションをトークン数のベクターに変換します。
- **[N-gram](./feature-transformation.md#ngram)**: テキストデータからn-gram シーケンスを生成します。
- **[ストップワードリムーバー](./feature-transformation.md#stopwordsremover)**：重要な意味を持たない一般的な単語をフィルタリングします。
- **[TF-IDF](./feature-transformation.md#tf-idf)**: コーパスに対する文書内の単語の重要度を測定します。
- **[Tokenizer](./feature-transformation.md#tokenizer)**: テキストを個別の用語（単語）に分割します。
- **[Word2Vec](./feature-transformation.md#word2vec)**：単語を固定サイズのベクターにマッピングし、単語の埋め込みを作成します。

### 機能変換 {#feature-transformation}

フィーチャを抽出するだけでなく、次の一般的なトランスフォーマを使用して、高度な統計モデルと派生データセットのフィーチャを準備します。 スケーリング、正規化、エンコーディングを適用して、機能が同じスケールで、分布が似ていることを確認します。

#### 汎用トランスフォーマ

ここでは、データ前処理ワークフローを強化するために、さまざまなデータタイプを処理できるツールの一覧を示します。

- **[数値インピュター](./feature-transformation.md#numeric-imputer)**：数値列の欠落している値を、平均や中央値など、指定した値で入力します。
- **[文字列インピュター](./feature-transformation.md#string-imputer)**：欠落している文字列値を、列の最も頻繁な文字列など、指定された値に置き換えます。
- **[ベクターアセンブラー](./feature-transformation.md#vector-assembler)**：複数の列を1つのベクター列に結合して、機械学習モデル用のデータを準備します。
- **[ブール値インピュター](./feature-transformation.md#boolean-imputer)**：指定された値（`true`や`false`など）を使用して、不足しているブール値を入力します。

#### 数値トランスフォーマ

これらの手法を適用して、数値データを効果的に処理および拡張し、モデルのパフォーマンスを向上させます。

- **[Binarizer](./feature-transformation.md#binarizer)**：連続的な特徴を、しきい値に基づいてバイナリ値に変換します。
- **[Bucketizer](./feature-transformation.md#bucketizer)**：連続するフィーチャを個別のバケットにマッピングします。
- **[Min-Max Scaler](./feature-transformation.md#minmaxscaler)**：機能を指定された範囲（通常は[0、1]）に再スケールします。
- **[Max Abs Scaler](./feature-transformation.md#maxabsscaler)**：スパースを変更せずに[-1, 1]の範囲に機能を再スケールします。
- **[Normalizer](./feature-transformation.md#normalizer)**: ベクトルを単位法線に正規化します。
- **[分位数ディスクリタイザー](./feature-transformation.md#quantilediscretizer)**：連続的な特徴を分位数に連結してカテゴリ分けの特徴に変換します。
- **[標準スケーラー](./feature-transformation.md#standardscaler)**: フィーチャを正規化して、単位標準偏差を持たせたり、平均を0にしたりします。

#### カテゴリ変圧器

これらのトランスフォーマーを使用して、カテゴリデータを変換し、マシンラーニングモデルに適した形式にエンコードします。

- **[文字列インデクサー](./feature-transformation.md#stringindexer)**：カテゴリ文字列データを数値インデックスに変換します。
- **[1つのHot Encoder](./feature-transformation.md#onehotencoder)**：カテゴリ データをバイナリ ベクトルにマッピングします。

### 機能の選択 {#feature-selection}

次に、元のセットから最も重要な機能のサブセットを選択することに焦点を当てます。 このプロセスは、データの次元を下げるのに役立ち、モデルが処理しやすくなり、モデル全体のパフォーマンスが向上します。

<!-- 
Commented out as it 
## Supported machine learning algorithms {#supported-ml-algorithms}

Once you have preprocessed your data, use the feature engineering SQL extension to prepare your data for the following machine learning algorithms:

### Classification and regression {#classification-regression}

Use logical regression to predict categorical outcomes and linear regression to predict continuous values.

- **Logical Regression**: Use this for binary classification tasks.
- **Linear Regression**: Apply this algorithm for predicting continuous values.

### Clustering {#clustering}

Use a clustering algorithm to group data points into distinct clusters based on their similarities.

- **[`K-Means`](./feature-transformation.md#kmeans)**: Use `K-Means` for unsupervised learning tasks to partition data into a specified number of clusters, with each data point assigned to the cluster with the nearest mean. 
-->

## OPTIONS条項の導入 {#options-clause}

モデルを定義する場合、`OPTIONS`句を使用して、アルゴリズムとそのパラメーターを指定します。 最初に、`type` パラメーターを設定して、使用しているアルゴリズム（`K-Means`など）を示します。 次に、`OPTIONS`句の関連パラメーターをキーと値のペアとして定義して、モデルを微調整します。 特定のパラメーターをカスタマイズしない場合は、デフォルト設定が適用されます。 各パラメーターの関数とデフォルト値については、関連するドキュメントを参照してください。

### 次の手順

このドキュメントで概説した機能エンジニアリング手法を学習したら、[&#x200B; モデル &#x200B;](./models.md)のドキュメントに進みます。 設計した機能を使用して、信頼できるモデルの作成、トレーニング、管理のプロセスをガイドします。 モデルを作成したら、[高度な統計モデルの実装ドキュメントに進みます。](./implement-models/implement-models.md)をインストールします。このドキュメントは、クラスタリング、分類、回帰など、様々なモデリング手法の詳細なガイドにリンクする概要として機能します。 これらのドキュメントに従うことで、SQL ワークフロー内で様々な信頼できるモデルを設定および実装し、高度なデータ分析のためにモデルを最適化する方法を学習します。
