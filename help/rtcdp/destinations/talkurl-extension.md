---
title: Talk URL Inbox 拡張機能
seo-title: Talk URL Inbox
description: Talk URL Inbox 拡張機能は、アドビのリアルタイム顧客データプラットフォームの顧客の声の宛先です。拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
seo-description: Talk URL Inbox 拡張機能は、アドビのリアルタイム顧客データプラットフォームの顧客の声の宛先です。拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
translation-type: tm+mt
source-git-commit: be4cf64c89a189a09a4a7774c8fadc76c6ee8458
workflow-type: tm+mt
source-wordcount: '570'
ht-degree: 94%

---


# [!DNL Talk URL Inbox] 拡張子 {#talk-url-extension}

## 概要 {#overview}

Web ページ訪問者から音声メッセージを受信します。ホスト UI とクラウドベースの機械学習機能を含み、音声メッセージをテキストインサイトへと即座に書き起こします。

[!DNL Talk URL Inbox] は、アドビのリアルタイム顧客データプラットフォームのの顧客の声拡張機能です。拡張機能について詳しくは、[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.100583.html) の拡張機能のページを参照してください。

この宛先は、Experience Platform Launch 拡張機能です。アドビのリアルタイム CDP の Launch の拡張機能について詳しくは、[Experience Platform Launch 拡張機能の概要](/help/rtcdp/destinations/experience-platform-launch-extensions.md)を参照してください。

![Talk URL Inbox 拡張機能](assets/talkurl-extension.png)

## 前提条件 {#prerequisites}

This extension is available in the [!DNL Destinations] catalog for all customers who have purchased Adobe Real-time CDP.

この拡張機能を使用するには、Experience Platform Launch にアクセスする必要があります。Experience Platform Launch は、Adobe Experience Cloud に付属の付加価値機能として提供されています。組織の管理者に問い合わせて Launch へのアクセス権を取得し、拡張機能のインストールに必要な **[!UICONTROL manage_properties]** 権限を付与するよう管理者に依頼します。

## 拡張機能のインストール {#install-extension}

拡張機能をインストールするに [!DNL Talk URL Inbox] は：

1. [AdobeReal-time CDPインターフェイスで](http://platform.adobe.com/)、 **[!UICONTROL Destinations]** / **[!UICONTROL Catalog]**&#x200B;に移動します。
2. カタログから拡張機能を選択するか、検索バーを使用します。
3. 宛先をクリックしてハイライトし、右側のパネルで「**[!UICONTROL 拡張機能のインストール]**」を選択します。「**[!UICONTROL 拡張機能のインストール]**」コントロールが灰色表示になっている場合は、**[!UICONTROL manage_properties]** 権限がありません。[前提条件](#prerequisites)を確認してください。
4. **[!UICONTROL 使用可能な Launch プロパティを選択]**&#x200B;ウィンドウで、拡張機能をインストールする Launch プロパティを選択します。また、Launch で新しいプロパティを作成することもできます。プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについては、Launch ドキュメントの[「プロパティ」ページに関する節](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/admin/companies-and-properties.html#プロパティページ)を参照してください。
5. ワークフローに従って、Launch でインストールを完了します。

拡張機能の設定オプションとインストールのサポートについて詳しくは、[Adobe Exchange の Talk URL Inbox ページ](https://exchange.adobe.com/experiencecloud.details.100583.html)を参照してください。

拡張機能は、[Experience Platform Launch インターフェイス](https://launch.adobe.com/)に直接インストールすることもできます。Launch ドキュメントの[新しい拡張機能の追加](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/manage-resources/extensions/overview.html#add-a-new-extension)に関するページを参照してください。

## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、その拡張機能のルールを Launch で直接設定できます。

Launch では、特定の状況でのみ拡張機能の宛先にイベントデータを送信するように、インストール済みの拡張機能のルールを設定できます。拡張機能のルールの設定について詳しくは、[ルールに関するドキュメント](https://docs.adobe.com/help/ja-JP/launch/using/reference/manage-resources/rules.html)を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

Launch インターフェイスで、拡張機能の設定、アップグレードおよび削除をおこなうことができます。

>[!TIP]
>
>拡張機能が既にいずれかのプロパティにインストールされている場合、アドビのリアルタイム CDP UI には、拡張機能に対して「**[!UICONTROL インストール]**」が表示されます。「[拡張機能のインストール](#install-extension)」の説明にしたがってインストールワークフローを開始し、Launch に移動して拡張機能を設定または削除します。

拡張機能をアップグレードするには、Launch ドキュメントの「[拡張機能のアップグレード](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/manage-resources/extensions/extension-upgrade.html)」を参照してください。