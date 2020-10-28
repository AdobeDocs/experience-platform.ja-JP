---
keywords: launch extensions;launch extension;launch destinations; platform launch extensions;platform launch extension;platform launch destinations
title: Experience Platform Launch の拡張機能
seo-title: Experience Platform Launch の拡張機能
description: ' Launch は、アドビが提供する次世代タグ管理機能です。 Launch は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のタグをデプロイおよび管理するためのシンプルな手段を提供します。'
seo-description: ' Launch は、アドビが提供する次世代タグ管理機能です。 Launch は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のタグをデプロイおよび管理するためのシンプルな手段を提供します。'
translation-type: tm+mt
source-git-commit: 6bb7bbeaceac8a69641138248a1c41cf9fb3dc63
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 60%

---


# Adobe Experience Platform Launch の拡張機能 {#experience-platform-launch-extensions}

Adobe Experience Platform Launchは、Adobeが提供する次世代のタグ管理機能です。  Platform Launch は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のタグをデプロイおよび管理するためのシンプルな手段を提供します。 Platform Launch は、Adobe Experience Cloud に付属の付加価値機能として提供されています。

Experience Platform Launch の機能の概要については、以下のリソースを参照してください。
* Adobe Experience Platform Launch [documentation](https://docs.adobe.com/content/help/ja-JP/launch/using/overview.html)
* Adobe Experience Platform Launch [quick start videos](https://docs.adobe.com/content/help/ja-JP/launch/using/intro/get-started/videos.html). Start with [Introduction to Adobe Experience Platform Launch](https://www.youtube.com/embed/rwqqkG1SERU) and [Publishing process overview](https://helpx.adobe.com/jp/analytics/how-to/adobe-launch-publishing-process.html), then move on to the next concepts.

## How to find the Platform Launch extensions in the Adobe Real-time CDP interface {#how-to-find-extensions-in-interface}

To find the Platform Launch extensions in the Adobe Real-time CDP interface, browse to **[!UICONTROL Destinations]** > **[!UICONTROL Catalog]** and select **[!UICONTROL Extensions]** in the **[!UICONTROL Types]** filter.

![インターフェイスの「拡張機能」フィルター](/help/rtcdp/destinations/assets/extensions-filter.png)

## How Platform Launch extensions work {#how-extensions-work}

Platform Launch拡張は、生のイベントデータを複数のタイプの宛先に転送します。 この拡張機能は、**イベント転送**&#x200B;タイプの宛先であると考えることができます。これは、宛先プラットフォームと単純に統合された機能で、イベントの生データを転送するだけです。例としては、[Gainsight パーソナライズ機能拡張機能](/help/rtcdp/destinations/gainsight-extension.md)や [Confirmit Voice of the Customer 拡張機能](/help/rtcdp/destinations/confirmit-digital-feedback-extension.md)などがあります。

アドビリアルタイム顧客データプラットフォームの&#x200B;**プロファイル／セグメント書き出し**&#x200B;の宛先では、イベントデータの取得と他のデータソースとの結合、セグメント化の適用、セグメントと認定プロファイルの宛先への書き出しが実行されます。例としては、[Amazon S3 クラウドストレージの宛先](/help/rtcdp/destinations/amazon-s3-destination.md)や [Google Display &amp; Video 360 の広告の宛先](/help/rtcdp/destinations/google-dv360-destination.md)などがあります。

![Experience Platform Launch の拡張機能と他の宛先との比較](/help/rtcdp/destinations/assets/launch-and-other-destinations.png)

## Benefits of using Platform Launch extensions {#extensions-benefits}

Adobe Experience Platform Launchは既存のExperience Cloudのお客様は無料です。 プラットフォームの起動により、導入、設定、更新、削除が可能な使いやすい拡張機能により、Webサイトへのタグの導入が簡単になります。 プラットフォームの起動は、Webサイトの設置面積が小さく、ページの読み込みを素早く維持できます。

>[!IMPORTANT]
>
>Platform Launch Extensionsへのセグメントのアクティブ化はできませんが、特定の状況でイベントデータを転送するだけのルールを設定できます。 詳しくは、以下を参照してください。

イベントデータを拡張機能に転送するタイミングを指定する&#x200B;*ルール*&#x200B;を作成できます。この強力な機能により、すべてのインタラクションでイベントデータを送信するのではなく、特定の状況でのみイベントデータを転送できます。For more information, read about rules in the [Adobe Experience Platform Launch documentation](https://docs.adobe.com/help/ja-JP/launch/using/reference/manage-resources/rules.html).

## Example use cases for Platform Launch extensions {#extensions-use-cases}

プラットフォーム起動の拡張機能を使用すると、様々な顧客の使用例を満たすことができます。 プラットフォーム起動拡張機能の使用例を以下に示します。

* Facebook ピクセル拡張機能を使用して、web サイトまたはネイティブのアプリケーションデータを Facebook に送信できます。Facebook ピクセルでは、訪問者がサイトまたはアプリケーションのどの部分にアクセスしたかを把握したり、その情報を Facebook に転送したりすることができます。また、Facebook を介して、訪問者を再ターゲティングすることができます。
* web サイトやアプリケーションのイベントデータを Google Analytics に転送してそのデータを分析し、それに基づいて決定を下すことができます。
* プラットフォーム起動で設定したルールに従って、ユーザーがページとどのようにやり取りしたかに基づいて、適切なタイミングでクライアント側のchatboxアプリを有効にできます。


## 拡張機能のカテゴリ {#extension-categories}

AdobeReal-time CDPでは、プラットフォーム起動の拡張は次のカテゴリに該当します。

* [広告](/help/rtcdp/destinations/advertising-destinations.md)
* [分析](/help/rtcdp/destinations/analytics-destinations.md)
* [データ管理プラットフォーム](/help/rtcdp/destinations/dmp-destinations.md)
* [電子メールマーケティングの宛先](/help/rtcdp/destinations/email-marketing-destinations.md)
* [パーソナライズ機能](/help/rtcdp/destinations/personalization-destinations.md)
* [調査](/help/rtcdp/destinations/survey-destinations.md)
* [顧客の声](/help/rtcdp/destinations/voice-of-customer-destinations.md)
