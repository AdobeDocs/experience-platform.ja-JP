---
keywords: Experience Platform;developer guide;Data Science Workspace;popular topics;Real time machine learning;
solution: Experience Platform
title: リアルタイム機械学習の概要
topic: Getting started
translation-type: tm+mt
source-git-commit: c9e6eb41e51ad17ad11b9a669ca5e4cf6c899f8b
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 0%

---


# リアルタイム機械学習の概要

>[!IMPORTANT]
>リアルタイム機械学習は、まだすべてのユーザーが利用できるわけではありません。 この機能はアルファベットで、まだテスト中です。 このドキュメントは変更される可能性があります。

Real-time Machine Learningを利用するには、Adobe Experience PlatformとData Science Workspaceのプロビジョニングを受けた組織にアクセスできる必要があります。 また、プラットフォームにデータセットが必要です。

リアルタイム機械学習ガイドでは、Python 3、 [Jupyterノートブック](../jupyterlab/overview.md)、データサイエンス、機械学習について十分に理解しておく必要があります。

## Adobe Experience Platformのデータセット

Real-time Machine Learningを使用した開始を行うには、Experience Platform内にデータセットを設定する必要があります。 外部データセットを使用してJupterLab環境にアップロードするか、まだ作成していない場合は、プラットフォーム内で新しいデータセットを作成するかを選択できます。

>[!NOTE]
>使用するデータセットが既にある場合は、次の2つの手順をスキップできます。

### 外部データセットの使用

JupterLab環境へのデータのアップロードなど、外部データセットの使用方法の詳細については、ノートブックを使用したデータ [分析のチュートリアルを参照してください](../jupyterlab/analyze-your-data.md#external-data)。

### 新しいデータセットの作成

リアルタイム機械学習で使用する新しいデータセットを作成するには、データセットのデータスキーマが必要です。 次に、作成したスキーマを使用してデータを取り込む必要があります。 次のチュートリアルを使用して、プラットフォームのデータセットを作成し、設定します。

- [APIでのデータセットの作成と設定](../../catalog/datasets/create.md)
- [UIでのデータセットの作成と設定](../../ingestion/tutorials/ingest-batch-data.md)

## GitとDocker

Data Science Workplaceレシピワークフローを使用してモデルをトレーニングする場合は、GitとDockerが必要です。

>[!NOTE]
>Pythonノートブックを使用してモデルのトレーニングを行う場合や、独自のONNXモデルを使用する場合は、GitやDockerをダウンロードする必要はありません。

- [Gitインストールガイド](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [Dockerインストールガイド](https://docs.docker.com/get-docker/)

## 次の手順

リアルタイム機械学習用のデータを準備したら、モデルの [トレーニングに関するチュートリアルに従って](./training-ml-model.md) 、ONNXモデルの作成方法とReal-time Machine Learningモデルストアへのアップロード方法を開始します。

