---
keywords: Experience Platform;developer guide;Data Science Workspace;popular topics;Real time machine learning;
solution: Experience Platform
title: リアルタイム機械学習の概要
topic: Getting started
translation-type: tm+mt
source-git-commit: 626bb7a0856a663e235ecd2b19954f4617fe9b6f
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 0%

---


# Real-time Machine Learning(Alpha)の概要

>[!IMPORTANT]
>リアルタイム機械学習は、まだすべてのユーザーが利用できるわけではありません。 この機能はアルファベットで、まだテスト中です。 このドキュメントは変更される可能性があります。

Real-time Machine Learningを利用するには、Adobe Experience PlatformとData Science Workspaceのプロビジョニングを受けた組織にアクセスできる必要があります。 さらに、トレーニングとスコアリングに使用する完全なデータセットが必要です。

Real-time Machine Learningのガイドでは、Python 3、 [Jupyterノートブック](../jupyterlab/overview.md)、データサイエンス、および機械学習に関する作業を理解する必要があります。

**キーワード:**

- **DSL:** ドメイン固有の言語を参照してください。
- **エッジ：** リアルタイムの機械学習スコアリングサービスは、アクティベーションやアプリケーションに近いエッジクラスターで実行できます。
- **ハブ：** 現在のアルファは、Experience Edge Networkの開発中に、Adobe Experience Platform HubでReal-time Machine Learning Scoringサービスを実行しています。
- **ノード：** 「ノード」は、グラフを形成する基本単位です。 各ノードは特定のタスクを実行し、リンクを使用してMLパイプラインを表すグラフを形成して、互いにチェーンできます。 タスクは、データやスキーマの変換、機械学習推論などの入力データに対する操作を表す。 ノードは、変換された値または推定された値を次のノードに出力します。

## Adobe Experience Platformのデータセット

Real-time Machine Learningを使用した開始を行うには、データセットにアクセスできる必要があります。 外部データセットを使用してJupterLab環境にアップロードするか、まだ作成していない場合は、プラットフォーム内で新しいデータセットを作成するかを選択できます。

>[!NOTE]
>使用するデータセットが既にある場合は、 [次の手順に進みます](#next-steps)。

### 外部データセットの使用

JupterLab環境へのデータのアップロードなど、外部データセットの使用方法の詳細については、ノートブックを使用したデータ [分析のチュートリアルを参照してください](../jupyterlab/analyze-your-data.md#external-data)。

### 新しいデータセットの作成

リアルタイム機械学習で使用する新しいデータセットを作成するには、データセットのデータスキーマが必要です。 次に、作成したスキーマを使用してデータを取り込む必要があります。 次のチュートリアルを使用して、プラットフォームのデータセットを作成し、設定します。

- [APIでのデータセットの作成と設定](../../catalog/datasets/create.md)
- [UIでのデータセットの作成と設定](../../ingestion/tutorials/ingest-batch-data.md)

## 次の手順 {#next-steps}

リアルタイム機械学習用のデータを準備したら、『 [Real-time Machine Learning notebook開始ガイド](./rtml-authoring-notebook.md) 』に従って、ONNXモデルの作成方法とReal-time Machine Learningモデルストアへのアップロード方法を学習します。

