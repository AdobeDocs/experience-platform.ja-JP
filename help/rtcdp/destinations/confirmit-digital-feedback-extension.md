---
keywords: Confirmit Digital Feedback;confirmit extension;confirmit
title: Confirmit Digital Feedback 拡張機能
seo-title: Confirmit Digital Feedback 拡張機能
description: Confirmit Digital Feedback 拡張機能は、アドビのリアルタイム顧客データプラットフォームの顧客の声マネジメントの宛先です。拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
seo-description: null
translation-type: tm+mt
source-git-commit: 2dfa46906374151628d46c309df724a59f8dc50e
workflow-type: tm+mt
source-wordcount: '562'
ht-degree: 84%

---


# [!DNL Confirmit Digital Feedback]拡張子

## 概要 {#overview}

[!DNL Confirmit Digital Feedback] ソリューションを使用すると、webサイトのトラフィックをリアルタイムのインサイトに変換できます。 With [!DNL Confirmit], unobtrusive and highly-targeted surveys can be displayed according to your requirements, encouraging visitors to provide feedback, such as:

* Web サイトのフィードバック
* 取引満足度
* 終了インテントの理由
* 買い物かご放棄情報
* 全体的な顧客満足度
* その他多数

[!DNL Confirmit] Digital Feedback は、アドビのリアルタイム顧客データプラットフォームの顧客の声マネジメント拡張機能です。拡張機能について詳しくは、[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.103247.confirmit-digital-feedback-for-adobe-launch.html) の拡張機能のページを参照してください。

この宛先は、Experience Platform Launch 拡張機能です。アドビのリアルタイム CDP の Launch の拡張機能について詳しくは、[Experience Platform Launch 拡張機能の概要](/help/rtcdp/destinations/experience-platform-launch-extensions.md)を参照してください。


![Confirmit Digital Feedback 拡張機能](assets/confirmit-digital-feedback-extension.png)

## 前提条件 {#prerequisites}

This extension is available in the [!DNL Destinations] catalog for all customers who have purchased Adobe Real-time CDP.

この拡張機能を使用するには、Experience Platform Launch にアクセスする必要があります。Experience Platform Launch は、Adobe Experience Cloud に付属の付加価値機能として提供されています。組織の管理者に問い合わせて Launch へのアクセス権を取得し、拡張機能のインストールに必要な **[!UICONTROL manage_properties]** 権限を付与するよう管理者に依頼します。

## 拡張機能のインストール {#install-extension}

To install the [!DNL Confirmit] Digital Feedback extension:

1. [AdobeReal-time CDPインターフェイスで](http://platform.adobe.com/)、 **[!UICONTROL Destinations]** / **[!UICONTROL Catalog]**&#x200B;に移動します。
2. カタログから拡張機能を選択するか、検索バーを使用します。
3. Click on the destination to highlight it, then select **[!UICONTROL Configure]** in the right rail. **[!UICONTROL Configure]** コントロールが灰色表示になっている場合は、 **[!UICONTROL manage_properties]** 権限がありません。 [前提条件](#prerequisites)を確認してください。
4. **[!UICONTROL 使用可能な Launch プロパティを選択]**&#x200B;ウィンドウで、拡張機能をインストールする Launch プロパティを選択します。また、Launch で新しいプロパティを作成することもできます。プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについては、Launch ドキュメントの[「プロパティ」ページに関する節](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/admin/companies-and-properties.html#プロパティページ)を参照してください。
5. ワークフローに従って、Launch でインストールを完了します。

拡張機能の設定オプションとインストールのサポートについて詳しくは、[Adobe Exchange の Confirmit Digital Feedback ページ](https://exchange.adobe.com/experiencecloud.details.103247.confirmit-digital-feedback-for-adobe-launch.html)を参照してください。

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