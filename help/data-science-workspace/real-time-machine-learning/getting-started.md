---
keywords: Experience Platform;開発者ガイド;データサイエンスワークスペース;人気のトピック;リアルタイム機械学習;
solution: Experience Platform
title: リアルタイム機械学習の基本を学ぶ
description: 次のドキュメントでは、Adobe Experience Platform でリアルタイム機械学習モデルを作成するために必要な手順の概要について説明します。
exl-id: 90a1c580-f6e7-4517-aa1e-da5092fbc4a2
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 83%

---

# リアルタイム機械学習の基本を学ぶ（アルファ版）

>[!NOTE]
>
>Data Science Workspaceは購入できなくなりました。
>
>このドキュメントは、Data Science Workspaceの以前の使用権限を持つ既存のお客様を対象としています。

>[!IMPORTANT]
>
>リアルタイム機械学習は、まだすべてのユーザーが利用できるわけではありません。この機能はアルファ版であり、まだテスト中です。このドキュメントは変更される場合があります。

リアルタイム機械学習を利用するには、Adobe Experience Platform と [!DNL Data Science Workspace] でプロビジョニングされた組織へのアクセス権が必要です。さらに、トレーニングとスコアリングに使用するための完全なデータセットが必要です。

リアルタイム機械学習のガイドを参照するには、Python 3、[Jupyter Notebooks](../jupyterlab/overview.md)、データサイエンスおよび機械学習に関する実践的な理解が必要です。

**キーワード:**

- **DSL：**&#x200B;ドメイン固有の言語。
- **Edge：**&#x200B;リアルタイム機械学習スコアリングサービスは、アクティベーションとアプリケーションに近い Edge クラスターで実行できます。
- **Hub:** 現在のアルファ版は、Adobe Experience Platform Hub でリアルタイム機械学習スコアリングサービスを実行していますが、Edge Networkは開発中です。
- **ノード：**&#x200B;ノードは、グラフを形成する基本的な単位です。各ノードでは特定のタスクを実行し、リンクを使用してノードを連結して、ML パイプラインを表すグラフを構成できます。 ノードで実行されるタスクは、入力データに対する操作（データやスキーマの変換など）や機械学習の推論を表します。 ノードは、変換された値や推論された値を次のノード（複数の場合あり）に出力します。

## Adobe Experience Platform のデータセット

リアルタイム機械学習の使用を開始するには、データセットへのアクセス権が必要です。 まだ行っていない場合は、外部データセットを使用して [!DNL JupyterLab] 環境にアップロードするか、Experience Platform内に新しいデータセットを作成するオプションがあります。

>[!NOTE]
>
>使用するデータセットが既にある場合は、[次の手順](#next-steps)にスキップできます。

### 外部データセットを使用する

[!DNL JupyterLab] 環境へのデータのアップロードなど、外部データセットの使用について詳しくは、[ノートブックを使用したデータの分析](../jupyterlab/analyze-your-data.md#external-data)に関するチュートリアルを参照してください。

### 新しいデータセットの作成

リアルタイム機械学習で使用する新しいデータセットを作成するには、データセットのデータスキーマが必要です。次に、作成したスキーマを使用してデータを取り込む必要があります。次のチュートリアルを使用して、[!DNL Experience Platform] のデータセットを作成して入力します。

- [API でのデータセットの作成と入力](../../catalog/datasets/create.md)
- [UI でのデータセットの作成と入力](../../ingestion/tutorials/ingest-batch-data.md)

## 次の手順 {#next-steps}

リアルタイム機械学習用のデータの準備が整ったら、まず[リアルタイム機械学習ノートブックユーザーガイド](./rtml-authoring-notebook.md)に従って、ONNX モデルを作成し、リアルタイム機械学習モデルストアにアップロードする方法を参照します。
