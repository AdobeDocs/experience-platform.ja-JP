---
title: データストリームの作成と設定
description: クライアントサイドの Web SDK 統合を他のアドビ製品やサードパーティの宛先と接続する方法について説明します。
exl-id: 4924cd0f-5ec6-49ab-9b00-ec7c592397c8
source-git-commit: a99b1c625e3ee30ebb4173b132b7d2d738980831
workflow-type: tm+mt
source-wordcount: '2893'
ht-degree: 43%

---


# データストリームの作成と設定

このドキュメントでは、UI で[データストリーム](./overview.md)を設定する手順を説明します。

## [!UICONTROL データストリーム]ワークスペースへのアクセス

左側のナビゲーションで&#x200B;**[!UICONTROL データストリーム]**&#x200B;を選択することで、データ収集 UI または Experience Platform UI でデータストリームを作成および管理できます。

![データ収集 UI の「データストリーム」タブ](assets/configure/datastreams-tab.png)

「**[!UICONTROL データストリーム]**」タブには、わかりやすい名前、ID および最終更新日を含む、既存のデータストリームのリストが表示されます。[ 詳細の表示とサービスの設定 ](#view-details) を行うには、データストリームの名前を選択します。

特定のデータストリームのその他のオプションを表示するには、「その他」アイコン（**...**）を選択します。 データストリームの [ 基本設定 ](#configure) を更新するには、「**[!UICONTROL 編集]**」を選択します。 データストリームを削除するには、「**[!UICONTROL 削除]**」を選択します。

![既存のデータストリームを編集または削除するためのオプション](assets/configure/edit-datastream.png)

## データストリームの作成 {#create}

データストリームを作成するには、最初に「**[!UICONTROL 新規データストリーム]**」を選択します。

![新しいデータストリームを選択](assets/configure/new-datastream-button.png)

設定手順から始まる、データストリーム作成ワークフローが表示されます。ここから、データストリームの名前およびオプションで説明を指定する必要があります。

Experience Platformで使用するデータストリームを設定し、さらに web SDKも使用する場合、取り込みを予定しているデータを表すために [ イベントベースのエクスペリエンスデータモデル（XDM）スキーマ ](../xdm/classes/experienceevent.md) を選択する必要があります。

![データストリームの基本設定](assets/configure/configure.png)

### ジオロケーションとネットワーク参照の設定 {#geolocation-network-lookup}

ジオロケーションとネットワーク検索設定は、収集する地理的データとネットワークレベルのデータの精度レベルを定義するのに役立ちます。

「**[!UICONTROL ジオロケーションとネットワークルックアップ]**」セクションを展開して、以下に説明する設定を指定します。

![ 位置情報とネットワーク参照設定がハイライト表示されたデータストリーム設定画面 ](assets/configure/geolookup.png)

| 設定 | 説明 |
| --- | --- |
| [!UICONTROL 位置情報の検索] | 訪問者の IP アドレスに基づいて、選択したオプションの位置情報の検索を有効にします。 次のオプションを使用できます。 <ul><li>**Country**:`xdm.placeContext.geo.countryCode` を入力します</li><li>**郵便番号**:`xdm.placeContext.geo.postalCode` を入力します</li><li>**都道府県**:`xdm.placeContext.geo.stateProvince` を入力します</li><li>**DMA**:`xdm.placeContext.geo.dmaID` を入力します</li><li>**City**:`xdm.placeContext.geo.city` を入力します</li><li>**Latitude**: `xdm.placeContext.geo._schema.latitude` を入力します</li><li>**経度**:`xdm.placeContext.geo._schema.longitude` を入力します</li></ul>**[!UICONTROL 市区町村]**、**[!UICONTROL 緯度]**&#x200B;または&#x200B;**[!UICONTROL 経度]**&#x200B;を選択すると、他にどのようなオプションが選択されているかに関係なく、小数第 2 位までの座標が表示されます。これは、都市レベルの精度と見なされます。<br> <br> オプションを選択しないと、位置情報の検索が無効になります。 ジオロケーションは [!UICONTROL IP の不明化 &#x200B;] の前に発生します。つまり、[!UICONTROL IP の不明化 &#x200B;] 設定の影響を受けません。 |
| [!UICONTROL ネットワークの検索] | 訪問者の IP アドレスに基づいて、選択したオプションのネットワーク検索を有効にします。 次のオプションを使用できます。 <ul><li>**携帯電話会社**:`xdm.environment.carrier` を入力します</li><li>**Domain**:`xdm.environment.domain` を入力します</li><li>**ISP**:`xdm.environment.ISP` を入力する</li><li>**接続タイプ**:`xdm.environment.connectionType` を入力します</li></ul> |

上記のフィールドのいずれかをデータ収集に対して有効にする場合は、web SDKを設定する際に [`context`](/help/web-sdk/commands/configure/context.md) 配列プロパティが正しく設定されていることを確認してください。

ジオロケーションルックアップフィールドでは `context` の配列文字列 `"placeContext"` を使用し、ネットワークルックアップフィールドでは `context` の配列文字列 `"environment"` を使用します。

また、目的の各 XDM フィールドがスキーマに存在することを確認します。 そうでない場合は、Adobeが提供する `Environment Details` フィールドグループをスキーマに追加できます。

### デバイス検索の設定 {#geolocation-device-lookup}

**[!UICONTROL デバイス検索]** 設定では、収集するデバイス固有の情報を選択できます。

「**[!UICONTROL デバイス検索]**」セクションを展開して、以下に説明する設定を指定します。

![ デバイス参照設定が強調表示されたデータストリーム設定画面 ](assets/configure/device-lookup.png)

>[!IMPORTANT]
>
>次の表に示す設定は、相互に排他的です。 ユーザーエージェント情報 *と* の両方のデバイス参照データを同時に選択することはできません。

| 設定 | 説明 |
| --- | --- |
| **[!UICONTROL ユーザーエージェントヘッダーとクライアントヒントヘッダーを保持]** | ユーザーエージェント文字列に格納された情報のみを収集する場合は、このオプションを選択します。 この設定は、デフォルトで選択されています。 `xdm.environment.browserDetails.userAgent` を入力 |
| **[!UICONTROL デバイス検索を使用して、次の情報を収集します]** | 次のデバイス固有の情報を 1 つ以上収集する場合は、このオプションを選択します。 <ul><li>**[!UICONTROL デバイス]** 情報：<ul><li>**デバイス製造元**:`xdm.device.manufacturer` を入力します</li><li>**デバイスモデル**:`xdm.device.modelNumber` を入力します</li><li>**マーケティング名**:`xdm.device.model` を入力します</li></ul></li><li>**[!UICONTROL ハードウェア]** 情報： <ul><li>**ハードウェアタイプ**:`xdm.device.type` を入力します</li><li>**ディスプレイの高さ**:`xdm.device.screenHeight` を入力します</li><li>**ディスプレイの幅**:`xdm.device.screenWidth` を入力します</li><li>**表示色の深度**:`xdm.device.colorDepth` を入力します</li></ul></li><li>**[!UICONTROL ブラウザー]** 情報： <ul><li>**ブラウザーベンダー**:`xdm.environment.browserDetails.vendor` を入力します</li><li>**ブラウザー名**:`xdm.environment.browserDetails.name` を入力します</li><li>**ブラウザーバージョン**:`xdm.environment.browserDetails.version` を入力します</li></ul></li><li>**[!UICONTROL オペレーティング システム]** 情報： <ul><li>**OS ベンダー**:`xdm.environment.operatingSystemVendor` を入力します</li><li>**OS 名**:`xdm.environment.operatingSystem` を入力します</li><li>**OS バージョン**:`xdm.environment.operatingSystemVersion` を入力します</li></ul></li></ul>デバイス参照情報は、ユーザーエージェントおよびクライアントヒントと共に収集できません。 デバイス情報を収集するように選択すると、ユーザーエージェントとクライアントヒントの収集が無効になり、逆も同様です。 |
| **[!UICONTROL デバイス情報を収集しない]** | デバイスのルックアップ情報を収集しない場合は、このオプションを選択します。 デバイス、ハードウェア、ブラウザー、オペレーティングシステム、ユーザーエージェント、クライアントヒントのデータは収集されません。 |

上記のフィールドのいずれかをデータ収集に対して有効にする場合は、web SDKを設定する際に [`context`](/help/web-sdk/commands/configure/context.md) 配列プロパティが正しく設定されていることを確認してください。

デバイスとハードウェアの情報では `context` 配列の文字列 `"device"` が使用され、ブラウザーとオペレーティングシステムの情報では `context` 配列の文字列 `"environment"` が使用されます。

また、目的の各 XDM フィールドがスキーマに存在することを確認します。 そうでない場合は、Adobeが提供する `Environment Details` フィールドグループをスキーマに追加できます。

### 詳細オプションの設定 {#advanced-options}

詳細設定オプションを表示するには、「**[!UICONTROL 詳細設定オプション]**」を選択します。 ここでは、IP の不明化、ファーストパーティ ID Cookie など、追加のデータストリーム設定を設定できます。

![詳細設定オプション](assets/configure/advanced-settings.png)

>[!IMPORTANT]
>
> お客様は、正確な位置情報を含む個人データの収集、処理、送信に関して、適用される法律および規制に基づいて必要なすべての必要な権限、同意、許可および承認を確実に取得する責任を負います。
> 
> IP アドレスの不明化の選択は、IP アドレスから派生して設定済みのAdobe ソリューションに送信される位置情報のレベルには影響しません。 位置情報の参照は、個別に制限するか無効にする必要があります。

| 設定 | 説明 |
| --- | --- |
| [!UICONTROL IP の不明化] | データストリームに適用される IP 不明化のタイプを示します。顧客 IP に基づく処理は、IP の不明化の設定の影響を受けます。 これには、データストリームからデータを受け取るすべてのExperience Cloud サービスが含まれます。 イベントがデータ準備などのダウンストリームサービスに送信される前に IP の不明化が発生します。 <p>選択可能なオプションは次のとおりです。</p> <ul><li>**[!UICONTROL なし]**：IP の不明化を無効にします。完全なユーザー IP アドレスは、データストリームを介して送信されます。</li><li>**[!UICONTROL 部分的]**：IPv4 アドレスの場合、ユーザー IP アドレスの最後のオクテットを不明化します。IPv6 アドレスの場合、アドレスの最後の 80 ビットを不明化します。 <p>例：</p> <ul><li>IPv4：`1.2.3.4` -> `1.2.3.0`</li><li>IPv6：`2001:0db8:1345:fd27:0000:ff00:0042:8329` -> `2001:0db8:1345:0000:0000:0000:0000:0000`</li></ul></li><li>**[!UICONTROL 完全]**：IP アドレス全体を不明化します。 <p>例：</p> <ul><li>IPv4：`1.2.3.4` -> `0.0.0.0`</li><li>IPv6：`2001:0db8:1345:fd27:0000:ff00:0042:8329` -> `0:0:0:0:0:0:0:0`</li></ul></li></ul> 他のアドビ製品に対する IP の不明化の影響は次のとおりです。 <ul><li>**Adobe Target**: Adobe Targetで [!UICONTROL IP の不明化 &#x200B;] が実行される前に、データストリームレベルの [!UICONTROL IP の不明化 &#x200B;] がリクエストに存在するすべての IP アドレスに適用されます。 例えば、データストリームレベルの [!UICONTROL IP の不明化 &#x200B;] オプションが **[!UICONTROL フル]** に設定され、Adobe Targetの IP の不明化オプションが **[!UICONTROL 最後のオクテットの不明化]** に設定されている場合、Adobe Targetは、完全に不明化された IP を受け取ります。 データストリームレベルの [!UICONTROL IP の不明化 &#x200B;] オプションが **[!UICONTROL 部分的]** に設定され、Adobe Targetの IP の不明化オプションが **[!UICONTROL 完全]** に設定されている場合、Adobe Targetは部分的に不明化された IP を受け取り、完全な不明化を適用します。 Adobe Targetの IP の不明化は、データストリームの IP の不明化とは独立して管理されます。 詳しくは、[IP の不明化](https://experienceleague.adobe.com/docs/target-dev/developer/implementation/privacy/privacy.html)および[位置情報](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html)に関する Adobe Target ドキュメントを参照してください。</li><li>**Audience Manager**: Audience Managerで [!UICONTROL IP の不明化 &#x200B;] が実行される前に、データストリームレベルの [!UICONTROL IP の不明化 &#x200B;] 設定がリクエストに存在するすべての IP アドレスに適用されます。 Audience Manager で実行される位置情報検索は、データストリームレベルの [!UICONTROL IP 不明化]オプションの影響を受けます。完全に不明化された IP に基づくAudience Managerでのジオロケーションルックアップの結果、リージョンが不明になり、結果として得られたジオロケーションデータに基づくセグメントが実現されません。 詳しくは、[IP の不明化](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/administration/ip-obfuscation.html)に関する Audience Manager のドキュメントを参照してください。</li><li>**Adobe Analytics**: データストリームレベルの IP の不明化の設定が **[!UICONTROL フル]** に設定されている場合、Adobe Analyticsは IP アドレスを空白として扱います。 これは、位置情報の検索や IP フィルタリングなど、IP アドレスに依存する Analytics の処理に影響します。 Analytics で、不明化されていない IP アドレスや部分的に不明化された IP アドレスを受信するには、IP の不明化設定を **[!UICONTROL 部分的]** または **[!UICONTROL なし]** に設定します。 IP アドレスが部分的に不明化された場合や不明化されていない場合は、Analytics 内でさらに不明化することができます。 Analytics で IP の不明化を有効にする方法について詳しくは、Adobe Analytics[ ドキュメント ](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/report-suite-general/general-acct-settings-admin.html?lang=ja) を参照してください。 IP アドレスが完全に不明化され、ページヒットに [!DNL ECID] も [!DNL VisitorID] もない場合、Analytics は、IP アドレスに部分的に基づく [ フォールバック ID](https://experienceleague.adobe.com/docs/id-service/using/reference/analytics-reference/analytics-ids.html?lang=en) を生成するのではなく、ヒットをドロップします。</li></ul> |
| [!UICONTROL ファーストパーティ ID Cookie] | この設定を有効にすると、Edge Network は[ファーストパーティデバイス ID](../web-sdk/identity/first-party-device-ids.md) を参照する際に、この値を ID Map で参照するのではなく、指定された Cookie を参照するように指示します。<br><br> この設定を有効にする場合、ID を保存する Cookie の名前を指定する必要があります。 |
| [!UICONTROL サードパーティ ID 同期] | ID 同期は、コンテナにグループ化して、異なる ID 同期を異なる時間に実行できます。この設定を有効にすると、どの ID 同期のコンテナがこのデータストリームに対して実行されるかを指定できます。 |
| [!UICONTROL サードパーティ ID 同期のコンテナ ID] | サードパーティ ID 同期に使用されるコンテナの数値 ID。 |
| [!UICONTROL コンテナ ID の上書き] | このセクションでは、追加のサードパーティ ID 同期コンテナ ID を定義し、これを使用してデフォルトの ID を上書きできます。 |
| [!UICONTROL アクセスタイプ] | Edge Network がデータストリームに受け入れる認証タイプを定義します。 <ul><li>**[!UICONTROL 混合認証]**：このオプションを選択すると、Edge Network は認証済みリクエストと未認証リクエストの両方を受け入れます。[Edge Network API](https://developer.adobe.com/client-sdks/home/) と共に Web SDKまたは [ モバイルSDK](https://developer.adobe.com/data-collection-apis/docs/api/) を使用する予定がある場合は、このオプションを選択します。 </li><li>**[!UICONTROL 認証済みのみ]**：このオプションを選択すると、Edge Network は認証済みのリクエストのみを受け入れます。Edge Network API のみを使用する予定で、未認証のリクエストがEdge Networkで処理されないようにする場合は、このオプションを選択します。</li></ul> |
| [!UICONTROL Media Analytics] | Experience Platform SDK または [Media Edge API](https://developer.adobe.com/cja-apis/docs/endpoints/media-edge/getting-started/) を使用して、Edge Network統合のストリーミングトラッキングデータの処理を有効にします。 [ ドキュメント ](https://experienceleague.adobe.com/docs/media-analytics/using/media-overview.html?lang=ja) から Media Analytics について説明します。 |

ここから、Experience Platformのデータストリームを設定している場合は、[ データ収集のためのデータ準備 ](./data-prep.md) に関するチュートリアルに従って、Experience Platform イベントスキーマにデータをマッピングしてから、このガイドに戻ってください。 それ以外の場合は、「**[!UICONTROL 保存]**」を選択して、次の節を続行します。

## データストリームの詳細の表示 {#view-details}

新しいデータストリームを設定したり、表示するために既存のデータストリームを選択したりすると、そのデータストリームの詳細ページが表示されます。ここでは、データストリームの詳細情報（ID など）を確認できます。

![ データストリームの詳細ページ。](assets/configure/view-details.png)

データストリームの詳細画面から、[サービスを追加](#add-services)して、アクセス権のある Adobe Experience Cloud 製品の機能を有効にできます。また、データストリームの[基本設定](#create)を編集したり、その[マッピングルール](./data-prep.md)を更新したり、[データストリームをコピー](#copy)したり、完全に削除したりできます。

## データストリームへのサービスの追加 {#add-services}

データストリームの詳細ページで、「**[!UICONTROL サービスを追加]**」を選択して、そのデータストリームで使用可能なサービスの追加を開始します。

![ 「サービスを追加」を選択して続行 ](assets/configure/add-service.png)。

次の画面で、ドロップダウンメニューを使用して、このデータストリームで設定するサービスを選択します。アクセス権のあるサービスのみが、このリストに表示されます。

![ リストからサービスを選択します。](assets/configure/service-selection.png)

目的のサービスを選択し、表示される設定オプションに入力してから、「**[!UICONTROL 保存]**」を選択してデータストリームにサービスを追加します。データストリームの詳細表示に、追加されたすべてのサービスが表示されます。

![データストリームに追加されたサービス](assets/configure/services-added.png)

次の項では、各サービスの設定オプションを説明します。

>[!NOTE]
>
>各サービス設定には、サービスが選択されると自動的にアクティブ化される&#x200B;**[!UICONTROL 有効]**&#x200B;切替スイッチが含まれます。このデータストリーム用に選択されたサービスを無効にするには、もう一度&#x200B;**[!UICONTROL 有効]**&#x200B;切替スイッチを選択します。

### Adobe Analytics 設定 {#analytics}

このサービスは、Adobe Analytics にデータを送信するかどうかと、どのように送信するかを制御します。[Adobe Analyticsへのデータの送信 ](/help/web-sdk/use-cases/adobe-analytics.md) を参照してください。

![Adobe Analytics データストリーム設定。](assets/configure/analytics-config.png)

| 設定 | 説明 |
| --- | --- |
| [!UICONTROL レポートスイート ID] | **（必須）**&#x200B;データの送信先の Analytics レポートスイートの ID。この ID は、Adobe Analytics UI の[!UICONTROL 管理者]／[!UICONTROL レポートスイート]にあります。複数のレポートスイートが指定された場合、データは各レポートスイートにコピーされます。 |
| [!UICONTROL &#x200B; 訪問者 ID 名前空間 &#x200B;] | （任意）Adobe Analytics [visitorID](https://experienceleague.adobe.com/docs/analytics/implementation/vars/config-vars/visitorid.html?lang=ja) に使用する名前空間。 この名前空間に指定された値でイベントを送信すると、Analytics で `visitorID` として自動的に使用されます。 |
| [!UICONTROL レポートスイートの上書き] | このセクションでは、デフォルトのレポートスイート ID を上書きするために使用できるレポートスイート ID を追加できます。 |

### Adobe Audience Manager 設定 {#audience-manager}

このサービスは、Adobe Audience Manager にデータを送信するかどうかと、どのように送信するかを制御します。Audience Manager にデータを送信するために必要なのは、このセクションを有効にすることだけです。その他の設定は、オプションですが推奨されます。

![Adobe Audience Manage のデータストリーム設定。](assets/configure/audience-manager-config.png)

| 設定 | 説明 |
| --- | --- |
| [!UICONTROL Cookie 宛先が有効] | SDK で、[!DNL Audience Manager] から [Cookie 宛先](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/destinations/custom-destinations/create-cookie-destination.html?lang=ja)経由でセグメント情報を共有できるようにします。 |
| [!UICONTROL URL 宛先が有効] | SDK で、[!DNL Audience Manager] から [URL 宛先](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/destinations/custom-destinations/create-url-destination.html?lang=ja)経由でセグメント情報を共有できるようにします。 |

### Adobe Experience Platform 設定 {#aep}

>[!IMPORTANT]
>
>Experience Platformのデータストリームを有効にする場合、UI の上部リボンに表示されている、現在使用しているExperience Platform サンドボックスに注意してください。
>
>![選択されたサンドボックス](assets/configure/platform-sandbox.png)
>
>サンドボックスは、Adobe Experience Platform の仮想パーティションで、組織内の他のユーザーからデータおよび実装を分離できます。一旦データストリームが作成されると、そのサンドボックスは変更できません。Experience Platform のサンドボックスの役割について詳しくは、[サンドボックスのドキュメント](../sandboxes/home.md)を参照してください。

このサービスは、Adobe Experience Platform にデータを送信するかどうかと、どのように送信するかを制御します。

![Adobe Experience Platform データストリーム設定。](assets/configure/platform-config.png)

| 設定 | 説明 |
|---| --- |
| [!UICONTROL イベントデータセット] | **（必須）** 顧客イベントデータのストリーミング先となるExperience Platform データセットを選択します。 このスキーマは、[XDM ExperienceEvent クラス](../xdm/classes/experienceevent.md)を使用する必要があります。データセットを追加するには、**[!UICONTROL イベントデータセットを追加]**&#x200B;を選択します。 |
| [!UICONTROL プロファイルデータセット] | **同意**、**プッシュトークン** および **ユーザーアクティビティ地域** 顧客属性を送信するために使用するExperience Platform データセットを選択します。 このスキーマは、[XDM Individual Profile クラス](../xdm/classes/individual-profile.md)を使用する必要があります。 |
| [!UICONTROL Offer Decisioning] | Web SDK実装用のOffer Decisioningを有効にします。 実装について詳しくは、[Web SDKでのOffer Decisioningの使用 ](../web-sdk/personalization/offer-decisioning/offer-decisioning-overview.md) に関するガイドを参照してください。<br><br>Offer Decisioning 機能について詳しくは、[Adobe Journey Optimizer のドキュメント](https://experienceleague.adobe.com/docs/journey-optimizer/using/offer-decisioning/get-started-decision/starting-offer-decisioning.html?lang=ja)を参照してください。 |
| [!UICONTROL エッジのセグメント化] | このデータストリームに対して [ エッジのセグメント化 ](../segmentation/methods/edge-segmentation.md) を有効にします。 [Web SDK](../web-sdk/home.md) または [Edge Network API](https://developer.adobe.com/data-collection-apis/docs/api/) がエッジセグメント化を有効にしてデータストリームを介してデータを送信すると、該当するプロファイルの更新されたオーディエンスメンバーシップが応答で返されます。<br><br> このオプションを **Personalizationの宛先** と組み合わせて使用すると、[ エッジの宛先 ](../destinations/ui/activate-edge-personalization-destinations.md)、[Offer Decisioning](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/decisioning/offer-decisioning/get-started-decision/starting-offer-decisioning)、[Adobe Target](https://experienceleague.adobe.com/en/docs/target)、[Adobe Journey Optimizerなど、同じページおよび次のページのパーソナライゼーションのユースケースを実現できま ](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/ajo-home)。 |
| [!UICONTROL パーソナライゼーションの宛先] | このデータストリームに対して [ カスタム Personalization](../destinations/catalog/personalization/custom-personalization.md) を有効にします。 [Web SDK](../web-sdk/home.md) または [Edge Network API](https://developer.adobe.com/data-collection-apis/docs/api/) が、パーソナライゼーションの宛先を有効にしたデータストリームを介してデータを送信すると、オーディエンスのメンバーシップおよび該当するプロファイルのマッピングされたプロファイル属性（認証済みの [Edge Network API](https://developer.adobe.com/data-collection-apis/docs/api/) リクエストの場合のみ）が応答で返されます。 |
| [!UICONTROL Adobe Journey Optimizer] | このデータストリームに対して [0&rbrace;Adobe Journey Optimizer&rbrace; を有効にします。](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/ajo-home)<br><br> このオプションを有効にすると、データストリームはAdobe Journey Optimizerの web およびアプリベースのインバウンドキャンペーンからパーソナライズされたコンテンツを返すことができるようになります。<br><br> このオプションを使用するには、選択したデータセットで、**[!UICONTROL エクスペリエンスイベント – 提案インタラクション]**&#x200B;[ フィールドグループ ](../xdm/ui/resources/schemas.md#add-field-groups) を含むスキーマを使用する必要があります。 このフィールドグループは、Adobe Journey Optimizerのキャンペーンおよびエクスペリエンスに対するすべてのユーザーインタラクションを記録するために使用されます。 |

### Adobe Target 設定 {#target}

このサービスは、Adobe Target にデータを送信するかどうかと、どのように送信するかを制御します。

![Adobe Target データストリーム設定。](assets/configure/target-config.png)

| 設定 | 説明 |
| --- | --- |
| [!UICONTROL プロパティトークン] | [!DNL Target] を使用すると、お客様はプロパティを使用して権限を制御できます。 プロパティについて詳しくは、[!DNL Target] ドキュメントの[エンタープライズ権限の設定](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html?lang=ja)に関するガイドを参照してください。<br><br>プロパティトークンは、Adobe Target UI の[!UICONTROL 設定]／[!UICONTROL プロパティ]にあります。 |
| [!UICONTROL Target 環境 ID] | [Adobe Target の環境](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html?lang=ja)を使用すると、開発のすべてのステージを通じて実装を管理できます。この設定は、このデータストリームで使用しようとしている環境を指定します。<br><br>ベストプラクティスは、`dev`、`stage`、`prod` の各データストリーム環境ごとに異なる設定を行って、物事をシンプルに保つことです。ただし、既に Adobe Target 環境を定義している場合は、それを使用できます。 |
| [!UICONTROL Target サードパーティ ID 名前空間] | このデータストリームに使用する `mbox3rdPartyId` の ID 名前空間。Adobe Targetとの [!DNL Customer Attributes] 統合を使用する場合、または `thirdPartyId` を使用して [Adobe Target プロファイル API](https://experienceleague.adobe.com/en/docs/target-dev/developer/api/profile-apis/profiles-api) を介してプロファイルを更新または作成する場合は、選択した名前空間値を指定する必要があります。 顧客属性ファイルのアップロードまたはプロファイル更新 API 呼び出しで使用される `IdentityMap` または `customerID` を送信するには、XDM スキーマの `thirdPartyId` セクションでこの名前空間を使用する必要があります。  詳しくは、[Web SDK を使用した `mbox3rdPartyId` の実装](../web-sdk/personalization/adobe-target/using-mbox-3rdpartyid.md)に関するガイドを参照してください。 |
| [!UICONTROL プロパティトークンの上書き] | このセクションでは、デフォルトのプロパティトークンを上書きするために使用できる、追加のプロパティトークンを定義できます。 |

### [!UICONTROL イベント転送]設定

このサービスは、[イベント転送](../tags/ui/event-forwarding/overview.md)にデータを送信するかどうかと、どのように送信するかを制御します。

![ データストリーム設定画面の「イベント転送」セクション。](assets/configure/event-forwarding-config.png)

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

![ データストリームリスト表示から「コピー」オプションが選択されていることを示す画像 ](assets/configure/copy-datastream-list.png)

または、指定されたデータストリームの詳細表示から「**[!UICONTROL データストリームをコピー]**」を選択することもできます。

![ データストリームの詳細表示から「コピー」オプションが選択されている。](assets/configure/copy-datastream-details.png)

作成する新しいデータストリームの一意の名前を指定するよう促す確認ダイアログが表示され、上書きされる設定オプションに関する詳細が表示されます。準備ができたら、「**[!UICONTROL コピー]**」を選択します。

![ データストリームのコピーに関する確認ダイアログ。](assets/configure/copy-datastream-confirm.png)

[!UICONTROL データストリーム]ワークスペースのメインページが再表示され、新しいデータストリームがリスト表示されます。

## 次の手順

このガイドでは、データ収集 UI でのデータストリームの管理方法について説明しました。データストリーム設定後の Web SDK のインストールおよび設定方法について詳しくは、[データ収集 E2E ガイド](../collection/e2e.md#install)を参照してください。
