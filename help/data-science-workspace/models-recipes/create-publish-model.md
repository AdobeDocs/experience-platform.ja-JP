---
keywords: Experience Platform;機械学習モデル;データ科学ワークスペース;人気のあるトピック;モデルの作成と公開する
solution: Experience Platform
title: マシン学習モデルの作成とPublish
type: Tutorial
description: 次のガイドでは、機械学習モデルの作成と公開するに必要な手順について説明します。
exl-id: f71e5a17-9952-411e-8e6a-aab46bc4c006
source-git-commit: 5d98dc0cbfaf3d17c909464311a33a03ea77f237
workflow-type: tm+mt
source-wordcount: '1096'
ht-degree: 8%

---


# マシンラーニングモデルの作成と公開

>[!NOTE]
>
>データサイエンスワークスペースは購入できなくなりました。
>
>このドキュメントは、以前に データ Science ワークスペース の利用資格を持つ既存のお客様を対象としています。

次のガイドでは、機械学習モデルの作成と公開するに必要な手順について説明します。 各セクションには、実行する内容の説明と、説明されている手順を実行するためのUIおよび API ドキュメントのリンクが含まれています。

## はじめに

このチュートリアルを開始する前に、次の前提条件を満たす必要があります。

- [!DNL Adobe Experience Platform]へのアクセス。[!DNL Experience Platform] の組織にアクセスする権限がない場合は、続行する前にシステム管理者に問い合わせてください。

- すべての Data Science Workspace チュートリアルでは、Luma の傾向モデルを使用します。 手順に従うには、[Luma propensty モデルスキーマとデータセット ](./create-luma-data.md) を作成する必要があります。

### データを調べてスキーマを理解する

[Adobe Experience Platform](https://platform.adobe.com/) にログインし、「**[!UICONTROL データセット]**」を選択して既存のすべてのデータセットをリストし、調査するデータセットを選択します。 この場合、**Luma web データ** データセットを選択する必要があります。

![「Luma Web データセット」を選択します。](../images/models-recipes/model-walkthrough/luma-dataset.png)

データセット アクティビティ ページが開き、データセットに関する情報が表示されます。 右上にある &lbrack;データセット **[!UICONTROL プレビュー]** を選択して、サンプル レコードを確認できます。 選択したデータセットのスキーマ表示こともできます。

![perview Luma Web データ](../images/models-recipes/model-walkthrough/preview-dataset.png)

右パネルでスキーマ リンクを選択します。 ポップオーバーが表示され、 **[!UICONTROL スキーマ名の下のリンクを選択すると]** 新しいタブでスキーマが開きます。

![Luma Web データスキーマプレビュー](../images/models-recipes/model-walkthrough/preview-schema.png)

提供されている探索的データ分析 (EDA) ノートブックを使用して、データをさらに探索できます。 このノートブックは、Luma データのパターンを理解し、データの健全性を確認し、予測傾向モデルの関連データを要約するために使用できます。 探索的データ分析の詳細については、 [EDA ドキュメント](../jupyterlab/eda-notebook.md) 訪問してください。

## Luma 傾向レシピ作成 {#author-your-model}

[!DNL Data Science Workspace]ライフサイクルの主要コンポーネントには、レシピとモデルのオーサリングが含まれます。Luma 傾向モデルは、顧客が Luma から製品を購入する傾向が高いかどうかの予測を生成するように設計されています。

Luma 傾向モデルを作成するには、レシピビルダーテンプレートを使用します。 レシピは、特定の問題を解決するために設計された機械学習アルゴリズムとロジックを含むため、モデルの基礎となります。 さらに重要な点は、レシピを使用すると、組織全体の機械学習を民主化でき、他のユーザーはコードを書かなくても様々な用途のモデルにアクセスできるようになることです。

[JupyterLab ノートブックを使用してモデルを作成する](../jupyterlab/create-a-model.md) チュートリアルに従って、後続のチュートリアルで使用する Luma 傾向モデルレシピを作成します。

## 外部ソースからのレシピインポートおよびパッケージ化(*オプション*)

データ Science ワークスペースで使用するためにレシピを読み込んでパッケージ化する場合は、ソースファイルをアーカイブファイルにパッケージ化する必要があります。 [パッケージソースファイルに従ってレシピ](./package-source-files-recipe.md) チュートリアルを作成します。このチュートリアルでは、レシピを データ Science ワークスペースに読み込むための前提条件ステップである、ソース ファイルをレシピにパッケージ化する方法を示します。 チュートリアルが完了すると、Azure コンテナー レジストリ内の Docker イメージと、対応するイメージ URL (つまりアーカイブ ファイル) が提供されます。

このアーカイブファイルを使用して、 [UI ワークフロー](./import-packaged-recipe-ui.md) または [APIワークフロー](./import-packaged-recipe-api.md)を使用してレシピインポートワークフローに従うことにより、データサイエンスワークスペースでレシピを作成できます。

## モデルのトレーニングと評価 {#train-and-evaluate-your-model}

データの準備が整い、レシピの準備が整ったら、機械学習モデルをさらに作成、トレーニング、評価できます。 レシピビルダーを使用する際は、モデルをレシピにパッケージ化する前に、モデルのトレーニング、スコアリングおよび評価が完了している必要があります。

Data Science Workspaceの UI および API を使用して、レシピをモデルとして公開できます。 さらに、ハイパーパラメータの追加、削除、変更など、モデルの特定の側面をさらに微調整できます。

### モデルの作成

UIを使用したモデルの作成の詳細については、データ Science ワークスペース [UI チュートリアル](./train-evaluate-model-ui.md) または [API チュートリアル](./train-evaluate-model-api.md)でモデルのトレーニングと評価を訪問してください。 このチュートリアルでは、ハイパーパラメーターを作成、トレーニング、更新してモデルを微調整する方法の例を示します。

>[!NOTE]
>
> ハイパーパラメーターは学習できないので、トレーニングを実行する前に割り当てる必要があります。ハイパーパラメーターを調整すると、トレーニング済みのモデルの精度が変わる可能性があります。 モデルの最適化は反復的なプロセスであるため、満足のいく評価が得られるまでに複数のトレーニング実行が必要になる場合があります。

## モデルの採点 {#score-a-model}

モデルを作成して公開する次の手順は、データ レイクと Real-時間 Customer プロフィール から分析情報をスコア付けして使用するために、モデルを運用化することです。

データ Science ワークスペース でのスコアリングは、既存のトレーニング済みモデルに入力データをフィードすることで実現できます。 次に、スコアリング結果が保存され、新しいバッチとして指定した出力データセットで表示可能になります。

モデルをスコア付けする方法を学習するには、モデルのスコア付け [UI チュートリアル](./score-model-ui.md) または [API チュートリアル](./score-model-api.md)訪問。

## スコア付けされたモデルをサービスとしてPublish

データ Science ワークスペースを使用すると、トレーニング済みのモデルをサービスとして公開するできます。 これにより、組織内のユーザーは、独自のモデルを作成しなくてもデータをスコアリングできます。

モデルをサービスとして公開する方法については、 [UI チュートリアル](./publish-model-service-ui.md) または [API チュートリアル](./publish-model-service-api.md)訪問してください。

### サービスの自動トレーニングのスケジュール設定

モデルをサービスとして公開したら、機械学習サービスのスケジュールされたスコアリングとトレーニングの実行を設定できます。 トレーニングとスコアリングプロセスを自動化することで、データ内のパターンに追いつくことで、時間を通じてサービスの効率を維持し、向上させることができます。 [Data Science Workspace UI でのモデルのスケジュール設定 ](./schedule-models-ui.md) チュートリアルにアクセスしてください。

>[!NOTE]
>
> 自動トレーニングとスコアリングのモデルをスケジュールできるのは、UIからのみです。

## 次の手順 {#next-steps}

Adobe Experience Platform [!DNL Data Science Workspace] は、機械学習モデルを作成、評価、利用してデータ予測と分析情報を生成するためのツールとリソースを提供します。 機械学習の分析情報が [!DNL Profile]対応データセットに取り込まれると、その同じデータも [!DNL Profile] レコードとして取り込まれ、 [!DNL Adobe Experience Platform Segmentation Service]を使用してセグメント化できます。

プロファイル データと時系列データが取り込まれると、Real-時間 Customer プロフィール は、既存のデータとマージして和集合 表示を更新する前に、ストリーミング セグメント化 と呼ばれる継続的なプロセスを通じて、そのデータをセグメントに含めるか除外するかを自動的に決定します。 その結果、計算を瞬時に実行し、顧客がブランドとやり取りする際に、強化されたパーソナライズされたエクスペリエンスを顧客に提供するための意思決定を行うことができます。

機械学習のインサイトを活用する方法の詳細については、「機械学習のインサイトで Real-時間 Customer プロフィールを [充実させる](./enrich-profile.md) に関するチュートリアルを参照してください。
