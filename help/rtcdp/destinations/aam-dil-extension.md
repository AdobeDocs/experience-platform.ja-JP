---
title: Audience Manager DIL 拡張機能
seo-title: Audience Manager DIL 拡張機能
description: Audience Manager DIL 拡張機能は、アドビのリアルタイム顧客データプラットフォームでのデータ管理プラットフォーム（DMP）の宛先です。拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
seo-description: Audience Manager DIL 拡張機能は、アドビのリアルタイム顧客データプラットフォームでのデータ管理プラットフォーム（DMP）の宛先です。拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
translation-type: tm+mt
source-git-commit: be4cf64c89a189a09a4a7774c8fadc76c6ee8458
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 75%

---


# Audience Manager DIL 拡張機能 {#aam-dil-extension}

## 概要 {#overview}

これは、Adobe Audience Manager データ統合ライブラリ拡張機能（クライアントサイド実装）です。メモ：この拡張機能は、Adobe Analytics データのサーバーサイド転送（SSF）で使用するためのものではありません。SSF の場合は、Adobe Analytics 拡張機能を使用します。Important: Starting with version 8.0, DIL has a hard dependency on the [!DNL Experience Cloud] ID Service, version 3.3 or higher. Please implement both [!DNL Experience Cloud] ID Service and DIL for full [!DNL Audience Manager] data integration capabilities.

[!DNL Audience Manager] DIL は、アドビのリアルタイム顧客データプラットフォームのデータ管理プラットフォーム（DMP）拡張機能です。拡張機能について詳しくは、Experience Platform Launch ドキュメントの [Audience Manager 拡張機能ページ](https://docs.adobe.com/content/help/ja-JP/launch/using/extensions-ref/adobe-extension/adobe-audience-manager-extension.html)を参照してください。

This destination is an [!DNL Experience Platform Launch] extension. アドビのリアルタイム CDP の Launch の拡張機能について詳しくは、[Experience Platform Launch 拡張機能の概要](/help/rtcdp/destinations/experience-platform-launch-extensions.md)を参照してください。

![Audience Manager DIL 拡張機能](/help/rtcdp/destinations/assets/aam-dil-extension.png)

## 前提条件 {#prerequisites}

This extension is available in the [!DNL Destinations] catalog for all customers who have purchased Adobe Real-time CDP.

To use this extension, you need access to [!DNL Experience Platform Launch]. [!DNL Experience Platform Launch] は、Adobe Experience Cloud に付属の付加価値機能として提供されています。Contact your organization administrator to get access to [!DNL Launch] and ask them to grant you the **[!UICONTROL manage_properties]** permission so you can install extensions.

## 拡張機能のインストール {#install-extension}

To install the [!DNL Audience Manager] DIL extension:

1. [AdobeReal-time CDPインターフェイスで](http://platform.adobe.com/)、 **[!UICONTROL Destinations]** / **[!UICONTROL Catalog]**&#x200B;に移動します。
2. カタログから拡張機能を選択するか、検索バーを使用します。
3. 宛先をクリックしてハイライトし、右側のパネルで「**[!UICONTROL 拡張機能のインストール]**」を選択します。「**[!UICONTROL 拡張機能のインストール]**」コントロールが灰色表示になっている場合は、**[!UICONTROL manage_properties]** 権限がありません。[前提条件](#prerequisites)を確認してください。
4. **[!UICONTROL 使用可能な プロパティを選択]**&#x200B;ウィンドウで、拡張機能をインストールする Launch プロパティを選択します。[!DNL Launch]また、Launch で新しいプロパティを作成することもできます。プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについては、 ドキュメントの[「プロパティ」ページに関する節](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/admin/companies-and-properties.html#プロパティページ)を参照してください。[!DNL Launch]
5. The workflow takes you to [!DNL Launch] to complete the installation.

拡張機能の設定オプションについて詳しくは、 ドキュメントの [Audience Manager 拡張機能のページ](https://docs.adobe.com/content/help/ja-JP/launch/using/extensions-ref/adobe-extension/adobe-audience-manager-extension.html)を参照してください。[!DNL Experience Launch]

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



