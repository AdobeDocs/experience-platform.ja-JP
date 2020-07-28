---
keywords: Experience Platform;developer guide;Data Science Workspace;popular topics;Real time machine learning;
solution: Experience Platform
title: リアルタイム機械学習の概要
topic: Getting started
translation-type: tm+mt
source-git-commit: 1e5526b54f3c52b669f9f6a792eda0abfc711fdd
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 1%

---


# Real-time Machine Learning(Alpha)の概要

>[!IMPORTANT]
>リアルタイム機械学習は、まだすべてのユーザーが利用できるわけではありません。 この機能はアルファベットで、まだテスト中です。 このドキュメントは変更される可能性があります。

リアルタイム機械学習を利用するには、Adobe Experience Platformおよびを使用してプロビジョニングされた組織にアクセスする必要があり [!DNL Data Science Workspace]ます。 さらに、トレーニングとスコアリングに使用する完全なデータセットが必要です。

Real-time Machine Learningのガイドでは、Python 3、 [Jupyterノートブック](../jupyterlab/overview.md)、データサイエンス、および機械学習に関する作業を理解する必要があります。

**キーワード:**

- **DSL:** ドメイン固有の言語を参照してください。
- **エッジ：** リアルタイムの機械学習スコアリングサービスは、アクティベーションやアプリケーションに近いエッジクラスターで実行できます。
- **ハブ：** 現在のアルファは、Experience Edge Networkの開発中に、Adobe Experience Platformハブでリアルタイム機械学習スコアリングサービスを実行しています。
- **ノード：** 「ノード」は、グラフを形成する基本単位です。 各ノードは特定のタスクを実行し、リンクを使用してMLパイプラインを表すグラフを形成して、互いにチェーンできます。 タスクは、データやスキーマの変換、機械学習推論などの入力データに対する操作を表す。 ノードは、変換された値または推定された値を次のノードに出力します。

## Adobe Experience Platformのデータセット

Real-time Machine Learningを使用した開始を行うには、データセットにアクセスできる必要があります。 外部データセットを使用して [!DNL JupyterLab] 環境にアップロードするか、Platform内で新しいデータセットを作成するかを選択できます（まだ作成していない場合）。

>[!NOTE]
>使用するデータセットが既にある場合は、 [次の手順に進みます](#next-steps)。

### 外部データセットの使用

データを [!DNL JupyterLab] 環境にアップロードするなど、外部データセットの使用について詳しくは、ノートブックを使用したデータの [分析に関するチュートリアルを参照してください](../jupyterlab/analyze-your-data.md#external-data)。

### 新しいデータセットの作成

リアルタイム機械学習で使用する新しいデータセットを作成するには、データセットのデータスキーマが必要です。 次に、作成したスキーマを使用してデータを取り込む必要があります。 次のチュートリアルを使用して、のデータセットを作成し、設定し [!DNL Platform]ます。

- [APIでのデータセットの作成と設定](../../catalog/datasets/create.md)
- [UIでのデータセットの作成と設定](../../ingestion/tutorials/ingest-batch-data.md)

## 次の手順 {#next-steps}

リアルタイム機械学習用のデータを準備したら、『 [Real-time Machine Learning notebook開始ガイド](./rtml-authoring-notebook.md) 』に従って、ONNXモデルの作成方法とReal-time Machine Learningモデルストアへのアップロード方法を学習します。

