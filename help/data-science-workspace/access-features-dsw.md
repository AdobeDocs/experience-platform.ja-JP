---
keywords: Experience Platform；ホーム；Data Science Workspace；人気のトピック；アクセス制御；サンドボックス；intelligence pack;dsw 機能；dsw アクセス；Adobe Experience Platform Intelligence;intelligence;aep intelligence パッケージ
solution: Experience Platform
title: Data Science Workspaceのアクセスと機能
description: 次のドキュメントでは、Data Science Workspaceの権限と機能へのアクセスの概要を説明します。
exl-id: 6759fea4-adb9-4e4e-9f3d-e0e8c885b1dd
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '717'
ht-degree: 15%

---

# Data Science Workspace のアクセスと機能

>[!NOTE]
>
>Data Science Workspaceは購入できなくなりました。
>
>このドキュメントは、Data Science Workspaceの以前の使用権限を持つ既存のお客様を対象としています。

次のドキュメントでは、Data Science Workspaceの権限と機能へのアクセスの概要を説明します。

![DSW タブ &#x200B;](./images/access/platform-tabs.png)

- **ノートブック：** Experience Platform上のデータを調査、分析、モデル化するためのインタラクティブな開発環境（[JupyterLab](./jupyterlab/overview.md)）を提供します。
- **モデル：** 高度な機械学習のレシピとモデルを作成、公開、保存するためのツールを提供します。 詳しくは、[&#x200B; 機械学習モデルの作成と公開 &#x200B;](./models-recipes/create-publish-model.md) チュートリアルを参照してください。
- **サービス：** [AI/ML サービスなど、Adobeが提供するサービスと &#x200B;](../intelligent-services/home.md)Data Science Workspaceで作成したカスタムサービスの両方が含まれます。

「サービス」タブのみが表示されるのはなぜですか。

- 組織は、顧客 AI/ML サービスを含むAdobe Real-Time Customer Data Platform（Real-Time CDP）のみを利用できる場合があります。

「**Data Science**」タブが表示されず、Adobe Experience Platform Workspace機能を使用する場合は、会社の管理者に連絡して、Data Science ライセンスがあるかどうかを確認してください。

## Data Science Workspaceのパッケージ

Data Science Workspaceの機能は、Adobe Experience Platform Intelligence パッケージと Advanced Intelligence Pack アドオンで利用できます

次の表に、Advanced Intelligence Pack アドオンを使用した場合と使用しない場合の、Data Science Workspaceの使用権限の主な違いを示します。

>[!NOTE]
>
>複数の Advanced Intelligence Pack アドオンのライセンスを取得でき、容量が増えて使用権限全体に追加されます。 例えば、2 つのAdobe Experience Platform Advanced Intelligence Pack アドオンのライセンスを取得した場合、ノートブックの合計 20 人の同時ユーザーに対する権限が付与されます。

| Data Science Workspaceの使用権限 | Adobe Experience Platform Intelligence パッケージのみ | Adobe Experience Platform Intelligence plus Advanced Intelligence Pack アドオン |
| --- | :---: | :---: |
| サポートされるノートブック ユーザーの数です。 | 5 人の同時ユーザー | 最初のパックでは同時ユーザーが 5 人追加され、追加の購入ではパッケージあたり 10 人の同時ユーザーが追加されます。 |
| 探索的なデータ分析とモデルオーサリングに統合された Jupyter Notebooks を利用できます。 | X （R、Python、Scala ライブラリをサポート） | X （PySpark および Spark ML ライブラリを追加） |
| クエリサービスとのネイティブ統合。 ノートブックで SQL を使用して、データセットを調査し形作る機能。 | X | X |
| 予測分析用の事前定義済みノートブックテンプレートにアクセスできます。 | X | X |
| Jupyter Notebooks を使用して、モデルのトレーニングとスコアリングを手動で行います。 | X | X |
| トレーニングと推論ジョブをスケジュールできる機能を備えたモデルのデプロイと運用 | | X |
| モデルを容易に設定、評価、トレーニング、スコアリングおよび実稼動環境に公開するためのレシピフレームワーク。 |  | X |
| UI 駆動モデルの実験と評価。 | | X |
| Tensorflow モデル（GPU コンピューティング）のディープラーニング サポート。 | | X |
| Spark ベースの分散コンピューティングで、大規模なデータセット（10 MM +行）に対してトレーニングとスコアリングを行います。 | | X |

## アクセス制御

Experience Platform のアクセス制御は、[Adobe Admin Console](https://adminconsole.adobe.com) で管理されます。この機能は、Admin Console の製品プロファイルを利用して、権限およびサンドボックスを持つユーザーをリンクします。詳しくは、「[アクセス制御の概要](../access-control/home.md)」を参照してください。

データサイエンスWorkspaceを使用するには、「データサイエンスWorkspaceの管理」権限を有効にする必要があります。 次の表に、この権限を有効または無効にした場合の影響を示します。

| 権限 | 有効 | 無効 |
|---|---|---|
| Data Science Workspace の管理 | Data Science Workspace のすべてのサービスへのアクセスを提供します。 | Data Science Workspace内のすべてのサービスへの API および UI アクセスが無効になっています。 無効の場合、**ノートブック**、**モデル**、**サービス** ページの選択はできません。 <li>**サービス** へのアクセスは、Adobe Real-Time Customer Data Platform（Real-Time CDP）を通じて引き続き利用できます。</li> |

## サンドボックスのサポート

サンドボックスは、Experience Platform の単一のインスタンス内の仮想パーティションです。各Experience Platform インスタンスは、複数の実稼動用サンドボックスおよび非実稼動用サンドボックスをサポートしており、それぞれがExperience Platform リソースの独自のライブラリを保持します。 非実稼動用サンドボックスを使用すると、実稼動用サンドボックスに影響を与えることなく、機能のテスト、実験の実行、カスタム設定を行うことができます。 サンドボックスについて詳しくは、「[サンドボックスの概要](../sandboxes/home.md)」を参照してください。

現在、Data Science Workspaceには、次のサンドボックス制限があります。

- 計算リソースは、実稼動用サンドボックスと非実稼動用サンドボックスで共有されます。

## 次の手順

このドキュメントでは、Data Science Workspaceで使用できる様々なタイプのアクセスと機能について説明しました。

日常的なワークフロー全体など、Data Science Workspaceについて詳しくは、まず [Data Science Workspaceのチュートリアル &#x200B;](./walkthrough.md) ドキュメントをお読みください。 一般的な情報については、[Data Science Workspaceの概要 &#x200B;](./home.md) を参照してください。
