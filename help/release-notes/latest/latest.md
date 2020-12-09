---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2020年12月9日）
doc-type: release notes
last-update: December 9, 2020
author: ens60013 & ens72471
translation-type: tm+mt
source-git-commit: a411ac92d946080abd7b22b9d57c7154d263a30a
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 36%

---


# Adobe Experience Platform リリースノート

**リリース日：2020年12月9日**

Adobe Experience Platform の既存の機能のアップデート：

- [[!DNL Data Science Workspace]](#dsw)
- [[!DNL Sources]](#sources)

## [!DNL Data Science Workspace] {#dsw}

Data Science Workspaceは、機械学習と人工知能を使用して、データからインサイトを作成します。 Adobe Experience Platform に統合された Data Science Workspace は、アドビソリューションでコンテンツやデータアセットを使用して予測をおこなうことを支援します。

### 主な特長

| 機能 | 説明 |
|--- | ---|
| Adobe Experience Platform情報パッケージアドオン | Adobe Experience Platformインテリジェンスパッケージアドオンは、Data Science Workspaceのアップグレードで、次のような追加の主要機能をロック解除します。 <li> UI主導のモデル実験と評価。</li><li> 予定されたトレーニングジョブと参照ジョブを使用して、モデルを導入および操作する機能。</li><li> Tensorflowモデル(GPU Compute)での詳細な学習のサポート。</li><li> Sparkベースの分散コンピューティングにより、大規模なデータセット（10MM +行）に対してトレーニングとスコアを実施。</li><li>その他</li> |

Adobe Experience Platformインテリジェンスパッケージアドオンの詳細については、 [Data Science Workspaceのアクセスと機能に関するドキュメントを参照してください](../../data-science-workspace/access-features-dsw.md)。

## [!DNL Sources] {#sources}

Adobe Experience Platform can ingest data from external sources while allowing you to structure, label, and enhance that data using [!DNL Platform] services. アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できるようにする RESTful API とインタラクティブな UI を提供します。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**主な特長**

| 機能 | 説明 |
| ------- | ----------- |
| ストリーミングソースのアカウントと接続の詳細の更新 | APIとUIを使用して、既存のストリーミング接続の名前、説明および秘密鍵証明書を更新できるようにな [!DNL Flow Service] りました。 詳しくは、APIを使用した接続の [更新とUIを使用したアカウントの詳細の](../../sources/tutorials/api/update.md) 編集に関するチュートリアルを参照してください [](../../sources/tutorials/ui/monitor.md)。 |
| データフローの削除 | エラーを含む、または不要になったストリーミングデータフローを、 [!DNL Flow Service] APIとUIを使用して削除できるようになりました。 詳細は、APIを使用したデータ・フローの [削除およびUIを使用したデータ・フローの](../../sources/tutorials/api/delete-dataflows.md) 削除に関するチュートリアルを参照してください [](../../sources/tutorials/ui/delete.md)。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。

