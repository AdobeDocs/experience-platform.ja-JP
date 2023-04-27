---
title: Adobe Experience Platform リリースノート 2023年4月
description: Adobe Experience Platform の 2023年4月のリリースノート。
exl-id: 8b8fa810-d301-43c1-98df-10d3903f3147
source-git-commit: ce2e80a7ea7385be98bbcda6a0704cd0814c62b2
workflow-type: tm+mt
source-wordcount: '1285'
ht-degree: 42%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年4月26日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [ダッシュボード](#dashboards)
- [データ準備](#data-prep)
- [データ収集](#data-collection)
- [宛先](#destinations)
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

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| データストリームの IP アドレスの難読化 | 部分的または完全なデータストリームレベルの IP 難読化オプションを [データストリーム設定 UI](../../edge/datastreams/configure.md). <br><br>データストリームレベルの IP 難読化の設定は、Adobe TargetおよびAudience Managerで設定された IP 難読化よりも優先されます。 <br><br>Adobe Analyticsに送信されたデータは、データストリームレベルの影響を受けません [!UICONTROL IP Obfuscation（IP の不明化）] 設定。 Adobe Analyticsは現在、不明化されていない IP アドレスを受け取ります。 不明化された IP アドレスを Analytics が受け取るには、Adobe Analyticsで、個別に IP の不明化を設定する必要があります。 この動作は、今後のリリースで更新される予定です。<br><br> IP の難読化と設定方法の詳細については、 [datastream 設定ドキュメント](../../edge/datastreams/configure.md#advanced-options). |
| [データストリーム設定の上書き](../../edge/datastreams/overrides.md) | データストリームの追加の設定オプションを定義できるようになりました。これを使用して、イベントデータセット、Target プロパティトークン、ID 同期コンテナ、Analytics レポートスイートなど、特定の設定を上書きできます。 <br><br>データストリーム設定の上書きは、次の 2 つの手順で行います。 <ol><li>まず、データストリーム設定の上書きを [datastream 設定ページ](../../edge/datastreams/configure.md).</li><li>次に、Web SDK コマンドを使用するか、Web SDK を使用して、オーバーライドを Edge ネットワークに送信する必要があります [タグ拡張](../../edge/extension/web-sdk-extension-configuration.md).</li></ol> |

{style="table-layout:auto"}

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新しい宛先** {#new-destinations}

| 宛先 | 説明 |
| ----------- | ----------- |
| [[!DNL Salesforce Marketing Cloud Account Engagement] 接続](../../destinations/catalog/email-marketing/salesforce-marketing-cloud-account-engagement.md) | SalesforceMarketing Cloudアカウントエンゲージメント（旧称 Pardot）の宛先を使用して、リードのキャプチャ、追跡、スコアリング、およびグレーディングをおこないます。 この宛先は、より長い販売サイクルと決定サイクルを必要とする複数の部門や意思決定者が関与する B2B の使用例に使用します。 |

{style="table-layout:auto"}

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| のデータフロー監視 [!DNL Custom Personalization] および [!DNL Adobe Commerce] 宛先 | <p> これで、 [Adobe Commerce](/help/destinations/catalog/personalization/adobe-commerce.md), [カスタムパーソナライゼーション](../../destinations/catalog/personalization/custom-personalization.md) そして [属性を含むカスタムパーソナライゼーション](../../destinations/catalog/personalization/custom-personalization.md) 接続。 </p> <p>![Adobe Commerce画像](/help/destinations/assets/common/adobe-commerce-metrics.png "Adobe Commerce指標"){width="100" zoomable="yes"}</p>  詳しくは、 [宛先ワークスペースでのデータフローの監視](../../dataflows/ui/monitor-destinations.md#monitor-dataflows-in-the-destinations-workspace) を参照してください。 |
| 新規 **[!UICONTROL セグメント名にセグメント ID を追加]** フィールド [!DNL Google Ad Manager] および [!DNL Google Ad Manager 360] 宛先 | これで、セグメント名を [[!DNL Google Ad Manager]](/help/destinations/catalog/advertising/google-ad-manager.md#parameters) および [[!DNL Google Ad Manager 360]](/help/destinations/catalog/advertising/google-ad-manager-360-connection.md#destination-details) 次のように、セグメントからExperience PlatformID を含めます。 `Segment Name (Segment ID)`. |

{style="table-layout:auto"}

<!--

| New **[!UICONTROL Append segment ID to segment name]** field for the [!DNL Google Ad Manager] and [!DNL Google Ad Manager 360] destinations | You can now have the segment name in [[!DNL Google Ad Manager]](/help/destinations/catalog/advertising/google-ad-manager.md#parameters) and [[!DNL Google Ad Manager 360]](/help/destinations/catalog/advertising/google-ad-manager-360-connection.md#destination-details) include the segment ID from Experience Platform, like this: `Segment Name (Segment ID)`. |
| Scheduled audience backfills | <p>For the [!DNL Google Display & Video 360] destination, the activation of audience backfills to the destination is scheduled to occur 24-48 hours after a segment is first mapped to a destination connection. This update is in response to Google's policy to wait 24 hours until ingesting data and will improve match rates between Real-time CDP and [!DNL Google Display & Video 360].</p> <p>Note that this is a backend configuration applicable to this destination only and that is unrelated to any customer-configurable scheduling options in the UI.</p> |

-->


**修正および機能強化** {#destinations-fixes-and-enhancements}

- 問題を修正しました： **除外された ID** ファイルベースの宛先エクスポートのレポート指標。 お客様は、アクティブ化されたエクスポートから期待どおりに書き出されたすべての ID を受け取っていました。 ただし、 **除外された ID** UI のレポート指標で、書き出すとは思われない ID が正しくカウントされないため、除外された ID が誤って多数表示されていました。 (PLAT-149774)
- アクティベーションワークフローのスケジュール設定手順の問題を修正しました。 マッピング ID が必要な宛先の場合、既存の宛先接続に追加されたセグメントのマッピング ID を追加できませんでした。 (PLAT-148808)

<!--
- We have fixed an issue with the beta SFTP destination where the port number was previously hardcoded to 22. The port is now configurable for this destination. 

-->

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

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
