---
title: Adobe Experience Platform リリースノート
description: Experience Platform リリースノート 2020 年 9 月 9 日
doc-type: release notes
last-update: September 8, 2020
author: crhoades, ens25212
translation-type: tm+mt
source-git-commit: adf8e8457c8ffef263223a38d3f9c345cf7c6ab2
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

Adobe Experience Platform データガバナンスは、顧客データを管理し、データの使用に適用される規制、制限、ポリシーへの準拠を確保するために使用される一連の戦略とテクノロジーです。[!DNL Experience Platform]内では、カタログ化、データ系列、データ使用状況のラベル付け、データアクセスポリシー、マーケティング活動のデータに関するアクセス制御など、様々なレベルで重要な役割を果たします。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| データセットのラベル付けUIの強化 | 大規模なスキーマでの作業を容易にするため、データセットのラベル付けUIに、新しい並べ替えとフィルタリングのコントロールがいくつか追加されました。 <ul><li>フルスキーマパスに基づいて、フィールドをアルファベット順に並べ替えます。</li><li>フィールドパス名に対して部分的な検索を実行します。</li><li>ラベル、選択したラベルまたはラベルカテゴリのないフィルタフィールド</li></ul> |

このサービスについて詳しくは、「[データガバナンスの概要](../../data-governance/home.md)」を参照してください。

## 宛先 {#destinations}

[リアルタイム顧客データプラットフォーム](../../rtcdp/overview.md)では、宛先は、それらのパートナーに対してシームレスにデータをアクティブ化する目的のプラットフォームとの事前に構築された統合です。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| UXの強化 | ユーザーはインラインテーブルアクションにアクセスでき、データの追加、スケジュールの編集、セグメントの追加などの主なアクションに簡単にアクセスできます。 詳しくは、[destinations workspace](../../destinations/ui/destinations-workspace.md)ドキュメントを参照してください。 |

詳しくは、[宛先の概要](../../destinations/home.md)を参照してください。

## [!DNL Observability Insights] {#observability}

[!DNL Observability Insights] 統計指標とイベント通知を使用して、Adobe Experience Platformのアクティビティを監視できます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| Adobe I/Oイベント通知 | [!DNL Observability Insights] adobe i/oイベントを利用して、複数のExperience Platformサービス向けのイベント通知を作成します。通知ペイロードは、設定済みのWebフックに送信され、このフックを使用して、下流のプロセスをさらに自動化できます。 詳しくは、[通知の概要](../../observability/notifications/overview.md)を参照してください。 |

サービスの詳細については、[[!DNL Observability Insights] 概要](../../observability/home.md)を参照してください。

## [!DNL Privacy Service] {#privacy}

法的規制や組織的規制の中には、ユーザーが要求に応じて、データストアから個人データにアクセスしたり削除したりする権利を与えるものもあります。 Adobe Experience Platform[!DNL Privacy Service]は、RESTful APIとユーザーインターフェイスを提供し、お客様からのこれらのデータリクエストを管理するのに役立ちます。 [!DNL Privacy Service]を使うと、個人顧客データや個人顧客データにアクセスして削除する要求をAdobe Experience Cloudのアプリケーションから送信でき、法的および組織のプライバシー規制への自動コンプライアンスが容易になります。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| LGPDのサポート（ブラジル） | ブラジルの[!DNL Lei Geral de Proteção de Dados](LGPD)規制の下で、プライバシージョブを作成できるようになりました。 これらのジョブは、規則コード`lgpd_bra`の下で追跡されます。 |

サービスの詳細については、[Privacy Serviceの概要](../../privacy-service/home.md)を参照してください。

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、いつでもどこでもブランドとのやり取りが顧客に対して調整され、一貫性と関連性のあるエクスペリエンスを提供できます。[!DNL Real-time Customer Profile]を使用すると、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルからのデータを組み合わせた個々の顧客の全体的な表示を確認できます。 [!DNL Profile] 個別の顧客データを統合表示に統合し、各顧客の操作に関する実用的でタイムスタンプのあるアカウントを提供できます。

| 機能 | 説明 |
| ------- | ----------- |
| プロファイルビューアー | プラットフォームUIのプロファイルビューアが、完全なカスタマイズが可能なダッシュボードに更新されました。 ユーザーは、次のタスクを行うことができます。 <ul><li>基本情報ウィジェットで、選択した標準属性とカスタマイズした属性を更新します。</li><li>カスタムウィジェットの作成、編集、削除</li><li>ウィジェットのサイズ変更と並べ替え</li></ul> |

[!DNL Real-time Customer Profile]について詳しくは、[!DNL Profile]データを操作するためのチュートリアルやベストプラクティスなど、に関する情報を[リアルタイム顧客プロファイルの概要](../../profile/home.md)を参照してください。

## セグメント化サービス {#segmentation}

Adobe Experience Platformセグメントサービスは、セグメントを作成して[!DNL Real-time Customer Profile]データからオーディエンスを生成するためのユーザーインターフェイスおよびRESTful APIを提供します。 これらのセグメントは[!DNL Platform]上で一元的に構成および管理され、どのAdobeアプリケーションでも容易にアクセスできます。

[!DNL Segmentation Service] 顧客ベース内のマーケティング可能な人々のグループを区別する基準を説明することで、特定のプロファイルのサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| ジョブの書き出し | セグメントを書き出しジョブの一部として評価できるフラグが追加されました。 その結果、セグメント化と書き出しの両方を1つのジョブで実行できます。 |
| 結合ポリシー | 単一のバッチセグメントジョブには、複数のマージポリシーを含めることができます。 |

[!DNL Segmentation Service]について詳しくは、[セグメントの概要](../../segmentation/home.md)を参照してください

## ソース {#sources}

Adobe Experience Platformは外部ソースからデータを取り込みながら、[!DNL Platform]サービスを使ってデータの構造、ラベル付け、拡張を行うことができます。 アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できるようにする RESTful API とインタラクティブな UI を提供します。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 自動マッピング | [!DNL Platform] は、ユーザーが選択したターゲットスキーマーまたはデータセットに基づいて、データ取り込みワークフロー中の自動マッピングに関するインテリジェントな推奨事項を提供します。柔軟な自動マッピングルールを手動で調整して、使用事例に合わせることができます。 |
| UXの強化 | ユーザーはインラインテーブルアクションにアクセスでき、データの追加、スケジュールの編集、セグメントの追加などの主なアクションに簡単にアクセスできます。 詳しくは、[監視データフロー](../../sources/tutorials/ui/monitor.md)ドキュメントを参照してください。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
