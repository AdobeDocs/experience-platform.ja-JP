---
title: Adobe Experience Platform リリースノート
description: 2021年4月22日Experience Platformリリースノート
doc-type: release notes
last-update: April 21, 2021
author: ens72741
translation-type: tm+mt
source-git-commit: 0c9b60fe0777286819841c520a41007634622578
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 35%

---


# Adobe Experience Platform リリースノート

**リリース日：2021 年 4 月 21 日**

Adobe Experience Platform の既存の機能のアップデート：

- [[!DNL Data Prep]](#data-prep)
- [[!DNL Intelligent Services]](#intelligent-services)
- [[!DNL Segmentation Service]](#segmentation)
- [[!DNL Sources]](#sources)

## [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] データエンジニアがエクスペリエンスデータモデル(XDM)との間でデータのマッピング、変換、検証を行えるようにします。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 既存のデータフローのマッピングの編集のサポート | 既存のデータフローのマッピング・セットを更新できるようになりました。 1回の取り込みにスケジュールされたデータフローのマッピングセットは更新できません。 この機能は、HTTP API、Adobe Analytics、Adobe Audience Manager、および[!DNL Marketo Engage]ではサポートされていません。 詳しくは、UI](../../sources/tutorials/ui/update-dataflows.md)の[ソースデータフローの更新に関するチュートリアルを参照してください。 |
| ストリーミング取り込みのサポート | ストリーミングソース接続の作成時に、データ準備機能を使用できるようになりました。 詳しくは、UI](../../sources/tutorials/ui/create/streaming/http.md)での[ストリーミングソース接続の作成に関するチュートリアルを参照してください。 |

詳しくは、[[!DNL Data Prep] 概要](../../data-prep/home.md)を参照してください。

## [!DNL Intelligent Services] {#intelligent-services}

インテリジェントサービスは、マーケティングアナリストや実践者に対して、顧客体験の使用事例で人工知能と機械学習の機能を活用する機能を提供します。これにより、マーケティングアナリストは、データ科学の専門知識を必要とせずに、ビジネスレベルの設定を使用して、会社のニーズに固有の予測を設定できます。

### 顧客 AI

リアルタイム顧客データプラットフォームで利用できる顧客AIは、個々のプロファイルの傾向スコア（チャーンやコンバージョンなど）を規模別に生成する場合に使用します。 ビジネスニーズから機械学習の問題への変換、アルゴリズムの選択、トレーニング、デプロイは必要ありません。

| 機能 | 説明 |
| ------- | ----------- |
| Adobe Analyticsデータのサポート | Analyticsソースコネクターを介したAdobe Analyticsデータセットをサポートする機能が更新されました。コンシューマーエクスペリエンスイベント(CEE)スキーマに準拠するために、データをETLする必要はありません。 |
| Adobe Audience Managerデータのサポート | Audience Managerソースコネクタを介したAdobe Audience Managerデータセットをサポートする機能が更新されました。コンシューマーエクスペリエンスイベント(CEE)スキーマに準拠するために、データをETLで送信する必要はありません。 |
| モデルのパフォーマンスの概要 | 顧客AIのサービスインスタンスインサイトページ内に、[モデルのパフォーマンスの概要タブ](../../intelligent-services/customer-ai/user-guide/discover-insights.md#performance-metrics)が表示されるようになりました。 [モデルのパフォーマンス]タブには、実際のコンバージョン率と可変コンバージョン率がすべて表示されます。 これにより、各傾向グループで発生していることを判断し、把握できます。 |

サポートされるデータセットの詳細については、[[!DNL Intelligent Services] データ準備ドキュメント](../../intelligent-services/data-preparation.md)を参照してください。

### Attribution AI

Attribution AI は、コンバージョンイベントにつながるタッチポイントの貢献度を明らかにするために使用します。これは、カスタマージャーニーにわたる個々のマーケティングタッチポイントのマーケティング効果をマーケターが定量化するのに役立ちます。

| 機能 | 説明 |
| ------- | ----------- |
| Adobe Analyticsデータのサポート | Analyticsソースコネクターを介したAdobe Analyticsデータセットをサポートする機能が更新されました。コンシューマーエクスペリエンスイベント(CEE)スキーマに準拠するために、データをETLする必要はありません。 |

サポートされるデータセットの詳細については、[[!DNL Intelligent Services] データ準備ドキュメント](../../intelligent-services/data-preparation.md)を参照してください。

## セグメント化サービス {#segmentation}

Adobe Experience Platformセグメントサービスは、セグメントを作成して[!DNL Real-time Customer Profile]データからオーディエンスを生成するためのユーザーインターフェイスおよびRESTful APIを提供します。 これらのセグメントは、Platform 上で一元的に設定および管理され、アドビのアプリケーションから簡単にアクセスできます。

[!DNL Segmentation Service] 顧客ベース内のマーケティング可能な人々のグループを区別する基準を説明することで、特定のプロファイルのサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| その他の集計関数 | セグメントビルダーにカウント機能が追加されました。 カウント関数を使用すると、指定したイベントが実行された回数をカウントできます。 カウント関数について詳しくは、[セグメントビルダーガイド](../../segmentation/ui/segment-builder.md#count-functions)のカウント関数の節を参照してください |

[!DNL Segmentation Service]について詳しくは、[セグメントの概要](../../segmentation/home.md)を参照してください。

## [!DNL Sources] {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform には、様々なデータプロバイダーへのソース接続を簡単に設定できる RESTful API とインタラクティブ UI が用意されています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

| 機能 | 説明 |
| ------- | ----------- |
| [!DNL Marketo Engage] (ベータ) | UIを使って[!DNL Marketo Engage]ソース接続を作成し、B2Bデータをプラットフォームに送り、プラットフォームに接続されたアプリケーションを使ってこのデータを最新の状態に保つことができるようになりました。 詳しくは、[[!DNL Marketo Engage] ソースコネクタのドキュメント](../../sources/connectors/adobe-applications/marketo/marketo.md)を参照してください。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
