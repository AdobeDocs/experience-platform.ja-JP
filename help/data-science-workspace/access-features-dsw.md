---
keywords: Experience Platform；ホーム；Data Science Workspace；人気の高いトピック；アクセス制御；サンドボックス；インテリジェンスパック；dsw機能；dswアクセス；Adobe Experience Platformインテリジェンス；インテリジェンス；aepインテリジェンスパッケージ
solution: Experience Platform
title: Data Science Workspaceのアクセスと機能
topic-legacy: Access and features for data science workspace
description: 次のドキュメントでは、Data Science Workspaceの権限と機能へのアクセスについて概説します。
exl-id: 6759fea4-adb9-4e4e-9f3d-e0e8c885b1dd
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '644'
ht-degree: 26%

---

# Data Science Workspaceのアクセスと機能

次のドキュメントでは、Data Science Workspaceの権限と機能へのアクセスについて概説します。

![DSWタブ](./images/access/platform-tabs.png)

- **ノートブック：Experience Platform** 上でデータの調査、分析、モデル化を行う、対話型の開発環境([JupterLab](./jupyterlab/overview.md))を提供します。
- **モデル：高度な機械学習レシピおよびモデルを作成、公開、保存するためのツール** です。詳細については、[機械学習モデルを作成して公開するチュートリアル](./models-recipes/create-publish-model.md)を参照してください。
- **サービス：インテリジェントサービスなどのAdobeが提供するサービス** と、Data Science Workspaceで作成した [カスタム](../intelligent-services/home.md) サービスの両方が含まれます。

「サービス」タブしか表示されないのはなぜですか。

- 組織は、インテリジェントサービスカスタマーAIを含むリアルタイムカスタマーデータプラットフォーム(RTCDP)のみを利用できます。

「**データサイエンス**」タブが見えない場合、Data Science Workspaceの機能を利用するには、会社管理者に連絡して、Adobe Experience Platformインテリジェンスライセンスがあるかどうかを確認してください。

## Adobe Experience Platform情報パッケージアドオン

次の表に、Data Science Workspaceの主な違いの一部を示します。Data Science Workspaceと、Adobe Experience Platformインテリジェンスパッケージアドオンの有無は次のとおりです。

>[!NOTE]
>
>複数のインテリジェンスパッケージアドオンをライセンス認証でき、拡張された容量がエンタイトルメント全体に追加されます。 たとえば、2つのAdobe Experience Platformインテリジェンスパッケージのライセンスを取得した場合、合計20人のノートブックユーザーに権利が付与されます。

|  | [!DNL Data Science Workspace] | [!DNL Data Science Workspace] インテリジェンスパッケージアドオン |
| --- | :---: | :---: |
| サポートされるノートブックユーザーの数。 | 5人の同時ユーザー | 1つ目のパックでは、同時に5人のユーザーが追加され、追加の購入では1つのパッケージにつき10人のユーザーが追加されます。 |
| Jupyterノートブックを統合し、探索的なデータ分析とモデルオーサリング(R、Python、Scala、PySpark)を可能にする | X | X |
| クエリサービスとのネイティブ統合。 ノートブックでSQLを使用してデータセットを調査および形成する機能。 | X | X |
| 予測分析用に事前に作成されたノートブックテンプレートにアクセスできます。 | X | X |
| Jupter Notebooksを使用して、モデルのトレーニングとスコアリングを手動で行います。 | X | X |
| トレーニングジョブと参照ジョブのスケジュール機能を使用して、モデルの導入と操作を行います。 |  | X |
| モデルを簡単に設定、評価、トレーニング、スコアリングおよび実稼動環境に公開するためのレシピフレームワーク。 |  | X |
| UI主導のモデル実験と評価。 |  | X |
| Tensorflowモデル(GPU Compute)の詳細な学習のサポート。 |  | X |
| Sparkベースの分散計算機。大規模なデータセット（10MM +行）に対してトレーニングを行い、スコアを計算します。 |  | X |

## アクセス制御

Experience Platform のアクセス制御は、[Adobe Admin Console](https://adminconsole.adobe.com) で管理されます。この機能は、Admin Console の製品プロファイルを利用して、権限およびサンドボックスを持つユーザーをリンクします。詳しくは、「[アクセス制御の概要](../access-control/home.md)」を参照してください。

 Data Science Workspace を使用するには、「Data Science Workspace の管理」権限を有効にする必要があります。次の表に、この権限を有効または無効にした場合の影響を示します。

| 権限 | 有効 | 無効 |
|---|---|---|
| Data Science Workspace の管理 | Data Science Workspace のすべてのサービスへのアクセスを提供します。 | Data Science Workspace内のすべてのサービスへのAPIとUIのアクセスが無効になっています。 無効にしている間は、**ノートブック**、**モデル**、**サービス**&#x200B;の各ページを選択できません。 <li>**サービス**&#x200B;へのアクセスは、リアルタイム顧客データプラットフォーム(RTCDP)を通じて引き続き利用できます。</li> |

## サンドボックスのサポート

サンドボックスは、Experience Platform の単一のインスタンス内の仮想パーティションです。Platform の各インスタンスは、1 つの実稼働用サンドボックスと複数の非実稼働用サンドボックスをサポートし、それぞれのサンドボックスが Platform リソースの独自のライブラリを保持します。非実稼働用サンドボックスを使用すると、実稼働用サンドボックスに影響を与えることなく、機能のテスト、実験の実行、カスタム設定をおこなうことができます。サンドボックスについて詳しくは、「[サンドボックスの概要](../sandboxes/home.md)」を参照してください。

現在、Data Science Workspaceには次のサンドボックスの制限があります。

- 計算リソースは、実稼働用サンドボックスと非実稼働用サンドボックスで共有されます。

## 次の手順

このドキュメントでは、Data Science Workspaceで使用できる様々なタイプのアクセスおよび機能について説明しました。

Data Science Workspaceの詳細（日々の完全なワークフローなど）については、まず[Data Science Workspaceのチュートリアル](./walkthrough.md)のドキュメントを読んでください。 一般的な情報については、[データサイエンスワークスペースの概要](./home.md)を参照してください。
