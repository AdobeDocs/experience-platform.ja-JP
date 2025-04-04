---
title: Adobe Experience Platform リリースノート 2020 年 12 月
description: Adobe Experience Platformの 2020 年 12 月のリリースノート。
doc-type: release notes
last-update: December 9, 2020
author: ens60013 & ens72471
exl-id: 89d631f1-1b11-4a18-98e1-08e1d5bd8b0d
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '433'
ht-degree: 28%

---

# Adobe Experience Platform リリースノート

**リリース日：2020 年 12 月 9 日**

Adobe Experience Platform の新機能：

- [[!DNL Dataflows]](#dataflows)

Adobe Experience Platform の既存の機能に対するアップデート：

- [[!DNL Data Science Workspace]](#dsw)
- [[!DNL Sources]](#sources)

## [!DNL Dataflows] {#dataflows}

データフローは、Experience Platform間でデータを移動するデータジョブを表します。 これらのデータフローは異なるサービスをまたいで設定され、ソースコネクタからターゲットデータセット、ID およびプロファイルサービス、宛先へとデータを移動できます。

**主な機能**

| 機能 | 説明 |
| ------- | ----------- |
| データフローの透明性 | ソースと宛先のデータフローを監視できます。 詳しくは、[ ソースの監視に関するチュートリアル ](../../dataflows/ui/monitor-sources.md) または [ 宛先の監視に関するチュートリアル ](../../dataflows/ui/monitor-destinations.md) を参照してください。 |

データフローについて詳しくは、[ データフローの概要 ](../../dataflows/home.md) を参照してください。

## [!DNL Data Science Workspace] {#dsw}

Data Science Workspace は、機械学習と人工知能を使用して、データからインサイトを作成します。Adobe Experience Platform に統合された Data Science Workspace は、アドビソリューションでコンテンツやデータアセットを使用して予測をおこなうことを支援します。

**主な特長**

| 機能 | 説明 |
| --- | ---|
| Adobe Experience Platform Intelligence パッケージアドオン | Adobe Experience Platform Intelligence パッケージのアドオンは、次のような主な機能をさらに活用できる、Data Science Workspaceのアップグレードです。 <li> UI 駆動モデルの実験と評価。</li><li> スケジュールされたトレーニングと推論ジョブを使用してモデルをデプロイし、運用する機能。</li><li> Tensorflow モデル（GPU コンピューティング）でのディープラーニングのサポート。</li><li> Spark ベースの分散コンピューティングで、大規模なデータセット（10 MM +行）に対してトレーニングとスコアリングを行います。</li><li>さらに件</li> |

Adobe Experience Platform Intelligence パッケージアドオンについて詳しくは、[Data Science Workspaceのアクセスと機能 ](../../data-science-workspace/access-features-dsw.md) に関するドキュメントを参照してください。

## [!DNL Sources] {#sources}

Adobe Experience Platformでは、外部ソースからデータを取り込むときに、[!DNL Experience Platform] サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。 アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を提供します。 これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**主な特長**

| 機能 | 説明 |
| ------- | ----------- |
| ストリーミングソースのアカウントと接続の詳細を更新 | [!DNL Flow Service] API と UI を使用して、既存のストリーミング接続の名前、説明、資格情報を更新できるようになりました。 詳しくは、[API を使用した接続の更新 ](../../sources/tutorials/api/update.md) および [UI を使用したアカウント詳細の編集 ](../../sources/tutorials/ui/monitor.md) に関するチュートリアルを参照してください。 |
| データフローの削除 | エラーを含むストリーミングデータフローまたは不要になったストリーミングデータフローを、[!DNL Flow Service] API と UI を使用して削除できるようになりました。 詳しくは、[API を使用したデータフローの削除 ](../../sources/tutorials/api/delete-dataflows.md) および [UI を使用したデータフローの削除 ](../../sources/tutorials/ui/delete.md) のチュートリアルを参照してください。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
