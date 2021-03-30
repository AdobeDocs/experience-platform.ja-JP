---
keywords: Experience Platform；パッケージレシピの読み込み；Data Science Workspace；人気の高いトピック；レシピ；ui；エンジンの作成
solution: Experience Platform
title: Data Science Workspace UIでのパッケージ化されたレシピの読み込み
topic: チュートリアル
type: チュートリアル
description: このチュートリアルでは、提供された「小売売上」の例を使用してパッケージ化されたレシピを設定およびインポートする方法に関するインサイトを提供します。このチュートリアルの最後までに、Adobe Experience Platform Data Science Workspace　でモデルの作成、トレーニング、評価をおこなう準備が整います。
translation-type: tm+mt
source-git-commit: 13fa4af388c6f31768a6b7e1da05cb56c5635c9e
workflow-type: tm+mt
source-wordcount: '1835'
ht-degree: 37%

---


# Data Science Workspace UIでパッケージ化されたレシピを読み込む

このチュートリアルでは、提供された「小売売上」の例を使用してパッケージ化されたレシピを設定およびインポートする方法に関するインサイトを提供します。このチュートリアルを終えるまでに、Adobe Experience Platform[!DNL Data Science Workspace]でモデルを作成、トレーニング、評価する準備が整います。

## 前提条件

このチュートリアルでは、DockerイメージURLの形式でパッケージ化されたレシピが必要です。 詳しくは、[ソースファイルをレシピにパッケージ化する](./package-source-files-recipe.md)方法に関するチュートリアルを参照してください。

## UI ワークフロー

パッケージ化されたレシピを[!DNL Data Science Workspace]に読み込むには、特定のレシピ設定が必要で、1つのJavaScript Object Notation(JSON)ファイルにコンパイルされている必要があります。このレシピ設定のコンパイルを設定ファイルと呼びます。 特定の設定のセットを含むパッケージ化されたレシピは、レシピインスタンスと呼ばれます。1つのレシピを使用して、多くのレシピインスタンスを[!DNL Data Science Workspace]に作成できます。

パッケージレシピをインポートするワークフローは、次の手順で構成されます。
- [レシピの設定](#configure)
- [Docker ベースのレシピのインポート — Python](#python)
- [Docker ベースのレシピのインポート — R](#r)
- [Dockerベースのレシピのインポート — PySpark](#pyspark)
- [Dockerベースのレシピのインポート — Scala](#scala)

### レシピの設定 {#configure}

[!DNL Data Science Workspace]内の各レシピインスタンスには一連の設定が付属し、特定の使用事例に合わせてレシピインスタンスを調整します。 設定ファイルは、このレシピインスタンスを使用して作成されたモデルのデフォルトのトレーニングおよびスコアリング動作を定義します。

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

このチュートリアルでは、小売販売のレシピのデフォルトの設定ファイルをそのまま使用する方法について、[!DNL Data Science Workspace]参照ページに記載されています。

### Dockerベースのレシピのインポート — [!DNL Python] {#python}

[!DNL Platform] UIの左上にある&#x200B;**[!UICONTROL ワークフロー]**&#x200B;を移動して選択すると開始します。 次に、「**レシピを読み込み**」を選択し、「****&#x200B;を起動」を選択します。

![](../images/models-recipes/import-package-ui/launch-import.png)

****&#x200B;読み込みレシピ&#x200B;**ワークフローの設定**&#x200B;ページが表示されます。 レシピの名前と説明を入力し、右上隅の「**[!UICONTROL 次へ]**」を選択します。

![ワークフローの設定](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
>
> 『[ソースファイルをレシピにパッケージ化](./package-source-files-recipe.md)』チュートリアルでは、Python ソースファイルを使用して Retail Sales レシピを作成する最後に Docker URL が提供されていました。

**ソース**&#x200B;を選択ページに移動したら、[!DNL Python]ソースファイルを使用して作成したパッケージ化されたレシピに対応するDocker URLを&#x200B;**[!UICONTROL ソースURL]**&#x200B;フィールドに貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップするか、ファイルシステムの&#x200B;**ブラウザー**&#x200B;を使用してインポートします。提供された設定ファイルは `experience-platform-dsw-reference/recipes/python/retail/retail.config.json` にあります。**ランタイム**&#x200B;ドロップダウンで&#x200B;**[!UICONTROL Python]**&#x200B;を選択し、**タイプ**&#x200B;ドロップダウンで&#x200B;**[!UICONTROL 分類]**&#x200B;を選択します。 すべての情報が入力されたら、右上隅の「**[!UICONTROL 次へ]**」を選択して、「**スキーマの管理**」に進みます。

>[!NOTE]
>
> 型は、**[!UICONTROL 分類]**&#x200B;と&#x200B;**[!UICONTROL 回帰]**&#x200B;をサポートします。 モデルがこれらのタイプのいずれにも該当しない場合は、**[!UICONTROL カスタム]**&#x200B;を選択します。

![](../images/models-recipes/import-package-ui/recipe_source_python.png)

次に、**スキーマの管理**&#x200B;セクションの「小売売上高の入出力スキーマー」を選択します。これらのパラメーターは、[小売売上スキーマーとデータセット](../models-recipes/create-retails-sales-dataset.md)の作成チュートリアルの提供されたブートストラップスクリプトを使用して作成されます。

![](../images/models-recipes/import-package-ui/recipe_schema.png)

「**機能の管理**」セクションで、スキーマビューアのテナントIDを選択し、小売売上高の入力スキーマを展開します。 目的の入出力機能をハイライト表示し、右側の&#x200B;**[!UICONTROL フィールドプロパティ]**&#x200B;ウィンドウで「**[!UICONTROL 入力機能]**」または「**[!UICONTROL ターゲット機能]**」を選択して、入力機能と出力機能を選択します。このチュートリアルの目的では、**[!UICONTROL weeklySales]** を&#x200B;**[!UICONTROL ターゲット機能]**、その他すべてを&#x200B;**[!UICONTROL 入力機能]**&#x200B;として設定します。「**[!UICONTROL 次へ]**」を選択して、新しく設定したレシピを確認します。

レシピを確認し、必要に応じて、設定を追加、変更または削除します。「**[!UICONTROL 完了]**」を選択して、レシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

[次の手順](#next-steps)に進み、新しく作成した小売売上高のレシピを使用して[!DNL Data Science Workspace]内にモデルを作成する方法を調べます。

### Docker ベースのレシピのインポート — R {#r}

[!DNL Platform] UIの左上にある&#x200B;**[!UICONTROL ワークフロー]**&#x200B;を移動して選択すると開始します。 次に、「**レシピを読み込み**」を選択し、「****&#x200B;を起動」を選択します。

![](../images/models-recipes/import-package-ui/launch-import.png)

****&#x200B;読み込みレシピ&#x200B;**ワークフローの設定**&#x200B;ページが表示されます。 レシピの名前と説明を入力し、右上隅の「**[!UICONTROL 次へ]**」を選択します。

![ワークフローの設定](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
>
> 「[ソースファイルをレシピにパッケージ化](./package-source-files-recipe.md)」チュートリアルでは、R ソースファイルを使用して Retail Sales レシピを作成する最後に Docker URL が提供されていました。

**ソース**&#x200B;を選択ページに移動したら、Rソースファイルを使用して作成したパッケージ化レシピに対応するDocker URLを&#x200B;**[!UICONTROL ソースURL]**&#x200B;フィールドに貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップするか、ファイルシステムの&#x200B;**ブラウザー**&#x200B;を使用してインポートします。提供された設定ファイルは `experience-platform-dsw-reference/recipes/R/Retail\ -\ GradientBoosting/retail.config.json` にあります。**ランタイム**&#x200B;ドロップダウンで&#x200B;**[!UICONTROL R]**&#x200B;を選択し、**タイプ**&#x200B;ドロップダウンで&#x200B;**[!UICONTROL 分類]**&#x200B;を選択します。 すべての情報が入力されたら、右上隅の「**[!UICONTROL 次へ]**」を選択して、「**スキーマの管理**」に進みます。

>[!NOTE]
>
> ** 型は、 **** 分類と **[!UICONTROL 回帰をサポートします]**。モデルがこれらのタイプのいずれにも該当しない場合は、**[!UICONTROL カスタム]**&#x200B;を選択します。

![](../images/models-recipes/import-package-ui/recipe_source_R.png)

次に、**スキーマの管理**&#x200B;セクションの「小売売上高の入出力スキーマー」を選択します。これらのパラメーターは、[小売売上スキーマーとデータセット](../models-recipes/create-retails-sales-dataset.md)の作成チュートリアルの提供されたブートストラップスクリプトを使用して作成されます。

![](../images/models-recipes/import-package-ui/recipe_schema.png)

「*機能の管理*」セクションで、スキーマビューアのテナントIDを選択し、小売売上高の入力スキーマを展開します。 目的の入出力機能をハイライト表示し、右側の&#x200B;**[!UICONTROL フィールドプロパティ]**&#x200B;ウィンドウで「**[!UICONTROL 入力機能]**」または「**[!UICONTROL ターゲット機能]**」を選択して、入力機能と出力機能を選択します。このチュートリアルの目的では、**[!UICONTROL weeklySales]** を&#x200B;**[!UICONTROL ターゲット機能]**、その他すべてを&#x200B;**[!UICONTROL 入力機能]**&#x200B;として設定します。「**[!UICONTROL 次へ]**」を選択して、新しい設定済みレシピを確認します。

レシピを確認し、必要に応じて、設定を追加、変更または削除します。「**完了**」を選択して、レシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

[次の手順](#next-steps)に進み、新しく作成した小売売上高のレシピを使用して[!DNL Data Science Workspace]内にモデルを作成する方法を調べます。

### Dockerベースのレシピのインポート — PySpark {#pyspark}

[!DNL Platform] UIの左上にある&#x200B;**[!UICONTROL ワークフロー]**&#x200B;を移動して選択すると開始します。 次に、「**レシピを読み込み**」を選択し、「****&#x200B;を起動」を選択します。

![](../images/models-recipes/import-package-ui/launch-import.png)

****&#x200B;読み込みレシピ&#x200B;**ワークフローの設定**&#x200B;ページが表示されます。 レシピの名前と説明を入力し、右上隅の「**[!UICONTROL 次へ]**」を選択して次に進みます。

![ワークフローの設定](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
>
> [ソースファイルをRecipe](./package-source-files-recipe.md)チュートリアルにパッケージ化すると、PySparkソースファイルを使用して小売売上のレシピを作成する際に、最後にDocker URLが提供されます。

**ソース**&#x200B;を選択ページに移動したら、PySparkソースファイルを使って作成したパッケージレシピに対応するDocker URLを&#x200B;**[!UICONTROL Source URL]**&#x200B;フィールドに貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップするか、ファイルシステムの&#x200B;**ブラウザー**&#x200B;を使用してインポートします。提供された設定ファイルは `experience-platform-dsw-reference/recipes/pyspark/retail/pipeline.json` にあります。**Runtime**&#x200B;ドロップダウンで&#x200B;**[!UICONTROL PySpark]**&#x200B;を選択します。 PySparkランタイムが選択されると、デフォルトのアーティファクトは&#x200B;**[!UICONTROL Docker]**&#x200B;に自動入力されます。 次に、**タイプ**&#x200B;ドロップダウンで&#x200B;**[!UICONTROL 分類]**&#x200B;を選択します。 すべての情報が入力されたら、右上隅の「**[!UICONTROL 次へ]**」を選択して、「**スキーマの管理**」に進みます。

>[!NOTE]
>
> ** 型は、 **** 分類と **[!UICONTROL 回帰をサポートします]**。モデルがこれらのタイプのいずれにも該当しない場合は、**[!UICONTROL カスタム]**&#x200B;を選択します。

![](../images/models-recipes/import-package-ui/pyspark-databricks.png)

次に、**スキーマの管理**&#x200B;セレクターを使用して小売売上の入出力スキーマーを選択し、[小売売上スキーマとデータセット](../models-recipes/create-retails-sales-dataset.md)の作成チュートリアルで提供されたブートストラップスクリプトを使用してスキーマーを作成します。

![スキーマの管理](../images/models-recipes/import-package-ui/manage-schemas.png)

「**機能の管理**」セクションで、スキーマビューアのテナントIDを選択し、小売売上高の入力スキーマを展開します。 目的の入出力機能をハイライト表示し、右側の&#x200B;**[!UICONTROL フィールドプロパティ]**&#x200B;ウィンドウで「**[!UICONTROL 入力機能]**」または「**[!UICONTROL ターゲット機能]**」を選択して、入力機能と出力機能を選択します。このチュートリアルの目的では、**[!UICONTROL weeklySales]** を&#x200B;**[!UICONTROL ターゲット機能]**、その他すべてを&#x200B;**[!UICONTROL 入力機能]**&#x200B;として設定します。「**[!UICONTROL 次へ]**」を選択して、新しく設定したレシピを確認します。

![](../images/models-recipes/import-package-ui/recipe_schema.png)

レシピを確認し、必要に応じて、設定を追加、変更または削除します。「**[!UICONTROL 完了]**」を選択して、レシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

[次の手順](#next-steps)に進み、新しく作成した小売売上高のレシピを使用して[!DNL Data Science Workspace]内にモデルを作成する方法を調べます。

### Dockerベースのレシピのインポート — Scala {#scala}

[!DNL Platform] UIの左上にある&#x200B;**[!UICONTROL ワークフロー]**&#x200B;を移動して選択すると開始します。 次に、「**レシピを読み込み**」を選択し、「****&#x200B;を起動」を選択します。

![](../images/models-recipes/import-package-ui/launch-import.png)

****&#x200B;読み込みレシピ&#x200B;**ワークフローの設定**&#x200B;ページが表示されます。 レシピの名前と説明を入力し、右上隅の「**[!UICONTROL 次へ]**」を選択して次に進みます。

![ワークフローの設定](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
>
> [ソースファイルをレシピ](./package-source-files-recipe.md)にパッケージ化するチュートリアルでは、Scala ([!DNL Spark])ソースファイルを使用して小売売上のレシピを作成する最後にDocker URLが提供されています。

「**ソース**&#x200B;を選択」ページに移動したら、「ソースURL」フィールドに、Scalaソースファイルを使用して作成したパッケージレシピに対応するDocker URLを貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップするか、ファイルシステムのブラウザーを使用してインポートします。提供された設定ファイルは`experience-platform-dsw-reference/recipes/scala/retail/pipelineservice.json`にあります。**ランタイム**&#x200B;ドロップダウンで、**[!UICONTROL Spark]**&#x200B;を選択します。 [!DNL Spark]ランタイムが選択されると、デフォルトのアーチファクトが&#x200B;**[!UICONTROL Docker]**&#x200B;に自動入力されます。 次に、**タイプ**&#x200B;ドロップダウンから&#x200B;**[!UICONTROL 回帰]**&#x200B;を選択します。 すべての情報が入力されたら、右上隅の「**[!UICONTROL 次へ]**」を選択して、「**スキーマの管理**」に進みます。

>[!NOTE]
>
> 型は、**[!UICONTROL 分類]**&#x200B;と&#x200B;**[!UICONTROL 回帰]**&#x200B;をサポートします。 モデルがこれらのタイプのいずれにも該当しない場合は、**[!UICONTROL カスタム]**&#x200B;を選択します。

![](../images/models-recipes/import-package-ui/scala-databricks.png)

次に、**スキーマの管理**&#x200B;セレクターを使用して小売売上の入出力スキーマーを選択し、[小売売上スキーマとデータセット](../models-recipes/create-retails-sales-dataset.md)の作成チュートリアルで提供されたブートストラップスクリプトを使用してスキーマーを作成します。

![スキーマの管理](../images/models-recipes/import-package-ui/manage-schemas.png)

「**機能の管理**」セクションで、スキーマビューアのテナントIDを選択し、小売売上高の入力スキーマを展開します。 目的の入出力機能をハイライト表示し、右側の&#x200B;**[!UICONTROL フィールドプロパティ]**&#x200B;ウィンドウで「**[!UICONTROL 入力機能]**」または「**[!UICONTROL ターゲット機能]**」を選択して、入力機能と出力機能を選択します。このチュートリアルでは、&quot;[!UICONTROL weeklySales]&quot;を&#x200B;**[!UICONTROL ターゲット機能]**&#x200B;に、その他すべてを&#x200B;**[!UICONTROL 入力機能]**&#x200B;に設定します。 「**[!UICONTROL 次へ]**」を選択して、新しく設定したレシピを確認します。

![](../images/models-recipes/import-package-ui/recipe_schema.png)

レシピを確認し、必要に応じて、設定を追加、変更または削除します。「**[!UICONTROL 完了]**」を選択して、レシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

[次の手順](#next-steps)に進み、新しく作成した小売売上高のレシピを使用して[!DNL Data Science Workspace]内にモデルを作成する方法を調べます。

## 次の手順 {#next-steps}

このチュートリアルでは、[!DNL Data Science Workspace]にレシピを設定して読み込む方法について説明しました。 新しく作成したレシピを使用して、モデルの作成、トレーニング、評価をおこなうことができるようになりました。

- [UI でのモデルのトレーニングと評価](./train-evaluate-model-ui.md)
- [API を使用したモデルのトレーニングと評価](./train-evaluate-model-api.md)