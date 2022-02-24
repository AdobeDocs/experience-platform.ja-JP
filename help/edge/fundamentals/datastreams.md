---
title: Experience PlatformWeb SDK 用の Datastream の設定
description: 'データストリームの設定方法を説明します。 '
keywords: 設定；datastreams;datastreamId;edge;datastream id；環境設定；edgeConfigId;ID 同期有効；ID 同期コンテナ ID；サンドボックス；ストリーミングインレット；イベントデータセット；ターゲットコード；クライアントコード；Cookie 宛先；Cookie 宛先；Analytics 設定ブロックスイート ID;
exl-id: 736c75cb-e290-474e-8c47-2a031f215a56
source-git-commit: d326267cacf8d678937e8c959de8acbfbbb88c93
workflow-type: tm+mt
source-wordcount: '1127'
ht-degree: 3%

---


# データストリームの設定

Adobe Experience Platform Web SDK の設定は、2 つの場所に分割されます。 この [設定コマンド](configuring-the-sdk.md) は、クライアントで処理する必要のある処理 ( `edgeDomain`. データストリームは、SDK のその他すべての設定を処理します。 リクエストがAdobe Experience Platform Edge Network に送信されると、 `edgeConfigId` は、サーバー側の設定を参照するために使用されます。 これにより、Web サイト上でコードを変更しなくても設定を更新できます。

この機能を使用するには、組織がプロビジョニングされている必要があります。 カスタマーサクセスマネージャー (CSM) に問い合わせて、を利用してく許可リストださい。

## データストリーム設定の作成

データ収集 UI でデータストリームを作成および管理するには、次を選択します。 **[!UICONTROL データストリーム]** をクリックします。

![datastreams ツールのナビゲーション](../images/datastreams/config.png)

>[!NOTE]
>
>次にアクセスすると、 [!UICONTROL データストリーム] タブは、Platform のタグ管理機能を使用するかどうかに関係なく、データストリーム自体を管理するための開発者権限が必要です。 詳しくは、 [ユーザー権限](../../tags/ui/administration/user-permissions.md) 詳しくは、タグに関するドキュメントの記事を参照してください。

「 **[!UICONTROL 新規データストリーム]** をクリックします。 名前と説明を入力した後、各環境のデフォルト設定を求められます。 次に、利用可能な設定について説明します。

データストリームを作成する場合、同じ設定で 3 つの環境が自動的に作成されます。 次の 3 つの環境があります。 *dev*, *ステージ*、および *prod*. タグの 3 つのデフォルト環境に一致します。 開発環境にタグライブラリを構築すると、ライブラリは設定から開発環境を自動的に使用します。 個々の環境の設定は、必要に応じて編集できます。

SDK で `edgeConfigId` は、設定と環境を指定する複合 ID です ( 例： `1c86778b-cdba-4684-9903-750e52912ad1:stage`) をクリックします。 複合 ID に環境が存在しない場合 ( 例： `stage` 前の例では )、実稼動環境が使用されます。

各設定環境で使用できる設定を次に示します。 ほとんどのセクションは、有効または無効にできます。 無効にした場合、設定は保存されますが、アクティブになりません。

## [!UICONTROL サードパーティ ID] 設定

サードパーティ ID セクションは、常に表示される唯一のセクションです。 次の 2 つの設定を使用できます。&quot;[!UICONTROL サードパーティ ID 同期が有効になっています]&quot;および&quot;[!UICONTROL サードパーティ ID 同期コンテナ ID]&quot;.

![設定 UI の ID セクション](../images/datastreams/edge_configuration_identity.png)

### [!UICONTROL サードパーティ ID 同期が有効になっています]

SDK がサードパーティパートナーとの ID 同期を実行するかどうかを制御します。

### [!UICONTROL サードパーティ ID 同期コンテナ ID]

ID 同期をコンテナにグループ化して、ID 同期を異なる時間に実行できるようにします。 指定した設定 ID に対して実行する ID 同期のコンテナを制御します。

## Adobe Experience Platform 設定

ここに示す設定を使用すると、Adobe Experience Platformにデータを送信できます。 Adobe Experience Platformを購入済みの場合にのみ、このセクションを有効にする必要があります。

![Adobe Experience Platform設定ブロック](../images/datastreams/platform-config.png)

| フィールド | 説明 |
| --- | --- |
| [!UICONTROL サンドボックス] | **（必須）** データの送信先の Platform サンドボックスを選択します。 サンドボックスは、Adobe Experience Platformの仮想パーティションで、データと実装を組織内の他のユーザーから分離できます。<br><br>サンドボックスを選択せずにデータストリームを作成した場合でも、後でサンドボックスを選択できます。<br><br>データストリームを作成し、サンドボックスを選択すると、サンドボックスを変更できなくなります。 この [!UICONTROL サンドボックス] したがって、選択したサンドボックスを使用して既存のデータストリームを編集する場合、「選択」フィールドは使用できません。<br><br> この [!UICONTROL サンドボックス] したがって、既存のデータストリームを編集する際には、「選択」フィールドを使用できません。<br><br>Experience Platformでのサンドボックスの役割について詳しくは、 [サンドボックスドキュメント](../../sandboxes/home.md). |
| [!UICONTROL イベントデータセット] | **（必須）** 顧客イベントデータのストリーミング先の Platform データセットを選択します。 このスキーマでは [XDM ExperienceEvent クラス](../../xdm/classes/experienceevent.md). |
| [!UICONTROL プロファイルデータセット] | 顧客属性データの送信先の Platform データセットを選択します。 このスキーマでは [XDM Individual Profile クラス](../../xdm/classes/individual-profile.md). |
| [!UICONTROL Offer Decisioning] | Platform Web SDK 実装のOffer decisioningを有効にするには、このチェックボックスを選択します。 詳しくは、 [Platform Web SDK でのOffer decisioningの使用](../personalization/offer-decisioning/offer-decisioning-overview.md) を参照してください。 offer decisioning機能について詳しくは、 [Adobe Journey Optimizerドキュメント](https://experienceleague.adobe.com/docs/journey-optimizer/using/offer-decisioniong/get-started/starting-offer-decisioning.html?lang=ja). |
| [!UICONTROL エッジセグメント化] | 有効にするには、このチェックボックスを選択します [エッジセグメント化](../../segmentation/ui/edge-segmentation.md) このデータストリーム用。 Platform Web SDK がエッジセグメント化対応データストリームを介してデータを送信すると、該当するプロファイルの更新済みのセグメントメンバーシップが応答に返されます。<br><br>このオプションは、 [!UICONTROL パーソナライズ機能の宛先] 対象 [次のページのパーソナライゼーションの使用例](../../destinations/ui/configure-personalization-destinations.md). |
| [!UICONTROL パーソナライズ機能の宛先] | を [!UICONTROL エッジセグメント化] 「 」チェックボックスにチェックを入れると、データストリームがAdobe Targetなどのパーソナライゼーションエンジンに接続できるようになります。 の具体的な手順については、宛先のドキュメントを参照してください。 [パーソナライゼーションの宛先の設定](../../destinations/ui/configure-personalization-destinations.md). |

## Adobe Target設定

Adobe Targetを設定するには、クライアントコードを指定する必要があります。 その他のフィールドはオプションです。

![Adobe Target設定ブロック](../images/datastreams/edge_configuration_target.png)

>[!NOTE]
>
>クライアントコードに関連付けられている組織は、設定 ID が作成される組織と一致する必要があります。

### [!UICONTROL クライアントコード]

ターゲットアカウントの一意の ID。 これを見つけるには、 [!UICONTROL Adobe Target] > [!UICONTROL 設定]> [!UICONTROL 実装] > [!UICONTROL 設定を編集] の横 [!UICONTROL ダウンロード] ボタン [!UICONTROL at.js] または [!UICONTROL mbox.js]

### [!UICONTROL プロパティトークン]

[!DNL Target] を使用すると、プロパティを使用して権限を制御できます。 詳しくは、 [Enterprise 権限](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html?lang=ja) セクション [!DNL Target] ドキュメント。

プロパティトークンは、 [!UICONTROL Adobe Target] > [!UICONTROL 設定] > [!UICONTROL プロパティ]

### [!UICONTROL Target 環境 ID]

[環境](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html) Adobe Targetでは、あらゆる開発段階を通じて実装を管理できます。 この設定は、各環境で使用する環境を指定します。

Adobeでは、 `dev`, `stage`、および `prod` データストリーム環境を使用して、操作をシンプルにします。 ただし、既にAdobe Target環境を定義している場合は、それらを使用できます。

## Adobe Audience Manager設定

Adobe Audience Managerにデータを送信するために必要な操作は、このセクションを有効にすることだけです。 その他の設定はオプションですが、推奨されます。

![AdobeAudience Manager 設定ブロック](../images/datastreams/edge_configuration_aam.png)

### [!UICONTROL Cookie の宛先が有効になっています]

SDK が [Cookie の宛先](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/destinations/custom-destinations/create-cookie-destination.html) から [!DNL Audience Manager].

### [!UICONTROL URL の宛先が有効になっています]

SDK が [URL の宛先](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/destinations/custom-destinations/create-url-destination.html). これらは、 [!DNL Audience Manager].

## Adobe Analytics設定

データをAdobe Analyticsに送信するかどうかを制御します。 追加の詳細は、 [Analytics の概要](../data-collection/adobe-analytics/analytics-overview.md).

![Adobe Analytics Settings Block](../images/datastreams/edge_configuration_aa.png)

### [!UICONTROL レポートスイート ID]

レポートスイートは、以下の「 Adobe Analytics Admin 」セクションにあります。 [!UICONTROL 管理者/レポートスイート]. 複数のレポートスイートが指定されている場合、データは各レポートスイートにコピーされます。
