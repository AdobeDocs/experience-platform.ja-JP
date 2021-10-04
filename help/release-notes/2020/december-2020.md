---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート 2020 年 12 月 9 日
doc-type: release notes
last-update: December 9, 2020
author: ens60013 & ens72471
exl-id: 89d631f1-1b11-4a18-98e1-08e1d5bd8b0d
source-git-commit: 0085306a2f5172eb19590cc12bc9645278bd2b42
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 37%

---

# Adobe Experience Platform リリースノート

**リリース日：2020 年 12 月 10 日**

Adobe Experience Platform の新機能：

- [[!DNL Dataflows]](#dataflows)

Adobe Experience Platform の既存の機能のアップデート：

- [[!DNL Data Science Workspace]](#dsw)
- [[!DNL Sources]](#sources)

## [!DNL Dataflows] {#dataflows}

データフローは、Platform 間でデータを移動するデータジョブを表します。これらのデータフローは、異なるサービス間で設定され、ソースコネクタからターゲットデータセット、ID およびプロファイルサービス、宛先へのデータの移動に役立ちます。

**主な機能**

| 機能 | 説明 |
| ------- | ----------- |
| データフローの透明性 | ソースおよび宛先のデータフローを監視できます。 詳しくは、[ ソースの監視に関するチュートリアル ](../../dataflows/ui/monitor-sources.md) または [ 宛先の監視に関するチュートリアル ](../../dataflows/ui/monitor-destinations.md) を参照してください。 |

データフローの詳細については、[ データフローの概要 ](../../dataflows/home.md) を参照してください。

## [!DNL Data Science Workspace] {#dsw}

Data Science Workspace は、機械学習と人工知能を使用して、データからインサイトを作成します。Adobe Experience Platform に統合された Data Science Workspace は、アドビソリューションでコンテンツやデータアセットを使用して予測をおこなうことを支援します。

**主な特長**

| 機能 | 説明 |
| --- | ---|
| Adobe Experience Platform Intelligence パッケージアドオン | Adobe Experience Platformインテリジェンスパッケージアドオンは、次のような追加の主要機能をロック解除する Data Science Workspace のアップグレードです。 <li> UI 主導型モデルの実験と評価。</li><li> スケジュールに沿ったトレーニングと参照ジョブを使用して、モデルをデプロイして操作する機能。</li><li> Tensorflow モデル (GPU Compute) でのディープラーニングのサポート。</li><li> 大規模なデータセット（10MM +行）に対してトレーニングとスコアリングをおこなう Spark ベースの分散計算。</li><li>その他</li> |

Adobe Experience Platformインテリジェンスパッケージアドオンの詳細については、[Data Science Workspace のアクセスと機能 ](../../data-science-workspace/access-features-dsw.md) に関するドキュメントを参照してください。

## [!DNL Sources] {#sources}

Adobe Experience Platformでは、外部ソースからデータを取り込みながら、[!DNL Platform] サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。 アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**主な特長**

| 機能 | 説明 |
| ------- | ----------- |
| ストリーミングソースのアカウントと接続の詳細の更新 | [!DNL Flow Service] API と UI を使用して、既存のストリーミング接続の名前、説明、資格情報を更新できるようになりました。 詳しくは、[API](../../sources/tutorials/api/update.md) を使用した接続の更新と [UI](../../sources/tutorials/ui/monitor.md) を使用したアカウントの詳細の編集に関するチュートリアルを参照してください。 |
| データフローの削除 | エラーを含む、または不要になったストリーミングデータフローは、[!DNL Flow Service] API と UI を使用して削除できるようになりました。 詳しくは、[API](../../sources/tutorials/api/delete-dataflows.md) を使用したデータフローの削除および [UI](../../sources/tutorials/ui/delete.md) を使用したデータフローの削除に関するチュートリアルを参照してください。 |

ソースについて詳しくは、 [ソースの概要](../../sources/home.md) を参照してください。
