---
keywords: Experience Platform；スキーマデータのプレビュー；Data Science Workspace；人気のトピック
solution: Experience Platform
title: 小売販売のスキーマとデータセットのプレビュー
type: Tutorial
description: 次のドキュメントでは、Adobe Experience Platformのスキーマとデータセットのプレビューの概要を説明します。
exl-id: dca9835b-4f76-42cc-b262-b20323bf4356
source-git-commit: 5d98dc0cbfaf3d17c909464311a33a03ea77f237
workflow-type: tm+mt
source-wordcount: '259'
ht-degree: 20%

---

# 小売販売のスキーマとデータセットのプレビュー

>[!NOTE]
>
>Data Science Workspaceは購入できなくなりました。
>
>このドキュメントは、Data Science Workspaceの以前の使用権限を持つ既存のお客様を対象としています。

[retail sales schema and dataset](./create-retails-sales-dataset.md) チュートリアルの bootstrap スクリプトが正常に完了した場合。 出力スキーマおよびデータセットは、[!DNL Experience Platform] で表示できます。 スキーマとデータセットを表示するには、次の手順に従います。

左側のナビゲーションにある「**[!UICONTROL スキーマ]**」タブを選択し、bootstrap スクリプトで作成された入力スキーマを見つけます。 スキーマの名前は、前の手順の `config.yaml` で定義した名前に対応します。スキーマをクリックし、スキーマの詳細とその構成を表示します。

![](../images/models-recipes/access-data/schema.PNG)

左側のナビゲーションにある **[!UICONTROL データセット]** タブを選択し、データセットの名前を選択して、作成した入力データセットを開きます。 データセットの名前は、前の手順で `config.yaml` で定義した名前に対応しています。

![](../images/models-recipes/access-data/dataset.PNG)

右上の **[!UICONTROL データセットをプレビュー]** を選択し、データセットのサブセットをプレビューします。

![](../images/models-recipes/access-data/preview.PNG)

## 次の手順

これで、提供された Bootstrap スクリプトを使用して、小売販売サンプルデータを [!DNL Experience Platform] に正常に取り込みました。

取得したデータを引き続き使用するには、以下を実行します。
- [Jupyter Notebook を使用してデータを分析](../jupyterlab/analyze-your-data.md)
   - Jupyter Notebooks を [!DNL Data Science Workspace] で使用し、データへのアクセス、調査、視覚化、理解を行います。
- [ソースファイルのレシピへのパッケージ化](./package-source-files-recipe.md)
   - このチュートリアルでは、ソースファイルをインポート可能なレシピファイルにパッケージ化して、独自のモデルを [!DNL Data Science Workspace] に取り込む方法を説明します。
