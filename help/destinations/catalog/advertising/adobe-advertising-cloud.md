---
keywords: Advertising Cloud;advertising cloud extension; advertising cloud destination
title: Adobe Advertising Cloud 拡張機能
seo-title: Adobe Advertising Cloud 拡張機能
description: ' Advertising Cloud 拡張機能は、アドビのリアルタイム顧客データプラットフォームの広告先です。拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。'
seo-description: ' Advertising Cloud 拡張機能は、アドビのリアルタイム顧客データプラットフォームの広告先です。拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。'
translation-type: tm+mt
source-git-commit: c24676970629f5a39297001357f8af40895533d9
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 42%

---


# Adobe Advertising Cloud 拡張機能 {#adobe-advertising-cloud-extension}

## 概要 {#overview}

This is the [!DNL Advertising Cloud] extension for implementing [!DNL Advertising Cloud] conversion and segment tags for both the DSP and Search (DCO is not supported currently).

 Advertising Cloud は、アドビのリアルタイム顧客データプラットフォームの広告拡張機能です。

This destination is an [!DNL Adobe Experience Platform Launch] extension. For more information about how [!DNL Platform Launch] extensions work in Real-time CDP, see [Experience Platform Launch extensions overview](../launch-extensions/overview.md).

![Adobe Advertising Cloud 拡張機能](../../assets/catalog/advertising/adobe-advertising-cloud/catalog.png)

## 前提条件 {#prerequisites}

この拡張機能は、リアルタイムCDPを購入したすべてのお客様のDestinationsカタログで利用できます。

To use this extension, you need access to [!DNL Experience Platform Launch]. [!DNL Experience Platform Launch] は、Adobe Experience Cloud に付属の付加価値機能として提供されています。Contact your organization administrator to get access to [!DNL Launch] and ask them to grant you the **[!UICONTROL manage_properties]** permission so you can install extensions.

## 拡張機能のインストール {#install-extension}

Adobe Advertising Cloud 拡張機能をインストールするには、以下を実行します。

In the [Real-time CDP interface](http://platform.adobe.com/), go to **[!UICONTROL Destinations]** > **[!UICONTROL Catalog]**.

カタログから拡張機能を選択するか、検索バーを使用します。

Click on the destination to highlight it, then select **[!UICONTROL Configure]** in the right rail. **[!UICONTROL Configure]** コントロールが灰色表示になっている場合は、 **[!UICONTROL manage_properties]** 権限がありません。 [前提条件](#prerequisites)を確認してください。

In the **[!UICONTROL Select available Platform Launch property]** window, select the [!DNL Platform Launch] property in which you want to install the extension. You also have the option of creating a new property in [!DNL Platform Launch]. プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについては、 ドキュメントの[「プロパティ」ページに関する節](https://experienceleague.adobe.com/docs/launch/using/reference/admin/companies-and-properties.html#properties-page)を参照してください。[!DNL Launch]

The workflow takes you to [!DNL Platform Launch] to complete the installation.

You can also install the extension directly in the [Adobe Experience Platform Launch interface](https://launch.adobe.com/).  ドキュメントの[新しい拡張機能の追加](https://experienceleague.adobe.com/docs/launch/using/reference/manage-resources/extensions/overview.html?lang=en#add-a-new-extension)に関するページを参照してください。[!DNL Platform Launch]


## 拡張機能の使用方法 {#how-to-use}

Once you have installed the extension, you can start setting up rules for it directly in [!DNL Platform Launch].

In [!DNL Platform Launch], you can set up rules for your installed extensions to send event data to the extension destination only in certain situations. 拡張機能のルールの設定について詳しくは、[ルールに関するドキュメント](https://experienceleague.adobe.com/docs/launch/using/reference/manage-resources/rules.html)を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

You can configure, upgrade, and delete extensions in the [!DNL Platform Launch] interface.

>[!TIP]
>
>If the extension is already installed on one of your properties, the Real-time CDP UI still displays **[!UICONTROL Install]** for the extension. 「[拡張機能のインストール](#install-extension)」の説明にしたがってインストールワークフローを開始し、 に移動して拡張機能を設定または削除します。[!DNL Platform Launch]

拡張機能をアップグレードするには、 ドキュメントの「[拡張機能のアップグレード](https://experienceleague.adobe.com/docs/launch/using/reference/manage-resources/extensions/extension-upgrade.html)」を参照してください。[!DNL Platform Launch]
