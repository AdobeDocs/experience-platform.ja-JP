---
keywords: Experience Platform;home;popular topics;dsw;DSW
solution: Experience Platform
title: Data Science Workspace のチュートリアル
topic: tutorial
description: Adobe Experience Platformデータサイエンスワークスペースでは、機械学習と人工知能を使用して、データからインサイトを作成します。 Adobe Experience Platform に統合された Data Science Workspace は、アドビソリューションでコンテンツやデータアセットを使用して予測をおこなうことを支援します。
translation-type: tm+mt
source-git-commit: 10c9ce66b0fb3b5d1be931f37d95d283673bef15
workflow-type: tm+mt
source-wordcount: '1297'
ht-degree: 18%

---


# [!DNL Data Science Workspace] チュートリアル

Adobe Experience Platform [!DNL Data Science Workspace] uses machine learning and artificial intelligence to create insights from your data. Integrated into Adobe Experience Platform, [!DNL Data Science Workspace] helps you make predictions using your content and data assets across Adobe solutions. あらゆるスキルレベルのデータサイエンティストは、マシンラーニングレシピの迅速な開発、トレーニング、チューニングをサポートする高度で使いやすいツールを活用できるほか、複雑な操作が不要な AI テクノロジーのすべてのメリットを享受できます。

詳細につては、最初に「[Data Science Workspace の概要](../data-science-workspace/home.md)」を参照してください。

## [!DNL Sensei Machine Learning] API

The [!DNL Sensei Machine Learning] API provides a mechanism for data scientists to organize and manage machine learning services, from algorithm onboarding through experimentation and to service deployment.

**以下のAPI開発ガイドを利用できます。**
- [エンジン](../data-science-workspace/api/engines.md) — レジストリの検索、 [!DNL Docker] エンジンの作成、機能パイプラインエンジンの作成、エンジンの情報の取得、エンジンの更新、およびエンジンの削除の方法について説明します。
- [MLInstances（レシピ）](../data-science-workspace/api/mlinstances.md) - MLInstanceの作成、MLInstanceの情報の取得、MLInstanceの更新、MLInstanceの削除の方法について説明します。
- [実験](../data-science-workspace/api/experiments.md) — テストの作成、テストまたはテストの実行、テストの更新、テストの削除の方法を学びます。
- [モデル](../data-science-workspace/api/models.md) — 独自のモデルの登録、モデルの情報の取得、モデルの更新、モデルの削除、モデルの新しいトランスコードの作成、トランスコードされたモデルの詳細の取得の方法を学習します。
- [MLServices](../data-science-workspace/api/mlservices.md) - MLServiceの作成、MLServiceの情報の取得、MLServiceの更新、MLServiceの削除の方法を学びます。
- [インサイト](../data-science-workspace/api/insights.md) — インサイトの情報を取得する方法、新しいモデルインサイトを追加する方法、アルゴリズムのデフォルト指標のリストを取得する方法について説明します。

Senesi Machine Learning APIを使用してCRUD操作を実行する際に必要な値の詳細と入手方法については、はじ [めにガイドを参照してください](../data-science-workspace/api/getting-started.md)。

## How to use [!DNL JupyterLab] Notebooks

[!DNL JupyterLab] は、のWebベースのユーザーインターフェイスで、Adobe Experience Platform [!DNL Project Jupyter] と緊密に統合されています。 It provides an interactive development environment for data scientists to work with [!DNL Jupyter notebooks], code, and data. This document provides an overview of [!DNL JupyterLab] and its features as well as instructions to perform common actions.

**このガイドは次の目的に役立ちます。**
- インター [!DNL JupyterLab] フェイスにアクセスし、理解します。
- コードセルと内の利用可能なカーネルを理解し [!DNL JupyterLab]ます。
- GPUとメモリサーバーの設定を [!DNL Python]/Rで理解します。

詳細については、 [JupyterLabユーザーガイドを参照してください](../data-science-workspace/jupyterlab/overview.md)。

## JupterLabノートブックでのデータアクセス

現在、Data Science WorkspaceのJupyterLabは、 [!DNL Python]、R、PySpark、およびScala用のノートブックをサポートしています。 サポートされる各カーネルは、ノートブック内のデータセットから Platform データを読み取るための組み込み機能を備えてます。However, support for paginating data is limited to [!DNL Python] and R notebooks. このガイドでは、JupyterLabノートブックを使用してデータにアクセスする方法について説明します。

**このガイドは次の目的に役立ちます。**
- Python、R、PySpark、またはScalaノートブックを使用して、Platformのデータを読み取り、書き込み、クエリします。
- 各ノートブックタイプの読み取り制限事項を理解します。

詳細については、 [JupterLab Notebookデータアクセス開発者ガイドを参照してください](../data-science-workspace/jupyterlab/access-notebook-data.md)

## レシピオーサリング用のパッケージ [!DNL Docker] ソースファイル

画像を使用すると、アプリケーションを必要なすべての部分と共にパッケージ化できます。 [!DNL Docker] これには、1つのパッケージ内のライブラリと他の依存関係も含まれます。 作成された [!DNL Docker][!DNL Azure Container Registry] 画像は、レシピ作成ワークフローで指定された資格情報を使用して、ユーザーにプッシュされます。

**このチュートリアルは、次の作業を行う場合に役立ちます。**
- レシピ作成に必要な前提条件をダウンロードします。
- モデルオーサリング [!DNL Docker] に関する知識
- R、PySpark [!DNL Docker] 、Scala ( [!DNL Python][!DNL Spark])用のイメージを作成します。
- ソー [!DNL Docker] スファイルのURLを取得します。

詳しくは、 [パッケージソースファイルをレシピチュートリアルに従ってください](../data-science-workspace/models-recipes/package-source-files-recipe.md)。

## レシピの読み込み

>[!NOTE]
>
>このチュートリアルでは、 [!DNL Docker] ソースファイルのURLが必要です。 ソースファイルの [URLがない場合は、](../data-science-workspace/models-recipes/package-source-files-recipe.md) パッケージソースファイルをレシピチュートリアル [!DNL Docker] にアクセスしてください。

読み込みレシピのチュートリアルでは、パッケージ化されたレシピを設定および読み込む方法に関するインサイトを提供します。 By the end of this tutorial, you can create, train, and evaluate a Model in Adobe Experience Platform [!DNL Data Science Workspace].

**このチュートリアルは、次の作業を行う場合に役立ちます。**
- レシピ用の設定のセットを作成します。
- 、 [!DNL Docker] R、PySpark、またはScala ( [!DNL Python][!DNL Spark])用のベースのレシピを読み込みます。

詳しくは、パッケージ化されたレシピの [UIチュートリアル](../data-science-workspace/models-recipes/import-packaged-recipe-ui.md) または [APIのチュートリアルに従ってください](../data-science-workspace/models-recipes/import-packaged-recipe-api.md)。

## モデルのトレーニングと評価

In Adobe Experience Platform [!DNL Data Science Workspace], a machine learning Model is created by incorporating an existing Recipe that is appropriate for the Model&#39;s intent. 次に、モデルに関連するハイパーパラメーターを微調整することで、モデルの動作効率と有効性を最適化するようにトレーニングおよび評価します。レシピは再利用可能で、複数のモデルを作成し、単一のレシピで特定の目的に合わせてカスタマイズできます。

**このチュートリアルは、次の作業を行う場合に役立ちます。**
- 新しいモデルを作成します。
- モデルのトレーニング実行を作成します。
- モデルトレーニングの実行を評価します。

開始するには、トレーニングに従ってモデル [APIのチュートリアル](../data-science-workspace/models-recipes/train-evaluate-model-api.md) 、または [UIのチュートリアルに従ってください](../data-science-workspace/models-recipes/train-evaluate-model-ui.md)。

## モデルインサイトフレームワークを使用したモデルの最適化

The Model Insights Framework provides the data scientist with tools in Adobe Experience Platform [!DNL Data Science Workspace] to make quick and informed choices for optimal machine learning models based on experiments. このフレームワークにより、機械学習ワークフローの速度と効果だけでなく、データサイエンティストによる使いやすさも向上します。これは、モデルの調整を支援するために、機械学習アルゴリズムのタイプごとにデフォルトのテンプレートを提供することによっておこなわれます。結果的に、データサイエンティストと市民データサイエンティストは、エンドカスタマーに対してより適切なモデル最適化の決定をおこなうことができます。

**このチュートリアルは、次の作業を行う場合に役立ちます。**
- レシピコードを設定します。
- カスタム指標を定義します。
- 事前に作成された評価指標とビジュアライゼーショングラフを使用します。

開始するには、モデルの [最適化に関するチュートリアルに従ってください](../data-science-workspace/models-recipes/optimize-model.md)。

## モデルにスコアを付ける

Scoring in Adobe Experience Platform [!DNL Data Science Workspace] can be achieved by feeding input data into an existing trained Model. 次に、スコアリング結果が保存され、新しいバッチとして指定した出力データセットで表示可能になります。

**このチュートリアルは、次の作業を行う場合に役立ちます。**
- 新しいスコアリングの実行の作成.
- スコアリング結果の表示。

To get started, follow the score a model [API tutorial](../data-science-workspace/models-recipes/score-model-api.md) or the [UI tutorial](../data-science-workspace/models-recipes/score-model-ui.md).

## サービスとしてのモデルの公開

Adobe Experience Platform [!DNL Data Science Workspace] allows you to publish your Model as a service, enabling users within your IMS Organization to score data without the need for creating their own Models. This can be done using the [!DNL Platform] user interface or the [!DNL Sensei Machine Learning] API.

**このチュートリアルは、次の作業を行う場合に役立ちます。**
- モデルをサービスとして発行します。
- サービスギャラリーを介したサービスを使用した [!DNL Platform] スコア [!UICONTROL データ]。

開始するには、モデルをサービスとして公開するための [API のチュートリアル](../data-science-workspace/models-recipes/publish-model-service-api.md)または [UI のチュートリアル](../data-science-workspace/models-recipes/publish-model-service-ui.md)に従ってください。

## モデルのトレーニングとスコアリングのスケジュール設定

Adobe Experience Platform [!DNL Data Science Workspace] allows you to set up scheduled scoring and training runs on a machine learning service. トレーニングとスコアリングプロセスを自動化すると、データ内のパターンに対応し、時間をかけてサービスの効率性を維持、向上させるのに役立ちます。

**このチュートリアルは、次の作業を行う場合に役立ちます。**
- スコアリングののスケジュール設定
- スケジュール済みトレーニングの設定

開始するには、モデルUIのチュートリアル [のスケジュールに従い](../data-science-workspace/models-recipes/schedule-models-ui.md)ます。

## フィーチャパイプラインの作成

>[!NOTE]
>
>現在、機能のパイプラインはAPIを介してのみ使用できます。

Adobe Experience Platformでは、カスタムフィーチャパイプラインを作成して作成し、を尺度に基づいてフィーチャエンジニアリングを実行で [!DNL Sensei Machine Learning Framework Runtime]きます。

**このガイドは次の目的に役立ちます。**
- フィーチャパイプラインクラスを実装します。
- APIを使用してフィーチャーパイプラインエンジンを作成します。

詳細については、フィーチャーパイプラインの [作成に関するチュートリアルを参照してください](../data-science-workspace/authoring/feature-pipeline.md)。

## アプ [!DNL Real-Time Machine Learning] リの作成（アルファ）

ハブとレスポンシブの両方でシームレスな計算を組み合わせることで、従来、関連性が高く応答性の高いパーソナライズされたエクスペリエンスの電源投入に関与していた待ち時間が大幅に削減されます。 [!DNL Edge] したがって、同期的な意思決定に対する遅延が非常に低いという推測が [!DNL Real-time Machine Learning] 提供されます。 例としては、パーソナライズされたWebページコンテンツのレンダリング、オファーの表示、Webストアでのコンバージョンを増やしながら変化を減らす割引などがあります。

**このガイドは次の目的に役立ちます。**
- アーキテクチャを理解し [!DNL Real-time Machine Learning] ます。
- ワークフローを理解し [!DNL Real-time Machine Learning] ます。
- の現在の機能を理解し [!DNL Real-time Machine Learning]ます。
- 独自のを作成するための次の手順を実行し [!DNL Real-time Machine Learning model]ます。

詳しくは、 [リアルタイム機械学習の概要を参照してください](../data-science-workspace/real-time-machine-learning/home.md)。