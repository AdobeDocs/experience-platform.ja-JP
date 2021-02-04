---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2020年12月9日）
doc-type: release notes
last-update: December 9, 2020
author: ens60013 & ens72471
translation-type: tm+mt
source-git-commit: ae353e6dda3f92647c32ee8e731be5785d24e5cb
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 34%

---


# Adobe Experience Platform リリースノート

**リリース日：2020年12月9日**

Adobe Experience Platform の新機能：

- [[!DNL Dataflows]](#dataflows)

Adobe Experience Platform の既存の機能のアップデート：

- [[!DNL Data Science Workspace]](#dsw)
- [[!DNL Sources]](#sources)

## [!DNL Dataflows] {#dataflows}

データフローは、Platform 間でデータを移動するデータジョブを表します。これらのデータフローは様々なサービスで構成され、ソース・コネクタからターゲット・データセット、IDおよびプロファイル・サービス、宛先にデータを移動できます。

**主な機能**

| 機能 | 説明 |
| ------- | ----------- |
| データフローの透過性 | ソースおよび宛先のデータフローを監視できます。 詳細については、監視ソース](../../dataflows/ui/monitor-sources.md)の[チュートリアル、または監視先](../../dataflows/ui/monitor-destinations.md)の[チュートリアルを参照してください。 |

データフローの詳細については、[データフローの概要](../../dataflows/home.md)を参照してください。

## [!DNL Data Science Workspace] {#dsw}

Data Science Workspaceは、機械学習と人工知能を使用して、データからインサイトを作成します。 Adobe Experience Platform に統合された Data Science Workspace は、アドビソリューションでコンテンツやデータアセットを使用して予測をおこなうことを支援します。

**主な特長**

| 機能 | 説明 |
| --- | ---|
| Adobe Experience Platform情報パッケージアドオン | Adobe Experience Platformインテリジェンスパッケージアドオンは、Data Science Workspaceのアップグレードで、次のような追加の主要機能をロック解除します。 <li> UI主導のモデル実験と評価。</li><li> 予定されたトレーニングジョブと参照ジョブを使用して、モデルを導入および操作する機能。</li><li> Tensorflowモデル(GPU Compute)での詳細な学習のサポート。</li><li> Sparkベースの分散コンピューティングにより、大規模なデータセット（10MM +行）に対してトレーニングとスコアを実施。</li><li>その他</li> |

Adobe Experience Platformインテリジェンスパッケージアドオンの詳細については、[Data Science Workspaceのアクセスと機能](../../data-science-workspace/access-features-dsw.md)に関するドキュメントを参照してください。

## [!DNL Sources] {#sources}

Adobe Experience Platformは外部ソースからデータを取り込みながら、[!DNL Platform]サービスを使ってデータの構造、ラベル付け、拡張を行うことができます。 アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できるようにする RESTful API とインタラクティブな UI を提供します。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**主な特長**

| 機能 | 説明 |
| ------- | ----------- |
| ストリーミングソースのアカウントと接続の詳細の更新 | [!DNL Flow Service] APIとUIを使用して、既存のストリーミング接続の名前、説明、資格情報を更新できるようになりました。 詳しくは、[API](../../sources/tutorials/api/update.md)を使用した接続の更新および[UI](../../sources/tutorials/ui/monitor.md)を使用したアカウントの詳細の編集に関するチュートリアルを参照してください。 |
| データフローの削除 | エラーを含む、または不要になったストリーミングデータフローを、[!DNL Flow Service] APIとUIを使用して削除できるようになりました。 詳細については、[API](../../sources/tutorials/api/delete-dataflows.md)を使用したデータフローの削除および[UI](../../sources/tutorials/ui/delete.md)を使用したデータフローの削除に関するチュートリアルを参照してください。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。


