---
keywords: Awin Advertiser mastertag extension;mastertag tag;Awin;awin;AWIN
title: Awin Advertiser Mastertag 拡張機能
seo-title: Awin Advertiser Mastertag 拡張機能
description: Awin Advertiser Mastertag extensionは、リアルタイム顧客データプラットフォームの広告先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
seo-description: Awin Advertiser Mastertag extensionは、リアルタイム顧客データプラットフォームの広告先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
translation-type: tm+mt
source-git-commit: 80db19822551883da272787affb6f7dc9dc3a745
workflow-type: tm+mt
source-wordcount: '547'
ht-degree: 37%

---


# [!DNL Awin Advertiser Mastertag] 拡張機能 {#awin-mastertag-extension}

## 概要 {#overview}

The [!DNL MasterTag] is a JavaScript library containing all functions required for Awin tracking solution and should be unconditionally appended to every page on the site, including the confirmation page, but excluding any page that displays or processes payment information.

[!DNL Awin Advertiser Mastertag] は、リアルタイム顧客データプラットフォームの広告拡張です。 拡張機能について詳しくは、[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.103176.awin-advertiser-mastertag.html) の拡張機能のページを参照してください。

This destination is an [!DNL Experience Platform Launch] extension. For more information about how Launch extensions work in Real-time CDP, see [Experience Platform Launch extensions overview](../launch-extensions/overview.md).

![UI での Awin Advertiser Mastertag 拡張機能](../../assets/catalog/advertising/awin-mastertag/catalog.png)

## 前提条件 {#prerequisites}

This extension is available in the [!DNL Destinations] catalog for all customers who have purchased Real-time CDP.

To use this extension, you need access to [!DNL Adobe Experience Platform Launch]. [!DNL Platform Launch] は、Adobe Experience Cloud に付属の付加価値機能として提供されています。Contact your organization administrator to get access to [!DNL Platform Launch] and ask them to grant you the **[!UICONTROL manage_properties]** permission so you can install extensions.

## 拡張機能のインストール {#install-extension}

拡張機能をインストールするに [!DNL Awin Advertiser Mastertag] は：

In the [Real-time CDP interface](http://platform.adobe.com/), go to **[!UICONTROL Destinations]** > **[!UICONTROL Catalog]**.

カタログから拡張機能を選択するか、検索バーを使用します。

Click on the destination to highlight it, then select **[!UICONTROL Configure]** in the right rail. **[!UICONTROL Configure]** コントロールが灰色表示になっている場合は、 **[!UICONTROL manage_properties]** 権限がありません。 [前提条件](#prerequisites)を確認してください。

In the **[!UICONTROL Select available Platform Launch property]** window, select the [!DNL Platform Launch] property in which you want to install the extension. You also have the option of creating a new property in [!DNL Platform Launch]. プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについては、 ドキュメントの[「プロパティ」ページに関する節](https://experienceleague.adobe.com/docs/launch/using/reference/admin/companies-and-properties.html#properties-page)を参照してください。[!DNL Launch]

The workflow takes you to [!DNL Platform Launch] to complete the installation.

この拡張機能の設定オプションとインストールのサポートについて詳しくは、[Adobe Exchange の Awin Advertiser Mastertag ページ](https://exchange.adobe.com/experiencecloud.details.103176.awin-advertiser-mastertag.html)を参照してください。

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
