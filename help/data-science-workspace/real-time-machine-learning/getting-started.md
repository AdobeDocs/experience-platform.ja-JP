---
keywords: Experience Platform、開発者ガイド、Data Science Workspace、人気の高いトピック、リアルタイムの機械学習、
solution: Experience Platform
title: リアルタイム機械学習の概要
topic-legacy: Getting started
description: 次のドキュメントでは、Adobe Experience Platformでリアルタイム機械学習モデルを作成するために必要な手順について説明します。
exl-id: 90a1c580-f6e7-4517-aa1e-da5092fbc4a2
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 1%

---

# リアルタイム機械学習（アルファ）の概要

>[!IMPORTANT]
>
>リアルタイム機械学習は、まだすべてのユーザーが利用できるわけではありません。 この機能はアルファ版で、まだテスト中です。 このドキュメントは変更される場合があります。

リアルタイム機械学習を利用するには、Adobe Experience Platformと [!DNL Data Science Workspace] がプロビジョニングされた組織にアクセスできる必要があります。 さらに、トレーニングとスコアリングに使用する完全なデータセットが必要です。

リアルタイム機械学習のガイドでは、Python 3、[Jupyter ノートブック ](../jupyterlab/overview.md)、データサイエンス、機械学習に関する十分な知識が必要です。

**キーワード:**

- **DSL:** ドメイン固有の言語。
- **Edge:** リアルタイムの機械学習スコアリングサービスは、アクティベーションやアプリケーションに近いエッジクラスターで実行できます。
- **ハブ：** 現在のアルファは、Experience Edge ネットワークの開発中に、Adobe Experience Platform Hub でリアルタイム機械学習スコアリングサービスを実行しています。
- **Node:** Node は、グラフを形成する基本単位です。各ノードは特定のタスクを実行し、リンクを使用して連結し、ML パイプラインを表すグラフを形成できます。 ノードが実行するタスクは、データやスキーマの変換、機械学習の推論などの入力データに対する操作を表す。 ノードは、変換または推論された値を次のノードに出力します。

## Adobe Experience Platformのデータセット

リアルタイム機械学習の使用を開始するには、データセットにアクセスできる必要があります。 外部データセットを使用して [!DNL JupyterLab] 環境にアップロードするか、まだ作成していない場合は Platform 内で新しいデータセットを作成するかを選択できます。

>[!NOTE]
>
>使用するデータセットが既にある場合は、「[ 次の手順 ](#next-steps)」にスキップできます。

### 外部データセットの使用

[!DNL JupyterLab] 環境へのデータのアップロードなど、外部データセットの使用について詳しくは、[ ノートブック ](../jupyterlab/analyze-your-data.md#external-data) を使用したデータの分析に関するチュートリアルを参照してください。

### 新しいデータセットの作成

リアルタイム機械学習で使用する新しいデータセットを作成するには、データセットのデータスキーマが必要です。 次に、作成したスキーマを使用してデータを取り込む必要があります。 次のチュートリアルを使用して、[!DNL Platform] のデータセットを作成し、設定します。

- [API でのデータセットの作成と設定](../../catalog/datasets/create.md)
- [UI でのデータセットの作成と設定](../../ingestion/tutorials/ingest-batch-data.md)

## 次の手順 {#next-steps}

リアルタイム機械学習用のデータを準備したら、まず『[ リアルタイム機械学習ノートブックユーザガイド ](./rtml-authoring-notebook.md)』に従って、ONNX モデルを作成し、リアルタイム機械学習モデルストアにアップロードする方法を学びます。
