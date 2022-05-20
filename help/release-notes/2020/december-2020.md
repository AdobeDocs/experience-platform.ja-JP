---
title: Adobe Experience Platformリリースノート 2020 年 12 月
description: Adobe Experience Platformの 2020 年 12 月のリリースノート。
doc-type: release notes
last-update: December 9, 2020
author: ens60013 & ens72471
exl-id: 89d631f1-1b11-4a18-98e1-08e1d5bd8b0d
source-git-commit: ce967ae176fce81aa26d92b3f0ee8be006808657
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Adobe Experience Platform リリースノート

**リリース日：2020 年 12 月 10 日**

Adobe Experience Platform の新機能：

- [[!DNL Dataflows]](#dataflows)

Adobe Experience Platform の既存の機能に対するアップデート：

- [[!DNL Data Science Workspace]](#dsw)
- [[!DNL Sources]](#sources)

## [!DNL Dataflows] {#dataflows}

データフローは、Platform 間でデータを移動するデータジョブを表します。これらのデータフローは、異なるサービス間で設定され、ソースコネクタからターゲットデータセット、ID およびプロファイルサービス、宛先へのデータの移動に役立ちます。

**主な機能**

| 機能 | 説明 |
| ------- | ----------- |
| データフローの透明性 | ソースおよび宛先のデータフローを監視できます。 詳しくは、 [ソースの監視に関するチュートリアル](../../dataflows/ui/monitor-sources.md) または [宛先の監視に関するチュートリアル](../../dataflows/ui/monitor-destinations.md). |

データフローの詳細については、 [データフローの概要](../../dataflows/home.md).

## [!DNL Data Science Workspace] {#dsw}

Data Science Workspace は、機械学習と人工知能を使用して、データからインサイトを作成します。Adobe Experience Platform に統合された Data Science Workspace は、アドビソリューションでコンテンツやデータアセットを使用して予測をおこなうことを支援します。

**主な特長**

| 機能 | 説明 |
| --- | ---|
| Adobe Experience Platform Intelligence パッケージアドオン | Adobe Experience Platformインテリジェンスパッケージアドオンは、次のような追加の主要機能をロック解除する Data Science Workspace のアップグレードです。 <li> UI 駆動型モデルの実験と評価。</li><li> スケジュールに沿ったトレーニングジョブと参照ジョブを使用して、モデルをデプロイし、運用する機能。</li><li> Tensorflow モデル (GPU Compute) でのディープラーニングのサポート。</li><li> 大規模なデータセット（10MM +行）に対してトレーニングとスコアリングをおこなう Spark ベースの分散計算機。</li><li>その他</li> |

Adobe Experience Platformインテリジェンスパッケージアドオンの詳細については、 [Data Science Workspace のアクセスと機能](../../data-science-workspace/access-features-dsw.md).

## [!DNL Sources] {#sources}

Adobe Experience Platformで外部ソースからデータを取り込みながら、を使用してデータの構造化、ラベル付け、拡張をおこなうことができます。 [!DNL Platform] サービス。 アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**主な特長**

| 機能 | 説明 |
| ------- | ----------- |
| ストリーミングソースのアカウントと接続の詳細を更新 | 既存のストリーミング接続の名前、説明および資格情報を、 [!DNL Flow Service] API と UI 詳しくは、 [API を使用した接続の更新](../../sources/tutorials/api/update.md) および [UI を使用したアカウント詳細の編集](../../sources/tutorials/ui/monitor.md). |
| データフローの削除 | エラーを含む、または不要になったストリーミングデータフローは、 [!DNL Flow Service] API と UI 詳しくは、 [API を使用したデータフローの削除](../../sources/tutorials/api/delete-dataflows.md) および [UI を使用したデータフローの削除](../../sources/tutorials/ui/delete.md). |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
