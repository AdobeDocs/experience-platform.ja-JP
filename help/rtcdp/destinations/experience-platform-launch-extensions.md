---
keywords: launch extensions;launch extension;launch destinations
title: Experience Platform Launch の拡張機能
seo-title: Experience Platform Launch の拡張機能
description: ' Launch は、アドビが提供する次世代タグ管理機能です。 Launch は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のタグをデプロイおよび管理するためのシンプルな手段を提供します。'
seo-description: ' Launch は、アドビが提供する次世代タグ管理機能です。 Launch は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のタグをデプロイおよび管理するためのシンプルな手段を提供します。'
translation-type: tm+mt
source-git-commit: 15323134f0c626cad2c4e90b3e1c0662cf7e57dd
workflow-type: tm+mt
source-wordcount: '622'
ht-degree: 96%

---


# Experience Platform Launch の拡張機能 {#experience-platform-launch-extensions}

Experience Platform Launch は、アドビが提供する次世代タグ管理機能です。
Launch は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のタグをデプロイおよび管理するためのシンプルな手段を提供します。Launch は、Adobe Experience Cloud に付属の付加価値機能として提供されています。

Experience Platform Launch の機能の概要については、以下のリソースを参照してください。
* Experience Platform Launch の[ドキュメント](https://docs.adobe.com/content/help/ja-JP/launch/using/overview.html)。
* Experience Platform Launch の[クイックスタートビデオ](https://docs.adobe.com/content/help/ja-JP/launch/using/intro/get-started/videos.html)。[Experience Platform Launch の紹介](https://www.youtube.com/embed/rwqqkG1SERU)と[公開プロセスの概要](https://helpx.adobe.com/jp/analytics/how-to/adobe-launch-publishing-process.html)を視聴してから、次のビデオに進んでください。

## アドビのリアルタイム CDP インターフェイスで Launch の拡張機能を探す {#how-to-find-extensions-in-interface}

To find the Launch extensions in the Adobe Real-time CDP interface, browse to **[!UICONTROL Destinations]** > **[!UICONTROL Catalog]** and select **[!UICONTROL Extensions]** in the **[!UICONTROL Types]** filter.

![インターフェイスの「拡張機能」フィルター](/help/rtcdp/destinations/assets/extensions-filter.png)

## Launch の拡張機能の仕組み {#how-extensions-work}

Launch の拡張機能は、イベントの生データを複数の種類の宛先に転送します。この拡張機能は、**イベント転送**&#x200B;タイプの宛先であると考えることができます。これは、宛先プラットフォームと単純に統合された機能で、イベントの生データを転送するだけです。例としては、[Gainsight パーソナライズ機能拡張機能](/help/rtcdp/destinations/gainsight-extension.md)や [Confirmit Voice of the Customer 拡張機能](/help/rtcdp/destinations/confirmit-digital-feedback-extension.md)などがあります。

アドビリアルタイム顧客データプラットフォームの&#x200B;**プロファイル／セグメント書き出し**&#x200B;の宛先では、イベントデータの取得と他のデータソースとの結合、セグメント化の適用、セグメントと認定プロファイルの宛先への書き出しが実行されます。例としては、[Amazon S3 クラウドストレージの宛先](/help/rtcdp/destinations/amazon-s3-destination.md)や [Google Display &amp; Video 360 の広告の宛先](/help/rtcdp/destinations/google-dv360-destination.md)などがあります。

![Experience Platform Launch の拡張機能と他の宛先との比較](/help/rtcdp/destinations/assets/launch-and-other-destinations.png)

## Launch の拡張機能を使用するメリット {#extensions-benefits}

Experience Cloud のお客様は、Experience Platform Launch を無料でご利用になれます。Launch では、使いやすい拡張機能を使用して、web サイト上にタグを簡単に導入できます。この拡張機能は、インストール、設定、更新および削除できます。Launch は、ｗeb サイト上に小さな足跡を残します。このため、ページをすばやく読み込むことができます。

>[!IMPORTANT]
>
>Launch の拡張機能に対してセグメントをアクティブ化することはできませんが、特定の状況でのみイベントデータを転送するルールを設定できます。詳しくは、以下を参照してください。

イベントデータを拡張機能に転送するタイミングを指定する&#x200B;*ルール*&#x200B;を作成できます。この強力な機能により、すべてのインタラクションでイベントデータを送信するのではなく、特定の状況でのみイベントデータを転送できます。詳しくは、[Launch ドキュメント](https://docs.adobe.com/help/ja-JP/launch/using/reference/manage-resources/rules.html)のルールに関するページを参照してください。

## Launch の拡張機能の使用事例 {#extensions-use-cases}

Launch の拡張機能を使用すると、様々な顧客の使用事例に対応できます。Launch の拡張機能の使用事例を以下に示します。

* Facebook ピクセル拡張機能を使用して、web サイトまたはネイティブのアプリケーションデータを Facebook に送信できます。Facebook ピクセルでは、訪問者がサイトまたはアプリケーションのどの部分にアクセスしたかを把握したり、その情報を Facebook に転送したりすることができます。また、Facebook を介して、訪問者を再ターゲティングすることができます。
* web サイトやアプリケーションのイベントデータを Google Analytics に転送してそのデータを分析し、それに基づいて決定を下すことができます。
* Launch で設定したルールに従い、ユーザーがページとどのようにやり取りしているかに基づいて、クライアントサイドの Chatbox アプリを適切なタイミングで有効にすることができます。


## 拡張機能のカテゴリ {#extension-categories}

アドビのリアルタイム CDP では、Launch の拡張機能は次のカテゴリに該当します。

* [広告](/help/rtcdp/destinations/advertising-destinations.md)
* [分析](/help/rtcdp/destinations/analytics-destinations.md)
* [データ管理プラットフォーム](/help/rtcdp/destinations/dmp-destinations.md)
* [電子メールマーケティングの宛先](/help/rtcdp/destinations/email-marketing-destinations.md)
* [パーソナライズ機能](/help/rtcdp/destinations/personalization-destinations.md)
* [調査](/help/rtcdp/destinations/survey-destinations.md)
* [顧客の声](/help/rtcdp/destinations/voice-of-customer-destinations.md)
