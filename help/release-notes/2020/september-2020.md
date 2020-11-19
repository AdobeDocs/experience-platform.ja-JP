---
title: Adobe Experience Platform リリースノート
description: Experience Platform リリースノート 2020 年 9 月 9 日
doc-type: release notes
last-update: September 8, 2020
author: crhoades, ens25212
translation-type: tm+mt
source-git-commit: 0232acdc64019b9d93888e8137ef9bc8e114779b
workflow-type: tm+mt
source-wordcount: '862'
ht-degree: 27%

---


# Adobe Experience Platform リリースノート

**リリース日：2020 年 9 月 10 日**

Adobe Experience Platform の既存の機能のアップデート：

- [[!DNL Data Governance]](#governance)
- [[!DNL Destinations]](#destinations)
- [[!DNL Observability Insights]](#observability)
- [[!DNL Privacy Service]](#privacy)
- [[!DNL Real-time Customer Profile]](#profile)
- [[!DNL Segmentation Service]](#segmentation)
- [[!DNL Sources]](#sources)

## [!DNL Data Governance] {#governance}

Adobe Experience Platform データガバナンスは、顧客データを管理し、データの使用に適用される規制、制限、ポリシーへの準拠を確保するために使用される一連の戦略とテクノロジーです。It plays a key role within [!DNL Experience Platform] at various levels, including cataloging, data lineage, data usage labeling, data access policies, and access control on data for marketing actions.

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| データセットのラベル付けUIの強化 | 大規模なスキーマでの作業を容易にするため、データセットのラベル付けUIに、新しい並べ替えとフィルタリングのコントロールがいくつか追加されました。 <ul><li>フルスキーマパスに基づいて、フィールドをアルファベット順に並べ替えます。</li><li>フィールドパス名に対して部分的な検索を実行します。</li><li>ラベル、選択したラベルまたはラベルカテゴリのないフィルタフィールド</li></ul> |

このサービスについて詳しくは、「[データガバナンスの概要](../../data-governance/home.md)」を参照してください。

## 宛先 {#destinations}

In [Real-time Customer Data Platform](../../rtcdp/overview.md), destinations are pre-built integrations with destination platforms that activate data to those partners in a seamless way.

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| UXの強化 | ユーザーはインラインテーブルアクションにアクセスでき、データの追加、スケジュールの編集、セグメントの追加などの主なアクションに簡単にアクセスできます。 See the [destinations workspace](../../rtcdp/destinations/destinations-workspace.md) document for more information. |

詳しくは、[宛先の概要](../../rtcdp/destinations/destinations-overview.md)を参照してください。

## [!DNL Observability Insights] {#observability}

[!DNL Observability Insights] 統計指標とイベント通知を使用して、Adobe Experience Platformのアクティビティを監視できます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| AdobeI/Oイベント通知 | [!DNL Observability Insights] adobeI/Oイベントを利用して、いくつかのExperience Platformサービスのイベント通知を作成します。 通知ペイロードは、設定済みのWebフックに送信され、このフックを使用して、下流のプロセスをさらに自動化できます。 See the [notifications overview](../../observability/notifications/overview.md) for more information. |

See the [[!DNL Observability Insights] overview](../../observability/home.md) for more information on the service.

## [!DNL Privacy Service] {#privacy}

法的規制や組織的規制の中には、ユーザーが要求に応じて、データストアから個人データにアクセスしたり削除したりする権利を与えるものもあります。 Adobe Experience Platform [!DNL Privacy Service] provides a RESTful API and user interface to help you manage these data requests from your customers. With [!DNL Privacy Service], you can submit requests to access and delete private or personal customer data from Adobe Experience Cloud applications, facilitating automated compliance with legal and organizational privacy regulations.

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| LGPDのサポート（ブラジル） | ブラジル [!DNL Lei Geral de Proteção de Dados] (LGPD)の規制に基づき、プライバシー・ジョブを作成できるようになった。 これらのジョブは、規制コードの下で追跡され `lgpd_bra`ます。 |

See the [Privacy Service overview](../../privacy-service/home.md) for more information on the service.

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、いつでもどこでもブランドとのやり取りが顧客に対して調整され、一貫性と関連性のあるエクスペリエンスを提供できます。With [!DNL Real-time Customer Profile], you can see a holistic view of each individual customer that combines data from multiple channels, including online, offline, CRM, and third party data. [!DNL Profile] 個別の顧客データを統合表示に統合し、各顧客の操作に関する実用的でタイムスタンプのあるアカウントを提供できます。

| 機能 | 説明 |
| ------- | ----------- |
| プロファイルビューアー | プラットフォームUIのプロファイルビューアが、完全なカスタマイズが可能なダッシュボードに更新されました。 ユーザーは、次のタスクを行うことができます。 <ul><li>基本情報ウィジェットで、選択した標準属性とカスタマイズした属性を更新します。</li><li>カスタムウィジェットの作成、編集、削除</li><li>ウィジェットのサイズ変更と並べ替え</li></ul> |

For more information on [!DNL Real-time Customer Profile], including tutorials and best practices for working with [!DNL Profile] data, please read the [Real-time Customer Profile overview](../../profile/home.md).

## セグメント化サービス {#segmentation}

Adobe Experience Platform Segmentation Service provides a user interface and RESTful API that allows you to build segments and generate audiences from your [!DNL Real-time Customer Profile] data. These segments are centrally configured and maintained on [!DNL Platform], making them readily accessible by any Adobe application.

[!DNL Segmentation Service] 顧客ベース内のマーケティング可能な人々のグループを区別する基準を説明することで、特定のプロファイルのサブセットを定義します。 セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| ジョブの書き出し | セグメントを書き出しジョブの一部として評価できるフラグが追加されました。 その結果、セグメント化と書き出しの両方を1つのジョブで実行できます。 |
| 結合ポリシー | 単一のバッチセグメントジョブには、複数のマージポリシーを含めることができます。 |

For more information on [!DNL Segmentation Service], please see the [Segmentation overview](../../segmentation/home.md)

## ソース {#sources}

Adobe Experience Platform can ingest data from external sources while allowing you to structure, label, and enhance that data using [!DNL Platform] services. アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できるようにする RESTful API とインタラクティブな UI を提供します。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 自動マッピング | [!DNL Platform] は、ユーザーが選択したターゲットスキーマーまたはデータセットに基づいて、データ取り込みワークフロー中の自動マッピングに関するインテリジェントな推奨事項を提供します。 柔軟な自動マッピングルールを手動で調整して、使用事例に合わせることができます。 |
| UXの強化 | ユーザーはインラインテーブルアクションにアクセスでき、データの追加、スケジュールの編集、セグメントの追加などの主なアクションに簡単にアクセスできます。 詳細は、 [監視データフロー](../../sources/tutorials/ui/monitor.md) ドキュメントを参照してください。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
