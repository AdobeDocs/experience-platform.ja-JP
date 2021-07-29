---
title: Experience PlatformWeb SDK用のDatastreamの設定
description: 'データストリームの設定方法を説明します。 '
keywords: 設定；datastreams;datastreamId;edge;datastream id；環境設定；edgeConfigId;ID同期有効；ID同期コンテナID；サンドボックス；ストリーミングインレット；イベントデータセット；ターゲットコード；クライアントコード；Target環境ID;Cookie宛先；Analytics設定ブロックレポートスイートID;
exl-id: 736c75cb-e290-474e-8c47-2a031f215a56
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '901'
ht-degree: 1%

---


# データストリームの設定

Adobe Experience Platform Web SDKの設定は、2つの場所に分かれています。 SDKの[configureコマンド](configuring-the-sdk.md)は、`edgeDomain`のように、クライアントで処理する必要があるものを制御します。 データストリームは、SDKのその他すべての設定を処理します。 リクエストがAdobe Experience Platform Edgeネットワークに送信されると、`edgeConfigId`がサーバー側の設定の参照に使用されます。 これにより、Webサイトでコードを変更することなく、設定を更新できます。

この機能を使用するには、組織がプロビジョニングされている必要があります。 カスタマーサクセスマネージャー(CSM)に問い合わせて、このマネージャーを使用してく許可リストださい。

## データストリーム設定の作成

データストリームは、データ収集UIでデータストリーム設定ツールを使用して作成できます。

![datastreamsツールのナビゲーション](../images/datastreams/config.png)

>[!NOTE]
>
>データストリーム設定ツールは、Platformをタグマネージャーとして使用しているかどうかに関係なく、許可リスト上のお客様が使用できます。 また、ユーザーには開発権限が必要です。 詳しくは、タグのドキュメントの[ユーザー権限](../../tags/ui/administration/user-permissions.md)の記事を参照してください。

画面の右上にある「**[!UICONTROL 新しいデータストリーム]**」をクリックして、データストリームを作成します。 名前と説明を入力すると、各環境のデフォルト設定が求められます。 以下に、使用可能な設定について説明します。

データストリームを作成する場合、同じ設定で3つの環境が自動的に作成されます。 これら3つの環境は、*dev*、*stage*、*prod*&#x200B;です。 タグの3つのデフォルト環境に一致します。 開発環境にタグライブラリを構築すると、ライブラリは設定の開発環境を自動的に使用します。 個々の環境の設定は、好きなだけ編集できます。

SDKで`edgeConfigId`として使用されるIDは、設定と環境を指定する複合IDです（例：`1c86778b-cdba-4684-9903-750e52912ad1:stage`）。 複合IDに環境が存在しない場合（前の例では`stage` ）、実稼動環境が使用されます。

各設定環境で使用できる設定を次に示します。 ほとんどのセクションは、有効または無効にできます。 無効にすると、設定は保存されますが、アクティブになりません。

## [!UICONTROL サードパーティID] の設定

サードパーティIDセクションは、常にオンになる唯一のセクションです。 次の2つの設定を使用できます。&quot;[!UICONTROL サードパーティID同期が有効]&quot;および&quot;[!UICONTROL サードパーティID同期コンテナID]&quot;。

![設定UIのIDセクション](../images/datastreams/edge_configuration_identity.png)

### [!UICONTROL サードパーティID同期が有効]

SDKがサードパーティパートナーとのID同期を実行するかどうかを制御します。

### [!UICONTROL サードパーティID同期コンテナID]

ID同期をコンテナにグループ化して、異なるID同期を異なる時間に実行できます。 これは、特定の設定IDに対して実行されるID同期のコンテナを制御します。

## Adobe Experience Platform Settings

ここに示す設定を使用すると、Adobe Experience Platformにデータを送信できます。 Adobe Experience Platformを購入済みの場合にのみ、このセクションを有効にする必要があります。

![Adobe Experience Platform設定ブロック](../images/datastreams/edge_configuration_aep.png)

### [!UICONTROL サンドボックス]

サンドボックスは、Adobe Experience Platform内のお客様がデータと実装を分離できる場所です。 その仕組みについて詳しくは、[サンドボックスのドキュメント](../../sandboxes/home.md)を参照してください。

### [!UICONTROL ストリーミングインレット]

ストリーミングインレットは、Adobe Experience PlatformのHTTPソースです。 これらは、Adobe Experience Platformの「[!UICONTROL Sources]」タブにHTTP APIとして作成されます。

### [!UICONTROL イベントデータセット]

データストリームは、クラス[!UICONTROL エクスペリエンスイベント]のスキーマを持つデータセットへのデータの送信をサポートします。

## Adobe Target Settings

Adobe Targetを設定するには、クライアントコードを指定する必要があります。 その他のフィールドはオプションです。

![Adobe Target設定ブロック](../images/datastreams/edge_configuration_target.png)

>[!NOTE]
>
>クライアントコードに関連付けられた組織は、設定IDを作成する組織と一致する必要があります。

### [!UICONTROL クライアントコード]

ターゲットアカウントの一意のID。 これを見つけるには、[!UICONTROL Adobe Target] > [!UICONTROL セットアップ] [!UICONTROL 実装] > [!UICONTROL 編集設定]の隣にある[!UICONTROL at.js]または&lt;a11/の[!UICONTROL ボタンに移動します。2/>mbox.js]][!UICONTROL 

### [!UICONTROL プロパティトークン]

[!DNL Target] では、プロパティを使用して権限を制御できます。詳しくは、[!DNL Target]ドキュメントの[Enterprise権限](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html?lang=ja)の節を参照してください。

プロパティトークンは、[!UICONTROL Adobe Target] > [!UICONTROL setup] > [!UICONTROL プロパティ]にあります。

### [!UICONTROL Target環境ID]

[](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html) Adobe Targetの環境は、あらゆる開発段階を通じて実装を管理するのに役立ちます。この設定は、各環境で使用する環境を指定します。

Adobeでは、この設定を`dev`、`stage`、`prod`の各データストリーム環境ごとに異なる方法でおこなうことをお勧めします。 ただし、既にAdobe Target環境を定義している場合は、それらを使用できます。

## Adobe Audience Manager Settings

Adobe Audience Managerにデータを送信するために必要な操作は、この節を有効にすることだけです。 その他の設定はオプションですが、推奨されます。

![Adobeオーディエンス管理設定ブロック](../images/datastreams/edge_configuration_aam.png)

### [!UICONTROL Cookieの宛先が有効]

SDKが[!DNL Audience Manager]の[Cookieの宛先](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/destinations/custom-destinations/create-cookie-destination.html)を介してセグメント情報を共有できるようにします。

### [!UICONTROL URL宛先の有効化]

SDKが[URLの宛先](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/destinations/custom-destinations/create-url-destination.html)を介してセグメント情報を共有できるようにします。 これらは[!DNL Audience Manager]で設定します。

## Adobe Analytics Settings

データをAdobe Analyticsに送信するかどうかを制御します。 追加の詳細については、[Analyticsの概要](../data-collection/adobe-analytics/analytics-overview.md)を参照してください。

![Adobe Analytics設定ブロック](../images/datastreams/edge_configuration_aa.png)

### [!UICONTROL レポートスイート ID]

このレポートスイートは、[!UICONTROL 管理者/Adobe Analytics]の下の「ReportSuites管理者」セクションにあります。 複数のレポートスイートを指定した場合、データは各レポートスイートにコピーされます。
