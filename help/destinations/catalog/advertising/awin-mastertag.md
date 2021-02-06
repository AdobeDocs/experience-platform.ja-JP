---
keywords: Awin Advertiser mastertag extension;mastertag;Awin;awin;AWIN
title: Awin広告主マスタータグ拡張の宛先
description: Awin Advertiser Mastertag extensionは、Adobe Experience Platformの広告先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
translation-type: tm+mt
source-git-commit: 6655714d4b57d9c414cd40529bcee48c7bcd862d
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 34%

---


# [!DNL Awin Advertiser Mastertag] 拡張機能 {#awin-mastertag-extension}

[!DNL MasterTag]はAwinトラッキングソリューションに必要なすべての関数を含むJavaScriptライブラリで、確認ページを含め、支払い情報を表示または処理するページを除き、サイト上のすべてのページに無条件に追加する必要があります。

[!DNL Awin Advertiser Mastertag] はAdobe Experience Platformの広告の延長です。拡張機能について詳しくは、[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.103176.awin-advertiser-mastertag.html) の拡張機能のページを参照してください。

この宛先は[!DNL Experience Platform Launch]拡張子です。 Launch拡張機能がプラットフォームでどのように機能するかについて詳しくは、[Experience Platform Launch拡張機能の概要](../launch-extensions/overview.md)を参照してください。

![UI での Awin Advertiser Mastertag 拡張機能](../../assets/catalog/advertising/awin-mastertag/catalog.png)

## 前提条件 {#prerequisites}

この拡張機能は、[!DNL Destinations]カタログにあり、プラットフォームを購入したすべてのお客様に提供されます。

この拡張機能を使用するには、[!DNL Adobe Experience Platform Launch]にアクセスする必要があります。 [!DNL Platform Launch] は、Adobe Experience Cloud に付属の付加価値機能として提供されています。組織の管理者に連絡して、[!DNL Platform Launch]へのアクセス権を取得し、**[!UICONTROL manage_properties]**&#x200B;の権限を与えて、拡張機能をインストールするように依頼してください。

## 拡張機能のインストール {#install-extension}

[!DNL Awin Advertiser Mastertag]拡張機能をインストールするには：

[プラットフォームインターフェイス](http://platform.adobe.com/)で、**[!UICONTROL 宛先]**/**[!UICONTROL カタログ]**&#x200B;に移動します。

カタログから拡張機能を選択するか、検索バーを使用します。

リンク先をクリックしてハイライトし、右側のパネルで「**[!UICONTROL 設定]**」を選択します。 **[!UICONTROL Configure]**&#x200B;コントロールがグレー表示になっている場合は、**[!UICONTROL manage_properties]**&#x200B;権限がありません。 [前提条件](#prerequisites)を確認してください。

**[!UICONTROL 利用可能なプラットフォームの起動プロパティを選択]**&#x200B;ウィンドウで、拡張機能をインストールする[!DNL Platform Launch]プロパティを選択します。 また、[!DNL Platform Launch]に新しいプロパティを作成するオプションもあります。 プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについては、 ドキュメントの[「プロパティ」ページに関する節](https://experienceleague.adobe.com/docs/launch/using/reference/admin/companies-and-properties.html#properties-page)を参照してください。[!DNL Launch]

このワークフローにより、[!DNL Platform Launch]に移動してインストールを完了します。

この拡張機能の設定オプションとインストールのサポートについて詳しくは、[Adobe Exchange の Awin Advertiser Mastertag ページ](https://exchange.adobe.com/experiencecloud.details.103176.awin-advertiser-mastertag.html)を参照してください。

[Adobe Experience Platform Launchインターフェイス](https://launch.adobe.com/)に直接インストールすることもできます。  ドキュメントの[新しい拡張機能の追加](https://experienceleague.adobe.com/docs/launch/using/reference/manage-resources/extensions/overview.html?lang=en#add-a-new-extension)に関するページを参照してください。[!DNL Platform Launch]


## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールすると、[!DNL Platform Launch]に直接ルールを設定する開始を作成できます。

[!DNL Platform Launch]では、特定の状況でのみイベントデータを拡張の宛先に送信するように、インストール済みの拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、[ルールに関するドキュメント](https://experienceleague.adobe.com/docs/launch/using/reference/manage-resources/rules.html)を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

[!DNL Platform Launch]インターフェイスでは、拡張機能の設定、アップグレード、削除が可能です。

>[!TIP]
>
>拡張機能が既にプロパティの1つにインストールされている場合、プラットフォームUIには、拡張機能の&#x200B;**[!UICONTROL Install]**&#x200B;が表示されます。 「[拡張機能のインストール](#install-extension)」の説明にしたがってインストールワークフローを開始し、 に移動して拡張機能を設定または削除します。[!DNL Platform Launch]

拡張機能をアップグレードするには、 ドキュメントの「[拡張機能のアップグレード](https://experienceleague.adobe.com/docs/launch/using/reference/manage-resources/extensions/extension-upgrade.html)」を参照してください。[!DNL Platform Launch]
