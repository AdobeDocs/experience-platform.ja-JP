---
keywords: Experience Platform;プレビュースキーマデータ；Data Science Workspace；人気の高いトピック
solution: Experience Platform
title: 小売売上スキーマとデータセットのプレビュー
topic: チュートリアル
type: チュートリアル
description: 次のドキュメントでは、Adobe Experience Platformでのスキーマとデータセットのプレビューの概要を説明します。
translation-type: tm+mt
source-git-commit: 5129a75071af680bc54a7f60bb89ce32d3216d09
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 22%

---


# 小売売上スキーマとデータセットのプレビュー

[小売売上スキーマとデータセット](./create-retails-sales-dataset.md)のチュートリアルで、ブートストラップスクリプトが正常に完了したとき。 出力スキーマーとデータセットは[!DNL Experience Platform]で表示できます。 スキーマとデータセットを表示するには、次の手順に従います。

左側のナビゲーションにある&#x200B;**[!UICONTROL スキーマ]**&#x200B;タブを選択し、ブートストラップスクリプトで作成された入力スキーマを探します。 スキーマの名前は、前の手順の `config.yaml` で定義した名前に対応します。スキーマをクリックし、スキーマの詳細とその構成を表示します。

![](../images/models-recipes/access-data/schema.PNG)

左側のナビゲーションにある「**[!UICONTROL データセット]**」タブを選択し、データセットの名前を選択して作成した入力データセットを開きます。 データセットの名前は、前の手順の`config.yaml`で定義した名前に対応しています。

![](../images/models-recipes/access-data/dataset.PNG)

右上にある「プレビューデータセット&#x200B;**[!UICONTROL 」を選択して、データセットのサブセットをプレビューします。]**

![](../images/models-recipes/access-data/preview.PNG)

## 次の手順

これで、提供されたブートストラップスクリプトを使用して、小売販売のサンプルデータを[!DNL Experience Platform]に正しく取り込みました。

取得したデータを引き続き使用するには、以下を実行します。
- [Jupyterノートブックを使用してデータを分析](../jupyterlab/analyze-your-data.md)
   - [!DNL Data Science Workspace]のJupyterノートブックを使用して、データにアクセスし、調査し、視覚化し、理解します。
- [ソースファイルのレシピへのパッケージ化](./package-source-files-recipe.md)
   - 読み込み可能なレシピファイルにソースファイルをパッケージ化して、独自のモデルを[!DNL Data Science Workspace]に取り込む方法を学ぶには、このチュートリアルに従ってください。