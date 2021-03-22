---
keywords: Nielsen VideoJS Player Handler;nielsen videojs player;nielsen videojs player;Nielsen;nielsen;Nielsen videojs;Nielsen Digital SDK;nielsen digital SDK
title: Nielsen VideoJS Player Handler 拡張機能
description: ニールセンビデオJSプレイヤーハンドラー拡張機能は、Adobe Experience Platformの分析の対象です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
translation-type: tm+mt
source-git-commit: 709908196bb5df665c7e7df10dc58ee9f3b0edbf
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 34%

---


# [!DNL Nielsen VideoJS Player Handler] 拡張機能 {#nielsen-vjs-extension}

## 概要 {#overview}

[!DNL Nielsen Digital SDK] 以下のデジタル測定製品を使用して、拡張機能オファーのオーディエンス測定を開始します。

DCR：広告を含む非線形デジタルコンテンツの日々の測定を提供する測定ソリューションは、デスクトップ、モバイル、タブレット、および接続されたデバイスにわたるデジタルコンテンツオーディエンスの消費を包括的に表示します。

DTVR：これは、参加しているプログラミングソースのデスクトップおよびモバイルデバイスで発生する線形 TV 視聴を考慮したものです。これは、MRC から認定を受けた最初のソリューションで、コンピューターや携帯端末での視聴プログラミングに対する TV オーディエンス測定への貢献度を示します。

[!DNL Nielsen VideoJS Player Handler] は、Adobe Experience Platformのanalytics拡張です。拡張機能について詳しくは、[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.101361.nielsen-digital-sdk-extension.html) の拡張機能のページを参照してください。

この目的地はAdobe Experience Platform Launchの延長線です。 platform launch拡張機能がプラットフォームでどのように機能するかについて詳しくは、[Adobe Experience Platform Launch拡張機能の概要](../launch-extensions/overview.md)を参照してください。

![Nielsen VideoJS Player Handler 拡張機能](../../assets/catalog/analytics/nielsen-videojs/catalog.png)

## 前提条件 {#prerequisites}

この拡張機能は、[!DNL Destinations]カタログにあり、プラットフォームを購入したすべてのお客様に提供されます。

この拡張機能を使用するには、Adobe Experience Platform Launchにアクセスする必要があります。  Platform Launch は、Adobe Experience Cloud に付属の付加価値機能として提供されています。組織の管理者に連絡して、Platform launchへのアクセス権を取得し、**[!UICONTROL manage_properties]**&#x200B;権限を付与して、拡張機能をインストールするように依頼します。

## 拡張機能のインストール {#install-extension}

[!DNL Nielsen VideoJS Player Handler]拡張機能をインストールするには：

[プラットフォームインターフェイス](http://platform.adobe.com/)で、**[!UICONTROL 宛先]**/**[!UICONTROL カタログ]**&#x200B;に移動します。

カタログから拡張機能を選択するか、検索バーを使用します。

リンク先をクリックしてハイライトし、右側のパネルで「**[!UICONTROL 設定]**」を選択します。 **[!UICONTROL Configure]**&#x200B;コントロールがグレー表示になっている場合は、**[!UICONTROL manage_properties]**&#x200B;権限がありません。 [前提条件](#prerequisites)を確認してください。

**[!UICONTROL 使用可能なPlatform launchプロパティを選択]**&#x200B;ウィンドウで、拡張機能をインストールするPlatform launchプロパティを選択します。 また、Platform launchで新しいプロパティを作成することもできます。 プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。platform launchドキュメントの[プロパティページのセクション](https://experienceleague.adobe.com/docs/launch/using/reference/admin/companies-and-properties.html#properties-page)で、プロパティについて説明します。

platform launchが表示され、インストールが完了します。

拡張機能の設定オプションとインストールのサポートについて詳しくは、[Adobe Exchange の Nielsen Digital SDK ページ](https://exchange.adobe.com/experiencecloud.details.101361.nielsen-digital-sdk-extension.html)を参照してください。

[Adobe Experience Platform Launchインターフェイス](https://launch.adobe.com/)に直接インストールすることもできます。 platform launchマニュアルの[追加新しい拡張子](https://experienceleague.adobe.com/docs/launch/using/reference/manage-resources/extensions/overview.html?lang=en#add-a-new-extension)を参照してください。

## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールすると、Platform launchで直接、拡張機能のルール設定を開始できます。

platform launchでは、特定の状況でのみイベントデータを拡張子の宛先に送信するように、インストール済みの拡張子のルールを設定できます。 拡張機能のルールの設定について詳しくは、[ルールに関するドキュメント](https://experienceleague.adobe.com/docs/launch/using/reference/manage-resources/rules.html)を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

platform launchインターフェイスでは、拡張機能の設定、アップグレードおよび削除を行うことができます。

>[!TIP]
>
>拡張機能が既にプロパティの1つにインストールされている場合、プラットフォームUIには、拡張機能の&#x200B;**[!UICONTROL Install]**&#x200B;が表示されます。 [Platform launchに行き、拡張機能を設定または削除するには、インストール拡張機能](#install-extension)の説明に従って、インストールワークフローを開始します。

拡張機能をアップグレードするには、Platform launchドキュメントの[拡張機能のアップグレード](https://experienceleague.adobe.com/docs/launch/using/reference/manage-resources/extensions/extension-upgrade.html)を参照してください。