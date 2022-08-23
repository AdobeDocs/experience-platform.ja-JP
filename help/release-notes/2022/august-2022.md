---
title: Adobe Experience Platformリリースノート 2022 年 8 月
description: Adobe Experience Platformの 2022 年 8 月リリースノート。
source-git-commit: 925991d58c3cdd84e13b12a095e9681b8f4b254b
workflow-type: tm+mt
source-wordcount: '942'
ht-degree: 35%

---

# Adobe Experience Platform リリースノート

**リリース日：2022年8月24日**

Adobe Experience Platform の既存の機能に対するアップデート：

- [データ準備](#data-prep)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [ソース](#sources)

## [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] を使用すると、データエンジニアは Experience Data Model（XDM）との間でデータをマッピング、変換および検証できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 警告を含むレコードの取り込みのサポート | Data Prep は、警告（重要でないエラー）をフィールドにローカライズし、残りの行を取り込めるようになりました。 すべてのマッパー変換エラーが、警告と共に、部分的に取り込まれた行が成功したと見なされ、警告と共にレポートされるようになりました。  監視は、警告および診断の詳細を含むレコードに対してもサポートされます。 警告を含むレコードの部分取り込みは、現在、データのストリーミングでのみ使用できます。 次のドキュメントを確認します。 [警告を含むレコードの取り込み](../../sources/tutorials/ui/monitor-streaming.md) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

[!DNL Data Prep] について詳しくは、[[!DNL Data Prep] 概要](../../data-prep/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションのために顧客属性を使用したりできます。

**新しい XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| グローバルスキーマ | [[!UICONTROL AJO エンティティスキーマ]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/customerJourneyManagement/ajo-entity.schema.json) | Adobe Journey Optimizerの非正規化エンティティを表します。 |
| クラス | [[!UICONTROL AJO 実行エンティティ]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/customerJourneyManagement/ajo-execution-entity.schema.json) | セグメント化で使用するAdobe Journey Optimizer実行エンティティを表します。 |
| フィールドグループ | [[!UICONTROL Workfront Work Objects]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/workobjects-all.schema.json) | Adobe Workfrontのすべての下位レベルのオブジェクト固有のフィールドグループを参照するラッパーフィールドグループ。 |

{style=&quot;table-layout:auto&quot;}

**更新された XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| フィールドグループ | [[!UICONTROL Journey Orchestrationステップイベントの共通フィールド]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/journeyOrchestration/stepEvents/journeyStepEventCommonFieldsMixin.schema.json) | 2 つの新しいプロパティが追加されました。 `origTimeStamp` および `experienceID`. |
| フィールドグループ | [[!UICONTROL セグメントメンバーシップの詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/segmentation.schema.json) | に加えて [!UICONTROL XDM 個人プロファイル]の場合、このフィールドグループは、XDM ビジネスアカウントクラスに基づくスキーマでも使用できるようになりました。 |
| フィールドグループ | （複数） | Marketo B2B アクティビティに関連するいくつかのフィールドグループが、安定したステータスに更新されました。 次を参照してください。 [プルリクエスト](https://github.com/adobe/xdm/pull/1593/files) 」を参照してください。 |
| フィールドグループ | （複数） | 発生していたエラーを修正するために、複数の天気関連フィールドグループが更新されました。 `uvIndex` および `sunsetTime`. 次を参照してください。 [プルリクエスト](https://github.com/adobe/xdm/pull/1602/files) 」を参照してください。 |
| データタイプ | [[!UICONTROL 製品リスト項目]](https://github.com/adobe/xdm/blob/master/components/datatypes/productlistitem.schema.json) | 新しいプロパティ `productImageUrl` が追加されました。 |
| データタイプ | [[!UICONTROL QoE データの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/qoedatadetails.schema.json) | 新しいプロパティ `framesPerSecond` が追加されました。 |
| データタイプ | [[!UICONTROL セッションの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/sessiondetails.schema.json) | `sdkVersion` の名前は `appVersion` に変更されました。`meta:enum` および `description` フィールドも更新されました。 |
| データタイプとフィールドグループ | （複数） | 複数のメディアデータタイプとフィールドグループに、新しいフィールドと説明が更新されました。 次を参照してください。 [プルリクエスト](https://github.com/adobe/xdm/pull/1582/files) 」を参照してください。 |
| (すべて) | （複数） | を含むすべてのスキーマオブジェクト `enum` フィールドに、対応する `meta:enum` 各制約の表示値を示すフィールド。 次を参照してください。 [プルリクエスト](https://github.com/adobe/xdm/pull/1601/files) 」を参照してください。 |

{style=&quot;table-layout:auto&quot;}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| セルフサービスソース（バッチ SDK）の一般リリース | REST API ベースのデータソースを開発、テスト、統合し、簡単にソース仕様を設定できる方法で、バッチデータをExperience Platformに取り込みます。 ソース SDK を使用すると、次のことができます。 <ul><li>新しいソースをExperience Platformカタログに設定します。</li><li>サポートされる認証タイプ、スケジュール、リソースデータの取得方法など、ソースの仕様を定義します。</li><li>新しいソースに関するユーザー向けドキュメントを作成します。</li></ul> 詳しくは、 [セルフサービスソース（バッチ SDK）](../../sources/sources-sdk/overview.md). |
| [!DNL Google BigQuery] ソースの一般提供 | 以下を使用： [!DNL Google BigQuery] データを取り込むソース [!DNL Google BigQuery] data warehouse からExperience Platformへ。 詳しくは、 [[!DNL Google BigQuery] ソース](../../sources/connectors/databases/bigquery.md). |
| [!DNL Teradata Vantage] ソース（ベータ版） | 以下を使用： [!DNL Teradata Vantage] ハイブリッドマルチクラウド環境からExperience Platformにデータを取り込むソース。 詳しくは、 [[!DNL Teradata Vantage] ソース](../../sources/connectors/databases/teradata-vantage.md). |
| Adobe Analyticsソースのクロスリージョンサポート | 任意の地域（米国、英国、シンガポール）からレポートスイートを取り込めるようになりました。 レポートスイートは、ソース接続が作成されるExperience Platformサンドボックスインスタンスと同じ組織にマッピングする必要があります。 詳しくは、 [UI でのAdobe Analyticsソース接続の作成](../../sources/tutorials/ui/create/adobe-applications/analytics.md). |
| オンデマンド取り込み用の API のサポート | オンデマンド取り込みを使用して、 [!DNL Flow Service] API 作成されたフロー実行は、1 回の取り込みに設定する必要があります。 詳しくは、 [API を使用したオンデマンド取り込み用のフロー実行の作成](../../sources/tutorials/api/on-demand-ingestion.md) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
