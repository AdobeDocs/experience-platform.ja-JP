---
title: エクスペリエンスプラットフォーム起動の拡張
seo-title: エクスペリエンスプラットフォーム起動の拡張
description: ' Launch は、アドビが提供する次世代タグ管理機能です。 Launch は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のタグをデプロイおよび管理するためのシンプルな手段を提供します。'
seo-description: ' Launch は、アドビが提供する次世代タグ管理機能です。 Launch は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のタグをデプロイおよび管理するためのシンプルな手段を提供します。'
translation-type: tm+mt
source-git-commit: 2a082dc46b50eba1a38eb9d6946e17f851b2fd3f

---


# Experience Platform Launch extensions {#experience-platform-launch-extensions}

Experience Platform Launch は、アドビが提供する次世代タグ管理機能です。
Launch は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のタグをデプロイおよび管理するためのシンプルな手段を提供します。Adobe Experience Cloudのお客様に対して、付加価値機能として発売が提供されます。

エクスペリエンスプラットフォームの開始機能の概要については、以下のリソースを参照してください。
* Experience Platform Launch [documentation](https://docs.adobe.com/content/help/ja-JP/launch/using/overview.html)
* エクスペリエンスプラットフォームの起 [動のクイック開始ビデオ](https://docs.adobe.com/content/help/en/launch/using/intro/get-started/videos.html)。 開始とエ [クスペリエンスプラットフォームの起動](https://www.youtube.com/embed/rwqqkG1SERU)[/公開](https://helpx.adobe.com/jp/analytics/how-to/adobe-launch-publishing-process.html)プロセスの概要を参照し、次の概念に進みます。

## Adobe Real-time CDPインターフェイスでの起動の拡張機能の検索方法

Adobe Real-time CDPインターフェイスで起動の拡張機能を探すには、フィルタ **[!UICONTROL Destinations > Catalog]** ーを参照し **[!UICONTROL Extensions]** て選択し **[!UICONTROL Types]** ます。

![インターフェイスの拡張フィルター](/help/rtcdp/destinations/assets/extensions-filter.png)

## 起動拡張機能の仕組み

起動拡張機能は、生のイベントデータを複数のタイプの宛先に転送します。 拡張子は、宛先の **イベント転送** (Destination Forwarding)タイプと考えます。 これは、生のプラットフォームデータの転送のみを行う宛先プラットフォームとの統合のより簡単なタイプでイベントします。 例えば、勝者パーソナライゼ [ーション拡張や](/help/rtcdp/destinations/gainsight-extension.md) 、顧客拡 [張の確認音声などがあります](/help/rtcdp/destinations/confirmit-digital-feedback-extension.md)。

**Adobe Real-time Customer** Data Platformのプロファイル/セグメントのエクスポート先は、イベントデータを取得し、他のデータソースと組み合わせて、セグメントを適用し、セグメントをエクスポートし、セグメントと条件を満たすプロファイルを宛先に適用します。 例えば、 [Amazon S3クラウドのストレージ先や](/help/rtcdp/destinations/amazon-s3-destination.md) 、 [Google Display &amp; Video 360の広告先などです](/help/rtcdp/destinations/google-dv360-destination.md)。

![エクスペリエンスプラットフォーム起動の拡張と他の宛先との比較](/help/rtcdp/destinations/assets/launch-and-other-destinations.png)

## 起動拡張機能を使用する利点

エクスペリエンスプラットフォームの発売は、既存のExperience Cloudのお客様は無料です。 「起動」を使用すると、使いやすい拡張機能を使用して、Webサイト上でのタグの導入が簡単になり、インストール、設定、更新および削除が可能になります。 Launchは、Webサイト上の設置面積が小さく、ページの読み込みを素早く維持できます。

>[!IMPORTANT]
>
>セグメントをアクティブ化して拡張機能を起動することはできませんが、特定の状況でイベントデータを転送するだけのルールを設定できます。 詳しくは、以下を参照してください。

拡張にイベントデータを *転送するタイミングを決定するルールを* 、いつ拡張に転送するかを指定できます。 この強力な機能により、すべてのインタラクションでイベントデータを送信するのとは異なり、特定の状況でのみイベントデータを転送できます。 For more information, read about rules in the [Launch documentation](https://docs.adobe.com/help/ja-JP/launch/using/reference/manage-resources/rules.html).

## Launch拡張機能の使用例

起動の拡張機能を使用すると、様々な顧客の使用例を満たすことができます。 Launch拡張機能を使用する場合の使用例を次に示します。

* Facebookのピクセル拡張子を使用して、WebサイトまたはネイティブのアプリデータをFacebookに送信できます。 「Facebookピクセル」は、訪問者がナビゲートしたサイトまたはアプリのどの部分にその情報を転送するかを示し、Facebookに転送します。Facebookを使用して訪問者のターゲットを再設定できます。
* WebサイトやアプリのイベントデータをGoogle Analyticsに転送して、そのデータを分析し、それに基づいて決定を下すことができます。
* 起動で設定したルールに従って、ユーザーがページとどのようにやり取りしているかに基づいて、適切なタイミングでクライアントサイドのChatboxアプリを有効にすることができます。


## 拡張カテゴリ

Launch extensionsは、Adobe Real-time CDPでは次のカテゴリに該当します。

* [広告](/help/rtcdp/destinations/advertising-destinations.md)
* [Analytics](/help/rtcdp/destinations/analytics-destinations.md)
* [データ管理プラットフォーム](/help/rtcdp/destinations/dmp-destinations.md)
* [電子メールマーケティングの宛先 ](/help/rtcdp/destinations/email-marketing-destinations.md)
* [パーソナライズ機能](/help/rtcdp/destinations/personalization-destinations.md)
* [調査](/help/rtcdp/destinations/survey-destinations.md)
* [顧客の声](/help/rtcdp/destinations/voice-of-customer-destinations.md)
