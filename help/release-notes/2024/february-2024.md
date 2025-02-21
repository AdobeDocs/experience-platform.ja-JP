---
title: Adobe Experience Platform リリースノート（2024年2月）
description: Adobe Experience Platform の 2024年2月のリリースノートです。
exl-id: 7e4b76b7-4027-4890-b869-1dbb79670c3e
source-git-commit: 02f2082e695d157415c9e0c59ca5d371c94bb991
workflow-type: tm+mt
source-wordcount: '1244'
ht-degree: 29%

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

Experience Platform では、様々な Platform アクティビティに関するイベントベースのアラートを登録できます。Platform ユーザーインターフェイスの「[!UICONTROL アラート]」タブを使用して、様々なアラートルールを購読し、UI 内または電子メール通知を通じてアラートメッセージを受け取るように選択できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 「アラートの履歴」タブ | Experience Platform管理者は、アラート購読者の管理機能を使用してAdobeのユーザー ID、外部のメールアドレス、メールグループリストのいずれかにアラートを割り当てることができます。 「履歴」タブについて詳しくは、[ アラート UI ドキュメント ](../../observability/alerts/ui.md) を参照してください。 |

{style="table-layout:auto"}

アラートについて詳しくは、[[!DNL Observability Insights]  概要 ](../../observability/home.md) を参照してください。

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [Web SDKでの Web アプリ内メッセージのサポート ](../../web-sdk/personalization/web-in-app-messaging.md) | Adobe Experience Platform Web SDKで、Adobe Journey Optimizer キャンペーン用の Web アプリ内メッセージ設定がサポートされるようになりました。 |

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
| [Gainsight PX 接続 ](../../destinations/catalog/analytics/gainsight-px.md) | Gainsight PX は、製品チームがユーザーがどのように製品を使用しているかを理解し、フィードバックを収集し、製品ウォークスルーなどのアプリ内エンゲージメントを作成して、ユーザーのオンボーディングと製品採用を促進できるようにする製品エクスペリエンスプラットフォームです。 |
| [Mailchimp タグ接続 ](../../destinations/catalog/email-marketing/mailchimp-tags.md) | Mailchimp は、人気のあるマーケティング自動化プラットフォームおよびメールマーケティングサービスです。 Mailchimp タグコネクタを使用して、連絡先の構造化、ラベル付け、分類を行うことができます。 |
| [SAP Commerce接続 ](../../destinations/catalog/ecommerce/sap-commerce.md) | SAP Commerceは、B2B および B2C 企業向けのクラウドベースの e コマースプラットフォームソリューションで、SAP カスタマーエクスペリエンス（顧客体験）ポートフォリオの一部として利用できます。 この宛先を使用すると、SAP Commerce内のお客様の詳細を既存のExperience Platform オーディエンスから更新できます。 |

{style="table-layout:auto"}

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| 一般入手可能なアカウントオーディエンスの有効化 | Real-Time Customer Data Platformの [Business-to-Business](/help/rtcdp/overview.md#rtcdp-b2b) エディションと [Business-to-Person](/help/rtcdp/overview.md#rtcdp-b2p) エディションを購入する企業は、特定の宛先に対してアカウントオーディエンスをアクティブ化する機能を使用できるようになりました。 サポートされる宛先を含む完全な情報については、[ アカウントオーディエンスのアクティブ化 ](/help/destinations/ui/activate-account-audiences.md) に関するチュートリアルをお読みください。 |
| Google宛先のデジタル市場法に基づく同意実施ツール | Googleは、欧州連合（EU）の [ デジタル市場法 ](https://developers.google.com/google-ads/api/docs/start) （DMA](https://digital-markets-act.ec.europa.eu/index_en)）（[EU ユーザー同意ポリシー ](https://www.google.com/about/company/user-consent-policy/)）で定義されているコンプライアンスおよび同意関連の要件をサポートするために、[Google Ads API](https://ads-developers.googleblog.com/2023/10/updates-to-customer-match-conversion.html)、[Customer Match および [Display &amp; Video 360 API](https://developers.google.com/display-video/api/guides/getting-started/overview) に対する変更内容をリリースしています。 同意要件に対するこれらの変更の適用は、2024 年 3 月 6 日（PT）から有効になる予定です。 <br/><br/> EU のユーザー同意ポリシーに準拠し、欧州経済領域（EEA）のユーザーに対するオーディエンスリストの作成を続行するには、広告主およびパートナーは、オーディエンスデータをアップロードする際にエンドユーザーの同意を渡していることを確認する必要があります。 Google パートナーであるAdobeは、欧州連合の DMA に基づく同意要件に準拠するために必要なツールを提供します。<br/><br/>Adobe Privacy &amp; Security Shield を購入し、同意のないプロファイルを除外する [ 同意ポリシー ](../../data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) を設定しているお客様は、何もする必要はありません。<br/><br/>Adobe Privacy &amp; Security Shield を購入していないお客様が既存のReal-Time CDP Googleの宛先を引き続き中断することなく使用するには、[ セグメントビルダー ](../../segmentation/ui/segment-builder.md) 内の [ セグメント定義 ](../../segmentation/home.md#segment-definitions) 機能を使用して、同意のないプロファイルを除外する必要があります。 |
| [!BADGE Beta]{type=Informative} バッチ宛先のマッピングフィールドの並べ替え | [ マッピング ](../../destinations/ui/activate-batch-profile-destinations.md#mapping) ステップのマッピングフィールドをドラッグ&amp;ドロップして、CSV 書き出しの列の順序を変更できるようになりました。 UI でマッピングされたフィールドの順序は、書き出された CSV ファイルの列の順序で上から下に反映されます。一番上の行は CSV ファイルの左端の列です。 <br/><br/> この機能はベータ版で、一部のお客様のみご利用いただけます。 この機能へのアクセスをリクエストするには、Adobe担当者にお問い合わせください。 |
| [!BADGE Beta]{type=Informative} バッチ宛先のデフォルトの書き出しスケジュールがあらかじめ選択されています | Experience Platformでは、各ファイル書き出しのデフォルトのスケジュールを自動的に設定するようになりました。 デフォルトのスケジュールを変更する方法については、[ オーディエンスの書き出しのスケジュール設定 ](../../destinations/ui/activate-batch-profile-destinations.md#scheduling) に関するドキュメントを参照してください。 <br/><br/> この機能はベータ版で、一部のお客様のみご利用いただけます。 この機能へのアクセスをリクエストするには、Adobe担当者にお問い合わせください。 |
| [!BADGE Beta]{type=Informative} バッチ宛先のオーディエンス有効化スケジュールを一括編集 | [ アクティベーションデータ ](../../destinations/ui/destination-details-page.md#bulk-edit-schedule) ページから、複数のオーディエンスのアクティベーションスケジュールを一括で編集できるようになりました。 <br/><br/> この機能はベータ版で、一部のお客様のみご利用いただけます。 この機能へのアクセスをリクエストするには、Adobe担当者にお問い合わせください。 |
| [!BADGE Beta]{type=Informative} オンデマンドでファイルをバッチ宛先に一括書き出し | [ オンデマンドでのファイルの書き出し ](../../destinations/ui/export-file-now.md) 機能を使用して、オーディエンスを一括でバッチ宛先に書き出すことができるようになりました。 <br/><br/> この機能はベータ版で、一部のお客様のみご利用いただけます。 この機能へのアクセスをリクエストするには、Adobe担当者にお問い合わせください。 |

{style="table-layout:auto"}

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## サンドボックス {#sandboxes}

Adobe Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。このニーズに対応するために、Experience Platformには、1 つの Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つサンドボックスが用意されています。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| サンドボックスツール | 同意ルールとガバナンスルールのオブジェクトタイプをサポートする以外に、サンドボックスツールを使用して、統合プロファイルを有効にせずにスキーマをインポートしたり、セグメントをインポートする際にターゲットサンドボックスに属性が見つからない有無を確認したり、デフォルトで既存の結合ポリシーを使用したりできるようになりました。 これらの機能について詳しくは、[ サンドボックスツール UI ガイド ](../../sandboxes/ui/sandbox-tooling.md) を参照してください。 |

{style="table-layout:auto"}

サンドボックスについて詳しくは、[ サンドボックスの概要 ](../../sandboxes/home.md) を参照してください。

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] を使用すると、[!DNL Experience Platform] に保存されている、個人（顧客、見込み客、ユーザー、組織など）に関連するデータをオーディエンスにセグメント化できます。オーディエンスは、セグメント定義または [!DNL Real-Time Customer Profile] データの他のソースを通じて作成できます。これらのオーディエンスは [!DNL Platform] で一元的に設定および管理されており、Adobe ソリューションから簡単にアクセスできます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| アカウントオーディエンス | アカウントオーディエンスの一般公開 Real-Time Customer Platform の B2B エディションと B2P エディションの両方で、ユーザーベースのオーディエンスからアカウントベースのオーディエンスまで、マーケティングセグメント化エクスペリエンスの完全な使いやすさと洗練を実現するために、アカウントのセグメント化を使用できるようになりました。 このリリースでは、ユーザーベースのオーディエンスをアカウントベースのオーディエンスに対する述語として使用でき、検索機能を追加し、カスタムエンティティの使用をサポートするほか、データガバナンスに準拠しています。 この機能について詳しくは、[ アカウントオーディエンスの概要 ](../../segmentation/types/account-audiences.md) を参照してください。 |

{style="table-layout:auto"}

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!BADGE Beta]{type=Informative} [!DNL Acxiom] ソース | [[!DNL Acxiom Prospecting Data Import] source](../../sources/tutorials/ui/create/data-partners/acxiom-prospecting-data-import.md) を使用して、見込み客サービスからExperience Platformにデータ [!DNL Acxiom] 取得してマッピングします。 |

{style="table-layout:auto"}

ソースについて詳しくは、[ ソースの概要 ](../../sources/home.md) を参照してください。
