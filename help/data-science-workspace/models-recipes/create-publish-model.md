---
keywords: Experience Platform；機械学習モデル；Data Science Workspace；一般的なトピック；モデルの作成と公開
solution: Experience Platform
title: 機械学習モデルの作成と公開
topic: チュートリアル
type: チュートリアル
description: Adobe Experience Platform Data Science Workspace は、事前に作成された製品推奨レシピを使用して目標を達成する手段を提供します。このチュートリアルに従って、小売データにアクセスして理解し、機械学習モデルを作成して最適化し、Data Science Workspaceでインサイトを生成する方法を確認します。
translation-type: tm+mt
source-git-commit: b5d42c6a38a50d39e1ca46e18623dde59c33833b
workflow-type: tm+mt
source-wordcount: '1580'
ht-degree: 41%

---


# マシンラーニングモデルの作成と公開

![](../images/models-recipes/model-walkthrough/objective.png)

あなたはオンラインの小売サイトを所有しているとします。あなたは顧客が小売 Web サイトで買い物をする際に、自社が提供する他の様々な製品が目に入るよう、パーソナライズされた製品レコメンデーションを提示したいと考えています。Web サイトの存在期間全体にわたって、顧客データを継続的に収集し、このデータを使用してパーソナライズされた商品レコメンデーションを生成しようとします。

[!DNL Adobe Experience Platform] [!DNL Data Science Workspace] 事前に組み込まれた [製品Recommendationsレシピを使用して目標を達成する手段を提供します](../pre-built-recipes/product-recommendations.md)。このチュートリアルに従って、小売データにアクセスして理解し、機械学習モデルを作成して最適化し、[!DNL Data Science Workspace]でインサイトを生成する方法を確認します。

このチュートリアルは、[!DNL Data Science Workspace]のワークフローを反映しており、機械学習モデルを作成するための次の手順をカバーしています。

1. [データの準備](#prepare-your-data)
2. [モデルのオーサリング](#author-your-model)
3. [モデルのトレーニングと評価](#train-and-evaluate-your-model)
4. [モデルを操作できるようにする](#operationalize-your-model)

## はじめに

このチュートリアルを開始する前に、次の前提条件を満たす必要があります。

- [!DNL Adobe Experience Platform]にアクセスします。 [!DNL Experience Platform]のIMS組織にアクセスできない場合は、先に進む前に、システム管理者にお問い合わせください。

- イネーブルメントアセット。次のアイテムをプロビジョニングするには、アカウント担当者にお問い合わせください。
   - Recommendations のレシピ
   - Recommendations の入力データセット
   - Recommendations の入力スキーマ
   - Recommendations の出力データセット
   - Recommendations の出力スキーマ
   - ゴールデンデータセット postValues
   - ゴールデンデータセットスキーマ

- 必要な3つの[!DNL Jupyter Notebook]ファイルを[Adobepublic [!DNL Git] repository](https://github.com/adobe/experience-platform-dsw-reference/tree/master/Summit/2019/resources/Notebooks-Thurs)からダウンロードします。これらは[!DNL Data Science Workspace]で[!DNL JupyterLab]ワークフローを示すのに使用されます。

このチュートリアルで使用する次の主要概念に対する十分な理解
- [[!DNL Experience Data Model]](../../xdm/home.md):Adobeが導く標準化の取り組み。Customer Experience Managementの標準スキーマ（例：ExperienceEvent） [!DNL Profile] を定義します。
- データセット：実際のデータ用に構成されたストレージおよび管理。[XDM スキーマ](../../xdm/schema/field-dictionary.md)の物理的にインスタンス化されたインスタンス。
- バッチ：データセットは、バッチで構成されます。バッチとは、一定期間に収集され、1 つの単位としてまとめて処理される一連のデータです。
- [!DNL JupyterLab]: [[!DNL JupyterLab]](https://blog.jupyter.org/jupyterlab-is-ready-for-users-5a6f039b8906) は、Project用のオープンソースのWebベースのインターフェイスで [!DNL Jupyter] 、に緊密に統合されてい [!DNL Experience Platform]ます。

## データの準備 {#prepare-your-data}

顧客に合わせてパーソナライズされた商品レコメンデーションを作成する機械学習モデルを作成するには、Web サイトにおける以前の顧客の購入を分析する必要があります。この節では、このデータが[!DNL Platform]から[!DNL Adobe Analytics]にどのように取り込まれるか、およびそのデータが機械学習モデルで使用する機能データセットにどのように変換されるかについて説明します。

### データを調べてスキーマを理解する

[Adobe Experience Platform](https://platform.adobe.com/)にログインし、**[!UICONTROL データセット]**&#x200B;を選択して既存のデータセットをすべてリストし、調査するデータセットを選択します。 この場合、[!DNL Analytics]データセット&#x200B;**ゴールデンデータセットpostValues**&#x200B;です。

![](../images/models-recipes/model-walkthrough/dataset-browse.png)

データセットのアクティビティページが開き、データセットに関する情報が一覧表示されます。 右上付近にある&#x200B;**[!UICONTROL プレビューデータセット]**&#x200B;を選択して、サンプルレコードを調べることができます。 また、選択したデータセットのスキーマを表示することもできます。 右側のパネルでスキーマリンクを選択します。 「**[!UICONTROL スキーマ名]**」の下のリンクを選択すると、スキーマが新しいタブに開きます。

![](../images/models-recipes/model-walkthrough/dataset-activity.png)


![](../images/models-recipes/model-walkthrough/schema-view.png)

その他のデータセットは、プレビュー用にバッチを使用して事前設定されています。これらのデータセットを表示するには、上記の手順を繰り返します。

| データセット名 | スキーマ | 説明 |
| ----- | ----- | ----- |
| ゴールデンデータセット postValues | ゴールデンデータセットスキーマ | [!DNL Analytics]Web サイトの ソースデータ |
| Recommendations の入力データセット | Recommendations の入力スキーマ | [!DNL Analytics]データは、機能パイプラインを使用してトレーニングデータセットに変換されます。 このデータは、製品レコメンデーションの機械学習モデルのトレーニングに使用されます。`itemid` および `userid` は、その顧客が購入した製品に対応しています。 |
| Recommendations の出力データセット | Recommendations の出力スキーマ | スコア付け結果が保存されるデータセットには、各顧客にお勧めする製品のリストが含まれます。 |

## モデルのオーサリング {#author-your-model}

[!DNL Data Science Workspace]ライフサイクルの2番目の要素は、レシピとモデルの作成です。 製品 Recommendations レシピは、過去の購入データと機械学習を利用して、製品 Recommendations を大規模に生成できるように作られています。

レシピは、特定の問題を解決するために設計された機械学習アルゴリズムとロジックを含んでおり、モデルの基盤となります。さらに重要な点は、レシピを使用すると、組織全体の機械学習を民主化でき、他のユーザーはコードを書かなくても様々な用途のモデルにアクセスできるようになることです。

### 製品 Recommendations レシピの参照

Experience Platformで、左側のナビゲーション列の&#x200B;**[!UICONTROL モデル]**&#x200B;に移動し、上部のナビゲーションで&#x200B;**[!UICONTROL レシピ]**&#x200B;を選択して、組織で使用可能なレシピのリストを表示します。

![](../images/models-recipes/model-walkthrough/recipe-tab.png)

次に、提供された&#x200B;**[!UICONTROL Recommendationsレシピ]**&#x200B;を探して開きます。名前を選択します。 レシピの概要ページが表示されます。

![](../images/models-recipes/model-walkthrough/Recipe-view.png)

次に、右側のレールで、「**[!UICONTROL Recommendations入力スキーマ]**」を選択して、レシピの電源を入れているスキーマを表示します。 スキーマフィールド「[!UICONTROL itemId]」と「[!UICONTROL userId]」は、特定の時刻([!UICONTROL timestamp])に、その顧客が購入した製品([!UICONTROL interactionType])に対応します。 同じ手順に従って、**[!UICONTROL Recommendations の出力スキーマ]**&#x200B;のフィールドを確認します。

![](../images/models-recipes/model-walkthrough/input-output.png)

これで、商品 Recommendations のレシピで必要な入力および出力スキーマを確認しました。次のセクションに進み、製品Recommendationsモデルの作成、トレーニング、評価方法を学びます。

## モデルのトレーニングと評価 {#train-and-evaluate-your-model}

データが準備され、レシピの準備が整ったら、機械学習モデルを作成、トレーニングおよび評価できます。

### モデルの作成

モデルはレシピのインスタンスで、データを大規模にトレーニングして、スコアを付けることができます。

Experience Platformで、左側のナビゲーション列の&#x200B;**[!UICONTROL モデル]**&#x200B;に移動し、上部のナビゲーションで&#x200B;**[!UICONTROL レシピ]**&#x200B;を選択します。 組織で使用可能なレシピのリストが表示されます。商品のレコメンデーションレシピを選択します。

![](../images/models-recipes/model-walkthrough/recipe-tab.png)

レシピページで、「**[!UICONTROL モデルを作成]**」を選択します。

![モデルの作成](../images/models-recipes/model-walkthrough/create-model-recipe.png)

モデル作成ワークフローは、まずレシピを選択して開始します。 **[!UICONTROL Recommendationsレシピ]**&#x200B;を選択し、右上隅の「**[!UICONTROL 次へ]**」を選択します。

![](../images/models-recipes/model-walkthrough/create-model.png)

次に、モデル名を入力します。 モデルに使用可能な設定が、モデルのデフォルトのトレーニングとスコアリング動作の設定を含む一覧に表示されます。 設定を確認し、「**[!UICONTROL 完了]**」を選択します。

![](../images/models-recipes/model-walkthrough/configure-model.png)

新しく生成されたトレーニングの実行によって、モデルの概要ページがリダイレクトされます。 トレーニング実行は、モデルの作成時にデフォルトで生成されます。

![](../images/models-recipes/model-walkthrough/model-overview.png)

トレーニング実行が完了するまで待つか、次のセクションで新しいトレーニング実行の作成を続行するかを選択できます。

### カスタムのハイパーパラメータを使用してモデルをトレーニングする

**モデルの概要**&#x200B;ページで、右上近くにある&#x200B;**[!UICONTROL トレーニング]**&#x200B;を選択して、新しいトレーニング経路を作成します。 モデルの作成時に使用したのと同じ入力データセットを選択し、「**[!UICONTROL 次へ]**」を選択します。

![](../images/models-recipes/model-walkthrough/select-train.png)

**[!UICONTROL 設定]**&#x200B;ページが表示されます。ここで、トレーニングの実行`num_recommendations`値（ハイパーパラメーターとも呼ばれます）を設定できます。 トレーニングを受け、最適化されたモデルは、トレーニングの実行結果に基づいて、パフォーマンスの最も高いハイパーパラメータを利用します。

ハイパーパラメーターは学習できないので、トレーニングを実行する前に割り当てる必要があります。ハイパーパラメータを調整すると、トレーニングを受けたモデルの精度が変わる場合があります。 モデルの最適化は反復的なプロセスなので、満足のいく評価を得る前に、複数のトレーニングの実行が必要になる場合があります。

>[!TIP]
>
>`num_recommendations`を10に設定します。

![](../images/models-recipes/model-walkthrough/training-configuration.png)

追加のデータポイントがモデル評価グラフに表示されます。 実行が完了すると、この処理が表示されるまで数分かかる場合があります。

![](../images/models-recipes/model-walkthrough/training-graphs.png)

### モデルの評価

トレーニング実行が完了するたびに、結果の評価指標を表示して、モデルのパフォーマンスを判断できます。

完了した各トレーニングの実行に関する評価指標（精度と再現率）を確認するには、トレーニングの実行を選択します。

![](../images/models-recipes/model-walkthrough/select-training-run.png)

各評価指標に対して提供される情報を調べることができます。 これらの指標が高いほど、モデルのパフォーマンスが向上します。

![](../images/models-recipes/model-walkthrough/metrics.png)

各トレーニング実行に使用されるスキーマセット、データセット、設定パラメーターは、右側のパネルで確認できます。「モデル」ページに戻り、評価指標を観察して、パフォーマンスが最も高いトレーニングを特定します。

## モデルを操作できるようにする  {#operationalize-your-model}

Data Science ワークフローの最後の手順では、モデルを操作できるようにして、データストアからのインサイトにスコアを付けて利用します。

### スコアを付けてインサイトを生成する

商品レコメンデーションモデルの概要ページで、再現率と精度の値が最も高い、パフォーマンスの最も高いトレーニング実行の名前を選択します。

![最高点を取る](../images/models-recipes/model-walkthrough/select-training-run.png)

次に、トレーニングの実行の詳細ページの右上にある「**[!UICONTROL スコア]**」を選択します。

![スコアの選択](../images/models-recipes/model-walkthrough/select-score.png)

次に、**[!UICONTROL Recommendations入力データセット]**&#x200B;をスコアリング入力データセットとして選択します。これは、モデルの作成時に使用し、トレーニング実行を行ったのと同じデータセットです。 次に、「**[!UICONTROL 次へ]**」を選択します。

![](../images/models-recipes/model-walkthrough/score-input.png)

入力データセットを入力したら、**[!UICONTROL Recommendations出力データセット]**&#x200B;をスコアリング出力データセットとして選択します。 スコアリング結果は、このデータセットにバッチとして保存されます。

![](../images/models-recipes/model-walkthrough/score-output.png)

最後に、スコアリングの設定を確認します。 これらのパラメーターには、前に選択した入出力データセットと適切なスキーマーが含まれます。 「**[!UICONTROL 終了]**」を選択して、スコアリングの実行を開始します。 実行が完了するまで数分かかる場合があります。

![](../images/models-recipes/model-walkthrough/score-finish.png)

### スコアが付けられたインサイトを表示する

スコアリングの実行が正常に完了すると、生成されたインサイトの結果と表示をプレビューできます。

スコアリングの実行ページで、完了したスコアリングの実行を選択し、右側のレールで「**[!UICONTROL プレビュースコアリング結果データセット]**」を選択します。

![](../images/models-recipes/model-walkthrough/preview-scores.png)

プレビューの表では、各行に特定の顧客に対する製品レコメンデーションが含まれ、それぞれに [!UICONTROL recommendations] および [!UICONTROL userId] というラベルが付けられます。サンプルのスクリーンショットでは[!UICONTROL num_recommendations]ハイパーパラメーターが10に設定されているので、レコメンデーションの各行には、番号記号(#)で区切られた最大10個の製品IDを含めることができます。

![](../images/models-recipes/model-walkthrough/preview_score_results.png)

## 次の手順 {#next-steps}

このチュートリアルでは、[!DNL Data Science Workspace]のワークフローについて説明し、未処理の生のデータを機械学習によって有用な情報に変える方法を説明します。 [!DNL Data Science Workspace]の使い方の詳細については、[小売売上スキーマとデータセット](./create-retails-sales-dataset.md)の作成に関する次のガイドに進んでください。