---
title: Adobe Experience Platform リリースノート
description: Experience Platform リリースノート 2020 年 9 月 9 日
doc-type: release notes
last-update: September 8, 2020
author: crhoades, ens28527
translation-type: tm+mt
source-git-commit: 64b6b59923d549cdcbf35d2e375529aec8cf81b8
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 45%

---


# Adobe Experience Platform リリースノート

**リリース日：2020 年 9 月 9 日**

Adobe Experience Platformの既存の機能の更新：

* [[!DNLデータガバナンス]](#governance)
* [[!DNL宛先]](#destinations)
* [[!DNLソース]](#sources)

## [!DNL Data Governance] {#governance}

Adobe Experience Platform データガバナンスは、顧客データを管理し、データの使用に適用される規制、制限、ポリシーへの準拠を確保するために使用される一連の戦略とテクノロジーです。It plays a key role within [!DNL Experience Platform] at various levels, including cataloging, data lineage, data usage labeling, data access policies, and access control on data for marketing actions.

**新機能**

| 機能 | 説明 |
| --- | --- |
| データセットのラベル付けUIの強化 | 大規模なスキーマでの作業を容易にするため、データセットのラベル付けUIに、新しい並べ替えとフィルタリングのコントロールがいくつか追加されました。 <ul><li>フルスキーマパスに基づいて、フィールドをアルファベット順に並べ替えます。</li><li>フィールドパス名に対して部分的な検索を実行します。</li><li>ラベル、選択したラベルまたはラベルカテゴリのないフィルタフィールド</li></ul> |

このサービスについて詳しくは、「[データガバナンスの概要](../../data-governance/home.md)」を参照してください。

## 宛先 {#destinations}

[アドビのリアルタイム顧客データプラットフォーム](../../rtcdp/overview.md) において、宛先は宛先プラットフォームとの事前に構築された統合であり、これらのパートナーに対してシームレスにデータを活用します。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| UXの強化 | ユーザーはインラインテーブルアクションにアクセスでき、データの追加、スケジュールの編集、セグメントの追加などの主なアクションに簡単にアクセスできます。 See the [destinations workspace](../../rtcdp/destinations/destinations-workspace.md) document for more information. |

詳しくは、[宛先の概要](../../rtcdp/destinations/destinations-overview.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform can ingest data from external sources while allowing you to structure, label, and enhance that data using [!DNL Platform] services. アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できるようにする RESTful API とインタラクティブな UI を提供します。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 自動マッピング | [!DNL Platform] は、ユーザーが選択したターゲットスキーマーまたはデータセットに基づいて、データ取り込みワークフロー中の自動マッピングに関するインテリジェントな推奨事項を提供します。 柔軟な自動マッピングルールを手動で調整して、使用事例に合わせることができます。 |
| UXの強化 | ユーザーはインラインテーブルアクションにアクセスでき、データの追加、スケジュールの編集、セグメントの追加などの主なアクションに簡単にアクセスできます。 詳細は、 [監視データフロー](../../sources/tutorials/ui/monitor.md) ドキュメントを参照してください。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。