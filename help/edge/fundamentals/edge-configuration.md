---
title: エッジ設定
seo-title: Experience PlatformWeb SDKのエッジ設定
description: 'Experience Platformエッジネットワークを構成する方法を説明します。 '
seo-description: 'Experience Platformエッジネットワークを構成する方法を説明します。 '
translation-type: tm+mt
source-git-commit: 7b07a974e29334cde2dee7027b9780a296db7b20
workflow-type: tm+mt
source-wordcount: '870'
ht-degree: 3%

---


# エッジの設定

Adobe Experience Platformの設定 [!DNL Web SDK] は2か所に分かれます。 SDKの [configureコマンド](configuring-the-sdk.md) は `edgeDomain`、クライアントで処理する必要のある操作（例：）を制御します。 エッジ設定は、SDKのその他すべての設定を処理します。 リクエストがAdobe Experience Platformに送信されると [!DNL Edge Network]、そのリクエスト `edgeConfigId` がサーバ側の設定の参照に使用されます。 これにより、Webサイトでコードを変更することなく、設定を更新できます。

## エッジ設定IDの作成

エッジ設定IDは、エッジ設定ツールを使用してAdobe [!DNL Launch] で作成できます。 このツールを使用すると、エッジ設定と、それらの設定内の環境の両方を作成できます。

![エッジ設定ツールのナビゲーション](../../assets/edge_configuration_nav.png)

>[!NOTE]
>
>
>
>エッジ設定ツールは、タグマネージャーとして使用しているかどうかに関係なく、許可リスト [!DNL Launch] 上で利用できます。 また、での開発権限も必要で [!DNL Launch]す。 詳しくは、ドキュメントの「 [ユーザー権限](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/admin/user-permissions.html) 」 [!DNL Launch] の記事を参照してください。

エッジ設定を作成するには、画面の右上領域にある **[!UICONTROL 新規エッジ設定]** (New Edge Configuration)をクリックします。 名前と説明を指定すると、各環境のデフォルト設定が求められます。

### デフォルトの環境設定

これらのデフォルト設定は、同じ設定で最初の3つの環境を作成する場合に使用します。 これらの3つの環境は、 *dev*、 *stage*、 *prod*&#x200B;です。 これらは、の3つのデフォルト環境に一致し [!DNL Launch]ます。 開発環境への [!DNL Launch] ライブラリを構築する場合、ライブラリは設定の開発環境を自動的に使用します。 個々の環境の設定を必要に応じて編集できます。

SDKでとして使用されるID `edgeConfigId` は、設定と環境を指定する複合IDです。 環境が存在しない場合は、実稼働環境が使用されます。

### 環境設定

以下に、環境が使用できる各設定を示します。 ほとんどのセクションは有効または無効にできます。 無効にすると、設定は保存されますが、アクティブになりません。

#### [!UICONTROL ID]

IDセクションは、常にオンになる唯一のセクションです。 次の2つの設定を使用できます。 [!UICONTROL ID同期が有効] 、 [!UICONTROL ID同期コンテナID]。

![設定UIの「ID」セクション](../../assets/edge_configuration_identity.png)

##### [!UICONTROL IDの同期が有効]

SDKがサードパーティパートナーとのID同期を実行するかどうかを制御します。

##### [!UICONTROL ID同期コンテナID]

ID同期をコンテナにグループ化して、異なるID同期を異なる時間に実行できるようにします。 これは、特定の設定IDに対して実行されるID同期のコンテナを制御します。

#### Adobe Experience Platform

ここに示す設定を使用して、Adobe Experience Platformにデータを送信できます。 このセクションは、Adobe Experience Platformを購入した場合にのみ有効にする必要があります。

![Adobe Experience Platform設定ブロック](../../assets/edge_configuration_aep.png)

##### [!UICONTROL サンドボックス]

サンドボックスは、Adobe Experience Platform内で顧客がデータと実装を相互に分離できる場所です。 サンドボックスの機能について詳しくは、 [サンドボックスのドキュメントを参照してください](../../sandboxes/home.md)。

##### [!UICONTROL ストリーミングインレット]

ストリーミングインレットは、Adobe Experience Platform内のHTTPソースです。 これらは、HTTP APIとしてAdobe Experience Platformの [!UICONTROL 「ソース] 」タブに作成されます。

##### [!UICONTROL イベントデータセット]

エッジ設定では、クラス [!UICONTROL エクスペリエンスイベントのスキーマを持つデータセットへのデータ送信がサポートされ]ます。

#### Adobe Target

Adobe Targetを設定するには、クライアントコードを指定する必要があります。 その他のフィールドはオプションです。

![Adobe Target設定ブロック](../../assets/edge_configuration_target.png)

>[!NOTE]
>
>
>
>クライアントコードに関連付けられた組織は、設定IDが作成された組織と一致する必要があります。

##### [!UICONTROL クライアントコード]

ターゲットアカウントの一意のID。 これを見つけるには、 [!UICONTROL Adobe Target] / [!UICONTROL 設定]/設定の設定 [!UICONTROL の編集/設定の編集/設定の次の.js mboxまたはmboxの] どちらかの.js mboxの [!UICONTROL download buttonに移動しま][!UICONTROL す。]

##### [!UICONTROL プロパティトークン]

[!DNL Target] プロパティを使用して権限を制御できます。 詳しくは、ドキュメントの「 [エンタープライズ権限](https://docs.adobe.com/content/help/en/target/using/administer/manage-users/enterprise/properties-overview.html) 」セクションを参照して [!DNL Target] ください。

プロパティトークンは、 [!UICONTROL Adobe Target] / [!UICONTROL 設定] / [!UICONTROL プロパティで確認できます]

##### [!UICONTROL Target環境ID]

[Adobe Targetの環境](https://docs.adobe.com/content/help/en/target/using/administer/hosts.html) は、開発のすべての段階を通じて実装を管理するのに役立ちます。 この設定は、各環境で使用する環境を指定します。

Adobeでは、この設定を簡単にするために、 `dev`、、 `stage`および `prod` エッジ設定環境ごとに異なる方法で行うことをお勧めします。 ただし、既に [!UICONTROL Adobe Target環境が定義されている場合は] 、それらを使用できます。

#### Adobe Audience Manager

Adobe Audience Managerにデータを送信する際に必要なのは、このセクションを有効にすることだけです。 その他の設定はオプションですが、推奨されています。

![Adobeオーディエンス設定の管理ブロック](../../assets/edge_configuration_aam.png)

##### [!UICONTROL Cookieの宛先が有効]

SDKが、 [Cookieの宛先を使用して、からセグメント情報を共有でき](https://docs.adobe.com/content/help/en/audience-manager/user-guide/features/destinations/custom-destinations/create-cookie-destination.html)[!DNL Audience Manager]ます。

##### [!UICONTROL URL宛先が有効]

SDKが [URLの宛先を介してセグメント情報を共有できるようにします](https://docs.adobe.com/content/help/en/audience-manager/user-guide/features/destinations/custom-destinations/create-url-destination.html)。 これらは、で設定し [!DNL Audience Manager]ます。

#### Adobe Analytics

データをAdobe Analyticsに送信するかどうかを制御します。 詳細は [Analytics概要](../solution-specific/analytics/analytics-overview.md)。

![Adobe Analytics設定ブロック](../../assets/edge_configuration_aa.png)

##### [!UICONTROL レポートスイート ID]

レポートスイートは、Adobe Analytics管理者/ [!UICONTROL 管理者/レポートスイートの下にある[管理者]]セクションにあります。 複数のレポートスイートを指定した場合は、各レポートスイートにデータがコピーされます。
