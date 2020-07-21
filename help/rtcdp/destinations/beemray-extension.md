---
title: Beemray拡張
seo-title: Beemray拡張
description: Beemray拡張機能は、アドビのリアルタイム顧客データPlatformでパーソナライズを行う場所です。 拡張機能の詳細については、Adobe Exchangeの拡張機能ページを参照してください。
seo-description: Beemray拡張機能は、アドビのリアルタイム顧客データPlatformでパーソナライズを行う場所です。 拡張機能の詳細については、Adobe Exchangeの拡張機能ページを参照してください。
translation-type: tm+mt
source-git-commit: b96286f6a06f0583b45343a513ee64f0025d79a7
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 4%

---


# [!DNL Beemray] 拡張機能 {#beemray-extension}

## 概要 {#overview}

[!DNL Beemray] 状況を考慮して製品の処理を迅速化できます。 洞察を得たり、新しいエクスペリエンスを構築したり、対話を促したり、本当に重要な瞬間に関わることを可能にします。 Beemrayは、機械学習を使用してコンテキストインテリジェンスを自動化します。 Beemrayは、Adobe Experience Cloudと他のテクニカルパートナーに接続します。 すべてがリアルタイムで行われます。 この拡張機能は、サイトに [!DNL Beemray] SDKをインストールします。

Beemrayは、アドビのリアルタイム顧客データPlatformのパーソナライゼーション拡張機能です。 拡張機能の詳細については、 [Adobe Exchangeの拡張機能ページを参照してください](https://exchange.adobe.com/experiencecloud.details.101063.beemray-human-context.html)。

この宛先は [!DNL Experience Platform Launch] 拡張子です。 Adobe Real-time CDPでの [!DNL Launch] 拡張機能の詳細については、 [Experience Platform Launch拡張機能の概要を参照してください](/help/rtcdp/destinations/experience-platform-launch-extensions.md)。

![Beemray拡張](assets/beemray-extension.png)

## 前提条件 {#prerequisites}

この拡張機能は、Adobe Real-time CDPを購入したすべてのお客様の [!DNL Destinations] カタログで入手できます。

この拡張機能を使用するには、にアクセスする必要があり [!DNL Experience Platform Launch]ます。 [!DNL Experience Platform Launch] は、付加価値機能としてAdobe Experience Cloudのお客様に提供されます。 組織の管理者に連絡して、へのアクセス権を取得し [!DNL Launch] 、 **[!UICONTROL manage_properties]** 権限を付与して、拡張機能をインストールできるようにするように依頼します。

## 拡張機能のインストール {#install-extension}

拡張機能をインストールするに [!DNL Beemray] は：

1. [Adobe Real-time CDPインターフェイスで](http://platform.adobe.com/)、 **[!UICONTROL Destinations/Catalogに移動します]**。
2. カタログから拡張子を選択するか、検索バーを使用します。
3. リンク先をクリックしてハイライト表示し、右側のレールで「 **[!UICONTROL Install Extension]** 」を選択します。 「 **[!UICONTROL Install Extension]** 」コントロールが灰色表示になっている場合は、 **[!UICONTROL manage_properties]** 権限がありません。 「 [前提条件](#prerequisites)」を参照してください。
4. 「使用可能な起動プロパティ **[!UICONTROL を選択]** 」ウィンドウで、拡張機能をインストールする [!DNL Launch] プロパティを選択します。 また、で新しいプロパティを作成することもでき [!DNL Launch]ます。 プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについては、ドキュメントの「 [プロパティ」ページのセクション](https://docs.adobe.com/content/help/en/launch/using/reference/admin/companies-and-properties.html#properties-page) で説明 [!DNL Launch] します。
5. ワークフローにより、インストール [!DNL Launch] が完了します。

拡張機能の構成オプションとインストールのサポートについて詳しくは、Adobe Exchangeの [Beemrayページを参照してください](https://exchange.adobe.com/experiencecloud.details.101063.beemray-human-context.html)。

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



