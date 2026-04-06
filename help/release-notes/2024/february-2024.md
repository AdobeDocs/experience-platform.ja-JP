---
title: Adobe Experience Platform リリースノート（2024年2月）
description: Adobe Experience Platform の 2024年2月のリリースノートです。
exl-id: 7e4b76b7-4027-4890-b869-1dbb79670c3e
source-git-commit: 82e41af32468febeda2dce6b471d72ef74359ea9
workflow-type: tm+mt
source-wordcount: '1237'
ht-degree: 25%

---

# Adobe Experience Platform リリースノート

**リリース日：2024年2月21日（PT）**

Experience Platformの既存の機能に対するアップデート：

- [アラート](#alerts)
- [データ収集](#data-collection)
- [宛先](#destinations)
- [サンドボックス](#sandboxes)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## アラート {#alerts}

Experience Platformでは、様々なExperience Platform アクティビティのイベントベースのアラートを購読できます。 Experience Platform ユーザーインターフェイスの「[!UICONTROL Alerts]」タブを使用して、様々なアラートルールを購読できます。また、UI自体またはメール通知を使用してアラートメッセージを受信することもできます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 「アラート履歴」タブ | Experience Platform管理者は、アラート購読者の管理機能を使用して、Adobe ユーザーID、外部メールアドレス、またはメールグループリストにアラートを割り当てることができます。 詳しくは、「履歴」タブについて詳しくは、[&#x200B; アラート UI ドキュメント &#x200B;](/help/observability/alerts/ui.md)を参照してください。 |

{style="table-layout:auto"}

アラートについて詳しくは、[[!DNL Observability Insights] 概要](/help/observability/home.md)を参照してください。

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Web SDKでのWeb アプリ内メッセージングのサポート | Adobe Experience Platform Web SDKで、Adobe Journey Optimizer キャンペーンのWeb アプリ内メッセージ設定がサポートされるようになりました。 |

{style="table-layout:auto"}

データ収集について詳しくは、[データ収集の概要](/help/tags/home.md)を参照してください。

<!-- 
## Data Prep {#data-prep}

Data Prep allows data engineers to map, transform, and validate data to and from Experience Data Model (XDM).

**New or updated features**

| Feature | Description |
| --- | --- |
| New mapper functions for Adobe Analytics | You can now use the following functions to extract event data from Adobe Analytics: <ul><li>`aa_get_event_id`</li><li>`aa_get_event_value`</li><li>`aa_get_product_categories`</li><li>`aa_get_product_names`</li><li>`aa_get_product_quantities`</li><li>`aa_get_product_prices`</li><li>`aa_get_product_event_values`</li><li>`aa_get_product_evars`</li></ul> For more information on these functions, read the [Data Prep functions guide](/help/data-prep/functions.md) |

{style="table-layout:auto"}

For more information on Data Prep, read the [Data Prep overview](/help/data-prep/home.md). 
-->

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新しい宛先** {#new-destinations}

| 宛先 | 説明 |
| ----------- | ----------- |
| [Gainsight PX接続](/help/destinations/catalog/analytics/gainsight-px.md) | Gainsight PXは、製品チームが、ユーザーによる製品の使用方法を把握し、フィードバックを収集し、製品ウォークスルーなどのアプリ内エンゲージメントを構築して、ユーザーのオンボーディングと製品の採用率を向上させることを可能にする製品体験プラットフォームです。 |
| [Mailchimp タグ接続](/help/destinations/catalog/email-marketing/mailchimp-tags.md) | Mailchimpは、人気のあるマーケティングオートメーションプラットフォームとメールマーケティングサービスです。 Mailchimp タグコネクタを使用して、連絡先の構成、ラベル付け、分類を行うことができます。 |
| [SAP Commerce接続](/help/destinations/catalog/ecommerce/sap-commerce.md) | SAP Commerceは、B2BおよびB2C企業向けのクラウドベースのe コマースプラットフォームソリューションで、SAP Customer Experience ポートフォリオの一部として利用できます。 この宛先を使用して、既存のExperience Platform オーディエンスからSAP Commerce内のお客様の詳細を更新できます。 |

{style="table-layout:auto"}

**新しい機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| 一般公開のアカウントオーディエンスを活用 | 特定の宛先に対してアカウントオーディエンスをアクティブ化する機能は、Real-Time Customer Data Platformの[Business-to-Business](/help/rtcdp/overview.md#rtcdp-b2b)および[Business-to-Person](/help/rtcdp/overview.md#rtcdp-b2p)版を購入する企業で一般に利用できるようになりました。 [&#x200B; アカウントオーディエンスのアクティブ化](/help/destinations/ui/activate-account-audiences.md)に関するチュートリアルを参照して、サポートされている宛先を含む完全な情報を入手してください。 |
| デジタル市場法Google宛先向けの同意履行ツール | Googleは、欧州連合（[EU ユーザーの同意ポリシー](https://developers.google.com/google-ads/api/docs/start)）の[&#x200B; デジタル市場法](https://ads-developers.googleblog.com/2023/10/updates-to-customer-match-conversion.html) （DMA）に基づいて定義されたコンプライアンスと同意に関する要件をサポートするために、[Google Ads API](https://developers.google.com/display-video/api/guides/getting-started/overview)、[Customer Match](https://digital-markets-act.ec.europa.eu/index_en)、および[Display &amp; Video 360 API](https://www.google.com/about/company/user-consent-policy/)に対する変更をリリースしています。 これらの同意要件の変更は、2024年3月6日から施行される予定です。 <br/><br/> EU ユーザーの同意ポリシーに準拠し、欧州経済地域（EEA）のユーザーに対して引き続きオーディエンスリストを作成するには、広告主とパートナーがオーディエンスデータをアップロードする際に、エンドユーザーの同意を確実に渡す必要があります。 Adobeは、Googleパートナーとして、欧州連合のDMAに基づく同意要件に準拠するために必要なツールを提供します。<br/><br/>Adobe Privacy &amp; Security Shieldを購入し、同意のないプロファイルを除外するように[同意ポリシー](/help/data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation)を設定したお客様は、何らかの操作を行う必要はありません。<br/><br/>Adobe Privacy &amp; Security Shieldを購入していないお客様が、既存のReal-Time CDP Googleの宛先を中断なく引き続き使用するには、[&#x200B; セグメントビルダー](/help/segmentation/home.md#segment-definitions)内の[&#x200B; セグメント定義](/help/segmentation/ui/segment-builder.md)機能を使用して、同意のないプロファイルを除外する必要があります。 |
| [!BADGE Beta]{type=Informative} バッチ宛先のマッピングフィールドの並べ替え | [&#x200B; マッピング &#x200B;](/help/destinations/ui/activate-batch-profile-destinations.md#mapping)手順でマッピングフィールドをドラッグ&amp;ドロップすることで、CSV エクスポートの列の順序を変更できるようになりました。 UIのマッピングされたフィールドの順序は、書き出されたCSV ファイルの列の順序で上から下に反映され、一番上の行がCSV ファイルの一番左の列になります。 <br/><br/>この機能はベータ版で、一部のお客様のみが利用できます。 この機能へのアクセスをリクエストするには、Adobe担当者にお問い合わせください。 |
| [!BADGE Beta]{type=Informative} バッチ宛先の既定の書き出しスケジュールが事前に選択されています | Experience Platformでは、ファイルの書き出しごとにデフォルトスケジュールが自動的に設定されるようになりました。 デフォルトのスケジュールを変更する方法については、[&#x200B; オーディエンスの書き出しのスケジュール設定](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling)に関するドキュメントを参照してください。 <br/><br/>この機能はベータ版で、一部のお客様のみが利用できます。 この機能へのアクセスをリクエストするには、Adobe担当者にお問い合わせください。 |
| [!BADGE Beta]{type=Informative} バッチ宛先のオーディエンス アクティベーション スケジュールの一括編集 | [&#x200B; アクティベーションデータ &#x200B;](/help/destinations/ui/destination-details-page.md#bulk-edit-schedule) ページから、複数のオーディエンスのアクティベーションスケジュールを一括で編集できるようになりました。 <br/><br/>この機能はベータ版で、一部のお客様のみが利用できます。 この機能へのアクセスをリクエストするには、Adobe担当者にお問い合わせください。 |
| [!BADGE Beta]{type=Informative} バッチ宛先へのファイルの一括書き出し | [&#x200B; オンデマンドでファイルを書き出す](/help/destinations/ui/export-file-now.md)機能を使用して、オーディエンスをバッチ宛先に一括で書き出せるようになりました。 <br/><br/>この機能はベータ版で、一部のお客様のみが利用できます。 この機能へのアクセスをリクエストするには、Adobe担当者にお問い合わせください。 |

{style="table-layout:auto"}

宛先の一般的な情報については、[宛先の概要](/help/destinations/home.md)を参照してください。

## サンドボックス {#sandboxes}

Adobe Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。このニーズに対応するために、Experience Platformでは、単一のExperience Platformインスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と開発に役立つサンドボックスを提供しています。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| サンドボックスツール | 同意ルールとガバナンスルールのオブジェクトタイプをサポートするようになっただけでなく、サンドボックスツールを使用して、統合プロファイルを有効にせずにスキーマを読み込み、セグメントを読み込む際にターゲットサンドボックスに欠けている属性を確認し、既存の結合ポリシーを使用するようにデフォルトで設定します。 これらの機能について詳しくは、[&#x200B; サンドボックスツール UI ガイド &#x200B;](/help/sandboxes/ui/sandbox-tooling.md)を参照してください。 |

{style="table-layout:auto"}

サンドボックスについて詳しくは、[&#x200B; サンドボックスの概要](/help/sandboxes/home.md)を参照してください。

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] を使用すると、[!DNL Experience Platform] に保存されている、個人（顧客、見込み客、ユーザー、組織など）に関連するデータをオーディエンスにセグメント化できます。オーディエンスは、セグメント定義または [!DNL Real-Time Customer Profile] データの他のソースを通じて作成できます。これらのオーディエンスは [!DNL Experience Platform] で一元的に設定および管理されており、Adobe ソリューションから簡単にアクセスできます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| アカウントオーディエンス | アカウントオーディエンスが一般公開されました。 Adobe Real-Time Customer Experience PlatformのB2BおよびB2P エディションにおいて、アカウントセグメンテーションを活用することで、ピープルベースのオーディエンスから、アカウントベースのオーディエンスに至るまで、マーケティングセグメンテーション体験を容易かつ洗練されたものにすることができます。 このリリースでは、個人ベースのオーディエンスをアカウントベースのオーディエンスの述語として使用し、検索機能を追加し、カスタムエンティティの使用をサポートし、データガバナンスに準拠しています。 この機能について詳しくは、[&#x200B; アカウントオーディエンスの概要](/help/segmentation/types/account-audiences.md)を参照してください。 |

{style="table-layout:auto"}

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!BADGE Beta]{type=Informative} [!DNL Acxiom] ソース | [[!DNL Acxiom Prospecting Data Import] source](/help/sources/tutorials/ui/create/data-partners/acxiom-prospecting-data-import.md)を使用して、[!DNL Acxiom]見込み客サービスからデータを取得し、Experience Platformにマッピングします。 |

{style="table-layout:auto"}

ソースについて詳しくは、[&#x200B; ソースの概要](/help/sources/home.md)を参照してください。
