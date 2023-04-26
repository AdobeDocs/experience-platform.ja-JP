---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の 2023年4月のリリースノート。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: e29bff2b8c576f92d239bb6c855710142df8db57
workflow-type: tm+mt
source-wordcount: '779'
ht-degree: 54%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年4月26日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [ダッシュボード](#dashboards)
- [データ準備](#data-prep)
- [エクスペリエンスデータモデル](#xdm)
- [リアルタイム顧客プロファイル](#profile)
- [ソース](#sources)

## ダッシュボード {#dashboards}

Adobe Experience Platform では、毎日のスナップショットで得られた、組織のデータに関する重要なインサイトを確認できる複数のダッシュボードを提供しています。

**新機能または更新された機能** {#dashboards-new-updated-features}

| 機能 | 説明 |
| --- | --- |
| ユーザー定義ダッシュボード | 次の操作を実行できます。 **履歴データをフィルター** ウィジェットのインサイトから、最近のデータまたはカスタムの分析期間を使用します。<br>また、 **既存のウィジェットを複製**. 複製をカスタマイズし、その属性を編集することで、新しい一意のウィジェットを作成する際に、最初から再起動するのを防ぐことができます。 |

{style="table-layout:auto"}

アクセス権限の付与方法やカスタムウィジェットの作成方法など、ダッシュボードの詳細については、まず[ダッシュボードの概要](../../dashboards/home.md)を参照してください。

## データ準備 {#data-prep}

データ準備を使用すると、データエンジニアはエクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換および検証を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 非実稼働用サンドボックスのAdobe Analyticsのバックフィル期間を更新しました | 非実稼働用サンドボックスのAdobe Analyticsのバックフィル期間が 3 か月に短縮されました。 実稼働用サンドボックスのバックフィルは、13 か月で同じままです。 この変更は新しいフローにのみ適用され、既存のフローには影響しません。 詳しくは、 [Adobe Analyticsの概要](../../sources/connectors/adobe-applications/analytics.md). |
| FPID 文字列を ECID に変換する新しいマッパー関数 | 以下を使用： `fpid_to_ecid` 関数を使用して、FPID 文字列を ECID に変換し、Experience PlatformおよびExperience Cloudアプリケーションで使用できます。 詳しくは、[データ準備関数ガイド](../../data-prep/functions.md)を参照してください。 |

{style="table-layout:auto"}

データ準備について詳しくは、[データ準備の概要](../../data-prep/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションのために顧客属性を使用したりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 表示名の切り替え | スキーマエディターで、元のフィールド名と、人間が読み取り可能な表示名を切り替えることができるようになりました。 この柔軟性により、フィールドの検出性とスキーマの編集性を向上できます。 標準フィールドグループの表示名はシステムで生成されますが、必要に応じて UI を使用してカスタマイズすることもできます。 |

{style="table-layout:auto"}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、顧客がいつどこからブランドとやり取りしても、顧客に合わせて調整された、一貫性と関連性のある体験を提供できます。リアルタイム顧客プロファイルでは、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせて、各顧客の全体像を確認できます。プロファイルを使用すると、顧客データを統合ビューに統合して、すべての顧客インタラクションの実用的なタイムスタンプ付きのアカウントを提供できます。

**更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| 偽名プロファイルデータの有効期限 | 偽名プロファイルデータの有効期限が一般に利用できるようになりました。 このリリースでは、有効にした後も、古い偽名プロファイルをExperience Platformインスタンスから継続的に削除します。 この機能と偽名プロファイルの詳細については、 [偽名プロファイルデータ有効期限ガイド](../../profile/pseudonymous-profiles.md). |

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