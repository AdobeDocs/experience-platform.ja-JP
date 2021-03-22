---
Keywords: ECID;ecid
title: Experience Cloud ID サービス拡張機能
description: Experience CloudIDサービス拡張は、Adobe Experience Platformのパーソナライズ先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
translation-type: tm+mt
source-git-commit: 709908196bb5df665c7e7df10dc58ee9f3b0edbf
workflow-type: tm+mt
source-wordcount: '501'
ht-degree: 35%

---


# [!DNL Experience Cloud] IDサービスの拡張  {#adobe-ecid-extension}

## 概要 {#overview}

この拡張機能は、すべての[!DNL Experience Cloud]ソリューションの訪問者を識別する[!DNL Experience Cloud] IDサービスを実装します。

[!DNL Experience Cloud] IDサービスは、Adobe Experience Platformのパーソナライゼーション拡張機能です。拡張機能について詳しくは、 ドキュメントの [Experience Cloud ID サービス拡張機能ページ](https://experienceleague.adobe.com/docs/launch/using/extensions-ref/adobe-extension/id-service-extension/overview.html)を参照してください。[!DNL Experience Platform Launch]

この宛先は[!DNL Adobe Experience Platform Launch]拡張子です。 プラットフォームでの[!DNL Platform Launch]拡張機能の動作について詳しくは、[Experience Platform Launch拡張機能の概要](../launch-extensions/overview.md)を参照してください。

![Adobe ECID 拡張機能](../../assets/catalog/personalization/adobe-ecid/catalog.png)

## 前提条件 {#prerequisites}

この拡張機能は、プラットフォームを購入したすべての顧客のDestinationsカタログで利用できます。

この拡張機能を使用するには、[!DNL Experience Platform Launch]にアクセスする必要があります。 [!DNL Experience Platform Launch] は、Adobe Experience Cloud に付属の付加価値機能として提供されています。組織の管理者に連絡して、[!DNL Launch]へのアクセス権を取得し、**[!UICONTROL manage_properties]**&#x200B;の権限を与えて、拡張機能をインストールするように依頼してください。

## 拡張機能のインストール {#install-extension}

[!DNL Experience Cloud] IDサービス拡張機能をインストールするには：

[プラットフォームインターフェイス](http://platform.adobe.com/)で、**[!UICONTROL 宛先]**/**[!UICONTROL カタログ]**&#x200B;に移動します。

カタログから拡張機能を選択するか、検索バーを使用します。

リンク先をクリックしてハイライトし、右側のパネルで「**[!UICONTROL 設定]**」を選択します。 **[!UICONTROL Configure]**&#x200B;コントロールがグレー表示になっている場合は、**[!UICONTROL manage_properties]**&#x200B;権限がありません。 [前提条件](#prerequisites)を確認してください。

**[!UICONTROL 使用可能なPlatform launchプロパティを選択]**&#x200B;ウィンドウで、拡張機能をインストールする[!DNL Platform Launch]プロパティを選択します。 また、[!DNL Platform Launch]に新しいプロパティを作成するオプションもあります。 プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについては、 ドキュメントの[「プロパティ」ページに関する節](https://experienceleague.adobe.com/docs/launch/using/reference/admin/companies-and-properties.html#properties-page)を参照してください。[!DNL Launch]

このワークフローにより、[!DNL Platform Launch]に移動してインストールを完了します。

拡張機能の設定オプションとインストールのサポートについて詳しくは、 ドキュメントの [Experience Cloud ID サービスの拡張機能ページ](https://experienceleague.adobe.com/docs/launch/using/extensions-ref/adobe-extension/id-service-extension/overview.html)を参照してください。[!DNL Experience Launch]

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