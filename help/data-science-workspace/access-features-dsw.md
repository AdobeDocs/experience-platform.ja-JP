---
keywords: Experience Platform；ホーム；Data Science Workspace；一般的なトピック；アクセス制御；サンドボックス；インテリジェンスパック；dsw機能；dswアクセス；Adobe Experience Platformインテリジェンス；インテリジェンス；aepインテリジェンスパッケージ
solution: Experience Platform
title: Data Science Workspaceのアクセスと機能
topic-legacy: Access and features for data science workspace
description: 次のドキュメントでは、Data Science Workspaceの権限と機能へのアクセスの概要を説明します。
exl-id: 6759fea4-adb9-4e4e-9f3d-e0e8c885b1dd
source-git-commit: 319cdb13c965010062aa9179b197d6f5b6a20287
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 18%

---

# Data Science Workspace のアクセスと機能

次のドキュメントでは、Data Science Workspaceの権限と機能へのアクセスの概要を説明します。

![DSWタブ](./images/access/platform-tabs.png)

- **ノートブック：** Experience Platform上のデータを調査、分析、モデル化するための、インタラクティブな開発環境([JupyterLab](./jupyterlab/overview.md))を提供します。
- **モデル：** 高度な機械学習のレシピとモデルを作成、公開、保存するためのツールを提供します。詳しくは、 [機械学習モデルの作成と公開](./models-recipes/create-publish-model.md)のチュートリアルを参照してください。
- **サービス：**  [AI/MLサービスなど、Adobeが提供するサービスと、Data Science Workspaceで作成し](../intelligent-services/home.md) たカスタムサービスの両方が含まれます。

「サービス」タブのみが表示されるのはなぜですか？

- 組織は、顧客AI AI/MLサービスを含むリアルタイム顧客データプラットフォーム(RTCDP)のみを利用できます。

「**Data Science**」タブが表示されず、Data Science Workspace機能の利用を希望する場合は、会社の管理者に連絡して、Adobe Experience Platform Intelligenceのライセンスをお持ちかどうかを確認してください。

## Data Science Workspaceのパッケージ化

Data Science Workspace機能は、 Adobe Experience Platform IntelligenceパッケージおよびAdvanced Intelligence Packアドオンで使用できます

次の表に、高度なインテリジェンスパックアドオンを使用した場合と使用しなかった場合のData Science Workspaceの使用権限の主な違いを示します。

>[!NOTE]
>
>複数のAdvanced Intelligence Pack Addonのライセンスを取得でき、容量の増加は権利全体に追加されます。 例えば、2つのAdobe Experience Platform Advanced Intelligence Pack Addonsのライセンスを持っている場合、合計20人の同時ノートブックユーザーの権利が付与されます。

| Data Science Workspaceの権限付与 | Adobe Experience Platform Intelligenceパッケージのみ | Adobe Experience Platform Intelligence plus Advanced Intelligence Packアドオン |
| --- | :---: | :---: |
| サポートされているノートブックユーザーの数。 | 5人の同時ユーザー | 最初のパックでは、5人の同時ユーザーが追加され、追加の購入では、1つのパッケージにつき10人の同時ユーザーが追加されます。 |
| Jupyterノートブックを統合して、調査データ分析とモデルオーサリングを可能にします。 | X （R、Python、Scalaの各ライブラリをサポート） | X（PySparkおよびSpark MLライブラリの追加） |
| クエリサービスとのネイティブ統合。 ノートブックのSQLを使用して、データセットを調べ、形成する機能。 | X | X |
| 予測分析用の事前定義済みノートブックテンプレートへのアクセス | X | X |
| Jupyterノートブックでモデルのトレーニングとスコアリングを手動でおこないます。 | X | X |
| トレーニングジョブと参照ジョブのスケジュールを設定できる機能を使用して、モデルをデプロイし、運用できます。 |  | X |
| モデルを簡単に設定、評価、トレーニング、スコアリング、実稼動環境に公開するためのレシピフレームワーク。 |  | X |
| UI駆動型モデルの実験と評価。 |  | X |
| Tensorflowモデル(GPU Compute)のディープラーニングのサポート。 |  | X |
| Sparkベースの分散計算により、大規模なデータセット（10MM +行）に対してトレーニングとスコアリングを実施します。 |  | X |

## アクセス制御

Experience Platform のアクセス制御は、[Adobe Admin Console](https://adminconsole.adobe.com) で管理されます。この機能は、Admin Console の製品プロファイルを利用して、権限およびサンドボックスを持つユーザーをリンクします。詳しくは、「[アクセス制御の概要](../access-control/home.md)」を参照してください。

 Data Science Workspace を使用するには、「Data Science Workspace の管理」権限を有効にする必要があります。次の表に、この権限を有効または無効にした場合の影響を示します。

| 権限 | 有効 | 無効 |
|---|---|---|
| Data Science Workspace の管理 | Data Science Workspace のすべてのサービスへのアクセスを提供します。 | Data Science Workspace内のすべてのサービスに対するAPIとUIのアクセスが無効になっています。 無効になっている間は、**ノートブック**、**モデル**、**サービス**&#x200B;の各ページを選択できません。 <li>**サービス**&#x200B;へのアクセスは、引き続きリアルタイム顧客データプラットフォーム(RTCDP)を通じて利用できます。</li> |

## サンドボックスのサポート

サンドボックスは、Experience Platform の単一のインスタンス内の仮想パーティションです。各Platformインスタンスは、複数の実稼動用サンドボックスと非実稼動用サンドボックスをサポートし、それぞれがPlatformリソースの独自のライブラリを維持します。 非実稼動用サンドボックスを使用すると、実稼動用サンドボックスに影響を与えることなく、機能のテスト、実験の実行、カスタム設定をおこなうことができます。 サンドボックスについて詳しくは、「[サンドボックスの概要](../sandboxes/home.md)」を参照してください。

現在、Data Science Workspaceには次のサンドボックス制限があります。

- 計算リソースは、実稼動用サンドボックスと非実稼動用サンドボックスで共有されます。

## 次の手順

このドキュメントでは、Data Science Workspaceで使用できる様々なタイプのアクセスおよび機能について説明しました。

日々のワークフローの完了など、Data Science Workspaceの詳細については、まず「[Data Science Workspaceのチュートリアル](./walkthrough.md)」をお読みください。 一般情報については、「[Data Science Workspaceの概要](./home.md)」を参照してください。
