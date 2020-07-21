---
title: Experience Cloud ID サービス拡張機能
seo-title: Experience Cloud ID サービス拡張機能
description: Experience CloudIDサービスの拡張機能は、アドビのリアルタイム顧客データPlatformのパーソナライズ先です。 拡張機能の詳細については、Adobe Exchangeの拡張機能ページを参照してください。
seo-description: Experience CloudIDサービスの拡張機能は、アドビのリアルタイム顧客データPlatformのパーソナライズ先です。 拡張機能の詳細については、Adobe Exchangeの拡張機能ページを参照してください。
translation-type: tm+mt
source-git-commit: b96286f6a06f0583b45343a513ee64f0025d79a7
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 10%

---


# [!DNL Experience Cloud] IDサービスの拡張 {#adobe-ecid-extension}

## 概要 {#overview}

この拡張には、すべてのソリューションで訪問者を識別する [!DNL Experience Cloud] IDサービスが実装され [!DNL Experience Cloud] ます。

[!DNL Experience Cloud] IDサービスは、アドビのリアルタイム顧客データPlatformのパーソナライゼーション拡張機能です。 拡張機能について詳しくは、ドキュメントの [Experience CloudIDサービス拡張のページ](https://docs.adobe.com/content/help/ja-JP/launch/using/extensions-ref/adobe-extension/id-service-extension/overview.html) を参照して [!DNL Experience Platform Launch] ください。

この宛先は [!DNL Experience Platform Launch] 拡張子です。 Adobe Real-time CDPでの [!DNL Launch] 拡張機能の詳細については、 [Experience Platform Launch拡張機能の概要を参照してください](/help/rtcdp/destinations/experience-platform-launch-extensions.md)。

![Adobe ECID拡張機能](/help/rtcdp/destinations/assets/adobe-ecid-extension.png)


## 前提条件 {#prerequisites}

この拡張機能は、アドビのリアルタイムCDPを購入したすべてのお客様のDestinationsカタログで利用できます。

この拡張機能を使用するには、にアクセスする必要があり [!DNL Experience Platform Launch]ます。 [!DNL Experience Platform Launch] は、付加価値機能としてAdobe Experience Cloudのお客様に提供されます。 組織の管理者に連絡して、へのアクセス権を取得し [!DNL Launch] 、 **[!UICONTROL manage_properties]** 権限を付与して、拡張機能をインストールできるようにするように依頼します。

## 拡張機能のインストール {#install-extension}

IDサービス拡張機能をインストールするには： [!DNL Experience Cloud]

1. [Adobe Real-time CDPインターフェイスで](http://platform.adobe.com/)、 **[!UICONTROL Destinations/Catalogに移動します]**。
2. カタログから拡張子を選択するか、検索バーを使用します。
3. リンク先をクリックしてハイライト表示し、右側のレールで「 **[!UICONTROL Install Extension]** 」を選択します。 「 **[!UICONTROL Install Extension]** 」コントロールが灰色表示になっている場合は、 **[!UICONTROL manage_properties]** 権限がありません。 「 [前提条件](#prerequisites)」を参照してください。
4. 「使用可能な起動プロパティ **[!UICONTROL を選択]** 」ウィンドウで、拡張機能をインストールする [!DNL Launch] プロパティを選択します。 また、で新しいプロパティを作成することもでき [!DNL Launch]ます。 プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについては、ドキュメントの「 [プロパティ」ページのセクション](https://docs.adobe.com/content/help/en/launch/using/reference/admin/companies-and-properties.html#properties-page) で説明 [!DNL Launch] します。
5. ワークフローにより、インストール [!DNL Launch] が完了します。

拡張機能の設定オプションとインストールのサポートについて詳しくは、ドキュメントの [Experience CloudIDサービス拡張のページ](https://docs.adobe.com/content/help/ja-JP/launch/using/extensions-ref/adobe-extension/id-service-extension/overview.html) を参照して [!DNL Experience Launch] ください。

拡張機能は、 [Experience Platform Launchインターフェイスに直接インストールすることもできます](https://launch.adobe.com/)。 ドキュメント [の新しい拡張子を追加参照してく](https://docs.adobe.com/content/help/en/launch/using/reference/manage-resources/extensions/overview.html#add-a-new-extension)[!DNL Launch] ださい。

## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールした後、で直接ルールの設定を開始でき [!DNL Launch]ます。

で [!DNL Launch]は、特定の状況でのみイベントデータを拡張子の宛先に送信するように、インストール済みの拡張子のルールを設定できます。 拡張機能のルールの設定について詳しくは、「ルール [ドキュメント](https://docs.adobe.com/help/ja-JP/launch/using/reference/manage-resources/rules.html)」を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

インターフェイスでは、拡張機能の設定、アップグレードおよび削除を行うことができ [!DNL Launch] ます。

>[!TIP]
>
>拡張機能が既にいずれかのプロパティにインストールされている場合、Adobe Real-time CDP UIには、拡張機能の **[!UICONTROL Install]** と表示されます。 「 [Install extension](#install-extension) 」の説明に従って、インストールワークフローを開始し、拡張機能を設定または削除し [!DNL Launch] ます。

拡張機能をアップグレードするには、ドキュメントの [拡張機能のアップグレード](https://docs.adobe.com/content/help/en/launch/using/reference/manage-resources/extensions/extension-upgrade.html) ( [!DNL Launch] Extension upgrade)を参照してください。