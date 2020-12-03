---
keywords: launch extensions;launch extension;launch destinations; platform launch extensions;platform launch extension;platform launch destinations
title: Experience Platform Launch の拡張機能
seo-title: Experience Platform Launch の拡張機能
description: ' Launch は、アドビが提供する次世代タグ管理機能です。 Launch は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のタグをデプロイおよび管理するためのシンプルな手段を提供します。'
seo-description: ' Launch は、アドビが提供する次世代タグ管理機能です。 Launch は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のタグをデプロイおよび管理するためのシンプルな手段を提供します。'
translation-type: tm+mt
source-git-commit: 85e6a65e1407ca60e7b63681c045fadaaa24aef9
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 53%

---


# Adobe Experience Platform Launch の拡張機能 {#overview.md}

Adobe Experience Platform Launchは、Adobeが提供する次世代のタグ管理機能です。  Platform Launch は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のタグをデプロイおよび管理するためのシンプルな手段を提供します。 Platform Launch は、Adobe Experience Cloud に付属の付加価値機能として提供されています。

Experience Platform Launch の機能の概要については、以下のリソースを参照してください。
- Adobe Experience Platform Launch [documentation](https://docs.adobe.com/content/help/ja-JP/experience-cloud/user-guides/home.translate.html)
- Adobe Experience Platform Launch [quick start videos](https://experienceleague.adobe.com/docs/launch/using/intro/get-started/videos.html?). Start with [Introduction to Adobe Experience Platform Launch](https://www.youtube.com/embed/rwqqkG1SERU) and [Publishing process overview](https://helpx.adobe.com/jp/analytics/how-to/adobe-launch-publishing-process.html), then move on to the next concepts.

## How to find the Platform Launch extensions in the Real-time CDP interface {#how-to-find-extensions-in-interface}

To find the Platform Launch extensions in the Real-time CDP interface, browse to **[!UICONTROL Destinations]** > **[!UICONTROL Catalog]** and select **[!UICONTROL Extensions]** in the **[!UICONTROL Types]** filter.

![インターフェイスの「拡張機能」フィルター](../../assets/catalog/launch-extensions/filter.png)

## How Platform Launch extensions work {#how-extensions-work}

Platform Launch拡張は、生のイベントデータを複数のタイプの宛先に転送します。 この拡張機能は、**イベント転送**&#x200B;タイプの宛先であると考えることができます。これは、宛先プラットフォームと単純に統合された機能で、イベントの生データを転送するだけです。例としては、[Gainsight パーソナライズ機能拡張機能](../personalization/gainsight.md)や [Confirmit Voice of the Customer 拡張機能](../voice/confirmit-digital-feedback.md)などがあります。

**リアルタイム顧客データプラットフォームのプロファイル/セグメントエクスポート** :イベントデータの取得、他のデータソースとの組み合わせ、セグメント化の適用、セグメント化の適用、および条件を満たすプロファイルのエクスポートを行います。 例としては、[Amazon S3 クラウドストレージの宛先](../cloud-storage/amazon-s3.md)や [Google Display &amp; Video 360 の広告の宛先](../advertising/google-dv360.md)などがあります。

![Experience Platform Launch の拡張機能と他の宛先との比較](../../assets/common/launch-and-other-destinations.png)

## Benefits of using Platform Launch extensions {#extensions-benefits}

Adobe Experience Platform Launchは既存のExperience Cloudのお客様は無料です。 プラットフォームの起動により、導入、設定、更新、削除が可能な使いやすい拡張機能により、Webサイトへのタグの導入が簡単になります。 プラットフォームの起動は、Webサイトの設置面積が小さく、ページの読み込みを素早く維持できます。

>[!IMPORTANT]
>
>Platform Launch Extensionsへのセグメントのアクティブ化はできませんが、特定の状況でイベントデータを転送するだけのルールを設定できます。 詳しくは、以下を参照してください。

イベントデータを拡張機能に転送するタイミングを指定する&#x200B;*ルール*&#x200B;を作成できます。この強力な機能により、すべてのインタラクションでイベントデータを送信するのではなく、特定の状況でのみイベントデータを転送できます。For more information, read about rules in the [Adobe Experience Platform Launch documentation](https://experienceleague.adobe.com/docs/launch/using/reference/manage-resources/rules.html).

## Example use cases for Platform Launch extensions {#extensions-use-cases}

プラットフォーム起動の拡張機能を使用すると、様々な顧客の使用例を満たすことができます。 プラットフォーム起動拡張機能の使用例を以下に示します。

- Facebook ピクセル拡張機能を使用して、web サイトまたはネイティブのアプリケーションデータを Facebook に送信できます。Facebook ピクセルでは、訪問者がサイトまたはアプリケーションのどの部分にアクセスしたかを把握したり、その情報を Facebook に転送したりすることができます。また、Facebook を介して、訪問者を再ターゲティングすることができます。
- web サイトやアプリケーションのイベントデータを Google Analytics に転送してそのデータを分析し、それに基づいて決定を下すことができます。
- プラットフォーム起動で設定したルールに従って、ユーザーがページとどのようにやり取りしたかに基づいて、適切なタイミングでクライアント側のchatboxアプリを有効にできます。

## 拡張機能のカテゴリ {#extension-categories}

Platform Launch拡張は、リアルタイムCDPでは次のカテゴリに該当します。

- [広告](../advertising/overview.md)
- [分析](../analytics/overview.md)
- [データ管理プラットフォーム](../data-management/overview.md)
- [電子メールマーケティングの宛先](../email-marketing/overview.md)
- [パーソナライズ機能](../personalization/overview.md)
- [調査](../survey/overview.md)
- [顧客の声](../voice/overview.md)
