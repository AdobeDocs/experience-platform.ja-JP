---
title: エッジ設定
seo-title: Experience PlatformWeb SDKのエッジ設定
description: 'Experience Platformエッジネットワークを構成する方法を説明します。 '
seo-description: 'Experience Platformエッジネットワークを構成する方法を説明します。 '
keywords: configuration;edge;edge configuration id;Environment Settings;edgeConfigId;identity;id sync enabled;ID Sync Container ID;Sandbox;Streaming Inlet;Event Dataset;target;client code;Property Token;Target Environment ID;Cookie Destinations;url Destinations;Analytics Settings Blockreport suite id;
translation-type: tm+mt
source-git-commit: 0928dd3eb2c034fac14d14d6e53ba07cdc49a6ea
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 3%

---


# エッジの設定

Adobe Experience PlatformWeb SDKの設定は、2か所に分かれています。 SDKの [configureコマンド](configuring-the-sdk.md) は `edgeDomain`、クライアントで処理する必要のある操作（例：）を制御します。 エッジ設定は、SDKのその他すべての設定を処理します。 要求がAdobe Experience Platformエッジネットワークに送信されると、その要求 `edgeConfigId` がサーバ側の設定の参照に使用されます。 これにより、Webサイトでコードを変更することなく、設定を更新できます。

この機能を使用するには、組織がプロビジョニングされている必要があります。 許可リストを使用するには、Certified Software Manager(CSM)に問い合わせてください。

## エッジ設定の作成

エッジ設定は、エッジ設定ツールを使用してAdobe [!DNL Experience Platform Launch] で作成できます。

![エッジ設定ツールのナビゲーション](../../assets/edge_configuration_nav.png)

>[!NOTE]
>
>エッジ設定ツールは、タグマネージャーとして使用しているかどうかに関係なく、許可リスト [!DNL Experience Platform Launch] 上で利用できます。 また、での開発権限も必要で [!DNL Experience Platform Launch]す。 詳しくは、ドキュメントの「 [ユーザー権限](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/admin/user-permissions.html) 」 [!DNL Experience Platform Launch] の記事を参照してください。

画面の右上領域にある「 **[!UICONTROL 新しいエッジ設定]** 」をクリックして、エッジ設定を作成します。 名前と説明を指定すると、各環境のデフォルト設定が求められます。 以下に、使用可能な設定を示します。

エッジ設定を作成する場合、3つの環境が同じ設定で自動的に作成されます。 これらの3つの環境は、 *dev*、 *stage*、 *prod*&#x200B;です。 これらは、の3つのデフォルト環境に一致し [!DNL Experience Platform Launch]ます。 開発環境への [!DNL Experience Platform Launch] ライブラリを構築する場合、ライブラリは設定の開発環境を自動的に使用します。 個々の環境の設定を必要に応じて編集できます。

SDKでのIDは、設定と環境(例えば、 `edgeConfigId``1c86778b-cdba-4684-9903-750e52912ad1:stage`)を指定する複合IDです。 複合IDに環境が存在しない場合(前の例 `stage` など)、実稼働環境が使用されます。

各設定環境で使用できる設定は次のとおりです。 ほとんどのセクションは有効または無効にできます。 無効にすると、設定は保存されますが、アクティブになりません。

## [!UICONTROL ID] Settings

IDセクションは、常にオンになる唯一のセクションです。 次の2つの設定を使用できます。「[!UICONTROL ID同期が有効になりました]」および「[!UICONTROL ID同期コンテナID]」。

![設定UIの「ID」セクション](../../assets/edge_configuration_identity.png)

### [!UICONTROL IDの同期が有効]

SDKがサードパーティパートナーとのID同期を実行するかどうかを制御します。

### [!UICONTROL ID同期コンテナID]

ID同期をコンテナにグループ化して、異なるID同期を異なる時間に実行できるようにします。 これは、特定の設定IDに対して実行されるID同期のコンテナを制御します。

## Adobe Experience Platform設定

ここに示す設定は、Adobe Experience Platformにデータを送信する場合に使用します。 このセクションは、Adobe Experience Platformを購入した場合にのみ有効にする必要があります。

![Adobe Experience Platform設定ブロック](../../assets/edge_configuration_aep.png)

### [!UICONTROL サンドボックス]

サンドボックスは、お客様がデータと実装を相互に分離できるAdobe Experience Platformの場所です。 サンドボックスの機能について詳しくは、 [サンドボックスのドキュメントを参照してください](../../sandboxes/home.md)。

### [!UICONTROL ストリーミングインレット]

ストリーミングインレットは、Adobe Experience PlatformのHTTPソースです。 これらは、HTTP APIとしてAdobe Experience Platformの「[!UICONTROL ソース]」タブに作成されます。

### [!UICONTROL イベントデータセット]

エッジ設定では、クラス [!UICONTROL エクスペリエンスイベントのスキーマを持つデータセットへのデータ送信がサポートされ]ます。

## Adobe Target設定

Adobe Targetを設定するには、クライアントコードを指定する必要があります。 その他のフィールドはオプションです。

![Adobe Target設定ブロック](../../assets/edge_configuration_target.png)

>[!NOTE]
>
>クライアントコードに関連付けられた組織は、設定IDが作成された組織と一致する必要があります。

### [!UICONTROL クライアントコード]

ターゲットアカウントの一意のID。 これを見つけるには、 [!UICONTROL Adobe Target] / [!UICONTROL セットアップ]/設定/設定の編集/mbox.jsの [!UICONTROL ダウンロード/mbox.jsのダウンロード/mbox.jsの] ダウンロード/mbox.jsのダウンロード/mbox. [!UICONTROL jsのいずれかに移動します。]

### [!UICONTROL プロパティトークン]

[!DNL Target] プロパティを使用して権限を制御できます。 詳しくは、ドキュメントの「 [エンタープライズ権限](https://docs.adobe.com/content/help/ja-JP/target/using/administer/manage-users/enterprise/properties-overview.translate.html) 」セクションを参照して [!DNL Target] ください。

プロパティトークンは、 [!UICONTROL Adobe Target] / [!UICONTROL セットアップ] / [!UICONTROL プロパティで確認できます]

### [!UICONTROL ターゲット環境ID]

[Adobe Targetの環境](https://docs.adobe.com/content/help/en/target/using/administer/hosts.html) は、開発の全段階を通じて実装を管理するお手伝いをします。 この設定は、各環境で使用する環境を指定します。

Adobeでは、この設定を簡単にするために、 `dev`、、 `stage`および `prod` エッジ設定環境ごとに異なる方法で行うことをお勧めします。 ただし、既にAdobe Target環境を定義している場合は、それらを使用できます。

## Adobe Audience Manager設定

データをAdobe Audience Managerに送るのに必要なのは、このセクションを有効にすることだけです。 その他の設定はオプションですが、推奨されています。

![Adobeオーディエンス設定の管理ブロック](../../assets/edge_configuration_aam.png)

### [!UICONTROL Cookieの宛先が有効]

SDKが、 [Cookieの宛先を使用して、からセグメント情報を共有でき](https://docs.adobe.com/content/help/en/audience-manager/user-guide/features/destinations/custom-destinations/create-cookie-destination.html)[!DNL Audience Manager]ます。

### [!UICONTROL URL宛先が有効]

SDKが [URLの宛先を介してセグメント情報を共有できるようにします](https://docs.adobe.com/content/help/en/audience-manager/user-guide/features/destinations/custom-destinations/create-url-destination.html)。 これらは、で設定し [!DNL Audience Manager]ます。

## Adobe Analytics設定

データをAdobe Analyticsに送信するかどうかを制御します。 詳しくは、 [Analyticsの概要を参照してください](../data-collection/adobe-analytics/analytics-overview.md)。

![Adobe Analytics設定ブロック](../../assets/edge_configuration_aa.png)

### [!UICONTROL レポートスイート ID]

レポートスイートは、Adobe Analytics管理者/ [!UICONTROL 管理者/レポートスイートの下にある[管理者]]セクションにあります。 複数のレポートスイートを指定した場合は、各レポートスイートにデータがコピーされます。
