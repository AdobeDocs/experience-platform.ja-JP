---
keywords: Experience Platform；ホーム；Data Science Workspace；人気の高いトピック；アクセス制御；サンドボックス；インテリジェンスパック；dsw 機能；dsw アクセス；Adobe Experience Platformインテリジェンス；インテリジェンス；aep インテリジェンスパッケージ
solution: Experience Platform
title: Data Science Workspace のアクセスと機能
topic-legacy: Access and features for data science workspace
description: 次のドキュメントでは、Data Science Workspace の権限と機能へのアクセスの概要を説明します。
exl-id: 6759fea4-adb9-4e4e-9f3d-e0e8c885b1dd
source-git-commit: 2ff2721f5420483ddc5caffd1eb0532df729e01b
workflow-type: tm+mt
source-wordcount: '684'
ht-degree: 18%

---

# Data Science Workspace のアクセスと機能

次のドキュメントでは、Data Science Workspace の権限と機能へのアクセスの概要を説明します。

![DSW タブ](./images/access/platform-tabs.png)

- **ノートブック：** Experience Platform上のデータを調査、分析、モデル化するためのインタラクティブな開発環境 ([JupyterLab](./jupyterlab/overview.md)) を提供します。
- **モデル：** 高度な機械学習のレシピとモデルを作成、公開、保存するためのツールを提供します。詳しくは、「[ 機械学習モデルの作成と公開 ](./models-recipes/create-publish-model.md)」のチュートリアルを参照してください。
- **サービス：**  [AI/ML サービスなどのAdobeが提供するサービスと、Data Science Workspace で作成し](../intelligent-services/home.md) たカスタムサービスの両方が含まれます。

「サービス」タブのみが表示されるのはなぜですか？

- お客様の組織は、顧客 AI AI/ML サービスを含むリアルタイム顧客データプラットフォーム (RTCDP) のみを利用できます。

「**Data Science**」タブが表示されず、Data Science Workspace 機能の使用を希望する場合は、会社の管理者に問い合わせて、Adobe Experience Platform Intelligence のライセンスを持っているかどうかを確認してください。

## Data Science Workspace のパッケージ化

Data Science Workspace 機能は、 Adobe Experience Platform Intelligence パッケージおよび Advanced Intelligence Pack アドオンで使用できます

次の表に、Advanced Intelligence Pack アドオンの有無にかかわらず、Data Science Workspace の使用権限の主な違いを示します。

>[!NOTE]
>
>複数の Advanced Intelligence Pack Addon のライセンスを取得でき、容量の増加は権利全体に追加されます。 例えば、2 つのAdobe Experience Platform Advanced Intelligence Pack Addons のライセンスを取得した場合、合計 20 人のノートブックユーザーが同時に使用できます。

| Data Science Workspace の権限付与 | Adobe Experience Platform Intelligence パッケージのみ | Adobe Experience Platform Intelligence plus Advanced Intelligence Pack アドオン |
| --- | :---: | :---: |
| サポートされるノートブックユーザーの数。 | 5 人の同時ユーザー | 最初のパックでは、5 人の同時ユーザーが追加され、追加の購入では、パッケージあたり 10 人の同時ユーザーが追加されます。 |
| Jupyter ノートブックを統合し、調査データ分析とモデルオーサリングを可能にします。 | X （R、Python、Scala ライブラリをサポート） | X（PySpark および Spark ML ライブラリの追加） |
| クエリサービスとのネイティブ統合。 ノートブックの SQL を使用して、データセットを調べ、形成する機能。 | X | X |
| 予測分析用の事前に作成されたノートブックテンプレートへのアクセス | X | X |
| Jupyter ノートブックでモデルのトレーニングとスコアリングを手動でおこないます。 | X | X |
| トレーニングジョブのスケジュールを設定し、ジョブを参照する機能を使用して、モデルをデプロイし、操作します。 |  | X |
| モデルの設定、評価、トレーニング、スコアリング、実稼動環境への公開を容易におこなうレシピフレームワーク。 |  | X |
| UI 主導型モデルの実験と評価。 |  | X |
| Tensorflow モデル (GPU Compute) のディープラーニングのサポート。 |  | X |
| Spark ベースの分散計算機で、大規模なデータセット（10MM +行）に対してトレーニングとスコアリングをおこないます。 |  | X |

## アクセス制御

Experience Platform のアクセス制御は、[Adobe Admin Console](https://adminconsole.adobe.com) で管理されます。この機能は、Admin Console の製品プロファイルを利用して、権限およびサンドボックスを持つユーザーをリンクします。詳しくは、「[アクセス制御の概要](../access-control/home.md)」を参照してください。

 Data Science Workspace を使用するには、「Data Science Workspace の管理」権限を有効にする必要があります。次の表に、この権限を有効または無効にした場合の影響を示します。

| 権限 | 有効 | 無効 |
|---|---|---|
| Data Science Workspace の管理 | Data Science Workspace のすべてのサービスへのアクセスを提供します。 | Data Science Workspace 内のすべてのサービスへの API および UI アクセスが無効になっています。 無効にしている間は、**ノートブック**、**モデル**、**サービス** の各ページを選択できません。 <li>**サービス** へのアクセスは、引き続きリアルタイム顧客データプラットフォーム (RTCDP) を通じて利用できます。</li> |

## サンドボックスのサポート

サンドボックスは、Experience Platform の単一のインスタンス内の仮想パーティションです。各 Platform インスタンスは、複数の実稼動用サンドボックスと非実稼動用サンドボックスをサポートし、それぞれが Platform リソースの独自のライブラリを維持します。 非実稼働用サンドボックスを使用すると、実稼働用サンドボックスに影響を与えることなく、機能のテスト、実験の実行、カスタム設定をおこなうことができます。 サンドボックスについて詳しくは、「[サンドボックスの概要](../sandboxes/home.md)」を参照してください。

現在、Data Science Workspace には次のサンドボックスの制限があります。

- 計算リソースは、実稼動用サンドボックスと非実稼動用サンドボックスで共有されます。

## 次の手順

このドキュメントでは、Data Science Workspace で使用できる様々なタイプのアクセスおよび機能について説明しました。

日々のワークフローの完了など、Data Science Workspace の詳細については、まず『[Data Science Workspace のチュートリアル ](./walkthrough.md)』をお読みください。 一般情報については、「[Data Science Workspace の概要 ](./home.md)」を参照してください。
