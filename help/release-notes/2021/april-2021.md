---
title: Adobe Experience Platform リリースノート 2021 年 4 月
description: Adobe Experience Platformの 2021 年 4 月のリリースノート。
doc-type: release notes
last-update: April 21, 2021
author: ens72741
exl-id: cc78e48a-3578-4c55-ae86-1946d62bddb9
source-git-commit: ce967ae176fce81aa26d92b3f0ee8be006808657
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Adobe Experience Platform リリースノート

**リリース日：2021 年 4 月 21 日（PT）**

Adobe Experience Platform の既存の機能のアップデート：

- [[!DNL Data Prep]](#data-prep)
- [[!DNL Experience Data Model (XDM)]](#xdm)
- [[!DNL Intelligent Services]](#intelligent-services)
- [[!DNL Segmentation Service]](#segmentation)
- [[!DNL Sources]](#sources)

## [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] を使用すると、データエンジニアは Experience Data Model（XDM）との間でデータのマッピング、変換、検証をおこなうことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 既存のデータフローのマッピング編集のサポート | 既存のデータフローのマッピングセットを更新できるようになりました。1 回限りの取り込み用にスケジュールされたデータフローのマッピングセットは更新できません。この機能は、HTTP API、Adobe Analytics、Adobe Audience Manager、および [!DNL Marketo Engage] ではサポートされていません。詳しくは、[UI でのソースデータフローの更新](../../sources/tutorials/ui/update-dataflows.md)に関するチュートリアルを参照してください。 |
| ストリーミング取得のサポート | ストリーミングソース接続を作成する際に、データ準備関数を使用できるようになりました。詳しくは、[UI でのストリーミングソースデータ接続の作成](../../sources/tutorials/ui/create/streaming/http.md)に関するチュートリアルを参照してください。 |

詳しくは、[[!DNL Data Prep]  の概要](../../data-prep/home.md)を参照してください。

## [!DNL Experience Data Model (XDM)] {#xdm}

エクスペリエンスデータモデル（XDM）は、デジタルエクスペリエンスを向上するよう設計されたオープンソースの仕様です。Adobe Experience Platform 上のサービスと通信するすべてのアプリケーションに共通の構造と定義を提供します。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

| 機能 | 説明 |
| --- | --- |
| 業界別のスキーマレコメンデーション | スキーマエディターの UI でクラスとスキーマフィールドグループを選択する場合、新しいフィルターを使用すると、特定の業界に基づいて推奨される標準コンポーネントを表示できます。様々な業界の使用例については、これらのコンポーネントの相関関係について詳しくは、[業界データモデル](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/industries/overview.html?lang=ja)のドキュメントを参照してください。 |

## [!DNL Intelligent Services] {#intelligent-services}

インテリジェントサービスは、マーケティングアナリストや実践者に対して、顧客体験の使用事例で人工知能と機械学習の機能を活用する機能を提供します。これにより、マーケティングアナリストは、データ科学の専門知識を必要とせずに、ビジネスレベルの設定を使用して、会社のニーズに固有の予測を設定できます。

### 顧客 AI

リアルタイム顧客データプラットフォームで使用できる顧客 AI は、個々のプロファイルのチャーンやコンバージョンなどのカスタム傾向スコアを大規模に生成するために使用します。ビジネスニーズから機械学習の問題への変換、アルゴリズムの選択、トレーニング、デプロイは必要ありません。

| 機能 | 説明 |
| ------- | ----------- |
| Adobe Analytics データのサポート | サポート機能が更新され、消費者エクスペリエンスイベント（CEE）スキーマに従ってデータの ETL を実行しなくても、Analytics ソースコネクタ経由で Adobe Analytics データセットをサポートするようになりました。 |
| Adobe Audience Manager データのサポート | サポート機能が更新され、消費者エクスペリエンスイベント（CEE）スキーマに従ってデータの ETL を実行しなくても、Analytics Manager ソースコネクタ経由で Adobe Audience Manager データセットをサポートするようになりました。 |
| モデルのパフォーマンスの概要 | 顧客 AI のサービスインスタンスインサイトページに[「モデルパフォーマンスの概要」タブ](../../intelligent-services/customer-ai/user-guide/discover-insights.md#performance-metrics)が表示されるようになりました。「モデルパフォーマンス」タブには、実際のコンバージョン率とチャーン率がすべて表示されます。これにより、各傾向バケットで何が起きているかを読み解き、把握できます。 |

サポートされるデータセットの詳細については、[[!DNL Intelligent Services] データ準備のドキュメント](../../intelligent-services/data-preparation.md)を参照してください。

### Attribution AI

アトリビューション AI は、コンバージョンイベントにつながるタッチポイントの貢献度を明らかにするために使用します。これは、マーケターが、カスタマージャーニーをまたいだ個別マーケティングタッチポイントのマーケティング効果を、マーケターが、定量化する際に役立ちます。

| 機能 | 説明 |
| ------- | ----------- |
| Adobe Analytics データのサポート | サポート機能が更新され、消費者エクスペリエンスイベント（CEE）スキーマに従ってデータの ETL を実行しなくても、Analytics ソースコネクタ経由で Adobe Analytics データセットをサポートするようになりました。 |

サポートされるデータセットの詳細については、[[!DNL Intelligent Services] データ準備のドキュメント](../../intelligent-services/data-preparation.md)を参照してください。

## セグメント化サービス {#segmentation}

Adobe Experience Platform セグメント化サービスは、セグメントを作成し、[!DNL Real-time Customer Profile] データからオーディエンスを生成できるユーザーインターフェイスおよび RESTful API を提供します。これらのセグメントは、Platform 上で一元的に設定および管理され、アドビのアプリケーションから簡単にアクセスできます。

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| その他の集計関数 | セグメントビルダーにカウント関数が追加されました。カウント関数を使用すると、指定したイベントがおこなわれた回数をカウントできます。カウント関数について詳しくは、『[セグメントビルダーガイド](../../segmentation/ui/segment-builder.md#count-functions)』の「カウント関数」の節を参照してください。 |

[!DNL Segmentation Service] について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## [!DNL Sources] {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

| 機能 | 説明 |
| ------- | ----------- |
| [!DNL Marketo Engage] （ベータ版） | UI を使用して [!DNL Marketo Engage] ソース接続を作成し、B2B データを Platform に取り込み、Platform 接続アプリケーションでこのデータを最新の状態に保つことができるようになりました。詳しくは、[[!DNL Marketo Engage] ソースコネクタのドキュメント](../../sources/connectors/adobe-applications/marketo/marketo.md)を参照してください。 |
| ベータ版ソースの一般公開（GA） | 以下のソースがベータ版から一般公開（GA）に昇格しました。 <ul><li>[[!DNL Amazon Kinesis]](../../sources/connectors/cloud-storage/kinesis.md)</li><li>[[!DNL Azure EventHubs]](../../sources/connectors/cloud-storage/eventhub.md)</li><li>[[!DNL HTTP API]](../../sources/connectors/streaming/http.md)</li><li>[[!DNL MariaDB]](../../sources/connectors/databases/mariadb.md)</li><li>[[!DNL Microsoft SQL Server]](../../sources/connectors/databases/sql-server.md)</li><li>[[!DNL Oracle]](../../sources/connectors/databases/oracle.md)</li></ul> |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
