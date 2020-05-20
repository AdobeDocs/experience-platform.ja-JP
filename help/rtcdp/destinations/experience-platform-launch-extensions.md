---
title: エクスペリエンスプラットフォーム起動拡張
seo-title: エクスペリエンスプラットフォーム起動拡張
description: ' Launch は、アドビが提供する次世代タグ管理機能です。Launch は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のタグをデプロイおよび管理するためのシンプルな手段を提供します。'
seo-description: ' Launch は、アドビが提供する次世代タグ管理機能です。Launch は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のタグをデプロイおよび管理するためのシンプルな手段を提供します。'
translation-type: tm+mt
source-git-commit: 98c3356db178507e0a8d94b47030e9490e721e46
workflow-type: tm+mt
source-wordcount: '622'
ht-degree: 22%

---


# Experience Platform Launch extensions {#experience-platform-launch-extensions}

Experience Platform Launch は、アドビが提供する次世代タグ管理機能です。Launch は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のタグをデプロイおよび管理するためのシンプルな手段を提供します。Adobe Experience Cloudのお客様に対して、付加価値機能の提供を開始します。

エクスペリエンスプラットフォームの起動機能の概要については、次のリソースを参照してください。
* Experience Platform Launch [documentation](https://docs.adobe.com/content/help/ja-JP/launch/using/overview.html)
* エクスペリエンスプラットフォーム起動 [のクイック開始ビデオ](https://docs.adobe.com/content/help/en/launch/using/intro/get-started/videos.html)。 エクスペリエンスプラットフォームの起動 [](https://www.youtube.com/embed/rwqqkG1SERU) / [公開プロセスの概要に関する開始を参照して](https://helpx.adobe.com/jp/analytics/how-to/adobe-launch-publishing-process.html)、次の概念に進みます。

## Adobe Real-time CDPインターフェイスでのLaunch拡張機能の検索方法 {#how-to-find-extensions-in-interface}

Adobe Real-time CDPインターフェイスでLaunch拡張機能を探すには、 **[!UICONTROL Destinations/Catalog]** を選択し、 **[!UICONTROL Types]** フィルタで「Extensions **** 」を選択します。

![インターフェイスの拡張フィルター](/help/rtcdp/destinations/assets/extensions-filter.png)

## 起動の拡張機能の仕組み {#how-extensions-work}

起動拡張機能は、生のイベントデータを複数のタイプの宛先に転送します。 拡張機能は、宛先の **イベント転送** (Destination)タイプと考えてください。 これは、生のイベントデータのみを転送する、宛先プラットフォームとの統合のよりシンプルなタイプです。 例えば、 [Gainsightパーソナライゼーション拡張機能](/help/rtcdp/destinations/gainsight-extension.md) 、またはCustomer拡張機能の [Confirmit Voice](/help/rtcdp/destinations/confirmit-digital-feedback-extension.md)。

**Adobe Real-time Customer Data Platformのプロファイル/セグメントエクスポート** :イベントデータの取得、他のデータソースとの組み合わせ、セグメント化の適用、セグメント化の適用、および条件を満たすプロファイルのエクスポートを行います。 例えば、 [Amazon S3クラウドのストレージ先](/help/rtcdp/destinations/amazon-s3-destination.md) 、 [Google Display &amp; Video 360の広告先などがあります](/help/rtcdp/destinations/google-dv360-destination.md)。

![他の宛先と比較したエクスペリエンスプラットフォーム起動の拡張](/help/rtcdp/destinations/assets/launch-and-other-destinations.png)

## 起動拡張機能を使用する利点 {#extensions-benefits}

エクスペリエンスプラットフォームの発売は、既存のExperience Cloudのお客様は無料です。 「起動」を使用すると、インストール、設定、更新、削除が可能な使いやすい拡張機能により、Webサイトでのタグの導入が簡単になります。 Launchは、Webサイトの設置面積が小さく、ページの読み込みを素早く維持できます。

>[!IMPORTANT]
>
>セグメントをアクティブ化して拡張機能を起動することはできませんが、特定の状況でイベントデータを転送するだけのルールを設定できます。 詳しくは、以下を参照してください。

イベントデータをいつ拡張に転送するかを決定する *ルール* を作成できます。 この強力な機能により、すべてのインタラクションでイベントデータを送信するのではなく、特定の状況でのみイベントデータを転送できます。 For more information, read about rules in the [Launch documentation](https://docs.adobe.com/help/ja-JP/launch/using/reference/manage-resources/rules.html).

## 起動拡張機能の使用例の例 {#extensions-use-cases}

起動拡張機能を使用すると、様々な顧客の使用例を満たすことができます。 Launch拡張機能の使用例を以下に示します。

* Facebookピクセル拡張子を使用して、WebサイトまたはネイティブのアプリケーションデータをFacebookに送信できます。 Facebookピクセルは、訪問者がナビゲートしたサイトまたはアプリのどの部分にその情報を転送したかを示し、Facebookに転送します。Facebookを使用して訪問者のターゲットを再設定できます。
* WebサイトやアプリのイベントデータをGoogle Analyticsに転送して、そのデータに基づいて分析や意思決定を行うことができます。
* 「起動」で設定したルールに従って、ユーザーがページとどのようにやり取りしているかに基づいて、適切なタイミングでクライアント側のchatboxアプリをオンにすることができます。


## 拡張カテゴリ {#extension-categories}

Launch extensionsは、Adobe Real-time CDPでは次のカテゴリに該当する場合があります。

* [広告](/help/rtcdp/destinations/advertising-destinations.md)
* [Analytics](/help/rtcdp/destinations/analytics-destinations.md)
* [データ管理プラットフォーム](/help/rtcdp/destinations/dmp-destinations.md)
* [電子メールマーケティングの宛先 ](/help/rtcdp/destinations/email-marketing-destinations.md)
* [パーソナライズ機能](/help/rtcdp/destinations/personalization-destinations.md)
* [調査](/help/rtcdp/destinations/survey-destinations.md)
* [お客様の声](/help/rtcdp/destinations/voice-of-customer-destinations.md)
