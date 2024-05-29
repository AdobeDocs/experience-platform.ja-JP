---
title: データストリームの作成と設定
description: クライアントサイドの Web SDK 統合を他のアドビ製品やサードパーティの宛先と接続する方法について説明します。
exl-id: 4924cd0f-5ec6-49ab-9b00-ec7c592397c8
source-git-commit: 370ab0b2a575cc2b5d17f3ae2b3b0b6a6999c462
workflow-type: tm+mt
source-wordcount: '2738'
ht-degree: 54%

---


# データストリームの作成と設定

このドキュメントでは、UI で[データストリーム](./overview.md)を設定する手順を説明します。

## [!UICONTROL データストリーム]ワークスペースへのアクセス

左側のナビゲーションで&#x200B;**[!UICONTROL データストリーム]**&#x200B;を選択することで、データ収集 UI または Experience Platform UI でデータストリームを作成および管理できます。

![データ収集 UI の「データストリーム」タブ](assets/configure/datastreams-tab.png)

「**[!UICONTROL データストリーム]**」タブには、わかりやすい名前、ID および最終更新日を含む、既存のデータストリームのリストが表示されます。終了 [詳細の表示とサービスの設定](#view-details)を選択し、データストリームの名前を選択します。

特定のデータストリームのその他のオプションを表示するには、「その他」アイコン（**...**）に設定します。 を更新するには [基本設定](#configure) データストリームとして、を選択します。 **[!UICONTROL 編集]**. データストリームを削除するには、以下を選択します。 **[!UICONTROL 削除]**.

![既存のデータストリームを編集または削除するためのオプション](assets/configure/edit-datastream.png)

## データストリームの作成 {#create}

データストリームを作成するには、最初に「**[!UICONTROL 新規データストリーム]**」を選択します。

![新しいデータストリームを選択](assets/configure/new-datastream-button.png)

設定手順から始まる、データストリーム作成ワークフローが表示されます。ここから、データストリームの名前およびオプションで説明を指定する必要があります。

Experience Platformで使用するデータストリームを設定し、さらに Web SDK を使用する場合、 [イベントベースのエクスペリエンスデータモデル（XDM）スキーマ](../xdm/classes/experienceevent.md) 取り込む予定のデータを表す。

![データストリームの基本設定](assets/configure/configure.png)

### ジオロケーションとネットワーク参照の設定 {#geolocation-network-lookup}

ジオロケーションとネットワーク検索設定は、収集する地理的データとネットワークレベルのデータの精度レベルを定義するのに役立ちます。

を展開します。 **[!UICONTROL ジオロケーションとネットワーク参照]** セクション：以下に説明する設定を指定します。

![ジオロケーションとネットワーク参照設定がハイライト表示されたデータストリーム設定画面。](assets/configure/geolookup.png)

| 設定 | 説明 |
| --- | --- |
| [!UICONTROL 位置情報の検索] | 訪問者の IP アドレスに基づいて、選択したオプションの位置情報の検索を有効にします。 次のオプションを使用できます。 <ul><li>**国**：値を入力します `xdm.placeContext.geo.countryCode`</li><li>**郵便番号**：値を入力します `xdm.placeContext.geo.postalCode`</li><li>**都道府県**：値を入力します `xdm.placeContext.geo.stateProvince`</li><li>**DMA**：値を入力します `xdm.placeContext.geo.dmaID`</li><li>**市区町村**：値を入力します `xdm.placeContext.geo.city`</li><li>**緯度**：値を入力します `xdm.placeContext.geo._schema.latitude`</li><li>**経度**：値を入力します `xdm.placeContext.geo._schema.longitude`</li></ul>**[!UICONTROL 市区町村]**、**[!UICONTROL 緯度]**&#x200B;または&#x200B;**[!UICONTROL 経度]**&#x200B;を選択すると、他にどのようなオプションが選択されているかに関係なく、小数第 2 位までの座標が表示されます。これは、都市レベルの精度と見なされます。<br> <br>どのオプションも選択しないと、位置情報の検索が無効になります。 位置情報は次の前に発生 [!UICONTROL IP の不明化]。これは、の影響を受けません [!UICONTROL IP の不明化] の設定値。 |
| [!UICONTROL ネットワークの検索] | 訪問者の IP アドレスに基づいて、選択したオプションのネットワーク検索を有効にします。 次のオプションを使用できます。 <ul><li>**通信事業者**：値を入力します `xdm.environment.carrier`</li><li>**ドメイン**：値を入力します `xdm.environment.domain`</li><li>**ISP**：値を入力します `xdm.environment.ISP`</li></ul> |

上記のフィールドのいずれかをデータ収集に対して有効にする場合は、正しく設定していることを確認してください [`context`](/help/web-sdk/commands/configure/context.md) web SDK の設定時の配列プロパティ。

ジオロケーション参照フィールドでは、 `context` 配列文字列 `"placeContext"`を選択します。ネットワーク検索フィールドでは、 `context` 配列文字列 `"environment"`.

また、目的の各 XDM フィールドがスキーマに存在することを確認します。 追加されていない場合は、Adobe提供のを追加できます `Environment Details` フィールドグループをスキーマに追加します。

### デバイス検索の設定 {#geolocation-device-lookup}

この **[!UICONTROL デバイス検索]** 設定を使用すると、収集するデバイス固有の情報を選択できます。

を展開します。 **[!UICONTROL デバイス検索]** セクション：以下に説明する設定を指定します。

![デバイス検索設定が強調表示されたデータストリーム設定画面。](assets/configure/device-lookup.png)

>[!IMPORTANT]
>
>次の表に示す設定は、相互に排他的です。 両方のユーザーエージェント情報を選択することはできません *および* 同時にデバイスのルックアップデータ。

| 設定 | 説明 |
| --- | --- |
| **[!UICONTROL ユーザーエージェントヘッダーとクライアントヒントヘッダーを保持]** | ユーザーエージェント文字列に格納された情報のみを収集する場合は、このオプションを選択します。 この設定は、デフォルトで選択されています。 入力 `xdm.environment.browserDetails.userAgent` |
| **[!UICONTROL デバイス検索を使用して、次の情報を収集します]** | 次のデバイス固有の情報を 1 つ以上収集する場合は、このオプションを選択します。 <ul><li>**[!UICONTROL デバイス]** 情報：<ul><li>**デバイスの製造元**：値を入力します `xdm.device.manufacturer`</li><li>**デバイスモデル**：値を入力します `xdm.device.modelNumber`</li><li>**マーケティング名**：値を入力します `xdm.device.model`</li></ul></li><li>**[!UICONTROL ハードウェア]** 情報： <ul><li>**ハードウェアタイプ**：値を入力します `xdm.device.type`</li><li>**高さを表示**：値を入力します `xdm.device.screenHeight`</li><li>**表示幅**：値を入力します `xdm.device.screenWidth`</li><li>**表示色の深度**：値を入力します `xdm.device.colorDepth`</li></ul></li><li>**[!UICONTROL ブラウザー]** 情報： <ul><li>**ブラウザーベンダー**：値を入力します `xdm.environment.browserDetails.vendor`</li><li>**ブラウザー名**：値を入力します `xdm.environment.browserDetails.name`</li><li>**ブラウザーバージョン**：値を入力します `xdm.environment.browserDetails.version`</li></ul></li><li>**[!UICONTROL オペレーティングシステム]** 情報： <ul><li>**OS ベンダー**：値を入力します `xdm.environment.operatingSystemVendor`</li><li>**OS 名**：値を入力します `xdm.environment.operatingSystem`</li><li>**OS バージョン**：値を入力します `xdm.environment.operatingSystemVersion`</li></ul></li></ul>デバイス参照情報は、ユーザーエージェントおよびクライアントヒントと共に収集できません。 デバイス情報を収集するように選択すると、ユーザーエージェントとクライアントヒントの収集が無効になり、逆も同様です。 |
| **[!UICONTROL デバイス情報を収集しない]** | デバイスのルックアップ情報を収集しない場合は、このオプションを選択します。 デバイス、ハードウェア、ブラウザー、オペレーティングシステム、ユーザーエージェント、クライアントヒントのデータは収集されません。 |

上記のフィールドのいずれかをデータ収集に対して有効にする場合は、正しく設定していることを確認してください [`context`](/help/web-sdk/commands/configure/context.md) web SDK の設定時の配列プロパティ。

デバイスとハードウェアの情報では、 `context` 配列文字列 `"device"`ブラウザーおよびオペレーティングシステムの情報では `context` 配列文字列 `"environment"`.

また、目的の各 XDM フィールドがスキーマに存在することを確認します。 追加されていない場合は、Adobe提供のを追加できます `Environment Details` フィールドグループをスキーマに追加します。

### 詳細オプションの設定 {#@advanced-options}

詳細設定オプションを表示するには、次を選択します： **[!UICONTROL 詳細オプション]**. ここでは、IP の不明化、ファーストパーティ ID Cookie など、追加のデータストリーム設定を設定できます。

![詳細設定オプション](assets/configure/advanced-settings.png)

>[!IMPORTANT]
>
> お客様は、正確な位置情報を含む個人データの収集、処理、送信に関して、適用される法律および規制に基づいて必要なすべての必要な権限、同意、許可および承認を確実に取得する責任を負います。
> 
> IP アドレスの不明化の選択は、IP アドレスから派生して設定済みのAdobeソリューションに送信される位置情報のレベルには影響しません。 位置情報の参照は、個別に制限するか無効にする必要があります。

| 設定 | 説明 |
| --- | --- |
| [!UICONTROL IP の不明化] | データストリームに適用される IP 不明化のタイプを示します。顧客 IP に基づく処理は、IP の不明化の設定の影響を受けます。 これには、データストリームからデータを受信するすべての Experience Cloud サービスが含まれます。 <p>選択可能なオプションは次のとおりです。</p> <ul><li>**[!UICONTROL なし]**：IP の不明化を無効にします。完全なユーザー IP アドレスは、データストリームを介して送信されます。</li><li>**[!UICONTROL 部分的]**：IPv4 アドレスの場合、ユーザー IP アドレスの最後のオクテットを不明化します。IPv6 アドレスの場合、アドレスの最後の 80 ビットを不明化します。 <p>例：</p> <ul><li>IPv4：`1.2.3.4` -> `1.2.3.0`</li><li>IPv6：`2001:0db8:1345:fd27:0000:ff00:0042:8329` -> `2001:0db8:1345:0000:0000:0000:0000:0000`</li></ul></li><li>**[!UICONTROL 完全]**：IP アドレス全体を不明化します。 <p>例：</p> <ul><li>IPv4：`1.2.3.4` -> `0.0.0.0`</li><li>IPv6：`2001:0db8:1345:fd27:0000:ff00:0042:8329` -> `0:0:0:0:0:0:0:0`</li></ul></li></ul> 他のアドビ製品に対する IP の不明化の影響は次のとおりです。 <ul><li>**Adobe Target**：データストリームレベル [!UICONTROL IP の不明化] 次の前に適用： [!UICONTROL IP の不明化] Adobe Targetで、リクエストに存在するすべての IP アドレスに対して実行されます。 例えば、次の場合：データストリームレベル [!UICONTROL IP の不明化] オプションの設定 **[!UICONTROL フル]** Adobe Targetの IP の不明化オプションがに設定されている場合。 **[!UICONTROL 最後のオクテットの不明化]** Adobe Targetは、完全に不明化された IP を受け取ります。 データストリームレベル [!UICONTROL IP の不明化] オプションの設定 **[!UICONTROL 一部]** Adobe Targetの IP の不明化オプションがに設定されている場合。 **[!UICONTROL フル]** Adobe Targetは、部分的に不明化された IP を受け取り、その完全な不明化を適用します。 Adobe Targetの IP の不明化は、データストリームの IP の不明化とは独立して管理されます。 詳しくは、[IP の不明化](https://experienceleague.adobe.com/docs/target-dev/developer/implementation/privacy/privacy.html)および[位置情報](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html)に関する Adobe Target ドキュメントを参照してください。</li><li>**Audience Manager**：データストリームレベル [!UICONTROL IP の不明化] 設定は、次の前に適用されます [!UICONTROL IP の不明化] リクエストに存在するすべての IP アドレスに対してAudience Managerで実行されます。 Audience Manager で実行される位置情報検索は、データストリームレベルの [!UICONTROL IP 不明化]オプションの影響を受けます。完全に不明化された IP に基づくAudience Managerでのジオロケーション検索は、結果が不明なリージョンになり、結果として得られたジオロケーションデータに基づくセグメントが実現されません。 詳しくは、[IP の不明化](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/administration/ip-obfuscation.html)に関する Audience Manager のドキュメントを参照してください。</li><li>**Adobe Analytics**：データストリームレベルの IP の不明化設定がに設定されている場合 **[!UICONTROL フル]**&#x200B;の場合、Adobe Analyticsはこの IP アドレスを空白として扱います。 これは、位置情報の検索や IP フィルタリングなど、IP アドレスに依存する Analytics の処理に影響します。 Analytics で、不明化されていない IP アドレスや部分的に不明化された IP アドレスを受信するには、IP 不明化設定をに設定します。 **[!UICONTROL 一部]** または **[!UICONTROL なし]**. IP アドレスが部分的に不明化された場合や不明化されていない場合は、Analytics 内でさらに不明化することができます。 Adobe Analyticsを参照 [詳細を見る](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/report-suite-general/general-acct-settings-admin.html?lang=ja) analytics で IP の不明化を有効にする方法について詳しくは、こちらを参照してください。 IP アドレスが完全に不明化され、ページヒットにものがない場合 [!DNL ECID] また、 [!DNL VisitorID]その後、Analytics は、 [フォールバック ID](https://experienceleague.adobe.com/docs/id-service/using/reference/analytics-reference/analytics-ids.html?lang=en)（一部は IP アドレスに基づいています）。</li></ul> |
| [!UICONTROL ファーストパーティ ID Cookie] | この設定を有効にすると、Edge Network は[ファーストパーティデバイス ID](../web-sdk/identity/first-party-device-ids.md) を参照する際に、この値を ID Map で参照するのではなく、指定された Cookie を参照するように指示します。<br><br>この設定を有効にする場合、ID を保存する Cookie の名前を指定する必要があります。 |
| [!UICONTROL サードパーティ ID 同期] | ID 同期は、コンテナにグループ化して、異なる ID 同期を異なる時間に実行できます。この設定を有効にすると、どの ID 同期のコンテナがこのデータストリームに対して実行されるかを指定できます。 |
| [!UICONTROL サードパーティ ID 同期のコンテナ ID] | サードパーティ ID 同期に使用されるコンテナの数値 ID。 |
| [!UICONTROL コンテナ ID の上書き] | このセクションでは、追加のサードパーティ ID 同期コンテナ ID を定義し、これを使用してデフォルトの ID を上書きできます。 |
| [!UICONTROL アクセスタイプ] | Edge Network がデータストリームに受け入れる認証タイプを定義します。 <ul><li>**[!UICONTROL 混合認証]**：このオプションを選択すると、Edge Network は認証済みリクエストと未認証リクエストの両方を受け入れます。[Server API](../server-api/overview.md) と一緒に Web SDK または [Mobile SDK](https://developer.adobe.com/client-sdks/home/) を使用する場合は、このオプションを選択してください。 </li><li>**[!UICONTROL 認証済みのみ]**：このオプションを選択すると、Edge Network は認証済みのリクエストのみを受け入れます。Server API のみを使用する予定で、未認証のリクエストが Edge Network で処理されないようにする場合は、このオプションを選択します。</li></ul> |
| [!UICONTROL Media Analytics] | Experience Platform SDK またはを介してEdge Network統合のためのストリーミングトラッキングデータの処理を有効にする [Media Edge API](https://developer.adobe.com/cja-apis/docs/endpoints/media-edge/getting-started/). からの Media Analytics について説明します [詳細を見る](https://experienceleague.adobe.com/docs/media-analytics/using/media-overview.html?lang=ja). |

ここから、Experience Platform のデータストリームを設定している場合は、[データ収集のためのデータ準備](./data-prep.md)に関するチュートリアルに従って、Platform イベントスキーマにデータをマッピングしてから、このガイドに戻ってください。それ以外の場合は、「**[!UICONTROL 保存]**」を選択して、次の節を続行します。

## データストリームの詳細の表示 {#view-details}

新しいデータストリームを設定したり、表示するために既存のデータストリームを選択したりすると、そのデータストリームの詳細ページが表示されます。ここでは、データストリームの詳細情報（ID など）を確認できます。

![データストリームの詳細ページ](assets/configure/view-details.png)

データストリームの詳細画面から、[サービスを追加](#add-services)して、アクセス権のある Adobe Experience Cloud 製品の機能を有効にできます。また、データストリームの[基本設定](#create)を編集したり、その[マッピングルール](./data-prep.md)を更新したり、[データストリームをコピー](#copy)したり、完全に削除したりできます。

## データストリームへのサービスの追加 {#add-services}

データストリームの詳細ページで、「**[!UICONTROL サービスを追加]**」を選択して、そのデータストリームで使用可能なサービスの追加を開始します。

![「サービスを追加」を選択して続行します。](assets/configure/add-service.png)

次の画面で、ドロップダウンメニューを使用して、このデータストリームで設定するサービスを選択します。アクセス権のあるサービスのみが、このリストに表示されます。

![リストからサービスを選択します。](assets/configure/service-selection.png)

目的のサービスを選択し、表示される設定オプションに入力してから、「**[!UICONTROL 保存]**」を選択してデータストリームにサービスを追加します。データストリームの詳細表示に、追加されたすべてのサービスが表示されます。

![データストリームに追加されたサービス](assets/configure/services-added.png)

次の項では、各サービスの設定オプションを説明します。

>[!NOTE]
>
>各サービス設定には、サービスが選択されると自動的にアクティブ化される「**[!UICONTROL 有効]**」トグルが含まれます。このデータストリーム用に選択されたサービスを無効にするには、もう一度「**[!UICONTROL 有効]**」トグルを選択します。

### Adobe Analytics 設定 {#analytics}

このサービスは、Adobe Analytics にデータを送信するかどうかと、どのように送信するかを制御します。参照： [Adobe Analyticsへのデータの送信](/help/web-sdk/use-cases/adobe-analytics.md).

![Adobe Analytics データストリームの設定。](assets/configure/analytics-config.png)

| 設定 | 説明 |
| --- | --- |
| [!UICONTROL レポートスイート ID] | **（必須）**&#x200B;データの送信先の Analytics レポートスイートの ID。この ID は、Adobe Analytics UI の[!UICONTROL 管理者]／[!UICONTROL レポートスイート]にあります。複数のレポートスイートが指定された場合、データは各レポートスイートにコピーされます。 |
| [!UICONTROL 訪問者 ID 名前空間] | （オプション）Adobe Analyticsに使用する名前空間 [visitorID](https://experienceleague.adobe.com/docs/analytics/implementation/vars/config-vars/visitorid.html?lang=ja). この名前空間に指定された値でイベントを送信すると、自動的にとして使用されます `visitorID` Analytics の場合。 |
| [!UICONTROL レポートスイートの上書き] | このセクションでは、デフォルトのレポートスイート ID を上書きするために使用できるレポートスイート ID を追加できます。 |

### Adobe Audience Manager 設定 {#audience-manager}

このサービスは、Adobe Audience Manager にデータを送信するかどうかと、どのように送信するかを制御します。Audience Manager にデータを送信するために必要なのは、このセクションを有効にすることだけです。その他の設定は、オプションですが推奨されます。

![Adobeオーディエンス管理データストリーム設定。](assets/configure/audience-manager-config.png)

| 設定 | 説明 |
| --- | --- |
| [!UICONTROL Cookie 宛先が有効] | SDK で、[!DNL Audience Manager] から [Cookie 宛先](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/destinations/custom-destinations/create-cookie-destination.html?lang=ja)経由でセグメント情報を共有できるようにします。 |
| [!UICONTROL URL 宛先が有効] | SDK で、[!DNL Audience Manager] から [URL 宛先](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/destinations/custom-destinations/create-url-destination.html?lang=ja)経由でセグメント情報を共有できるようにします。 |

### Adobe Experience Platform 設定 {#aep}

>[!IMPORTANT]
>
>Platform のデータストリームを有効にする場合、UI の上部リボンに表示されている、現在使用中の Platform サンドボックスに注意してください。
>
>![選択されたサンドボックス](assets/configure/platform-sandbox.png)
>
>サンドボックスは、Adobe Experience Platform の仮想パーティションで、組織内の他のユーザーからデータおよび実装を分離できます。一旦データストリームが作成されると、そのサンドボックスは変更できません。Experience Platform のサンドボックスの役割について詳しくは、[サンドボックスのドキュメント](../sandboxes/home.md)を参照してください。

このサービスは、Adobe Experience Platform にデータを送信するかどうかと、どのように送信するかを制御します。

![Adobe Experience Platform データストリームの設定。](assets/configure/platform-config.png)

| 設定 | 説明 |
|---| --- |
| [!UICONTROL イベントデータセット] | **（必須）**&#x200B;顧客イベントデータのストリーミング先となる Platform データセットを選択します。このスキーマは、[XDM ExperienceEvent クラス](../xdm/classes/experienceevent.md)を使用する必要があります。データセットを追加するには、**[!UICONTROL イベントデータセットを追加]**&#x200B;を選択します。 |
| [!UICONTROL プロファイルデータセット] | 顧客属性データの送信先となる Platform データセットを選択します。このスキーマは、[XDM Individual Profile クラス](../xdm/classes/individual-profile.md)を使用する必要があります。 |
| [!UICONTROL Offer Decisioning] | Web SDK 実装のOffer decisioningを有効にします。 のガイドを参照してください [web SDK でのOffer decisioningの使用](../web-sdk/personalization/offer-decisioning/offer-decisioning-overview.md) を参照してください。<br><br>Offer Decisioning 機能について詳しくは、[Adobe Journey Optimizer のドキュメント](https://experienceleague.adobe.com/docs/journey-optimizer/using/offer-decisioning/get-started-decision/starting-offer-decisioning.html?lang=ja)を参照してください。 |
| [!UICONTROL エッジのセグメント化] | 有効 [エッジのセグメント化](../segmentation/ui/edge-segmentation.md) このデータストリーム用。 SDK がエッジセグメント化対応データストリームでデータを送信すると、当該プロファイルの更新されたセグメントメンバーシップが応答で返されます。<br><br>このオプションは、[次のページパーソナライゼーションのユースケース](../destinations/ui/activate-edge-personalization-destinations.md)の[!UICONTROL パーソナライゼーションの宛先]と組み合わせて使用できます。 |
| [!UICONTROL パーソナライゼーションの宛先] | 「[!UICONTROL エッジセグメント化]」チェックボックスを有効にした後でこの項目を有効にすると、[カスタムパーソナライゼーション](../destinations/catalog/personalization/custom-personalization.md)などのパーソナライゼーションの宛先にデータストリームが接続できるようになります。<br><br>[パーソナライゼーションの宛先の設定](../destinations/ui/activate-edge-personalization-destinations.md)に関する特定の手順については、宛先のドキュメントを参照してください。 |
| [!UICONTROL Adobe Journey Optimizer] | 有効 [Adobe Journey Optimizer](https://experienceleague.adobe.com/docs/journey-optimizer/using/ajo-home.html?lang=ja) このデータストリーム用。 <br><br> このオプションを有効にすると、データストリームは [!DNL Adobe Journey Optimizer] の web およびアプリベースのインバウンドキャンペーンからパーソナライズされたコンテンツを返すことができるようになります。このオプションを使用するには、[!UICONTROL エッジセグメント化]をアクティブにする必要があります。次の場合 [!UICONTROL エッジのセグメント化] をオフにすると、このオプションはグレー表示されます。 |

### Adobe Target 設定 {#target}

このサービスは、Adobe Target にデータを送信するかどうかと、どのように送信するかを制御します。

![Adobe Target データストリームの設定。](assets/configure/target-config.png)

| 設定 | 説明 |
| --- | --- |
| [!UICONTROL プロパティトークン] | [!DNL Target] を使用すると、お客様は、プロパティを使用して権限を制御できます。 プロパティについて詳しくは、[!DNL Target] ドキュメントの[エンタープライズ権限の設定](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html?lang=ja)に関するガイドを参照してください。<br><br>プロパティトークンは、Adobe Target UI の[!UICONTROL 設定]／[!UICONTROL プロパティ]にあります。 |
| [!UICONTROL Target 環境 ID] | [Adobe Target の環境](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html?lang=ja)を使用すると、開発のすべてのステージを通じて実装を管理できます。この設定は、このデータストリームで使用しようとしている環境を指定します。<br><br>ベストプラクティスは、`dev`、`stage`、`prod` の各データストリーム環境ごとに異なる設定を行って、物事をシンプルに保つことです。ただし、既に Adobe Target 環境を定義している場合は、それを使用できます。 |
| [!UICONTROL Target サードパーティ ID 名前空間] | このデータストリームに使用する `mbox3rdPartyId` の ID 名前空間。詳しくは、[Web SDK を使用した `mbox3rdPartyId` の実装](../web-sdk/personalization/adobe-target/using-mbox-3rdpartyid.md)に関するガイドを参照してください。 |
| [!UICONTROL プロパティトークンの上書き] | このセクションでは、デフォルトのプロパティトークンを上書きするために使用できる、追加のプロパティトークンを定義できます。 |

### [!UICONTROL イベント転送]設定

このサービスは、[イベント転送](../tags/ui/event-forwarding/overview.md)にデータを送信するかどうかと、どのように送信するかを制御します。

![データストリーム設定画面の「イベント転送」セクション。](assets/configure/event-forwarding-config.png)

| 設定 | 説明 |
| --- | --- |
| [!UICONTROL Launch プロパティ] | **（必須）**&#x200B;データの送信先のイベント転送プロパティ。 |
| [!UICONTROL Launch 環境] | **（必須）**&#x200B;データの送信先の選択されたプロパティ内の環境。 |

>[!NOTE]
>
>「**[!UICONTROL 手動で ID を入力]**」を選択すると、ドロップダウンメニューを使用せずに、プロパティ名および環境名を入力できます。

## データストリームのコピー {#copy}

既存のデータストリームのコピーを作成し、必要に応じて、その詳細を変更できます。

>[!NOTE]
>
>データストリームは、同じ[サンドボックス](../sandboxes/home.md)内でのみコピーできます。つまり、あるサンドボックスから別のサンドボックスにデータストリームをコピーすることはできません。

[!UICONTROL データストリーム]ワークスペースのメインページから、当該データストリームの省略記号（**...**）を選択してから、「**[!UICONTROL コピー]**」を選択します。

![データストリームリスト表示から「コピー」オプションが選択されていることを示す画像。](assets/configure/copy-datastream-list.png)

または、指定されたデータストリームの詳細表示から「**[!UICONTROL データストリームをコピー]**」を選択することもできます。

![データストリームの詳細表示から選択されている「コピー」オプション。](assets/configure/copy-datastream-details.png)

作成する新しいデータストリームの一意の名前を指定するよう促す確認ダイアログが表示され、上書きされる設定オプションに関する詳細が表示されます。準備ができたら、「**[!UICONTROL コピー]**」を選択します。

![データストリームのコピーを確認するダイアログ。](assets/configure/copy-datastream-confirm.png)

[!UICONTROL データストリーム]ワークスペースのメインページが再表示され、新しいデータストリームがリスト表示されます。

## 次の手順

このガイドでは、データ収集 UI でのデータストリームの管理方法について説明しました。データストリーム設定後の Web SDK のインストールおよび設定方法について詳しくは、[データ収集 E2E ガイド](../collection/e2e.md#install)を参照してください。
