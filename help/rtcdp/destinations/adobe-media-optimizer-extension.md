---
title: Adobe Analytics 拡張機能
seo-title: Adobe Analytics 拡張機能
description: Adobe Analytics 拡張機能は、Adobe リアルタイム顧客データプラットフォームの分析の対象です。拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
seo-description: Adobe Analytics 拡張機能は、アドビのリアルタイム顧客データプラットフォームの分析の対象です。拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
translation-type: tm+mt
source-git-commit: b96286f6a06f0583b45343a513ee64f0025d79a7
workflow-type: tm+mt
source-wordcount: '542'
ht-degree: 77%

---


# Adobe Analytics 拡張機能 {#adobe-analytics-extension}

## 概要 {#overview}

Adobe Analytics は、顧客像を把握し、顧客インテリジェンスを活用してビジネスを導く力をユーザーに提供する、業界最先端のソリューションです。

Adobe Analytics は、アドビのリアルタイム顧客データプラットフォームの分析拡張機能です。拡張機能の詳細については、[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.100156.html) の拡張機能のページを参照してください。

This destination is an [!DNL Experience Platform Launch] extension. For more information about how [!DNL Launch] extensions work in Adobe Real-time CDP, see [Experience Platform Launch extensions overview](/help/rtcdp/destinations/experience-platform-launch-extensions.md).

![Adobe Analytics 拡張機能](/help/rtcdp/destinations/assets/adobe-analytics-extension.png)

## 前提条件 {#prerequisites}

アドビのリアルタイム CDP を購入したすべての顧客は、この拡張機能を機能カタログから利用できます。

To use this extension, you need access to [!DNL Experience Platform Launch]. [!DNL Experience Platform Launch] は、Adobe Experience Cloud に付属の付加価値機能として提供されています。Contact your organization administrator to get access to [!DNL Launch] and ask them to grant you the **[!UICONTROL manage_properties]** permission so you can install extensions.

## 拡張機能のインストール {#install-extension}

Adobe Analytics 拡張機能をインストールするには、以下をおこないます。

1. [アドビのリアルタイム CDP インターフェイス](http://platform.adobe.com/)で、**[!UICONTROL 宛先／カタログ]**&#x200B;に移動します。
2. カタログから拡張機能を選択するか、検索バーを使用します。
3. 宛先をクリックしてハイライトし、右側のパネルで「**[!UICONTROL 拡張機能のインストール]**」を選択します。「**[!UICONTROL 拡張機能のインストール]**」コントロールが灰色表示になっている場合は、**[!UICONTROL manage_properties]** 権限がありません。[前提条件](#prerequisites)を確認してください。
4. **[!UICONTROL 使用可能な プロパティを選択]**&#x200B;ウィンドウで、拡張機能をインストールする Launch プロパティを選択します。[!DNL Launch]You also have the option of creating a new property in [!DNL Launch]. プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについては、 ドキュメントの[「プロパティ」ページに関する節](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/admin/companies-and-properties.html#プロパティページ)を参照してください。[!DNL Launch]
5. The workflow takes you to [!DNL Launch] to complete the installation.

拡張機能の設定オプションについて詳しくは、 ドキュメントの [Adobe Analytics 拡張機能ページ](https://docs.adobe.com/content/help/en/core-services-learn/implementing-in-websites-with-launch/implement-solutions/analytics.html)を参照してください。[!DNL Experience Launch]

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



