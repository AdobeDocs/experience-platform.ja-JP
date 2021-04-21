---
keywords: Experience Platform；開発者ガイド；Data Science Workspace；よく読まれるトピック；リアルタイムの機械学習；
solution: Experience Platform
title: リアルタイム機械学習の手引き
topic-legacy: Getting started
description: 次のドキュメントでは、Adobe Experience Platformでリアルタイム機械学習モデルを作成するために必要な手順について説明します。
exl-id: 90a1c580-f6e7-4517-aa1e-da5092fbc4a2
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 1%

---

# Real-time Machine Learning(Alpha)の概要

>[!IMPORTANT]
>
>リアルタイム機械学習は、まだすべてのユーザーが利用できるわけではありません。 この機能はアルファベットで、まだテスト中です。 このドキュメントは変更される可能性があります。

リアルタイム機械学習を利用するには、Adobe Experience Platformと[!DNL Data Science Workspace]のプロビジョニングを受けた組織にアクセスできる必要があります。 さらに、トレーニングとスコアリングに使用する完全なデータセットが必要です。

Real-time Machine Learningのガイドでは、Python 3、[Jupyter Notebooks](../jupyterlab/overview.md)、データサイエンス、および機械学習に関する作業を理解する必要があります。

**キーワード:**

- **DSL:** ドメイン固有の言語。
- **エッジ：** リアルタイムの機械学習スコアリングサービスは、アクティベーションやアプリケーションに近いエッジクラスターで実行できます。
- **ハブ：** 現在のアルファは、Experience Edge Networkの開発中に、Adobe Experience Platformハブでリアルタイム機械学習スコアリングサービスを実行しています。
- **ノード：** ノードは、グラフを形成する基本単位です。各ノードは特定のタスクを実行し、リンクを使用してMLパイプラインを表すグラフを形成して、互いにチェーンできます。 タスクは、データやスキーマの変換、機械学習推論などの入力データに対する操作を表す。 ノードは、変換された値または推定された値を次のノードに出力します。

## Adobe Experience Platformのデータセット

Real-time Machine Learningを使用した開始を行うには、データセットにアクセスできる必要があります。 外部データセットを使用して[!DNL JupyterLab]環境にアップロードするか、まだ作成していない場合は、Platform内で新しいデータセットを作成するかを選択できます。

>[!NOTE]
>
>使用したいデータセットが既にある場合は、[次の手順](#next-steps)に進みます。

### 外部データセットの使用

データを[!DNL JupyterLab]環境にアップロードするなど、外部データセットの使用について詳しくは、[ノートブック](../jupyterlab/analyze-your-data.md#external-data)を使用したデータの分析のチュートリアルを参照してください。

### 新しいデータセットの作成

リアルタイム機械学習で使用する新しいデータセットを作成するには、データセットのデータスキーマが必要です。 次に、作成したスキーマを使用してデータを取り込む必要があります。 次のチュートリアルを使用して、[!DNL Platform]のデータセットを作成し、設定します。

- [APIでのデータセットの作成と設定](../../catalog/datasets/create.md)
- [UIでのデータセットの作成と設定](../../ingestion/tutorials/ingest-batch-data.md)

## 次の手順 {#next-steps}

リアルタイム機械学習用のデータを準備したら、『[リアルタイム機械学習ノートブック開始](./rtml-authoring-notebook.md)』に従って、ONNXモデルの作成方法とReal-time Machine Learningモデルストアへのアップロード方法を学習します。
