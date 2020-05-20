---
title: オーディエンスマネージャDIL拡張
seo-title: オーディエンスマネージャDIL拡張
description: オーディエンスマネージャーDIL拡張機能は、Adobe Real-time Customer Data Platformのデータ管理プラットフォーム(DMP)の保存先です。 拡張機能の詳細については、Adobe Exchangeの拡張機能ページを参照してください。
seo-description: オーディエンスマネージャーDIL拡張機能は、Adobe Real-time Customer Data Platformのデータ管理プラットフォーム(DMP)の保存先です。 拡張機能の詳細については、Adobe Exchangeの拡張機能ページを参照してください。
translation-type: tm+mt
source-git-commit: bfcbc56f05fa1c3b5fafd57b1166e50130b6007d
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 7%

---


# オーディエンスマネージャDIL拡張 {#aam-dil-extension}

## 概要 {#overview}

これは、Adobeオーディエンスマネージャーデータ統合ライブラリ拡張機能（クライアント側実装）です。 注意： この拡張機能は、Adobe Analyticsデータのサーバー側転送(SSF)には使用できません。 SSFの場合は、Adobe Analytics拡張機能を使用します。 重要： バージョン8.0以降、DILはExperience Cloud IDサービスバージョン3.3以降に強い依存関係を持ちます。 Experience Cloud IDサービスとDILの両方を実装して、Experience Managerの全オーディエンス統合機能を実現してください。

オーディエンスマネージャーDILは、Adobe Real-time Customer Data Platformのデータ管理プラットフォーム(DMP)拡張機能です。 拡張機能について詳しくは、Experience Platform Launchドキュメントの [オーディエンスマネージャー拡張機能ページ](https://docs.adobe.com/content/help/ja-JP/launch/using/extensions-ref/adobe-extension/adobe-audience-manager-extension.html) を参照してください。

この宛先は、エクスペリエンスプラットフォーム起動の拡張です。 Adobe Real-time CDPでのLaunch拡張機能の動作について詳しくは、「 [エクスペリエンスプラットフォーム起動拡張機能の概要](/help/rtcdp/destinations/experience-platform-launch-extensions.md)」を参照してください。

![オーディエンスマネージャDIL拡張](/help/rtcdp/destinations/assets/aam-dil-extension.png)

## 前提条件 {#prerequisites}

この拡張機能は、アドビのリアルタイムCDPを購入したすべてのお客様のDestinationsカタログで利用できます。

この拡張機能を使用するには、エクスペリエンスプラットフォームの起動にアクセスする必要があります。 エクスペリエンスプラットフォームの発売は、Adobe Experience Cloudのお客様に対して、付加価値機能として提供されます。 Launchへのアクセス権を取得するには、組織管理者に連絡して、拡張機能をインストールできるように **[!UICONTROL manage_properties]** 権限を付与するように依頼します。

## 拡張機能のインストール {#install-extension}

オーディエンスマネージャーDIL拡張機能をインストールするには：

1. [Adobe Real-time CDPインターフェイスで](http://platform.adobe.com/)、 **[!UICONTROL Destinations/Catalogに移動します]**。
2. カタログから拡張子を選択するか、検索バーを使用します。
3. リンク先をクリックしてハイライト表示し、右側のレールで「 **[!UICONTROL Install Extension]** 」を選択します。 「 **[!UICONTROL Install Extension]** 」コントロールが灰色表示になっている場合は、 **[!UICONTROL manage_properties]** 権限がありません。 「 [前提条件](#prerequisites)」を参照してください。
4. 「使用可能な起動プロパティ **[!UICONTROL を選択]** 」ウィンドウで、拡張機能をインストールするLaunchプロパティを選択します。 また、「起動」で新しいプロパティを作成するオプションもあります。 プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについては、起動ドキュメントの [プロパティページセクション](https://docs.adobe.com/content/help/en/launch/using/reference/admin/companies-and-properties.html#properties-page) で説明します。
5. ワークフローが起動し、インストールが完了します。

拡張機能の設定オプションについて詳しくは、Experience Launchドキュメントの [オーディエンスマネージャー拡張機能ページ](https://docs.adobe.com/content/help/ja-JP/launch/using/extensions-ref/adobe-extension/adobe-audience-manager-extension.html) を参照してください。

この拡張機能は、 [Experience Platform Launchインターフェイスに直接インストールすることもできます](https://launch.adobe.com/)。 Launchドキュメ [ントで新しい拡張機能を追加参照してください](https://docs.adobe.com/content/help/en/launch/using/reference/manage-resources/extensions/overview.html#add-a-new-extension) 。


## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールすると、「起動」で直接、拡張機能のルール設定を開始できます。

「起動」では、特定の状況でのみイベントデータを拡張子の宛先に送信するように、インストール済みの拡張子のルールを設定できます。 拡張機能のルールの設定について詳しくは、「ルール [ドキュメント](https://docs.adobe.com/help/ja-JP/launch/using/reference/manage-resources/rules.html)」を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

起動インターフェイスでは、拡張機能の設定、アップグレードおよび削除が可能です。

>[!TIP]
>
>拡張機能が既にいずれかのプロパティにインストールされている場合、Adobe Real-time CDP UIには、拡張機能の **[!UICONTROL Install]** と表示されます。 「 [Install extension](#install-extension) 」の説明に従ってインストールワークフローを開始し、拡張機能を起動して設定または削除します。

拡張機能をアップグレードするには、Launchドキュメントの [Extensionアップグレード](https://docs.adobe.com/content/help/en/launch/using/reference/manage-resources/extensions/extension-upgrade.html) を参照してください。



