---
keywords: Experience Platform；スキーマデータのプレビュー；Data Science Workspace；人気の高いトピック
solution: Experience Platform
title: 小売販売スキーマとデータセットのプレビュー
type: Tutorial
description: 次のドキュメントでは、Adobe Experience Platformでのプレビュースキーマとデータセットの概要を説明します。
exl-id: dca9835b-4f76-42cc-b262-b20323bf4356
source-git-commit: 86e6924078c115fb032ce39cd678f1d9c622e297
workflow-type: tm+mt
source-wordcount: '236'
ht-degree: 22%

---

# 小売販売スキーマとデータセットのプレビュー

が正常に完了すると、 [小売販売スキーマとデータセット](./create-retails-sales-dataset.md) チュートリアル 出力スキーマとデータセットは、で表示できます。 [!DNL Experience Platform]. スキーマとデータセットを表示するには、次の手順に従います。

を選択します。 **[!UICONTROL スキーマ]** タブが左側のナビゲーションに配置され、ブートストラップスクリプトで作成された入力スキーマを探します。 スキーマの名前は、前の手順の `config.yaml` で定義した名前に対応します。スキーマをクリックし、スキーマの詳細とその構成を表示します。

![](../images/models-recipes/access-data/schema.PNG)

を選択します。 **[!UICONTROL データセット]** 左側のナビゲーションにある「 」タブで、データセットの名前を選択して作成した入力データセットを開きます。 データセットの名前は、 `config.yaml` 前の手順から。

![](../images/models-recipes/access-data/dataset.PNG)

選択 **[!UICONTROL データセットをプレビュー]** 右上にあり、データセットのサブセットをプレビューします。

![](../images/models-recipes/access-data/preview.PNG)

## 次の手順

小売販売のサンプルデータをに正常に取り込みました [!DNL Experience Platform] 提供されたブートストラップスクリプトを使用する。

取得したデータを引き続き使用するには、以下を実行します。
- [Jupyter Notebook を使用したデータ分析](../jupyterlab/analyze-your-data.md)
   - での Jupyter Notebook の使用 [!DNL Data Science Workspace] データへのアクセス、調査、視覚化、理解
- [ソースファイルのレシピへのパッケージ化](./package-source-files-recipe.md)
   - このチュートリアルでは、独自のモデルをに取り込む方法を説明します。 [!DNL Data Science Workspace] インポート可能なレシピファイルにソースファイルをパッケージ化する。
