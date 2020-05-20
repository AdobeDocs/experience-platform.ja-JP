---
keywords: Experience Platform;import packaged recipe;Data Science Workspace;popular topics
solution: Experience Platform
title: パッケージ化されたレシピの読み込み(UI)
topic: Tutorial
translation-type: tm+mt
source-git-commit: 19823c7cf0459e045366f0baae2bd8a98416154c
workflow-type: tm+mt
source-wordcount: '2451'
ht-degree: 1%

---


# パッケージ化されたレシピの読み込み(UI)

このチュートリアルでは、提供される小売売上の例を使用して、パッケージ化されたレシピを設定およびインポートする方法について説明します。 このチュートリアルを終了するまでに、Adobe Experience Platform Data Science Workspaceでモデルを作成、トレーニング、評価する準備が整います。

## 前提条件

このチュートリアルでは、DockerイメージURLの形式でパッケージ化されたレシピが必要です。 詳しくは、ソースファイルをレシピに [パッケージ化する方法のチュートリアルを参照してください](./package-source-files-recipe.md) 。

## UIワークフロー

パッケージ化されたレシピをData Science Workspaceに読み込むには、特定のレシピ設定を1つのJavaScript Object Notation(JSON)ファイルにコンパイルする必要があります。このレシピ設定のコンパイルを **設定ファイル**&#x200B;と呼びます。 特定の設定のセットを含むパッケージ化されたレシピは、 **レシピインスタンス**&#x200B;と呼ばれます。 1つのレシピを使用して、Data Science Workspaceで多数のレシピインスタンスを作成できます。

パッケージレシピを読み込むためのワークフローは、次の手順で構成されます。
- [レシピの設定](#configure)
- [Dockerベースのレシピのインポート — Python](#python)
- [Dockerベースのレシピのインポート — R](#r)
- [Dockerベースのレシピのインポート — PySpark](#pyspark)
- [Dockerベースのレシピのインポート — Scala](#scala)

非推奨ワークフロー:
- [バイナリベースのレシピの読み込み — PySpark](#pyspark-deprecated)
- [バイナリベースのレシピの読み込み — Scala Spark](#scala-deprecated)

### レシピの設定 {#configure}

Data Science Workspaceのすべてのレシピインスタンスには、特定の使用例に合わせてレシピインスタンスをカスタマイズする一連の設定が伴います。 設定ファイルは、このレシピインスタンスを使用して作成されたモデルのデフォルトのトレーニングおよびスコアリング動作を定義します。

>[!NOTE] 設定ファイルは、レシピとケースに固有のものです。

以下に、小売売上のレシピのデフォルトのトレーニングとスコアリングの動作を示す設定ファイルの例を示します。

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

| パラメータキー | タイプ | 説明 |
| ----- | ----- | ----- |
| `learning_rate` | 数値 | グラデーション乗算のスカラー。 |
| `n_estimators` | 数値 | ランダムフォレスト分類子のフォレスト内のツリー数。 |
| `max_depth` | 数値 | ランダムフォレスト分類子のツリーの最大深さです。 |
| `ACP_DSW_INPUT_FEATURES` | 文字列 | 入力スキーマ属性をコンマで区切ったリスト。 |
| `ACP_DSW_TARGET_FEATURES` | 文字列 | カンマ区切りの出力スキーマ属性のリスト。 |
| `ACP_DSW_FEATURE_UPDATE_SUPPORT` | Boolean | 入出力フィーチャを変更可能にするかどうかを指定します |
| `tenantId` | 文字列 | このIDを使用すると、作成するリソースの名前が適切に付けられ、IMS組織内に含まれるようになります。 [テナントIDを検索するには](../../xdm/api/getting-started.md#know-your-tenant_id) 、ここの手順に従います。 |
| `ACP_DSW_TRAINING_XDM_SCHEMA` | 文字列 | モデルのトレーニングに使用する入力スキーマ。 APIを使用して読み込む場合、UIに読み込むときは、この値を空のままにして、トレーニングSchemaIDに置き換えます。 |
| `evaluation.labelColumn` | 文字列 | 評価のビジュアライゼーションの列ラベル。 |
| `evaluation.metrics` | 文字列 | モデルの評価に使用する評価指標のコンマ区切りリスト。 |
| `ACP_DSW_SCORING_RESULTS_XDM_SCHEMA` | 文字列 | モデルのスコアリングに使用する出力スキーマ。 UIに読み込む場合は、この値を空のままにし、APIを使用して読み込む際にスコアリングSchemaIDに置き換えます。 |

このチュートリアルでは、『データサイエンスワークスペースリファレンス』の小売販売レシピの設定ファイルは、そのままにしておきます。

### Dockerベースのレシピのインポート — Python {#python}

開始を行うには、Platform UIの左上にある **[!UICONTROL ワークフローを移動して選択します]** 。 次に、「レシピを *読み込む* 」を選択し、「 **[!UICONTROL 起動]**」をクリックします。

![](../images/models-recipes/import-package-ui/launch-import.png)

「 *読み込みレシピ* 」ワークフローの「 *設定* 」ページが表示されます。 レシピの名前と説明を入力し、右上隅の **[!UICONTROL 「次]** 」を選択します。

![ワークフローの設定](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
> 「 [Package source files into a Recipe](./package-source-files-recipe.md) tutorial」では、Pythonソースファイルを使用して小売売上のレシピを作成する最後にDocker URLが提供されています。

「 *Select source* 」ページに移動したら、Pythonソースファイルを使用して作成したパッケージレシピに対応するDocker URLを「 **[!UICONTROL Source URL]** 」フィールドに貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップして読み込むか、ファイルシステムの **Browser**&#x200B;を使用します。 提供された設定ファイルは、にあり `experience-platform-dsw-reference/recipes/python/retail/retail.config.json`ます。 「 **[!UICONTROL Runtime]** 」ドロップダウンで「 *Python* 」を選択し、「 **[!UICONTROL Type」ドロップダウンで「]** Classification ** 」を選択します。 すべての情報が入力されたら、右上隅の **[!UICONTROL 「次へ]** 」をクリックして「スキーマの *管理*」に進みます。

>[!NOTE]
> *タイプは&#x200B;*、**[!UICONTROL 分類]**と&#x200B;**[!UICONTROL 回帰をサポートします]**。 モデルがこれらのタイプのいずれにも該当しない場合は、「**[!UICONTROL カスタム]**」(Custom)を選択します。

![](../images/models-recipes/import-package-ui/recipe_source_python.png)

次に、「スキーマの *管理*」セクションの「小売売上高」の入出力スキーマーを選択します。これらは、「小売売上スキーマとデータセット [の](../models-recipes/create-retails-sales-dataset.md) 作成」チュートリアルの付属のブートストラップスクリプトを使用して作成されたものです。

![](../images/models-recipes/import-package-ui/recipe_schema.png)

「 *機能の管理* 」セクションで、スキーマビューアのテナントIDをクリックして、「小売売上高」入力スキーマを展開します。 目的の機能をハイライト表示して入出力機能を選択し、右側の「フィールドプロパティ **[!UICONTROL 」ウィンドウで「]** 入力機能 **[!UICONTROL 」または「]** ターゲット機能 **** 」を選択します。 このチュートリアルでは、 **[!UICONTROL weeklySales]** を **[!UICONTROL ターゲット機能]** 、その他すべてを **[!UICONTROL 入力機能に設定します]**。 「 **[!UICONTROL 次へ]** 」をクリックして、新しく設定したレシピを確認します。

必要に応じて、レシピを確認し、設定を追加、変更、または削除します。 「 **[!UICONTROL 完了]** 」をクリックしてレシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

次の手順に進み、新しく作成した小売売上高のレシピを使用してData Science Workspaceでモデルを作成する方法を調べます [](#next-steps) 。

### Dockerベースのレシピのインポート — R {#r}

開始を行うには、Platform UIの左上にある **[!UICONTROL ワークフローを移動して選択します]** 。 次に、「レシピを *読み込む* 」を選択し、「 **[!UICONTROL 起動]**」をクリックします。

![](../images/models-recipes/import-package-ui/launch-import.png)

「 *読み込みレシピ* 」ワークフローの「 *設定* 」ページが表示されます。 レシピの名前と説明を入力し、右上隅の **[!UICONTROL 「次]** 」を選択します。

![ワークフローの設定](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
> 「 [Package source files into a Recipe](./package-source-files-recipe.md) tutorial」では、Rソースファイルを使用して小売売上のレシピを作成する際に、最後にDocker URLが提供されています。

「 *Select source* （ソースを選択）」ページに移動したら、「Source URL **[!UICONTROL (ソースURL]** )」フィールドに、Rソースファイルを使用して作成したパッケージ化レシピに対応するDocker URLを貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップして読み込むか、ファイルシステムの **Browser**&#x200B;を使用します。 提供された設定ファイルは、にあり `experience-platform-dsw-reference/recipes/R/Retail\ -\ GradientBoosting/retail.config.json`ます。 「 **[!UICONTROL 実行時]** 」ドロップダウンで「 *R* 」を選択し、「 **[!UICONTROL タイプ]** 」ドロップダウンで「分類 ** 」を選択します。 すべての情報が入力されたら、右上隅の **[!UICONTROL 「次へ]** 」をクリックして「スキーマの *管理*」に進みます。

>[!NOTE]
> *タイプは&#x200B;*、**[!UICONTROL 分類]**と&#x200B;**[!UICONTROL 回帰をサポートします]**。 モデルがこれらのタイプのいずれにも該当しない場合は、「**[!UICONTROL カスタム]**」(Custom)を選択します。

![](../images/models-recipes/import-package-ui/recipe_source_R.png)

次に、「スキーマの *管理*」セクションの「小売売上高」の入出力スキーマーを選択します。これらは、「小売売上スキーマとデータセット [の](../models-recipes/create-retails-sales-dataset.md) 作成」チュートリアルの付属のブートストラップスクリプトを使用して作成されたものです。

![](../images/models-recipes/import-package-ui/recipe_schema.png)

「 *機能の管理* 」セクションで、スキーマビューアのテナントIDをクリックして、「小売売上高」入力スキーマを展開します。 目的の機能をハイライト表示して入出力機能を選択し、右側の「フィールドプロパティ **[!UICONTROL 」ウィンドウで「]** 入力機能 **[!UICONTROL 」または「]** ターゲット機能 **** 」を選択します。 このチュートリアルでは、 **[!UICONTROL weeklySales]** を **[!UICONTROL ターゲット機能]** 、その他すべてを **[!UICONTROL 入力機能に設定します]**。 「 **[!UICONTROL 次へ]** 」をクリックして、新しい設定済みレシピを確認します。

必要に応じて、レシピを確認し、設定を追加、変更、または削除します。 「 **完了** 」をクリックしてレシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

次の手順に進み、新しく作成した小売売上高のレシピを使用してData Science Workspaceでモデルを作成する方法を調べます [](#next-steps) 。

### Dockerベースのレシピのインポート — PySpark {#pyspark}

開始を行うには、Platform UIの左上にある **[!UICONTROL ワークフローを移動して選択します]** 。 次に、「レシピを *読み込む* 」を選択し、「 **[!UICONTROL 起動]**」をクリックします。

![](../images/models-recipes/import-package-ui/launch-import.png)

「 *読み込みレシピ* 」ワークフローの「 *設定* 」ページが表示されます。 レシピの名前と説明を入力し、右上隅の **[!UICONTROL 「次へ]** 」を選択して次に進みます。

![ワークフローの設定](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
> PySparkソースファイルを使用した小売売上レシピの構築の最後に、 [Package source files into a Recipe](./package-source-files-recipe.md) tutorialでDocker URLが提供されました。

「 *Select source* 」ページに移動したら、PySparkソースファイルを使用して作成したパッケージレシピに対応するDocker URLを「 **[!UICONTROL Source URL]** 」フィールドに貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップして読み込むか、ファイルシステムの **Browser**&#x200B;を使用します。 提供された設定ファイルは、にあり `experience-platform-dsw-reference/recipes/pyspark/retail/pipeline.json`ます。 「 **[!UICONTROL Runtime]** 」ドロップダウンで「PySpark ** 」を選択します。 PySparkランタイムが選択されると、デフォルトのアーティファクトが **[!UICONTROL Dockerに自動入力されます]**。 次に、「 **[!UICONTROL タイプ]** 」ドロップダウンで「 *分類* 」を選択します。 すべての情報が入力されたら、右上隅の **[!UICONTROL 「次へ]** 」をクリックして「スキーマの *管理*」に進みます。

>[!NOTE]
> *タイプは&#x200B;*、**[!UICONTROL 分類]**と&#x200B;**[!UICONTROL 回帰をサポートします]**。 モデルがこれらのタイプのいずれにも該当しない場合は、「**[!UICONTROL カスタム]**」(Custom)を選択します。

![](../images/models-recipes/import-package-ui/pyspark-databricks.png)

次に、「スキーマの *管理*」セクションの「小売売上高」の入出力スキーマーを選択します。これらは、「小売売上スキーマとデータセット [の](../models-recipes/create-retails-sales-dataset.md) 作成」チュートリアルの付属のブートストラップスクリプトを使用して作成されたものです。

![](../images/models-recipes/import-package-ui/recipe_schema.png)

「 *機能の管理* 」セクションで、スキーマビューアのテナントIDをクリックして、「小売売上高」入力スキーマを展開します。 目的の機能をハイライト表示して入出力機能を選択し、右側の「フィールドプロパティ **[!UICONTROL 」ウィンドウで「]** 入力機能 **[!UICONTROL 」または「]** ターゲット機能 **** 」を選択します。 このチュートリアルでは、 **[!UICONTROL weeklySales]** を **[!UICONTROL ターゲット機能]** 、その他すべてを **[!UICONTROL 入力機能に設定します]**。 「 **[!UICONTROL 次へ]** 」をクリックして、新しく設定したレシピを確認します。

必要に応じて、レシピを確認し、設定を追加、変更、または削除します。 「 **[!UICONTROL 完了]** 」をクリックしてレシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

次の手順に進み、新しく作成した小売売上高のレシピを使用してData Science Workspaceでモデルを作成する方法を調べます [](#next-steps) 。

### Dockerベースのレシピのインポート — Scala {#scala}

開始を行うには、Platform UIの左上にある **[!UICONTROL ワークフローを移動して選択します]** 。 次に、「レシピを *読み込む* 」を選択し、「 **[!UICONTROL 起動]**」をクリックします。

![](../images/models-recipes/import-package-ui/launch-import.png)

「 *読み込みレシピ* 」ワークフローの「 *設定* 」ページが表示されます。 レシピの名前と説明を入力し、右上隅の **[!UICONTROL 「次へ]** 」を選択して次に進みます。

![ワークフローの設定](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
> 「 [Package source files into a Recipe](./package-source-files-recipe.md) tutorial」では、Scala(Spark)ソースファイルを使用して小売売上のレシピを作成する最後に、Docker URLが提供されていました。

「 *Select source* （ソースを選択）」ページに移動したら、「 *Source URL* （ソースURL）」フィールドに、Scalaソースファイルを使用して作成したパッケージレシピに対応するDocker URLを貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップして読み込むか、ファイルシステムの **Browser**&#x200B;を使用します。 提供された設定ファイルは、にあり `experience-platform-dsw-reference/recipes/scala/retail/pipelineservice.json`ます。 「 **[!UICONTROL Runtime]** 」ドロップダウンで「 *Spark* 」を選択します。 Sparkランタイムが選択されると、デフォルトのアーティファクトが **[!UICONTROL Dockerに自動入力されます]**。 次に、「 **[!UICONTROL タイプ]** 」ドロップダウンから「 *回帰* 」を選択します。 すべての情報が入力されたら、右上隅の **[!UICONTROL 「次へ]** 」をクリックして「スキーマの *管理*」に進みます。

>[!NOTE]
> *タイプは&#x200B;*、**[!UICONTROL 分類]**と&#x200B;**[!UICONTROL 回帰をサポートします]**。 モデルがこれらのタイプのいずれにも該当しない場合は、「**[!UICONTROL カスタム]**」(Custom)を選択します。

![](../images/models-recipes/import-package-ui/scala-databricks.png)

次に、「スキーマの *管理*」セクションの「小売売上高」の入出力スキーマーを選択します。これらは、「小売売上スキーマとデータセット [の](../models-recipes/create-retails-sales-dataset.md) 作成」チュートリアルの付属のブートストラップスクリプトを使用して作成されたものです。

![](../images/models-recipes/import-package-ui/recipe_schema.png)

「 *機能の管理* 」セクションで、スキーマビューアのテナントIDをクリックして、「小売売上高」入力スキーマを展開します。 目的の機能をハイライト表示して入出力機能を選択し、右側の「フィールドプロパティ **[!UICONTROL 」ウィンドウで「]** 入力機能 **[!UICONTROL 」または「]** ターゲット機能 **** 」を選択します。 このチュートリアルでは、 **[!UICONTROL weeklySales]** を **[!UICONTROL ターゲット機能]** 、その他すべてを **[!UICONTROL 入力機能に設定します]**。 「 **[!UICONTROL 次へ]** 」をクリックして、新しく設定したレシピを確認します。

必要に応じて、レシピを確認し、設定を追加、変更、または削除します。 「 **[!UICONTROL 完了]** 」をクリックしてレシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

次の手順に進み、新しく作成した小売売上高のレシピを使用してData Science Workspaceでモデルを作成する方法を調べます [](#next-steps) 。

## 次の手順 {#next-steps}

このチュートリアルでは、Data Science Workspaceにレシピを設定して読み込む方法について説明します。 新しく作成したレシピを使用して、モデルを作成、トレーニング、評価できるようになりました。

- [UIでのモデルのトレーニングと評価](./train-evaluate-model-ui.md)
- [APIを使用したモデルのトレーニングと評価](./train-evaluate-model-api.md)

## 非推奨ワークフロー

>[!CAUTION]
>バイナリベースのレシピの読み込みは、PySpark 3 (Spark 2.4)およびScala (Spark 2.4)ではサポートされなくなりました。

### バイナリベースのレシピの読み込み — PySpark {#pyspark-deprecated}

Package [sourceファイルをRecipe](./package-source-files-recipe.md) Tutorialに含めると、Retail Sales PySparkソースファイルを使用して **EGG** binaryファイルが構築されます。

1. [Adobe Experience Platform](https://platform.adobe.com/)で、左側のナビゲーションパネルを探し、「 **ワークフロー**」をクリックします。 ワークフローインターフェイスで、 **「ソースファイルから** 読み込みレシピを新規に起動 **** 」処理を実行します。
   ![](../images/models-recipes/import-package-ui/workflow_ss.png)
2. 小売売上のレシピに適切な名前を入力します。 例えば、「Retail Sales recipe PySpark」とします。 オプションで、レシピの説明とドキュメントURLを含めます。 完了したら、「**Next**」をクリックします。
   ![](../images/models-recipes/import-package-ui/recipe_info.png)
3. ドラッグ&amp;ドロップするか、ファイルシステムの [Browser](./package-source-files-recipe.md)****. パッケージ化されたレシピはに配置する必要があり `experience-platform-dsw-reference/recipes/pyspark/dist`ます。
同様に、指定した設定ファイルをドラッグ&amp;ドロップして読み込むか、ファイルシステムの **Browser**&#x200B;を使用します。 提供された設定ファイルは、にあり `experience-platform-dsw-reference/recipes/pyspark/pipeline.json`ます。 両方のファイルが提供されたら **「次へ** 」をクリックします。
   ![](../images/models-recipes/import-package-ui/recipe_source.png)
4. この時点でエラーが発生する場合があります。 これは正常な動作で、期待される動作です。 「スキーマの **管理**」セクションの「小売売上の入出力スキーマー」を選択します。これらのパラメーターは、「小売売上スキーマとデータセット [の](../models-recipes/create-retails-sales-dataset.md) 作成」チュートリアルの付属のブートストラップスクリプトを使用して作成されています。
   ![](../images/models-recipes/import-package-ui/recipe_schema.png)
「 **機能の管理** 」セクションで、スキーマビューアのテナントIDをクリックして、「小売売上高」入力スキーマを展開します。 目的の機能をハイライト表示して入出力機能を選択し、右側の「フィールドプロパティ **」ウィンドウで「** 入力機能 **」または「** ターゲット機能 **** 」を選択します。 このチュートリアルでは、 **weeklySales** を **ターゲット機能** 、その他すべてを **入力機能に設定します**。 「 **次へ** 」をクリックして、新しく設定したレシピを確認します。
5. 必要に応じて、レシピを確認し、設定を追加、変更、または削除します。 「 **完了** 」をクリックしてレシピを作成します。
   ![](../images/models-recipes/import-package-ui/recipe_review.png)

次の手順に進み、新しく作成した小売売上高のレシピを使用してData Science Workspaceでモデルを作成する方法を調べます [](#next-steps) 。


### バイナリベースのレシピの読み込み — Scala Spark {#scala-deprecated}

Package [source files into a Recipe](./package-source-files-recipe.md) tutorialでは、 **JAR** binaryファイルはRetail Sales Scala Sparkソースファイルを使用して構築されました。

1. [Adobe Experience Platform](https://platform.adobe.com/)で、左側のナビゲーションパネルを探し、「 **ワークフロー**」をクリックします。 ワークフローインターフェイスで、 **「ソースファイルから** 読み込みレシピを新規に起動 **** 」処理を実行します。
   ![](../images/models-recipes/import-package-ui/workflow_ss.png)
2. 小売売上のレシピに適切な名前を入力します。 例えば、「Retail Sales recipe Scala Spark」などです。 オプションで、レシピの説明とドキュメントURLを含めます。 完了したら、「**Next**」をクリックします。
   ![](../images/models-recipes/import-package-ui/recipe_info_scala.png)
3. ドラッグ&amp;ドロップして [パッケージソースファイルで作成したScala Spark Retail SalesレシピをRecipe](./package-source-files-recipe.md) Tutorialに読み込むか、ファイルシステムの **Browser**. 依存関係 **を持つパッケージレシピは** 、にあり `experience-platform-dsw-reference/recipes/scala/target`ます。 同様に、指定した設定ファイルをドラッグ&amp;ドロップして読み込むか、ファイルシステムの **Browser**&#x200B;を使用します。 提供された設定ファイルは、にあり `experience-platform-dsw-reference/recipes/scala/src/main/resources/pipelineservice.json`ます。 両方のファイルが提供されたら **「次へ** 」をクリックします。
   ![](../images/models-recipes/import-package-ui/recipe_source_scala.png)
4. この時点でエラーが発生する場合があります。 これは正常な動作で、期待される動作です。 「スキーマの **管理**」セクションの「小売売上の入出力スキーマー」を選択します。これらのパラメーターは、「小売売上スキーマとデータセット [の](../models-recipes/create-retails-sales-dataset.md) 作成」チュートリアルの付属のブートストラップスクリプトを使用して作成されています。
   ![](../images/models-recipes/import-package-ui/recipe_schema.png)
「 **機能の管理** 」セクションで、スキーマビューアのテナントIDをクリックして、「小売売上高」入力スキーマを展開します。 目的の機能をハイライト表示して入出力機能を選択し、右側の「フィールドプロパティ **」ウィンドウで「** 入力機能 **」または「** ターゲット機能 **** 」を選択します。 このチュートリアルでは、 **weeklySales** を **ターゲット機能** 、その他すべてを **入力機能に設定します**。 「 **次へ** 」をクリックして、新しく設定したレシピを確認します。
5. 必要に応じて、レシピを確認し、設定を追加、変更、または削除します。 「 **完了** 」をクリックしてレシピを作成します。
   ![](../images/models-recipes/import-package-ui/recipe_review.png)

次の手順に進み、新しく作成した小売売上高のレシピを使用してData Science Workspaceでモデルを作成する方法を調べます [](#next-steps) 。