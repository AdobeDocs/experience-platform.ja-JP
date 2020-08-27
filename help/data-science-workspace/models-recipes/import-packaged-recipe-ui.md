---
keywords: Experience Platform;import packaged recipe;Data Science Workspace;popular topics;recipes;ui;create engine
solution: Experience Platform
title: パッケージ化されたレシピのインポート（UI）
topic: Tutorial
description: このチュートリアルでは、提供された「小売売上」の例を使用してパッケージ化されたレシピを設定およびインポートする方法に関するインサイトを提供します。このチュートリアルの最後までに、Adobe Experience Platform Data Science Workspace　でモデルの作成、トレーニング、評価をおこなう準備が整います。
translation-type: tm+mt
source-git-commit: 9ba229195892245d29fb4f17b9f2e5cd6c6ea567
workflow-type: tm+mt
source-wordcount: '1803'
ht-degree: 42%

---


# パッケージ化されたレシピのインポート（UI）

このチュートリアルでは、提供された「小売売上」の例を使用してパッケージ化されたレシピを設定およびインポートする方法に関するインサイトを提供します。By the end of this tutorial, you will be ready to create, train, and evaluate a Model in Adobe Experience Platform [!DNL Data Science Workspace].

## 前提条件

このチュートリアルでは、DockerイメージURLの形式でパッケージ化されたレシピが必要です。 詳しくは、[ソースファイルをレシピにパッケージ化する](./package-source-files-recipe.md)方法に関するチュートリアルを参照してください。

## UI ワークフロー

Importing a packaged recipe into [!DNL Data Science Workspace] requires specific recipe configurations, compiled into a single JavaScript Object Notation (JSON) file, this compilation of recipe configurations is referred to as the **configuration file**. 特定の設定のセットを含むパッケージ化されたレシピは、**レシピインスタンス**&#x200B;と呼ばれます。One recipe can be used to create many recipe instances in [!DNL Data Science Workspace].

パッケージレシピをインポートするワークフローは、次の手順で構成されます。
- [レシピの設定](#configure)
- [Docker ベースのレシピのインポート — Python](#python)
- [Docker ベースのレシピのインポート — R](#r)
- [Dockerベースのレシピのインポート — PySpark](#pyspark)
- [Dockerベースのレシピのインポート — Scala](#scala)

### レシピの設定 {#configure}

Every recipe instance in [!DNL Data Science Workspace] is accompanied with a set of configurations that tailor the recipe instance to suit a particular use case. 設定ファイルは、このレシピインスタンスを使用して作成されたモデルのデフォルトのトレーニングおよびスコアリング動作を定義します。

>[!NOTE]
>
> 設定ファイルは、レシピとケースに固有です。

以下に、「小売売上」レシピのデフォルトのトレーニングとスコアリングの動作を示す設定ファイルの例を示します。

```json
[
    {
        "name": "train",
        "parameters": [
            {
                "key": "learning_rate",
                "value": "0.1"  
            },
            {
                "key": "n_estimators",
                "value": "100"
            },
            {
                "key": "max_depth",
                "value": "3"
            },
            {
                "key": "ACP_DSW_INPUT_FEATURES",
                "value": "date,store,storeType,storeSize,temperature,regionalFuelPrice,markdown,cpi,unemployment,isHoliday"
            },
            {
                "key": "ACP_DSW_TARGET_FEATURES",
                "value": "weeklySales"
            },
            {
                "key": "ACP_DSW_FEATURE_UPDATE_SUPPORT",
                "value": false
            },
            {
                "key": "tenantId",
                "value": "_{TENANT_ID}"
            },
            {
                "key": "ACP_DSW_TRAINING_XDM_SCHEMA",
                "value": "{SEE BELOW FOR DETAILS}"
            },
            {
                "key": "evaluation.labelColumn",
                "value": "weeklySalesAhead"
            },
            {
                "key": "evaluation.metrics",
                "value": "MAPE,MAE,RMSE,MASE"
            }
        ]
    },
    {
        "name": "score",
        "parameters": [
            {
                "key": "tenantId",
                "value": "_{TENANT_ID}"
            },
            {
                "key":"ACP_DSW_SCORING_RESULTS_XDM_SCHEMA",
                "value":"{SEE BELOW FOR DETAILS}"
            }
        ]
    }
]
```

| パラメーターキー | タイプ | 説明 |
| ----- | ----- | ----- |
| `learning_rate` | 数値 | グラデーション乗算用のスカラー |
| `n_estimators` | 数値 | ランダムフォレスト分類子のフォレスト内のツリーの数 |
| `max_depth` | 数値 | ランダムフォレスト分類子のツリーの最大深さ |
| `ACP_DSW_INPUT_FEATURES` | 文字列 | コンマ区切りの入力スキーマ属性のリスト |
| `ACP_DSW_TARGET_FEATURES` | 文字列 | コンマ区切りの出力スキーマ属性のリスト |
| `ACP_DSW_FEATURE_UPDATE_SUPPORT` | Boolean | 入出力機能が変更可能かどうかを特定します。 |
| `tenantId` | 文字列 | この ID は、作成するリソースの名前空間が適切に設定され、IMS 組織内に含まれるようにします。テナント ID を検索するには、[こちらの手順](../../xdm/api/getting-started.md#know-your-tenant_id)に従います。 |
| `ACP_DSW_TRAINING_XDM_SCHEMA` | 文字列 | モデルのトレーニングに使用する入力スキーマ。UI にインポートする場合は空のままにし、API を使用してインポートする場合はトレーニングスキーマ ID に置き換えます。 |
| `evaluation.labelColumn` | 文字列 | 評価のビジュアライゼーションの列ラベル |
| `evaluation.metrics` | 文字列 | モデルの評価に使用される評価指標のカンマ区切りのリスト |
| `ACP_DSW_SCORING_RESULTS_XDM_SCHEMA` | 文字列 | モデルのスコアリングに使用される出力スキーマUI にインポートする場合は空のままにし、API を使用してインポートする場合はスコアリングスキーマ ID に置き換えます。 |

For the purpose of this tutorial, you can leave the default configuration files for Retail Sales recipe in the [!DNL Data Science Workspace] Reference the way they are.

### Import Docker based recipe - [!DNL Python] {#python}

開始するには、 **[!UICONTROL UIの左上にある]**[!DNL Platform] ワークフローを移動して選択します。 次に、「レシピを *読み込む* 」を選択し、「 **[!UICONTROL 起動]**」をクリックします。

![](../images/models-recipes/import-package-ui/launch-import.png)

「 *読み込みレシピ* 」ワークフローの「 *設定* 」ページが表示されます。 レシピの名前と説明を入力し、右上隅の **[!UICONTROL 「次]** 」を選択します。

![ワークフローの設定](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
>
> 『[ソースファイルをレシピにパッケージ化](./package-source-files-recipe.md)』チュートリアルでは、Python ソースファイルを使用して Retail Sales レシピを作成する最後に Docker URL が提供されていました。

「 *Select source* (ソースを [!DNL Python] 選択)」ページに移動したら、「 **[!UICONTROL Source URL]** （ソースURL）」フィールドに、ソースファイルを使用して作成したパッケージレシピに対応するDocker URLを貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップするか、ファイルシステムの&#x200B;**ブラウザー**&#x200B;を使用してインポートします。提供された設定ファイルは `experience-platform-dsw-reference/recipes/python/retail/retail.config.json` にあります。「 **[!UICONTROL Runtime]** 」ドロップダウンで「 *Python* 」を選択し、「 **[!UICONTROL Type」ドロップダウンで「]** Classification ** 」を選択します。 すべての情報が入力されたら、右上隅の **[!UICONTROL 「次へ]** 」をクリックして「スキーマの *管理*」に進みます。

>[!NOTE]
>
> *タイプは* 、 **[!UICONTROL 分類]** と **[!UICONTROL 回帰をサポートします]**。 モデルがこれらのタイプのいずれにも該当しない場合は、「 **[!UICONTROL カスタム]**」(Custom)を選択します。

![](../images/models-recipes/import-package-ui/recipe_source_python.png)

Next, select the Retail Sales input and output schemas under the section *Manage Schemas*, they were created using the provided bootstrap script in the [create the retail sales schema and dataset](../models-recipes/create-retails-sales-dataset.md) tutorial.

![](../images/models-recipes/import-package-ui/recipe_schema.png)

Under the *Feature Management* section, click on your tenant identification in the schema viewer to expand the Retail Sales input schema. 目的の入出力機能をハイライト表示し、右側の&#x200B;**[!UICONTROL フィールドプロパティ]**&#x200B;ウィンドウで「**[!UICONTROL 入力機能]**」または「**[!UICONTROL ターゲット機能]**」を選択して、入力機能と出力機能を選択します。このチュートリアルの目的では、**[!UICONTROL weeklySales]** を&#x200B;**[!UICONTROL ターゲット機能]**、その他すべてを&#x200B;**[!UICONTROL 入力機能]**&#x200B;として設定します。「**[!UICONTROL 次へ]**」をクリックして、新しく設定したレシピを確認します。

レシピを確認し、必要に応じて、設定を追加、変更または削除します。「**[!UICONTROL 完了]**」をクリックし、レシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

Proceed to the [next steps](#next-steps) to find out how to create a Model in [!DNL Data Science Workspace] using the newly created Retail Sales recipe.

### Docker ベースのレシピのインポート — R {#r}

開始するには、 **[!UICONTROL UIの左上にある]**[!DNL Platform] ワークフローを移動して選択します。 次に、「レシピを *読み込む* 」を選択し、「 **[!UICONTROL 起動]**」をクリックします。

![](../images/models-recipes/import-package-ui/launch-import.png)

「 *読み込みレシピ* 」ワークフローの「 *設定* 」ページが表示されます。 レシピの名前と説明を入力し、右上隅の **[!UICONTROL 「次]** 」を選択します。

![ワークフローの設定](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
>
> 「[ソースファイルをレシピにパッケージ化](./package-source-files-recipe.md)」チュートリアルでは、R ソースファイルを使用して Retail Sales レシピを作成する最後に Docker URL が提供されていました。

「 *Select source* 」ページに移動したら、「 **[!UICONTROL Source URL]** 」フィールドに、Rソースファイルを使用して作成したパッケージ化レシピに対応するDocker URLを貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップするか、ファイルシステムの&#x200B;**ブラウザー**&#x200B;を使用してインポートします。提供された設定ファイルは `experience-platform-dsw-reference/recipes/R/Retail\ -\ GradientBoosting/retail.config.json` にあります。「 **[!UICONTROL 実行時]** 」ドロップダウンで「 *R* 」を選択し、「 **[!UICONTROL タイプ]** 」ドロップダウンで「分類 ** 」を選択します。 すべての情報が入力されたら、右上隅の **[!UICONTROL 「次へ]** 」をクリックして「スキーマの *管理*」に進みます。

>[!NOTE]
>
> *タイプは* 、 **[!UICONTROL 分類]** と **[!UICONTROL 回帰をサポートします]**。 モデルがこれらのタイプのいずれにも該当しない場合は、「 **[!UICONTROL カスタム]**」(Custom)を選択します。

![](../images/models-recipes/import-package-ui/recipe_source_R.png)

Next, select the Retail Sales input and output schemas under the section *Manage Schemas*, they were created using the provided bootstrap script in the [create the retail sales schema and dataset](../models-recipes/create-retails-sales-dataset.md) tutorial.

![](../images/models-recipes/import-package-ui/recipe_schema.png)

Under the *Feature Management* section, click on your tenant identification in the schema viewer to expand the Retail Sales input schema. 目的の入出力機能をハイライト表示し、右側の&#x200B;**[!UICONTROL フィールドプロパティ]**&#x200B;ウィンドウで「**[!UICONTROL 入力機能]**」または「**[!UICONTROL ターゲット機能]**」を選択して、入力機能と出力機能を選択します。このチュートリアルの目的では、**[!UICONTROL weeklySales]** を&#x200B;**[!UICONTROL ターゲット機能]**、その他すべてを&#x200B;**[!UICONTROL 入力機能]**&#x200B;として設定します。Click **[!UICONTROL Next]** to review your new Configured recipe.

レシピを確認し、必要に応じて、設定を追加、変更または削除します。「**完了**」をクリックし、レシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

Proceed to the [next steps](#next-steps) to find out how to create a Model in [!DNL Data Science Workspace] using the newly created Retail Sales recipe.

### Import Docker based recipe - PySpark {#pyspark}

開始するには、 **[!UICONTROL UIの左上にある]**[!DNL Platform] ワークフローを移動して選択します。 次に、「レシピを *読み込む* 」を選択し、「 **[!UICONTROL 起動]**」をクリックします。

![](../images/models-recipes/import-package-ui/launch-import.png)

「 *読み込みレシピ* 」ワークフローの「 *設定* 」ページが表示されます。 レシピの名前と説明を入力し、右上隅の **[!UICONTROL 「次へ]** 」を選択して次に進みます。

![ワークフローの設定](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
>
> In the [Package source files into a Recipe](./package-source-files-recipe.md) tutorial, a Docker URL was provided at the end of building the Retail Sales recipe using PySpark source files.

「 *Select source* 」ページに移動したら、PySparkソースファイルを使用して作成したパッケージレシピに対応するDocker URLを「 **[!UICONTROL Source URL]** 」フィールドに貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップするか、ファイルシステムの&#x200B;**ブラウザー**&#x200B;を使用してインポートします。提供された設定ファイルは `experience-platform-dsw-reference/recipes/pyspark/retail/pipeline.json` にあります。「 **[!UICONTROL Runtime]** 」ドロップダウンで「PySpark ** 」を選択します。 PySparkランタイムが選択されると、デフォルトのアーティファクトが **[!UICONTROL Dockerに自動入力されます]**。 次に、「 **[!UICONTROL タイプ]** 」ドロップダウンで「 *分類* 」を選択します。 すべての情報が入力されたら、右上隅の **[!UICONTROL 「次へ]** 」をクリックして「スキーマの *管理*」に進みます。

>[!NOTE]
>
> *タイプは* 、 **[!UICONTROL 分類]** と **[!UICONTROL 回帰をサポートします]**。 モデルがこれらのタイプのいずれにも該当しない場合は、「 **[!UICONTROL カスタム]**」(Custom)を選択します。

![](../images/models-recipes/import-package-ui/pyspark-databricks.png)

Next, select the Retail Sales input and output schemas under the section *Manage Schemas*, they were created using the provided bootstrap script in the [create the retail sales schema and dataset](../models-recipes/create-retails-sales-dataset.md) tutorial.

![](../images/models-recipes/import-package-ui/recipe_schema.png)

Under the *Feature Management* section, click on your tenant identification in the schema viewer to expand the Retail Sales input schema. 目的の入出力機能をハイライト表示し、右側の&#x200B;**[!UICONTROL フィールドプロパティ]**&#x200B;ウィンドウで「**[!UICONTROL 入力機能]**」または「**[!UICONTROL ターゲット機能]**」を選択して、入力機能と出力機能を選択します。このチュートリアルの目的では、**[!UICONTROL weeklySales]** を&#x200B;**[!UICONTROL ターゲット機能]**、その他すべてを&#x200B;**[!UICONTROL 入力機能]**&#x200B;として設定します。「**[!UICONTROL 次へ]**」をクリックして、新しく設定したレシピを確認します。

レシピを確認し、必要に応じて、設定を追加、変更または削除します。「**[!UICONTROL 完了]**」をクリックし、レシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

Proceed to the [next steps](#next-steps) to find out how to create a Model in [!DNL Data Science Workspace] using the newly created Retail Sales recipe.

### Import Docker based recipe - Scala {#scala}

開始するには、 **[!UICONTROL UIの左上にある]**[!DNL Platform] ワークフローを移動して選択します。 次に、「レシピを *読み込む* 」を選択し、「 **[!UICONTROL 起動]**」をクリックします。

![](../images/models-recipes/import-package-ui/launch-import.png)

「 *読み込みレシピ* 」ワークフローの「 *設定* 」ページが表示されます。 レシピの名前と説明を入力し、右上隅の **[!UICONTROL 「次へ]** 」を選択して次に進みます。

![ワークフローの設定](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
>
> In the [Package source files into a Recipe](./package-source-files-recipe.md) tutorial, a Docker URL was provided at the end of building the Retail Sales recipe using Scala ([!DNL Spark]) source files.

「 *Select source* （ソースを選択）」ページに移動したら、「 *Source URL* （ソースURL）」フィールドに、Scalaソースファイルを使用して作成したパッケージレシピに対応するDocker URLを貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップするか、ファイルシステムの&#x200B;**ブラウザー**&#x200B;を使用してインポートします。提供された設定ファイルは `experience-platform-dsw-reference/recipes/scala/retail/pipelineservice.json` にあります。「 **[!UICONTROL Runtime]** 」ドロップダウンで「 *Spark* 」を選択します。 ラン [!DNL Spark] タイムが選択されると、デフォルトのアーチファクトが **[!UICONTROL Dockerに自動入力されます]**。 次に、「 **[!UICONTROL タイプ]** 」ドロップダウンから「 *回帰* 」を選択します。 すべての情報が入力されたら、右上隅の **[!UICONTROL 「次へ]** 」をクリックして「スキーマの *管理*」に進みます。

>[!NOTE]
>
> *タイプは* 、 **[!UICONTROL 分類]** と **[!UICONTROL 回帰をサポートします]**。 モデルがこれらのタイプのいずれにも該当しない場合は、「 **[!UICONTROL カスタム]**」(Custom)を選択します。

![](../images/models-recipes/import-package-ui/scala-databricks.png)

Next, select the Retail Sales input and output schemas under the section *Manage Schemas*, they were created using the provided bootstrap script in the [create the retail sales schema and dataset](../models-recipes/create-retails-sales-dataset.md) tutorial.

![](../images/models-recipes/import-package-ui/recipe_schema.png)

Under the *Feature Management* section, click on your tenant identification in the schema viewer to expand the Retail Sales input schema. 目的の入出力機能をハイライト表示し、右側の&#x200B;**[!UICONTROL フィールドプロパティ]**&#x200B;ウィンドウで「**[!UICONTROL 入力機能]**」または「**[!UICONTROL ターゲット機能]**」を選択して、入力機能と出力機能を選択します。このチュートリアルの目的では、**[!UICONTROL weeklySales]** を&#x200B;**[!UICONTROL ターゲット機能]**、その他すべてを&#x200B;**[!UICONTROL 入力機能]**&#x200B;として設定します。「**[!UICONTROL 次へ]**」をクリックして、新しく設定したレシピを確認します。

レシピを確認し、必要に応じて、設定を追加、変更または削除します。「**[!UICONTROL 完了]**」をクリックし、レシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

Proceed to the [next steps](#next-steps) to find out how to create a Model in [!DNL Data Science Workspace] using the newly created Retail Sales recipe.

## 次の手順 {#next-steps}

This tutorial provided insight on configuring and importing a recipe into [!DNL Data Science Workspace]. 新しく作成したレシピを使用して、モデルの作成、トレーニング、評価をおこなうことができるようになりました。

- [UI でのモデルのトレーニングと評価](./train-evaluate-model-ui.md)
- [API を使用したモデルのトレーニングと評価](./train-evaluate-model-api.md)