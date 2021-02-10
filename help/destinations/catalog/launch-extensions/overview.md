---
keywords: 起動拡張；起動拡張；起動先；プラットフォーム起動拡張；プラットフォーム起動拡張；プラットフォーム起動先
title: Adobe Experience Platform Launch拡張
description: Adobe Experience Platform Launchは、Adobeが提供する次世代のタグ管理機能です。  Platform Launch は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のタグをデプロイおよび管理するためのシンプルな手段を提供します。
translation-type: tm+mt
source-git-commit: e13a19640208697665b0a7e0106def33fd1e456d
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 46%

---


# Adobe Experience Platform Launch の拡張機能

Adobe Experience Platform Launchは、Adobeが提供する次世代のタグ管理機能です。  Platform Launch は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のタグをデプロイおよび管理するためのシンプルな手段を提供します。 Platform Launch は、Adobe Experience Cloud に付属の付加価値機能として提供されています。

Experience Platform Launch の機能の概要については、以下のリソースを参照してください。
- Adobe Experience Platform Launch[ドキュメント](https://docs.adobe.com/content/help/ja-JP/experience-cloud/user-guides/home.translate.html)
- Adobe Experience Platform Launch[クイック開始ビデオ](https://experienceleague.adobe.com/docs/launch/using/intro/get-started/videos.html?)。 [Adobe Experience Platform Launch](https://www.youtube.com/embed/rwqqkG1SERU)と[パブリッシングプロセスの概要](https://helpx.adobe.com/jp/analytics/how-to/adobe-launch-publishing-process.html)との開始を確認し、次の概念に進みます。

## プラットフォームインターフェイスでのプラットフォーム起動拡張機能の見つけ方{#how-to-find-extensions-in-interface}

プラットフォームの起動の拡張機能をプラットフォームインターフェイスで探すには、**[!UICONTROL 宛先]**/**[!UICONTROL カタログ]**&#x200B;を参照し、**[!UICONTROL タイプ]**&#x200B;フィルターで&#x200B;**[!UICONTROL 拡張機能]**&#x200B;を選択します。

![インターフェイスの「拡張機能」フィルター](../../assets/catalog/launch-extensions/filter.png)

## プラットフォーム起動拡張の仕組み{#how-extensions-work}

Platform Launch拡張は、生のイベントデータを複数のタイプの宛先に転送します。 この拡張機能は、**イベント転送**&#x200B;タイプの宛先であると考えることができます。これは、宛先プラットフォームと単純に統合された機能で、イベントの生データを転送するだけです。例としては、[Gainsight パーソナライズ機能拡張機能](../personalization/gainsight.md)や [Confirmit Voice of the Customer 拡張機能](../voice/confirmit-digital-feedback.md)などがあります。

**Adobe Experience Platformのプロファイル/セグメント** エクスポート先イベントデータを取得し、他のデータソースと組み合わせて、セグメント化を適用し、セグメントをエクスポートし、条件を満たすプロファイルを宛先にエクスポートします。例としては、[Amazon S3 クラウドストレージの宛先](../cloud-storage/amazon-s3.md)や [Google Display &amp; Video 360 の広告の宛先](../advertising/google-dv360.md)などがあります。

![Experience Platform Launch の拡張機能と他の宛先との比較](../../assets/common/launch-and-other-destinations.png)

## プラットフォーム起動拡張機能を使用する利点{#extensions-benefits}

Adobe Experience Platform Launchは既存のExperience Cloudのお客様は無料です。 プラットフォームの起動により、導入、設定、更新、削除が可能な使いやすい拡張機能により、Webサイトへのタグの導入が簡単になります。 プラットフォームの起動は、Webサイトの設置面積が小さく、ページの読み込みを素早く維持できます。

>[!IMPORTANT]
>
>Platform Launch Extensionsへのセグメントのアクティブ化はできませんが、特定の状況でイベントデータを転送するだけのルールを設定できます。 詳しくは、以下を参照してください。

イベントデータを拡張機能に転送するタイミングを指定する&#x200B;*ルール*&#x200B;を作成できます。この強力な機能により、すべてのインタラクションでイベントデータを送信するのではなく、特定の状況でのみイベントデータを転送できます。詳しくは、[Adobe Experience Platform Launchのドキュメント](https://experienceleague.adobe.com/docs/launch/using/reference/manage-resources/rules.html)のルールを参照してください。

## プラットフォーム起動拡張機能の使用例{#extensions-use-cases}

プラットフォーム起動の拡張機能を使用すると、様々な顧客の使用例を満たすことができます。 プラットフォーム起動拡張機能の使用例を以下に示します。

- Facebook ピクセル拡張機能を使用して、web サイトまたはネイティブのアプリケーションデータを Facebook に送信できます。Facebook ピクセルでは、訪問者がサイトまたはアプリケーションのどの部分にアクセスしたかを把握したり、その情報を Facebook に転送したりすることができます。また、Facebook を介して、訪問者を再ターゲティングすることができます。
- web サイトやアプリケーションのイベントデータを Google Analytics に転送してそのデータを分析し、それに基づいて決定を下すことができます。
- プラットフォーム起動で設定したルールに従って、ユーザーがページとどのようにやり取りしたかに基づいて、適切なタイミングでクライアント側のchatboxアプリを有効にできます。

## 拡張機能のカテゴリ {#extension-categories}

プラットフォーム起動の拡張機能は、プラットフォームでは次のカテゴリに該当する場合があります。

- [広告](../advertising/overview.md)
- [分析](../analytics/overview.md)
- [データ管理プラットフォーム](../data-management/overview.md)
- [電子メールマーケティングの宛先](../email-marketing/overview.md)
- [パーソナライズ機能](../personalization/overview.md)
- [調査](../survey/overview.md)
- [顧客の声](../voice/overview.md)
