---
keywords: Experience Platform;preview schema data;Data Science Workspace;popular topics
solution: Experience Platform
title: プレビュースキーマとデータセット
topic: Tutorial
translation-type: tm+mt
source-git-commit: 4b0f0dda97f044590f55eaf75a220f631f3313ee
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 73%

---


# プレビュースキーマとデータセット

「[小売販売スキーマとデータセットの作成](./create-retails-sales-dataset.md)」のチュートリアルのブートストラップスクリプトの実行が正常に完了したら、Output schemas and datasets can be viewed on [!DNL Experience Platform]. スキーマとデータセットを表示するには、次の手順に従います。

1. 左側のナビゲーション列にある「**[!UICONTROL スキーマ]**」リンクをクリックし、ブートストラップスクリプトで作成された入力スキーマを見つけます。スキーマの名前は、前の手順の `config.yaml` で定義した名前に対応します。スキーマをクリックし、スキーマの詳細とその構成を表示します。

   ![](../images/models-recipes/access-data/schema_overview.png)

2. 左側のナビゲーション列にある「**[!UICONTROL データセット]**」リンクをクリックし、リストの名前をクリックして、作成した入力データセットを開きます。データセットの名前は、前の手順の `config.yaml` で定義した名前に対応します。

   ![](../images/models-recipes/access-data/dataset_overview.png)

3. 右上の「**[!UICONTROL プレビューデータセット」]**&#x200B;をクリックし、データセットのサブセットを表示します。

   ![](../images/models-recipes/access-data/preview_dataset.png)

## 次の手順

You have now successfully ingested Retail Sales sample data into [!DNL Experience Platform] using the provided bootstrap script.

取得したデータを引き続き使用するには、以下を実行します。
- [Jupyter ノートブックを使用したデータ分析](../jupyterlab/analyze-your-data.md)
   - Use Jupyter notebooks in [!DNL Data Science Workspace] to access, explore, visualize, and understand your data.
- [ソースファイルのレシピへのパッケージ化](./package-source-files-recipe.md)
   - Follow this tutorial to learn how to bring your own Model into [!DNL Data Science Workspace] by packaging source files in an importable Recipe file.