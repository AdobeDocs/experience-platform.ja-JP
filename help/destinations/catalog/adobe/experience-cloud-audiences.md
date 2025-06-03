---
title: Experience Cloud Audiences
description: Real-Time Customer Data Platformのオーディエンスを様々なExperience Cloud アプリに共有する方法を説明します。
last-substantial-update: 2023-09-28T00:00:00Z
exl-id: 2bdbcda3-2efb-4a4e-9702-4fd9991e9461
source-git-commit: 00da5d8c0eaaecb5b1f5ad5bff7482f3bf4023e2
workflow-type: tm+mt
source-wordcount: '1710'
ht-degree: 16%

---


# [!UICONTROL Experience Cloud Audiences] 接続

>[!AVAILABILITY]
>
> この宛先は、[Adobe Real-Time Customer Data Platform PrimeおよびUltimate](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform.html) のお客様が利用できます。

この宛先を使用して、Real-Time CDPからAudience ManagerおよびAdobe Analyticsにオーディエンスをアクティブ化します。

オーディエンスをAdobe Analyticsに送信するには、Audience Manager ライセンスが必要です。 詳しくは、[Audience Analyticsの概要 ](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html?lang=en) を参照してください。

オーディエンスを他のAdobe ソリューションに送信するには、Real-Time CDPから [Adobe Target](../personalization/adobe-target-connection.md)、[Adobe Advertising](../advertising/adobe-advertising-cloud-connection.md)、[Adobe Campaign](../email-marketing/adobe-campaign.md) および [Marketo Engage](../adobe/marketo-engage.md) への直接接続を使用します。

>[!IMPORTANT]
>
>この宛先は、Real-Time Customer Data Platformから様々なExperience Cloud ソリューションへの [ 従来のオーディエンス共有統合 ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aep-segments-in-aam) に代わるものです。
> 
>[ 従来のオーディエンス共有統合 ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aep-segments-in-aam) を使用して、既にReal-Time CDPからAudience Managerおよび他のExperience Cloud ソリューションにオーディエンスを共有している場合、この宛先を使用する前に、カスタマーケアに連絡して従来の統合を無効にする必要があります。

![ 宛先カタログでハイライト表示されているExperience Cloud オーディエンスの宛先。](../../assets/catalog/adobe/experience-cloud-audiences/experience-cloud-audiences-destination-catalog.png)

## 使用例とメリット {#use-cases}

[!UICONTROL Experience Cloud Audiences] の宛先を使用する方法とタイミングをより深く理解するために、Real-Time CDPのお客様がこの宛先を使用して解決できるサンプルユースケースを以下に示します。

### データ管理プラットフォームのユースケースの有効化 {#dmp-use-cases}

Audience Managerでは、次のような、Data Management Platform のユースケースにReal-Time CDP オーディエンスを使用できます。

* セグメントへの [ サードパーティデータ ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-types-collected.html#third-party-data) の追加
* [ アルゴリズムモデリング ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/algorithmic-models/look-alike-modeling/understanding-models.html);
* Real-Time CDPの宛先カタログでまだサポートされていない cookie ベースの宛先に対するオーディエンスのアクティブ化。

### 書き出されたオーディエンスの詳細な制御 {#segments-control}

Audience Manager以降に書き出すオーディエンスを選択するには、Experience Cloud Audiences 宛先に追加された新しい self-service audience-sharing integration を使用します。  これにより、他のExperience Cloud ソリューションと共有するオーディエンスと、Real-Time CDPでのみ保持するオーディエンスを決定できます。

従来のオーディエンス共有統合では、Audience Manager以降に書き出すオーディエンスを詳細に制御できませんでした。

### Real-Time CDP オーディエンスのAdobe Analyticsとの共有 {#share-audiences-with-analytics}

Experience Cloud Audiences 宛先に送信するオーディエンスは、Adobe Analyticsに自動的に表示されません。

オーディエンスをAdobe Analyticsに送信する前に、[Analytics およびAudience Manager用のExperience Cloud ID サービスを実装する ](https://experienceleague.adobe.com/docs/id-service/using/implementation/setup-aam-analytics.html?lang=en) 必要があります。

>[!IMPORTANT]
>
>Experience Cloud Audiences の宛先を通じてReal-Time CDPからAdobe Analyticsにオーディエンスを送信するには、Audience Manager ライセンスが必要です。

### Real-Time CDP オーディエンスを他のExperience Cloud ソリューションと共有する {#share-segments-with-other-solutions}

Real-Time CDP オーディエンス宛先カードを使用して、オーディエンスを他のExperience Cloud ソリューションと共有できます。

Adobeただし、オーディエンスをこれらのソリューションと共有する場合は、次の専用の宛先カードを使用することを強くお勧めします。

* [Adobe Campaign](../email-marketing/adobe-campaign.md)
* [Adobe Target](../personalization/adobe-target-connection.md)
* [Advertising Cloud](../advertising/adobe-advertising-cloud-connection.md)
* [Marketo](../adobe/marketo-engage.md)

## 前提条件 {#prerequisites}

>[!IMPORTANT]
>
> * 前述の [Data Management Platform のユースケース ](#dmp-use-cases) を有効にするには、Audience Manager ライセンスが必要です。
> * Real-Time CDP オーディエンスをAdobe Analyticsと共有する *には、Audience Manager ライセンスが必要です*。
> * *上記の節* で説明したように、Real-Time CDP オーディエンスを Adobe Advertising Cloud、Adobe Target、Marketo、その他のExperience Cloud ソリューションと共有する場合は、Audience Manager ライセンスは必要ありません [](#share-segments-with-other-solutions)。

### 従来のオーディエンス共有ソリューションを使用している顧客の場合

[ 従来のオーディエンス共有統合 ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aep-segments-in-aam) を通じて、既にReal-Time CDPからAudience Managerおよび他のExperience Cloud ソリューションにオーディエンスを共有している場合、カスタマーケアに連絡して従来の統合を無効にする必要があります。

プロビジョニング解除チケットを解決するまでの所要時間は、6 営業日以内です。 既存の従来の統合を無効にした後、セルフサービスの宛先カードを使用して [ 接続の作成 ](#connect) に進むことができます。

>[!IMPORTANT]
>
>Real-Time CDPから他のソリューションへのオーディエンスの書き出しは、チケットの解決から、宛先カードを介した新しい接続が確立されるまでの間に停止されます。 チケットを閉じた後に宛先カードを介して接続を作成すると、このダウンタイムを最小限に抑えることができます。

## 既知の制限事項と引き出し {#known-limitations}

Experience Cloud Audiences カードを使用する際には、次の既知の制限事項と重要な引き出しに注意してください。

* 現在、組織ごとに 1 つのサンドボックスでExperience Cloud オーディエンスの宛先を設定できます。 別のサンドボックスで 2 つ目の宛先接続を設定しようとすると、エラーが発生します。
* 宛先に接続する際に、[ データフローアラートを有効にする ](../../ui/alerts.md) オプションが表示されます。 UI には表示されますが、**アラートを有効にする」オプションは現在サポートされていません**。
* **オーディエンスのバックフィルのサポート**:Audience Managerまたは他のExperience Cloud ソリューションへの最初の書き出しには、オーディエンスの履歴母集団が含まれます。 [ 従来のオーディエンス共有統合 ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aep-segments-in-aam) のユーザーがこの宛先を設定する場合、バックフィル差は約 6 時間を想定してください。
* [ オーディエンスコンポジション ](../../../segmentation/ui/audience-composition.md) から生じるオーディエンスは、直接サポートされていません。 この宛先に対して複合オーディエンスをアクティブ化するには、複合オーディエンスに基づいて [ セグメントビルダー ](../../../segmentation/ui/segment-builder.md) を使用してオーディエンス定義を作成し、新しく作成したオーディエンスをアクティブ化する必要があります。

### オーディエンスをアクティブ化する際の待ち時間 {#audience-activation-latency}

オーディエンスがReal-Time CDPで最初にアクティブ化されてから、Audience Managerやその他のExperience Cloud ソリューションで使用できるようになるまでの待ち時間は 4 時間です。

オーディエンスがすべてのユースケースでAudience Managerで完全に使用できるようになるまで、最大 24 時間かかる場合があります。 Experience Cloud Audiences のオーディエンスがAudience Manager レポートに表示されるまで、最大 48 時間かかる場合があります。

オーディエンス名などのメタデータは、Experience Cloud Audiences 宛先への書き出しの設定後数分以内にAudience Managerで使用できます。

## サポートされている ID {#supported-identities}

[!UICONTROL Experience Cloud Audiences] の宛先に書き出されたプロファイルは、以下の表で説明されている ID にマッピングされます。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| ECID | Experience Cloud ID | ECID を表す名前空間。 この名前空間は、「Adobe Marketing Cloud ID」、「Adobe Experience Cloud ID」、「Adobe Experience Platform ID」という別名で呼ばれることもあります。詳しくは、[ECID](/help/identity-service/features/ecid.md) に関する次のドキュメントを参照してください。 |
| GAID | GOOGLE ADVERTISING ID | Google Advertising ID （GAID）のプライマリ ID を持つReal-Time CDPに取り込まれたプロファイルは、この宛先に書き出すことができます。 |
| IDFA | Apple の広告主 ID | Apple ID for Advertisers （IDFA）のプライマリ ID を持つReal-Time CDPに取り込まれたプロファイルは、この宛先に書き出すことができます。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | ハッシュ化されたメールアドレスのプライマリ ID を使用してReal-Time CDPに取り込まれたプロファイルは、この宛先に書き出すことができます。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
| ---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
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
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対する認証を行うには、カタログの宛先カード表示で **[!UICONTROL 設定]** を選択し、**[!UICONTROL 宛先に接続]** を選択します。

![Experience Cloud Audiences 宛先の「宛先に接続」オプションの表示 ](../../assets/catalog/adobe/experience-cloud-audiences/experience-cloud-audiences-authenticate-to-destination.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![Experience Cloud Audiences の宛先に接続するための必須の設定とオプションの設定を示す、新しい宛先画面の設定 ](../..//assets/catalog/adobe/experience-cloud-audiences/connect-to-destination.png)

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先にオーディエンスをアクティブ化する手順は、[ ストリーミングオーディエンス書き出し宛先へのプロファイルとオーディエンスのアクティブ化 ](/help/destinations/ui/activate-segment-streaming-destinations.md) を参照してください。 [ マッピングステップ ](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping) は不要で、この宛先で [ スケジュールステップ ](/help/destinations/ui/activate-segment-streaming-destinations.md#scheduling) を使用することはできません。

## データの書き出しを検証する {#exported-data}

データの書き出しが正常に行われたことを検証するには、オーディエンスが目的のExperience Cloud ソリューションに正常に移行したことを確認します。

### Audience Managerでのデータの検証

Real-Time CDP オーディエンスは、Audience Managerで [ シグナル ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aep-segments-as-aam-signals)、[ 特性 ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aep-segments-as-aam-traits) および [ セグメント ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aep-segments-as-aam-segments) として表示されます。 上記のドキュメントリンクに記載されているように、データが表示されているかどうかをAudience Managerで確認できます。

セグメント名は、オーディエンスがReal-Time CDPから送信されてから 15 分後にAudience Managerで入力を開始します。

セグメント母集団は、Real-Time CDPから送信されてから 6 時間以内にAudience Managerに流入し始め、Audience Managerで 24 時間ごとに更新されます。

完全な母集団は、72 時間後にAudience Managerに表示され、オーディエンスがReal-Time CDPで宛先から削除されない限り、母集団は引き続きAudience Managerに送られます。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Real-Time CDP] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

Real-Time CDPのデータガバナンスは、[ データ使用ラベル ](/help/data-governance/labels/reference.md) とマーケティングアクションの両方によって適用されます。
データ使用ラベルはアプリケーションに転送されますが、マーケティングアクションは転送されません。 つまり、オーディエンスがAudience Managerに到着すると、Real-Time CDPのオーディエンスを使用可能な任意の宛先に書き出すことができます。 Audience Managerでは、[ データ書き出しコントロール ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/data-export-controls.html) を使用して、オーディエンスが特定の宛先に書き出されるのをブロックできます。

[!DNL HIPAA] マーケティングアクションでマークされたオーディエンスは、Real-Time CDPからAudience Managerに送信されません。

### Audience Managerの権限管理

Audience Managerのオーディエンスと特性には、[Role-Based Access Controls](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/administration/administration-overview.html) （RBAC）の対象となります。

Real-Time CDPから書き出されたオーディエンスは、Audience Managerの **[!UICONTROL Experience Platform Segments]** という特定のデータソースに割り当てられます。

特定のユーザーのみにオーディエンスへのアクセスを許可するには、[ 役割ベースのアクセス制御 ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/administration/administration-overview.html) を使用して、Real-Time CDP オーディエンスから作成されたオーディエンスおよび特性へのユーザーアクセスを設定します。
