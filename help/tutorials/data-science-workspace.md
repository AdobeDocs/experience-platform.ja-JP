---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Data Science Workspaceのチュートリアル
topic: tutorial
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '1252'
ht-degree: 0%

---


# Data Science Workspaceのチュートリアル

Adobe Experience Platformデータサイエンスワークスペースでは、機械学習と人工知能を使用して、データからインサイトを作成します。 Data Science WorkspaceはAdobe Experience Platformに統合されており、アドビのソリューション全体でコンテンツとデータアセットを使用して予測を行うのに役立ちます。 すべてのスキルレベルのデータ科学者は、機械学習レシピの迅速な開発、トレーニング、調整をサポートする高度で使いやすいツールを備えています。AIテクノロジーのすべてのメリットを、複雑さを伴うことなくサポートします。

詳しくは、 [Data Science Workspaceの概要を読むことから始めてください](../data-science-workspace/home.md)。

## Senesie Machine Learning API

Senesie Machine Learning APIは、データ科学者が、アルゴリズムによる搭乗から実験まで、サービスの展開まで、機械学習サービスを整理、管理するメカニズムを提供します。

**以下のAPI開発ガイドを利用できます。**
- [エンジン](../data-science-workspace/api/engines.md) - Dockerレジストリの検索、エンジンの作成、機能パイプラインエンジンの作成、エンジンの情報の取得、エンジンの更新、エンジンの削除の方法を説明します。
- [MLInstances（レシピ）](../data-science-workspace/api/mlinstances.md) - MLInstanceの作成、MLInstanceの情報の取得、MLInstanceの更新、MLInstanceの削除の方法について説明します。
- [実験](../data-science-workspace/api/experiments.md) — テストの作成、テストまたはテストの実行、テストの更新、テストの削除の方法を学びます。
- [モデル](../data-science-workspace/api/models.md) — 独自のモデルの登録、モデルの情報の取得、モデルの更新、モデルの削除、モデルの新しいトランスコードの作成、トランスコードされたモデルの詳細の取得の方法を学習します。
- [MLServices](../data-science-workspace/api/mlservices.md) - MLServiceの作成、MLServiceの情報の取得、MLServiceの更新、MLServiceの削除の方法を学びます。
- [インサイト](../data-science-workspace/api/insights.md) — インサイトの情報を取得する方法、新しいモデルインサイトを追加する方法、アルゴリズムのデフォルト指標のリストを取得する方法について説明します。

Senesi Machine Learning APIを使用してCRUD操作を実行する際に必要な値の詳細と入手方法については、 [入門ガイドを参照してください](../data-science-workspace/api/getting-started.md)。

## JupyterLabノートブックの使用方法

[!DNL JupyterLab] は、Adobe Experience Platform用のWebベースのユーザーインターフェイスで [!DNL Project Jupyter] あり、緊密に統合されています。 これは、データ科学者がJupterのノート、コード、データを扱うための対話型の開発環境を提供します。 このドキュメントでは、とその機能の概要 [!DNL JupyterLab] と、一般的なアクションを実行する手順を説明します。

**このガイドは次の目的に役立ちます。**
- インター [!DNL JupyterLab] フェイスにアクセスし、理解します。
- コードセルと内の利用可能なカーネルを理解し [!DNL JupyterLab]ます。
- Python/RでのGPUとメモリサーバの設定について理解します。
- ノートブックを使用してデータを読み取り、クエリ [!DNL Platform] します。
- ノートブックのデータ制限を理解します。

詳細については、 [JupyterLabユーザーガイドを参照してください](../data-science-workspace/jupyterlab/overview.md)。

## Dockerレシピオーサリング用のパッケージソースファイル

Dockerイメージを使用すると、アプリケーションを必要なすべての部分と共にパッケージ化できます。 これには、1つのパッケージ内のライブラリと他の依存関係も含まれます。 作成されたDockerイメージは、レシピ作成ワークフローで指定された資格情報を使用してAzureコンテナレジストリにプッシュされます。

**このチュートリアルは、次の作業を行う場合に役立ちます。**
- レシピ作成に必要な前提条件をダウンロードします。
- Dockerベースのモデルオーサリングについて理解します。
- Python、R、PySpark、またはScala (Spark)用のDockerイメージを作成します。
- DockerソースファイルのURLを取得します。

詳しくは、 [パッケージソースファイルをレシピチュートリアルに従ってください](../data-science-workspace/models-recipes/package-source-files-recipe.md)。

## レシピの読み込み

>[!NOTE]
>
>
>このチュートリアルでは、DockerソースファイルのURLが必要です。 DockerソースファイルのURLがない場合は、 [パッケージソースファイルをレシピチュートリアルにアクセスし](../data-science-workspace/models-recipes/package-source-files-recipe.md) 、

読み込みレシピのチュートリアルでは、パッケージ化されたレシピを設定および読み込む方法に関するインサイトを提供します。 このチュートリアルを終了するまでに、Adobe Experience Platformデータサイエンスワークスペースでモデルを作成、トレーニング、評価することができます。

**このチュートリアルは、次の作業を行う場合に役立ちます。**
- レシピ用の設定のセットを作成します。
- Python、R、PySpark、またはScala (Spark)用のDockerベースのレシピを読み込みます。

詳しくは、パッケージ化されたレシピの [UIチュートリアル](../data-science-workspace/models-recipes/import-packaged-recipe-ui.md) または [APIのチュートリアルに従ってください](../data-science-workspace/models-recipes/import-packaged-recipe-api.md)。

## モデルのトレーニングと評価

Adobe Experience Platformデータサイエンスワークスペースでは、機械学習モデルは、モデルの意図に適した既存のレシピを組み込むことで作成されます。 次に、モデルのトレーニングと評価を行い、関連するハイパーパラメーターを微調整して、その動作効率と有効性を最適化します。 レシピは再利用可能です。つまり、複数のモデルを作成し、1つのレシピで特定の目的に合わせてカスタマイズできます。

**このチュートリアルは、次の作業を行う場合に役立ちます。**
- 新しいモデルを作成します。
- モデルのトレーニング実行を作成します。
- モデルトレーニングの実行を評価します。

開始するには、トレーニングに従ってモデル [APIのチュートリアル](../data-science-workspace/models-recipes/train-evaluate-model-api.md) 、または [UIのチュートリアルに従ってください](../data-science-workspace/models-recipes/train-evaluate-model-ui.md)。

## モデルインサイトフレームワークを使用したモデルの最適化

Model Insights Frameworkは、Adobe Experience Platformデータ科学ワークスペースのツールをデータサイエンティストに提供し、実験に基づく最適な機械学習モデルに関する情報に基づく迅速な選択を可能にします。 フレームワークは、機械学習ワークフローの速度と効果を向上させるとともに、データ科学者にとっての使いやすさを改善します。 これは、モデルの調整に役立つように、機械学習アルゴリズムのタイプごとにデフォルトのテンプレートを指定することで行います。 その結果、データ科学者と市民データ科学者は、エンドユーザーに対してより良いモデル最適化の判断を行うことができます。

**このチュートリアルは、次の作業を行う場合に役立ちます。**
- レシピコードを設定します。
- カスタム指標を定義します。
- 事前に作成された評価指標とビジュアライゼーショングラフを使用します。

開始するには、モデルの [最適化に関するチュートリアルに従ってください](../data-science-workspace/models-recipes/optimize-model.md)。

## モデルにスコアを付ける

Adobe Experience Platformデータサイエンスワークスペースでのスコアは、既存のトレーニングを受けたモデルに入力データをフィードすることで達成できます。 次に、スコアリング結果が新しいバッチとして指定した出力データセットに保存され、表示可能になります。

**このチュートリアルは、次の作業を行う場合に役立ちます。**
- 新しいスコアリング実行を作成します。
- スコアリング結果の表示。

開始するには、モデル [APIチュートリアル](../data-science-workspace/models-recipes/score-model-api.md) 、または [UIチュートリアルのスコアに従います](../data-science-workspace/models-recipes/score-model-ui.md)。

## モデルをサービスとして発行

Adobe Experience Platformデータサイエンスワークスペースを使用すると、モデルをサービスとして公開でき、IMS組織内のユーザーは、独自のモデルを作成することなく、データをスコアリングできます。 これは、ユー [!DNL Platform] ザーインターフェイスまたはSenesie Machine Learning APIを使用して行うことができます。

**このチュートリアルは、次の作業を行う場合に役立ちます。**
- モデルをサービスとして発行します。
- サービスギャラリーを介したサービスを使用したスコアデータ [!DNL Platform] です。

開始するには、「モデルをサービス [APIとして公開](../data-science-workspace/models-recipes/publish-model-service-api.md) 」チュートリアル [、または](../data-science-workspace/models-recipes/publish-model-service-ui.md)UIチュートリアルに従ってください。

## モデルのトレーニングとスコアリングのスケジュール設定

Adobe Experience Platformデータサイエンスワークスペースでは、機械学習サービスでのスケジュール済みスコアおよびトレーニングの実行を設定できます。 トレーニングとスコアリングプロセスを自動化すると、データ内のパターンに対応し、時間をかけてサービスの効率性を維持、向上させるのに役立ちます。

**このチュートリアルは、次の作業を行う場合に役立ちます。**
- スケジュール済みスコアの設定
- 予定されたトレーニングの設定

開始するには、モデルUIのチュートリアル [のスケジュールに従い](../data-science-workspace/models-recipes/schedule-models-ui.md)ます。

## フィーチャパイプラインの作成

>[!NOTE]
>現在、機能のパイプラインはAPIを介してのみ使用できます。

Adobe Experience Platformを使用すると、Senesi Machine Learning Framework Runtimeを介して、カスタムフィーチャパイプラインを作成し、スケールでフィーチャエンジニアリングを実行できます。

**このガイドは次の目的に役立ちます。**
- フィーチャパイプラインクラスを実装します。
- APIを使用してフィーチャーパイプラインエンジンを作成します。

詳細については、フィーチャーパイプラインの [作成に関するチュートリアルを参照してください](../data-science-workspace/authoring/feature-pipeline.md)。

## Real-Time Machine Learningアプリケーション（アルファ）の構築

ハブとレスポンシブの両方でシームレスな計算を組み合わせることで、従来、関連性が高く応答性の高いパーソナライズされたエクスペリエンスの電源投入に関与していた待ち時間が大幅に削減されます。 [!DNL Edge] したがって、リアルタイム機械学習では、同期的な意思決定に対する遅延が非常に低いという推測が提供されます。 例としては、パーソナライズされたWebページコンテンツのレンダリング、オファーの表示、Webストアでのコンバージョンを増やしながら変化を減らす割引などがあります。

**このガイドは次の目的に役立ちます。**
- リアルタイム機械学習アーキテクチャについて説明します。
- リアルタイム機械学習ワークフローについて説明します。
- リアルタイム機械学習の現在の機能を理解します。
- 独自のリアルタイム機械学習モデルを作成するための次の手順を説明します。

詳しくは、 [リアルタイム機械学習の概要を参照してください](../data-science-workspace/real-time-machine-learning/home.md)。