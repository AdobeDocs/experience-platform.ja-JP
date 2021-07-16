---
keywords: Analytics拡張機能， analytics拡張機能，宛先分析
title: Adobe Analytics 拡張機能
description: Adobe Analytics拡張機能は、Adobe Experience Platformの分析の宛先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: 95b6e079-09a6-4262-bd01-11f155286aa9
source-git-commit: 8dfb7bdc16d0654ee1d76dc5f5af50938b122d33
workflow-type: tm+mt
source-wordcount: '499'
ht-degree: 42%

---

# Adobe Analytics 拡張機能

## 概要 {#overview}

Adobe Analytics は、顧客像を把握し、顧客インテリジェンスを活用してビジネスを導く力をユーザーに提供する、業界最先端のソリューションです。

Adobe Analyticsは、Adobe Experience Platformのanalytics拡張機能です。 拡張機能について詳しくは、[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.100156.html) の拡張機能のページを参照してください。

この宛先は[!DNL Adobe Experience Platform Launch]拡張です。 Platformでの[!DNL Platform Launch]Experience Platform Launchの動作について詳しくは、「[拡張機能の概要](../launch-extensions/overview.md)」を参照してください。

![Adobe Analytics 拡張機能](../../assets/catalog/analytics/adobe-analytics/catalog.png)

## 前提条件 {#prerequisites}

この拡張機能は、Platformを購入したすべての顧客の宛先カタログで利用できます。

この拡張機能を使用するには、[!DNL Experience Platform Launch]にアクセスする必要があります。 [!DNL Experience Platform Launch] は、Adobe Experience Cloud に付属の付加価値機能として提供されています。組織の管理者に問い合わせて[!DNL Launch]へのアクセス権を取得し、拡張機能のインストールに必要な&#x200B;**[!UICONTROL manage_properties]**&#x200B;権限を付与するよう管理者に依頼します。

## 拡張機能のインストール {#install-extension}

Adobe Analytics 拡張機能をインストールするには、以下をおこないます。

[Platformインターフェイス](http://platform.adobe.com/)で、**[!UICONTROL 宛先]**/**[!UICONTROL カタログ]**&#x200B;に移動します。

カタログから拡張機能を選択するか、検索バーを使用します。

宛先をクリックしてハイライトし、右側のレールで「**[!UICONTROL 設定]**」を選択します。 **[!UICONTROL Configure]**&#x200B;コントロールがグレー表示になっている場合は、**[!UICONTROL manage_properties]**&#x200B;権限がありません。 [前提条件](#prerequisites)を確認してください。

**[!UICONTROL 使用可能なPlatform launchプロパティを選択]**&#x200B;ウィンドウで、拡張機能をインストールする[!DNL Platform Launch]プロパティを選択します。 また、[!DNL Platform Launch]に新しいプロパティを作成することもできます。 プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについては、 ドキュメントの[「プロパティ」ページに関する節](https://experienceleague.adobe.com/docs/launch/using/reference/admin/companies-and-properties.html#properties-page)を参照してください。[!DNL Launch]

ワークフローで[!DNL Platform Launch]が開き、インストールが完了します。

拡張機能の設定オプションについて詳しくは、Experience ドキュメントの [Adobe Analytics 拡張機能ページ](https://experienceleague.adobe.com/docs/launch-learn/implementing-in-websites-with-launch/implement-solutions/analytics.html)を参照してください。[!DNL Launch]

拡張機能は、[Adobe Experience Platform Launchインターフェイス](https://launch.adobe.com/)に直接インストールすることもできます。  ドキュメントの[新しい拡張機能の追加](https://experienceleague.adobe.com/docs/launch/using/reference/manage-resources/extensions/overview.html?lang=en#add-a-new-extension)に関するページを参照してください。[!DNL Platform Launch]

## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、[!DNL Platform Launch]で直接拡張機能のルールを設定できます。

[!DNL Platform Launch]では、特定の状況でのみ拡張機能の宛先にイベントデータを送信するように、インストールした拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、[ルールに関するドキュメント](https://experienceleague.adobe.com/docs/launch/using/reference/manage-resources/rules.html?lang=ja)を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

[!DNL Platform Launch]インターフェイスで、拡張機能の設定、アップグレード、削除を行うことができます。

>[!TIP]
>
>拡張機能が既にいずれかのプロパティにインストールされている場合、Platform UIには、拡張機能に対して&#x200B;**[!UICONTROL Install]**&#x200B;が表示されます。 「[拡張機能のインストール](#install-extension)」の説明にしたがってインストールワークフローを開始し、 に移動して拡張機能を設定または削除します。[!DNL Platform Launch]

拡張機能をアップグレードするには、 ドキュメントの「[拡張機能のアップグレード](https://experienceleague.adobe.com/docs/launch/using/reference/manage-resources/extensions/extension-upgrade.html)」を参照してください。[!DNL Platform Launch]
