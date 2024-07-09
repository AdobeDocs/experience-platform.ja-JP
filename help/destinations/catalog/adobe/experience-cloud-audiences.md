---
title: Experience Cloud Audiences
description: Real-time Customer Data Platformのオーディエンスを様々なExperience Cloudアプリと共有する方法を説明します。
last-substantial-update: 2023-09-28T00:00:00Z
exl-id: 2bdbcda3-2efb-4a4e-9702-4fd9991e9461
source-git-commit: c35b43654d31f0f112258e577a1bb95e72f0a971
workflow-type: tm+mt
source-wordcount: '1703'
ht-degree: 16%

---


# [!UICONTROL Experience Cloudオーディエンス] 接続

>[!AVAILABILITY]
>
> この宛先は、次のユーザーが使用できます [Adobe Real-time Customer Data Platform Prime と Ultimate](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform.html) 顧客。

この宛先を使用して、Real-Time CDPからAudience ManagerやAdobe Analyticsにオーディエンスをアクティブ化します。

オーディエンスをAdobe Analyticsに送信するには、Audience Managerライセンスが必要です。 詳しくは、 [Audience Analyticsの概要](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html?lang=en).

オーディエンスを他のAdobeソリューションに送信するには、Real-Time CDPからへの直接接続を使用します。 [Adobe Target](../personalization/adobe-target-connection.md), [Adobe Advertising](../advertising/adobe-advertising-cloud-connection.md), [Adobe Campaign](../email-marketing/adobe-campaign.md) および [Marketo Engage](../adobe/marketo-engage.md).

>[!IMPORTANT]
>
>この宛先はを置き換えます [従来のオーディエンス共有統合](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aep-segments-in-aam) Real-time Customer Data Platformから様々なExperience Cloudソリューションへ。
> 
>を介して、Real-Time CDPからAudience Managerや他のExperience Cloudソリューションにオーディエンスを既に共有している場合 [従来のオーディエンス共有統合](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aep-segments-in-aam)。この宛先を使用する前に、カスタマーケアに連絡して、従来の統合を無効にする必要があります。

![宛先カタログでハイライト表示されているExperience Cloudオーディエンスの宛先。](../../assets/catalog/adobe/experience-cloud-audiences/experience-cloud-audiences-destination-catalog.png)

## 使用例とメリット {#use-cases}

を使用する方法とタイミングをより深く理解するために、 [!UICONTROL Experience Cloudオーディエンス] 宛先の場合、Real-Time CDPのお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### データ管理プラットフォームのユースケースの有効化 {#dmp-use-cases}

Audience Managerすると、次のような、Data Management Platform のユースケースにReal-Time CDP オーディエンスを使用できます。

* 追加中 [サードパーティデータ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-types-collected.html#third-party-data) セグメントに対して
* [アルゴリズムモデリング](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/algorithmic-models/look-alike-modeling/understanding-models.html);
* Real-Time CDPの宛先カタログでまだサポートされていない cookie ベースの宛先に対するオーディエンスのアクティブ化。

### 書き出されたオーディエンスの詳細な制御 {#segments-control}

Audience Managerに書き出すオーディエンスおよびそれ以降のオーディエンスを選択するには、Experience Cloudオーディエンス宛先を介した新しい self-service audience-sharing 統合を使用します。  これにより、他のExperience Cloudソリューションと共有するオーディエンスや、Real-Time CDPでのみ保持するオーディエンスを決定できます。

従来のオーディエンス共有統合では、Audience Manager以降に書き出すオーディエンスを詳細に制御できませんでした。

### Real-Time CDP オーディエンスのAdobe Analyticsとの共有 {#share-audiences-with-analytics}

Experience Cloudオーディエンス宛先に送信するオーディエンスは、Adobe Analyticsに自動的に表示されません。

オーディエンスをAdobe Analyticsに送信する前に、次の操作をおこなう必要があります [analytics とAudience Manager用のExperience Cloud ID サービスの実装](https://experienceleague.adobe.com/docs/id-service/using/implementation/setup-aam-analytics.html?lang=en).

>[!IMPORTANT]
>
>オーディエンスExperience Cloud宛先を使用してReal-Time CDPからAdobe Analyticsにオーディエンスを送信するには、Audience Managerライセンスが必要です。

### Real-Time CDP オーディエンスを他のExperience Cloudソリューションと共有する {#share-segments-with-other-solutions}

Real-Time CDP AudiencesExperience Cloudカードを使用して、オーディエンスを他の宛先ソリューションと共有できます。

ただし、Adobeでは、オーディエンスをこれらのソリューションと共有する場合に、次の専用の宛先カードを使用することを強くお勧めします。

* [Adobe Campaign](../email-marketing/adobe-campaign.md)
* [Adobe Target](../personalization/adobe-target-connection.md)
* [Advertising Cloud](../advertising/adobe-advertising-cloud-connection.md)
* [Marketo](../adobe/marketo-engage.md)

## 前提条件 {#prerequisites}

>[!IMPORTANT]
>
> * を有効にするには、Audience Managerライセンスが必要です [データ管理プラットフォームのユースケース](#dmp-use-cases) 上記で述べた。
> * あなた *実行* Real-Time CDP オーディエンスをAdobe Analyticsと共有するには、Audience Managerライセンスが必要です。
> * あなた *必要ありません* Real-Time CDP オーディエンスをAdobe Advertising Cloud、Adobe Target、Marketoおよびその他のExperience Cloudソリューションと共有するためのAudience Managerライセンス（で言及） [上のセクション](#share-segments-with-other-solutions).

### 従来のオーディエンス共有ソリューションを使用している顧客の場合

を介して、Real-Time CDPからAudience Managerや他のExperience Cloudソリューションにオーディエンスを既に共有している場合 [従来のオーディエンス共有統合](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aep-segments-in-aam)レガシー統合を無効にするには、カスタマーケアへのお問い合わせ。

プロビジョニング解除チケットを解決するまでの所要時間は、6 営業日以内です。 既存のレガシー統合を無効にした後、次の手順に進むことができます [接続の作成](#connect) セルフサービスの宛先カードを使用する。

>[!IMPORTANT]
>
>Real-Time CDPから他のソリューションへのオーディエンスの書き出しは、チケットの解決から、宛先カードを介した新しい接続が確立されるまでの間に停止されます。 チケットを閉じた後に宛先カードを介して接続を作成すると、このダウンタイムを最小限に抑えることができます。

## 既知の制限事項と引き出し {#known-limitations}

Experience Cloudオーディエンスカードを使用する際には、次の既知の制限事項と重要な引き出しに注意してください。

* 現在、1 つのExperience Cloudオーディエンスの宛先がサポートされています。 2 つ目の宛先接続を設定しようとすると、エラーが発生します。
* 宛先に接続する際には、へのオプションが表示されます [データフローアラートの有効化](../../ui/alerts.md). UI には表示されますが、は **現在、「アラートを有効にする」オプションはサポートされていません**.
* **オーディエンスバックフィルのサポート**:Audience Managerまたはその他のExperience Cloudソリューションへの最初の書き出しには、オーディエンスの母集団履歴が含まれます。 ユーザー（ [従来のオーディエンス共有統合](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aep-segments-in-aam) この宛先を設定しているユーザーは、約 6 時間のバックフィル差を想定する必要があります。
* 元のオーディエンス [オーディエンス構成](../../../segmentation/ui/audience-composition.md) は直接にはサポートされません。 この宛先に対して複合オーディエンスをアクティブ化するには、を使用してオーディエンス定義を作成する必要があります。 [セグメントビルダー](../../../segmentation/ui/segment-builder.md) 複合オーディエンスに基づいて、新しく作成したオーディエンスをアクティブ化します。

### オーディエンスをアクティブ化する際の待ち時間 {#audience-activation-latency}

オーディエンスがReal-Time CDPで最初にアクティブ化されてから、Audience Managerやその他のExperience Cloudソリューションで使用できるようになるまでの待ち時間は 4 時間です。

オーディエンスがすべてのユースケースでAudience Managerで完全に使用できるようになるまで、最大 24 時間かかる場合があります。 Experience CloudオーディエンスのオーディエンスがAudience Managerレポートに表示されるまで、最大 48 時間かかる場合があります。

オーディエンス名などのメタデータは、Experience Cloudオーディエンスの宛先への書き出しの設定後数分以内にAudience Managerで使用できます。

## サポートされている ID {#supported-identities}

に書き出されるプロファイル [!UICONTROL Experience Cloudオーディエンス] 宛先は、以下の表で説明する ID にマッピングされます。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| ECID | Experience Cloud ID | ECID を表す名前空間。 この名前空間は、「Adobe Marketing Cloud ID」、「Adobe Experience Cloud ID」、「Adobe Experience Platform ID」という別名で呼ばれることもあります。次のドキュメントを参照してください： [ECID](/help/identity-service/features/ecid.md) を参照してください。 |
| GAID | GOOGLE ADVERTISING ID | Google Advertising ID （GAID）のプライマリ ID を持つReal-Time CDPに取り込まれたプロファイルは、この宛先に書き出すことができます。 |
| IDFA | Apple の広告主 ID | Apple ID for Advertisers （IDFA）のプライマリ ID を持つReal-Time CDPに取り込まれたプロファイルは、この宛先に書き出すことができます。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | ハッシュ化されたメールアドレスのプライマリ ID を使用してReal-Time CDPに取り込まれたプロファイルは、この宛先に書き出すことができます。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
| ---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platformを通じて生成されたオーディエンス [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | 上記の節で示した ID をキーにオーディエンスのすべてのメンバーを書き出しています。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンスの評価に基づいてReal-Time CDP内でプロファイルが更新されると、コネクタは更新を宛先プラットフォームに送信します。 詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、以下を選択します **[!UICONTROL の設定]** カタログ内の宛先カード表示で、以下を選択します。 **[!UICONTROL 宛先への接続]**.

![Experience Cloudオーディエンス宛先の「宛先に接続」オプションのビュー。](../../assets/catalog/adobe/experience-cloud-audiences/experience-cloud-audiences-authenticate-to-destination.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![Experience Cloudオーディエンスの宛先に接続するための必須の設定とオプションの設定を示す、新しい宛先画面を設定します。](../..//assets/catalog/adobe/experience-cloud-audiences/connect-to-destination.png)

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

Read [ストリーミングオーディエンス書き出し宛先に対するプロファイルとオーディエンスのアクティブ化](/help/destinations/ui/activate-segment-streaming-destinations.md) この宛先に対してオーディエンスをアクティブ化する手順については、を参照してください。 不可 [マッピングステップ](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping) は必須で、 [スケジュール手順](/help/destinations/ui/activate-segment-streaming-destinations.md#scheduling) はこの宛先で利用できます。

## データの書き出しを検証する {#exported-data}

データの書き出しが正常に行われたことを検証するには、オーディエンスが目的のExperience Cloudソリューションを成功に導いたかどうかを確認します。

### Audience Manager内のデータの検証

Real-Time CDP オーディエンスは、次のようにAudience Managerに表示されます [signals](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aep-segments-as-aam-signals), [特性](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aep-segments-as-aam-traits)、および [セグメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aep-segments-as-aam-segments). 上記のドキュメントリンクに記載されているように、データが表示されているかどうかをAudience Managerで確認できます。

セグメント名は、オーディエンスがReal-Time CDPから送信されてから 15 分後にAudience Managerで入力を開始します。

セグメント母集団は、Real-Time CDPから送信されてから 6 時間以内にAudience Managerに送られ始め、Audience Managerで 24 時間ごとに更新されます。

完全な母集団は 72 時間後にAudience Managerに表示され、オーディエンスがReal-Time CDPで宛先から削除されない限り、母集団は引き続きAudience Managerに送られます。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Real-Time CDP] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

Real-Time CDPのデータガバナンスは、次の両方によって適用されます [データ使用ラベル](/help/data-governance/labels/reference.md) とマーケティングアクション。
データ使用ラベルはアプリケーションに転送されますが、マーケティングアクションは転送されません。 つまり、Audience Managerに到達したReal-Time CDPのオーディエンスは、使用可能な任意の宛先に書き出すことができます。 Audience Managerでは、次を使用できます [データ書き出しコントロール](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/data-export-controls.html) オーディエンスが特定の宛先に書き出されるのをブロックする。

とマークされたオーディエンス [!DNL HIPAA] マーケティングアクションは、Real-Time CDPからAudience Managerには送信されません。

### Audience Managerでの権限管理

Audience Managerのオーディエンスと特性は、次の条件の対象となります [役割ベースのアクセス制御](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/administration/administration-overview.html) （RBAC）。

Real-Time CDPから書き出されたオーディエンスは、というAudience Managerの特定のデータソースに割り当てられます **[!UICONTROL Experience Platformセグメント]**.

特定のユーザーのみにオーディエンスへのアクセスを許可するには、を使用します [役割ベースのアクセス制御](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/administration/administration-overview.html) Real-Time CDP オーディエンスから作成されたオーディエンスおよび特性へのユーザーアクセスを設定します。
