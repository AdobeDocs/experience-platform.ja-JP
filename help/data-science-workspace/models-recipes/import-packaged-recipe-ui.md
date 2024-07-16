---
keywords: Experience Platform；パッケージ化されたレシピの読み込み；Data Science Workspace；人気のトピック；レシピ；ui;create engine
solution: Experience Platform
title: Data Science Workspace UI へのパッケージ化されたレシピの読み込み
type: Tutorial
description: このチュートリアルでは、提供された「小売売上」の例を使用してパッケージ化されたレシピを設定およびインポートする方法に関するインサイトを提供します。このチュートリアルの最後までに、Adobe Experience Platform Data Science Workspace　でモデルの作成、トレーニング、評価をおこなう準備が整います。
exl-id: 2556e1f0-3f9c-4884-a699-06c041d5c4d1
source-git-commit: 81f48de908b274d836f551bec5693de13c5edaf1
workflow-type: tm+mt
source-wordcount: '1832'
ht-degree: 32%

---

# Data Science Workspace UI へのパッケージ化されたレシピの読み込み

このチュートリアルでは、提供された「小売売上」の例を使用してパッケージ化されたレシピを設定およびインポートする方法に関するインサイトを提供します。このチュートリアルを終了すると、Adobe Experience Platform [!DNL Data Science Workspace] でモデルを作成、トレーニング、評価する準備が整います。

## 前提条件

このチュートリアルでは、Docker イメージ URL の形式でパッケージ化されたレシピが必要です。 詳しくは、[ソースファイルをレシピにパッケージ化する](./package-source-files-recipe.md)方法に関するチュートリアルを参照してください。

## UI ワークフロー

パッケージ化されたレシピを [!DNL Data Science Workspace] に読み込むには、特定のレシピ設定が必要で、そのレシピ設定は単一のJavaScript Object Notation （JSON）ファイルにコンパイルされます。このレシピ設定のコンパイルは、設定ファイルと呼ばれます。 特定の設定セットを使用してパッケージ化されたレシピは、レシピインスタンスと呼ばれます。 1 つのレシピを使用して、[!DNL Data Science Workspace] で多数のレシピインスタンスを作成できます。

パッケージレシピをインポートするワークフローは、次の手順で構成されます。
- [レシピの設定](#configure)
- [Docker ベースのレシピの読み込み – Python](#python)
- [Docker ベースのレシピを読み込み – R](#r)
- [Docker ベースのレシピを読み込む – PySpark](#pyspark)
- [Docker ベースのレシピの読み込み – Scala](#scala)

### レシピの設定 {#configure}

[!DNL Data Science Workspace] のすべてのレシピインスタンスには、特定のユースケースに合わせてレシピインスタンスを調整する一連の設定が付属しています。 設定ファイルは、このレシピインスタンスを使用して作成されたモデルのデフォルトのトレーニングおよびスコアリング動作を定義します。

>[!NOTE]
>
>設定ファイルは、レシピとケースに固有です。

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
| `tenantId` | 文字列 | この ID により、作成したリソースの名前空間が適切に設定され、組織内に含まれるようになります。 テナント ID を検索するには、[こちらの手順](../../xdm/api/getting-started.md#know-your-tenant_id)に従います。 |
| `ACP_DSW_TRAINING_XDM_SCHEMA` | 文字列 | モデルのトレーニングに使用する入力スキーマ。UI にインポートする場合は空のままにし、API を使用してインポートする場合はトレーニングスキーマ ID に置き換えます。 |
| `evaluation.labelColumn` | 文字列 | 評価のビジュアライゼーションの列ラベル |
| `evaluation.metrics` | 文字列 | モデルの評価に使用される評価指標のカンマ区切りのリスト |
| `ACP_DSW_SCORING_RESULTS_XDM_SCHEMA` | 文字列 | モデルのスコアリングに使用される出力スキーマUI にインポートする場合は空のままにし、API を使用してインポートする場合はスコアリングスキーマ ID に置き換えます。 |

このチュートリアルでは、小売販売レシピのデフォルトの設定ファイルをそのままの状態で [!DNL Data Science Workspace] Reference に残すことができます。

### Docker ベースのレシピの読み込み – [!DNL Python] {#python}

まず、[!DNL Platform] UI の左上にある **[!UICONTROL ワークフロー]** に移動して選択します。 次に、「**レシピを読み込み**」を選択し、「**[!UICONTROL ローンチ]**」を選択します。

![](../images/models-recipes/import-package-ui/launch-import.png)

**レシピを読み込み** ワークフローの **設定** ページが表示されます。 レシピの名前と説明を入力し、右上隅にある「**[!UICONTROL 次へ]**」を選択します。

![ ワークフローの設定 ](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
>
> 『[ソースファイルをレシピにパッケージ化](./package-source-files-recipe.md)』チュートリアルでは、Python ソースファイルを使用して Retail Sales レシピを作成する最後に Docker URL が提供されていました。

**ソースを選択** ページが表示されたら、「**[!UICONTROL Source URL]**」フィールドに、[!DNL Python] ソースファイルを使用して作成されたパッケージ化されたレシピに対応する Docker URL を貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップするか、ファイルシステムの&#x200B;**ブラウザー**&#x200B;を使用してインポートします。提供された設定ファイルは `experience-platform-dsw-reference/recipes/python/retail/retail.config.json` にあります。「**Runtime]**」ドロップダウンで **[!UICONTROL Python** を選択し、「**[!UICONTROL タイプ]**」ドロップダウンで **分類** を選択します。 すべてが入力されたら、右上隅にある **[!UICONTROL 次へ]** を選択して、**スキーマの管理** に進みます。

>[!NOTE]
>
> タイプは、**[!UICONTROL 分類]** および **[!UICONTROL 回帰]** をサポートします。 モデルがこれらのタイプに該当しない場合は、「**[!UICONTROL カスタム]**」を選択します。

![](../images/models-recipes/import-package-ui/recipe_source_python.png)

次に、「**スキーマの管理** のセクションで小売販売の入力および出力スキーマを選択します。これらは、「小売販売のスキーマおよびデータセットの作成 [ チュートリアルで提供される bootstrap スクリプトを使用して作成され ](../models-recipes/create-retails-sales-dataset.md) した。

![](../images/models-recipes/import-package-ui/recipe_schema.png)

「**機能管理**」セクションで、スキーマビューアのテナント ID を選択して、小売販売入力スキーマを展開します。 目的の入出力機能をハイライト表示し、右側の&#x200B;**[!UICONTROL フィールドプロパティ]**&#x200B;ウィンドウで「**[!UICONTROL 入力機能]**」または「**[!UICONTROL ターゲット機能]**」を選択して、入力機能と出力機能を選択します。このチュートリアルの目的では、**[!UICONTROL weeklySales]** を&#x200B;**[!UICONTROL ターゲット機能]**、その他すべてを&#x200B;**[!UICONTROL 入力機能]**&#x200B;として設定します。「**[!UICONTROL 次へ]**」を選択して、新しく設定したレシピを確認します。

レシピを確認し、必要に応じて、設定を追加、変更または削除します。「**[!UICONTROL 終了]**」を選択して、レシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

[ 次の手順 ](#next-steps) に進み、新しく作成した小売販売レシピを使用して [!DNL Data Science Workspace] でモデルを作成する方法を確認します。

### Docker ベースのレシピを読み込み – R {#r}

まず、[!DNL Platform] UI の左上にある **[!UICONTROL ワークフロー]** に移動して選択します。 次に、「**レシピを読み込み**」を選択し、「**[!UICONTROL ローンチ]**」を選択します。

![](../images/models-recipes/import-package-ui/launch-import.png)

**レシピを読み込み** ワークフローの **設定** ページが表示されます。 レシピの名前と説明を入力し、右上隅にある「**[!UICONTROL 次へ]**」を選択します。

![ ワークフローの設定 ](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
>
> 「[ソースファイルをレシピにパッケージ化](./package-source-files-recipe.md)」チュートリアルでは、R ソースファイルを使用して Retail Sales レシピを作成する最後に Docker URL が提供されていました。

**ソースを選択** ページが表示されたら、「**[!UICONTROL Source URL]**」フィールドに、R ソースファイルを使用して作成されたパッケージ化されたレシピに対応する Docker URL を貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップするか、ファイルシステムの&#x200B;**ブラウザー**&#x200B;を使用してインポートします。提供された設定ファイルは `experience-platform-dsw-reference/recipes/R/Retail\ -\ GradientBoosting/retail.config.json` にあります。「**ランタイム]**」ドロップダウンで「**[!UICONTROL R**」を選択し、「**[!UICONTROL タイプ]**」ドロップダウンで「**分類**」を選択します。 すべてが入力されたら、右上隅にある **[!UICONTROL 次へ]** を選択して、**スキーマの管理** に進みます。

>[!NOTE]
>
> *タイプ* は、**[!UICONTROL 分類]** および **[!UICONTROL 回帰]** をサポートします。 モデルがこれらのタイプに該当しない場合は、「**[!UICONTROL カスタム]**」を選択します。

![](../images/models-recipes/import-package-ui/recipe_source_R.png)

次に、「**スキーマの管理** のセクションで小売販売の入力および出力スキーマを選択します。これらは、「小売販売のスキーマおよびデータセットの作成 [ チュートリアルで提供される bootstrap スクリプトを使用して作成され ](../models-recipes/create-retails-sales-dataset.md) した。

![](../images/models-recipes/import-package-ui/recipe_schema.png)

「*機能管理*」セクションで、スキーマビューアのテナント ID を選択して、小売販売入力スキーマを展開します。 目的の入出力機能をハイライト表示し、右側の&#x200B;**[!UICONTROL フィールドプロパティ]**&#x200B;ウィンドウで「**[!UICONTROL 入力機能]**」または「**[!UICONTROL ターゲット機能]**」を選択して、入力機能と出力機能を選択します。このチュートリアルの目的では、**[!UICONTROL weeklySales]** を&#x200B;**[!UICONTROL ターゲット機能]**、その他すべてを&#x200B;**[!UICONTROL 入力機能]**&#x200B;として設定します。**[!UICONTROL 次へ]** を選択して、新しい設定済みレシピを確認します。

レシピを確認し、必要に応じて、設定を追加、変更または削除します。「**終了**」を選択して、レシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

[ 次の手順 ](#next-steps) に進み、新しく作成した小売販売レシピを使用して [!DNL Data Science Workspace] でモデルを作成する方法を確認します。

### Docker ベースのレシピを読み込む – PySpark {#pyspark}

まず、[!DNL Platform] UI の左上にある **[!UICONTROL ワークフロー]** に移動して選択します。 次に、「**レシピを読み込み**」を選択し、「**[!UICONTROL ローンチ]**」を選択します。

![](../images/models-recipes/import-package-ui/launch-import.png)

**レシピを読み込み** ワークフローの **設定** ページが表示されます。 レシピの名前と説明を入力し、右上隅にある「**[!UICONTROL 次へ]**」を選択して続行します。

![ ワークフローの設定 ](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
>
> [ ソースファイルをレシピにパッケージ化 ](./package-source-files-recipe.md) チュートリアルでは、PySpark ソースファイルを使用して小売販売レシピを作成する際の最後に、Docker URL が提供されました。

**ソースを選択** ページが表示されたら、「**[!UICONTROL Source URL]**」フィールドに、PySpark ソースファイルを使用して作成されたパッケージ化されたレシピに対応する Docker URL を貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップするか、ファイルシステムの&#x200B;**ブラウザー**&#x200B;を使用してインポートします。提供された設定ファイルは `experience-platform-dsw-reference/recipes/pyspark/retail/pipeline.json` にあります。「**[!UICONTROL ランタイム]**」ドロップダウンで **PySpark** を選択します。 PySpark ランタイムが選択されると、デフォルトのアーティファクトが **[!UICONTROL Docker]** に自動入力されます。 次に、「**[!UICONTROL タイプ]**」ドロップダウンで **分類** を選択します。 すべてが入力されたら、右上隅にある **[!UICONTROL 次へ]** を選択して、**スキーマの管理** に進みます。

>[!NOTE]
>
> *タイプ* は、**[!UICONTROL 分類]** および **[!UICONTROL 回帰]** をサポートします。 モデルがこれらのタイプに該当しない場合は、「**[!UICONTROL カスタム]**」を選択します。

![](../images/models-recipes/import-package-ui/pyspark-databricks.png)

次に、**スキーマの管理** セレクターを使用して小売販売の入力および出力スキーマを選択します。スキーマは、「小売販売のスキーマおよびデータセットの作成 [ チュートリアルで提供される bootstrap スクリプトを使用して作成され ](../models-recipes/create-retails-sales-dataset.md) した。

![ スキーマの管理 ](../images/models-recipes/import-package-ui/manage-schemas.png)

「**機能管理**」セクションで、スキーマビューアのテナント ID を選択して、小売販売入力スキーマを展開します。 目的の入出力機能をハイライト表示し、右側の&#x200B;**[!UICONTROL フィールドプロパティ]**&#x200B;ウィンドウで「**[!UICONTROL 入力機能]**」または「**[!UICONTROL ターゲット機能]**」を選択して、入力機能と出力機能を選択します。このチュートリアルの目的では、**[!UICONTROL weeklySales]** を&#x200B;**[!UICONTROL ターゲット機能]**、その他すべてを&#x200B;**[!UICONTROL 入力機能]**&#x200B;として設定します。「**[!UICONTROL 次へ]**」を選択して、新しく設定したレシピを確認します。

![](../images/models-recipes/import-package-ui/recipe_schema.png)

レシピを確認し、必要に応じて、設定を追加、変更または削除します。「**[!UICONTROL 終了]**」を選択して、レシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

[ 次の手順 ](#next-steps) に進み、新しく作成した小売販売レシピを使用して [!DNL Data Science Workspace] でモデルを作成する方法を確認します。

### Docker ベースのレシピの読み込み – Scala {#scala}

まず、[!DNL Platform] UI の左上にある **[!UICONTROL ワークフロー]** に移動して選択します。 次に、「**レシピを読み込み**」を選択し、「**[!UICONTROL ローンチ]**」を選択します。

![](../images/models-recipes/import-package-ui/launch-import.png)

**レシピを読み込み** ワークフローの **設定** ページが表示されます。 レシピの名前と説明を入力し、右上隅にある「**[!UICONTROL 次へ]**」を選択して続行します。

![ ワークフローの設定 ](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
>
> [ ソースファイルをレシピにパッケージ化 ](./package-source-files-recipe.md) チュートリアルでは、Scala （[!DNL Spark]）ソースファイルを使用して小売販売レシピを作成する際の最後に Docker URL が提供されました。

**ソースを選択** ページが表示されたら、Scala ソースファイルを使用して作成されたパッケージ化されたレシピに対応する Docker URL を「Source URL」フィールドに貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップで読み込むか、ファイルシステムブラウザーを使用します。 提供された設定ファイルは`experience-platform-dsw-reference/recipes/scala/retail/pipelineservice.json`にあります。**[!UICONTROL Runtime]** ドロップダウンで **Spark** を選択します。 [!DNL Spark] ランタイムが選択されると、デフォルトのアーティファクトが **[!UICONTROL Docker]** に自動入力されます。 次に、「**[!UICONTROL タイプ]**」ドロップダウンから **リグレッション** を選択します。 すべてが入力されたら、右上隅にある **[!UICONTROL 次へ]** を選択して、**スキーマの管理** に進みます。

>[!NOTE]
>
> タイプは、**[!UICONTROL 分類]** および **[!UICONTROL 回帰]** をサポートします。 モデルがこれらのタイプに該当しない場合は、「**[!UICONTROL カスタム]**」を選択します。

![](../images/models-recipes/import-package-ui/scala-databricks.png)

次に、**スキーマの管理** セレクターを使用して小売販売の入力および出力スキーマを選択します。スキーマは、「小売販売のスキーマおよびデータセットの作成 [ チュートリアルで提供される bootstrap スクリプトを使用して作成され ](../models-recipes/create-retails-sales-dataset.md) した。

![ スキーマの管理 ](../images/models-recipes/import-package-ui/manage-schemas.png)

「**機能管理**」セクションで、スキーマビューアのテナント ID を選択して、小売販売入力スキーマを展開します。 目的の入出力機能をハイライト表示し、右側の&#x200B;**[!UICONTROL フィールドプロパティ]**&#x200B;ウィンドウで「**[!UICONTROL 入力機能]**」または「**[!UICONTROL ターゲット機能]**」を選択して、入力機能と出力機能を選択します。このチュートリアルでは、「[!UICONTROL weeklySales]」を **[!UICONTROL Target 機能]** それ以外を **[!UICONTROL 入力機能]** に設定します。 「**[!UICONTROL 次へ]**」を選択して、新しく設定したレシピを確認します。

![](../images/models-recipes/import-package-ui/recipe_schema.png)

レシピを確認し、必要に応じて、設定を追加、変更または削除します。「**[!UICONTROL 終了]**」を選択して、レシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

[ 次の手順 ](#next-steps) に進み、新しく作成した小売販売レシピを使用して [!DNL Data Science Workspace] でモデルを作成する方法を確認します。

## 次の手順 {#next-steps}

このチュートリアルでは、レシピの設定と [!DNL Data Science Workspace] への読み込みに関するインサイトを提供しました。 新しく作成したレシピを使用して、モデルの作成、トレーニング、評価をおこなうことができるようになりました。

- [UI でのモデルのトレーニングと評価](./train-evaluate-model-ui.md)
- [API を使用したモデルのトレーニングと評価](./train-evaluate-model-api.md)
