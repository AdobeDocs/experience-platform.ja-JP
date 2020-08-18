---
title: Bing Ads ユニバーサルイベントトラッキング（UET）拡張機能
seo-title: Bing Ads ユニバーサルイベントトラッキング（UET）拡張機能
description: Bing Ads ユニバーサルイベントトラッキング（UET）拡張機能は、アドビのリアルタイム顧客データプラットフォームの広告の宛先です。拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
seo-description: null
translation-type: tm+mt
source-git-commit: 33eba9e3f2e993c6958480b091ff004dc057f438
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 61%

---


# [!DNL Bing Ads Universal Event Tracking] (UET)拡張子 {#bing-ads-extension}

## 概要 {#overview}

[!DNL Bing Ads Universal Event Tracking] (UET) for [!DNL Experience Platform Launch] は、誰かが検索広告をクリックした後に何が起きるかを追跡するのに便利です。 1 つの UET タグを使用して Web サイトでの顧客の行動を記録することによってそのデータを活用し、リマーケティングリストを使用してコンバージョンやターゲットオーディエンスを追跡できます。

[!DNL Bing Ads Universal Event Tracking] (UET)は、Adobeリアルタイム顧客データプラットフォームの広告拡張機能です。 拡張機能について詳しくは、[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.100154.html) の拡張機能のページを参照してください。

This destination is an [!DNL Experience Platform Launch] extension. For more information about how [!DNL Launch] extensions work in Adobe Real-time CDP, see [Experience Platform Launch extensions overview](/help/rtcdp/destinations/experience-platform-launch-extensions.md).

![Bing Ads 拡張](assets/bing-ads-extension.png)


## 前提条件 {#prerequisites}

This extension is available in the [!DNL Destinations] catalog for all customers who have purchased Adobe Real-time CDP.

To use this extension, you need access to [!DNL Experience Platform Launch]. [!DNL Experience Platform Launch] は、Adobe Experience Cloud に付属の付加価値機能として提供されています。Contact your organization administrator to get access to [!DNL Launch] and ask them to grant you the **[!UICONTROL manage_properties]** permission so you can install extensions.

## 拡張機能のインストール {#install-extension}

To install the [!DNL Bing Ads Universal Event Tracking] (UET) extension:

1. [AdobeReal-time CDPインターフェイスで](http://platform.adobe.com/)、 **[!UICONTROL Destinations]** / **[!UICONTROL Catalog]**&#x200B;に移動します。
2. カタログから拡張機能を選択するか、検索バーを使用します。
3. Click on the destination to highlight it, then select **[!UICONTROL Configure]** in the right rail. **[!UICONTROL Configure]** コントロールが灰色表示になっている場合は、 **[!UICONTROL manage_properties]** 権限がありません。 [前提条件](#prerequisites)を確認してください。
4. **[!UICONTROL 使用可能な プロパティを選択]**&#x200B;ウィンドウで、拡張機能をインストールする Launch プロパティを選択します。[!DNL Launch]You also have the option of creating a new property in [!DNL Launch]. プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについては、 ドキュメントの[「プロパティ」ページに関する節](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/admin/companies-and-properties.html#プロパティページ)を参照してください。[!DNL Launch]
5. The workflow takes you to [!DNL Launch] to complete the installation.

拡張機能の設定オプションとインストールのサポートについて詳しくは、[Adobe Exchange の Bing Ads ユニバーサルイベントトラッキング（UET）のページ](https://exchange.adobe.com/experiencecloud.details.100154.html)を参照してください。

拡張機能は、[Experience Platform Launch インターフェイス](https://launch.adobe.com/)に直接インストールすることもできます。 ドキュメントの[新しい拡張機能の追加](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/manage-resources/extensions/overview.html#add-a-new-extension)に関するページを参照してください。[!DNL Launch]


## 拡張機能の使用方法 {#how-to-use}

Once you have installed the extension, you can start setting up rules for it directly in [!DNL Launch].

In [!DNL Launch], you can set up rules for your installed extensions to send event data to the extension destination only in certain situations. 拡張機能のルールの設定について詳しくは、[ルールに関するドキュメント](https://docs.adobe.com/help/ja-JP/launch/using/reference/manage-resources/rules.html)を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

You can configure, upgrade, and delete extensions in the [!DNL Launch] interface.

>[!TIP]
>
>拡張機能が既にいずれかのプロパティにインストールされている場合、アドビのリアルタイム CDP UI には、拡張機能に対して「**[!UICONTROL インストール]**」が表示されます。「[拡張機能のインストール](#install-extension)」の説明にしたがってインストールワークフローを開始し、 に移動して拡張機能を設定または削除します。[!DNL Launch]

拡張機能をアップグレードするには、 ドキュメントの「[拡張機能のアップグレード](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/manage-resources/extensions/extension-upgrade.html)」を参照してください。[!DNL Launch]
