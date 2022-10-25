---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の最新のリリースノートです。
source-git-commit: 0ea2718247792e997b7a90ab9027946e800c8157
workflow-type: tm+mt
source-wordcount: '769'
ht-degree: 54%

---

# Adobe Experience Platform リリースノート

**リリース日：2022 年 10 月 26 日**

Adobe Experience Platform の新機能：

- [顧客管理キー](#cmk)

Adobe Experience Platform の既存の機能に対するアップデート：

- [データ収集](#data-collection)
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
| クエリアクセラレーターレポートインサイトデータモデル | Data Distiller SKU の一部として、クエリ高速化ストアを使用すると、データから重要なインサイトを得るのに必要な時間と処理能力を削減できます。 Query Accelerated Store を使用すると、カスタムデータモデルを作成したり、既存のAdobe Real-time Customer Data Platformデータモデルを拡張したりして、レポートのインサイトとそのビジュアライゼーションを改善できます。 詳しくは、 [query accelerated store reporting insights ドキュメント](https://experienceleague.adobe.com/docs/experience-platform/query/query-accelerated-store/reporting-insights-data-model.html) この機能の詳細については、を参照してください。 |

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
