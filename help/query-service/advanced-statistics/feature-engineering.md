---
title: 機能エンジニアリング SQL 拡張機能
description: 高度な統計モデリング用にデータを前処理する Data Distiller機能エンジニアリング SQL 拡張機能について説明します。 使用可能な機能の抽出、変換、選択の手法について説明します。
role: Developer
exl-id: 622c8ef3-9651-46b3-ad22-021a93190149
source-git-commit: e7bc30c153f67c59e9c04e8c8df60394f48871d0
workflow-type: tm+mt
source-wordcount: '735'
ht-degree: 1%

---

# 機能エンジニアリング SQL 拡張機能

>[!AVAILABILITY]
>
>この機能は、Data Distiller アドオンを購入したお客様が利用できます。 詳しくは、アドビ担当者にお問い合わせください。

機能のエンジニアリングニーズを満たすには、SQL Transformer 拡張機能を使用して、データの前処理を簡素化および自動化します。 この拡張機能を使用して機能を構築し、モデルとの関連付けなど、様々な機能エンジニアリング手法を使用したシームレスな実験を楽しみます。 分散コンピューティング向けに設計された SQL 拡張機能を使用すると、大規模なデータセットに対する機能エンジニアリングを並行してスケーラブルに実行でき、Data Distillerの機能エンジニアリングによりデータの前処理に要する時間を大幅に短縮できます。

## 手法の概要 {#technique-overview}

フィーチャ エンジニアリング機能は、フィーチャ抽出、フィーチャ変換、フィーチャ選択の 3 つの主要な領域をカバーしています。 各領域には、データの前処理の抽出、変換、フォーカスおよび向上のために設計された特定の関数が含まれています。

### 機能の抽出 {#feature-extraction}

データ、特にテキストデータから関連情報を抽出し、サポートされているモデルが使用または変換してデータセットを取得できる数値形式に変換します。 次の関数を使用して、機能の抽出を実行します。

- **[テキストトランスフォーマー](./feature-transformation.md#textual-transformations)**：テキストデータを数値特性に変換します。
- **[Count Vectorizer](./feature-transformation.md#countvectorizer)**：テキストドキュメントのコレクションをトークンカウントのベクトルに変換します。
- **[N-gram](./feature-transformation.md#ngram)**：テキストデータから n-gram のシーケンスを生成します。
- **[ストップワード削除](./feature-transformation.md#stopwordsremover)**：重要な意味を持たない一般的な単語を除外します。
- **[TF-IDF](./feature-transformation.md#tf-idf)**: コーパスに対するドキュメント内の単語の重要度を測定します。
- **[Tokenizer](./feature-transformation.md#tokenizer)**：テキストを個別の用語（単語）に分類します。
- **[Word2Vec](./feature-transformation.md#word2vec)**：単語を固定サイズのベクトルにマッピングし、単語埋め込みを作成します。

### 機能変換 {#feature-transformation}

機能の抽出に加えて、以下の一般的なトランスフォーマーを使用して、高度な統計モデルと派生データセットの機能を準備します。 スケーリング、正規化、またはエンコーディングを適用して、フィーチャが同じ尺度で同じ分布を持つようにします。

#### 一般変圧器

以下は、データの前処理ワークフローを強化するために様々なデータタイプを処理するツールのリストです。

- **[数値入力](./feature-transformation.md#numeric-imputer)**：数値列の欠落値を、平均値や中央値などの指定した値で入力します。
- **[文字列演算子](./feature-transformation.md#string-imputer)**：欠落している文字列値を、列の最も頻度の高い文字列などの、指定した値に置き換えます。
- **[ベクトルアセンブラー](./feature-transformation.md#vector-assembler)**：複数の列を 1 つのベクトル列に組み合わせて、機械学習モデルのデータを準備します。
- **[ブール値の入力](./feature-transformation.md#boolean-imputer)**：欠落しているブール値を、`true` や `false` などの指定した値で入力します。

#### 数値変換

これらの手法を適用して、数値データを効果的に処理およびスケーリングし、モデルのパフォーマンスを向上させます。

- **[バイナライザ](./feature-transformation.md#binarizer)**：しきい値に基づいて、連続する特性をバイナリ値に変換します。
- **[バケット化](./feature-transformation.md#bucketizer)**：連続したフィーチャを個別のバケットにマッピングします。
- **[Min-Max スケーラー](./feature-transformation.md#minmaxscaler)**：指定された範囲（通常は [0、1] に機能を再スケールします。
- **[Max Abs Scaler](./feature-transformation.md#maxabsscaler)**：スパーシティを変更せずに、機能を [～1、1] の範囲に再スケールします。
- **[Normalizer](./feature-transformation.md#normalizer)**：ベクトルを正規化して単位ノルムを持たせます。
- **[Quantile Discretizer](./feature-transformation.md#quantilediscretizer)**：連続特性を量子化することにより、カテゴリ特性に変換します。
- **[標準スケーラー](./feature-transformation.md#standardscaler)**：機能を正規化して、単位標準偏差や平均値が 0 になるようにします。

#### 分類変圧器

これらのトランスフォーマーを使用すると、カテゴリデータを変換し、機械学習モデルに適した形式にエンコードできます。

- **[文字列インデクサー](./feature-transformation.md#stringindexer)**：カテゴリ文字列データを数値インデックスに変換します。
- **[One Hot Encoder](./feature-transformation.md#onehotencoder)**：カテゴリデータをバイナリベクトルにマッピングします。

### 機能の選択 {#feature-selection}

次に、元のセットから最も重要な機能のサブセットを選択することに焦点を当てます。 このプロセスにより、データのディメンションが減り、モデルの処理が容易になり、モデル全体のパフォーマンスが向上します。

<!-- Commented out as it 
## Supported machine learning algorithms {#supported-ml-algorithms}

Once you have preprocessed your data, use the feature engineering SQL extension to prepare your data for the following machine learning algorithms:

### Classification and regression {#classification-regression}

Use logical regression to predict categorical outcomes and linear regression to predict continuous values.

- **Logical Regression**: Use this for binary classification tasks.
- **Linear Regression**: Apply this algorithm for predicting continuous values.

### Clustering {#clustering}

Use a clustering algorithm to group data points into distinct clusters based on their similarities.

- **[`K-Means`](./feature-transformation.md#kmeans)**: Use `K-Means` for unsupervised learning tasks to partition data into a specified number of clusters, with each data point assigned to the cluster with the nearest mean. -->

## OPTIONS句を実装します {#options-clause}

モデルを定義する場合、`OPTIONS` 句を使用して、アルゴリズムとそのパラメーターを指定します。 最初に、使用しているアルゴリズムを示す `type` パラメーター（`K-Means` など）を設定します。 次に、`OPTIONS` 句で関連するパラメーターをキーと値のペアとして定義し、モデルを微調整します。 特定のパラメータをカスタマイズしない場合は、デフォルト設定が適用されます。 各パラメーターの関数とデフォルト値について詳しくは、関連するドキュメントを参照してください。

### 次の手順

このドキュメントで概要を説明する機能エンジニアリング手法を学習したら、[&#x200B; モデル &#x200B;](./models.md) ドキュメントに進みます。 エンジニアリングした機能を使用して、信頼できるモデルを作成、トレーニング、および管理するプロセスを順を追って説明します。 モデルが構築されたら、[&#x200B; 高度な統計モデルの実装」ドキュメントに進みます。](./implement-models/implement-models.md)。このドキュメントは、クラスタリング、分類、回帰など、様々なモデリング手法の詳細なガイドにリンクする概要として機能します。 これらのドキュメントでは、SQL ワークフロー内で様々な信頼済みモデルを設定および実装し、高度なデータ分析用にモデルを最適化する方法について説明します。
