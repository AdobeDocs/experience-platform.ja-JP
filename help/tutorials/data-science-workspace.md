---
keywords: Experience Platform；ホーム；人気の高いトピック；dsw;DSW
solution: Experience Platform
title: Data Science Workspace のチュートリアル
topic-legacy: tutorial
type: Tutorial
description: Adobe Experience Platformデータサイエンスワークスペースでは、機械学習と人工知能を使用して、データからインサイトを作成します。 Adobe Experience Platform に統合された Data Science Workspace は、アドビソリューションでコンテンツやデータアセットを使用して予測をおこなうことを支援します。
exl-id: 7cfd71b1-584f-4588-bbcd-bc42a08a0bc0
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1304'
ht-degree: 18%

---

# [!DNL Data Science Workspace] チュートリアル

Adobe Experience Platform[!DNL Data Science Workspace]は、機械学習と人工知能を使用して、データから洞察を作成します。 [!DNL Data Science Workspace]は、Adobe Experience Platformに統合され、Adobeソリューション全体でコンテンツやデータを使用して予測を行うのに役立ちます。 あらゆるスキルレベルのデータサイエンティストは、マシンラーニングレシピの迅速な開発、トレーニング、チューニングをサポートする高度で使いやすいツールを活用できるほか、複雑な操作が不要な AI テクノロジーのすべてのメリットを享受できます。

詳細につては、最初に「[Data Science Workspace の概要](../data-science-workspace/home.md)」を参照してください。

## [!DNL Sensei Machine Learning] API

[!DNL Sensei Machine Learning] APIは、データ科学者が機械学習サービスを、アルゴリズムのオンボーディングから実験まで、およびサービスの展開まで、編成および管理するメカニズムを提供します。

**以下のAPI開発ガイドを利用できます。**
- [エンジン](../data-science-workspace/api/engines.md) - [!DNL Docker] レジストリの検索、エンジンの作成、機能パイプラインエンジンの作成、エンジンの情報の取得、エンジンの更新、およびエンジンの削除の方法を学びます。
- [MLInstances（レシピ）](../data-science-workspace/api/mlinstances.md) - MLInstanceの作成、MLInstanceの情報の取得、MLInstanceの更新、MLInstanceの削除の方法を説明します。
- [実験](../data-science-workspace/api/experiments.md)  — テストの作成、テストまたはテストの実行、テストの更新、テストの削除の方法を学びます。
- [モデル](../data-science-workspace/api/models.md)  — 独自のモデルの登録、モデルの情報の取得、モデルの更新、モデルの削除、モデルの新しいトランスコードの作成、トランスコードされたモデルの詳細の取得の方法を学習します。
- [MLServices](../data-science-workspace/api/mlservices.md) :MLServiceの作成、MLServiceの情報の取得、MLServiceの更新、MLServiceの削除の方法を学びます。
- [インサイト](../data-science-workspace/api/insights.md)  — インサイトの情報を取得する方法、新しいモデルインサイトを追加する方法、アルゴリズムのデフォルト指標のリストを取得する方法について説明します。

Sensei Machine Learning APIを使用してCRUD操作を実行する際に必要な値の詳細と入手方法については、[はじめに](../data-science-workspace/api/getting-started.md)を参照してください。

## [!DNL JupyterLab]ノートブックの使い方

[!DNL JupyterLab] は、のWebベースのユーザーインターフェイスで、Adobe Experience Platform [!DNL Project Jupyter] と緊密に統合されています。[!DNL Jupyter Notebooks]、コード、およびデータを扱うデータ科学者向けの対話型開発環境を提供します。 このドキュメントでは、[!DNL JupyterLab]とその機能の概要と、一般的な操作を実行する手順を説明します。

**このガイドは次の目的に役立ちます。**
- [!DNL JupyterLab]インターフェイスにアクセスし、理解します。
- コードセルと[!DNL JupyterLab]内の利用可能なカーネルを理解します。
- [!DNL Python]/RのGPUとメモリサーバーの設定について理解します。

詳細については、[JupyterLabユーザーガイド](../data-science-workspace/jupyterlab/overview.md)を参照してください。

## JupterLabノートブックでのデータアクセス

現在、Data Science WorkspaceのJupyterLabは、[!DNL Python]、R、PySpark、およびScala用のノートブックをサポートしています。 サポートされる各カーネルは、ノートブック内のデータセットから Platform データを読み取るための組み込み機能を備えてます。ただし、データのページ番号付けのサポートは[!DNL Python]とRノートブックに限定されます。 このガイドでは、JupyterLabノートブックを使用してデータにアクセスする方法について説明します。

**このガイドは次の目的に役立ちます。**
- Python、R、PySpark、またはScalaノートブックを使用して、Platformのデータを読み取り、書き込み、クエリします。
- 各ノートブックタイプの読み取り制限事項を理解します。

詳細については、[JupyterLabノートブックデータアクセス開発ガイド](../data-science-workspace/jupyterlab/access-notebook-data.md)を参照してください。

## [!DNL Docker]レシピオーサリング用のパッケージソースファイル

[!DNL Docker]イメージを使用すると、必要な部分をすべて含むアプリケーションをパッケージ化できます。 これには、1つのパッケージ内のライブラリと他の依存関係も含まれます。 作成された[!DNL Docker]画像は、レシピ作成ワークフローで提供された資格情報を使用して[!DNL Azure Container Registry]にプッシュされます。

**このチュートリアルは、次の作業を行う場合に役立ちます。**
- レシピ作成に必要な前提条件をダウンロードします。
- [!DNL Docker]ベースのモデルオーサリングについて理解します。
- [!DNL Python], R, PySpark, Scala ([!DNL Spark])用の[!DNL Docker]イメージを構築します。
- [!DNL Docker]ソースファイルのURLを取得します。

詳しくは、[パッケージソースファイルをレシピチュートリアル](../data-science-workspace/models-recipes/package-source-files-recipe.md)に含めてください。

## レシピの読み込み

>[!NOTE]
>
>このチュートリアルでは、[!DNL Docker]ソースファイルのURLが必要です。 [!DNL Docker]ソースファイルのURLがない場合は、[パッケージソースファイルをレシピチュートリアル](../data-science-workspace/models-recipes/package-source-files-recipe.md)にアクセスしてください。

読み込みレシピのチュートリアルでは、パッケージ化されたレシピを設定および読み込む方法に関するインサイトを提供します。 このチュートリアルを終えるまでに、Adobe Experience Platform[!DNL Data Science Workspace]でモデルを作成、トレーニング、評価することができます。

**このチュートリアルは、次の作業を行う場合に役立ちます。**
- レシピ用の設定のセットを作成します。
- [!DNL Python]、R、PySpark、Scala ([!DNL Spark])用の[!DNL Docker]ベースのレシピを読み込みます。

詳しくは、パッケージ化されたレシピの読み込み[UIチュートリアル](../data-science-workspace/models-recipes/import-packaged-recipe-ui.md)または[APIチュートリアル](../data-science-workspace/models-recipes/import-packaged-recipe-api.md)に従ってください。

## モデルのトレーニングと評価

Adobe Experience Platform[!DNL Data Science Workspace]では、機械学習モデルは、モデルの意図に合った既存のレシピを組み込むことで作成されます。 次に、モデルに関連するハイパーパラメーターを微調整することで、モデルの動作効率と有効性を最適化するようにトレーニングおよび評価します。レシピは再利用可能で、複数のモデルを作成し、単一のレシピで特定の目的に合わせてカスタマイズできます。

**このチュートリアルは、次の作業を行う場合に役立ちます。**
- 新しいモデルを作成します。
- モデルのトレーニング実行を作成します。
- モデルトレーニングの実行を評価します。

開始するには、トレーニングに従って、モデル[APIチュートリアル](../data-science-workspace/models-recipes/train-evaluate-model-api.md)または[UIチュートリアル](../data-science-workspace/models-recipes/train-evaluate-model-ui.md)を評価します。

## モデルインサイトフレームワークを使用したモデルの最適化

Model Insights Frameworkは、データサイエンティストにAdobe Experience Platform[!DNL Data Science Workspace]のツールを提供し、実験に基づく最適な機械学習モデルのための情報に基づく迅速な選択を行います。 このフレームワークにより、機械学習ワークフローの速度と効果だけでなく、データサイエンティストによる使いやすさも向上します。これは、モデルの調整を支援するために、機械学習アルゴリズムのタイプごとにデフォルトのテンプレートを提供することによっておこなわれます。結果的に、データサイエンティストと市民データサイエンティストは、エンドカスタマーに対してより適切なモデル最適化の決定をおこなうことができます。

**このチュートリアルは、次の作業を行う場合に役立ちます。**
- レシピコードを設定します。
- カスタム指標を定義します。
- 事前に作成された評価指標とビジュアライゼーショングラフを使用します。

開始するには、[モデル](../data-science-workspace/models-recipes/optimize-model.md)の最適化のチュートリアルに従ってください。

## モデルにスコアを付ける

Adobe Experience Platform[!DNL Data Science Workspace]でのスコアは、既存のトレーニングを受けたモデルに入力データを送ることで達成できます。 次に、スコアリング結果が保存され、新しいバッチとして指定した出力データセットで表示可能になります。

**このチュートリアルは、次の作業を行う場合に役立ちます。**
- 新しいスコアリングの実行の作成.
- スコアリング結果の表示。

開始するには、モデル[APIチュートリアル](../data-science-workspace/models-recipes/score-model-api.md)または[UIチュートリアル](../data-science-workspace/models-recipes/score-model-ui.md)のスコアに従ってください。

## サービスとしてのモデルの公開

Adobe Experience Platform[!DNL Data Science Workspace]を使用すると、モデルをサービスとして公開でき、IMS組織内のユーザーは、独自のモデルを作成することなくデータをスコアできます。 これは、[!DNL Platform]ユーザーインターフェイスまたは[!DNL Sensei Machine Learning] APIを使用して行うことができます。

**このチュートリアルは、次の作業を行う場合に役立ちます。**
- モデルをサービスとして発行します。
- [!DNL Platform] [!UICONTROL サービスギャラリー]を介して、サービスを使用したスコアデータ。

開始するには、モデルをサービスとして公開するための [API のチュートリアル](../data-science-workspace/models-recipes/publish-model-service-api.md)または [UI のチュートリアル](../data-science-workspace/models-recipes/publish-model-service-ui.md)に従ってください。

## モデルのトレーニングとスコアリングのスケジュール設定

Adobe Experience Platform[!DNL Data Science Workspace]では、機械学習サービスでのスケジュール済みスコアおよびトレーニングの実行を設定できます。 トレーニングとスコアリングプロセスを自動化すると、データ内のパターンに対応し、時間をかけてサービスの効率性を維持、向上させるのに役立ちます。

**このチュートリアルは、次の作業を行う場合に役立ちます。**
- スコアリングののスケジュール設定
- スケジュール済みトレーニングの設定

開始するには、[モデルUIチュートリアルのスケジュール](../data-science-workspace/models-recipes/schedule-models-ui.md)に従ってください。

## フィーチャパイプラインの作成

>[!NOTE]
>
>現在、機能のパイプラインはAPIを介してのみ使用できます。

Adobe Experience Platformでは、カスタムフィーチャパイプラインを作成して作成し、[!DNL Sensei Machine Learning Framework Runtime]を通して尺度でフィーチャエンジニアリングを実行できます。

**このガイドは次の目的に役立ちます。**
- フィーチャパイプラインクラスを実装します。
- APIを使用してフィーチャーパイプラインエンジンを作成します。

詳細については、[フィーチャパイプラインの作成](../data-science-workspace/authoring/feature-pipeline.md)のチュートリアルを参照してください。

## [!DNL Real-Time Machine Learning]アプリケーション（アルファ）を作成する

ハブと[!DNL Edge]の両方でシームレスな計算を組み合わせることで、従来、関連性が高く応答性の高いパーソナライズされたエクスペリエンスの電力供給に関与していた待ち時間が大幅に削減されます。 したがって、[!DNL Real-time Machine Learning]は、同期的な意思決定の遅延が非常に低い推論を提供します。 例としては、パーソナライズされたWebページコンテンツのレンダリング、オファーの表示、Webストアでのコンバージョンを増やしながら変化を減らす割引などがあります。

**このガイドは次の目的に役立ちます。**
- [!DNL Real-time Machine Learning]アーキテクチャを理解します。
- [!DNL Real-time Machine Learning]ワークフローを理解します。
- [!DNL Real-time Machine Learning]の現在の機能を理解します。
- 独自の[!DNL Real-time Machine Learning model]を作成する次の手順を説明します。

詳しくは、[リアルタイム機械学習の概要](../data-science-workspace/real-time-machine-learning/home.md)を参照してください。
