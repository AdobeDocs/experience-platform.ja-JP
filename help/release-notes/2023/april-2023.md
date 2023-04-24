---
title: Adobe Experience Platform リリースノート 2023年4月
description: Adobe Experience Platform の 2023年4月のリリースノート。
source-git-commit: 9f50ca4b2a4c576af5ce8ee5c085a7603fed2560
workflow-type: tm+mt
source-wordcount: '407'
ht-degree: 48%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年4月26日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [データ準備](#data-prep)
- [ソース](#sources)

## データ準備 {#data-prep}

データ準備を使用すると、データエンジニアはエクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換および検証を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 非実稼働用サンドボックスのAdobe Analyticsのバックフィル期間を更新しました | 非実稼働用サンドボックスのAdobe Analyticsのバックフィル期間が 3 か月に短縮されました。 実稼働用サンドボックスのバックフィルは、13 か月で同じままです。 この変更は新しいフローにのみ適用され、既存のフローには影響しません。 詳しくは、 [Adobe Analyticsの概要](../../sources/connectors/adobe-applications/analytics.md). |
| FPID 文字列を ECID に変換する新しいマッパー関数 | 以下を使用： `fpid_to_ecid` 関数を使用して、FPID 文字列を ECID に変換し、Experience PlatformおよびExperience Cloudアプリケーションで使用できます。 詳しくは、[データ準備関数ガイド](../../data-prep/functions.md)を参照してください。 |

{style="table-layout:auto"}

データ準備について詳しくは、[データ準備の概要](../../data-prep/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むことができ、Platform サービスを使用してそのデータの構造化、ラベル付け、および拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| Microsoft Dynamics、Salesforce CRM、SalesforceMarketing Cloudの行レベルのデータのフィルタリング API のサポート | 論理演算子と比較演算子を使用して、Microsoft Dynamics、Salesforce CRM、SalesforceMarketing Cloudソースの行レベルのデータをフィルタリングします。 [API を使用してソースのデータのフィルタリング](../../sources/tutorials/api/filter.md)に関するガイドを参照してください。 |
| Shopify ストリーミングのベータ可用性 | この [Shopify ストリーミングソース](../../sources/connectors/ecommerce/shopify-streaming.md) は、ベータ版で利用できるようになりました。 Shopify ストリーミングソースを使用して、Shopify パートナーアカウントからExperience Platformにデータをストリーミングします。 |
| OneTrust 統合の一般提供 | この [OneTrust 統合ソース](../../sources/connectors/consent-and-preferences/onetrust.md) は現在 GA です。 OneTrust Integration ソースを使用して、OneTrust Integration アカウントから同意と環境設定のデータをExperience Platformに取り込みます。 |
| oracleサービスクラウドの一般提供 | この [Oracleサービスクラウドソース](../../sources/connectors/customer-success/oracle-service-cloud.md) は現在 GA です。 oracleサービスクラウドソースを使用して、OracleサービスクラウドデータをExperience Platformに取り込みます。 |

{style="table-layout:auto"}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
