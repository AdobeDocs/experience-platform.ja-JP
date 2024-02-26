---
title: Experience Cloud Audiences
description: Real-time Customer Data Platformのオーディエンスを様々なExperience Cloudアプリに共有する方法を説明します。
last-substantial-update: 2023-09-28T00:00:00Z
exl-id: 2bdbcda3-2efb-4a4e-9702-4fd9991e9461
source-git-commit: 188398e3483541ca482f5c1cfdce307160ada2da
workflow-type: tm+mt
source-wordcount: '1703'
ht-degree: 16%

---


# [!UICONTROL Experience Cloudオーディエンス] 接続

>[!AVAILABILITY]
>
> この宛先は次の場所で使用できます： [Adobe Real-time Customer Data Platform Prime と Ultimate](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform.html) 顧客。

この宛先を使用して、Real-Time CDPからAudience ManagerおよびAdobe Analyticsにオーディエンスをアクティブ化します。

オーディエンスをAdobe Analyticsに送信するには、Audience Managerライセンスが必要です。 詳しくは、 [Audience Analyticsの概要](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html?lang=ja).

オーディエンスを他のAdobeソリューションに送信するには、Real-Time CDPからへの直接接続を使用します。 [Adobe Target](../personalization/adobe-target-connection.md), [Adobe Advertising](../advertising/adobe-advertising-cloud-connection.md), [Adobe Campaign](../email-marketing/adobe-campaign.md) および [Marketo Engage](../adobe/marketo-engage.md).

>[!IMPORTANT]
>
>この宛先は、 [レガシーオーディエンス共有の統合](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aep-segments-in-aam) Real-time Customer Data Platformから様々なExperience Cloudソリューションへ
> 
>既に、Real-Time CDPからAudience Managerおよび他のExperience Cloudソリューションに、 [レガシーオーディエンス共有の統合](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aep-segments-in-aam)の場合、この宛先を使用する前に、カスタマーケアに連絡して従来の統合を無効にする必要があります。

![Experience Cloudオーディエンスの宛先。宛先カタログでハイライト表示されます。](../../assets/catalog/adobe/experience-cloud-audiences/experience-cloud-audiences-destination-catalog.png)

## 使用例とメリット {#use-cases}

をいつどのように使用するかをより深く理解するのに役立ちます。 [!UICONTROL Experience Cloudオーディエンス] の宛先について、Real-Time CDPのお客様がこの宛先を使用して解決できる使用例を以下に示します。

### データ管理プラットフォームの使用例の有効化 {#dmp-use-cases}

Audience Managerでは、Real-Time CDPオーディエンスを、次のようなデータ管理プラットフォームの使用例に使用できます。

* 追加中 [サードパーティデータ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-types-collected.html#third-party-data) をセグメントに追加します。
* [アルゴリズムモデリング](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/algorithmic-models/look-alike-modeling/understanding-models.html)
* Real-Time CDPの宛先カタログでまだサポートされていない Cookie ベースの宛先に対してオーディエンスをアクティブ化する。

### エクスポートされたオーディエンスの詳細な制御 {#segments-control}

Audience Manager以降に書き出すオーディエンスを選択するには、 AudiencesExperience Cloudの宛先で、新しいセルフサービスのオーディエンス共有統合を使用します。  これにより、他のExperience Cloudソリューションと共有するオーディエンスや、Real-Time CDPにのみ保持するオーディエンスを決定できます。

従来のオーディエンス共有統合では、どのオーディエンスをAudience Manager以降に書き出すかを詳細に制御できませんでした。

### Real-Time CDPオーディエンスをAdobe Analyticsと共有する {#share-audiences-with-analytics}

Audiences の宛先に送信したオーディエンスは、Experience CloudAdobe Analyticsに自動的には表示されません。

オーディエンスをAdobe Analyticsに送信する前に、 [Experience CloudID サービスの Analytics への実装とAudience Manager](https://experienceleague.adobe.com/docs/id-service/using/implementation/setup-aam-analytics.html?lang=en).

>[!IMPORTANT]
>
>Real-Time CDPからAdobe Analyticsに Audiences の宛先を通じてオーディエンスを送信するには、Experience Cloudライセンスが必要です。

### 他のExperience CloudソリューションとReal-Time CDPオーディエンスを共有する {#share-segments-with-other-solutions}

「 Real-Time CDP Audiences 」の宛先カードを使用して、オーディエンスを他のExperience Cloudソリューションと共有できます。

ただし、Adobeでオーディエンスを共有する場合は、次の専用の宛先カードを使用することを強くお勧めします。

* [Adobe Campaign](../email-marketing/adobe-campaign.md)
* [Adobe Target](../personalization/adobe-target-connection.md)
* [Advertising Cloud](../advertising/adobe-advertising-cloud-connection.md)
* [Marketo](../adobe/marketo-engage.md)

## 前提条件 {#prerequisites}

>[!IMPORTANT]
>
> * を有効にするにはAudience Managerライセンスが必要です。 [データ管理プラットフォームの使用例](#dmp-use-cases) 詳しくは、上記を参照してください。
> * あなた *do* Real-Time CDPオーディエンスをAdobe Analyticsと共有するには、Audience Managerライセンスが必要です。
> * あなた *不要* Real-Time CDPオーディエンスをAdobe Advertising Cloud、Adobe Target、Marketoおよびその他のExperience Cloudソリューション ( [上のセクション](#share-segments-with-other-solutions).

### 従来のオーディエンス共有ソリューションを使用しているお客様向け

既に、Real-Time CDPからAudience Managerおよび他のExperience Cloudソリューションに、 [レガシーオーディエンス共有の統合](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aep-segments-in-aam)の場合、レガシー統合を無効にするには、カスタマーケアにお問い合わせいただく必要があります。

プロビジョニング解除チケットの解決にかかる所要時間は 6 営業日以下です。 既存のレガシー統合を無効にした後、次に進むことができます： [接続の作成](#connect) セルフサービスの宛先カードを使用する。

>[!IMPORTANT]
>
>Real-Time CDPから他のソリューションへのオーディエンスのエクスポートは、チケットの解決から宛先カードを通じて新しい接続が確立されるまでの間に停止されます。 チケットが閉じられた後に宛先カードを介して接続を作成することで、このダウンタイムを最小限に抑えることができます。

## 既知の制限事項と注意事項 {#known-limitations}

オーディエンスExperience Cloudを使用する際には、次の既知の制限事項と重要な注意事項に注意してください。

* 現在、単一のExperience Cloudオーディエンスの宛先がサポートされています。 2 つ目の宛先接続を構成しようとすると、エラーが発生します。
* 宛先に接続する際に、次のオプションが表示されます。 [データフローアラートを有効にする](../../ui/alerts.md). UI には表示されますが、 **アラートの有効化オプションは、現在サポートされていません**.
* **オーディエンスのバックフィルのサポート**：最初のAudience Managerへのエクスポートまたは他のExperience Cloudソリューションには、オーディエンスの過去の母集団が含まれます。 のユーザー [レガシーオーディエンス共有の統合](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aep-segments-in-aam) この宛先を設定するユーザーは、約 6 時間のバックフィルの違いを期待する必要があります。
* 元のオーディエンス [オーディエンスの構成](../../../segmentation/ui/audience-composition.md) は、直接はサポートされていません。 この宛先に対して複合オーディエンスをアクティブ化するには、次を通じてオーディエンス定義を作成する必要があります。 [セグメントビルダー](../../../segmentation/ui/segment-builder.md) 複合オーディエンスに基づいて、新しく作成したオーディエンスをアクティブ化します。

### オーディエンスをアクティブ化する際の遅延 {#audience-activation-latency}

Real-Time CDPでオーディエンスが最初にアクティブ化されてから、Audience Managerや他のExperience Cloudソリューションでオーディエンスを使用する準備が整うまでに、4 時間の遅延が生じます。

すべてのユースケースで、オーディエンスがAudience Managerで完全に利用できるようになるまでに最大 24 時間かかる場合があります。 Experience CloudオーディエンスからのオーディエンスがAudience Managerレポートに表示されるまで、最大 48 時間かかる場合があります。

オーディエンス名などのメタデータは、「オーディエンスのExperience Cloud」の宛先への書き出しが設定されてから数分以内にAudience Managerで使用できます。

## サポートされている ID {#supported-identities}

に書き出されるプロファイル [!UICONTROL Experience Cloudオーディエンス] 宛先は、次の表で説明する id にマッピングされます。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| ECID | Experience Cloud ID | ECID を表す名前空間。 この名前空間は、「Adobe Marketing Cloud ID」、「Adobe Experience Cloud ID」、「Adobe Experience Platform ID」という別名で呼ばれることもあります。次のドキュメントを参照してください： [ECID](/help/identity-service/features/ecid.md) を参照してください。 |
| GAID | Google Advertising ID | Google広告 ID(GAID) のプライマリ ID を持つReal-Time CDPに取り込まれたプロファイルは、この宛先に書き出すことができます。 |
| IDFA | Apple の広告主 ID | Apple ID for Advertisers(IDFA) のプライマリ ID を持つReal-Time CDPに取り込まれたプロファイルは、この宛先に書き出すことができます。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | ハッシュ化された電子メールアドレスのプライマリ ID を使用してReal-Time CDPに取り込まれたプロファイルは、この宛先に書き出すことができます。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの起源 | サポートあり | 説明 |
---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/overview.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | 上記の節に示した ID をキーにしたオーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンスの評価に基づいてReal-Time CDPでプロファイルが更新されると、コネクタは更新を宛先プラットフォームに送信します。 詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、「 」を選択します。 **[!UICONTROL 設定]** カタログの宛先カード表示で、「 」を選択します。 **[!UICONTROL 宛先に接続]**.

![Audiences の宛先の「宛先に接続」オプションのExperience Cloud。](../../assets/catalog/adobe/experience-cloud-audiences/experience-cloud-audiences-authenticate-to-destination.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![Audiences の宛先に接続するための必須およびオプション設定を示す新しいExperience Cloud画面を設定します。](../..//assets/catalog/adobe/experience-cloud-audiences/connect-to-destination.png)

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

読み取り [ストリーミングオーディエンスの書き出し先に対するプロファイルとオーディエンスのアクティブ化](/help/destinations/ui/activate-segment-streaming-destinations.md) を参照してください。 いいえ [マッピング手順](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping) 必須で、いいえ [スケジューリング手順](/help/destinations/ui/activate-segment-streaming-destinations.md#scheduling) は、この宛先で使用できます。

## データの書き出しを検証する {#exported-data}

データの書き出しが正常に行われたことを検証するには、オーディエンスが目的のExperience Cloudソリューションに正常に到達したことを確認します。

### データの検証のAudience Manager

Real-Time CDPオーディエンスは、Audience Managerに [signals](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aep-segments-as-aam-signals), [traits](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aep-segments-as-aam-traits)、および [セグメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aep-segments-as-aam-segments). 上記のドキュメントリンクで説明したようにデータが表示されたかどうかをAudience Managerで確認できます。

セグメント名は、オーディエンスがReal-Time CDPから送信されてから 15 分後にAudience Managerに入力され始めます。

セグメント母集団は、Real-Time CDPから送信されてから 6 時間以内にAudience Managerに流入し始め、Audience Managerで 24 時間ごとに更新されます。

72 時間後にはAudience Managerに完全な母集団が表示され、オーディエンスがReal-Time CDPの宛先から削除されない限り、母集団は引き続きAudience Managerに送られます。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Real-Time CDP] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

Real-Time CDPのデータガバナンスは、 [データ使用ラベル](/help/data-governance/labels/reference.md) およびマーケティングアクション。
データ使用ラベルはアプリケーションに転送されますが、マーケティングアクションは転送されません。 つまり、Audience Managerに到達すると、Real-Time CDPのオーディエンスを使用可能な任意の宛先に書き出すことができます。 Audience Managerでは、 [データ書き出しコントロール](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/data-export-controls.html?lang=ja) を使用して、オーディエンスが特定の宛先に書き出されるのを防ぎます。

次の項目でマークされたオーディエンス [!DNL HIPAA] マーケティングアクションがReal-Time CDPからAudience Managerに送信されない。

### 権限の管理 (Audience Manager)

Audience Managerのオーディエンスと特性は、 [ロールベースのアクセス制御](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/administration/administration-overview.html?lang=ja) (RBAC) を参照してください。

Real-Time CDPから書き出されたオーディエンスは、Audience Manager「 」の特定のデータソースに割り当てられます **[!UICONTROL Experience Platformセグメント]**.

特定のユーザーのみがオーディエンスにアクセスできるようにするには、 [ロールベースのアクセス制御](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/administration/administration-overview.html?lang=ja) Real-Time CDPオーディエンスから作成されたオーディエンスおよび特性へのユーザーアクセスを設定する場合。
