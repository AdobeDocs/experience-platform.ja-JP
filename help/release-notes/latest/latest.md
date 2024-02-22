---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の 2024年1月のリリースノートです。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: b41a69244c7eb1111759b2af5c1ae6a0fb90be32
workflow-type: tm+mt
source-wordcount: '1242'
ht-degree: 26%

---

# Adobe Experience Platform リリースノート

**リリース日：2024年2月21日（PT）**

Experience Platformの既存の機能の更新：

- [アラート](#alerts)
- [データ収集](#data-collection)
<!-- - [Data Prep](#data-prep) -->
- [宛先](#destinations)
- [サンドボックス](#sandboxes)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## アラート {#alerts}

Experience Platform では、様々な Platform アクティビティに関するイベントベースのアラートを登録できます。を使用して、様々なアラートルールを購読できます。 [!UICONTROL アラート] 」タブをクリックし、UI 内または電子メール通知を通じてアラートメッセージを受け取るように選択できます。
**新機能または更新された機能**
| 機能 | 説明 | | — | — | | 「アラート履歴」タブ | Experience Platform管理者は、アラート購読者の管理機能を使用して、Adobeユーザー ID、外部電子メールアドレス、または電子メールグループリストにアラートを割り当てることができます。 詳しくは、 [アラート UI ドキュメント](../../observability/alerts/ui.md) 「履歴」タブの詳細を参照してください。 |

{style="table-layout:auto"}

アラートの詳細については、 [[!DNL Observability Insights] 概要](../../observability/home.md).

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [Web SDK での Web アプリ内メッセージのサポート](../../edge/personalization/web-in-app-messaging.md) | Adobe Experience Platform Web SDK で、Adobe Journey Optimizerキャンペーン用の Web アプリ内メッセージ設定がサポートされるようになりました。 |

{style="table-layout:auto"}

データ収集について詳しくは、[データ収集の概要](../../tags/home.md)を参照してください。

<!-- ## Data Prep {#data-prep}

Data Prep allows data engineers to map, transform, and validate data to and from Experience Data Model (XDM).

**New or updated features**

| Feature | Description |
| --- | --- |
| New mapper functions for Adobe Analytics | You can now use the following functions to extract event data from Adobe Analytics: <ul><li>`aa_get_event_id`</li><li>`aa_get_event_value`</li><li>`aa_get_product_categories`</li><li>`aa_get_product_names`</li><li>`aa_get_product_quantities`</li><li>`aa_get_product_prices`</li><li>`aa_get_product_event_values`</li><li>`aa_get_product_evars`</li></ul> For more information on these functions, read the [Data Prep functions guide](../../data-prep/functions.md) |

{style="table-layout:auto"}

For more information on Data Prep, read the [Data Prep overview](../../data-prep/home.md). -->

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新しい宛先** {#new-destinations}

| 宛先 | 説明 |
| ----------- | ----------- |
| [Gainsight PX 接続](../../destinations/catalog/analytics/gainsight-px.md) | Gainsight PX は、製品チームが製品の使用方法を理解し、フィードバックを収集し、製品のチュートリアルなどのアプリ内エンゲージメントを作成して、ユーザーのオンボーディングと製品の採用を促進できる製品エクスペリエンスプラットフォームです。 |
| [Mailchimp タグ接続](../../destinations/catalog/email-marketing/mailchimp-tags.md) | Mailchimp は、人気のあるマーケティングオートメーションプラットフォームおよび電子メールマーケティングサービスです。 Mailchimp タグコネクタを使用して、連絡先の構造化、ラベル付け、分類を行うことができます。 |
| [SAP Commerce 接続](../../destinations/catalog/ecommerce/sap-commerce.md) | SAP Commerce は、B2B および B2C 企業向けのクラウドベースの e コマースプラットフォームソリューションで、SAP Customer Experience ポートフォリオの一部として利用できます。 この宛先を使用して、既存の顧客オーディエンスから SAP Commerce 内の顧客の詳細を更新することができます。 |

{style="table-layout:auto"}

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| 一般に使用可能なアカウントオーディエンスを有効化 | 特定の宛先に対してアカウントオーディエンスをアクティブ化する機能は、 [B2B](/help/rtcdp/overview.md#rtcdp-b2b) および [B2P](/help/rtcdp/overview.md#rtcdp-b2b) Real-time Customer Data Platformの各エディション。 に関するチュートリアルをお読みください。 [アカウントオーディエンスの有効化](/help/destinations/ui/activate-account-audiences.md) ：サポートされる宛先を含む完全な情報を取得します。 |
| Googleの宛先のデジタル市場法同意実施ツール | Googleは [Google Ads API](https://developers.google.com/google-ads/api/docs/start), [Customer Match](https://ads-developers.googleblog.com/2023/10/updates-to-customer-match-conversion.html)、および [ディスプレイおよびビデオ 360 API](https://developers.google.com/display-video/api/guides/getting-started/overview) コンプライアンスおよび同意関連の要件をサポートするために、 [デジタル市場法](https://digital-markets-act.ec.europa.eu/index_en) 欧州連合 (EU) の (DMA)[EU ユーザー同意ポリシー](https://www.google.com/about/company/user-consent-policy/)) をクリックします。 同意要件に対するこれらの変更の適用は、2024 年 3 月 6 日から施行される予定です。 <br/><br/> EU ユーザーの同意ポリシーに従い、欧州経済圏 (EEA) のユーザーに対してオーディエンスリストを作成し続けるには、広告主やパートナーは、オーディエンスデータをアップロードする際にエンドユーザーの同意を渡す必要があります。 GoogleパートナーのAdobeは、EU の DMA に基づくこれらの同意要件を満たすために必要なツールを提供します。<br/><br/>Adobeのプライバシーとセキュリティシールドを購入し、 [同意ポリシー](../../data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) 同意しないプロファイルを除外するには、何のアクションも実行する必要はありません。<br/><br/>Adobeプライバシーとセキュリティシールドを購入していないお客様は、 [セグメント定義](../../segmentation/home.md#segment-definitions) 内の機能 [セグメントビルダー](../../segmentation/ui/segment-builder.md) を使用して、同意されていないプロファイルを除外し、既存のReal-Time CDP Googleの宛先を中断することなく使用し続けます。 |
| [!BADGE ベータ版]{type=Informative} バッチ保存先のマッピングフィールドを並べ替える | CSV エクスポートの列の順序を変更するには、 [マッピング](../../destinations/ui/activate-batch-profile-destinations.md#mapping) 手順 UI でマッピングされたフィールドの順序は、書き出された CSV ファイル内の列の順序に上から下へと反映されます。上の行は CSV ファイル内の一番左の列になります。 <br/><br/> この機能はベータ版で、一部のお客様のみが利用できます。 この機能へのアクセスをリクエストするには、Adobe担当者にお問い合わせください。 |
| [!BADGE ベータ版]{type=Informative} バッチ保存先用に事前に選択されたデフォルトの書き出しスケジュール | Experience Platformは、各ファイル書き出しのデフォルトスケジュールを自動的に設定するようになりました。 次のドキュメントを参照してください： [オーディエンスの書き出しをスケジュール](../../destinations/ui/activate-batch-profile-destinations.md#scheduling) を参照して、デフォルトのスケジュールを変更する方法を確認してください。 <br/><br/> この機能はベータ版で、一部のお客様のみが利用できます。 この機能へのアクセスをリクエストするには、Adobe担当者にお問い合わせください。 |
| [!BADGE ベータ版]{type=Informative} バッチ保存先のオーディエンスアクティベーションスケジュールを一括編集します | 複数のオーディエンスのアクティベーションスケジュールを一括で編集できるようになりました。 [アクティベーションデータ](../../destinations/ui/destination-details-page.md#bulk-edit-schedule) ページに貼り付けます。 <br/><br/> この機能はベータ版で、一部のお客様のみが利用できます。 この機能へのアクセスをリクエストするには、Adobe担当者にお問い合わせください。 |
| [!BADGE ベータ版]{type=Informative} 一括書き出しファイルをオンデマンドでバッチ保存先に | を使用して、オーディエンスを一括でバッチ保存先に書き出せるようになりました。 [オンデマンドでのファイルの書き出し](../../destinations/ui/export-file-now.md) 機能。 <br/><br/> この機能はベータ版で、一部のお客様のみが利用できます。 この機能へのアクセスをリクエストするには、Adobe担当者にお問い合わせください。 |

{style="table-layout:auto"}

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## サンドボックス {#sandboxes}

Adobe Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。このニーズに対処するため、Experience Platformは、1 つの Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展を支援するサンドボックスを提供します。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| サンドボックスツール | 同意ルールとガバナンスルールのオブジェクトタイプに加えて、サンドボックスツールを使用して、統合プロファイルを有効にせずにスキーマをインポートし、セグメントのインポート時にターゲットサンドボックスに見つからない属性を確認し、デフォルトで既存の結合ポリシーを使用します。 これらの機能について詳しくは、 [サンドボックスツール UI ガイド](../../sandboxes/ui/sandbox-tooling.md). |

{style="table-layout:auto"}

サンドボックスについて詳しくは、 [サンドボックスの概要](../../sandboxes/home.md).

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] を使用すると、[!DNL Experience Platform] に保存されている、個人（顧客、見込み客、ユーザー、組織など）に関連するデータをオーディエンスにセグメント化できます。オーディエンスは、セグメント定義または [!DNL Real-Time Customer Profile] データの他のソースを通じて作成できます。これらのオーディエンスは [!DNL Platform] で一元的に設定および管理されており、Adobe ソリューションから簡単にアクセスできます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| アカウントオーディエンス | アカウントオーディエンスが一般公開されました。 アカウントのセグメント化を使用して、リアルタイム顧客プラットフォームの B2B エディションと B2P エディションの両方で、ユーザーベースのオーディエンスからアカウントベースのオーディエンスに、マーケティングセグメント化の完全な簡単性と高度化を提供できます。 このリリースでは、ユーザーベースのオーディエンスをアカウントベースのオーディエンスの述語として使用し、検索機能を追加し、カスタムエンティティの使用をサポートし、データガバナンスに準拠できます。 この機能の詳細については、 [アカウントオーディエンスの概要](../../segmentation/ui/account-audiences.md). |

{style="table-layout:auto"}

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!BADGE ベータ版]{type=Informative} [!DNL Acxiom] ソース | 以下を使用します。 [[!DNL Acxiom Prospecting Data Import] ソース](../../sources/tutorials/ui/create/data-partners/acxiom-prospecting-data-import.md) データの取得とマッピング [!DNL Acxiom] 見込み客のサービスからExperience Platformへ。 |

{style="table-layout:auto"}

ソースの詳細については、 [ソースの概要](../../sources/home.md).
