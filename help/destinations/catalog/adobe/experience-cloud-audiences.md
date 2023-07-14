---
title: （ベータ版）Experience CloudAudiences
description: Experience Platformから様々なExperience Platformソリューションにオーディエンスを共有する方法を説明します。
last-substantial-update: 2023-01-25T00:00:00Z
exl-id: 2bdbcda3-2efb-4a4e-9702-4fd9991e9461
source-git-commit: 1288652ca3b18b4adb357b2d8884f408725cb0a2
workflow-type: tm+mt
source-wordcount: '1631'
ht-degree: 20%

---

# （ベータ版） [!UICONTROL Experience Cloudオーディエンス] 接続

この宛先を使用すると、Experience Platformから、Audience Manager、Analytics、Advertising Cloud、Adobe Campaign、Target、Marketoなどの様々なExperience Cloudソリューションに対するオーディエンスを共有できます。

![Experience Cloudオーディエンスの宛先。宛先カタログでハイライト表示されます。](/help/destinations/assets/catalog/adobe/experience-cloud-audiences/experience-cloud-audiences-destination-catalog.png)

>[!IMPORTANT]
>
>* この宛先は、 [従来のオーディエンス共有統合](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html?lang=en#aep-segments-in-aam) Experience Platformから様々なExperience Cloudソリューション
>* この宛先は現在ベータ版です。 ドキュメントと機能は変更される場合があります。

## 使用例とメリット {#use-cases}

をいつどのように使用するかをより深く理解するのに役立ちます。 [!UICONTROL Experience Cloudオーディエンス] の宛先について、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。

### データ管理プラットフォームの使用例の有効化 {#dmp-use-cases}

Audience Managerでは、次のようなExperience Platformオーディエンスを Data Management Platform の使用例に使用できます。

* [サードパーティデータ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-types-collected.html?lang=en#third-party-data)をセグメントに追加する
* [アルゴリズムモデリング](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/algorithmic-models/look-alike-modeling/understanding-models.html?lang=en);
* オーディエンスの宛先カタログでまだサポートされていない Cookie ベースの宛先に対してオーディエンスをExperience Platform化します。

### エクスポートされたオーディエンスの詳細な制御 {#segments-control}

Experience Cloudのオーディエンスの宛先で、新しいセルフサービスオーディエンス共有統合を使用して、Audience Manager以降に書き出すオーディエンスを選択します。 これにより、他のExperience Cloudソリューションと共有するオーディエンスや、Experience Platformのみに保持するオーディエンスを決定できます。

従来のオーディエンス共有統合では、どのオーディエンスをAudience Manager以降に書き出すかを詳細に制御できませんでした。

### 他のExperience PlatformソリューションとExperience Cloudオーディエンスを共有 {#share-segments-with-other-solutions}

オーディエンスをAudience Managerと共有する以外にも、「オーディエンスのExperience Platform」の宛先カードを使用すると、次のような、プロビジョニングした他のExperience Cloudソリューションとオーディエンスを共有できます。

* Adobe Campaign
* Adobe Target
* Advertising Cloud
* Analytics
* Marketo

<!--

Note: briefly talk about when to share audiences to these destinations using the existing destination cards and when to share using the new Experience Cloud Audiences destination. 

-->

## 前提条件 {#prerequisites}

>[!IMPORTANT]
>
> * この宛先は次の場所で使用できます： [Adobe Real-time Customer Data Platform Prime と Ultimate](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform.html) 顧客。
> * を有効にするにはAudience Managerライセンスが必要です [データ管理プラットフォームの使用例](#dmp-use-cases) 詳しくは、上記を参照してください。
> * あなた *不要* Experience PlatformオーディエンスをAdobe Advertising Cloud、Adobe Target、Marketoおよびその他のExperience Cloudソリューション ( [上のセクション](#share-segments-with-other-solutions).

### 従来のオーディエンス共有ソリューションを使用しているお客様向け

既に、 [従来のオーディエンス共有統合](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aep-segments-in-aam)従来の統合を無効にするには、カスタマーケアまたはAdobeのアカウントチームに問い合わせる必要があります。 カスタマーケアおよびAdobeのアカウントチームは、統合を無効にする Jira チケットを提出する必要があります ( テンプレートチケット PLAT-160986を参照 )。

ベータ版のお客様向けのプロビジョニング解除チケットの解決にかかる所要時間は 6 営業日以下です。 既存のレガシー統合を無効にした後、次に進むことができます： [接続の作成](#connect) セルフサービスの宛先カードを使用する。

>[!IMPORTANT]
>
>Experience Platformから他のソリューションへのオーディエンスのエクスポートは、Jira チケットの解決から宛先カードを介した新しい接続が確立されるまでの間に停止されます。 Jira チケットが閉じられ次第、宛先カード経由で接続を作成することで、このダウンタイムを最小限に抑えることができます。

## 既知の制限事項と注意事項 {#known-limitations}

「Experience Cloudオーディエンス」カードのベータリリースにおける既知の制限事項と重要な注意事項を次に示します。

* [データフローの監視](/help/dataflows/ui/monitor-destinations.md) はサポートされていません。
* 宛先に接続する際に、次のオプションが表示されます。 [データフローアラートの有効化](#enable-alerts). UI には表示されますが、 **アラートオプションを有効にするはサポートされていません** ベータ版リリースの
* **バックフィルはサポートされていません**. 最初のAudience Managerへの書き出しや他のExperience Cloudソリューションには、オーディエンスの過去の母集団は含まれません。
* ベータ版リリースでは、 **Audiences の宛先への単一のExperience Cloud接続**&#x200B;を、Experience Platform組織に属するすべてのサンドボックスにまたがって使用できます。

### オーディエンスをアクティブ化する際の遅延 {#audience-activation-latency}

オーディエンスがExperience Platformで最初にアクティブ化されてから、特定の使用例に対してAudience Managerや他のExperience Cloudソリューションでオーディエンスを使用する準備が整うまでに、4 時間の遅延が生じます。

オーディエンスがすべての使用例に対してAudience Managerで完全に利用できるようになるまでに最大 24 時間かかり、Experience CloudのオーディエンスからのオーディエンスがAudience Managerレポートに表示されるまで最大 48 時間かかる場合があります。

オーディエンス名などのメタデータは、「オーディエンスのExperience Cloud」の宛先への書き出しが設定されてから数分以内にAudience Managerで使用できます。

## サポートされている ID {#supported-identities}

に書き出されるプロファイル [!UICONTROL Experience Cloudオーディエンス] 宛先は、次の表で説明する id にマッピングされます。 [ID](/help/identity-service/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| ECID | Experience Cloud ID | ECID を表す名前空間。 この名前空間は、「Adobe Marketing Cloud ID」、「Adobe Experience Cloud ID」、「Adobe Experience Platform ID」という別名で呼ばれることもあります。次のドキュメントを参照してください： [ECID](/help/identity-service/ecid.md) を参照してください。 |
| GAID | Google Advertising ID | Google広告 ID(GAID) のプライマリ ID を持つExperience Platformに取り込まれたプロファイルは、この宛先に書き出すことができます。 |
| IDFA | Apple の広告主 ID | Apple ID for Advertisers(IDFA) のプライマリ ID を持つExperience Platformに取り込まれたプロファイルは、この宛先に書き出すことができます。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | ハッシュ化された電子メールアドレスのプライマリ ID を持つExperience Platformに取り込まれたプロファイルは、この宛先に書き出すことができます。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるすべてのオーディエンスについて説明します。

すべての宛先は、Experience Platformを通じて生成されたオーディエンスのアクティブ化をサポートします [セグメント化サービス](../../../segmentation/home.md).

また、この宛先では、以下の表で説明するオーディエンスのアクティブ化もサポートされます。

| オーディエンスタイプ | 説明 |
---------|----------|
| カスタムアップロード | CSV ファイルからExperience Platformに取り込まれたオーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | 上記の節に示した ID をキーにしたオーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 [ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

>[!IMPORTANT]
> 
>ベータリリースでは、Experience Platform組織に属するすべてのサンドボックスにわたって、Experience Cloudの Audiences の宛先への単一の宛先接続を作成できます。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、「 」を選択します。 **[!UICONTROL 設定]** カタログの宛先カード表示で、「 」を選択します。 **[!UICONTROL 宛先に接続]**.

![Audiences の宛先の「宛先に接続」オプションのExperience Cloud。](/help/destinations/assets/catalog/adobe/experience-cloud-audiences/experience-cloud-audiences-authenticate-to-destination.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![Audiences の宛先に接続するための必須およびオプション設定を示す新しいExperience Cloud画面を設定します。](/help/destinations/assets/catalog/adobe/experience-cloud-audiences/connect-to-destination.png)

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。

<!--

Commenting this part out for the duration of the beta program

### Enable alerts {#enable-alerts}

You can enable alerts to receive notifications on the status of the dataflow to your destination. Select an alert from the list to subscribe to receive notifications on the status of your dataflow. For more information on alerts, see the guide on [subscribing to destinations alerts using the UI](../../ui/alerts.md).
-->

宛先接続への詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。


## この宛先に対するオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

読み取り [ストリーミングオーディエンスの書き出し先に対するプロファイルとオーディエンスのアクティブ化](/help/destinations/ui/activate-segment-streaming-destinations.md) を参照してください。 注意： [マッピング手順](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping) 必須で、いいえ [スケジューリング手順](/help/destinations/ui/activate-segment-streaming-destinations.md#scheduling) は、この宛先で使用できます。

## データの書き出しを検証する {#exported-data}

データの書き出しが正常に行われたことを検証するには、オーディエンスが目的のExperience Cloudソリューションに正常に到達したことを確認します。

### データの検証Audience Manager

Experience Platformのオーディエンスは、次のようにAudience Managerに表示されます。 [シグナル](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html?lang=en#aep-segments-as-aam-signals), [特性](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html?lang=en#aep-segments-as-aam-traits)、および [セグメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html?lang=en#aep-segments-as-aam-segments). 上記のドキュメントリンクで説明したようにデータが表示されたかどうかをAudience Managerで確認できます。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

Experience Platformのデータガバナンスは、 [データ使用ラベル](/help/data-governance/labels/reference.md) およびマーケティングアクション。
データ使用ラベルはアプリケーションに転送されますが、マーケティングアクションは転送されません。 つまり、Audience Managerに到達すると、Experience Platformのオーディエンスを、使用可能な任意の宛先に書き出すことができます。 Audience Managerでは、 [データ書き出しコントロール](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/data-export-controls.html?lang=en) を使用して、オーディエンスが特定の宛先に書き出されるのを防ぎます。

### 権限管理 (Audience Manager)

Audience Managerのオーディエンスと特性は、 [ロールベースのアクセス制御](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/administration/administration-overview.html?lang=ja) (RBAC) を参照してください。

Experience Platformから書き出されたオーディエンスは、Audience Manager「 」の特定のデータソースに割り当てられます。 **[!UICONTROL Experience Platformセグメント]**.

特定のユーザーのみがオーディエンスにアクセスできるようにするには、データソースに属するオーディエンスにアクセス制御を適用します。 これらのオーディエンスおよび特性セグメントから作成されたAudience Managerに対して、新しいアクセス制御権限をExperience Platformで設定する必要があります。
