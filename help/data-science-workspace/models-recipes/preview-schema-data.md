---
keywords: Experience Platform;プレビュースキーマデータ；Data Science Workspace；人気の高いトピック
solution: Experience Platform
title: 小売売上スキーマとデータセットのプレビュー
topic: tutorial
type: Tutorial
description: 次のドキュメントでは、Adobe Experience Platformでのスキーマとデータセットのプレビューの概要を説明します。
translation-type: tm+mt
source-git-commit: 698639d6c2f7897f0eb4cce2a1f265a0f7bb57c9
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 59%

---


# 小売売上スキーマとデータセットのプレビュー

「[小売販売スキーマとデータセットの作成](./create-retails-sales-dataset.md)」のチュートリアルのブートストラップスクリプトの実行が正常に完了したら、出力スキーマーとデータセットは[!DNL Experience Platform]で表示できます。 スキーマとデータセットを表示するには、次の手順に従います。

1. 左側のナビゲーション列にある「**[!UICONTROL スキーマ]**」リンクをクリックし、ブートストラップスクリプトで作成された入力スキーマを見つけます。スキーマの名前は、前の手順の `config.yaml` で定義した名前に対応します。スキーマをクリックし、スキーマの詳細とその構成を表示します。

   ![](../images/models-recipes/access-data/schema_overview.png)

2. 左側のナビゲーション列にある「**[!UICONTROL データセット]**」リンクをクリックし、リストの名前をクリックして、作成した入力データセットを開きます。データセットの名前は、前の手順の `config.yaml` で定義した名前に対応します。

   ![](../images/models-recipes/access-data/dataset_overview.png)

3. 右上の「**[!UICONTROL プレビューデータセット」]**&#x200B;をクリックし、データセットのサブセットを表示します。

   ![](../images/models-recipes/access-data/preview_dataset.png)

## 次の手順

これで、提供されたブートストラップスクリプトを使用して、小売販売のサンプルデータを[!DNL Experience Platform]に正しく取り込みました。

取得したデータを引き続き使用するには、以下を実行します。
- [Jupyterノートブックを使用してデータを分析](../jupyterlab/analyze-your-data.md)
   - [!DNL Data Science Workspace]のJupyterノートブックを使用して、データにアクセスし、調査し、視覚化し、理解します。
- [ソースファイルのレシピへのパッケージ化](./package-source-files-recipe.md)
   - 読み込み可能なレシピファイルにソースファイルをパッケージ化して、独自のモデルを[!DNL Data Science Workspace]に取り込む方法を学ぶには、このチュートリアルに従ってください。