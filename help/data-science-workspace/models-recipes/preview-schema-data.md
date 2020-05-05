---
keywords: Experience Platform;preview schema data;Data Science Workspace;popular topics
solution: Experience Platform
title: プレビュースキーマとデータセット
topic: Tutorial
translation-type: tm+mt
source-git-commit: e08460bc76d79920bbc12c7665a1416d69993f34

---


# プレビュースキーマとデータセット

「小売売上データセットの [作成」チュートリアルのブートストラップスクリプトが正常に完了したら、スキーマセット](./create-retails-sales-dataset.md) （英語のみ） 出力スキーマーとデータセットは、エクスペリエンスプラットフォームで表示できます。 スキーマとデータセットを表示するには、次の手順に従います。

1. 左のナビゲーション列にある **[!UICONTROL Schemas]** リンクをクリックし、ブートストラップスクリプトで作成された入力スキーマを探します。 スキーマの名前は、前の手順で定義した名前に対応 `config.yaml` します。 スキーマの詳細を表示し、クリックして組成します。

   ![](../images/models-recipes/access-data/schema_overview.png)

2. 左側のナビゲーション列にある **[!UICONTROL Datasets]** リンクをクリックし、リスト名をクリックして作成した入力データセットを開きます。 データセットの名前は、前の手順で定義した名前 `config.yaml` に対応します。

   ![](../images/models-recipes/access-data/dataset_overview.png)

3. 右上のプレビュー **[!UICONTROL Preview Dataset]** にあるデータセットのサブセットをクリックします。

   ![](../images/models-recipes/access-data/preview_dataset.png)

## 次の手順

提供されたブートストラップスクリプトを使用して、小売販売のサンプルデータをエクスペリエンスプラットフォームに正常に取り込むことができました。

取り込んだデータを引き続き使用するには：
- [Jupyterノートブックを使用してデータを分析する](../jupyterlab/analyze-your-data.md)
   - Data Science WorkspaceのJupyterノートブックを使用して、データにアクセス、調査、視覚化、理解します。
- [ソースファイルのレシピへのパッケージ化](./package-source-files-recipe.md)
   - 読み込み可能なレシピファイルにソースファイルをパッケージ化して、独自のモデルをData Science Workspaceに取り込む方法を学ぶには、このチュートリアルに従います。