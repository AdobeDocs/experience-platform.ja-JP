---
title: エッジ設定
seo-title: エクスペリエンスプラットフォームWeb SDKのエッジ設定
description: 'Experience Platform Edge Networkを設定する方法を説明します。 '
seo-description: 'Experience Platform Edge Networkを設定する方法を説明します。 '
translation-type: tm+mt
source-git-commit: e9fb726ddb84d7a08afb8c0f083a643025b0f903
workflow-type: tm+mt
source-wordcount: '883'
ht-degree: 3%

---


# エッジ設定

Adobe Experience Platform Web SDKの設定は、2か所に分かれています。 SDKの [configureコマンド](configuring-the-sdk.md) は `edgeDomain`、クライアントで処理する必要のある操作（例：）を制御します。 エッジ設定は、SDKのその他すべての設定を処理します。 リクエストがAdobe Experience Platform Edge Networkに送信されると、の値 `edgeConfigId` がサーバー側の設定の参照に使用されます。 これにより、Webサイトでコードを変更することなく、設定を更新できます。

## エッジ設定IDの作成

エッジ設定IDは、「起動」で、エッジ設定ツールを使用して作成できます。 このツールを使用すると、エッジ設定と、それらの設定内の環境の両方を作成できます。

![エッジ設定ツールのナビゲーション](../../assets/edge_configuration_nav.png)

>[!NOTE]
>
>エッジ設定ツールは、ホワイトリスト登録を受けたお客様は、Launchをタグマネージャーとして使用しているかどうかに関係なく使用できます。 また、ユーザーは起動時に開発権限が必要です。 詳しくは、起動ドキュメントの「 [ユーザー権限](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/admin/user-permissions.html) 」の記事を参照してください。

エッジ設定を作成するには、画面の右上領域にある「 **[UICONTROL」「新しいエッジ設定]** 」(New Edge Configuration)の順にクリックします。 名前と説明を指定すると、各環境のデフォルト設定が求められます。

### デフォルトの環境設定

これらのデフォルト設定は、同じ設定で最初の3つの環境を作成する場合に使用します。 これらの3つの環境は、dev、stage、prodです。 これらは、「起動」の3つのデフォルト環境に一致します。 開発環境に対して起動ライブラリを構築する場合、ライブラリは設定の開発環境を自動的に使用します。 個々の環境の設定を必要に応じて編集できます。

SDKでとして使用されるID `edgeConfigId` は、設定と環境を指定する複合IDです。 環境が存在しない場合は、実稼働環境が使用されます。

### 環境設定

以下に、環境が使用できる各設定を示します。 ほとんどのセクションは有効または無効にできます。 無効にすると、設定は保存されますが、アクティブになりません。

#### [!UICONTROL ID]

IDセクションは、常にオンになる唯一のセクションです。 次の2つの設定を使用できます。 ID同期が有効になり、ID同期コンテナIDが有効になります。

![設定UIの「ID」セクション](../../assets/edge_configuration_identity.png)

##### [!UICONTROL IDの同期が有効]

SDKがサードパーティパートナーとのID同期を実行するかどうかを制御します。

##### [!UICONTROL ID同期コンテナID]

ID同期をコンテナにグループ化して、異なるID同期を異なる時間に実行できるようにします。 これは、特定の設定IDに対して実行されるID同期のコンテナを制御します。

#### Adobe Experience Platform

ここに示す設定を使用して、Adobe Experience Platformにデータを送信できます。 このセクションは、Adobe Experience Platformを購入した場合にのみ有効にする必要があります。

![Adobe Experience Platform設定ブロック](../../assets/edge_configuration_aep.png)

##### [!UICONTROL サンドボックス]

サンドボックスは、Adobe Experience Platform内の場所で、顧客はデータと実装を相互に分離できます。 動作方法の詳細については、 [サンドボックスのドキュメントを参照してください](../../sandboxes/home.md)。

##### [!UICONTROL ストリーミングインレット]

ストリーミングインレットは、Adobe Experience PlatformのHTTPソースです。 これらは、Adobe Experience Platformの「 [!UICONTROL Sources] 」タブの下にHTTP APIとして作成されます。

##### [!UICONTROL イベントデータセット]

エッジ設定では、クラス [!UICONTROL エクスペリエンスイベントのスキーマを持つデータセットへのデータ送信がサポートされ]ます。

#### Adobe Target

Adobeターゲットを設定するには、クライアントコードを指定する必要があります。 その他のフィールドはオプションです。

![Adobeターゲット設定ブロック](../../assets/edge_configuration_target.png)

>[!NOTE]
>
>クライアントコードに関連付けられた組織は、設定IDが作成された組織と一致する必要があります。

##### [!UICONTROL クライアントコード]

ターゲットアカウントの一意のID。 これについては、 [!UICONTROL Adobeターゲット] / [!UICONTROL 設定/]設定/ [!UICONTROL 実装/次の編集/mbox.jsの設定/mbox.jsのいずれかのdownloadボタンを表示する場合は、] Adobe [!UICONTROL /設定/] 設定/設定/ [!UICONTROL mbox.jsの設定/次の設定に移動します。]

##### [!UICONTROL プロパティトークン]

ターゲットを使用すると、プロパティを使用して権限を制御できます。 詳しくは、ターゲットドキュメントの「 [Enterprise Permissions](https://docs.adobe.com/content/help/en/target/using/administer/manage-users/enterprise/properties-overview.html) 」セクションを参照してください。

プロパティトークンは、 [!UICONTROL Adobeターゲット] / [!UICONTROL セットアップ/][UICONTROLプロパティにあります]

##### [!UICONTROL ターゲット環境ID]

[アドビターゲットの環境](https://docs.adobe.com/content/help/en/target/using/administer/hosts.html) は、開発のすべての段階を通じて実装を管理するのに役立ちます。 この設定は、各環境で使用する環境を指定します。

簡単な設定にするために、 `dev`、 `stage``prod` エッジ設定環境ごとに異なる設定を行うことをお勧めします。 ただし、既に [!UICONTROL Adobeターゲット環境を定義している場合は] 、それらを使用できます。

#### Adobe Audience Manager

Adobeオーディエンスマネージャーにデータを送信する際に必要なのは、このセクションを有効にすることだけです。 その他の設定はオプションですが、推奨されています。

![Adobeオーディエンス管理設定ブロック](../../assets/edge_configuration_aam.png)

##### [!UICONTROL Cookieの宛先が有効]

SDKが、オーディエンスマネージャーの [Cookie宛先を使用してセグメント情報を共有できるようにします](https://docs.adobe.com/content/help/en/audience-manager/user-guide/features/destinations/custom-destinations/create-cookie-destination.html) 。

##### [!UICONTROL URL宛先が有効]

SDKが [URLの宛先を介してセグメント情報を共有できるようにします](https://docs.adobe.com/content/help/en/audience-manager/user-guide/features/destinations/custom-destinations/create-url-destination.html)。 これらは、オーディエンスマネージャーで設定します。

#### Adobe Analytics

データをAdobe Analyticsに送信するかどうかを制御します。 詳しくは、 [Analyticsの概要を参照してください](../solution-specific/analytics/analytics-overview.md)。

![Adobe Analytics設定ブロック](../../assets/edge_configuration_aa.png)

##### [!UICONTROL レポートスイート ID]

このレポートスイートは、Adobe Analyticsの管理者セクションの [!UICONTROL 管理者/レポートスイートにあります]。 複数のレポートスイートを指定した場合は、各レポートスイートにデータがコピーされます。
