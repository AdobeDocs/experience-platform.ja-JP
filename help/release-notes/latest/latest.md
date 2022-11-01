---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の最新のリリースノートです。
source-git-commit: cf8f630360c2cdbba1082913b179e719156183f4
workflow-type: tm+mt
source-wordcount: '1060'
ht-degree: 47%

---

# Adobe Experience Platform リリースノート

**リリース日：2022 年 10 月 26 日**

Adobe Experience Platform の新機能：

- [顧客管理キー](#cmk)

Adobe Experience Platform の既存の機能に対するアップデート：

- [データ収集](#data-collection)
- [宛先](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [クエリサービス](#query-service)
- [ソース](#sources)

## 顧客管理キー {#cmk}

Adobe Experience Platformに保存されるすべてのデータは、システムレベルのキーを使用して、保存時に暗号化されます。 Platform 上に構築されたアプリケーションを使用している場合、代わりに独自の暗号化キーを使用するように選択できるようになり、データのセキュリティをより詳細に制御できます。

概要については、 [顧客管理キー](../../landing/governance-privacy-security/customer-managed-keys.md) 」を参照してください。

## データ収集 {#data-collection}

Adobe Experience Platform は、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信します。そこでデータを強化、変換、アドビまたはアドビ以外の宛先への配信を可能にする一連のテクノロジーを提供します。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| データストリームの機密データ処理 | データストリームは、複数の Platform テクノロジーを活用して、HIPAA(Health Insurance Portability and Accountability Act) などの規制によって適用される機密データを適切に処理するようになりました。 詳しくは、 [データストリーム内の感覚的なデータの処理](../../edge/datastreams/overview.md#sensitive) を参照してください。 |
|  イベント転送用 [!DNL Splunk]拡張機能 | これで、次にデータを送信できます： [!DNL Splunk] の使用 [イベント転送](../../tags/ui/event-forwarding/overview.md) 拡張子。 詳しくは、 [[!DNL Splunk] 拡張機能の概要](../../tags/extensions/web/splunk/overview.md) を参照してください。 |
|  イベント転送用 [!DNL Zendesk]拡張機能 | これで、次にデータを送信できます： [!DNL Zendesk] の使用 [イベント転送](../../tags/ui/event-forwarding/overview.md) 拡張子。 詳しくは、 [[!DNL Zendesk] 拡張機能の概要](../../tags/extensions/web/zendesk/overview.md) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

## [!DNL Destinations] {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先**

| 宛先 | 説明 |
| ----------- | ----------- |
| [[!DNL Line]](../../destinations/catalog/mobile-engagement/line.md) | LINE は、人々、サービス、情報を結び付け、チャットアプリからエンターテインメント、ソーシャル、日々のアクティビティのハブに成長した人気のコミュニケーションプラットフォームです。 |
| [[!DNL Microsoft Dynamics 365]](../../destinations/catalog/crm/microsoft-dynamics-365.md) | Microsoft Dynamics 365 は、企業のリソース計画 (ERP) と顧客関係管理 (CRM) を、生産性の高いアプリケーションと AI ツールと組み合わせ、エンドツーエンドのスムーズで制御可能な運用を実現し、潜在的な成長力を向上させ、コストを削減するクラウドベースのビジネスアプリケーションプラットフォームです。 |
| [[!DNL (Beta) Adobe Commerce]](../../destinations/catalog/personalization/adobe-commerce.md) | この [!DNL (Beta) Adobe Commerce] 宛先コネクタを使用すると、アクティブ化するReal-Time CDPセグメントを 1 つ以上選択できます [!DNL Adobe Commerce] 顧客に動的にパーソナライズされたエクスペリエンスを提供するアカウント。 内 [!DNL Adobe Commerce]その後、これらのReal-Time CDPセグメントを選択して、「buy 2 get 1 free」など、買い物かご内の個別オファーをパーソナライズできます。 また、ヒーローバナーを表示したり、プロモーションオファーを通じて製品の価格を変更したりすることもできます。これらはすべてAdobe Real-Time CDPセグメント用にカスタマイズされています。 |

{style=&quot;table-layout:auto&quot;}

**新規ドキュメントまたは更新されたドキュメント**

| ドキュメント | 説明 |
| ----------- | ----------- |
| [宛先ガードレール](../../destinations/guardrails.md) | このページでは、アクティベーション動作に関するデフォルトの使用方法とレートの制限について説明します。 |

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションのために顧客属性を使用したりできます。

**更新された XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| データタイプ | [[!UICONTROL セッションの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/sessiondetails.schema.json) | 更新された `authorized` フィールドをブール型から文字列に変換します。 `season` および `episode` は、整数から文字列に変更されました。 |
| データタイプ | [[!UICONTROL 広告の詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/advertisingdetails.schema.json) | `name` の名前はに変更されました。 `friendlyName`、および `ID` の名前はに変更されました。 `name`. |
| データタイプ | [[!UICONTROL エラーの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/errordetails.schema.json) | `ID` の名前は `name` に変更されました。 |

{style=&quot;table-layout:auto&quot;}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## クエリサービス {#query-service}

クエリサービスを使用すると、標準 SQL を使用して Adobe Experience Platform [!DNL Data Lake] でデータに対してクエリを実行できます。任意のデータセットを [!DNL Data Lake] から結合し、クエリの結果を新しいデータセットとして取得することで、レポートや Data Science Workspace で使用したり、リアルタイム顧客プロファイルに取り込んだりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| Platform UI でのクエリの監視 | クエリサービス [!UICONTROL 予定クエリ] 「 」タブでは、UI を使用して、すべてのクエリジョブのステータスをより明確に表示できます。 これで、クエリの実行状態に関する重要な情報を、次の場所から検索できるようになりました。失敗した場合のエラーメッセージやコードは含まれます。 [!UICONTROL 予定クエリ] タブをクリックします。 UI を使用して、これらのクエリのステータスに基づいてアラートを購読することもできます。 詳しくは、 [クエリドキュメントの監視](../../query-service/monitor-queries.md) この機能の詳細については、を参照してください。 |
| クエリアクセラレーターレポートインサイトデータモデル | Data Distiller SKU の一部として、クエリ高速化ストアを使用すると、データから重要なインサイトを得るのに必要な時間と処理能力を削減できます。 Query Accelerated Store を使用すると、カスタムデータモデルを作成したり、既存のAdobe Real-time Customer Data Platformデータモデルを拡張したりして、レポートのインサイトとそのビジュアライゼーションを改善できます。 詳しくは、 [query accelerated store reporting insights ドキュメント](../../query-service/query-accelerated-store/reporting-insights-data-model.md) この機能の詳細については、を参照してください。 |

{style=&quot;table-layout:auto&quot;}

クエリサービスについて詳しくは、[クエリサービスの概要](../../query-service/home.md)を参照してください。Adobe Experience Platform の新機能：

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- | 
| Adobe Workfrontソースのベータ版の可用性 | 以下を使用： [Adobe Workfrontソース](../../sources/connectors/adobe-applications/workfront.md) WorkfrontデータをExperience Platformに取り込み、作業用レコードとサードパーティデータの組み合わせ、作業用レコードに対する履歴と時系列の分析の適用、標準 SQL を使用した作業用データのクエリなどの使用例を実行します。 詳しくは、 [UI でのWorkfrontソース接続の作成](../../sources/tutorials/ui/create/adobe-applications/workfront.md). |
| ベータ版のOracleサービスクラウドソースの可用性 | oracleサービスクラウドソースを使用して、OracleサービスクラウドアカウントからExperience Platformにデータを取り込みます。 詳しくは、 [Oracleサービスクラウドソース](../../sources/connectors/customer-success/oracle-service-cloud.md). |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
