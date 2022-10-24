---
keywords: Experience Platform；ホーム；Data Science Workspace；人気の高いトピック；アクセス制御；サンドボックス；インテリジェンスパック；dsw 機能；dsw アクセス；Adobe Experience Platformインテリジェンス；インテリジェンス；aep インテリジェンスパッケージ
solution: Experience Platform
title: Data Science Workspace のアクセスと機能
topic-legacy: Access and features for data science workspace
description: 次のドキュメントでは、Data Science Workspace の権限と機能へのアクセスの概要を説明します。
exl-id: 6759fea4-adb9-4e4e-9f3d-e0e8c885b1dd
source-git-commit: e67b3a6f9f57a3971a5bfa755db3b1043bebc96b
workflow-type: tm+mt
source-wordcount: '688'
ht-degree: 18%

---

# Data Science Workspace のアクセスと機能

次のドキュメントでは、Data Science Workspace の権限と機能へのアクセスの概要を説明します。

![DSW タブ](./images/access/platform-tabs.png)

- **ノートブック：** インタラクティブな開発環境 ([JupyterLab](./jupyterlab/overview.md)) を参照して、Experience Platform上のデータを調査、分析およびモデル化します。
- **モデル：** 高度な機械学習レシピとモデルを作成、公開、保存するためのツールを提供します。 詳しくは、 [機械学習モデルの作成と公開](./models-recipes/create-publish-model.md) チュートリアル
- **サービス：** 次のようなAdobeが提供するサービスを含みます。 [AI/ML サービス](../intelligent-services/home.md) と、Data Science Workspace で作成したカスタムサービス。

「サービス」タブのみが表示されるのはなぜですか？

- 組織は、顧客 AI AI/ML サービスを含むAdobe Real-time Customer Data Platform(Real-Time CDP) にのみ権利を付与される場合があります。

以下の **データサイエンス** タブを開き、Data Science Workspace 機能の使用を希望する場合は、会社の管理者に問い合わせて、Adobe Experience Platform Intelligence のライセンスをお持ちかどうかを確認してください。

## Data Science Workspace のパッケージ化

Data Science Workspace 機能は、 Adobe Experience Platform Intelligence パッケージと Advanced Intelligence Pack アドオンで使用できます

次の表に、高度なインテリジェンスパックアドオンを使用した場合と使用しない場合の Data Science Workspace の使用権限の主な違いを示します。

>[!NOTE]
>
>複数の Advanced Intelligence Pack Addon のライセンスを取得でき、容量の増加が権利全体に追加されます。 例えば、2 件のAdobe Experience Platform Advanced Intelligence Pack Addons をライセンスした場合、合計 20 人の同時ノートブックユーザーの権利が付与されます。

| Data Science Workspace の権限付与 | Adobe Experience Platform Intelligence パッケージのみ | Adobe Experience Platform Intelligence + Advanced Intelligence Pack アドオン |
| --- | :---: | :---: |
| サポートされているノートブックユーザーの数。 | 5 人の同時ユーザー | 最初のパックでは、5 人の同時ユーザーが追加され、追加の購入では、パッケージあたり 10 人の同時ユーザーが追加されます。 |
| 統合された Jupyter Notebook を、調査的なデータ分析とモデルのオーサリングに使用できます。 | X （R、Python、Scala ライブラリをサポート） | X（PySpark および Spark ML ライブラリを追加） |
| クエリサービスとのネイティブ統合。 ノートブックの SQL を使用してデータセットを調べ、形成する機能。 | X | X |
| 予測分析用の事前定義済みノートブックテンプレートへのアクセス | X | X |
| Jupyter Notebooks を使用して、モデルのトレーニングとスコアリングを手動でおこないます。 | X | X |
| トレーニングジョブと参照ジョブのスケジュールを設定し、モデルをデプロイして運用します。 |  | X |
| モデルを簡単に設定、評価、トレーニング、スコアリング、実稼動環境に公開するためのレシピフレームワーク。 |  | X |
| UI 駆動型モデルの実験と評価。 |  | X |
| Tensorflow モデル (GPU Compute) のディープラーニングのサポート。 |  | X |
| Spark ベースの分散計算機で、大規模なデータセット（10MM +行）に対してトレーニングとスコアリングをおこないます。 |  | X |

## アクセス制御

Experience Platform のアクセス制御は、[Adobe Admin Console](https://adminconsole.adobe.com) で管理されます。この機能は、Admin Console の製品プロファイルを利用して、権限およびサンドボックスを持つユーザーをリンクします。詳しくは、「[アクセス制御の概要](../access-control/home.md)」を参照してください。

 Data Science Workspace を使用するには、「Data Science Workspace の管理」権限を有効にする必要があります。次の表に、この権限を有効または無効にした場合の影響を示します。

| 権限 | 有効 | 無効 |
|---|---|---|
| Data Science Workspace の管理 | Data Science Workspace のすべてのサービスへのアクセスを提供します。 | Data Science Workspace 内のすべてのサービスへの API および UI アクセスが無効になっています。 無効の場合、 **ノートブック**, **モデル**、および **サービス** ページが表示されなくなります。 <li>アクセス先 **サービス** は、引き続きAdobe Real-time Customer Data Platform(Real-Time CDP) を通じて使用できます。</li> |

## サンドボックスのサポート

サンドボックスは、Experience Platform の単一のインスタンス内の仮想パーティションです。Platform の各インスタンスは、複数の実稼動用サンドボックスと非実稼動用サンドボックスをサポートし、それぞれが Platform リソースの独自のライブラリを維持します。 非実稼働用サンドボックスを使用すると、実稼働用サンドボックスに影響を与えることなく、機能のテスト、実験の実行、カスタム設定をおこなうことができます。 サンドボックスについて詳しくは、「[サンドボックスの概要](../sandboxes/home.md)」を参照してください。

現在、Data Science Workspace には次のサンドボックス制限があります。

- 計算リソースは、実稼働用サンドボックスと非実稼働用サンドボックスで共有されます。

## 次の手順

このドキュメントでは、Data Science Workspace で使用できる様々なタイプのアクセスおよび機能について説明しました。

日々のワークフローの完了など、Data Science Workspace の詳細については、まず [Data Science Workspace のチュートリアル](./walkthrough.md) ドキュメント。 一般情報については、 [Data Science Workspace の概要](./home.md).
