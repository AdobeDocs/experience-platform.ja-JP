---
keywords: Experience Platform；パッケージ化されたレシピのインポート；Data Science Workspace；人気のあるトピック；レシピ；ui；エンジンの作成
solution: Experience Platform
title: パッケージ化されたレシピのインポート (Data Science Workspace UI)
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、提供された「小売売上」の例を使用してパッケージ化されたレシピを設定およびインポートする方法に関するインサイトを提供します。このチュートリアルの最後までに、Adobe Experience Platform Data Science Workspace　でモデルの作成、トレーニング、評価をおこなう準備が整います。
exl-id: 2556e1f0-3f9c-4884-a699-06c041d5c4d1
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1833'
ht-degree: 37%

---

# パッケージ化されたレシピのインポート（Data Science Workspace UI 版）

このチュートリアルでは、提供された「小売売上」の例を使用してパッケージ化されたレシピを設定およびインポートする方法に関するインサイトを提供します。このチュートリアルの最後までに、Adobe Experience Platform [!DNL Data Science Workspace] でモデルの作成、トレーニング、評価をおこなう準備が整います。

## 前提条件

このチュートリアルでは、Docker イメージ URL の形式でパッケージ化されたレシピが必要です。 詳しくは、[ソースファイルをレシピにパッケージ化する](./package-source-files-recipe.md)方法に関するチュートリアルを参照してください。

## UI ワークフロー

パッケージ化されたレシピを [!DNL Data Science Workspace] に読み込むには、特定のレシピ設定が必要で、単一の JSON(JavaScript Object Notation) ファイルにコンパイルされます。このレシピ設定のコンパイルは、設定ファイルと呼ばれます。 特定の設定のセットを含むパッケージ化されたレシピは、レシピインスタンスと呼ばれます。1 つのレシピを使用して、[!DNL Data Science Workspace] 内に多数のレシピインスタンスを作成できます。

パッケージレシピをインポートするワークフローは、次の手順で構成されます。
- [レシピの設定](#configure)
- [Docker ベースのレシピのインポート — Python](#python)
- [Docker ベースのレシピのインポート — R](#r)
- [Docker ベースのレシピのインポート — PySpark](#pyspark)
- [Docker ベースのレシピのインポート — Scala](#scala)

### レシピの設定 {#configure}

[!DNL Data Science Workspace] の各レシピインスタンスには、特定の使用例に合わせてレシピインスタンスをカスタマイズする一連の設定が付属しています。 設定ファイルは、このレシピインスタンスを使用して作成されたモデルのデフォルトのトレーニングおよびスコアリング動作を定義します。

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

このチュートリアルの目的では、 [!DNL Data Science Workspace] 「リテール販売」レシピのデフォルトの設定ファイルをそのまま使用できます。

### Docker ベースのレシピのインポート — [!DNL Python] {#python}

まず、[!DNL Platform] UI の左上にある「**[!UICONTROL ワークフロー]**」を選択します。 次に、「**レシピをインポート**」を選択し、「**[!UICONTROL Launch]**」を選択します。

![](../images/models-recipes/import-package-ui/launch-import.png)

**レシピのインポート** ワークフローの **** 設定ページが表示されます。 レシピの名前と説明を入力し、右上隅の「**[!UICONTROL 次へ]**」を選択します。

![ワークフローの設定](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
>
> 『[ソースファイルをレシピにパッケージ化](./package-source-files-recipe.md)』チュートリアルでは、Python ソースファイルを使用して Retail Sales レシピを作成する最後に Docker URL が提供されていました。

**ソース** を選択ページで、[!DNL Python] ソースファイルを使用して作成したパッケージレシピに対応する Docker URL を「**[!UICONTROL ソース URL]**」フィールドに貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップするか、ファイルシステムの&#x200B;**ブラウザー**&#x200B;を使用してインポートします。提供された設定ファイルは `experience-platform-dsw-reference/recipes/python/retail/retail.config.json` にあります。「**Runtime**」ドロップダウンで「**[!UICONTROL Python]**」を選択し、「**Type**」ドロップダウンで「**[!UICONTROL Classification]**」を選択します。 すべてを入力したら、右上隅の「**[!UICONTROL 次へ]**」を選択して、「**スキーマの管理**」に進みます。

>[!NOTE]
>
> 型は **[!UICONTROL 分類]** と **[!UICONTROL 回帰]** をサポートします。 モデルがこれらのタイプのいずれにも該当しない場合は、**[!UICONTROL カスタム]** を選択します。

![](../images/models-recipes/import-package-ui/recipe_source_python.png)

次に、「**スキーマの管理**」セクションで「小売販売」の入出力スキーマを選択します。これらは、『[ 小売販売スキーマとデータセットの作成 ](../models-recipes/create-retails-sales-dataset.md)』チュートリアルで提供されたブートストラップスクリプトを使用して作成します。

![](../images/models-recipes/import-package-ui/recipe_schema.png)

「**機能の管理**」セクションで、スキーマビューアのテナント ID を選択して、「小売売上」入力スキーマを展開します。 目的の入出力機能をハイライト表示し、右側の&#x200B;**[!UICONTROL フィールドプロパティ]**&#x200B;ウィンドウで「**[!UICONTROL 入力機能]**」または「**[!UICONTROL ターゲット機能]**」を選択して、入力機能と出力機能を選択します。このチュートリアルの目的では、**[!UICONTROL weeklySales]** を&#x200B;**[!UICONTROL ターゲット機能]**、その他すべてを&#x200B;**[!UICONTROL 入力機能]**&#x200B;として設定します。「**[!UICONTROL 次へ]**」を選択して、新しく設定したレシピを確認します。

レシピを確認し、必要に応じて、設定を追加、変更または削除します。「**[!UICONTROL 完了]**」を選択して、レシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

[ 次の手順 ](#next-steps) に進み、新しく作成した「小売売上」レシピを使用して [!DNL Data Science Workspace] でモデルを作成する方法を確認します。

### Docker ベースのレシピのインポート — R {#r}

まず、[!DNL Platform] UI の左上にある「**[!UICONTROL ワークフロー]**」を選択します。 次に、「**レシピをインポート**」を選択し、「**[!UICONTROL Launch]**」を選択します。

![](../images/models-recipes/import-package-ui/launch-import.png)

**レシピのインポート** ワークフローの **** 設定ページが表示されます。 レシピの名前と説明を入力し、右上隅の「**[!UICONTROL 次へ]**」を選択します。

![ワークフローの設定](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
>
> 「[ソースファイルをレシピにパッケージ化](./package-source-files-recipe.md)」チュートリアルでは、R ソースファイルを使用して Retail Sales レシピを作成する最後に Docker URL が提供されていました。

「**ソースを選択**」ページで、R ソースファイルを使用して作成したパッケージレシピに対応する Docker URL を「**[!UICONTROL ソース URL]**」フィールドに貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップするか、ファイルシステムの&#x200B;**ブラウザー**&#x200B;を使用してインポートします。提供された設定ファイルは `experience-platform-dsw-reference/recipes/R/Retail\ -\ GradientBoosting/retail.config.json` にあります。「**Runtime**」ドロップダウンで「**[!UICONTROL R]**」を選択し、「**Type**」ドロップダウンで「**[!UICONTROL Classification]**」を選択します。 すべてを入力したら、右上隅の「**[!UICONTROL 次へ]**」を選択して、「**スキーマの管理**」に進みます。

>[!NOTE]
>
> ** 型は分類 **** と回 **[!UICONTROL 帰をサポート]**&#x200B;モデルがこれらのタイプのいずれにも該当しない場合は、**[!UICONTROL カスタム]** を選択します。

![](../images/models-recipes/import-package-ui/recipe_source_R.png)

次に、「**スキーマの管理**」セクションで「小売販売」の入出力スキーマを選択します。これらは、『[ 小売販売スキーマとデータセットの作成 ](../models-recipes/create-retails-sales-dataset.md)』チュートリアルで提供されたブートストラップスクリプトを使用して作成します。

![](../images/models-recipes/import-package-ui/recipe_schema.png)

「*機能の管理*」セクションで、スキーマビューアのテナント ID を選択して、「小売売上」入力スキーマを展開します。 目的の入出力機能をハイライト表示し、右側の&#x200B;**[!UICONTROL フィールドプロパティ]**&#x200B;ウィンドウで「**[!UICONTROL 入力機能]**」または「**[!UICONTROL ターゲット機能]**」を選択して、入力機能と出力機能を選択します。このチュートリアルの目的では、**[!UICONTROL weeklySales]** を&#x200B;**[!UICONTROL ターゲット機能]**、その他すべてを&#x200B;**[!UICONTROL 入力機能]**&#x200B;として設定します。「**[!UICONTROL 次へ]**」を選択して、新しく設定したレシピを確認します。

レシピを確認し、必要に応じて、設定を追加、変更または削除します。「**完了**」を選択して、レシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

[ 次の手順 ](#next-steps) に進み、新しく作成した「小売売上」レシピを使用して [!DNL Data Science Workspace] でモデルを作成する方法を確認します。

### Docker ベースのレシピのインポート — PySpark {#pyspark}

まず、[!DNL Platform] UI の左上にある「**[!UICONTROL ワークフロー]**」を選択します。 次に、「**レシピをインポート**」を選択し、「**[!UICONTROL Launch]**」を選択します。

![](../images/models-recipes/import-package-ui/launch-import.png)

**レシピのインポート** ワークフローの **** 設定ページが表示されます。 レシピの名前と説明を入力し、右上隅の「**[!UICONTROL 次へ]**」を選択して次に進みます。

![ワークフローの設定](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
>
> 『[ ソースファイルをレシピにパッケージ化 ](./package-source-files-recipe.md)』チュートリアルでは、PySpark ソースファイルを使用して Retail Sales レシピを作成する最後に Docker URL が提供されていました。

**ソースを選択** ページで、PySpark ソースファイルを使用して作成したパッケージレシピに対応する Docker URL を「**[!UICONTROL ソース URL]**」フィールドに貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップするか、ファイルシステムの&#x200B;**ブラウザー**&#x200B;を使用してインポートします。提供された設定ファイルは `experience-platform-dsw-reference/recipes/pyspark/retail/pipeline.json` にあります。**Runtime** ドロップダウンで **[!UICONTROL PySpark]** を選択します。 PySpark ランタイムを選択すると、デフォルトのアーティファクトが **[!UICONTROL Docker]** に自動入力されます。 次に、「**タイプ**」ドロップダウンで「**[!UICONTROL 分類]**」を選択します。 すべてを入力したら、右上隅の「**[!UICONTROL 次へ]**」を選択して、「**スキーマの管理**」に進みます。

>[!NOTE]
>
> ** 型は分類 **** と回 **[!UICONTROL 帰をサポート]**&#x200B;モデルがこれらのタイプのいずれにも該当しない場合は、**[!UICONTROL カスタム]** を選択します。

![](../images/models-recipes/import-package-ui/pyspark-databricks.png)

次に、**スキーマの管理** セレクターを使用して小売販売の入出力スキーマを選択します。スキーマは、『[ 小売販売スキーマとデータセットの作成 ](../models-recipes/create-retails-sales-dataset.md)』チュートリアルで提供されたブートストラップスクリプトを使用して作成しました。

![スキーマの管理](../images/models-recipes/import-package-ui/manage-schemas.png)

「**機能の管理**」セクションで、スキーマビューアのテナント ID を選択して、「小売売上」入力スキーマを展開します。 目的の入出力機能をハイライト表示し、右側の&#x200B;**[!UICONTROL フィールドプロパティ]**&#x200B;ウィンドウで「**[!UICONTROL 入力機能]**」または「**[!UICONTROL ターゲット機能]**」を選択して、入力機能と出力機能を選択します。このチュートリアルの目的では、**[!UICONTROL weeklySales]** を&#x200B;**[!UICONTROL ターゲット機能]**、その他すべてを&#x200B;**[!UICONTROL 入力機能]**&#x200B;として設定します。「**[!UICONTROL 次へ]**」を選択して、新しく設定したレシピを確認します。

![](../images/models-recipes/import-package-ui/recipe_schema.png)

レシピを確認し、必要に応じて、設定を追加、変更または削除します。「**[!UICONTROL 完了]**」を選択して、レシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

[ 次の手順 ](#next-steps) に進み、新しく作成した「小売売上」レシピを使用して [!DNL Data Science Workspace] でモデルを作成する方法を確認します。

### Docker ベースのレシピのインポート — Scala {#scala}

まず、[!DNL Platform] UI の左上にある「**[!UICONTROL ワークフロー]**」を選択します。 次に、「**レシピをインポート**」を選択し、「**[!UICONTROL Launch]**」を選択します。

![](../images/models-recipes/import-package-ui/launch-import.png)

**レシピのインポート** ワークフローの **** 設定ページが表示されます。 レシピの名前と説明を入力し、右上隅の「**[!UICONTROL 次へ]**」を選択して次に進みます。

![ワークフローの設定](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
>
> 『[ ソースファイルをレシピにパッケージ化 ](./package-source-files-recipe.md)』チュートリアルでは、Scala([!DNL Spark]) ソースファイルを使用して Retail Sales レシピを作成する最後に Docker URL が提供されていました。

「**ソースを選択**」ページで、Scala ソースファイルを使用して作成したパッケージレシピに対応する Docker URL を「ソース URL」フィールドに貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップするか、ファイルシステムのブラウザーを使用してインポートします。提供された設定ファイルは`experience-platform-dsw-reference/recipes/scala/retail/pipelineservice.json`にあります。**Runtime** ドロップダウンで **[!UICONTROL Spark]** を選択します。 [!DNL Spark] ランタイムを選択すると、デフォルトのアーティファクトが **[!UICONTROL Docker]** に自動入力されます。 次に、「**型**」ドロップダウンから「**[!UICONTROL 回帰]**」を選択します。 すべてを入力したら、右上隅の「**[!UICONTROL 次へ]**」を選択して、「**スキーマの管理**」に進みます。

>[!NOTE]
>
> 型は **[!UICONTROL 分類]** と **[!UICONTROL 回帰]** をサポートします。 モデルがこれらのタイプのいずれにも該当しない場合は、**[!UICONTROL カスタム]** を選択します。

![](../images/models-recipes/import-package-ui/scala-databricks.png)

次に、**スキーマの管理** セレクターを使用して小売販売の入出力スキーマを選択します。スキーマは、『[ 小売販売スキーマとデータセットの作成 ](../models-recipes/create-retails-sales-dataset.md)』チュートリアルで提供されたブートストラップスクリプトを使用して作成しました。

![スキーマの管理](../images/models-recipes/import-package-ui/manage-schemas.png)

「**機能の管理**」セクションで、スキーマビューアのテナント ID を選択して、「小売売上」入力スキーマを展開します。 目的の入出力機能をハイライト表示し、右側の&#x200B;**[!UICONTROL フィールドプロパティ]**&#x200B;ウィンドウで「**[!UICONTROL 入力機能]**」または「**[!UICONTROL ターゲット機能]**」を選択して、入力機能と出力機能を選択します。このチュートリアルの目的では、「[!UICONTROL weeklySales]」を **[!UICONTROL ターゲット機能]**、その他すべてを **[!UICONTROL 入力機能]** に設定します。 「**[!UICONTROL 次へ]**」を選択して、新しく設定したレシピを確認します。

![](../images/models-recipes/import-package-ui/recipe_schema.png)

レシピを確認し、必要に応じて、設定を追加、変更または削除します。「**[!UICONTROL 完了]**」を選択して、レシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

[ 次の手順 ](#next-steps) に進み、新しく作成した「小売売上」レシピを使用して [!DNL Data Science Workspace] でモデルを作成する方法を確認します。

## 次の手順 {#next-steps}

このチュートリアルでは、[!DNL Data Science Workspace] へのレシピの設定とインポートに関するインサイトを提供しました。 新しく作成したレシピを使用して、モデルの作成、トレーニング、評価をおこなうことができるようになりました。

- [UI でのモデルのトレーニングと評価](./train-evaluate-model-ui.md)
- [API を使用したモデルのトレーニングと評価](./train-evaluate-model-api.md)
