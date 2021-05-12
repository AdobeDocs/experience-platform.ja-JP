---
title: Experience PlatformWeb SDK用のデータストリームの設定
description: 'データストリームの設定方法を説明します。 '
keywords: 設定；datastreams;datastreamId;edge;edge configuration id;環境設定；edgeConfigId;idsync enabled;ID同期コンテナID;Sandbox；ストリーミングインレット；イベントデータセット；ターゲット；クライアントコード；プロパティトークン；ターゲット環境ID;Cookie宛先；Analytics設定ブロックレポートスイートid;
exl-id: 736c75cb-e290-474e-8c47-2a031f215a56
source-git-commit: 5642fa155d487982f01d25fa765bb36ad5c3bb21
workflow-type: tm+mt
source-wordcount: '914'
ht-degree: 2%

---


# データストリームの設定

Adobe Experience PlatformWeb SDKの設定は、2か所に分かれています。 SDKの[configureコマンド](configuring-the-sdk.md)は、`edgeDomain`のように、クライアントで処理する必要のある処理を制御します。 データストリームは、SDKの他のすべての設定を処理します。 要求がAdobe Experience Platformエッジネットワークに送信されると、`edgeConfigId`はサーバ側の設定を参照するために使用されます。 これにより、Webサイトでコードを変更することなく、設定を更新できます。

この機能を使用するには、組織がプロビジョニングされている必要があります。 Customer Success Manager(CSM)に問い合わせて、許可リストを使用してください。

## データストリーム設定の作成

データストリームは、Adobe[!DNL Experience Platform Launch]でデータストリーム設定ツールを使用して作成できます。

![datastreamsツールのナビゲーション](../../assets/datastreams_config.png)

>[!NOTE]
>
>[!DNL Experience Platform Launch]をタグマネージャーとして使用しているかどうかに関係なく、許可リスト上のお客様はデータストリーム設定ツールを使用できます。 さらに、[!DNL Experience Platform Launch]での開発権限が必要です。 詳しくは、[!DNL Experience Platform Launch]ドキュメントの[ユーザー権限](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/admin/user-permissions.html)の記事を参照してください。

画面の右上の領域にある「**[!UICONTROL 新しいデータストリーム]**」をクリックして、データストリームを作成します。 名前と説明を指定すると、各環境のデフォルト設定が求められます。 以下に、使用可能な設定を示します。

データストリームを作成する場合、同じ設定で3つの環境が自動的に作成されます。 これらの3つの環境は、*dev*、*stage*、*prod*&#x200B;です。 これらは[!DNL Experience Platform Launch]の3つのデフォルト環境に一致します。 Dev環境に対して[!DNL Experience Platform Launch]ライブラリを構築する場合、ライブラリは設定のDev環境を自動的に使用します。 個々の環境の設定を必要に応じて編集できます。

SDKで`edgeConfigId`として使用されるIDは、設定と環境（例えば`1c86778b-cdba-4684-9903-750e52912ad1:stage`）を指定する複合IDです。 複合IDに環境が存在しない場合（前の例では`stage`）、実稼働環境が使用されます。

各設定環境で使用できる設定は次のとおりです。 ほとんどのセクションは有効または無効にできます。 無効にすると、設定は保存されますが、アクティブになりません。

## [!UICONTROL サードパーティ] IDの設定

サードパーティIDセクションは、常にオンになる唯一のセクションです。 次の2つの設定を使用できます。&quot;[!UICONTROL サードパーティIDの同期が有効になりました]&quot;および&quot;[!UICONTROL サードパーティIDの同期コンテナID]&quot;。

![設定UIの「ID」セクション](../../assets/edge_configuration_identity.png)

### [!UICONTROL サードパーティIDの同期が有効]

SDKがサードパーティパートナーとのID同期を実行するかどうかを制御します。

### [!UICONTROL サードパーティIDの同期コンテナID]

ID同期をコンテナにグループ化して、異なるID同期を異なる時間に実行できるようにします。 これは、特定の設定IDに対して実行されるID同期のコンテナを制御します。

## Adobe Experience Platform設定

ここに示す設定は、Adobe Experience Platformにデータを送信する場合に使用します。 このセクションは、Adobe Experience Platformを購入した場合にのみ有効にする必要があります。

![Adobe Experience Platform設定ブロック](../../assets/edge_configuration_aep.png)

### [!UICONTROL サンドボックス]

サンドボックスは、お客様がデータと実装を相互に分離できるAdobe Experience Platformの場所です。 動作方法の詳細については、[サンドボックスのドキュメント](../../sandboxes/home.md)を参照してください。

### [!UICONTROL ストリーミングインレット]

ストリーミングインレットは、Adobe Experience PlatformのHTTPソースです。 これらは、HTTP APIとしてAdobe Experience Platformの「[!UICONTROL Sources]」タブに作成されます。

### [!UICONTROL イベントデータセット]

データストリームは、クラス[!UICONTROL エクスペリエンスイベント]のスキーマを持つデータセットへのデータ送信をサポートします。

## Adobe Target設定

Adobe Targetを設定するには、クライアントコードを指定する必要があります。 その他のフィールドはオプションです。

![Adobe Target設定ブロック](../../assets/edge_configuration_target.png)

>[!NOTE]
>
>クライアントコードに関連付けられた組織は、設定IDが作成された組織と一致する必要があります。

### [!UICONTROL クライアントコード]

ターゲットアカウントの一意のID。 これを調べるには、[!UICONTROL at.js]または[!UICONTROL の[!UICONTROL download]ボタンの横にある]Adobe Target] >[!UICONTROL セットアップ[!UICONTROL  > ]実装[!UICONTROL  >設定を編集]に移動します。2/>mbox.js][!UICONTROL 

### [!UICONTROL プロパティトークン]

[!DNL Target] プロパティを使用して権限を制御できます。詳しくは、[!DNL Target]ドキュメントの[エンタープライズ権限](https://docs.adobe.com/content/help/ja-JP/target/using/administer/manage-users/enterprise/properties-overview.translate.html)セクションを参照してください。

プロパティトークンは、[!UICONTROL Adobe Target] > [!UICONTROL セットアップ] > [!UICONTROL プロパティ]にあります

### [!UICONTROL ターゲット環境ID]

[Adobe Targetの](https://docs.adobe.com/content/help/en/target/using/administer/hosts.html) 環境は、開発の全段階を通じて導入の管理を支援します。この設定は、各環境で使用する環境を指定します。

Adobeでは、この設定を`dev`、`stage`、`prod`の各データストリーム環境ごとに異なる方法で行うことをお勧めします。 ただし、既にAdobe Target環境を定義している場合は、それらを使用できます。

## Adobe Audience Manager設定

データをAdobe Audience Managerに送るのに必要なのは、このセクションを有効にすることだけです。 その他の設定はオプションですが、推奨されています。

![Adobeオーディエンス設定の管理ブロック](../../assets/edge_configuration_aam.png)

### [!UICONTROL Cookieの宛先が有効]

SDKが[!DNL Audience Manager]の[Cookieの宛先](https://docs.adobe.com/content/help/en/audience-manager/user-guide/features/destinations/custom-destinations/create-cookie-destination.html)を介してセグメント情報を共有できるようにします。

### [!UICONTROL URL宛先が有効]

SDKが[URL宛先](https://docs.adobe.com/content/help/en/audience-manager/user-guide/features/destinations/custom-destinations/create-url-destination.html)を介してセグメント情報を共有できるようにします。 これらは[!DNL Audience Manager]に設定されます。

## Adobe Analytics設定

データをAdobe Analyticsに送信するかどうかを制御します。 詳しくは、「[Analyticsの概要](../data-collection/adobe-analytics/analytics-overview.md)」を参照してください。

![Adobe Analytics設定ブロック](../../assets/edge_configuration_aa.png)

### [!UICONTROL レポートスイート ID]

このレポートスイートは、[!UICONTROL 管理者/]の下のAdobe Analytics管理者セクションにあります。 複数のレポートスイートを指定した場合は、各レポートスイートにデータがコピーされます。
