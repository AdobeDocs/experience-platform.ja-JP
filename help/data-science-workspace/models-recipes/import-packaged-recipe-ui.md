---
keywords: Experience Platform;import packaged recipe;Data Science Workspace;popular topics
solution: Experience Platform
title: パッケージ化されたレシピの読み込み(UI)
topic: Tutorial
translation-type: tm+mt
source-git-commit: a7db31793d33d4571a867f5632243c59b5cb7975

---


# パッケージ化されたレシピの読み込み(UI)

このチュートリアルでは、提供された小売販売の例を使用してパッケージ化されたレシピを設定およびインポートする方法に関する洞察を提供します。 このチュートリアルの最後までに、Adobe Experience Platform Data Science Workspaceでモデルの作成、トレーニング、評価の準備が整います。

## 前提条件

このチュートリアルでは、Docker画像URLの形式でパッケージ化されたレシピが必要です。 詳しくは、ソースファイルをレシピにパッ [ケージ化する方法に関するチュートリアル](./package-source-files-recipe.md) （英語のみ）を参照してください。

## UIワークフロー

パッケージ化されたレシピをData Science Workspaceに読み込むには、特定のレシピ設定が必要で、単一のJavaScript Object Notation(JSON)ファイルにコンパイルされます。このレシピ設定のコンパイルは、設定ファイルと呼ば **れます**。 特定の設定のセットを含むパッケージ化されたレシピは、レシピインスタンスと **呼ばれます**。 1つのレシピを使用して、Data Science Workspaceで多数のレシピインスタンスを作成できます。

パッケージレシピの読み込みのワークフローは、次の手順で構成されます。
- [レシピの設定](#configure)
- [Dockerベースのレシピのインポート — Python](#python)
- [Dockerベースのレシピの読み込み — R](#r)
- [Dockerベースのレシピの読み込み — PySpark](#pyspark)
- [Dockerベースのレシピの読み込み — Scala](#scala)

非推奨ワークフロー:
- [バイナリベースのレシピの読み込み — PySpark](#pyspark-deprecated)
- [バイナリベースのレシピの読み込み — Scala Spark](#scala-deprecated)

### レシピの設定 {#configure}

Data Science Workspaceの各レシピインスタンスには、特定の使用事例に合わせてレシピインスタンスをカスタマイズする一連の設定が付属しています。 設定ファイルは、このレシピインスタンスを使用して作成されたモデルのデフォルトのトレーニングおよびスコアリング動作を定義します。

>[!NOTE] 設定ファイルは、レシピと大文字と小文字が区別されます。

以下に、小売販売のレシピのデフォルトのトレーニングとスコアリングの動作を示す設定ファイルの例を示します。

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
| `learning_rate` | 数値 | グラデーションの乗算のスカラー。 |
| `n_estimators` | 数値 | ランダムフォレスト分類子のフォレスト内のツリーの数。 |
| `max_depth` | 数値 | ランダムフォレスト分類子のツリーの最大深さ。 |
| `ACP_DSW_INPUT_FEATURES` | 文字列 | リスト。 |
| `ACP_DSW_TARGET_FEATURES` | 文字列 | リスト。 |
| `ACP_DSW_FEATURE_UPDATE_SUPPORT` | Boolean | 入出力機能を変更可能にするかどうかを指定します。 |
| `tenantId` | 文字列 | このIDを使用すると、作成したリソースの名前が適切に付けられ、IMS組織内に含まれるようになります。 [テナントIDを検索するには](../../xdm/api/getting-started.md#know-your-tenant_id) 、次の手順に従います。 |
| `ACP_DSW_TRAINING_XDM_SCHEMA` | 文字列 | モデルのトレーニングに使用する入力スキーマ。 UIに読み込む場合は空のままにし、APIを使用して読み込む場合はトレーニングSchemaIDに置き換えます。 |
| `evaluation.labelColumn` | 文字列 | 評価のビジュアライゼーションの列ラベル。 |
| `evaluation.metrics` | 文字列 | モデルのリストに使用する評価指標のコンマ区切りの評価。 |
| `ACP_DSW_SCORING_RESULTS_XDM_SCHEMA` | 文字列 | モデルのスコアスキーマに使用する出力データ。 UIに読み込む場合は空のままにし、APIを使用して読み込む場合はスコアリングスキーマIDに置き換えます。 |

このチュートリアルでは、Data Science Workspace Referenceの小売販売レシピのデフォルトの設定ファイルをそのまま使用できます。

### Dockerベースのレシピのインポート — Python {#python}

開始を設定し **ます** 。 次に、「レシピの読み込 *み」を選択し* 、「起動」をク **リックします**。

![](../images/models-recipes/import-package-ui/launch-import.png)

読み込み *レシピ* ・ワークフローの *「設定* 」ページが表示されます。 レシピの名前と説明を入力し、右上隅 **の** 「次へ」を選択します。

![ワークフローの設定](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
> 「 [Package source files into a Recipe](./package-source-files-recipe.md) tutorial」では、Pythonソースファイルを使用して小売売上のレシピを作成する際の最後に、Docker URLが提供されていました。

「 *Select source* 」ページに移動したら、Pythonソースファイルを使用して作成したパッケージレシピに対応するDocker URLを「 **Source URL** 」フィールドに貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップして読み込むか、ファイルシステムの **Browserを使用しま**&#x200B;す。 提供された設定ファイルは、にありま `experience-platform-dsw-reference/recipes/python/retail/retail.config.json`す。 「 **Runtime** 」ドロップダウンで「 *Python* 」を選択し、「 **Type** 」ドロップ ** ダウンで「Classification」を選択します。 すべての情報が入力されたら、 **右上隅の** 「次へ」をクリックして「スキーマの管理 **」に進みます。

>[!NOTE]
> *タイプは&#x200B;*、分&#x200B;**類と回帰**をサポート&#x200B;**します**。 モデルがこれらのタイプの1つに該当しない場合は、「カスタム」(**Custom**)を選択します。

![](../images/models-recipes/import-package-ui/recipe_source_python.png)

次に、「スキーマの管理」セクションの「小売売上の入出力スキーマ」を選択します。これらのオプションは、「小売売上スキーマとデータセットの作成」チュートリアルに用意されているブートストラップスクリ *プトを使用して作成されました*[](../models-recipes/create-retails-sales-dataset.md) 。

![](../images/models-recipes/import-package-ui/recipe_schema.png)

「機能の管 *理* 」セクションで、スキーマビューアのテナントIDをクリックして、「小売売上」入力スキーマを展開します。 目的の機能をハイライト表示し、右側の「フィールドプロパティ」ウィンドウで「入力機能 **」または「** ターゲット機能 **」を選択して、入力機能と出力** 機能を選択します **** 。 このチュートリアルの目的では、 **weeklySalesを** ターゲット機能 **、その他すべてを** 入力機能として設定します ****。 「次へ **** 」をクリックして、新しく設定したレシピを確認します。

必要に応じて、レシピを確認し、設定を追加、変更または削除します。 「完了」 **をクリックし** 、レシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

次の手順に進み [、新しく作成した](#next-steps) Retail Salesレシピを使用してData Science Workspaceでモデルを作成する方法を確認します。

### Dockerベースのレシピの読み込み — R {#r}

開始を設定し **ます** 。 次に、「レシピの読み込 *み」を選択し* 、「起動」をク **リックします**。

![](../images/models-recipes/import-package-ui/launch-import.png)

読み込み *レシピ* ・ワークフローの *「設定* 」ページが表示されます。 レシピの名前と説明を入力し、右上隅 **の** 「次へ」を選択します。

![ワークフローの設定](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
> Package [source files into a Recipe](./package-source-files-recipe.md) tutorialで、Rソースファイルを使用して小売売上のレシピを作成する際の最後にDocker URLが提供されていました。

「 *Select source* 」ページに移動したら、Rソースファイルを使用して作成したパッケージレシピに対応するDocker URLを「 **Source URL** 」フィールドに貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップして読み込むか、ファイルシステムの **Browserを使用しま**&#x200B;す。 提供された設定ファイルは、にありま `experience-platform-dsw-reference/recipes/R/Retail\ -\ GradientBoosting/retail.config.json`す。 「実行時 **** 」ドロップダウンで「 *R* 」を選択し、「タイプ」ドロップダ ****** ウンで「分類」を選択します。 すべての情報が入力されたら、 **右上隅の** 「次へ」をクリックして「スキーマの管理 **」に進みます。

>[!NOTE]
> *タイプは&#x200B;*、分&#x200B;**類と回帰**をサポート&#x200B;**します**。 モデルがこれらのタイプの1つに該当しない場合は、「カスタム」(**Custom**)を選択します。

![](../images/models-recipes/import-package-ui/recipe_source_R.png)

次に、「スキーマの管理」セクションの「小売売上の入出力スキーマ」を選択します。これらのオプションは、「小売売上スキーマとデータセットの作成」チュートリアルに用意されているブートストラップスクリ *プトを使用して作成されました*[](../models-recipes/create-retails-sales-dataset.md) 。

![](../images/models-recipes/import-package-ui/recipe_schema.png)

「機能の管 *理* 」セクションで、スキーマビューアのテナントIDをクリックして、「小売売上」入力スキーマを展開します。 目的の機能をハイライト表示し、右側の「フィールドプロパティ」ウィンドウで「入力機能 **」または「** ターゲット機能 **」を選択して、入力機能と出力** 機能を選択します **** 。 このチュートリアルの目的では、 **weeklySalesを** ターゲット機能 **、その他すべてを** 入力機能として設定します ****。 「次へ **** 」をクリックして、新しい設定済みレシピを確認します。

必要に応じて、レシピを確認し、設定を追加、変更または削除します。 「完了」 **をクリックし** 、レシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

次の手順に進み [、新しく作成した](#next-steps) Retail Salesレシピを使用してData Science Workspaceでモデルを作成する方法を確認します。

### Dockerベースのレシピの読み込み — PySpark {#pyspark}

開始を設定し **ます** 。 次に、「レシピの読み込 *み」を選択し* 、「起動」をク **リックします**。

![](../images/models-recipes/import-package-ui/launch-import.png)

読み込み *レシピ* ・ワークフローの *「設定* 」ページが表示されます。 レシピの名前と説明を入力し、右上隅の「 **次へ** 」を選択して次に進みます。

![ワークフローの設定](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
> PySparkソースフ [ァイルを使用した小売販売レシピの構築の最後に、](./package-source-files-recipe.md) Package source files into a Recipe tutorialでDocker URLが提供されていました。

「 *Select source* 」ページに移動したら、PySparkソースファイルを使用して作成したパッケージレシピに対応するDocker URLを「 **Source URL** 」フィールドに貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップして読み込むか、ファイルシステムの **Browserを使用しま**&#x200B;す。 提供された設定ファイルは、にありま `experience-platform-dsw-reference/recipes/pyspark/retail/pipeline.json`す。 「 **Runtime** 」ドロップダウン *で「PySpark* 」を選択します。 PySparkランタイムが選択されると、デフォルトのアーティファクトが **Dockerに自動入力されます**。 次に、「タイプ」ド **ロップダウ** ンで「分類 ** 」を選択します。 すべての情報が入力されたら、 **右上隅の** 「次へ」をクリックして「スキーマの管理 **」に進みます。

>[!NOTE]
> *タイプは&#x200B;*、分&#x200B;**類と回帰**をサポート&#x200B;**します**。 モデルがこれらのタイプの1つに該当しない場合は、「カスタム」(**Custom**)を選択します。

![](../images/models-recipes/import-package-ui/pyspark-databricks.png)

次に、「スキーマの管理」セクションの「小売売上の入出力スキーマ」を選択します。これらのオプションは、「小売売上スキーマとデータセットの作成」チュートリアルに用意されているブートストラップスクリ *プトを使用して作成されました*[](../models-recipes/create-retails-sales-dataset.md) 。

![](../images/models-recipes/import-package-ui/recipe_schema.png)

「機能の管 *理* 」セクションで、スキーマビューアのテナントIDをクリックして、「小売売上」入力スキーマを展開します。 目的の機能をハイライト表示し、右側の「フィールドプロパティ」ウィンドウで「入力機能 **」または「** ターゲット機能 **」を選択して、入力機能と出力** 機能を選択します **** 。 このチュートリアルの目的では、 **weeklySalesを** ターゲット機能 **、その他すべてを** 入力機能として設定します ****。 「次へ **** 」をクリックして、新しく設定したレシピを確認します。

必要に応じて、レシピを確認し、設定を追加、変更または削除します。 「完了」 **をクリックし** 、レシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

次の手順に進み [、新しく作成した](#next-steps) Retail Salesレシピを使用してData Science Workspaceでモデルを作成する方法を確認します。

### Dockerベースのレシピの読み込み — Scala {#scala}

開始を設定し **ます** 。 次に、「レシピの読み込 *み」を選択し* 、「起動」をク **リックします**。

![](../images/models-recipes/import-package-ui/launch-import.png)

読み込み *レシピ* ・ワークフローの *「設定* 」ページが表示されます。 レシピの名前と説明を入力し、右上隅の「 **次へ** 」を選択して次に進みます。

![ワークフローの設定](../images/models-recipes/import-package-ui/configure-workflow.png)

>[!NOTE]
> 「 [Package source files into a Recipe](./package-source-files-recipe.md) tutorial」で、Scala(Spark)ソースファイルを使用して小売売上のレシピを作成する際の最後に、Docker URLが提供されていました。

「 *Select source* 」ページに移動したら、「Scala source files」を使用して作成したパッケージレシピに対応するDocker URLを「 *Source URL* 」フィールドに貼り付けます。 次に、指定した設定ファイルをドラッグ&amp;ドロップして読み込むか、ファイルシステムの **Browserを使用しま**&#x200B;す。 提供された設定ファイルは、にありま `experience-platform-dsw-reference/recipes/scala/retail/pipelineservice.json`す。 「 **Runtime** 」ドロップダ *ウンでSpark* を選択します。 Sparkランタイムが選択されると、デフォルトのアーティファクトが **Dockerに自動的に設定されま**&#x200B;す。 次に、[タイプ]ド **ロップダウ** ンから *[回帰* ]を選択します。 すべての情報が入力されたら、 **右上隅の** 「次へ」をクリックして「スキーマの管理 **」に進みます。

>[!NOTE]
> *タイプは&#x200B;*、分&#x200B;**類と回帰**をサポート&#x200B;**します**。 モデルがこれらのタイプの1つに該当しない場合は、「カスタム」(**Custom**)を選択します。

![](../images/models-recipes/import-package-ui/scala-databricks.png)

次に、「スキーマの管理」セクションの「小売売上の入出力スキーマ」を選択します。これらのオプションは、「小売売上スキーマとデータセットの作成」チュートリアルに用意されているブートストラップスクリ *プトを使用して作成されました*[](../models-recipes/create-retails-sales-dataset.md) 。

![](../images/models-recipes/import-package-ui/recipe_schema.png)

「機能の管 *理* 」セクションで、スキーマビューアのテナントIDをクリックして、「小売売上」入力スキーマを展開します。 目的の機能をハイライト表示し、右側の「フィールドプロパティ」ウィンドウで「入力機能 **」または「** ターゲット機能 **」を選択して、入力機能と出力** 機能を選択します **** 。 このチュートリアルの目的では、 **weeklySalesを** ターゲット機能 **、その他すべてを** 入力機能として設定します ****。 「次へ **** 」をクリックして、新しく設定したレシピを確認します。

必要に応じて、レシピを確認し、設定を追加、変更または削除します。 「完了」 **をクリックし** 、レシピを作成します。

![](../images/models-recipes/import-package-ui/recipe_review.png)

次の手順に進み [、新しく作成した](#next-steps) Retail Salesレシピを使用してData Science Workspaceでモデルを作成する方法を確認します。

## 次の手順

このチュートリアルでは、Data Science Workspaceでレシピを設定および読み込む方法に関する洞察を提供しました。 新しく作成したレシピを使用して、モデルの作成、トレーニング、評価を行うことができるようになりました。

- [UIでのモデルのトレーニングと評価](./train-evaluate-model-ui.md)
- [APIを使用したモデルのトレーニングと評価](./train-evaluate-model-api.md)

## 廃止されたワークフロー

>[!CAUTION]
>バイナリベースのレシピの読み込みは、PySpark 3(Spark 2.4)およびScala(Spark 2.4)ではサポートされなくなりました。

### バイナリベースのレシピの読み込み — PySpark {#pyspark-deprecated}

Package [source files into a Recipe](./package-source-files-recipe.md) tutorialでは、 **EGG** binaryファイルがRetail Sales PySparkソースファイルを使用して構築されていました。

1. Adobe Experience Platformで [、左側のナビゲーションパネルを探し、「オプション」をクリッ](https://platform.adobe.com/)クします ****。 「ワークフロー」インターフェイスで **、「ソースファイルか** らの新しいレシピの読み込み **** 」処理を開始します。
   ![](../images/models-recipes/import-package-ui/workflow_ss.png)
2. 小売販売レシピの適切な名前を入力します。 例えば、「Retail Sales recipe PySpark」とします。 オプションで、レシピの説明とドキュメントのURLを含めます。 完了したら、「**Next**」をクリックします。
   ![](../images/models-recipes/import-package-ui/recipe_info.png)
3. パッケージソースファイルで作成されたPySpark Retail Salesレシピをドラッグ&amp;ドロッ [プしてレシピ](./package-source-files-recipe.md) ・チュートリアルに読み込むか、ファイル・システムの **Browserを使用します**。 パッケージ化されたレシピは、に配置する必要がありま `experience-platform-dsw-reference/recipes/pyspark/dist`す。
同様に、指定した設定ファイルをドラッグ&amp;ドロップして読み込むか、ファイルシステムの **Browserを使用しま**&#x200B;す。 提供された設定ファイルは、にありま `experience-platform-dsw-reference/recipes/pyspark/pipeline.json`す。 両方のフ **ァイルが** 指定されたら、「次へ」をクリックします。
   ![](../images/models-recipes/import-package-ui/recipe_source.png)
4. この時点でエラーが発生する可能性があります。 これは正常な動作で、期待される動作です。 「スキーマの管理」セクションの「小売売上の入出力スキーマ」を選択します。これらのオプションは **、「小売売上スキーマとデータセットの作成」チュートリアルに用意されているブートストラップスクリ**&#x200B;プトを使用して作成されています [](../models-recipes/create-retails-sales-dataset.md) 。
   ![](../images/models-recipes/import-package-ui/recipe_schema.png)
「機能の管 **理** 」セクションで、スキーマビューアのテナントIDをクリックして、「小売売上」入力スキーマを展開します。 目的の機能をハイライト表示し、右側の「フィールドプロパティ」ウィンドウで「入力機能 **」または「** ターゲット機能 **」を選択して、入力機能と出力** 機能を選択します **** 。 このチュートリアルの目的では、 **weeklySalesを** ターゲット機能 **、その他すべてを** 入力機能として設定します ****。 「次へ **** 」をクリックして、新しく設定したレシピを確認します。
5. 必要に応じて、レシピを確認し、設定を追加、変更または削除します。 「完了」 **をクリックし** 、レシピを作成します。
   ![](../images/models-recipes/import-package-ui/recipe_review.png)

次の手順に進み [、新しく作成した](#next-steps) Retail Salesレシピを使用してData Science Workspaceでモデルを作成する方法を確認します。


### バイナリベースのレシピの読み込み — Scala Spark {#scala-deprecated}

「 [Package source files into a Recipe](./package-source-files-recipe.md) tutorial」で、 **JAR** binaryファイルがRetail Sales Scala Sparkソースファイルを使用して構築されました。

1. Adobe Experience Platformで [、左側のナビゲーションパネルを探し、「オプション」をクリッ](https://platform.adobe.com/)クします ****。 「ワークフロー」インターフェイスで **、「ソースファイルか** らの新しいレシピの読み込み **** 」処理を開始します。
   ![](../images/models-recipes/import-package-ui/workflow_ss.png)
2. 小売販売レシピの適切な名前を入力します。 例えば、「Retail Sales recipe Scala Spark」とします。 オプションで、レシピの説明とドキュメントのURLを含めます。 完了したら、「**Next**」をクリックします。
   ![](../images/models-recipes/import-package-ui/recipe_info_scala.png)
3. パッケージソースファイルで作成されたScala Spark Retail Salesレシピをドラッグ&amp;ドロップして [Recipe](./package-source-files-recipe.md) Tutorialに読み込むか、ファイルシステムの **Browserを使用します**。 依存関係を持つパッケ **ージレシピは** 、にありま `experience-platform-dsw-reference/recipes/scala/target`す。 同様に、指定した設定ファイルをドラッグ&amp;ドロップして読み込むか、ファイルシステムの **Browserを使用しま**&#x200B;す。 提供された設定ファイルは、にありま `experience-platform-dsw-reference/recipes/scala/src/main/resources/pipelineservice.json`す。 両方のフ **ァイルが** 指定されたら、「次へ」をクリックします。
   ![](../images/models-recipes/import-package-ui/recipe_source_scala.png)
4. この時点でエラーが発生する可能性があります。 これは正常な動作で、期待される動作です。 「スキーマの管理」セクションの「小売売上の入出力スキーマ」を選択します。これらのオプションは **、「小売売上スキーマとデータセットの作成」チュートリアルに用意されているブートストラップスクリ**&#x200B;プトを使用して作成されています [](../models-recipes/create-retails-sales-dataset.md) 。
   ![](../images/models-recipes/import-package-ui/recipe_schema.png)
「機能の管 **理** 」セクションで、スキーマビューアのテナントIDをクリックして、「小売売上」入力スキーマを展開します。 目的の機能をハイライト表示し、右側の「フィールドプロパティ」ウィンドウで「入力機能 **」または「** ターゲット機能 **」を選択して、入力機能と出力** 機能を選択します **** 。 このチュートリアルの目的では、 **weeklySalesを** ターゲット機能 **、その他すべてを** 入力機能として設定します ****。 「次へ **** 」をクリックして、新しく設定したレシピを確認します。
5. 必要に応じて、レシピを確認し、設定を追加、変更または削除します。 「完了」 **をクリックし** 、レシピを作成します。
   ![](../images/models-recipes/import-package-ui/recipe_review.png)

次の手順に進み [、新しく作成した](#next-steps) Retail Salesレシピを使用してData Science Workspaceでモデルを作成する方法を確認します。