---
keywords: Awin Advertiser Conversion Tag extension;conversion tag;Awin;awin;AWIN
title: Awin Advertiser コンバージョンタグの拡張
seo-title: Awin Advertiser コンバージョンタグの拡張
description: Awin Advertiser コンバージョンタグの拡張は、Adobe リアルタイム顧客データプラットフォームの広告先です。拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
seo-description: Awin Advertiser コンバージョンタグの拡張は、Adobe リアルタイム顧客データプラットフォームの広告先です。拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
translation-type: tm+mt
source-git-commit: d9bf874dbfcc00c0a6e267f1a2e96f1223054825
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 65%

---


# Awin Advertiser コンバージョンタグの拡張 {#awin-conversiontag-extension}

## 概要 {#overview}

コンバージョンタグは、AWIN.Tracking.Sale JavaScript オブジェクトの宣言です。この宣言は、確認ページで実行され、コンバージョンがおこなわれたことをマスタータグに指示します。その後、必要なトラッキングリクエストが実行されます。

Awin Advertiser コンバージョンタグは、Adobe リアルタイム顧客データプラットフォームの広告先です。拡張機能の詳細については、「[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.103240.awin-conversion-tag.html) の拡張機能」のページを参照してください。

This destination is an [!DNL Adobe Experience Platform Launch] extension. For more information about how [!DNL Platform Launch] extensions work in Adobe Real-time CDP, see [Experience Platform Launch extensions overview](/help/rtcdp/destinations/experience-platform-launch-extensions.md).

![UI の Awin Advertiser コンバージョン拡張](/help/rtcdp/destinations/assets/awin-conversiontag-extension.png)

## 前提条件 {#prerequisites}

アドビのリアルタイム CDP を購入したすべての顧客は、この拡張機能を機能カタログから利用できます。

To use this extension, you need access to [!DNL Experience Platform Launch]. [!DNL Experience Platform Launch] は、Adobe Experience Cloud に付属の付加価値機能として提供されています。Contact your organization administrator to get access to [!DNL Launch] and ask them to grant you the **[!UICONTROL manage_properties]** permission so you can install extensions.

## 拡張機能のインストール {#install-extension}

拡張機能をインストールするに [!DNL Awin Advertiser Conversion Tag] は：

1. [AdobeReal-time CDPインターフェイスで](http://platform.adobe.com/)、 **[!UICONTROL Destinations]** / **[!UICONTROL Catalog]**&#x200B;に移動します。
2. カタログから拡張機能を選択するか、検索バーを使用します。
3. Click on the destination to highlight it, then select **[!UICONTROL Configure]** in the right rail. **[!UICONTROL Configure]** コントロールが灰色表示になっている場合は、 **[!UICONTROL manage_properties]** 権限がありません。 [前提条件](#prerequisites)を確認してください。
4. In the **[!UICONTROL Select available Platform Launch property]** window, select the [!DNL Platform Launch] property in which you want to install the extension. You also have the option of creating a new property in [!DNL Platform Launch]. プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについては、 ドキュメントの[「プロパティ」ページに関する節](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/admin/companies-and-properties.html#プロパティページ)を参照してください。[!DNL Launch]
5. The workflow takes you to [!DNL Platform Launch] to complete the installation.

拡張機能の設定オプションとインストールのサポートについて詳しくは、「[Adobe Exchange の Awin Advertiser コンバージョンタグページ](https://exchange.adobe.com/experiencecloud.details.103240.awin-conversion-tag.html)」を参照してください。

You can also install the extension directly in the [Adobe Experience Platform Launch interface](https://launch.adobe.com/).  ドキュメントの[新しい拡張機能の追加](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/manage-resources/extensions/overview.html#add-a-new-extension)に関するページを参照してください。[!DNL Platform Launch]


## 拡張機能の使用方法 {#how-to-use}

Once you have installed the extension, you can start setting up rules for it directly in [!DNL Platform Launch].

In [!DNL Platform Launch], you can set up rules for your installed extensions to send event data to the extension destination only in certain situations. 拡張機能のルールの設定について詳しくは、[ルールに関するドキュメント](https://docs.adobe.com/help/ja-JP/launch/using/reference/manage-resources/rules.html)を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

You can configure, upgrade, and delete extensions in the [!DNL Platform Launch] interface.

>[!TIP]
>
>拡張機能が既にいずれかのプロパティにインストールされている場合、アドビのリアルタイム CDP UI には、拡張機能に対して「**[!UICONTROL インストール]**」が表示されます。「[拡張機能のインストール](#install-extension)」の説明にしたがってインストールワークフローを開始し、 に移動して拡張機能を設定または削除します。[!DNL Platform Launch]

拡張機能をアップグレードするには、 ドキュメントの「[拡張機能のアップグレード](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/manage-resources/extensions/extension-upgrade.html)」を参照してください。[!DNL Platform Launch]
