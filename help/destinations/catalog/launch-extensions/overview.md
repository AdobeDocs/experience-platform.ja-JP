---
keywords: launchの拡張機能；launchの拡張機能；launchの宛先；platform launch拡張機能；platform launch拡張；platform launch先
title: Adobe Experience Platform Launch拡張機能
description: Adobe Experience Platform Launch は、アドビが提供する次世代タグ管理機能です。 Platform Launch は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のタグをデプロイおよび管理するためのシンプルな手段を提供します。
exl-id: 54fca635-0e37-460e-abb3-5da294d4e0cf
source-git-commit: 12c3f440319046491054b3ef3ec404798bb61f06
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 50%

---

# Adobe Experience Platform Launch の拡張機能

Adobe Experience Platform Launchは、Adobeの次世代タグ管理機能です。  Platform Launch は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のタグをデプロイおよび管理するためのシンプルな手段を提供します。 Platform Launch は、Adobe Experience Cloud に付属の付加価値機能として提供されています。

Experience Platform Launch の機能の概要については、以下のリソースを参照してください。

- Adobe Experience Platform Launch [documentation](https://experienceleague.adobe.com/docs/launch/using/home.html?lang=ja)
- Adobe Experience Platform Launch [クイックスタートビデオ](../../../tags/quick-start/videos.md)。 [Adobe Experience Platform Launch](https://www.youtube.com/embed/rwqqkG1SERU)と[公開プロセスの概要](https://helpx.adobe.com/jp/analytics/how-to/adobe-launch-publishing-process.html)から始め、次の概念に進みます。

## PlatformインターフェイスでPlatform launch拡張機能を見つける方法 {#how-to-find-extensions-in-interface}

PlatformインターフェイスでPlatform launch拡張機能を探すには、**[!UICONTROL 宛先]** / **[!UICONTROL カタログ]**&#x200B;を参照し、**[!UICONTROL タイプ]**&#x200B;フィルターで&#x200B;**[!UICONTROL 拡張機能]**&#x200B;を選択します。

![インターフェイスの「拡張機能」フィルター](../../assets/catalog/launch-extensions/filter.png)

## platform launch拡張機能の仕組み {#how-extensions-work}

platform launch拡張機能は、イベントの生データを複数のタイプの宛先に転送します。 この拡張機能は、**イベント転送**&#x200B;タイプの宛先であると考えることができます。これは、宛先プラットフォームと単純に統合された機能で、イベントの生データを転送するだけです。例としては、[Gainsight パーソナライズ機能拡張機能](../personalization/gainsight.md)や [Confirmit Voice of the Customer 拡張機能](../voice/confirmit-digital-feedback.md)などがあります。

**Adobe Experience Platformのプロファイル/セ** グメントの書き出し先は、イベントデータをキャプチャし、他のデータソースと組み合わせ、セグメント化を適用し、セグメントと絞り込まれたプロファイルを宛先に書き出します。例としては、[Amazon S3 クラウドストレージの宛先](../cloud-storage/amazon-s3.md)や [Google Display &amp; Video 360 の広告の宛先](../advertising/google-dv360.md)などがあります。

![Experience Platform Launch の拡張機能と他の宛先との比較](../../assets/common/launch-and-other-destinations.png)

## 拡張機能を使用するメリットPlatform launch {#extensions-benefits}

Adobe Experience Platform Launchは、既存のExperience Cloudのお客様は無料です。 platform launchは、使いやすい拡張機能を使用して、Webサイト上にタグを簡単に導入できるようにします。拡張機能は、インストール、設定、更新、削除が可能です。 platform launchは、Webサイト上に小さな足跡を残し、ページをすばやく読み込むことができます。

>[!IMPORTANT]
>
>セグメント拡張機能に対してセグメントをアクティブ化することはできませんが、特定の状況でのみPlatform launchデータを転送するルールを設定できます。 詳しくは、以下を参照してください。

イベントデータを拡張機能に転送するタイミングを指定する&#x200B;*ルール*&#x200B;を作成できます。この強力な機能により、すべてのインタラクションでイベントデータを送信するのではなく、特定の状況でのみイベントデータを転送できます。詳しくは、[Adobe Experience Platform Launchのドキュメント](../../../tags/ui/managing-resources/rules.md)のルールに関するページを参照してください。

## 拡張機能の使用例 {#extensions-use-cases}

platform launch拡張機能を使用すると、様々な顧客の使用例に対応できます。 拡張機能を使用する使用例の一部を次に示します。Platform launch

- Facebook ピクセル拡張機能を使用して、web サイトまたはネイティブのアプリケーションデータを Facebook に送信できます。Facebook ピクセルでは、訪問者がサイトまたはアプリケーションのどの部分にアクセスしたかを把握したり、その情報を Facebook に転送したりすることができます。また、Facebook を介して、訪問者を再ターゲティングすることができます。
- web サイトやアプリケーションのイベントデータを Google Analytics に転送してそのデータを分析し、それに基づいて決定を下すことができます。
- platform launchで設定したルールに従って、ユーザーがページとどのようにやり取りしているかに基づいて、適切なタイミングでクライアント側のChatboxアプリをオンにすることができます。

## 拡張機能のカテゴリ {#extension-categories}

platform launchの拡張機能は、プラットフォームで次のカテゴリに該当する可能性があります。

- [広告](../advertising/overview.md)
- [Analytics](../analytics/overview.md)
- [データ管理プラットフォーム](../data-management/overview.md)
- [電子メールマーケティングの宛先](../email-marketing/overview.md)
- [パーソナライゼーション](../personalization/overview.md)
- [調査](../survey/overview.md)
- [顧客の声](../voice/overview.md)
