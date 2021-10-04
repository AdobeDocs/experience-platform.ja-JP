---
keywords: Experience Platform；スキーマデータのプレビュー；Data Science Workspace；よく読まれるトピック
solution: Experience Platform
title: 小売販売スキーマとデータセットのプレビュー
topic-legacy: tutorial
type: Tutorial
description: 次のドキュメントでは、Adobe Experience Platformでのプレビュースキーマとデータセットの概要を説明します。
exl-id: dca9835b-4f76-42cc-b262-b20323bf4356
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '236'
ht-degree: 22%

---

# 小売販売スキーマとデータセットのプレビュー

[ 小売販売スキーマとデータセット ](./create-retails-sales-dataset.md) のチュートリアルのブートストラップスクリプトが正常に完了したら、 出力スキーマとデータセットは [!DNL Experience Platform] で表示できます。 スキーマとデータセットを表示するには、次の手順に従います。

左側のナビゲーションにある「**[!UICONTROL スキーマ]**」タブを選択し、ブートストラップスクリプトで作成された入力スキーマを探します。 スキーマの名前は、前の手順の `config.yaml` で定義した名前に対応します。スキーマをクリックし、スキーマの詳細とその構成を表示します。

![](../images/models-recipes/access-data/schema.PNG)

左側のナビゲーションにある「**[!UICONTROL データセット]**」タブを選択し、データセットの名前を選択して作成した入力データセットを開きます。 データセットの名前は、前の手順の `config.yaml` で定義した名前に対応します。

![](../images/models-recipes/access-data/dataset.PNG)

右上にある **[!UICONTROL プレビューデータセット]** を選択して、データセットのサブセットをプレビューします。

![](../images/models-recipes/access-data/preview.PNG)

## 次の手順

これで、提供されたブートストラップスクリプトを使用して、小売販売のサンプルデータを [!DNL Experience Platform] に取り込みました。

取得したデータを引き続き使用するには、以下を実行します。
- [Jupyter ノートブックを使用したデータ分析](../jupyterlab/analyze-your-data.md)
   - [!DNL Data Science Workspace] の Jupyter ノートブックを使用して、データにアクセスし、調査、視覚化、理解します。
- [ソースファイルのレシピへのパッケージ化](./package-source-files-recipe.md)
   - このチュートリアルでは、インポート可能なレシピファイルにソースファイルをパッケージ化して、独自のモデルを [!DNL Data Science Workspace] に取り込む方法を説明します。
