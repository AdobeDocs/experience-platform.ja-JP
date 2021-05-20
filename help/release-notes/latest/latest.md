---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2021年4月22日）
doc-type: release notes
last-update: April 21, 2021
author: ens72741
exl-id: 8f2c9bf8-1487-46e4-993b-bd9b63774cab
source-git-commit: ab0798851e5f2b174d9f4241ad64ac8afa20a938
workflow-type: tm+mt
source-wordcount: '885'
ht-degree: 36%

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
| 既存のデータフローのマッピングの編集のサポート | 既存のデータフローのマッピングセットを更新できるようになりました。 1回限りの取り込み用にスケジュールされたデータフローのマッピングセットは更新できません。 この機能は、HTTP API、Adobe Analytics、Adobe Audience Manager、および[!DNL Marketo Engage]ではサポートされていません。 詳しくは、UI](../../sources/tutorials/ui/update-dataflows.md)でのソースデータフローの更新に関するチュートリアルを参照してください。[ |
| ストリーミング取得のサポート | ストリーミングソース接続を作成する際に、データ準備関数を使用できるようになりました。 詳しくは、UIでのストリーミングソース接続の作成に関するチュートリアル([)を参照してください。](../../sources/tutorials/ui/create/streaming/http.md) |

詳しくは、[[!DNL Data Prep]  の概要](../../data-prep/home.md)を参照してください。

## [!DNL Experience Data Model (XDM)] {#xdm}

エクスペリエンスデータモデル(XDM)は、デジタルエクスペリエンスのパワーを向上させるように設計されたオープンソース仕様です。 Adobe Experience Platform 上のサービスと通信するすべてのアプリケーションに共通の構造と定義を提供します。XDM標準に準拠することで、すべての顧客体験データを共通の表現に組み込み、より迅速で統合的な方法でインサイトを提供できます。 顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

| 機能 | 説明 |
| --- | --- |
| 業界別のスキーマレコメンデーション | スキーマエディターのUIでクラスとスキーマフィールドグループを選択する場合、新しいフィルターを使用して、特定の業界に基づいて推奨される標準コンポーネントを表示できます。 様々な業界の使用例に関して、これらのコンポーネントが互いにどのように関連し合うかについて詳しくは、[業界データモデル](https://www.adobe.com/go/xdm-industry-erds-en)のドキュメントを参照してください。 |

## [!DNL Intelligent Services] {#intelligent-services}

インテリジェントサービスは、マーケティングアナリストや実践者に対して、顧客体験の使用事例で人工知能と機械学習の機能を活用する機能を提供します。これにより、マーケティングアナリストは、データサイエンスの専門知識を必要とせずに、ビジネスレベルの設定を使用して、会社のニーズに固有の予測を設定できます。

### 顧客 AI

リアルタイム顧客データプラットフォームで使用できる顧客AIは、個々のプロファイルのチャーンやコンバージョンなどのカスタム傾向スコアをスケールで生成するために使用します。 ビジネスニーズから機械学習の問題への変換、アルゴリズムの選択、トレーニング、デプロイは必要ありません。

| 機能 | 説明 |
| ------- | ----------- |
| Adobe Analyticsデータのサポート | Analyticsソースコネクタ経由でAdobe Analyticsデータセットをサポートする機能が更新されました。消費者エクスペリエンスイベント(CEE)スキーマに準拠するためにデータをETLする必要はありません。 |
| Adobe Audience Managerデータのサポート | Audience Managerソースコネクタを介したAdobe Audience Managerデータセットをサポートする機能が更新されました。消費者エクスペリエンスイベント(CEE)スキーマに準拠するためにデータをETLする必要はありません。 |
| モデルのパフォーマンスの概要 | 顧客AIのサービスインスタンスインサイトページに「[モデルパフォーマンスの概要](../../intelligent-services/customer-ai/user-guide/discover-insights.md#performance-metrics)」タブが表示されるようになりました。 「モデルパフォーマンス」タブには、実際のコンバージョン率とチャーン率がすべて表示されます。 これにより、各傾向バケットで何が起きているかを判断し、把握できます。 |

サポートされるデータセットの詳細については、[[!DNL Intelligent Services] データ準備のドキュメント](../../intelligent-services/data-preparation.md)を参照してください。

### Attribution AI

Attribution AI は、コンバージョンイベントにつながるタッチポイントの貢献度を明らかにするために使用します。これは、カスタマージャーニーにわたる個々のマーケティングタッチポイントのマーケティング効果をマーケターが定量化するのに役立ちます。

| 機能 | 説明 |
| ------- | ----------- |
| Adobe Analyticsデータのサポート | Analyticsソースコネクタ経由でAdobe Analyticsデータセットをサポートする機能が更新されました。消費者エクスペリエンスイベント(CEE)スキーマに準拠するためにデータをETLする必要はありません。 |

サポートされるデータセットの詳細については、[[!DNL Intelligent Services] データ準備のドキュメント](../../intelligent-services/data-preparation.md)を参照してください。

## セグメント化サービス {#segmentation}

Adobe Experience Platformセグメント化サービスは、セグメントを作成し、[!DNL Real-time Customer Profile]データからオーディエンスを生成できるユーザーインターフェイスとRESTful APIを提供します。 これらのセグメントは、Platform 上で一元的に設定および管理され、アドビのアプリケーションから簡単にアクセスできます。

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能な人々のグループを区別する条件を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| その他の集計関数 | セグメントビルダーにカウント関数が追加されました。 カウント関数を使用すると、指定したイベントがおこなわれた回数をカウントできます。 カウント関数について詳しくは、『[セグメントビルダーガイド](../../segmentation/ui/segment-builder.md#count-functions)』の「カウント関数」の節を参照してください。 |

[!DNL Segmentation Service]について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## [!DNL Sources] {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform には、様々なデータプロバイダーへのソース接続を簡単に設定できる RESTful API とインタラクティブ UI が用意されています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

| 機能 | 説明 |
| ------- | ----------- |
| [!DNL Marketo Engage] (ベータ) | UIを使用して[!DNL Marketo Engage]ソース接続を作成し、B2BデータをPlatformに取り込み、Platform接続アプリケーションを使用してこのデータを最新の状態に保つことができるようになりました。 詳しくは、[[!DNL Marketo Engage] ソースコネクタのドキュメント](../../sources/connectors/adobe-applications/marketo/marketo.md)を参照してください。 |
| ベータ版ソースの一般公開(GA) | 以下のソースがベータ版からGA版にプロモーションされました。 <ul><li>[[!DNL Amazon Kinesis]](../../sources/connectors/cloud-storage/kinesis.md)</li><li>[[!DNL Azure EventHubs]](../../sources/connectors/cloud-storage/eventhub.md)</li><li>[[!DNL HTTP API]](../../sources/connectors/streaming/http.md)</li><li>[[!DNL MariaDB]](../../sources/connectors/databases/mariadb.md)</li><li>[[!DNL Microsoft SQL Server]](../../sources/connectors/databases/sql-server.md)</li><li>[[!DNL Oracle]](../../sources/connectors/databases/oracle.md)</li></ul> |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
