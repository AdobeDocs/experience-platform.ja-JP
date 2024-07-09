---
title: Adobe Experience Platform リリースノート（2024年3月）
description: Adobe Experience Platform の 2024年3月のリリースノート。
exl-id: cab47a76-04f3-48ec-82aa-d17645e4eb15
source-git-commit: c35b43654d31f0f112258e577a1bb95e72f0a971
workflow-type: tm+mt
source-wordcount: '1190'
ht-degree: 33%

---

# Adobe Experience Platform リリースノート

**リリース日：2024年3月19日**

>[!TIP]
>
>の使用 [Adobe Experience Platform用語集](/help/landing/glossary.md) Real-time Customer Data PlatformとAdobe Experience Platformで使用される用語を理解します。 探している用語が見つからない場合は、ページのフィードバックオプションを使用して、用語集に新しい用語を追加するようにリクエストします。

Experience Platformの既存の機能に対するアップデート：

- [カタログサービス](#catalog-service)
- [データ収集](#data-collection)
- [データ準備](#data-prep)
- [宛先](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## カタログサービス {#catalog-service}

カタログサービスは、Adobe Experience Platform 内のデータの場所と系列のレコードのシステムです。Experience Platformに取り込まれるすべてのデータはファイルおよびディレクトリとして Data Lake に保存されますが、カタログには、参照や監視のために、これらのファイルおよびディレクトリのメタデータと説明が保持されます。

| 機能 | 説明 |
| --- | --- |
| その他のアクション | 操作の柔軟性を高め、データの管理に役立つように、詳細ビューの「その他のアクション」機能を使用して、データセットに対して追加のタスクを実行できるようになりました。 選択したデータセットの詳細ページから、データセットを削除するか、リアルタイム顧客プロファイルで使用できるようにします。<br>**注意：** プロファイル取り込みに対してデータセットを有効にする場合、データセットのスキーマは、リアルタイム顧客プロファイルと互換性がある必要があります。<br>![を使用したデータセットワークスペース [!UICONTROL ...詳細] ドロップダウンメニューがハイライト表示されている。](../2024/assets/march/more-actions.png "「詳細」ドロップダウンメニューがハイライト表示されたデータセットワークスペース。"){width="100" zoomable="yes"}。<br>を読み取る [データセットユーザーガイド](../../catalog/datasets/user-guide.md) 追加情報のドキュメント。 |

{style="table-layout:auto"}

Catalog Service について詳しくは、[Catalog Service の概要](../../catalog/home.md)を参照してください。

## データ準備 {#data-prep}

データ準備を使用すると、データエンジニアはエクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換および検証を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Adobe Analyticsの新しいマッパー関数 | Adobe Analyticsからイベントデータを抽出するために、次の関数を使用できるようになりました。 <ul><li>`aa_get_event_id`</li><li>`aa_get_event_value`</li><li>`aa_get_product_categories`</li><li>`aa_get_product_names`</li><li>`aa_get_product_quantities`</li><li>`aa_get_product_prices`</li><li>`aa_get_product_event_values`</li><li>`aa_get_product_evars`</li></ul> これらの関数について詳しくは、 [データ準備関数ガイド](../../data-prep/functions.md#analytics-functions) |

{style="table-layout:auto"}

データ準備について詳しくは、 [データ準備の概要](../../data-prep/home.md).

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能**

| タイプ | 機能 | 説明 |
| --- | --- | --- |
| 拡張機能 | [!DNL Merkury] タグ拡張機能 | この [[!DNL Merkury] タグ拡張機能](https://exchange.adobe.com/apps/ec/600027/merkury-tag) は、への匿名 web サイト訪問者に、業界をリードする一致率を提供します [!DNL Merkury] ID。 ブランドは、の機能を活用できます [!DNL Merkury] タグ付けおよびAdobeして、パーソナライズされた web サイトエクスペリエンスをリアルタイムで提供します。 さらに、 [!DNL Merkury] タグを使用すると、ファーストパーティのデジタルデータを、オンラインおよびオフラインの接続された顧客プロファイルと共に成長させることができます。 |

{style="table-layout:auto"}

データ収集について詳しくは、を参照してください。 [データ収集の概要](../../tags/home.md).

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先と更新された宛先** {#new-updated-destinations}

| 宛先 | タイプ | 説明 |
| ----------- | --------- | ----------- |
| [（Beta） Acxiom Data Enhancement Connection](../../destinations/catalog/data-partner/acxiom-data-enhancement.md) | 新規 | このコネクタを使用して、Real-Time CDPから Acxiom に対してファーストパーティ・プロファイルをアクティブ化し、データのエンリッチメントとマーケティング・チャネル全体での使用を実現します。 その後、Acxiom ソースを使用して、拡張データを含むプロファイルをインポートし、Real-Time CDPで操作できます。 |
| [（Beta） Acxiom Prospect Suppression connection](../../destinations/catalog/data-partner/acxiom-prospect-suppression.md) | 新規 | ファーストパーティオーディエンスを Acxiom の宛先にエクスポートして、Acxiom が既知の顧客やコンバート済み顧客を抑制できるようにします。 次に、を使用します [Acxiom prospecting データのインポート](../../sources/connectors/data-partners/acxiom-prospecting-data-import.md) ソース・コネクタ：既知のお客様またはコンバート済みのお客様を削除して、Acxiom から見込み客リストを取り込み、アクティブ化します。 |
| [Amazon Ads 接続](../../destinations/catalog/advertising/amazon-ads.md) | 更新 | Amazon Ads のMarketing Cloudにデータを書き出す際に、Amazon DSPまたはAmazon宛先（新規）にデータをルーティングできるようになりました。 |
| [LiveRamp オンボーディング接続](../../destinations/catalog/advertising/liveramp-onboarding.md) | 更新 | LiveRamp オンボーディング宛先で、ヨーロッパおよびオーストラリアへの配信がサポートされるようになりました [!DNL LiveRamp] [!DNL SFTP] インスタンス。 また、書き出されるファイルの最大サイズも 1,000 万行に増えました（以前は 500 万行でした）。 |

{style="table-layout:auto"}

<!--

**New or updated functionality** {#destinations-new-updated-functionality}

-->

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| Experience Platform UI マップのデータタイプのサポート | Platform UI でマップフィールドを定義することで、エクスペリエンスデータモデル（XDM）データ構造をさらにカスタマイズできます。 スキーマエディターでマップフィールドを作成して、柔軟なデータ構造をモデル化したり、キーと値のペアを効率的に保存したりできるようになりました。 新しいフィールドを定義してサブフィールドを設定し、フィールドグループに割り当てるには、タイプ ドロップダウンから「マップ」を選択します。 サポートされるマップ値のタイプは文字列および整数です。<br>![タイプおよびマップ値タイプのフィールドがハイライト表示されたスキーマエディター。](../2024/assets/march/maps.png "タイプおよびマップ値タイプのフィールドがハイライト表示されたスキーマエディター。"){width="100" zoomable="yes"}<br> 方法を説明します [ui でのマップフィールドの定義](../../xdm/ui/fields/map.md)、UI ガイドを参照してください。 |

{style="table-layout:auto"}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] を使用すると、[!DNL Experience Platform] に保存されている、個人（顧客、見込み客、ユーザー、組織など）に関連するデータをオーディエンスにセグメント化できます。オーディエンスは、セグメント定義または [!DNL Real-Time Customer Profile] データの他のソースを通じて作成できます。これらのオーディエンスは [!DNL Platform] で一元的に設定および管理されており、Adobe ソリューションから簡単にアクセスできます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 一括アクション | オーディエンスインベントリで一括アクションがサポートされるようになりました。 一括アクションを使用すると、複数のオーディエンスをすばやく選択して、それらをフォルダーに移動、タグを適用、アクセスラベルを適用または削除できます。 <br> ![オーディエンス UI ワークスペースでの一括アクション。](../2024/assets/march/bulk-actions.png "オーディエンス UI ワークスペースでの一括アクション。"){width="100" zoomable="yes"} <br>この機能について詳しくは、 [オーディエンスポータルの概要](../../segmentation/ui/audience-portal.md#bulk-actions). |

{style="table-layout:auto"}

セグメント化サービスの詳細については、 [セグメント化サービスの概要](../../segmentation/home.md).

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新しいソースと更新されたソース**

| 機能 | タイプ | 説明 |
| --- | --- | --- |
| [!BADGE Beta]{type=Informative} [!DNL Acxiom Data Ingestion] | 新規 | の使用 [[!DNL Acxiom Data Ingestion] ソース](../../sources/tutorials/ui/create/data-partners/acxiom-data-ingestion.md) 取り込む [!DNL Acxiom] データをReal-time Customer Data Platformに取り込み、ファーストパーティプロファイルを強化します。 その後、を使用できます [!DNL Acxiom] – の強化されたファーストパーティプロファイルにより、オーディエンスを向上させ、複数のマーケティングチャネルをまたいでアクティブ化します。 <br> ![Acxiom データ取り込みソース。](../2024/assets/march/acxiom-data-ingestion.png "新しい Acxiom データ取り込みソース。"){width="100" zoomable="yes"} <br> を読み取る [[!DNL Acxiom Data Ingestion] の概要](../../sources/connectors/data-partners/acxiom-data-ingestion.md) を開始する方法について説明します。 |
| [!BADGE Beta]{type=Informative} [!DNL Stripe] | 新規 | の使用 [[!DNL Stripe] ソース](../../sources/connectors/payments/stripe.md) 顧客の購入フロー中に取り込んだデータをExperience Platformに取り込みます。 取り込んだら、このデータを使用して、パーソナライズされたオファーを作成し、より豊富なビジネスインサイトを活用できます。 <br> ![Stripeソース。](../2024/assets/march/stripe.png "新しいStripeソース。"){width="100" zoomable="yes"} <br> を読み取る [[!DNL Stripe] の概要](../../sources/connectors/payments/stripe.md) を開始する方法について説明します。 |
| の UI サポート [!DNL Snowflake Streaming] | 新規 | これで、を使用できます [[!DNL Snowflake Streaming] ソース](../../sources/tutorials/ui/create/databases/snowflake-streaming.md) Experience PlatformUI で、からデータをストリーミングするには [!DNL Snowflake] データベース。 <br> ![Snowflakeストリーミングソース。](../2024/assets/march/snowflake-streaming.png "新しいSnowflakeストリークソース。"){width="100" zoomable="yes"} <br> を読み取る [[!DNL Snowflake Streaming] の概要](../../sources/connectors/databases/snowflake-streaming.md) を開始する方法について説明します。 |

{style="table-layout:auto"}

ソースについて詳しくは、 [ソースの概要](../../sources/home.md).
