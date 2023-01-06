---
keywords: Experience Platform、開発者ガイド、Data Science Workspace、人気の高いトピック、リアルタイムの機械学習、
solution: Experience Platform
title: リアルタイム機械学習の概要
description: 次のドキュメントでは、Adobe Experience Platformでリアルタイム機械学習モデルを作成するために必要な手順について説明します。
exl-id: 90a1c580-f6e7-4517-aa1e-da5092fbc4a2
source-git-commit: 86e6924078c115fb032ce39cd678f1d9c622e297
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 1%

---

# リアルタイム機械学習（アルファ）の概要

>[!IMPORTANT]
>
>リアルタイム機械学習は、まだすべてのユーザーが利用できるわけではありません。 この機能はアルファ版で、まだテスト中です。 このドキュメントは変更される場合があります。

リアルタイム機械学習を利用するには、Adobe Experience Platformと [!DNL Data Science Workspace]. さらに、トレーニングとスコアリングで使用する完全なデータセットが必要です。

リアルタイム機械学習のガイドは、Python 3, [Jupyter Notebooks](../jupyterlab/overview.md)、データサイエンス、機械学習の 3 つに分かれています。

**キーワード:**

- **DSL:** ドメイン固有の言語。
- **エッジ：** リアルタイムの機械学習スコアリングサービスは、アクティベーションやアプリケーションに近いエッジクラスターで実行できます。
- **ハブ：** Experience Edge ネットワークの開発中に、現在のアルファがAdobe Experience Platform Hub でリアルタイムの機械学習スコアリングサービスを実行しています。
- **ノード：** ノードは、グラフが形成される基本単位です。 各ノードは特定のタスクを実行し、リンクを使用して連結し、ML パイプラインを表すグラフを形成できます。 ノードが実行するタスクは、データやスキーマの変換、機械学習の推論などの入力データに対する操作を表します。 ノードは、変換された値または推測される値を次のノードに出力します。

## Adobe Experience Platformのデータセット

リアルタイム機械学習の使用を開始するには、データセットにアクセスできる必要があります。 外部データセットを使用し、 [!DNL JupyterLab] 環境を使用するか、Platform 内で新しいデータセットを作成します（まだ作成していない場合）。

>[!NOTE]
>
>使用するデータセットが既にある場合は、次に進んでください： [次の手順](#next-steps).

### 外部データセットを使用

外部データセットの使用 ( データを [!DNL JupyterLab] 環境については、次のチュートリアルを参照してください： [ノートブックを使用したデータの分析](../jupyterlab/analyze-your-data.md#external-data).

### 新しいデータセットの作成

リアルタイム機械学習で使用する新しいデータセットを作成するには、データセットのデータスキーマが必要です。 次に、作成したスキーマを使用してデータを取り込む必要があります。 次のチュートリアルを使用して、のデータセットを作成および設定します。 [!DNL Platform]:

- [API でのデータセットの作成と入力](../../catalog/datasets/create.md)
- [UI でのデータセットの作成と入力](../../ingestion/tutorials/ingest-batch-data.md)

## 次の手順 {#next-steps}

リアルタイム機械学習のデータを準備したら、まず [リアルタイムの機械学習ノートブックユーザーガイド](./rtml-authoring-notebook.md) を参照して、ONNX モデルを作成し、リアルタイム機械学習モデルストアにアップロードする方法を確認してください。
