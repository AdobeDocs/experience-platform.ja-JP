---
title: Adobe Experience Platform リリースノート（2024年3月）
description: Adobe Experience Platform の 2024年3月のリリースノート。
exl-id: cab47a76-04f3-48ec-82aa-d17645e4eb15
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1192'
ht-degree: 33%

---

# Adobe Experience Platform リリースノート

**リリース日：2024年3月19日**

>[!TIP]
>
>Real-Time Customer Data PlatformおよびAdobe Experience Platformで使用される用語については [&#128279;](/help/landing/glossary.md)Adobe Experience Platform用語集 &rbrace; を参照してください。 探している用語が見つからない場合は、ページのフィードバックオプションを使用して、用語集に新しい用語を追加するようにリクエストします。

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
| その他のアクション | 操作の柔軟性を高め、データの管理に役立つように、詳細ビューの「その他のアクション」機能を使用して、データセットに対して追加のタスクを実行できるようになりました。 選択したデータセットの詳細ページから、データセットを削除するか、リアルタイム顧客プロファイルで使用できるようにします。<br>**メモ：** プロファイル取り込みに対してデータセットを有効にする場合、データセットのスキーマはリアルタイム顧客プロファイルと互換性がある必要があります。<br>![[!UICONTROL &#x200B; を持つデータセットワークスペース詳細 &#x200B;] ドロップダウンメニューがハイライト表示されている。](../2024/assets/march/more-actions.png " 詳細ドロップダウンメニューがハイライト表示されたデータセットワークスペース。"){width="100" zoomable="yes"}。詳しくは、<br> データセットユーザーガイド [&#128279;](../../catalog/datasets/user-guide.md) ドキュメントを参  してください。 |

{style="table-layout:auto"}

Catalog Service について詳しくは、[Catalog Service の概要](../../catalog/home.md)を参照してください。

## データ準備 {#data-prep}

データ準備を使用すると、データエンジニアはエクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換および検証を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Adobe Analyticsの新しいマッパー関数 | Adobe Analyticsからイベントデータを抽出するために、次の関数を使用できるようになりました。 <ul><li>`aa_get_event_id`</li><li>`aa_get_event_value`</li><li>`aa_get_product_categories`</li><li>`aa_get_product_names`</li><li>`aa_get_product_quantities`</li><li>`aa_get_product_prices`</li><li>`aa_get_product_event_values`</li><li>`aa_get_product_evars`</li></ul> これらの関数の詳細については、[ データ準備関数ガイド ](../../data-prep/functions.md#analytics-functions) を参照してください。 |

{style="table-layout:auto"}

データ準備について詳しくは、[ データ準備の概要 ](../../data-prep/home.md) を参照してください。

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能**

| タイプ | 機能 | 説明 |
| --- | --- | --- |
| 拡張機能 | [!DNL Merkury] タグ拡張機能 | [[!DNL Merkury]  タグ拡張機能 ](https://exchange.adobe.com/apps/ec/600027/merkury-tag) は、[!DNL Merkury] ID に対する匿名 web サイト訪問者の一致率を業界トップクラスに高めます。 ブランドは、[!DNL Merkury] タグとAdobeの機能を活用して、パーソナライズされた web サイトエクスペリエンスをリアルタイムで提供できます。 さらに、[!DNL Merkury] タグを使用すると、ファーストパーティデジタルデータに加えて、オンラインおよびオフラインの接続された顧客プロファイルを拡張できます。 |

{style="table-layout:auto"}

データ収集について詳しくは、[ データ収集の概要 ](../../tags/home.md) を参照してください。

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先と更新された宛先** {#new-updated-destinations}

| 宛先 | タイプ | 説明 |
| ----------- | --------- | ----------- |
| [ （Beta） Acxiom Data Enhancement Connection](../../destinations/catalog/data-partner/acxiom-data-enhancement.md) | 新規 | このコネクタを使用して、Real-Time CDPから Acxiom に対してファーストパーティ・プロファイルをアクティブ化し、データのエンリッチメントとマーケティング・チャネル全体での使用を実現します。 その後、Acxiom ソースを使用して、拡張データを含むプロファイルをインポートし、Real-Time CDPで操作できます。 |
| [ （Beta） Acxiom Prospect Suppression connection](../../destinations/catalog/data-partner/acxiom-prospect-suppression.md) | 新規 | ファーストパーティオーディエンスを Acxiom の宛先にエクスポートして、Acxiom が既知の顧客やコンバート済み顧客を抑制できるようにします。 次に、[Acxiom prospecting data import](../../sources/connectors/data-partners/acxiom-prospecting-data-import.md) ソース・コネクタを使用して、既知のお客様またはコンバート済みのお客様を削除した状態で、Acxiom から見込み客リストを取り込んでアクティブ化します。 |
| [Amazon Ads 接続 ](../../destinations/catalog/advertising/amazon-ads.md) | 更新 | Amazon Marketing Cloudの宛先にデータを書き出す際に、Amazon Ads またはAmazon DSP（新規）にデータをルーティングできるようになりました。 |
| [LiveRamp オンボーディング接続 ](../../destinations/catalog/advertising/liveramp-onboarding.md) | 更新 | LiveRamp オンボーディングの宛先で、[!DNL SFTP] インスタンスを [!DNL LiveRamp] 用したヨーロッパおよびオーストラリアへの配信がサポートされるようになりました。 また、書き出されるファイルの最大サイズも 1,000 万行に増えました（以前は 500 万行でした）。 |

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
| Experience Platform UI マップのデータタイプのサポート | Experience Platform UI でマップフィールドを定義することで、エクスペリエンスデータモデル（XDM）データ構造をさらにカスタマイズできます。 スキーマエディターでマップフィールドを作成して、柔軟なデータ構造をモデル化したり、キーと値のペアを効率的に保存したりできるようになりました。 新しいフィールドを定義してサブフィールドを設定し、フィールドグループに割り当てるには、タイプ ドロップダウンから「マップ」を選択します。 サポートされるマップ値のタイプは文字列および整数です。<br>![ タイプおよびマップ値タイプのフィールドがハイライト表示されたスキーマエディター。](../2024/assets/march/maps.png " タイプおよびマップ値タイプのフィールドがハイライト表示されたスキーマエディター。"){width="100" zoomable="yes"}<br> UI でマップフィールドを定義 [ する方法については ](../../xdm/ui/fields/map.md) UI ガイドを参照してください。 |

{style="table-layout:auto"}

Experience Platformの XDM について詳しくは、「[XDM システムの概要 ](../../xdm/home.md)」を参照してください。

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] を使用すると、[!DNL Experience Platform] に保存されている、個人（顧客、見込み客、ユーザー、組織など）に関連するデータをオーディエンスにセグメント化できます。オーディエンスは、セグメント定義または [!DNL Real-Time Customer Profile] データの他のソースを通じて作成できます。これらのオーディエンスは [!DNL Experience Platform] で一元的に設定および管理されており、Adobe ソリューションから簡単にアクセスできます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 一括アクション | オーディエンスインベントリで一括アクションがサポートされるようになりました。 一括アクションを使用すると、複数のオーディエンスをすばやく選択して、それらをフォルダーに移動、タグを適用、アクセスラベルを適用または削除できます。<br> ![ オーディエンス UI ワークスペースでの一括アクション。](../2024/assets/march/bulk-actions.png " オーディエンス UI ワークスペースでの一括アクション。"){width="100" zoomable="yes"} <br> この機能について詳しくは、[Audience Portal の概要 ](../../segmentation/ui/audience-portal.md#bulk-actions) を参照してください。 |

{style="table-layout:auto"}

セグメント化サービスについて詳しくは、[ セグメント化サービスの概要 ](../../segmentation/home.md) を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新しいソースと更新されたソース**

| 機能 | タイプ | 説明 |
| --- | --- | --- |
| [!BADGE Beta]{type=Informative} [!DNL Acxiom Data Ingestion] | 新規 | [[!DNL Acxiom Data Ingestion]  ソース ](../../sources/tutorials/ui/create/data-partners/acxiom-data-ingestion.md) を使用して、[!DNL Acxiom] データをReal-Time Customer Data Platformに取り込み、ファーストパーティプロファイルを強化します。 その後、エンリッチメントされ [!DNL Acxiom] ファーストパーティプロファイルを使用して、オーディエンスを向上させ、すべてのマーケティングチャネルをアクティブ化できます。<br> ![Acxiom データ取り込みソース。](../2024/assets/march/acxiom-data-ingestion.png " 新しい Acxiom データ取り込みソース。"){width="100" zoomable="yes"} <br> 始める方法については、[[!DNL Acxiom Data Ingestion]  概要 ](../../sources/connectors/data-partners/acxiom-data-ingestion.md) を参照してください。 |
| [!BADGE Beta]{type=Informative} [!DNL Stripe] | 新規 | [[!DNL Stripe]  ソース ](../../sources/connectors/payments/stripe.md) を使用すると、購入フロー中に顧客が取り込んだデータをExperience Platformに取り込むことができます。 取り込んだら、このデータを使用して、パーソナライズされたオファーを作成し、より豊富なビジネスインサイトを活用できます。<br> ![Stripe ソース。](../2024/assets/march/stripe.png " 新しいStripe ソース。"){width="100" zoomable="yes"} <br> 始める方法については、[[!DNL Stripe]  概要 ](../../sources/connectors/payments/stripe.md) を参照してください。 |
| [!DNL Snowflake Streaming] の UI サポート | 新規 | Experience Platform UI で [[!DNL Snowflake Streaming] source](../../sources/tutorials/ui/create/databases/snowflake-streaming.md) を使用して、[!DNL Snowflake] データベースからデータをストリーミングできるようになりました。<br> ![Snowflake ストリーミングソース。](../2024/assets/march/snowflake-streaming.png " 新しいSnowflakeのストリークソース。"){width="100" zoomable="yes"} <br> 始める方法については、[[!DNL Snowflake Streaming]  概要 ](../../sources/connectors/databases/snowflake-streaming.md) を参照してください。 |

{style="table-layout:auto"}

ソースについて詳しくは、[ ソースの概要 ](../../sources/home.md) を参照してください。
