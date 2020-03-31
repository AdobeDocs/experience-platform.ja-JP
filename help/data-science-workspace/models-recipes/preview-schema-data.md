---
keywords: Experience Platform;preview schema data;Data Science Workspace;popular topics
solution: Experience Platform
title: プレビュースキーマとデータセット
topic: Tutorial
translation-type: tm+mt
source-git-commit: 11eb1859f3e4b059765709e750f18b61509a2f93

---


# プレビュースキーマとデータセット

「小売売上データの作成」チュートリアルのブートストラップ [スクリプトが正常に完了したら](./create-retails-sales-dataset.md) 、スキーマします。 出力スキーマとデータセットは、Experience Platformで表示できます。 スキーマとデータセットを表示するには、次の手順に従います。

1. 左側のナビゲ **ーション** (スキーマ)列にある「スキーマ」リンクをクリックし、ブートストラップスクリプトで作成された入力スクリプトを探します。 スキーマの名前は、前の手順で定義した `config.yaml` 名前に対応します。 表示スキーマの詳細と、クリックして組版を表示します。

   ![](../images/models-recipes/access-data/schema_overview.png)

2. 左側のナビゲ **ーション列にある** 「Datasets」リンクをクリックし、リスト名をクリックして作成した入力データセットを開きます。 データセットの名前は、前の手順で定義したもの `config.yaml` に対応します。

   ![](../images/models-recipes/access-data/dataset_overview.png)

3. 右上の **プレビュー** （データセットのサブセット）にあるプレビューデータセットをクリックします。

   ![](../images/models-recipes/access-data/preview_dataset.png)

## 次の手順

これで、提供されたブートストラップスクリプトを使用して、小売販売のサンプルデータをエクスペリエンスプラットフォームに正常に取り込むことができました。

取り込んだデータを引き続き使用するには：
- [Jupyterノートブックを使用してデータを分析する](../jupyterlab/analyze-your-data.md)
   - Data Science WorkspaceのJupyterノートブックを使用して、データにアクセス、調査、視覚化、理解します。
- [ソースファイルのレシピへのパッケージ化](./package-source-files-recipe.md)
   - このチュートリアルでは、読み込み可能なレシピファイルにソースファイルをパッケージ化して、独自のモデルをData Science Workspaceに取り込む方法を説明します。